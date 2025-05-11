# 🔄 Oracle Integration Cloud – Data Exchange Between Two Systems

## 📌 Project Overview

This project demonstrates an **OIC-based integration** that automates the **transfer and processing of daily CSV files** between two Oracle SaaS applications:

- **System A**: Oracle CX Sales Cloud – generates daily transaction files.
- **System B**: Oracle RightNow – receives and processes the transformed data.

The goal is to implement a auditable integration that reads CSV files from a secure FTP location, processes them, and ensures accurate data delivery to System B.

---

## 🧩 Use Case

- System A exports daily CSV files to a secure FTP directory.
- Oracle Integration Cloud:
  - Reads the files
  - Transforms the data
  - Sends the data to Oracle RightNow (System B)
  - Archives the files or moves them to an error folder based on success/failure
  - Sends email notifications based on success/failure

## 🛠 Technologies Used

- **Oracle Integration Cloud (OIC)**
- **FTP Adapter** (trigger + file move)
- **Oracle RightNow Adapter** (target system)
- **Email Notification**
- **Scope + Fault Handler for error processing**

---

## 🔧 Implementation Steps

### 1. **Create OIC Connections**
- **FTP Adapter**: To read and move files from/to FTP server
- **RightNow Adapter**: To send transformed data to System B
- **Notification Action**: For success/failure notifications

### 2. **Set Up FTP Server (System A)**
Using **WinSCP or any SFTP tool**, create:
- `/submain` → incoming files from System A
- `/archive` → for successfully processed files
- `/error` → for failed files

### 3. **Create Scheduled Orchestration Integration**
- Type: **Scheduled**
- Purpose: Poll FTP directory regularly for new files

### 4. **Design Integration Flow**
- Add **Scope** to group file processing actions
  - Inside Scope:
    - **Read file** using FTP Adapter
    - **Transform data** using Stage File 
    - **Invoke RightNow** to push the data
- After Scope (Success Path):
  - **Move file to /archive/**
  - **Send success email**

### 5. **Configure Fault Handler**
- On error:
  - **Move file to /error/**
  - **Send failure notification** with details (file name, error)

### 6. **Test and Validate**
- Drop test files in `/submain`
- Run or schedule integration
- Monitor:
  - Target file folder
  - Notification email inbox
  - OIC tracking logs

---

## 📂 Repository Contents

| File/Folder              | Description                                      |
|--------------------------|--------------------------------------------------|
| `integration-artifacts/` | Sample files, mappings, and payloads             |
| `diagrams/`              | Integration flow diagrams                        |
| `docs/cookbook.md`       | Step-by-step build guide                         |
| `README.md`              | Project summary and usage                        |

---

## 📬 Email Notification Samples

- ✅ **Success Email**: `"File abc.csv processed successfully. Moved to archive."`
- ❌ **Failure Email**: `"File abc.csv failed to process. Moved to error. Error: [error message]"`

---

## 🧪 Sample Output Behavior

| File Name   | Status     | Moved To   |
|-------------|------------|------------|
| abc.csv     | Success    | /archive/  |
| xyz.csv     | Failed     | /error/    |

---

## ✅ Benefits

- Fully automated data transfer between cloud systems
- Real-time monitoring and alerting
- Easy auditing via archive/error directories
- Clean error handling and retry support

---

 


