<template>
  <div class="auth-page">
    <div class="container">
      <div class="auth-box">
        <h1 class="auth-title">Welcome Back</h1>
        <p class="auth-subtitle">Sign in to your account</p>

        <div v-if="error" class="error-message">{{ error }}</div>

        <form @submit.prevent="handleLogin" class="auth-form">
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
              placeholder="Enter your password"
              required
            />
          </div>

          <button type="submit" class="btn btn-primary btn-full">
            Sign In
          </button>
        </form>

        <p class="auth-footer">
          Don't have an account?
          <router-link to="/signup" class="link">Sign up</router-link>
        </p>

        <router-link to="/" class="back-link">← Back to Home</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      email: '',
      password: '',
      error: ''
    }
  },
  methods: {
    handleLogin() {
      // Clear previous errors
      this.error = ''

      // Simple validation
      if (!this.email || !this.password) {
        this.error = 'Please fill in all fields'
        return
      }

      // Simulate authentication (accept any email/password for demo)
      const token = 'demo_token_' + Date.now()
      localStorage.setItem('ticketapp_session', token)

      // Redirect to dashboard
      this.$router.push('/dashboard')
    }
  }
}
</script>

<style scoped>
.auth-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  width: 100%;
  display: flex;
  justify-content: center;
}

.auth-box {
  background: white;
  padding: 3rem;
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  width: 100%;
  max-width: 450px;
}

.auth-title {
  font-size: 2rem;
  color: #1e293b;
  margin-bottom: 0.5rem;
  text-align: center;
}

.auth-subtitle {
  color: #64748b;
  text-align: center;
  margin-bottom: 2rem;
}

.error-message {
  background: #fee2e2;
  color: #dc2626;
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
</style>