<template>
  <div class="auth-page">
    <div class="container">
      <div class="auth-box">
        <h1 class="auth-title">Create Account</h1>
        <p class="auth-subtitle">Join TicketFlow today</p>

        <div v-if="error" class="error-message">{{ error }}</div>
        <div v-if="success" class="success-message">{{ success }}</div>

        <form @submit.prevent="handleSignup" class="auth-form">
          <div class="form-group">
            <label for="name">Full Name</label>
            <input
              type="text"
              id="name"
              v-model="name"
              placeholder="Enter your full name"
              required
            />
          </div>

          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="email"
              id="email"
              v-model="email"
              placeholder="Enter your email"
              required
            />
          </div>

          <div class="form-group">
            <label for="password">Password</label>
            <input
              type="password"
              id="password"
              v-model="password"
              placeholder="Create a password"
              required
            />
          </div>

          <button type="submit" class="btn btn-primary btn-full">
            Create Account
          </button>
        </form>

        <p class="auth-footer">
          Already have an account?
          <router-link to="/login" class="link">Sign in</router-link>
        </p>

        <router-link to="/" class="back-link">← Back to Home</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Signup',
  data() {
    return {
      name: '',
      email: '',
      password: '',
      error: '',
      success: ''
    }
  },
  methods: {
    handleSignup() {
      this.error = ''
      this.success = ''

      // Validate inputs
      if (!this.name || !this.email || !this.password) {
        this.error = 'Please fill in all fields'
        return
      }

      if (this.password.length < 6) {
        this.error = 'Password must be at least 6 characters'
        return
      }

      // Get existing users from localStorage
      const usersJSON = localStorage.getItem('ticketapp_users')
      const users = usersJSON ? JSON.parse(usersJSON) : []

      // Check if email already exists
      const emailExists = users.some(user => user.email.toLowerCase() === this.email.toLowerCase())
      
      if (emailExists) {
        this.error = 'An account with this email already exists. Please login instead.'
        return
      }

      // Create new user object
      const newUser = {
        id: 'user_' + Date.now(),
        name: this.name,
        email: this.email.toLowerCase(),
        password: this.password, // In production, this should be hashed!
        createdAt: new Date().toISOString()
      }

      // Add new user to users array
      users.push(newUser)

      // Save updated users array to localStorage
      localStorage.setItem('ticketapp_users', JSON.stringify(users))

      // Create session for the new user
      const token = 'token_' + newUser.id + '_' + Date.now()
      localStorage.setItem('ticketapp_session', token)
      localStorage.setItem('ticketapp_current_user', JSON.stringify(newUser))

      this.success = 'Account created successfully! Redirecting...'

      setTimeout(() => {
        this.$router.push('/dashboard')
      }, 1500)
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.auth-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  width: 100%;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 0 1rem;
}

.auth-box {
  background: white;
  padding: 2.5rem 2rem;
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  width: 100%;
  max-width: 450px;
}

.auth-title {
  font-size: 1.75rem;
  color: #1e293b;
  margin-bottom: 0.5rem;
  text-align: center;
}

.auth-subtitle {
  color: #64748b;
  text-align: center;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}

.error-message {
  background: #fee2e2;
  color: #dc2626;
  padding: 0.75rem;
  border-radius: 8px;
  margin-bottom: 1.5rem;
  font-size: 0.9rem;
}

.success-message {
  background: #d1fae5;
  color: #059669;
  padding: 0.75rem;
  border-radius: 8px;
  margin-bottom: 1.5rem;
  font-size: 0.9rem;
}

.auth-form {
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  color: #334155;
  font-weight: 600;
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.form-group input {
  width: 100%;
  padding: 0.875rem;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.form-group input:focus {
  outline: none;
  border-color: #2563eb;
}

.btn {
  padding: 0.875rem 2rem;
  border-radius: 8px;
  font-weight: 600;
  font-size: 1rem;
  border: none;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-primary {
  background: #2563eb;
  color: white;
}

.btn-primary:hover {
  background: #1d4ed8;
}

.btn-full {
  width: 100%;
}

.auth-footer {
  text-align: center;
  color: #64748b;
  font-size: 0.95rem;
}

.link {
  color: #2563eb;
  text-decoration: none;
  font-weight: 600;
}

.link:hover {
  text-decoration: underline;
}

.back-link {
  display: block;
  text-align: center;
  color: #64748b;
  text-decoration: none;
  margin-top: 1.5rem;
  font-size: 0.9rem;
}

.back-link:hover {
  color: #2563eb;
}

/* Tablet - 768px and above */
@media (min-width: 768px) {
  .auth-page {
    padding: 2rem;
  }
  
  .auth-box {
    padding: 3rem;
  }
  
  .auth-title {
    font-size: 2rem;
  }
  
  .auth-subtitle {
    font-size: 1rem;
  }
}

/* Small Mobile - below 480px */
@media (max-width: 480px) {
  .auth-box {
    padding: 2rem 1.5rem;
  }
  
  .auth-title {
    font-size: 1.5rem;
  }
  
  .form-group input {
    padding: 0.75rem;
    font-size: 0.95rem;
  }
  
  .btn {
    padding: 0.75rem 1.5rem;
    font-size: 0.95rem;
  }
}

/* Extra Small Mobile - below 360px */
@media (max-width: 360px) {
  .auth-page {
    padding: 0.5rem;
  }
  
  .auth-box {
    padding: 1.5rem 1rem;
  }
  
  .auth-title {
    font-size: 1.25rem;
  }
}
</style>