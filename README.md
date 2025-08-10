# WhatsApp-Driven-Google-Drive-Assistant-n8n-workflow-
### WhatsApp-Driven Google Drive Assistant (n8n Workflow)

This project provides an n8n workflow that enables users to interact with their Google Drive via simple text commands sent through WhatsApp. The workflow is designed to handle common file management tasks such as listing files, deleting files, moving files, and generating summaries of document content using a large language model.

#### **Features**

  * **Inbound Channel**: Utilizes the Twilio Sandbox for WhatsApp to receive commands.
  * **Google Drive Integration**: Connects to your Google Drive via OAuth2 to perform file and folder operations.
  * **File Management**: Supports commands to `LIST` files in a folder, `DELETE` specific files, and `MOVE` files between folders.
  * **AI Summarization**: Generates concise, bulleted summaries of documents (PDF, DOCX, TXT) within a specified folder using a large language model (Google Gemini).
  * **Error Handling**: Provides user feedback for invalid commands or non-existent files/folders.

#### **Command Syntax**

The workflow parses simple text commands sent via WhatsApp. All commands are case-sensitive and must be the first word of the message.

  * `LIST /<folder_name>`: Lists all files and subfolders within the specified folder.
  * `DELETE /<folder_name>/<file_name>`: Deletes a specific file. **Note:** This workflow does not include a confirmation keyword for mass deletion.
  * `MOVE /<source_folder>/<file_name> /<destination_folder>`: Moves a file from the source folder to the destination folder.
  * `SUMMARY /<folder_name>`: Generates and returns a bulleted summary for each supported document type (PDF, DOCX, TXT) in the specified folder.

#### **Setup Guide**

1.  **Clone the Repository**:

    ```bash
    git clone [repository_url]
    cd [repository_name]
    ```

2.  **Run n8n via Docker**:
    The workflow is designed to be run on n8n. Use the provided Docker setup instructions to get n8n up and running. An `env-sample` file is included in the repository to help you configure the necessary environment variables.

3.  **Import the Workflow**:

      * Open your n8n instance and navigate to "Workflows".
      * Click "New" and then "Import from file".
      * Select the `My workflow.json` file from this repository.

4.  **Configure Credentials**:

      * **Twilio**: In the n8n workflow editor, click on the "Twilio Trigger" node. Configure a new Twilio API credential using your Account SID and Auth Token.
      * **Google Drive**: The workflow requires an OAuth2 connection. Configure a new Google Drive OAuth2 API credential. Ensure the scopes are set to allow file management and reading of content.
      * **Google Docs**: The workflow needs a Google Docs OAuth2 credential to handle `.docx` files.
      * **Google Gemini**: Configure a new Google Gemini (PaLM) API credential with your API key for the summarization feature.

5.  **Set Up Twilio Sandbox**:

      * Go to your Twilio console and navigate to the WhatsApp Sandbox settings.
      * Copy the webhook URL from the "Twilio Trigger" node in your n8n workflow.
      * Paste this URL into the "When a message comes in" field in your Twilio Sandbox configuration and save.

#### **Technical Details**

  * **Workflow Structure**: The workflow starts with a **Twilio Trigger** node that listens for incoming WhatsApp messages. A **Switch** node then routes the message based on the command to the appropriate action.
  * **File Handling**: The workflow uses **Google Drive** nodes to search, list, move, and delete files. It also uses a combination of **Google Docs** and **HTTP Request** nodes to handle the conversion and extraction of `.docx` files for summarization.
  * **AI Integration**: The `SUMMARY` command branches into a process that downloads file content, extracts text, and then uses a **Concise Summary Generator** node with a **Google Gemini Chat Model** to create summaries.
  * **Output**: Responses are sent back to the user's WhatsApp number using a **Twilio** node.
