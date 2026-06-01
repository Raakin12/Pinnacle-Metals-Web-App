# Pinnacle Metals Web App

A modern React and TypeScript web application for Pinnacle Metals, a UK-based scrap metal recycling and collection business. The app combines a polished public marketing website with authenticated customer and admin portal features for account access, document uploads, compliance review, metal price visibility, and user management.

This project was built as a production-style frontend using Vite, React Router, TypeScript, reusable UI layouts, authenticated API calls, protected routes, dashboard views, and admin workflows.

## Tech Stack

- React
- TypeScript
- Vite
- React Router
- CSS Modules / custom CSS
- Lucide React icons
- Fetch API wrapper
- Environment-based API configuration
- Vercel deployment configuration

## Core Features

### Public Marketing Website

The public-facing website presents Pinnacle Metals as a professional scrap metal recycling and collection service.

It includes:

- Home section
- Service overview
- About section
- Materials accepted
- Recycling process
- Testimonials
- Contact details
- Mobile navigation
- Responsive layout
- WhatsApp/contact widget support

### Authentication Flow

The app includes a full account access flow for customers and admins.

Supported flows include:

- Create account
- Login
- Logout
- Forgot password
- Reset password
- Email verification
- Protected dashboard routes
- Role-based admin navigation

### Customer Dashboard

Authenticated customers can access a private dashboard that displays:

- Account verification status
- Account ID
- Uploaded compliance documents
- Profile details
- Live copper/base metal pricing
- Document upload controls
- Document status tracking

### Document Upload and Compliance

Customers can upload required compliance files through the dashboard.

Supported document categories include:

- Photo ID
- Proof of address
- Business document
- General document upload

The dashboard tracks document metadata such as:

- File name
- Document type
- Upload date
- Approval status
- Download/view action

### Admin Dashboard

The admin area provides internal tools for reviewing the platform.

Admin features include:

- Total user count
- Verified and unverified user counts
- Total document count
- Pending document count
- Approved document count
- Rejected document count
- Live copper price display
- Base metal pricing controls
- Price synchronization status

### Admin User Management

Admins can manage customer accounts and compliance documents.

The admin user management screen supports:

- User directory
- Verification status filtering
- Search by email or account ID
- Pagination
- User profile inspection
- Compliance vault review
- Document approval
- Document reset to pending
- Document deletion
- User deletion
- CSV export of users
- Global document review queue

### Metal Price Display

The app connects to backend pricing endpoints to display market-linked metal prices.

Displayed price data includes:

- Copper price
- Aluminum price
- Lead price
- Zinc price
- Last updated timestamp
- Market feed/source status

### Account Settings

Customers can update profile and security details, including:

- Full name
- Phone number
- Business name
- City
- Password change

## Application Flow

```text
Public Website
      |
      |-- Home
      |-- About
      |-- Services
      |-- Materials
      |-- Process
      |-- Contact
      |
      v
Create Account / Login
      |
      v
Authenticated Area
      |
      |-- Customer Dashboard
      |     |-- View account status
      |     |-- Upload compliance documents
      |     |-- View metal prices
      |     |-- Track document status
      |
      |-- Settings
      |     |-- Update profile
      |     |-- Change password
      |
      |-- Admin Dashboard
            |-- View platform stats
            |-- Manage metal prices
            |-- Review documents
            |-- Manage users
```

## Project Structure

```text
Pinnacle-Metals-Web-App
├── public
├── src
│   ├── assets
│   ├── lib
│   │   ├── api.ts
│   │   ├── AuthContext.tsx
│   │   └── types.ts
│   ├── pages
│   │   ├── AdminDashboardPage.tsx
│   │   ├── AdminUserManagement.tsx
│   │   ├── ApplyPage.tsx
│   │   ├── DashboardPage.tsx
│   │   ├── ForgotPage.tsx
│   │   ├── HomePage.tsx
│   │   ├── LoginPage.tsx
│   │   ├── PrivacyPage.tsx
│   │   ├── ResetPage.tsx
│   │   ├── SettingsPage.tsx
│   │   ├── TermsPage.tsx
│   │   └── VerifyEmailPage.tsx
│   ├── ui
│   │   ├── DashboardShell.tsx
│   │   ├── Layout.tsx
│   │   ├── RequireAuth.tsx
│   │   ├── RequireAdmin.tsx
│   │   ├── RequireUser.tsx
│   │   └── WhatsAppWidget.tsx
│   ├── App.tsx
│   ├── main.tsx
│   ├── App.css
│   └── index.css
├── .env.example
├── package.json
├── vite.config.ts
├── tsconfig.json
└── vercel.json
```

## Main Pages

### Home Page

The home page is the public-facing Pinnacle Metals website. It presents the business, services, recycling process, accepted materials, customer trust signals, testimonials, and contact information.

### Apply Page

The apply page allows new users to create an account by submitting basic profile details such as name, email, phone number, city, and password.

### Login Page

The login page authenticates users and redirects them based on role. Admin users are routed to the admin dashboard, while regular customers are routed to the customer dashboard.

### Customer Dashboard

The customer dashboard displays account status, live metal pricing, account details, and uploaded compliance documents. Customers can upload documents and track whether files are pending or approved.

### Admin Dashboard

The admin dashboard shows high-level platform statistics, document review counts, verified/unverified user counts, and live metal price data. It also includes controls for updating base metal pricing.

### Admin User Management

The admin user management page gives admins a full compliance review interface. Admins can search users, filter by verification status, review uploaded documents, approve documents, reset document status, delete documents, verify users, reject users, delete accounts, and export user records.

### Settings Page

The settings page allows authenticated users to update profile information and change their password.

### Forgot and Reset Password Pages

The password recovery flow allows users to request a reset link and create a new password using a reset token.

### Verify Email Page

The email verification page reads a verification token from the URL and confirms the user's email through the backend API.

## API Integration

The frontend communicates with a backend API using a reusable API wrapper located in:

```text
src/lib/api.ts
```

The API wrapper handles:

- Runtime API base URL resolution
- JSON request headers
- Bearer token injection from local storage
- FormData upload requests
- Error handling
- JSON and text responses
- Credential forwarding

The app supports runtime API configuration through `VITE_API_URL`.

## Environment Variables

Create a `.env` file in the root of the project.

```env
VITE_API_URL=http://localhost:4000
VITE_GOOGLE_MAPS_EMBED_API_KEY=your_google_maps_embed_api_key
```

For production, `VITE_API_URL` should point to the deployed backend API.

Do not commit real API keys or secrets to GitHub.

## Data Types

The frontend defines shared TypeScript types for users, profiles, documents, and complaints.

### User

```ts
type User = {
  _id: string;
  email: string;
  role: "user" | "admin";
  accountId: string;
  verificationStatus: "unverified" | "verified";
};
```

### Profile

```ts
type Profile = {
  fullName: string;
  phone?: string;
  addressLine1?: string;
  city?: string;
  postcode?: string;
  businessName?: string;
};
```

### Document

```ts
type Document = {
  _id: string;
  userId: string;
  type: "all" | "id" | "proof_of_address" | "business_doc";
  originalName: string;
  storageName: string;
  mimeType: string;
  size: number;
  status: "pending" | "approved" | "rejected";
  cloudinaryUrl?: string;
  uploadedAt: string;
};
```

## Key API Endpoints Used

### Auth

```http
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET /api/auth/me
POST /api/auth/forgot-password
POST /api/auth/reset-password
GET /api/auth/verify-email
```

### Account

```http
GET /api/account
PUT /api/account/profile
POST /api/account/change-password
```

### Documents

```http
GET /api/documents
POST /api/documents
```

### Prices

```http
GET /api/prices/copper
GET /api/prices/base-metals
GET /api/prices/materials
```

### Admin

```http
GET /api/admin/stats
GET /api/admin/users
GET /api/admin/users/:id
PATCH /api/admin/users/:id/verification
DELETE /api/admin/users/:id
GET /api/admin/users/export
GET /api/admin/documents
PATCH /api/admin/documents/:id/status
DELETE /api/admin/documents/:id
GET /api/admin/pricing
PATCH /api/admin/pricing
```

## Route Protection

The app uses route guard components to separate public, authenticated, customer, and admin-only areas.

```text
RequireAuth
RequireUser
RequireAdmin
```

This keeps sensitive customer dashboard pages and admin tools separate from the public marketing website.

## UI and Styling

The project uses a custom visual style built with React components and CSS.

The UI includes:

- Public website layout
- Sticky responsive header
- Mobile menu overlay
- Footer
- Dashboard shell
- Collapsible sidebar
- Admin dashboard cards
- User management tables
- Compliance vault interface
- Upload controls
- Auth page styling
- Reusable protected route wrappers

## Running Locally

### 1. Clone the repository

```bash
git clone https://github.com/Raakin12/Pinnacle-Metals-Web-App.git
cd Pinnacle-Metals-Web-App
```

### 2. Install dependencies

```bash
npm install
```

### 3. Create environment file

```bash
cp .env.example .env
```

Update the API URL:

```env
VITE_API_URL=http://localhost:4000
VITE_GOOGLE_MAPS_EMBED_API_KEY=your_google_maps_embed_api_key
```

### 4. Run the development server

```bash
npm run dev
```

### 5. Open the app

```text
http://localhost:5173
```

## Available Scripts

```bash
npm run dev
```

Runs the app locally using Vite.

```bash
npm run build
```

Builds the TypeScript project and creates a production Vite build.

```bash
npm run preview
```

Previews the production build locally.

```bash
npm run lint
```

Runs ESLint across the project.

## What This Project Demonstrates

This project demonstrates practical frontend engineering skills across:

- React and TypeScript development
- Vite project structure
- React Router navigation
- Protected route patterns
- Role-based UI rendering
- Authenticated API requests
- Token-based frontend auth handling
- Customer dashboard development
- Admin dashboard development
- File upload workflows
- Compliance document review UI
- Data tables and pagination
- CSV export
- Form handling
- Runtime environment configuration
- Responsive business website design
- Deployment-ready frontend configuration

## Future Improvements

- Add automated tests for auth, dashboard, and admin flows
- Add stronger form validation with Zod or React Hook Form
- Add loading skeletons for dashboard cards and tables
- Add better toast notifications for success/error states
- Add screenshots and a short demo GIF
- Add chart visualizations for pricing history
- Add accessibility improvements for all interactive components
- Add API documentation for the backend endpoints
- Add a full backend repository link when available

## Project Status

Completed frontend portfolio project demonstrating a professional business website with authenticated customer dashboard functionality, admin compliance tools, live metal pricing UI, document upload workflows, and role-based portal access.
