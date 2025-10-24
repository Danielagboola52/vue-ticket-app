<template>
  <div class="landing-page">
    <!-- Navigation -->
    <nav class="navbar">
      <div class="container">
        <div class="nav-brand">TicketFlow</div>
        <button class="mobile-menu-btn" @click="toggleMenu">
          <span v-if="!menuOpen">☰</span>
          <span v-else>✕</span>
        </button>
        <div class="nav-links" :class="{ 'mobile-open': menuOpen }">
          <router-link to="/login" class="nav-link" @click="closeMenu">Login</router-link>
          <router-link to="/signup" class="nav-button" @click="closeMenu">Get Started</router-link>
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero">
      <div class="container">
        <div class="hero-content">
          <!-- Decorative Circles -->
          <div class="decorative-circle circle-1"></div>
          <div class="decorative-circle circle-2"></div>
          <div class="decorative-circle circle-3"></div>
          
          <h1 class="hero-title">Manage Your Tickets Effortlessly</h1>
          <p class="hero-description">
            Streamline your support workflow with our powerful ticket management system. 
            Track, organize, and resolve tickets faster than ever before.
          </p>
          
          <div class="hero-buttons">
            <router-link to="/signup" class="btn btn-primary">Get Started Free</router-link>
            <router-link to="/login" class="btn btn-secondary">Sign In</router-link>
          </div>
        </div>
      </div>
      
      <!-- Wave SVG -->
      <div class="wave-container">
        <svg viewBox="0 0 1440 120" fill="none" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none">
          <path d="M0,64 C240,20 480,20 720,64 C960,108 1200,108 1440,64 L1440,120 L0,120 Z" fill="#ffffff"/>
        </svg>
      </div>
    </section>

    <!-- Features Section -->
    <section class="features">
      <div class="container">
        <h2 class="section-title">Why Choose TicketFlow?</h2>
        
        <div class="features-grid">
          <div class="feature-box">
            <div class="feature-icon">📊</div>
            <h3>Real-Time Dashboard</h3>
            <p>Monitor all your tickets at a glance with live statistics and insights.</p>
          </div>
          
          <div class="feature-box">
            <div class="feature-icon">⚡</div>
            <h3>Lightning Fast</h3>
            <p>Create, update, and resolve tickets in seconds with our intuitive interface.</p>
          </div>
          
          <div class="feature-box">
            <div class="feature-icon">🔒</div>
            <h3>Secure & Private</h3>
            <p>Your data is protected with enterprise-grade security measures.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="cta-section">
      <div class="container">
        <div class="cta-box">
          <div class="decorative-circle circle-4"></div>
          <h2>Ready to Get Started?</h2>
          <p>Join thousands of teams already using TicketFlow to manage their support tickets.</p>
          <router-link to="/signup" class="btn btn-primary btn-large">
            Create Free Account
          </router-link>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-brand">
            <h3>TicketFlow</h3>
            <p>Efficient ticket management made simple.</p>
          </div>
          <div class="footer-links">
            <div class="footer-column">
              <h4>Product</h4>
              <a href="#">Features</a>
              <a href="#">Pricing</a>
              <a href="#">Security</a>
            </div>
            <div class="footer-column">
              <h4>Company</h4>
              <a href="#">About</a>
              <a href="#">Contact</a>
              <a href="#">Support</a>
            </div>
          </div>
        </div>
        <div class="footer-bottom">
          <p>&copy; 2025 TicketFlow. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Landing',
  data() {
    return {
      menuOpen: false
    }
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen
    },
    closeMenu() {
      this.menuOpen = false
    }
  }
}
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  background-color: #f5f7fa;
}

/* Navigation */
.navbar {
  background: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 0 2rem;
}

.navbar .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem 2rem;
}

.nav-brand {
  font-size: 1.75rem;
  font-weight: bold;
  color: #2563eb;
}

.mobile-menu-btn {
  display: none;
  background: none;
  border: none;
  font-size: 1.75rem;
  color: #2563eb;
  cursor: pointer;
  padding: 0.5rem;
}

.nav-links {
  display: flex;
  gap: 1.5rem;
  align-items: center;
}

.nav-link {
  color: #475569;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
}

.nav-link:hover {
  color: #2563eb;
}

.nav-button {
  background: #2563eb;
  color: white;
  padding: 0.625rem 1.5rem;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  transition: background 0.3s, transform 0.3s;
}

.nav-button:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
}

/* Hero Section */
.hero {
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  color: white;
  padding: 6rem 0 8rem;
  position: relative;
  overflow: hidden;
}

.hero-content {
  text-align: center;
  position: relative;
  z-index: 2;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 1.5rem;
  line-height: 1.2;
}

.hero-description {
  font-size: 1.25rem;
  max-width: 700px;
  margin: 0 auto 2.5rem;
  opacity: 0.95;
  line-height: 1.6;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}

.btn {
  padding: 0.875rem 2rem;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  font-size: 1.05rem;
  transition: all 0.3s;
  display: inline-block;
}

.btn-primary {
  background: white;
  color: #2563eb;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

.btn-secondary {
  background: transparent;
  color: white;
  border: 2px solid white;
}

.btn-secondary:hover {
  background: white;
  color: #2563eb;
}

.btn-large {
  padding: 1rem 2.5rem;
  font-size: 1.15rem;
}

/* Decorative Circles - More Visible */
.decorative-circle {
  position: absolute;
  border-radius: 50%;
  z-index: 1;
}

.circle-1 {
  width: 400px;
  height: 400px;
  background: rgba(255, 255, 255, 0.15);
  top: -150px;
  right: 5%;
  animation: float 6s ease-in-out infinite;
}

.circle-2 {
  width: 250px;
  height: 250px;
  background: rgba(255, 255, 255, 0.1);
  bottom: 80px;
  left: 3%;
  animation: float 8s ease-in-out infinite reverse;
}

.circle-3 {
  width: 180px;
  height: 180px;
  background: rgba(255, 255, 255, 0.12);
  top: 50%;
  right: 20%;
  animation: float 7s ease-in-out infinite;
}

.circle-4 {
  width: 200px;
  height: 200px;
  background: rgba(37, 99, 235, 0.1);
  top: -50px;
  right: -50px;
  animation: float 5s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-20px);
  }
}

/* Wave */
.wave-container {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  line-height: 0;
}

.wave-container svg {
  display: block;
  width: 100%;
  height: 120px;
}

/* Features Section */
.features {
  padding: 5rem 0;
  background: white;
}

.section-title {
  text-align: center;
  font-size: 2.5rem;
  color: #1e293b;
  margin-bottom: 3rem;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.feature-box {
  background: #f8fafc;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
  border: 2px solid transparent;
}

.feature-box:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  border-color: #2563eb;
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.feature-box h3 {
  color: #2563eb;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.feature-box p {
  color: #64748b;
  line-height: 1.6;
}

/* CTA Section */
.cta-section {
  padding: 5rem 0;
  background: #f8fafc;
}

.cta-box {
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  color: white;
  padding: 4rem 3rem;
  border-radius: 16px;
  text-align: center;
  position: relative;
  overflow: hidden;
  box-shadow: 0 10px 40px rgba(37, 99, 235, 0.3);
}

.cta-box h2 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.cta-box p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  opacity: 0.95;
}

/* Footer */
.footer {
  background: #1e293b;
  color: white;
  padding: 3rem 0 1.5rem;
}

.footer-content {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 3rem;
  margin-bottom: 2rem;
}

.footer-brand h3 {
  color: #2563eb;
  font-size: 1.75rem;
  margin-bottom: 0.5rem;
}

.footer-brand p {
  color: #94a3b8;
}

.footer-links {
  display: flex;
  gap: 3rem;
}

.footer-column {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.footer-column h4 {
  color: white;
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
}

.footer-column a {
  color: #94a3b8;
  text-decoration: none;
  transition: color 0.3s;
}

.footer-column a:hover {
  color: #2563eb;
}

.footer-bottom {
  padding-top: 2rem;
  border-top: 1px solid #334155;
  text-align: center;
  color: #94a3b8;
}

/* Responsive */
@media (max-width: 1024px) {
  .hero-title {
    font-size: 3rem;
  }
  
  .circle-1 {
    width: 300px;
    height: 300px;
  }
}

@media (max-width: 768px) {
  .mobile-menu-btn {
    display: block;
  }
  
  .nav-links {
    position: fixed;
    top: 70px;
    right: -100%;
    width: 70%;
    max-width: 300px;
    height: calc(100vh - 70px);
    background: white;
    flex-direction: column;
    padding: 2rem 1rem;
    box-shadow: -2px 0 8px rgba(0, 0, 0, 0.1);
    transition: right 0.3s ease;
  }
  
  .nav-links.mobile-open {
    right: 0;
  }
  
  .nav-link,
  .nav-button {
    width: 100%;
    text-align: center;
    padding: 1rem;
  }
  
  .hero-title {
    font-size: 2.5rem;
  }
  
  .hero-description {
    font-size: 1.1rem;
  }
  
  .circle-1 {
    width: 250px;
    height: 250px;
  }
  
  .circle-2 {
    width: 180px;
    height: 180px;
  }
  
  .circle-3 {
    width: 120px;
    height: 120px;
  }
  
  .features-grid {
    grid-template-columns: 1fr;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .cta-box {
    padding: 3rem 2rem;
  }
  
  .cta-box h2 {
    font-size: 2rem;
  }
  
  .footer-content {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .footer-links {
    gap: 2rem;
  }
}

@media (max-width: 480px) {
  .hero-title {
    font-size: 2rem;
  }
  
  .hero-description {
    font-size: 1rem;
  }
  
  .btn {
    padding: 0.75rem 1.5rem;
    font-size: 0.95rem;
  }
  
  .cta-box h2 {
    font-size: 1.75rem;
  }
  
  .cta-box p {
    font-size: 1rem;
  }
}
</style>