# Worthfinding Studio

A web application where clients design their own website or app by answering guided questions, receive suggestions and an automatically generated design blueprint, and talk to the Worthfinding team, who monitor and join those conversations from a secure employee portal. It acts as the bridge between the client and the employee.

---

## 🚀 Key Features Implemented

### A. Accounts and Authentication
- **Client registration & login**: Email, name, company, secure password hashing with bcrypt, session cookies.
- **Consent Tracking**: Mandatory consent checkbox: *"conversations can be reviewed by Worthfinding staff"* with timestamp recorded on user registration (`consentAt`).
- **Separate Employee & Admin Login**: Dedicated `/staff/login` portal with shorter session lifetime.
- **Role-Based Access Control**: `CLIENT`, `EMPLOYEE`, `ADMIN` with server-side authorization enforcement.

### B. Client Input Collection
- **Guided 4-Step Brief**: Basics, Feature Picker, Look & Feel, and Review.
- **Completeness scoring**: Real-time progress bar tracking brief completeness.
- **Auto-Save**: Automatic debounced brief synchronization to the database (`PATCH /api/projects/:id/brief`).
- **Style tokens**: Presets & color palette selection, corner radius styling, font pairings, and mood tags.

### C. Suggestions Engine
- **Rule-based suggestions**: Reacts to brief choices (e.g. online stores suggest payments, search, structured data; booking sites suggest appointment calendars).
- **Interactive Actions**: "Add to brief" and "Dismiss" buttons per suggestion.
- **Findability Suggestions**: Rich JSON-LD Schema, Google Business profile advice, OpenGraph cards.

### D. Automated Design Generation
- **Deterministic Blueprint Builder**: Generates sitemap, sections per page, hero headline, supporting copy, delivery phases, and SEO plans without requiring third-party APIs.
- **AI Refinement pass**: Optional Anthropic Claude refinement with deterministic fallbacks.
- **Interactive Live Mockup**: Rendered directly from brief tokens (colors, font pairing, corner radiuses, sitemap pages).
- **Exports**: Instant download of the strategic blueprint as **Markdown** or standalone **HTML Mockup**.
- **Send for Review**: Handoff flow updating project status to `SUBMITTED` for staff review.

### E. Client & Employee Conversation (The Bridge)
- **Live Chat Bridge**: Dedicated thread per client project.
- **AI Assistant**: Converses with clients with brief context under 110 words, focusing on findability and business goals.
- **Talk to a team member**: Client hand-off action that pauses AI and flags the project for staff.
- **Staff Live Replies**: Staff can join any conversation in real time.
- **Review Notice**: Persistent on-screen notice under composer alerting users that staff review conversations.

### F. Employee Portal
- **Dashboard**: Stats cards (Total clients, Needs reply, Awaiting review, In progress).
- **Filters & Search**: Filter by status (`DRAFT`, `SUBMITTED`, `REVIEW`, `PROGRESS`, `DELIVERED`) and search by client/project name.
- **Brief & Mockup Inspection**: View client answers and live interactive preview.
- **Internal Notes**: Confidential notes visible exclusively to staff (`EMPLOYEE` / `ADMIN`), completely isolated from client API endpoints.
- **Security Audit Log**: Tracks who viewed conversations, changed project statuses, or added notes.
- **Admin Management**: `/admin/employees` for provisioning and managing staff accounts.

---

## 📦 Local Setup & Execution

### 1. Requirements
- Node.js 18+ and npm
- Windows PowerShell / Bash

### 2. Run the Web Application
```bash
cd worthfinding-studio
npm run dev
```
The app will run at `http://localhost:3000`.

### 3. Run the Realtime Server (Optional)
```bash
npx tsx src/server/realtime.ts
```

---

## 🔑 Demo Credentials (Pre-seeded)

| Role | Email | Password | Access URL |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@worthfinding.com` | `admin123` | `http://localhost:3000/staff/login` |
| **Staff / Employee** | `alex@worthfinding.com` | `employee123` | `http://localhost:3000/staff/login` |
| **Client** | `client@example.com` | `client123` | `http://localhost:3000/login` |

---

## 🧪 Verification & Tests
Run automated unit tests for the suggestions engine and deterministic blueprint builder:
```bash
npx tsx src/lib/verify.ts
```
