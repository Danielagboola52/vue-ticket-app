# TicketFlow - Vue.js Ticket Management System

A modern, responsive ticket management web application built with Vue.js 3, featuring complete CRUD operations, authentication, and a beautiful blue and white UI design.

## 🌐 Live Demo

- **Live Application**: https://vue-ticket-app.netlify.app/
- **GitHub Repository**: https://github.com/Danielagboola52/vue-ticket-app.git

---


## 🚀 Features

- **Landing Page**: Eye-catching hero section with wavy SVG background and animated decorative circles
- **Authentication System**: Secure login and signup with form validation and localStorage-based session management
- **Dashboard**: Real-time statistics showing total tickets, open tickets, and resolved tickets
- **Ticket Management**: Full CRUD operations (Create, Read, Update, Delete) with:
  - Form validation for all required fields
  - Status-based color coding (Open = Green, In Progress = Amber, Closed = Gray)
  - Priority levels (Low, Medium, High)
  - Confirmation dialogs for destructive actions
  - Toast notifications for user feedback
- **Responsive Design**: Fully responsive layout that works seamlessly on mobile, tablet, and desktop devices
- **Protected Routes**: Dashboard and Tickets pages require authentication

## 🛠️ Technologies Used

- **Frontend Framework**: Vue.js 3 (Composition API with Options API)
- **Build Tool**: Vite
- **Router**: Vue Router 4
- **Styling**: CSS3 with scoped styles
- **Storage**: LocalStorage for data persistence and session management
- **Icons**: Unicode emojis for a lightweight, accessible icon system

## 📋 Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js** (v16 or higher)
- **npm** (v7 or higher)

## 🔧 Installation & Setup

### 1. Clone or Download the Project
```bash
cd vue-ticket-app
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Run the Development Server
```bash
npm run dev
```

The application will be available at `http://localhost:5173/`

### 4. Build for Production
```bash
npm run build
```

The production-ready files will be in the `dist/` folder.

## 📁 Project Structure
```
vue-ticket-app/
├── src/
│   ├── assets/           # Static assets (images, SVGs)
│   ├── components/       # Reusable Vue components
│   │   └── Toast.vue    # Toast notification component
│   ├── router/           # Vue Router configuration
│   │   └── index.js     # Route definitions and navigation guards
│   ├── views/            # Page components
│   │   ├── Landing.vue  # Landing/home page
│   │   ├── Login.vue    # Login page
│   │   ├── Signup.vue   # Signup page
│   │   ├── Dashboard.vue # Dashboard with statistics
│   │   └── Tickets.vue  # Ticket management page (CRUD)
│   ├── App.vue          # Root component
│   ├── main.js          # Application entry point
│   └── style.css        # Global styles
├── public/              # Public static files
├── index.html           # HTML template
├── package.json         # Dependencies and scripts
├── vite.config.js       # Vite configuration
└── README.md            # This file
```

## 🎨 Design Specifications

### Color Theme
- **Primary Blue**: `#2563eb` (buttons, headings, branding)
- **Light Blue**: `#3b82f6` (gradients, hover states)
- **White**: `#ffffff` (backgrounds, text on blue)
- **Gray Tones**: `#64748b`, `#475569` (secondary text, borders)
- **Status Colors**:
  - Open: `#10b981` (Green)
  - In Progress: `#f59e0b` (Amber)
  - Closed: `#6b7280` (Gray)

### Layout
- **Max Width**: 1440px (centered on larger screens)
- **Hero Section**: Wavy SVG bottom edge with animated decorative circles
- **Card-Based Design**: Rounded corners (12px) with subtle shadows
- **Responsive Breakpoints**:
  - Mobile: < 768px
  - Tablet: 768px - 1024px
  - Desktop: > 1024px

## 🔐 Authentication

### Test Credentials
The application uses simulated authentication. You can log in with **any email and password combination**.

For demonstration:
- **Email**: `demo@ticketflow.com`
- **Password**: `password123`

### Session Management
- Sessions are stored in `localStorage` with the key `ticketapp_session`
- Protected routes (Dashboard, Tickets) redirect to login if no session exists
- Logout clears the session and redirects to the home page

## 📝 Data Validation Rules

### Ticket Form Validation
- **Title**: Required, cannot be empty
- **Status**: Required, must be one of: `open`, `in_progress`, `closed`
- **Description**: Optional, no character limit
- **Priority**: Optional, one of: `Low`, `Medium`, `High`

### Error Handling
- **Invalid Form Input**: Inline error messages below fields with red borders
- **Empty Required Fields**: "Title is required" / "Status is required"
- **Invalid Status**: "Invalid status selected"
- **Success Messages**: Toast notifications for successful operations
- **Delete Confirmation**: Modal dialog before deleting tickets

## 💾 Data Storage

All ticket data is stored in `localStorage` under the key `tickets` as a JSON array. Each ticket object contains:
```javascript
{
  id: 1234567890,           // Unique timestamp-based ID
  title: "Fix login bug",   // Ticket title
  description: "...",       // Optional description
  status: "open",           // One of: open, in_progress, closed
  priority: "High",         // Optional: Low, Medium, High
  createdAt: "2025-01-01T..." // ISO timestamp
}
```

## 🧪 Testing the Application

### 1. Test Authentication Flow
1. Visit the landing page at `/`
2. Click "Get Started" or "Login"
3. Enter any email and password
4. Verify redirect to Dashboard

### 2. Test Protected Routes
1. Log out
2. Try accessing `/dashboard` or `/tickets` directly
3. Verify redirect to `/login`

### 3. Test Ticket CRUD Operations

**Create:**
1. Go to Tickets page
2. Click "+ New Ticket"
3. Fill in title and status (required)
4. Submit form
5. Verify ticket appears in list

**Read:**
1. View ticket cards with status badges
2. Check color coding (Green/Amber/Gray)

**Update:**
1. Click "Edit" on any ticket
2. Modify fields
3. Save changes
4. Verify updates reflected

**Delete:**
1. Click "Delete" on any ticket
2. Confirm deletion in modal
3. Verify ticket removed from list

### 4. Test Responsive Design
1. Resize browser window to mobile size (< 768px)
2. Click hamburger menu (☰)
3. Test navigation
4. Verify all layouts adapt properly

## 🐛 Known Issues

None currently reported. If you encounter any issues, they are likely related to:
- Browser localStorage limits (5-10MB typically)
- Very old browsers not supporting modern CSS features

## ♿ Accessibility Features

- Semantic HTML5 elements (`<nav>`, `<section>`, `<footer>`)
- Sufficient color contrast ratios (WCAG AA compliant)
- Keyboard navigation support
- Focus indicators on interactive elements
- Form labels properly associated with inputs
- Required fields marked with asterisk (*)

## 📱 Browser Compatibility

- Chrome/Edge: ✅ (Latest 2 versions)
- Firefox: ✅ (Latest 2 versions)
- Safari: ✅ (Latest 2 versions)
- Mobile browsers: ✅ (iOS Safari, Chrome Mobile)

## 🚀 Deployment

### Deploy to Netlify (Recommended)
1. Build the project: `npm run build`
2. Deploy the `dist/` folder to Netlify
3. Configure redirects for SPA routing

### Deploy to Vercel
1. Install Vercel CLI: `npm i -g vercel`
2. Run: `vercel`
3. Follow prompts

## 📄 License

This project is part of the HNG Internship Task 2 assessment.

## 👨‍💻 Author

Created for HNG Task 2 - Vue.js Implementation

## 🔗 Links

- [Vue.js Documentation](https://vuejs.org/)
- [Vue Router Documentation](https://router.vuejs.org/)
- [Vite Documentation](https://vitejs.dev/)

---

**Note**: This is the Vue.js implementation. React and Twig versions are separate implementations with identical functionality and design.