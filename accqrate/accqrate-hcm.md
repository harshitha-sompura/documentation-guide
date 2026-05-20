[← Back to Index](../index.md)

# Accqrate HCM (Web)

HCM platform used by HR, Employees, and Managers for workforce management workflows.

---

## Tech Stack

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Ant Design](https://img.shields.io/badge/Ant%20Design-0170FE?style=for-the-badge&logo=ant-design&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-404D59?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS%20S3-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![npm](https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white)

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Ant Design 5, MUI 6, Bootstrap 5 |
| Backend | Node.js, Express 4 |
| Database | MongoDB (Mongoose ODM) |
| File Storage | AWS S3 |
| Node Version | **v20** (both FE & BE) |
| Package Manager | **npm** |

---

## Repositories

| Repo | Link |
|---|---|
| Frontend | [HR_HCM_FRONTEND](https://github.com/mcbitss/HR_HCM_FRONTEND) |
| Backend | [HCM_HR_Backend](https://github.com/mcbitss/HCM_HR_Backend) |

---

## System Topology

```
┌─────────────────────────────────────────────────────┐
│                    CLIENT LAYER                      │
│  HR_HCM_FRONTEND (React 18 + TypeScript, Port 3001) │
└────────────────────────┬────────────────────────────┘
                         │ HTTP/REST (Axios)
                         │ Bearer JWT token
┌────────────────────────▼────────────────────────────┐
│                    API LAYER                         │
│  HCM_HR_Backend (Node.js + Express 4, Port 7003)    │
└────────────────────────┬────────────────────────────┘
                         │ Mongoose ODM
┌────────────────────────▼────────────────────────────┐
│                   DATA LAYER                         │
│            MongoDB  +  AWS S3 (documents)            │
└─────────────────────────────────────────────────────┘
```

---

## Backend - HCM_HR_Backend

**Pattern:** Feature-based Modular MVC (Router -> Controller -> Model)  
**Entry point:** `src/app.js` - initializes Express, Mongoose, CORS, cron scheduler, Puppeteer (PDF), static file serving.

```
src/
├── app.js                 # Express server bootstrap
├── config.js              # dotenv-safe config (exports all env vars)
├── api/
│   ├── index.js           # Central router (~80+ sub-routers, all under /api)
│   ├── auth/              # Login (password + MFA)
│   ├── users/
│   ├── absenceManagement/ # Leave, holidays, balances, policies
│   ├── payroll/
│   ├── timeTracker/
│   ├── masterData/        # 40+ lookup/reference tables
│   ├── workflows/
│   ├── hrManagement/
│   ├── leadsManagement/
│   └── ...                # Each folder: index.js + controller.js + model.js
└── services/
    ├── passport/          # Auth middleware (password strategy + JWT strategy)
    ├── express/           # Global middleware (CORS, compression, Morgan, body parser)
    ├── mongoose/          # DB connection + schema factory
    ├── cron/              # 8 scheduled jobs (daily, weekly, monthly)
    ├── email/             # Nodemailer + 50+ EJS templates
    ├── jwt/               # Token sign/verify
    └── s3/                # AWS S3 file storage
```

Each API module follows this pattern:

```
featureName/
├── index.js       # Express Router: mounts routes, applies auth middleware
├── controller.js  # Business logic, DB queries, response shaping
└── model.js       # Mongoose schema + model export
```

**Auth flow:** `POST /api/auth` -> email normalize -> MD5 password check -> issue JWT. MFA via `/api/auth/mfa-login`. All protected routes use `token({ required: true })` middleware (Passport JWT), which injects `req.user`, `req.company`, `req.network`.

**Multi-tenancy:** Every model carries `network + company` ObjectId refs. JWT payload provides these; middleware injects them into `req`, so controllers filter by them automatically.

**Cron jobs** (node-cron): visa/permit expiry alerts, leave balance updates, payroll processing (10th-25th nightly), timesheet reminders, monthly PF attendance reports.

**Key env vars:** `MONGO_URI`, `JWT_SECRET`, `AWS_*`, `MAIL_*`, `PORT=7003`

---

## Frontend - HR_HCM_FRONTEND

**Stack:** React 18 + TypeScript + Create React App (Webpack 5)  
**Entry point:** `src/index.tsx` -> `src/App.tsx` - wraps app in Redux Provider, i18next, Theme context, DnD provider, BrowserRouter.

```
src/
├── index.tsx              # DOM render, all providers
├── App.tsx                # Root: providers + AppRoutes
├── config.ts              # Runtime env config (API_URL etc.)
├── i18n.tsx               # i18next init (language files fetched from API)
├── themeContext.tsx        # Dark/Light theme context + localStorage persist
├── routes/
│   ├── index.tsx          # Auth-conditional route switch
│   ├── publicRoutesConfig.tsx    # Login, register, forgot-password
│   ├── innnerRoutes.tsx   # All /app/* protected routes
│   └── ProtectedRoute.tsx # ACCOUNTING_TOKEN guard
├── store/
│   ├── store.ts           # Redux store
│   ├── rootReducer.ts     # Combines all slices
│   ├── hooks.ts           # useAppDispatch, useAppSelector
│   └── slice/             # userInfo, masterInfo, roles, language, timeEntries, invoice...
├── Actions/               # createAsyncThunk actions (UserAction, WorkFlowAction)
├── Screens/               # Feature pages (by domain)
│   ├── Dashboard/
│   ├── HumanResources/    # AbsenceManagement, EmployeeDetails, Shifts...
│   ├── MasterData/        # 40+ master data screens
│   ├── Payroll/
│   ├── TimeSheet/
│   └── LeadManagement/
├── Components/            # Reusable UI (Formik fields, TableBoxGrid, Modals, MFA...)
├── Layout/                # Layout.tsx, Header.tsx, Sidebar.tsx, MenuJson.ts
├── util/
│   └── apiClient.ts       # Axios instance: auto-injects Bearer token, 401 -> redirect
└── Style/                 # global.scss, themes.scss, colors.scss, ant-overwrite.scss
```

**State management:** Redux Toolkit slices for server state; local `useState` for UI state; Context API for theme only.

**API layer:** Single `apiClient.ts` - Axios with request interceptor (adds `Authorization: Bearer <token>` from `localStorage.ACCOUNTING_TOKEN`) and response interceptor (401 -> `/login`).

**Forms:** Formik + Yup validation. Shared field components in `src/Components/Formik/` wrap Ant Design inputs.

**Styling:** SCSS + Ant Design 5 (primary) + MUI 6 (secondary) + Bootstrap 5 utilities. Dark/light via CSS custom properties toggled by `data-theme` on `<html>`.

**Code splitting:** `@loadable/component` wraps every route and heavy component - lazy loaded with `FullLoader`/`FieldLoader` HOCs.

**Key env vars:** `REACT_APP_API_URL=http://localhost:7013/api`, `REACT_APP_IMAGE_PATH`, `PORT=3001`

---

## Cross-Cutting Concerns

| Concern | Backend | Frontend |
| --- | --- | --- |
| Auth | Passport JWT, MD5 password | JWT in localStorage, axios interceptor |
| Multi-tenancy | network+company on all models | Stored in Redux userInfoSlice |
| i18n | Language resources served via API | react-i18next, keys fetched at login |
| Theming | User preference stored in DB | Context + CSS vars + localStorage |
| File storage | AWS S3 (multer upload + S3 SDK) | Axios blob download + file-saver |
| PDF | Puppeteer + pdf-lib | @react-pdf/renderer + react-pdf |
| Notifications | Nodemailer EJS templates + cron | Ant Design message/notification |
| Migrations | migrate-mongo (52 migrations) | - |

---

## Local Setup

### Backend

```bash
cp .env.example .env   # fill in MONGO_URI, JWT_SECRET, AWS_*, MAIL_*
npm install
npm run migrate        # run pending migrations
npm run seed           # optional: seed initial data
npm start              # nodemon on port 7003
```

### Frontend

```bash
cp .env.example .env   # set REACT_APP_API_URL=http://localhost:7003/api
npm install
npm start              # CRA dev server on port 3001
```

---

## GitHub Workflow

| Environment | Branch | Purpose |
| --- | --- | --- |
| Development | `test_stage` | Active dev / integration |
| QA | `qa` | Quality assurance testing |
| Production | `main` / `production` | Live environment |

**Branching:**

1. Create a personal branch (e.g., `harshitha-feature-xyz`)
2. Build and demo your changes
3. Merge into `test_stage`
4. QA to Production is handled by leads

```
your-name-branch -> test_stage -> qa -> production
```

---

## Team Contacts

### Leads

| Name | Role |
|---|---|
| Anthony Raj | Product Owner |
| Abhinav | DevOps + Infrastructure |
| Ranjith Ramesh | Lead |
| Santhosh Kumar | Lead |

### Support & Team

| Area | People |
|---|---|
| Issues / Raising Tickets | Anand, Santhosh |
| Backend | Surendar, Kabhil, Pratik, Sneha, Kaviya, Krishna, Sanjai |
| Frontend | Vikram, Anand |
