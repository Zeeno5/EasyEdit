#  EasyEdit System Architecture

###  Overview
EasyEdit combines a friendly frontend interface with a powerful backend and smart edit engine.  
The system ensures that any document, whether a spreadsheet, text file, or form can be processed, edited, and previewed seamlessly across devices.



##  System Diagram (Visual Overview)

Below is a simplified text-based diagram representing EasyEdit’s core structure:

         ┌──────────────────────────────┐
         │          USER SIDE           │
         │ ──────────────────────────── │
         │ Upload file or connect drive │
         │ Type or voice an edit prompt │
         └──────────────┬───────────────┘
                        │
                        ▼
         ┌──────────────────────────────┐
         │       FRONTEND (React)       │
         │ Handles UI, login & preview  │
         │ Sends file + prompt request  │
         └──────────────┬───────────────┘
                        │
                        ▼
         ┌──────────────────────────────┐
         │     BACKEND (Node.js API)    │
         │ Validates user input & auth  │
         │ Passes request to Edit Engine│
         └──────────────┬───────────────┘
                        │
                        ▼
         ┌──────────────────────────────┐
         │       EDIT ENGINE CORE       │
         │ NLP Interpreter parses prompt│
         │ Applies changes to document  │
         └──────────────┬───────────────┘
                        │
      ┌─────────────────┼──────────────────┐
      ▼                 ▼              ▼
      




This diagram visually explains how user input flows from the frontend to the backend.



##  Technical Breakdown

###  Frontend
- **Built with:** React.js + TailwindCSS  
- **Functions:**  
  - Upload interface for multiple file types.  
  - Displays live previews of document changes.  
  - Handles authentication and edit confirmations.  



###  Backend
- **Built with:** Node.js + Express.js  
- **Responsibilities:**  
  - Receives and validates file uploads.  
  - Forwards prompts to the Edit Engine.  
  - Manages authentication and access control.  
  - Returns processed file to frontend.



###  Edit Engine (Smart Processor)
- **Built with:** Python NLP microservice  
- **Functions:**  
  - Interprets user’s natural language prompts.  
  - Maps instructions (e.g., “resize row 3 to 50”) to structured edit actions.  
  - Uses third-party document APIs (Google Docs, Sheets API).  
  - Returns edited content for review.  



###  Database & Storage
- **Database:** PostgreSQL for users, logs, and session history.  
- **Storage:** AWS S3 / Google Drive for temporary document storage.  
- **Security:** End-to-end encryption, auto file delete after processing.  



###  Integrations
- **Google Docs & Sheets API:** For real-time document edits.  
- **OneDrive API:** For Microsoft users.  
- **Firebase Authentication:** For secure login across platforms.  


###   How Components Communicate

**User Interaction (Frontend → Backend)**
- When a user uploads a file or enters an edit command, the **frontend** sends a REST API request to the **backend** using JSON payloads via HTTPS.
- Files are sent using multipart/form-data for upload endpoints.

**Backend Processing (Backend → AI Service)**
- The backend validates requests, then sends the document content and prompt to theAI microservice (Python) through an internal API (secured with API keys).
- The AI service interprets the instruction, applies NLP-based edits, and returns structured edit data.

**Storage & Sync (Backend → Database & Cloud Storage)**
- The backend saves document versions and metadata to MongoDB.
- Actual files are stored in S3, while metadata (e.g., version, edit timestamp) is saved in the database.

**Response Delivery (Backend → Frontend)**
- Once edits are complete, the backend sends the processed file or diff result back to the frontend.
- The frontend updates the user interface in real time, showing edited previews and download options.

**Notification & Status Updates**
- WebSockets (or Firebase for mobile) enable real-time edit status updates and progress tracking.


###  Why This Approach Works
- The modular structure ensures scalability.  
- Each service can evolve independently.  
- Real-time updates make collaboration fast and reliable.  
- Integrations provide flexibility for different file formats.  



##  System Communication Flow

```plaintext
Frontend (React)
   |
   |---> Node.js Backend (Express API)
           |
           |---> AI Microservice (Flask)
           |        |
           |        --> NLP + File Processing
           |
           |---> MongoDB (Metadata)
           |
           |---> AWS S3 / GCP Storage (Documents)
           |
           ---> Returns JSON response to Frontend




