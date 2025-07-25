# General Information

This document contains all other relevant information that does not fit into technical or business categories.

## Contents
- Project Contacts
- Meeting Notes
- Change Log
- References
- Directory Structure
- Key Files
- Logs

---

### Project Contacts
- Project Owner: Mohan Chatla (mohanchatla@gmail.com)
- Maintainer: cvrrocket@gmail.com

### Meeting Notes
- 2025-06-23: Discussed automation of daily report uploads and notification improvements.
- 2025-06-29: Reviewed Google Drive integration and error handling.

### Change Log
- 2025-06-29: Improved logging and notification system.
- 2025-06-23: Added support for subdivision-wise reports.
- 2025-06-10: Initial deployment of web interface.

### References
- [Google Drive API Documentation](https://developers.google.com/drive)
- [Pandas Documentation](https://pandas.pydata.org/)

### Directory Structure
- `Analysis/`: LaTeX tables and analysis outputs
- `Net_Additions_Reports/`: Excel reports (daily, past, subdivision-wise)
- `Postal Accounts misc. docs/`: Source and reference Excel files
- `app_logs/`: Application logs
- `static/`, `templates/`: Web assets and HTML templates

### Key Files
- `advanced.py`, `app.py`, `drive_utils.py`, `sendNotification.py`, `requirements.txt`, `setup.py`

### Logs
- See `app_logs/` for detailed logs of each run
- Example log entry:
  - 2025-06-29 12:11:08: Successfully uploaded 'curr_date_open_acc.xlsx' to Google Drive
  - 2025-06-29 12:11:13: Success alert email sent!
