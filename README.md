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
