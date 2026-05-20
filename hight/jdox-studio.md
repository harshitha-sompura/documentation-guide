[← Back to Index](../index.md)

# JDOX Studio (Web)

Document management and cloud workflow platform.

---

## Tech Stack

![Vue.js](https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vue.js&logoColor=4FC08D)
![Nuxt.js](https://img.shields.io/badge/Nuxt.js-00C58E?style=for-the-badge&logo=nuxt.js&logoColor=white)
![Vuetify](https://img.shields.io/badge/Vuetify-1867C0?style=for-the-badge&logo=vuetify&logoColor=white)
![Amazon DynamoDB](https://img.shields.io/badge/DynamoDB-4053D6?style=for-the-badge&logo=Amazon%20DynamoDB&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)
![Yarn](https://img.shields.io/badge/Yarn-2188B6?style=for-the-badge&logo=yarn&logoColor=white)

| Layer | Technology |
|---|---|
| Frontend | Vue.js, Nuxt 3 (latest), Vuetify |
| Backend / DB | Amazon DynamoDB |
| Auth | AWS Cognito |
| Node Version | **v22** |
| Package Manager | **yarn** |
| Deployment | Vercel (auto-deploy on push) |

---

## Local Setup

```bash
# Use Node 22
nvm use 22

# Install dependencies (use yarn, NOT npm)
yarn install

# Start dev server
yarn dev
```

---

## Deployment

| Branch | Environment |
|---|---|
| `dev` | Development (auto-deploys via Vercel) |
| `prod` / `main` | Production (auto-deploys via Vercel) |

---

## API Architecture

JDOX Studio uses multiple separate API repos, each handling a different part of the app.

```
JDOX Studio (Frontend)
      |
      +-- JDOX-API             → Main backend logic, S3 upload, user verification (Vercel)
      +-- JDOX-AZURE-API       → Azure routes & logic (AWS Lambda)
      +-- JDOX-OCI-API         → OCI upload verification
      +-- Hight-AI / Content API → AI & content features (separate repo)
      +-- Authenticator-hight  → Auth service shared across ALL Hight.io projects
```

| API Repo | Purpose | Hosting |
| --- | --- | --- |
| `JDOX-API` | Main backend - S3 upload, user verification, core routes | Vercel (auto-deploy) |
| `JDOX-AZURE-API` | Azure-specific API routes and logic | AWS Lambda |
| `JDOX-OCI-API` | OCI upload verification logic | _(confirm hosting)_ |
| `Hight-AI / Content API` | AI and content features | Separate repo |
| `Authenticator-hight` | Authentication across all Hight.io projects | Separate repo |

---

## Environment Variables

- [ ] AWS credentials (DynamoDB, S3, Lambda)
- [ ] AWS Cognito pool details
- [ ] Vercel project tokens (if deploying)
- [ ] Org ID and config for Folder Scanner builds

---

## Team Contacts

| Name | Role |
|---|---|
| Jan Hox | Founder |
| Karthik Pandurangan | Product Owner (Swiftin) & POC |
| Glen McCallum | Tech Lead & POC |
| Kristian | Software Developer |

If Jan is unavailable, Karthik is the go-to for AWS and Vercel credentials.
