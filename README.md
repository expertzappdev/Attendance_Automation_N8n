# :clock3: Attendance Automation Workflow (n8n)
This n8n workflow automates the attendance summary process by:
1. Listening for file uploads in a Slack channel.
2. Downloading and reading the Excel attendance sheet.
3. Processing attendance data to calculate present, half, absent days, and paid leaves.
4. Writing the results to a Google Sheet (monthly worksheet).
5. Sending a summary message back to Slack.
---
## :rocket: Features
- :inbox_tray: Triggers on Slack file mentions.
- :bar_chart: Calculates:
  - Present Days
  - Half Days
  - Absent Days
  - Paid Leave
  - Effective Present Days
  - Attendance Percentage
- :brain: Intelligent leave deduction logic.
- :outbox_tray: Sends summary in Slack.
- :card_index_dividers: Auto-appends or updates Google Sheets based on the month.
---
## :hammer_and_wrench: Setup Instructions
### Prerequisites
- [n8n](https://n8n.io/) instance running (self-hosted or cloud).
- Slack Bot with:
  - `channels:read`
  - `files:read`
  - `chat:write`
  - `app_mentions:read`
- Google Sheets integration connected to n8n.
- A formatted Google Sheet with monthly sheets (`April`, `May`, etc.).
### Environment Setup
1. **Slack Setup**
   - Invite your bot to the target Slack channel.
   - Make sure the bot has permission to read uploaded files and post messages.
2. **Google Sheets**
   - Create a Google Sheet with separate sheets for each month.
   - Format header columns as:
     ```
     Employee / Date | presentDays | halfDays | absentDays | paidLeave | totalWorkingDays | effectivePresentDays | presentPercentage
     ```
3. **n8n Workflow Import**
   - Import the included `attendance_automation.json` file into your n8n instance.
   - Replace:
     - `Slack Token` in HTTP Request headers.
     - `Channel ID` for your workspace.
     - `Google Sheets documentId` and `sheetName` as per your structure.
4. **Monthly Sheet Detection**
   - Filename should include pattern like `From_DD-MM-YYYY.xlsx`.
   - Workflow auto-detects month and posts data into the corresponding sheet.
---
## :arrows_counterclockwise: Workflow Flow
```text
Slack Trigger (app_mention) ➝ HTTP Request (download file) ➝ Read Excel
  ➝ Parse Data (Code node) ➝ Append to Google Sheet ➝ Format Summary
  ➝ Post Summary in Slack
