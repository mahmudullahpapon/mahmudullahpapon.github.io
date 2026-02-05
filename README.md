<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your Name | Photography</title>

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

</body>
</html>
