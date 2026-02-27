import { useState } from "react";
import { toast } from "sonner";
import {
  usePosts,
  useAddOrUpdatePost,
  useDeletePost,
  type PostWithId,
} from "../hooks/useQueries";
import type { BlogPost } from "../backend.d";

interface BlogPageProps {
  isAdmin: boolean;
}

function formatDate(dateStr: string): string {
  try {
    const d = new Date(dateStr);
    if (isNaN(d.getTime())) return dateStr;
    return d.toLocaleDateString("en-US", {
      year: "numeric",
      month: "long",
      day: "numeric",
    });
  } catch {
    return dateStr;
  }
}

function todayIso(): string {
  return new Date().toISOString().slice(0, 10);
}

function makeEmptyPost(): BlogPost {
  return { title: "", body: "", date: todayIso() };
}

export default function BlogPage({ isAdmin }: BlogPageProps) {
  const { data: posts, isLoading } = usePosts();
  const addOrUpdate = useAddOrUpdatePost();
  const deletePost = useDeletePost();

  const [selectedPostId, setSelectedPostId] = useState<string | null>(null);
  const [editingId, setEditingId] = useState<string | null>(null);
  const [formData, setFormData] = useState<BlogPost>(makeEmptyPost());
  const [isAddingNew, setIsAddingNew] = useState(false);

  function startEdit(post: PostWithId) {
    setEditingId(post._id);
    setFormData({ title: post.title, body: post.body, date: post.date });
    setIsAddingNew(false);
    setSelectedPostId(null);
  }

  function startNew() {
    setIsAddingNew(true);
    setEditingId(null);
    setFormData(makeEmptyPost());
    setSelectedPostId(null);
  }

  function cancelEdit() {
    setEditingId(null);
    setIsAddingNew(false);
  }

  async function handleSave(originalId: string | null) {
    if (!formData.title.trim()) {
      toast.error("Title is required");
      return;
    }
    try {
      await addOrUpdate.mutateAsync({ oldId: originalId, post: formData });
      setEditingId(null);
      setIsAddingNew(false);
      toast.success("Saved");
    } catch {
      toast.error("Failed to save");
    }
  }

  async function handleDelete(id: string) {
    try {
      await deletePost.mutateAsync(id);
      if (selectedPostId === id) setSelectedPostId(null);
      toast.success("Deleted");
    } catch {
      toast.error("Failed to delete");
    }
  }

  if (isLoading) {
    return (
      <section aria-label="Blog posts">
        <p className="font-mono text-xs text-muted-foreground">...</p>
      </section>
    );
  }

  const selectedPost = posts?.find((p) => p._id === selectedPostId) ?? null;
  const isEmpty = !posts || posts.length === 0;

  // Detail view
  if (selectedPost) {
    return (
      <section aria-label="Blog post">
        <button
          type="button"
          onClick={() => setSelectedPostId(null)}
          className="font-mono text-xs text-muted-foreground underline underline-offset-4 hover:text-foreground transition-colors mb-12 block"
        >
          ← writing
        </button>

        <PostDetail post={selectedPost} />

        {isAdmin && (
          <div className="flex gap-4 mt-10 pt-8 border-t border-border">
            <button
              type="button"
              onClick={() => startEdit(selectedPost)}
              className="font-mono text-[11px] text-muted-foreground underline underline-offset-2 hover:text-foreground transition-colors"
            >
              edit post
            </button>
            <button
              type="button"
              onClick={() => handleDelete(selectedPost._id)}
              disabled={deletePost.isPending}
              className="font-mono text-[11px] text-muted-foreground underline underline-offset-2 hover:text-foreground transition-colors disabled:opacity-40"
            >
              delete post
            </button>
          </div>
        )}
      </section>
    );
  }

  return (
    <section aria-label="Blog posts">
      <header className="mb-12">
        <h2 className="font-serif text-2xl text-foreground">Writing</h2>
        <p className="font-mono text-xs text-muted-foreground mt-2">
          thoughts, notes, observations
        </p>
      </header>

      {isEmpty && !isAddingNew && (
        <div className="py-12">
          <p className="font-mono text-sm text-muted-foreground italic">
            Nothing written yet.{" "}
            {isAdmin
              ? "Start your first post below."
              : "Check back later."}
          </p>
        </div>
      )}

      <div>
        {posts?.map((post) => {
          if (editingId === post._id) {
            return (
              <PostForm
                key={post._id}
                data={formData}
                onChange={setFormData}
                onSave={() => handleSave(post._id)}
                onCancel={cancelEdit}
                isPending={addOrUpdate.isPending}
              />
            );
          }

          return (
            <PostRow
              key={post._id}
              post={post}
              isAdmin={isAdmin}
              onSelect={() => setSelectedPostId(post._id)}
              onEdit={() => startEdit(post)}
              onDelete={() => handleDelete(post._id)}
              isDeleting={deletePost.isPending}
            />
          );
        })}

        {isAddingNew && (
          <PostForm
            data={formData}
            onChange={setFormData}
            onSave={() => handleSave(null)}
            onCancel={cancelEdit}
            isPending={addOrUpdate.isPending}
            isNew
          />
        )}
      </div>

      {isAdmin && !isAddingNew && editingId === null && (
        <div className="mt-12">
          <button
            type="button"
            onClick={startNew}
            className="font-mono text-xs text-muted-foreground underline underline-offset-4 hover:text-foreground transition-colors"
          >
            + new post
          </button>
        </div>
      )}
    </section>
  );
}

function PostRow({
  post,
  isAdmin,
  onSelect,
  onEdit,
  onDelete,
  isDeleting,
}: {
  post: PostWithId;
  isAdmin: boolean;
  onSelect: () => void;
  onEdit: () => void;
  onDelete: () => void;
  isDeleting: boolean;
}) {
  return (
    <article className="py-7 border-b border-border last:border-0">
      <div className="flex items-baseline justify-between gap-4">
        <button
          type="button"
          onClick={onSelect}
          className="font-serif text-lg text-foreground hover:underline underline-offset-4 text-left leading-snug transition-colors flex-1"
        >
          {post.title}
        </button>
        <time
          dateTime={post.date}
          className="font-mono text-xs text-muted-foreground shrink-0 whitespace-nowrap"
        >
          {formatDate(post.date)}
        </time>
      </div>

      {isAdmin && (
        <div className="flex gap-4 mt-3">
          <button
            type="button"
            onClick={onEdit}
            className="font-mono text-[11px] text-muted-foreground underline underline-offset-2 hover:text-foreground transition-colors"
          >
            edit
          </button>
          <button
            type="button"
            onClick={onDelete}
            disabled={isDeleting}
            className="font-mono text-[11px] text-muted-foreground underline underline-offset-2 hover:text-foreground transition-colors disabled:opacity-40"
          >
            delete
          </button>
        </div>
      )}
    </article>
  );
}

function PostDetail({ post }: { post: PostWithId }) {
  return (
    <article>
      <header className="mb-10">
        <h1 className="font-serif text-3xl sm:text-4xl text-foreground leading-tight mb-4">
          {post.title}
        </h1>
        <time dateTime={post.date} className="font-mono text-xs text-muted-foreground">
          {formatDate(post.date)}
        </time>
        <hr className="border-0 border-t border-border mt-8" />
      </header>

      <div className="font-mono text-sm leading-loose text-foreground whitespace-pre-wrap">
        {post.body || (
          <span className="text-muted-foreground italic">No content yet.</span>
        )}
      </div>
    </article>
  );
}

function PostForm({
  data,
  onChange,
  onSave,
  onCancel,
  isPending,
  isNew = false,
}: {
  data: BlogPost;
  onChange: (d: BlogPost) => void;
  onSave: () => void;
  onCancel: () => void;
  isPending: boolean;
  isNew?: boolean;
}) {
  return (
    <div className="py-8 border-b border-border">
      <p className="font-mono text-[10px] uppercase tracking-widest text-muted-foreground mb-6">
        {isNew ? "new post" : "editing"}
      </p>

      <div className="space-y-5">
        <div>
          <label
            htmlFor="post-title"
            className="block font-mono text-[10px] text-muted-foreground mb-1 uppercase tracking-widest"
          >
            Title
          </label>
          <input
            id="post-title"
            type="text"
            value={data.title}
            onChange={(e) => onChange({ ...data, title: e.target.value })}
            className="w-full bg-transparent border-0 border-b border-border pb-1 font-serif text-xl focus:outline-none focus:border-foreground transition-colors"
            placeholder="Post title"
          />
        </div>

        <div>
          <label
            htmlFor="post-date"
            className="block font-mono text-[10px] text-muted-foreground mb-1 uppercase tracking-widest"
          >
            Date
          </label>
          <input
            id="post-date"
            type="date"
            value={data.date}
            onChange={(e) => onChange({ ...data, date: e.target.value })}
            className="bg-transparent border-0 border-b border-border pb-1 font-mono text-xs focus:outline-none focus:border-foreground transition-colors appearance-none"
          />
        </div>

        <div>
          <label
            htmlFor="post-body"
            className="block font-mono text-[10px] text-muted-foreground mb-1 uppercase tracking-widest"
          >
            Body
          </label>
          <textarea
            id="post-body"
            value={data.body}
            onChange={(e) => onChange({ ...data, body: e.target.value })}
            rows={12}
            className="w-full bg-transparent border-0 border-b border-border pb-1 font-mono text-sm leading-loose focus:outline-none focus:border-foreground transition-colors resize-none"
            placeholder="Write your post..."
          />
        </div>
      </div>

      <div className="flex gap-6 mt-6">
        <button
          type="button"
          onClick={onSave}
          disabled={isPending}
          className="font-mono text-xs text-foreground underline underline-offset-4 hover:no-underline disabled:opacity-40 transition-all"
        >
          {isPending ? "saving..." : "save"}
        </button>
        <button
          type="button"
          onClick={onCancel}
          className="font-mono text-xs text-muted-foreground underline underline-offset-4 hover:text-foreground hover:no-underline transition-all"
        >
          cancel
        </button>
      </div>
    </div>
  );
}
