# Developer Guide 2025-2026

> Onboarding and reference guide for developers, interns, and replacements joining any active project.
> Last updated: May 2026

---

## Table of Contents

- [Overview](#overview)
- [Organization 1 - Internal (Accqrate HCM)](#organization-1---internal-accqrate-hcm)
  - [GitHub Workflow](#github-workflow)
  - [Team Contacts](#team-contacts---accqrate)
  - [Tech Stack & Setup](#tech-stack--setup---accqrate)
- [Organization 2 - Hight.io](#organization-2---hightio)
  - [Team Contacts](#team-contacts---hightio)
  - [JDOX Studio (Web)](#jdox-studio-web)
  - [JDOX Folder Scanner App](#jdox-folder-scanner-app)
  - [JDOX Folder Scanner Service](#jdox-folder-scanner-service-windows)
  - [Reducr.io](#reducrio)
  - [Oratix.io](#oratixio)
  - [OBOT](#obot)
  - [Hight Components Library](#hight-components-library)
- [Quick Reference - All Projects](#quick-reference---all-projects)

---

## Overview

This guide covers all active projects across two organizations. Each section has team contacts, tech stack, local setup, branching strategy, and deployment notes.

---

## Organization 1 - Internal (Accqrate HCM)

## Accqrate HCM (Web)

The internal company platform used to manage HCM (Human Capital Management) workflows. Used by HR, Employees, and Managers.

---

### GitHub Workflow

| Environment | Branch | Purpose |
| --- | --- | --- |
| Development | `test_stage` | Active dev / integration |
| QA | `qa` | Quality assurance testing |
| Production | `main` / `production` | Live environment |

**Branching Rules for Interns / Devs:**

1. Create a personal branch named after yourself (e.g., `harshitha-feature-xyz`)
2. Build and optionally demo your changes
3. Get review and merge into `test_stage` (dev environment)
4. QA to Production follows from there via leads

```
your-name-branch -> test_stage (dev) -> qa -> production
```

**Repos:**

| Repo | Link |
|---|---|
| Frontend | [HR_HCM_FRONTEND](https://github.com/mcbitss/HR_HCM_FRONTEND) |
| Backend | [HCM_HR_Backend](https://github.com/mcbitss/HCM_HR_Backend) |

---

### Team Contacts - Accqrate

#### Leads

| Name | Role |
|---|---|
| Anthony Raj | Product Owner |
| Abhinav | DevOps + Infrastructure |
| Ranjith Ramesh | Lead |
| Santhosh Kumar | Lead |

#### Support & Team

| Area | People |
|---|---|
| Issues / Raising Tickets | Anand, Santhosh |
| Backend | Surendar, Kabhil, Pratik, Sneha, Kaviya, Krishna, Sanjai |
| Frontend | Vikram, Anand |

---

### Tech Stack & Setup - Accqrate

#### Stack

| Layer | Technology |
|---|---|
| Frontend | React, TypeScript, Ant Design (UI Library) |
| Backend | Node.js, Express |
| Database | MongoDB |
| Full Stack | MERN |
| Node Version | **v20** (both FE & BE) |

#### Frontend Local Setup

```bash
# 1. Clone the frontend repo
git clone https://github.com/mcbitss/HR_HCM_FRONTEND.git
cd HR_HCM_FRONTEND

# 2. Use Node v20
nvm use 20

# 3. Install dependencies
npm install

# 4. Start the dev server
npm start
```

> Make sure you have your local environment variables configured before running. Ask a lead or support contact for the `.env` values.

#### Backend Local Setup

```bash
# 1. Clone the backend repo
git clone https://github.com/mcbitss/HCM_HR_Backend.git
cd HCM_HR_Backend

# 2. Use Node v20
nvm use 20

# 3. Install dependencies
npm install

# 4. Start the server
npm start  # or npm run dev
```

> Set up local MongoDB connection and all required environment variables before starting.

---

## Organization 2 - Hight.io

Hight.io is an external client organization with multiple active products. The team works across web apps, desktop apps, background services, and AI agents.

---

### Team Contacts - Hight.io

#### Leadership

| Name | Role |
|---|---|
| Jan Hox | Founder |
| Karthik Pandurangan | Product Owner (Swiftin) & POC |
| Glen McCallum | Tech Lead & POC |
| Kristian | Software Developer |

> If Jan is unavailable, Karthik is your go-to for setting up AWS and Vercel credentials.

---

## Global Standards - Hight.io Projects

These apply across all Hight.io projects unless noted otherwise.

| Standard | Value |
|---|---|
| Node Version | **v22** |
| Package Manager | **yarn** (NOT npm) |
| Common UI Lib | Hight-components (internal) |

---

## JDOX Studio (Web)

The main JDOX web application for document management and cloud workflows.

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vue.js, Nuxt 3 (latest), Vuetify (UI components) |
| Backend / DB | Amazon DynamoDB |
| Auth | AWS Cognito |
| Node Version | **v22** |
| Package Manager | **yarn** |
| Deployment | Vercel (auto-deploy on push to dev/prod branches) |

### Frontend Setup

```bash
# Use Node 22
nvm use 22

# Install dependencies (use yarn - NOT npm)
yarn install

# Start dev server
yarn dev
```

> Vercel login is required. Contact Karthik or Jan for credentials and access.

### Deployment - JDOX Studio

| Branch | Environment |
|---|---|
| `dev` | Development (auto-deploys via Vercel) |
| `prod` / `main` | Production (auto-deploys via Vercel) |

---

### JDOX API Architecture

JDOX Studio uses multiple separate API repos, each handling a different part of the app.

```
JDOX Studio (Frontend)
      |
      +-- JDOX-API          -> Main backend logic, S3 upload, user verification routes (Vercel)
      +-- JDOX-AZURE-API    -> Azure routes & logic (AWS Lambda)
      +-- JDOX-OCI-API      -> OCI upload verification
      +-- Hight-AI / Content API  -> AI & content features (separate repo)
      +-- Authenticator-hight    -> Auth service used across ALL Hight.io projects
```

#### API Details

| API Repo | Purpose | Hosting |
| --- | --- | --- |
| `JDOX-API` | Main backend - S3 upload, user verification, core routes | Vercel (auto-deploy dev + prod) |
| `JDOX-AZURE-API` | Azure-specific API routes and logic | AWS Lambda |
| `JDOX-OCI-API` | OCI upload verification logic | _(confirm hosting)_ |
| `Hight-AI / Content API` | AI and content features | Separate repo |
| `Authenticator-hight` | Authentication across all Hight.io projects | Separate repo |

> Each API has its own repo. Clone and configure env variables for each one you work on.

---

## JDOX Folder Scanner App

An Electron desktop app for scanning folders and uploading files. Uses the same JDOX API endpoints with a few additional Folder Scanner-specific routes.

### Tech Stack

| Layer | Technology |
|---|---|
| Framework | Electron |
| Runtime | Node.js |
| APIs | Same as JDOX-API + Folder Scanner-specific routes |

### Build & Deployment

The app is compiled into `.exe` installer files using two company configurations:

| Build | Used For |
|---|---|
| **Hight version** | Internal testing |
| **Spirax version** | Customer (Spirax is the client) |

When testing, always use the Hight version. Spirax is what gets deployed to the customer.

#### Adding a New Customer Config

When a new customer needs their own version:

1. Create a separate `.exe` and Node service
2. Configure based on their `org_id` and other org-specific config values
3. Test with internal (Hight) build first

#### GitHub Releases (Distribution)

Once the build is finalized and tested:

```
1. Build the .exe
2. Create a GitHub Release on the customer's repo
3. Upload the .exe AND the .yml file to the release
4. Customers receive auto-updates from this GitHub release
```

The `.yml` file is required for the auto-update mechanism to work.

---

## JDOX Folder Scanner Service (Windows)

A Windows background service version of the Folder Scanner. Runs via `services.msc` and handles file uploads asynchronously, even across Windows logins and remote sessions.

### Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js (compiled to Windows Service) |
| Installer | **InnoSetup Compiler** (creates the `.exe` installer package) |

### Key Differences from the Desktop App

| Feature | Folder Scanner App | Folder Scanner Service |
| --- | --- | --- |
| UI | Electron GUI | No UI - background Windows service |
| Runs when | App is open | Always (runs via `services.msc`) |
| Remote sessions | Limited | Works across logins and remote sessions |
| Upload mode | Interactive | Async background |

### Build & Distribution

```
1. Build Node.js service
2. Compile with InnoSetup -> creates installer .exe
3. Distribute installer to customer
4. Service registers in Windows services (services.msc)
```

---

## Reducr.io

A Hight.io web product.

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Next.js |
| Backend | Separate API (Node-based) |
| Node Version | **v22** |
| Package Manager | **yarn** |

### Setup

```bash
nvm use 22
yarn install
yarn dev
```

> Get environment variables from the team before running locally.

---

## Oratix.io

A Hight.io web product with AI-powered file extraction via AWS Bedrock.

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Next.js |
| Backend | Node.js API, deployed to AWS |
| AI Layer | **AWS Bedrock Agent** (file extraction + business rules) |
| Node Version | **v22** |
| Package Manager | **yarn** |

### Setup

```bash
nvm use 22
yarn install
yarn dev
```

> AWS credentials and Bedrock agent config must be set in environment variables. Contact Glen or Karthik.

---

## OBOT

A Hight.io product with the same architecture as Oratix. AWS-deployed Node backend with a Bedrock AI agent.

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Next.js |
| Backend | Node.js API, deployed to AWS |
| AI Layer | **AWS Bedrock Agent** (file extraction + rules) |
| Node Version | **v22** |
| Package Manager | **yarn** |

### Setup

```bash
nvm use 22
yarn install
yarn dev
```

---

## Hight Components Library

An internal UI component library shared across Hight.io projects. Contains input elements, buttons, and other reusable UI parts.

### Usage

This library is used as a dependency in other Hight.io projects. When making changes, updates must be published and projects must update their dependency version.

| Detail | Notes |
|---|---|
| Type | UI Component Library |
| Used By | JDOX Studio, Reducr, Oratix, OBOT, and others |
| Components | Input elements, buttons, and shared UI parts |

---

## Quick Reference - All Projects

| Project | Org | Frontend | Backend | DB | Node | Pkg Mgr | Deployment |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Accqrate HCM | Internal | React + TypeScript + Ant Design | Express | MongoDB | 20 | npm | GitHub branches |
| JDOX Studio | Hight.io | Vue.js, Nuxt3, Vuetify | Multi-API (see above) | DynamoDB | 22 | yarn | Vercel (auto) |
| Reducr.io | Hight.io | React, Next.js | Node API | TBD | 22 | yarn | TBD |
| Oratix.io | Hight.io | React, Next.js | Node API + AWS | TBD | 22 | yarn | AWS |
| OBOT | Hight.io | React, Next.js | Node API + AWS | TBD | 22 | yarn | AWS |
| Folder Scanner App | Hight.io | Electron | Node (JDOX API) | - | 22 | yarn | GitHub Releases (.exe) |
| Folder Scanner Service | Hight.io | None (background) | Node Windows Service | - | 22 | yarn | InnoSetup installer |
| Hight Components | Hight.io | Component Library | - | - | 22 | yarn | npm package |

---

## Environment Variables Checklist

Get these values from a lead or senior dev before starting locally. Never commit `.env` files.

### Accqrate HCM

- [ ] MongoDB connection string
- [ ] API base URL
- [ ] Auth tokens / secrets

### JDOX Studio & APIs

- [ ] AWS credentials (DynamoDB, S3, Lambda)
- [ ] AWS Cognito pool details
- [ ] Vercel project tokens (if deploying)
- [ ] Org ID and config for Folder Scanner builds

### Oratix / OBOT

- [ ] AWS Bedrock agent ID and region
- [ ] AWS access keys
- [ ] API endpoint URLs
