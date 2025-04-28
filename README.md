# SMHCC Uptime Kuma Networking Monitoring 🌐

![SMHCC Monitoring Dashboard](media/image6.png)

## Overview 🌟

The **SMHCC Uptime Kuma Networking Monitoring** tool integrates [Uptime Kuma](https://github.com/louislam/uptime-kuma) with Google Spreadsheets to provide real-time network device monitoring across SMHCC properties. This solution uses webhook notifications to log device status updates and displays them on a centralized Google Sheet dashboard 📊. Key features include automated data logging, historical analysis, and timestamp tracking via Google Apps Script, enabling proactive network management 🚀.

## Features ✨

- **Real-Time Monitoring** 🕒: Tracks network device status using Uptime Kuma and displays it on a Google Sheet dashboard.
- **Webhook Integration** 🔗: Logs are sent from Uptime Kuma to property-specific sheets via webhook POST URLs.
- **Automated Timestamps** ⏰: Google Apps Script updates timestamps for status changes, ensuring accurate tracking.
- **Historical Analysis** 📜: Maintains a history of status changes for each monitor in a dedicated "HISTORY" sheet.
- **Customizable Dashboard** 🎨: Easily configurable for new properties and monitors using predefined formulas.

## How It Works 🛠️

### Centralized Dashboard 📋
- Operates within a Google Sheet, with a main "DASHBOARD" sheet displaying the latest monitor status 🌈.
- Status cells pull data from property-specific log sheets, updated in real-time.

### Property Log Collection 📑
- Each SMHCC property has a dedicated log sheet that aggregates logs from a local Uptime Kuma instance.
- Logs are delivered via webhook notifications, with the **POST URL** serving as the critical link for data transfer 🔄.

### Status Tracking ✅
- The dashboard’s status cells reflect the most recent status from each property’s log sheet, ensuring up-to-date monitoring.

### Automated Timestamp Updates ⏳
- A Google Apps Script (`updateTimestamps`) tracks status changes and logs timestamps.
- The script updates both the "DASHBOARD" and "HISTORY" sheets for accurate record-keeping 📅.

### Apps Script Functionality 🤖
- **Purpose**: Automatically updates timestamps when status values change.
- **Mechanism**:
  - Operates on two sheets: "DASHBOARD" (main status display) and "HISTORY" (status change tracking).
  - Uses column pairs to link status columns (e.g., D, J, P) with timestamp columns (e.g., E, K, Q) and history columns.
  - Compares current and historical values, updating timestamps and history when changes occur.
  - Initializes headers in the "HISTORY" sheet for values, timestamps, and previous values if not present.
- **Periodic Execution**: The `checkStatusChanges` function runs `updateTimestamps` on a schedule to ensure continuous monitoring 🕰️.

## Setup Instructions 🧰

### Prerequisites ✅
- Access to a local Uptime Kuma instance.
- Google account with access to Google Sheets and Apps Script.
- Approval from SMHCC IT VP for tool access (restricted to SMHCC Property IT personnel) 🔒.

### Google Sheets Setup 📝
1. **Access the SNM Tool**:
   - Submit a request to [helpdesk.pidv@gmail.com](mailto:helpdesk.pidv@gmail.com) for access, subject to SMHCC IT VP approval 🥰.
2. **Add a Property Dashboard**:
   - Copy the dashboard from the "TEMPLATE" sheet in the Google Sheet.
   - Fill in the appropriate cells for the new property.
   - Update the **Status** column formula:
     ```excel
     =IF(ISNUMBER(SEARCH("down", INDEX(FILTER(<PROPERTY_CODE>!C:C, ISNUMBER(SEARCH("<MONITOR_NAME>", <PROPERTY_CODE>!C:C))), COUNTA(FILTER(<PROPERTY_CODE>!C:C, ISNUMBER(SEARCH("<MONITOR_NAME>", <PROPERTY_CODE>!C:C))))))), "🔴 Down", IF(ISNUMBER(SEARCH("up", INDEX(FILTER(<PROPERTY_CODE>!C:C, ISNUMBER(SEARCH("<MONITOR_NAME>", <PROPERTY_CODE>!C:C))), COUNTA(FILTER(<PROPERTY_CODE>!C:C, ISNUMBER(SEARCH("<MONITOR_NAME>", <PROPERTY_CODE>!C:C))))))), "✅ Up", ""))
     ```
   - Replace `<PROPERTY_CODE>` with the property’s sheet code (e.g., PIDV, PSH).
   - Replace `<MONITOR_NAME>` with the Uptime Kuma monitor’s friendly name 🌟.

### Uptime Kuma Setup 🌍
1. Request a **POST URL** for the property by emailing [helpdesk.pidv@gmail.com](mailto:helpdesk.pidv@gmail.com).
2. Access the local Uptime Kuma Web UI.
3. Navigate to **Settings** > **Notifications** > **Setup Notification**.
4. Fill in the required fields, including the provided **POST URL**.
5. Test the webhook setup and save 🥳.
6. Attach the notification to the desired monitor.
7. Simulate a status change:
   - Set a wrong IP address to trigger a "DOWN" notification 😕.
   - Revert to the correct IP to trigger an "UP" notification 😊.
   - Save changes to confirm notifications are logged in the property’s sheet.

### Apps Script Configuration 🖥️
- The `updateTimestamps` script is pre-configured in the Google Sheet.
- Ensure the script references the correct column pairs for status and timestamp columns.
- Set up a time-driven trigger for the `checkStatusChanges` function to run periodically (e.g., every minute) ⏲️.

## Backup and Restoration 💾

### Objective 🎯
Ensure critical Google Sheets are regularly backed up both onsite (e.g., local drives) and offsite (e.g., cloud storage outside Google Workspace) to protect against accidental deletion, corruption, or service outages 🛡️.

### Backup Strategy Overview 📋
| **Type** | **Method** | **Frequency** |
|----------|------------|---------------|
| **Onsite Backup** | Download Google Sheet as Excel (.xlsx) or CSV to local storage | Monthly 📅 |
| **Offsite Backup** | Sync or export Google Sheet to another cloud platform (OneDrive, Dropbox, AWS S3, etc.) | Monthly ☁️ |

### Onsite Backup Procedures 🖴
1. Open the Google Sheet.
2. Click **File** > **Download** > **Microsoft Excel (.xlsx)** or **CSV**.
3. Save the file to a local secure folder: `\\10.36.173.31\dvopd\shared drive\IT\Backups\SNMDashboard`.
4. Use the naming convention: `[MONTH]_[YEAR].xlsx` (e.g., `APRIL_2025.xlsx`).
5. (Optional) Copy the file to an external hard drive monthly for extra security 💽.

### Offsite Backup Procedures ☁️
1. After completing the onsite backup, upload the file to the **Carlson Rezidor OneDrive** cloud platform.
2. Maintain the same naming format (e.g., `APRIL_2025.xlsx`) for consistency 📦.

### Backup Retention Policy 📜
- Keep monthly backups for **12 months**.
- Delete files older than 1 year unless required for audit or legal purposes 🗑️.

### Restoration Procedures 🔄
If the Google Sheet is corrupted or lost:
1. Locate the most recent backup (onsite or offsite).
2. Open the Excel or CSV file.
3. Re-upload it to Google Drive.
4. (Optional) Reformat if necessary to restore the original structure 🌟.

### Additional Tip 🌈
- Enable **Version History** in Google Sheets for easy recovery:
  - Go to **File** > **Version History** > **See Version History** 🕰️.

## Troubleshooting 🐞

| **Issue** | **Resolution** |
|-----------|----------------|
| Dashboard status does not match Uptime Kuma 😕 | - Verify the property log sheet.<br>- Check the latest monitor status.<br>- Validate the formula (correct `<PROPERTY_CODE>` and `<MONITOR_NAME>`).<br>- Ensure the webhook notification is attached to the monitor. |
| Cannot add more monitors 🚫 | - Check for restricted cells and unlock them if necessary. |
| Duration column shows incorrect time ⏱️ | - Reload the sheet.<br>- Confirm the formula references cell C328 (real-time clock).<br>- Verify the hidden timestamp column between "Status" and "Duration" is updated by the script.<br>- Review the Apps Script for correct column mappings. |
| POST URL connection fails 😢 | - Request a new POST URL via [helpdesk.pidv@gmail.com](mailto:helpdesk.pidv@gmail.com). |
| Property sheet not visible 🕵️ | - Go to **View** > **Hidden Sheets** > **Show [Property Sheet]**. |

## Contributing 🤝

Contributions are welcome! To contribute:
1. Fork the repository 🍴.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request 🎉.

Please ensure your changes align with SMHCC’s IT policies and are tested thoroughly.

## License 📄

This project
