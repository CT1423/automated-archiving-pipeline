# automated-archiving-pipeline
A web archiving project using Docker, Python (Archivebox), Rclone, and Bacblaze B2 Object Storage. 

# 📂 Project: Self-Hosted Digital Archive Pipeline

### 📖 Overview
A robust, automated web archiving solution designed to preserve digital history, news articles, and research materials. This project implements a **3-2-1 backup strategy** by hosting a local archive engine and automating daily encrypted backups to enterprise-grade cloud object storage.

### 🏗️ Architecture
```mermaid
graph LR
    A[Web Source] -->|Headless Chrome| B(ArchiveBox Container)
    B -->|Local Storage| C{Local Hard Drive}
    C -->|Rclone Sync| D[Backblaze B2 Cloud]
    E[Windows Task Scheduler] -->|Triggers| C

Data Flow:

Ingest: ArchiveBox (running in Docker) captures URLs via CLI or Web UI.

Process: Content is extracted in multiple formats (PDF, Screenshot, HTML, Text).

Store: Data is indexed in SQLite and stored on the local file system.

Sync: A PowerShell script utilizes Rclone to diff local data against the Cloud Remote.

Backup: New/Changed files are encrypted and pushed to Backblaze B2 Object Storage.

🛠️ Tech Stack
Containerization: Docker & Docker Compose

Application: ArchiveBox (Django/Python based)

Cloud Storage: Backblaze B2 (Object Storage)

Infrastructure as Code: YAML (Docker Compose configuration)

Automation: Windows Task Scheduler & Batch Scripting

Data Sync: Rclone (Command Line Cloud Storage Tool)

OS: Windows 10/11 (PowerShell environment)

✨ Key Features
Full-Fidelity Capture: Saves dynamic content using a headless Chrome instance to bypass paywalls and JavaScript-heavy layouts.

Automated Redundancy: "Set and forget" nightly backups ensure data persistence even in the event of local hardware failure.

Bandwidth Efficiency: Uses Rclone's delta sync to upload only new or modified files, minimizing bandwidth usage.

Searchability: Local full-text search capability via SQLite backend.

⚙️ Implementation Details
1. Docker Configuration
The system runs in an isolated environment using docker-compose.yml.

Constraint Solved: Configured SEARCH_BACKEND_ENGINE=db to resolve stability issues with the Sonic search backend on Windows architecture.

Storage: Persistent volumes mapped to local directory for easy access and backup.

2. Cloud Sync Strategy (Rclone)
Backblaze B2 was selected for high durability (99.999999999%) and low cost. Rclone is configured with a dedicated B2 Application Key restricted to a single bucket for security.

The Automation Script (AutoBackup.bat):

@echo off
cd /d "C:\Users\defaultuser0\archivebox"
:: Delta sync to Cloud Bucket with logging
.\rclone.exe copy "data/archive" b2:my-b2-bucket-name --transfers 4 --log-file="backup_log.txt" --log-level=INFO

🐛 Challenges & Solutions
Filename Length Limits: Windows MAX_PATH (260 chars) caused crashes with deep URL structures (e.g., Google News redirects).

Solution: Implemented manual pruning of corrupted snapshots and switched to archiving canonical URLs to maintain file system compatibility.

Database Locks: Improper container shutdowns occasionally locked the SQLite index.

Solution: Established a maintenance protocol using docker compose down and init repair commands to preserve data integrity.

🚀 Future Improvements
Implement a simplified frontend using Nginx for easier browsing on mobile devices.

Set up a Telegram or Discord bot to feed URLs into the archive remotely.

Migrate from Docker Desktop to a dedicated Linux VPS (AWS EC2) for 24/7 uptime.














