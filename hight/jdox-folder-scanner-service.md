[← Back to Index](../index.md)

# JDOX Folder Scanner Service (Windows)

Windows background service version of the Folder Scanner. Runs via `services.msc`, handles file uploads asynchronously across Windows logins and remote sessions.

---

## Tech Stack

![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Yarn](https://img.shields.io/badge/Yarn-2188B6?style=for-the-badge&logo=yarn&logoColor=white)

| Layer | Technology |
|---|---|
| Runtime | Node.js (compiled to Windows Service) |
| Installer | **InnoSetup Compiler** |
| Node Version | **v22** |
| Package Manager | **yarn** |

---

## Folder Scanner App vs. Service

| Feature | Folder Scanner App | Folder Scanner Service |
| --- | --- | --- |
| UI | Electron GUI | No UI - background Windows service |
| Runs when | App is open | Always (runs via `services.msc`) |
| Remote sessions | Limited | Works across logins and remote sessions |
| Upload mode | Interactive | Async background |

---

## Build & Distribution

```
1. Build Node.js service
2. Compile with InnoSetup → creates installer .exe
3. Distribute installer to customer
4. Service registers in Windows services (services.msc)
```

---

## Team Contacts

| Name | Role |
|---|---|
| Jan Hox | Founder |
| Karthik Pandurangan | Product Owner & POC |
| Glen McCallum | Tech Lead & POC |
