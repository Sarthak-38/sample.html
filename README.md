<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Basic Home Page</title>
</head>
<body>

  <!-- Header -->
  <header>
    <h1>MyWebsite</h1>
    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <!-- Hero Section -->
  <section>
    <h2>Welcome to My Website</h2>
    <p>Your journey to simplicity starts here.</p>
    <a href="#about">Learn More</a>
  </section>

  <!-- About Section -->
  <section id="about">
    <h2>About Us</h2>
    <p>We are a team of professionals dedicated to providing the best services to help you grow and succeed.</p>
  </section>

  <!-- Services Section -->
  <section id="services">
    <h2>Our Services</h2>
    <div>
      <h3>Design</h3>
      <p>Creative and user-friendly designs tailored to your needs.</p>
    </div>
    <div>
      <h3>Development</h3>
      <p>Robust and scalable websites and applications built for performance.</p>
    </div>
    <div>
      <h3>Support</h3>
      <p>24/7 customer support to ensure your satisfaction.</p>
    </div>
  </section>

  <!-- Contact Section -->
  <section id="contact">
    <h2>Contact Us</h2>
    <form>
      <label for="name">Name:</label><br>
      <input type="text" id="name" name="name" required><br><br>

      <label for="email">Email:</label><br>
      <input type="email" id="email" name="email" required><br><br>

      <label for="message">Message:</label><br>
      <textarea id="message" name="message" rows="5" required></textarea><br><br>

      <button type="submit">Send Message</button>
    </form>
  </section>

  <!-- Footer -->
  <footer>
    <p>&copy; 2025 MyWebsite. All rights reserved.</p>
  </footer>

</body>
</html>
