<template>
  <div class="dashboard-page">
    <nav class="navbar">
      <div class="container">
        <div class="nav-brand">TicketFlow</div>
        <button class="mobile-menu-btn" @click="toggleMenu">
          <span v-if="!menuOpen">☰</span>
          <span v-else>✕</span>
        </button>
        <div class="nav-links" :class="{ 'mobile-open': menuOpen }">
          <router-link to="/dashboard" class="nav-link" @click="closeMenu">Dashboard</router-link>
          <router-link to="/tickets" class="nav-link" @click="closeMenu">Tickets</router-link>
          <button @click="handleLogout" class="btn-logout">Logout</button>
        </div>
      </div>
    </nav>

    <div class="dashboard-content">
      <div class="container">
        <h1 class="page-title">Dashboard</h1>

        <div class="stats-grid">
          <div class="stat-box">
            <div class="stat-icon">📊</div>
            <div class="stat-number">{{ totalTickets }}</div>
            <div class="stat-label">Total Tickets</div>
          </div>

          <div class="stat-box stat-open">
            <div class="stat-icon">🟢</div>
            <div class="stat-number">{{ openTickets }}</div>
            <div class="stat-label">Open Tickets</div>
          </div>

          <div class="stat-box stat-resolved">
            <div class="stat-icon">✅</div>
            <div class="stat-number">{{ resolvedTickets }}</div>
            <div class="stat-label">Resolved Tickets</div>
          </div>
        </div>

        <div class="action-section">
          <h2>Quick Actions</h2>
          <router-link to="/tickets" class="btn btn-primary">
            Manage Tickets
          </router-link>
        </div>
      </div>
    </div>

    <footer class="footer">
      <div class="container">
        <p>&copy; 2025 TicketFlow. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      totalTickets: 0,
      openTickets: 0,
      resolvedTickets: 0,
      menuOpen: false
    }
  },
  mounted() {
    this.loadStats()
  },
  methods: {
    loadStats() {
      const tickets = JSON.parse(localStorage.getItem('tickets') || '[]')
      this.totalTickets = tickets.length
      this.openTickets = tickets.filter(t => t.status === 'open').length
      this.resolvedTickets = tickets.filter(t => t.status === 'closed').length
    },
    handleLogout() {
      localStorage.removeItem('ticketapp_session')
      this.$router.push('/')
    },
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
.dashboard-page {
  min-height: 100vh;
  background-color: #f5f7fa;
  display: flex;
  flex-direction: column;
}

/* Navigation with better spacing */
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
  gap: 2rem; /* Added spacing */
}

.nav-brand {
  font-size: 1.75rem;
  font-weight: bold;
  color: #2563eb;
  white-space: nowrap;
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
  gap: 2rem; /* Increased spacing between links */
  align-items: center;
  margin-left: auto; /* Push links to the right */
}

.nav-link {
  color: #475569;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
  padding: 0.5rem 1rem;
}

.nav-link:hover,
.nav-link.router-link-active {
  color: #2563eb;
}

.btn-logout {
  background: #dc2626;
  color: white;
  padding: 0.625rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s;
  white-space: nowrap;
}

.btn-logout:hover {
  background: #b91c1c;
}

.dashboard-content {
  flex: 1;
  padding: 3rem 0;
}

.page-title {
  font-size: 2.5rem;
  color: #1e293b;
  margin-bottom: 2rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  margin-bottom: 3rem;
}

.stat-box {
  background: white;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
}

.stat-box:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.stat-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.stat-number {
  font-size: 3rem;
  font-weight: bold;
  color: #2563eb;
  margin-bottom: 0.5rem;
}

.stat-label {
  color: #64748b;
  font-weight: 600;
  font-size: 1.1rem;
}

.stat-open .stat-number {
  color: #10b981;
}

.stat-resolved .stat-number {
  color: #6b7280;
}

.action-section {
  background: white;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.action-section h2 {
  color: #1e293b;
  margin-bottom: 1.5rem;
  font-size: 1.75rem;
}

.btn {
  padding: 0.875rem 2rem;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  display: inline-block;
  transition: all 0.3s;
  border: none;
  cursor: pointer;
  font-size: 1rem;
}

.btn-primary {
  background: #2563eb;
  color: white;
}

.btn-primary:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
}

.footer {
  background: #1e293b;
  color: white;
  padding: 2rem 0;
  text-align: center;
  margin-top: auto;
}

/* Tablet Responsive (768px - 1024px) */
@media (max-width: 1024px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .page-title {
    font-size: 2rem;
  }
}

/* Mobile Responsive (below 768px) */
@media (max-width: 768px) {
  .mobile-menu-btn {
    display: block;
  }
  
  .navbar .container {
    padding: 1rem;
  }
  
  .nav-brand {
    font-size: 1.5rem;
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
    align-items: stretch;
    gap: 1rem;
  }
  
  .nav-links.mobile-open {
    right: 0;
  }
  
  .nav-link {
    padding: 1rem;
    text-align: center;
    border-bottom: 1px solid #e2e8f0;
  }
  
  .btn-logout {
    width: 100%;
    margin-top: 1rem;
  }
  
  .stats-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  
  .page-title {
    font-size: 1.75rem;
  }
  
  .stat-number {
    font-size: 2.5rem;
  }
  
  .dashboard-content {
    padding: 2rem 0;
  }
  
  .container {
    padding: 0 1rem;
  }
}

/* Small Mobile (below 480px) */
@media (max-width: 480px) {
  .nav-brand {
    font-size: 1.25rem;
  }
  
  .page-title {
    font-size: 1.5rem;
  }
  
  .stat-icon {
    font-size: 2.5rem;
  }
  
  .stat-number {
    font-size: 2rem;
  }
}
</style>