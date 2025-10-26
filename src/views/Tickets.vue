<template>
  <div class="tickets-page">
    <!-- Toast Notification -->
    <Toast :show="toast.show" :message="toast.message" :type="toast.type" />

    <!-- Navigation -->
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

    <!-- Content -->
    <div class="tickets-content">
      <div class="container">
        <div class="header-section">
          <div class="header-text">
            <h1 class="page-title">Ticket Management</h1>
            <p class="page-subtitle">Create and manage your support tickets</p>
          </div>
          <button @click="openCreateModal" class="btn btn-primary">
            + New Ticket
          </button>
        </div>

        <!-- Tickets List -->
        <div v-if="tickets.length === 0" class="empty-state">
          <div class="empty-icon">📋</div>
          <h3>No tickets yet</h3>
          <p>Create your first ticket to get started</p>
          <button @click="openCreateModal" class="btn btn-primary">Create Ticket</button>
        </div>

        <div v-else class="tickets-grid">
          <div v-for="ticket in tickets" :key="ticket.id" class="ticket-card">
            <div class="ticket-header">
              <h3 class="ticket-title">{{ ticket.title }}</h3>
              <span :class="['status-badge', ticket.status]">
                {{ getStatusLabel(ticket.status) }}
              </span>
            </div>
            
            <p v-if="ticket.description" class="ticket-description">
              {{ ticket.description }}
            </p>
            
            <div class="ticket-meta">
              <span class="ticket-priority" v-if="ticket.priority">
                Priority: <strong>{{ ticket.priority }}</strong>
              </span>
              <span class="ticket-date">{{ formatDate(ticket.createdAt) }}</span>
            </div>

            <div class="ticket-actions">
              <button @click="openEditModal(ticket)" class="btn-action btn-edit">
                ✏️ Edit
              </button>
              <button @click="confirmDelete(ticket)" class="btn-action btn-delete">
                🗑️ Delete
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Create/Edit Modal -->
    <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
      <div class="modal">
        <div class="modal-header">
          <h2>{{ isEditing ? 'Edit Ticket' : 'Create New Ticket' }}</h2>
          <button @click="closeModal" class="modal-close">✕</button>
        </div>

        <form @submit.prevent="saveTicket" class="modal-form">
          <div class="form-group">
            <label for="title">Title <span class="required">*</span></label>
            <input
              type="text"
              id="title"
              v-model="form.title"
              placeholder="Enter ticket title"
              :class="{ 'error': errors.title }"
            />
            <span v-if="errors.title" class="error-message">{{ errors.title }}</span>
          </div>

          <div class="form-group">
            <label for="description">Description</label>
            <textarea
              id="description"
              v-model="form.description"
              placeholder="Enter ticket description (optional)"
              rows="4"
            ></textarea>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="status">Status <span class="required">*</span></label>
              <select
                id="status"
                v-model="form.status"
                :class="{ 'error': errors.status }"
              >
                <option value="">Select status</option>
                <option value="open">Open</option>
                <option value="in_progress">In Progress</option>
                <option value="closed">Closed</option>
              </select>
              <span v-if="errors.status" class="error-message">{{ errors.status }}</span>
            </div>

            <div class="form-group">
              <label for="priority">Priority</label>
              <select id="priority" v-model="form.priority">
                <option value="">Select priority</option>
                <option value="Low">Low</option>
                <option value="Medium">Medium</option>
                <option value="High">High</option>
              </select>
            </div>
          </div>

          <div class="modal-actions">
            <button type="button" @click="closeModal" class="btn btn-secondary">
              Cancel
            </button>
            <button type="submit" class="btn btn-primary">
              {{ isEditing ? 'Update Ticket' : 'Create Ticket' }}
            </button>
          </div>
        </form>
      </div>
    </div>

    <!-- Delete Confirmation Modal -->
    <div v-if="showDeleteModal" class="modal-overlay" @click.self="showDeleteModal = false">
      <div class="modal modal-small">
        <div class="modal-header">
          <h2>Delete Ticket?</h2>
          <button @click="showDeleteModal = false" class="modal-close">✕</button>
        </div>

        <div class="modal-body">
          <p>Are you sure you want to delete <strong>"{{ ticketToDelete?.title }}"</strong>?</p>
          <p class="warning-text">This action cannot be undone.</p>
        </div>

        <div class="modal-actions">
          <button @click="showDeleteModal = false" class="btn btn-secondary">
            Cancel
          </button>
          <button @click="deleteTicket" class="btn btn-danger">
            Delete
          </button>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <p>&copy; 2025 TicketFlow. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script>
import Toast from '../components/Toast.vue'

export default {
  name: 'Tickets',
  components: {
    Toast
  },
  data() {
    return {
      menuOpen: false,
      showModal: false,
      showDeleteModal: false,
      isEditing: false,
      tickets: [],
      form: {
        id: null,
        title: '',
        description: '',
        status: '',
        priority: '',
        createdAt: null
      },
      errors: {},
      ticketToDelete: null,
      toast: {
        show: false,
        message: '',
        type: 'success'
      }
    }
  },
  mounted() {
    this.loadTickets()
  },
  methods: {
    loadTickets() {
      const stored = localStorage.getItem('tickets')
      this.tickets = stored ? JSON.parse(stored) : []
    },
    saveTickets() {
      localStorage.setItem('tickets', JSON.stringify(this.tickets))
    },
    openCreateModal() {
      this.isEditing = false
      this.resetForm()
      this.showModal = true
    },
    openEditModal(ticket) {
      this.isEditing = true
      this.form = { ...ticket }
      this.errors = {}
      this.showModal = true
    },
    closeModal() {
      this.showModal = false
      this.resetForm()
    },
    resetForm() {
      this.form = {
        id: null,
        title: '',
        description: '',
        status: '',
        priority: '',
        createdAt: null
      }
      this.errors = {}
    },
    validateForm() {
      this.errors = {}

      if (!this.form.title || this.form.title.trim() === '') {
        this.errors.title = 'Title is required'
      }

      if (!this.form.status) {
        this.errors.status = 'Status is required'
      } else if (!['open', 'in_progress', 'closed'].includes(this.form.status)) {
        this.errors.status = 'Invalid status selected'
      }

      return Object.keys(this.errors).length === 0
    },
    saveTicket() {
      if (!this.validateForm()) {
        this.showToast('Please fix the errors in the form', 'error')
        return
      }

      if (this.isEditing) {
        const index = this.tickets.findIndex(t => t.id === this.form.id)
        if (index !== -1) {
          this.tickets[index] = { ...this.form }
          this.showToast('Ticket updated successfully!', 'success')
        }
      } else {
        const newTicket = {
          ...this.form,
          id: Date.now(),
          createdAt: new Date().toISOString()
        }
        this.tickets.unshift(newTicket)
        this.showToast('Ticket created successfully!', 'success')
      }

      localStorage.setItem('tickets', JSON.stringify(this.tickets))
      this.closeModal()
    },
    confirmDelete(ticket) {
      this.ticketToDelete = ticket
      this.showDeleteModal = true
    },
    deleteTicket() {
      this.tickets = this.tickets.filter(t => t.id !== this.ticketToDelete.id)
      localStorage.setItem('tickets', JSON.stringify(this.tickets))
      this.showDeleteModal = false
      this.showToast('Ticket deleted successfully', 'success')
      this.ticketToDelete = null
    },
    getStatusLabel(status) {
      const labels = {
        open: 'Open',
        in_progress: 'In Progress',
        closed: 'Closed'
      }
      return labels[status] || status
    },
    formatDate(dateString) {
      if (!dateString) return 'Just now'
      const date = new Date(dateString)
      return date.toLocaleDateString('en-US', { 
        month: 'short', 
        day: 'numeric', 
        year: 'numeric' 
      })
    },
    showToast(message, type = 'success') {
      this.toast = { show: true, message, type }
      setTimeout(() => {
        this.toast.show = false
      }, 3000)
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
* {
  box-sizing: border-box;
}

.tickets-page {
  min-height: 100vh;
  background-color: #f5f7fa;
  display: flex;
  flex-direction: column;
  width: 100%;
  overflow-x: hidden;
}

.navbar {
  background: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 0 1rem;
  width: 100%;
}

.navbar .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.nav-brand {
  font-size: 1.5rem;
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
  z-index: 101;
}

.nav-links {
  display: flex;
  gap: 1.5rem;
  align-items: center;
  margin-left: auto;
}

.nav-link {
  color: #475569;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
  padding: 0.5rem 0.75rem;
}

.nav-link:hover,
.nav-link.router-link-active {
  color: #2563eb;
}

.btn-logout {
  background: #dc2626;
  color: white;
  padding: 0.625rem 1.25rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s;
  white-space: nowrap;
  font-size: 0.95rem;
}

.btn-logout:hover {
  background: #b91c1c;
}

.tickets-content {
  flex: 1;
  padding: 2.5rem 0;
}

.header-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  gap: 1rem;
  flex-wrap: wrap;
}

.header-text {
  flex: 1;
  min-width: 200px;
}

.page-title {
  font-size: 2rem;
  color: #1e293b;
  margin-bottom: 0.5rem;
}

.page-subtitle {
  font-size: 1rem;
  color: #64748b;
}

.btn {
  padding: 0.875rem 1.5rem;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.3s;
  font-size: 0.95rem;
  text-decoration: none;
  display: inline-block;
}

.btn-primary {
  background: #2563eb;
  color: white;
}

.btn-primary:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
}

.btn-secondary {
  background: #64748b;
  color: white;
}

.btn-secondary:hover {
  background: #475569;
}

.btn-danger {
  background: #dc2626;
  color: white;
}

.btn-danger:hover {
  background: #b91c1c;
}

.empty-state {
  background: white;
  padding: 3rem 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  text-align: center;
}

.empty-icon {
  font-size: 3.5rem;
  margin-bottom: 1.5rem;
  opacity: 0.5;
}

.empty-state h3 {
  color: #1e293b;
  font-size: 1.35rem;
  margin-bottom: 0.5rem;
}

.empty-state p {
  color: #64748b;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}

.tickets-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.25rem;
}

.ticket-card {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s, box-shadow 0.3s;
}

.ticket-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.ticket-header {
  display: flex;
  justify-content: space-between;
  align-items: start;
  gap: 0.75rem;
  margin-bottom: 1rem;
}

.ticket-title {
  color: #1e293b;
  font-size: 1.15rem;
  margin: 0;
  flex: 1;
  word-break: break-word;
  line-height: 1.3;
}

.status-badge {
  padding: 0.35rem 0.75rem;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  white-space: nowrap;
  flex-shrink: 0;
}

.status-badge.open {
  background: #d1fae5;
  color: #065f46;
}

.status-badge.in_progress {
  background: #fef3c7;
  color: #92400e;
}

.status-badge.closed {
  background: #e5e7eb;
  color: #374151;
}

.ticket-description {
  color: #64748b;
  margin-bottom: 1rem;
  line-height: 1.6;
  font-size: 0.95rem;
}

.ticket-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
  font-size: 0.85rem;
  color: #64748b;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.ticket-priority strong {
  color: #1e293b;
}

.ticket-actions {
  display: flex;
  gap: 0.625rem;
}

.btn-action {
  flex: 1;
  padding: 0.625rem 0.875rem;
  border: none;
  border-radius: 6px;
  font-size: 0.85rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-edit {
  background: #eff6ff;
  color: #2563eb;
}

.btn-edit:hover {
  background: #dbeafe;
}

.btn-delete {
  background: #fee2e2;
  color: #dc2626;
}

.btn-delete:hover {
  background: #fecaca;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.modal {
  background: white;
  border-radius: 12px;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

.modal-small {
  max-width: 450px;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem 1.5rem;
  border-bottom: 1px solid #e2e8f0;
}

.modal-header h2 {
  color: #1e293b;
  font-size: 1.35rem;
  margin: 0;
}

.modal-close {
  background: none;
  border: none;
  font-size: 1.75rem;
  color: #64748b;
  cursor: pointer;
  padding: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 6px;
  transition: background 0.3s;
  flex-shrink: 0;
}

.modal-close:hover {
  background: #f1f5f9;
}

.modal-body {
  padding: 1.5rem;
}

.modal-body p {
  color: #475569;
  line-height: 1.6;
  margin-bottom: 1rem;
  font-size: 0.95rem;
}

.warning-text {
  color: #dc2626;
  font-weight: 600;
  font-size: 0.9rem;
}

.modal-form {
  padding: 1.5rem;
}

.form-group {
  margin-bottom: 1.25rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group label {
  display: block;
  color: #334155;
  font-weight: 600;
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.required {
  color: #dc2626;
}

.form-group input,
.form-group textarea,
.form-group select {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  font-size: 0.95rem;
  transition: border-color 0.3s;
  font-family: inherit;
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
  outline: none;
  border-color: #2563eb;
}

.form-group input.error,
.form-group select.error {
  border-color: #dc2626;
}

.error-message {
  display: block;
  color: #dc2626;
  font-size: 0.8rem;
  margin-top: 0.375rem;
}

.modal-actions {
  display: flex;
  gap: 0.75rem;
  padding: 1.25rem 1.5rem;
  border-top: 1px solid #e2e8f0;
  justify-content: flex-end;
}

.footer {
  background: #1e293b;
  color: white;
  padding: 2rem 0;
  text-align: center;
  margin-top: auto;
  width: 100%;
}

.footer p {
  font-size: 0.9rem;
  color: #94a3b8;
}

@media (min-width: 768px) {
  .container {
    padding: 0 2rem;
  }
  
  .navbar .container {
    padding: 1.25rem 2rem;
  }
  
  .nav-brand {
    font-size: 1.75rem;
  }
  
  .nav-links {
    gap: 2rem;
  }
  
  .nav-link {
    padding: 0.5rem 1rem;
  }
  
  .btn-logout {
    padding: 0.625rem 1.5rem;
    font-size: 1rem;
  }
  
  .tickets-content {
    padding: 3rem 0;
  }
  
  .header-section {
    margin-bottom: 2.5rem;
    gap: 1.5rem;
  }
  
  .page-title {
    font-size: 2.5rem;
  }
  
  .page-subtitle {
    font-size: 1.1rem;
  }
  
  .btn {
    padding: 0.875rem 1.75rem;
    font-size: 1rem;
  }
  
  .empty-state {
    padding: 4rem 2rem;
  }
  
  .empty-icon {
    font-size: 4rem;
  }
  
  .empty-state h3 {
    font-size: 1.5rem;
  }
  
  .tickets-grid {
    grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    gap: 1.5rem;
  }
  
  .ticket-card {
    padding: 1.75rem;
  }
  
  .ticket-title {
    font-size: 1.25rem;
  }
  
  .status-badge {
    padding: 0.375rem 0.875rem;
    font-size: 0.85rem;
  }
  
  .ticket-description {
    font-size: 1rem;
  }
  
  .ticket-meta {
    font-size: 0.9rem;
  }
  
  .btn-action {
    padding: 0.625rem 1rem;
    font-size: 0.9rem;
  }
  
  .modal-header {
    padding: 1.5rem 2rem;
  }
  
  .modal-header h2 {
    font-size: 1.5rem;
  }
  
  .modal-form {
    padding: 2rem;
  }
  
  .modal-body {
    padding: 2rem;
  }
  
  .form-group {
    margin-bottom: 1.5rem;
  }
  
  .form-row {
    gap: 1.5rem;
  }
  
  .form-group label {
    font-size: 1rem;
  }
  
  .form-group input,
  .form-group textarea,
  .form-group select {
    padding: 0.875rem;
    font-size: 1rem;
  }
  
  .modal-actions {
    gap: 1rem;
    padding: 1.5rem 2rem;
  }
}

@media (max-width: 767px) {
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
    align-items: stretch;
    gap: 1rem;
    margin-left: 0;
  }
  
  .nav-links.mobile-open {
    right: 0;
  }
  
  .nav-link {
    padding: 1rem;
    text-align: center;
    border-bottom: 1px solid #e2e8f0;
    display: block;
    width: 100%;
  }
  
  .btn-logout {
    width: 100%;
    margin-top: 1rem;
  }
  
  .header-section {
    flex-direction: column;
    align-items: stretch;
  }
  
  .header-text {
    text-align: center;
  }
  
  .btn {
    width: 100%;
  }
  
  .tickets-grid {
    grid-template-columns: 1fr;
  }
  
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .modal-actions {
    flex-direction: column-reverse;
  }
  
  .modal-actions .btn {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .nav-brand {
    font-size: 1.25rem;
  }
  
  .page-title {
    font-size: 1.75rem;
  }
  
  .page-subtitle {
    font-size: 0.9rem;
  }
  
  .ticket-card {
    padding: 1.25rem;
  }
  
  .ticket-title {
    font-size: 1.05rem;
  }
  
  .status-badge {
    font-size: 0.75rem;
    padding: 0.3rem 0.65rem;
  }
  
  .modal {
    margin: 0.5rem;
  }
  
  .modal-header h2 {
    font-size: 1.15rem;
  }
}

@media (max-width: 360px) {
  .tickets-content {
    padding: 2rem 0;
  }
  
  .ticket-card {
    padding: 1rem;
  }
  
  .modal-form,
  .modal-body {
    padding: 1rem;
  }
  
  .modal-header,
  .modal-actions {
    padding: 1rem;
  }
}
</style>