# Developer Guide 2025-2026

> Onboarding and reference guide for developers joining any active projects.
> Last updated: 20 May 2026

---

## Organizations

### Document IT - Accqrate HCM

| Project | Stack | Node | Pkg Manager | Link |
|---|---|---|---|---|
| [Accqrate HCM (Web)](accqrate/accqrate-hcm.md) | React · TypeScript · Express · MongoDB | Node 20 | npm | [View →](accqrate/accqrate-hcm.md) |

---

### Hight.io

> Global standard across all Hight.io projects: **Node v22** · **yarn** (never npm)

| Project | Stack | Link |
|---|---|---|
| [JDOX Studio (Web)](hight/jdox-studio.md) | Vue · Nuxt 3 · DynamoDB · Vercel | [View →](hight/jdox-studio.md) |
| [JDOX Folder Scanner App](hight/jdox-folder-scanner-app.md) | Electron · Node · GitHub Releases | [View →](hight/jdox-folder-scanner-app.md) |
| [JDOX Folder Scanner Service](hight/jdox-folder-scanner-service.md) | Node · Windows Service · InnoSetup | [View →](hight/jdox-folder-scanner-service.md) |
| [Reducr.io](hight/reducr.md) | React · Next.js · Node | [View →](hight/reducr.md) |
| [Oratix.io](hight/oratix.md) | React · Next.js · AWS Bedrock | [View →](hight/oratix.md) |
| [OBOT](hight/obot.md) | React · Next.js · AWS Bedrock | [View →](hight/obot.md) |
| [Hight Components Library](hight/hight-components.md) | Internal UI Library | [View →](hight/hight-components.md) |

---

## Quick Reference

| Project | Org | Frontend | Backend | DB | Node | Pkg Mgr | Deployment |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Accqrate HCM | Document IT | React + TS + Ant Design | Express | MongoDB | 20 | npm | GitHub branches |
| JDOX Studio | Hight.io | Vue.js, Nuxt3, Vuetify | Multi-API | DynamoDB | 22 | yarn | Vercel (auto) |
| Reducr.io | Hight.io | React, Next.js | Node API | TBD | 22 | yarn | TBD |
| Oratix.io | Hight.io | React, Next.js | Node API + AWS | TBD | 22 | yarn | AWS |
| OBOT | Hight.io | React, Next.js | Node API + AWS | TBD | 22 | yarn | AWS |
| Folder Scanner App | Hight.io | Electron | Node (JDOX API) | — | 22 | yarn | GitHub Releases (.exe) |
| Folder Scanner Service | Hight.io | None (background) | Node Windows Service | — | 22 | yarn | InnoSetup installer |
| Hight Components | Hight.io | Component Library | — | — | 22 | yarn | npm package |
