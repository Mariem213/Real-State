# 🏠 RealEstate Platform

A full-stack real estate web application built with **React 19**, **Firebase**, and **Vite** — supporting bilingual (Arabic / English) navigation, property listings, investment applications, job careers, and an admin dashboard.

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Target Users](#target-users)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [UI Screens](#ui-screens)
- [Project Structure](#project-structure)
- [Database Schemas](#database-schemas)
- [Getting Started](#getting-started)
- [Environment & Firebase Setup](#environment--firebase-setup)
- [Deployment](#deployment)

---

## About the Project

**RealEstate** is a modern, bilingual (Arabic ↔ English) real estate platform that connects property buyers, sellers, and investors with real estate agents and administrators. The platform supports full RTL (right-to-left) layout for Arabic users, property browsing with detail pages, investment opportunity applications, property sell requests, and a career portal — all managed through a secure admin dashboard.

The application is a **Single Page Application (SPA)** powered by React Router v7, with Firebase as the backend (authentication, Firestore database, and file storage).

---

## Target Users

| User Type | Description |
|-----------|-------------|
| **Visitors / Buyers** | Browse properties, view details, and submit purchase inquiries |
| **Sellers** | Submit property listings for review by the admin |
| **Investors** | Apply for real estate investment opportunities |
| **Job Seekers** | Submit career / job applications through the platform |
| **Admins** | Manage all submissions (sell requests, investment requests, job applications) via a protected dashboard |

---

## Tech Stack

| Category | Technology |
|----------|------------|
| **Frontend Framework** | React 19 |
| **Build Tool** | Vite 8 |
| **Routing** | React Router DOM v7 |
| **Backend / Auth** | Firebase 12 (Auth, Firestore, Storage) |
| **Analytics** | Firebase Analytics (conditionally loaded) |
| **Icons** | Lucide React |
| **PDF Generation** | jsPDF |
| **Styling** | Custom CSS with RTL support |
| **Language** | JavaScript (ESM), JSX |
| **Node Version** | 20.x |

---

## Features

### Public
- 🌐 **Bilingual support** — Arabic (RTL) and English (LTR), persisted via localStorage
- 🏠 **Property listings** — Browse available properties with filters
- 📄 **Property detail pages** — Full info, images, and contact options

### Authenticated Users
- 🔐 **Login & Registration** — Firebase email/password authentication
- 💼 **Job Application** — Submit a career application (`/careers`)
- 💰 **Investment Application** — Apply for investment opportunities (`/investment`)
- 🏷️ **Sell Property** — List a property for sale (`/sell`)
- 🛒 **Buy Property** — Browse and view properties (`/buy`, `/buy/:id`)

### Admin Dashboard (`/admin`)
- 📊 **Dashboard overview** — Summary of all activity
- 📋 **Investment Requests** — Review and manage investment applications
- 🏘️ **Sell Requests** — Review and manage property sell submissions
- 👤 **Job Applications** — Review and manage career submissions
- 🔒 **Protected routes** — All admin routes require authentication

---

## UI Screens

| Route | Screen | Access |
|-------|--------|--------|
| `/` | Home / Landing Page | Public |
| `/login` | Login Page | Public |
| `/signup` | Registration Page | Public |
| `/buy` | Property Listings | Authenticated |
| `/buy/:id` | Property Detail | Authenticated |
| `/sell` | Submit Property for Sale | Authenticated |
| `/investment` | Investment Application Form | Authenticated |
| `/careers` | Job Application Form | Authenticated |
| `/admin` | Admin Dashboard | Authenticated |
| `/admin/investment-requests` | Investment Requests Manager | Authenticated |
| `/admin/sell-requests` | Sell Requests Manager | Authenticated |
| `/admin/job-applications` | Job Applications Manager | Authenticated |

> **Note:** The Navbar and Footer are hidden on login, signup, and all `/admin/*` pages for a cleaner auth/admin experience.

---

## Project Structure

```
real-state-project/
├── index.html                  # App shell with RTL pre-init script
├── package.json
├── vite.config.js
└── src/
    ├── main.jsx                # React entry point
    ├── App.jsx                 # Router, layout, and route definitions
    ├── firebase.js             # Firebase app initialization & exports
    ├── styles/
    │   └── index.css           # Global styles (LTR + RTL)
    ├── context/
    │   ├── AuthContext.jsx     # Firebase auth state provider
    │   └── LanguageContext.jsx # AR/EN language & direction provider
    ├── components/
    │   ├── Navbar.jsx          # Top navigation bar
    │   ├── Footer.jsx          # Site footer
    │   └── ProtectedRoute.jsx  # Auth guard for private routes
    └── pages/
        ├── Index.jsx               # Landing / home page
        ├── Login.jsx               # Login form
        ├── Register.jsx            # Registration form
        ├── Buy.jsx                 # Property listings
        ├── PropertyDetail.jsx      # Single property detail
        ├── SellProperty.jsx        # Sell property form
        ├── InvestmentApplication.jsx  # Investment form
        ├── JobApplication.jsx      # Career application form
        ├── Dashboard.jsx           # Admin overview
        ├── InvestmentRequests.jsx  # Admin: investment requests
        ├── SellRequests.jsx        # Admin: sell requests
        └── JobApplicationsAdmin.jsx   # Admin: job applications
```

---

## Database Schemas

The application uses **Firebase Firestore** (NoSQL). Below are the inferred collection schemas:

### `users` collection
Managed by Firebase Authentication. Extended profile data may be stored here.

```
users/{uid}
├── uid: string
├── email: string
├── displayName: string
├── createdAt: timestamp
└── role: string           // "user" | "admin"
```

---

### `properties` collection
Stores property listings available for browsing and purchase.

```
properties/{propertyId}
├── title: string
├── description: string
├── price: number
├── location: string
├── area: number           // in m²
├── type: string           // "apartment" | "villa" | "land" | etc.
├── images: string[]       // Firebase Storage URLs
├── status: string         // "available" | "sold" | "pending"
├── createdAt: timestamp
└── createdBy: string      // admin uid
```

---

### `sellRequests` collection
Submitted by authenticated users who want to list their property.

```
sellRequests/{requestId}
├── userId: string
├── userEmail: string
├── propertyTitle: string
├── propertyType: string
├── location: string
├── area: number
├── price: number
├── description: string
├── images: string[]       // Firebase Storage URLs
├── status: string         // "pending" | "approved" | "rejected"
└── submittedAt: timestamp
```

---

### `investmentRequests` collection
Submitted by users interested in investment opportunities.

```
investmentRequests/{requestId}
├── userId: string
├── userEmail: string
├── fullName: string
├── phone: string
├── investmentAmount: number
├── investmentType: string
├── message: string
├── status: string         // "pending" | "reviewed" | "approved"
└── submittedAt: timestamp
```

---

### `jobApplications` collection
Submitted by users applying for careers at the company.

```
jobApplications/{applicationId}
├── userId: string
├── userEmail: string
├── fullName: string
├── phone: string
├── position: string
├── experience: string
├── cvUrl: string          // Firebase Storage URL (uploaded PDF/doc)
├── coverLetter: string
├── status: string         // "pending" | "reviewed" | "hired" | "rejected"
└── submittedAt: timestamp
```

---

## Getting Started

### Prerequisites

- **Node.js** `>= 20.x` ([Download](https://nodejs.org/))
- **npm** `>= 9.x`
- A **Firebase** project (see setup below)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Mariem213/Real-State.git
cd real-state-project

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

The app will be available at `http://localhost:5173`

### Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start local development server (HMR enabled) |
| `npm run build` | Build for production (outputs to `dist/`) |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint on all source files |

---

## Environment & Firebase Setup

The Firebase config is currently hardcoded in `src/firebase.js`. For production, it is strongly recommended to move all sensitive keys to environment variables:

### 1. Create a `.env` file in the project root

```env
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX
```

### 2. Update `src/firebase.js`

```js
const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
  measurementId: import.meta.env.VITE_FIREBASE_MEASUREMENT_ID,
}
```

> ⚠️ **Never commit `.env` files to version control.** Add `.env` to your `.gitignore`.

### 3. Firebase Services to Enable

In the [Firebase Console](https://console.firebase.google.com/):

- **Authentication** → Enable Email/Password provider
- **Firestore Database** → Create in production mode, apply security rules
- **Storage** → Enable for file/image uploads
- **Analytics** → Optional (auto-loaded if supported)

---

## Deployment

### Deploy to Firebase Hosting

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize hosting (select your project)
firebase init hosting
# Set public directory to: dist
# Configure as SPA: Yes
# Overwrite index.html: No

# Build and deploy
npm run build
firebase deploy --only hosting
```

### Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# For production
vercel --prod
```

> Make sure to add all `VITE_FIREBASE_*` environment variables in your Vercel project settings under **Settings → Environment Variables**.

### Deploy to Vercel

1. Push your repository to GitHub
2. Connect it in [Vercel](https://vercel.com/)
3. Set **Build command:** `npm run build`
4. Set **Publish directory:** `dist`
5. Add all `VITE_FIREBASE_*` environment variables under **Site Settings → Environment Variables**
6. Add a `_redirects` file in `/public`:
   ```
   /*  /index.html  200
   ```

Link of Real-State Project :
[Real-State](https://real-state-gvnb.vercel.app/)

---

## Security Notes

- All routes under `/buy`, `/sell`, `/investment`, `/careers`, and `/admin/*` are protected by the `ProtectedRoute` component, which checks Firebase Auth state before rendering.
- Admin routes currently share the same auth guard as user routes. Consider adding role-based access control (checking a `role` field in Firestore) to restrict `/admin` to admins only.
- Firestore security rules should be configured to restrict read/write access per collection and user role.

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## License

This project is private and proprietary. All rights reserved.
