
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ahmudullah Papon| Photography</title>

  <!-- Google Font -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="style.css">
</head>
<body>

  <!-- Navbar -->
  <header>
    <nav>
      <h2 class="logo">YourName</h2>

      <ul class="nav-links">
        <li><a href="#home">Home</a></li>
        <li><a href="#gallery">Gallery</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <!-- Hero Section -->
  <section id="home" class="hero">
    <div class="hero-text">
      <h1>Capturing Moments</h1>
      <p>Professional Photographer & Visual Storyteller</p>
      <a href="#gallery" class="btn">View My Work</a>
    </div>
  </section>

  <!-- Gallery -->
  <section id="gallery" class="gallery">
    <h2>My Work</h2>

    <div class="gallery-grid">
      <img src="https://source.unsplash.com/600x600/?portrait" alt="">
      <img src="https://source.unsplash.com/600x600/?nature" alt="">
      <img src="https://source.unsplash.com/600x600/?wedding" alt="">
      <img src="https://source.unsplash.com/600x600/?street" alt="">
      <img src="https://source.unsplash.com/600x600/?travel" alt="">
      <img src="https://source.unsplash.com/600x600/?fashion" alt="">
    </div>
  </section>

  <!-- About -->
  <section id="about" class="about">
    <h2>About Me</h2>

    <p>
      I am a passionate photographer specializing in portraits, events, 
      and creative visual storytelling. My goal is to capture emotions,
      beauty, and authenticity through every frame.
    </p>
  </section>

  <!-- Contact -->
  <section id="contact" class="contact">
    <h2>Contact Me</h2>

    <form>
      <input type="text" placeholder="Your Name" required>
      <input type="email" placeholder="Your Email" required>
      <textarea placeholder="Your Message" required></textarea>

      <button type="submit">Send Message</button>
    </form>
  </section>

  <!-- Footer -->
  <footer>
    <p>© 2026 Your Name | Photography Portfolio</p>
  </footer>

/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

/* Body */
body {
  background: #0f0f0f;
  color: #ffffff;
  line-height: 1.6;
}

/* Navbar */
header {
  background: #111;
  position: fixed;
  width: 100%;
  top: 0;
  z-index: 1000;
}

nav {
  max-width: 1200px;
  margin: auto;
  padding: 20px;

  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 24px;
  font-weight: 600;
}

.nav-links {
  list-style: none;
  display: flex;
}

.nav-links li {
  margin-left: 25px;
}

.nav-links a {
  text-decoration: none;
  color: #fff;
  transition: 0.3s;
}

.nav-links a:hover {
  color: #00adb5;
}

/* Hero */
.hero {
  height: 100vh;
  background: url("https://source.unsplash.com/1600x900/?camera,photography") center/cover no-repeat;

  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;

  padding-top: 70px;
}

.hero-text {
  background: rgba(0,0,0,0.6);
  padding: 40px;
  border-radius: 10px;
}

.hero h1 {
  font-size: 48px;
  margin-bottom: 10px;
}

.hero p {
  margin-bottom: 20px;
  font-weight: 300;
}

.btn {
  background: #00adb5;
  color: #000;
  padding: 12px 25px;
  text-decoration: none;
  border-radius: 25px;
  font-weight: 500;
  transition: 0.3s;
}

.btn:hover {
  background: #028a90;
}

/* Sections */
section {
  padding: 80px 20px;
  max-width: 1200px;
  margin: auto;
}

/* Gallery */
.gallery h2,
.about h2,
.contact h2 {
  text-align: center;
  margin-bottom: 40px;
  font-size: 32px;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.gallery-grid img {
  width: 100%;
  border-radius: 10px;
  transition: 0.4s;
}

.gallery-grid img:hover {
  transform: scale(1.05);
  opacity: 0.8;
}

/* About */
.about p {
  max-width: 800px;
  margin: auto;
  text-align: center;
  color: #ccc;
}

/* Contact */
.contact form {
  max-width: 500px;
  margin: auto;
  display: flex;
  flex-direction: column;
}

.contact input,
.contact textarea {
  background: #1a1a1a;
  border: none;
  margin-bottom: 15px;
  padding: 12px;
  color: #fff;
  border-radius: 5px;
}

.contact textarea {
  resize: none;
  height: 120px;
}

.contact button {
  background: #00adb5;
  border: none;
  padding: 12px;
  cursor: pointer;
  font-weight: 600;
  border-radius: 25px;
}

.contact button:hover {
  background: #028a90;
}

/* Footer */
footer {
  background: #111;
  text-align: center;
  padding: 20px;
  color: #777;
}

/* Responsive */
@media (max-width: 768px) {
  .hero h1 {
    font-size: 32px;
  }

  .nav-links {
    display: none;
  }
}
</body>
</html>
