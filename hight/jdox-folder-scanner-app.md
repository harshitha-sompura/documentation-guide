[← Back to Index](../index.md)

# JDOX Folder Scanner App

Electron desktop app for scanning folders and uploading files. Uses JDOX API endpoints with additional Folder Scanner-specific routes.

---

## Tech Stack

![Electron](https://img.shields.io/badge/Electron-191970?style=for-the-badge&logo=Electron&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![Yarn](https://img.shields.io/badge/Yarn-2188B6?style=for-the-badge&logo=yarn&logoColor=white)

| Layer | Technology |
|---|---|
| Framework | Electron |
| Runtime | Node.js |
| Node Version | **v22** |
| Package Manager | **yarn** |
| APIs | Same as JDOX-API + Folder Scanner-specific routes |

---

## Build Configurations

The app compiles into `.exe` installer files with two company configurations:

| Build | Used For |
|---|---|
| **Hight version** | Internal testing |
| **Spirax version** | Customer deployment |

Always test with the Hight version. Spirax is what gets delivered to the customer.

---

## Adding a New Customer Config

When a new customer needs their own build:

1. Create a separate `.exe` and Node service
2. Configure based on their `org_id` and other org-specific config values
3. Test with internal (Hight) build first

---

## Distribution via GitHub Releases

```
1. Build the .exe
2. Create a GitHub Release on the customer's repo
3. Upload the .exe AND the .yml file to the release
4. Customers receive auto-updates from this GitHub release
```

The `.yml` file is required for auto-updates to work - do not skip it.

---

## Team Contacts

| Name | Role |
|---|---|
| Jan Hox | Founder |
| Karthik Pandurangan | Product Owner & POC |
| Glen McCallum | Tech Lead & POC |
