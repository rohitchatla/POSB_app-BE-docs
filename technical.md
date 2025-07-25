# Technical Documentation

This document covers the technical aspects of the POSB Account project, including architecture, APIs, modules, and integration details.

## Contents
- System Architecture
- API Endpoints
- Module Descriptions
- Data Flow Diagrams
- Deployment Instructions
- Requirements
- Main Scripts

---

### System Architecture
The POSB Account Application is a Python-based system for downloading, processing, and reporting on POSB account data. It uses Flask for the web interface, pandas for data processing, and integrates with Google Drive for file storage and retrieval. The system is modular, with separate scripts for business logic, Drive utilities, notifications, and automation.

### API Endpoints
- `/` : Home page (Flask)
- `/upload` : Upload Excel files
- `/download` : Download processed reports
- `/process` : Trigger processing logic

### Module Descriptions
- `advanced.py` / `advanced_drive.py`: Main logic for downloading, processing, and generating reports (Excel, LaTeX).
- `app.py`: Flask web application, handles HTTP requests, file uploads, and triggers processing.
- `drive_utils.py` / `google_drive_api.py`: Google Drive integration for file upload/download.
- `sendNotification.py`: Email notifications for process completion and alerts.
- `automation.py`: Automation scripts for scheduled tasks.

### Data Flow Diagrams
- Data is downloaded from external sources (MIS, CBS, etc.)
- Processed using pandas and custom logic
- Reports are generated and uploaded to Google Drive
- Notifications are sent to stakeholders

### Deployment Instructions
1. Install dependencies: `pip install -r requirements.txt`
2. Run the application: `python advanced.py` or `python app.py`
3. To build an executable: `python setup.py build`

### Requirements
- Python 3.11+
- pandas, requests, openpyxl, xlsxwriter, flask, pydrive2, google-auth-oauthlib, dotenv

### Main Scripts
- `advanced.py`, `advanced_drive.py`, `app.py`, `drive_utils.py`, `google_drive_api.py`, `sendNotification.py`, `automation.py`
