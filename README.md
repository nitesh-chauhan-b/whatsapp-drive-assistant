# WhatsApp-Powered Google Drive Assistant (n8n)

Welcome to the WhatsApp-Driven Google Drive Assistant! This project enables you to manage your Google Drive files directly from WhatsApp using simple chat commands, all orchestrated through an n8n workflow. You can list, move, delete, and summarize documents with the help of AI, making file management seamless and interactive.

This solution was developed as part of an internship assignment and demonstrates practical automation using modern cloud and messaging tools.

## 🌟 Key Features

- **File Listing**: Instantly retrieve details about files or folders in your Google Drive.
- **File Movement**: Effortlessly move files between folders.
- **Secure Deletion**: Files are deleted only after a two-step confirmation to prevent accidental loss.
- **AI Summaries**: Get concise, bullet-point summaries of your documents (PDF, TXT, DOCX, etc.) using Google Gemini AI.
- **Activity Logging**: Every command is logged in Google Sheets with timestamps for easy auditing.



## 🧰 Technology Stack

- **Automation**: n8n (self-hosted via Docker)
- **Messaging**: Twilio WhatsApp Sandbox
- **Cloud Storage**: Google Drive API
- **Logging**: Google Sheets API
- **AI Summarization**: Google Gemini API
- **Public Webhook**: ngrok



## 🚦 Getting Started

Follow these steps to set up and run the assistant locally.

### Prerequisites

- **Docker Desktop**: Install and start Docker.
- **n8n**: The workflow runs on a local n8n instance.
- **Twilio Account**: Free account is sufficient.
- **Google Cloud Account**: Needed for API credentials.
- **Google AI Studio Account**: For Gemini API Key.

### 1. Clone the Repository

```bash
git clone https://github.com/nitesh-chauhan-b/whatsapp-drive-assistant.git
cd whatsapp-drive-assistant
```

### 2. Launch n8n with Docker

```bash
docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```

Visit `http://localhost:5678` in your browser and complete the n8n setup.

### 3. Set Up Google Cloud Credentials

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project.
3. Enable the **Google Drive API** and **Google Sheets API** (search for them in "APIs & Services > Library").
4. If you encounter authentication issues, ensure you:
   - Add the required scopes (e.g., `https://www.googleapis.com/auth/drive`) in the OAuth consent screen.
   - Add your email as a **Test User** in the OAuth consent screen if you get access errors.
5. Configure the **OAuth consent screen** (set to External, fill in app details).
6. Create OAuth credentials:
   - Go to **APIs & Services > Credentials**.
   - Click **+ Create Credentials > OAuth client ID**.
   - Set Application type to **Web application**.
   - For **Authorized redirect URI**, use the URL provided by n8n when setting up Google credentials.

### 4. Import Workflow & Add Credentials

1. In n8n, go to **Workflows > Import from File** and select `workflow.json`.
2. Add credentials for:
   - **Twilio**: Use your Account SID and Auth Token from [Twilio Console](https://console.twilio.com/).
   - **Google Drive API**: Use Client ID and Secret from Google Cloud. Paste the n8n Redirect URI into Google Cloud settings. Add the scope `https://www.googleapis.com/auth/drive`.
   - **Google Gemini**: Get your API key from [Google AI Studio](https://aistudio.google.com/).

### 5. Set Up the Webhook

1. Start ngrok to expose your n8n instance:
   ```bash
   ngrok http 5678
   ```
2. Copy the public forwarding URL from ngrok.
3. In n8n, copy the Webhook node's Test URL path.
4. Combine the ngrok URL and webhook path for your public webhook URL.
5. In Twilio Console, go to **Messaging > Sandbox settings** and paste the webhook URL into "When a message comes in".


## 💬 WhatsApp Commands

| Command   | Arguments                        | Example                        | Description                                      |
|-----------|----------------------------------|--------------------------------|--------------------------------------------------|
| LIST      | [FileName]                       | LIST MyReport.pdf              | Find and confirm a file's existence.             |
| DELETE    | [FileName]                       | DELETE OldReport.pdf           | Start the two-step deletion process.             |
| CONFIRM   | DELETE [FileID]                  | CONFIRM DELETE 1a2b...         | Confirm deletion using the File ID.              |
| MOVE      | [SourceFile] [DestinationFolder] | MOVE MyReport.pdf Archive      | Move a file to a folder.                         |
| SUMMARY   | [FileName]                       | SUMMARY MyReport.pdf           | Get an AI summary of the file.                   |

**Note:** The MOVE command does not support file/folder names with spaces.

---

## 📝 Troubleshooting

- If Google Drive authentication fails, double-check that the required scopes are added in the OAuth consent screen and your email is listed as a test user.
- Make sure the Google Drive API is enabled in your Google Cloud project.
- For any issues linking Google Drive in n8n, verify the Redirect URI and scopes.

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.
