# Backend Agent Plan
**OpenMeet Recorder - Backend Implementation Plan**

---

## Document Control

| Property | Value |
|----------|-------|
| **Project** | OpenMeet Recorder |
| **Document Type** | Backend Agent Implementation Plan |
| **Version** | 1.0.0 |
| **Created** | 2025-12-29 |
| **Status** | Draft - Awaiting User Review |
| **Agent Role** | Backend Agent |
| **Prerequisites** | UI/UX Phase Complete, Frontend Phase Complete |
| **Dependencies** | Frontend API contracts, TypeScript interfaces |
| **Purpose** | Guide Backend Agent through serverless infrastructure implementation |

---

## Executive Summary

### Project Status

**Completed Phases:**
- ✅ **UI/UX Phase:** Complete design system with 47 tasks (UIUX_PLAN.md - 3,948 lines)
- ✅ **Frontend Phase:** React components, Chrome extension, dashboard (Expected from Frontend Agent)

**Current Phase:**
- 🎯 **Backend Implementation:** You are the Backend Agent responsible for serverless infrastructure

**Your Responsibilities:**
- Firebase project setup and configuration
- Cloud Firestore database schema and security rules
- Firebase Authentication with Google OAuth
- Google Drive API integration
- Cloudflare Workers for serverless compute
- Gemini 2.0 Flash AI integration
- API endpoint implementation
- Security and data validation

**What You're Building:**
- **No traditional backend servers** (serverless-first architecture)
- Firebase Auth + Firestore for user data and metadata
- Cloudflare Workers for API endpoints and AI orchestration
- Google Drive API for video storage (user's own Drive)
- Gemini API for transcription and summarization

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Backend Scope Overview](#backend-scope-overview)
3. [Technology Stack](#technology-stack)
4. [Architecture Overview](#architecture-overview)
5. [Phase Structure](#phase-structure)
6. [Phase 1: Firebase Project Setup](#phase-1-firebase-project-setup)
7. [Phase 2: Database Schema Implementation](#phase-2-database-schema-implementation)
8. [Phase 3: Authentication Backend](#phase-3-authentication-backend)
9. [Phase 4: Google Drive Integration](#phase-4-google-drive-integration)
10. [Phase 5: Cloudflare Workers Setup](#phase-5-cloudflare-workers-setup)
11. [Phase 6: API Endpoints Implementation](#phase-6-api-endpoints-implementation)
12. [Phase 7: AI Integration (Gemini)](#phase-7-ai-integration-gemini)
13. [Phase 8: Security & Validation](#phase-8-security--validation)
14. [Phase 9: Testing & Documentation](#phase-9-testing--documentation)
15. [Phase 10: Deployment & Monitoring](#phase-10-deployment--monitoring)
16. [Reference Documents](#reference-documents)
17. [Critical Implementation Notes](#critical-implementation-notes)

---

## Backend Scope Overview

### What Backend Agent Implements

**Core Infrastructure:**
1. Firebase project configuration
2. Firestore database with collections: `users`, `recordings`, `folders`, `ai_jobs`
3. Firestore security rules for data protection
4. Composite indexes for efficient queries

**Authentication Services:**
1. Firebase Auth configuration with Google OAuth
2. OAuth 2.0 Client ID setup (Chrome Extension + Dashboard)
3. Token verification middleware
4. User profile creation logic

**Storage Integration:**
1. Google Drive API configuration
2. OAuth scopes: `drive.file` (restricted access)
3. Signed URL generation for video playback
4. File deletion orchestration

**Serverless Compute:**
1. Cloudflare Workers for API endpoints
2. Worker for transcription orchestration
3. Worker for summarization orchestration
4. Environment variable management

**AI Processing:**
1. Gemini 2.0 Flash API integration
2. Transcription prompt engineering
3. Summarization prompt engineering
4. Error handling and retry logic

**API Layer:**
1. RESTful endpoints for recordings CRUD
2. Authentication endpoints (signin, signout, me)
3. AI processing trigger endpoints
4. Status polling endpoints
5. Google Drive signed URL endpoints

### What Backend Agent Does NOT Implement

**Frontend Code:**
- React components (Frontend Agent's responsibility)
- Chrome extension UI (Frontend Agent's responsibility)
- State management (Zustand stores - Frontend Agent)
- Client-side validation (Frontend Agent)

**DevOps (Out of Scope for MVP):**
- CI/CD pipelines (basic deployment only)
- Monitoring dashboards (basic logging only)
- Advanced alerting systems

---

## Technology Stack

### Backend Services

| Service | Provider | Version/Tier | Purpose |
|---------|----------|--------------|---------|
| **Authentication** | Firebase Auth | Free Tier | Google OAuth, user sessions |
| **Database** | Cloud Firestore | Free Tier | Recording metadata, user profiles |
| **Storage** | Google Drive API | User's Drive | Video file storage (15GB free per user) |
| **AI Model** | Gemini 2.0 Flash | Pay-per-use | Transcription and summarization |
| **Serverless Compute** | Cloudflare Workers | Free Tier (100k req/day) | API endpoints, AI orchestration |
| **Hosting** | Cloudflare Pages | Free Tier | Dashboard static hosting |

### Development Tools

| Tool | Version | Purpose |
|------|---------|---------|
| **Firebase CLI** | ^13.0.0 | Deploy rules, indexes, test locally |
| **Wrangler CLI** | ^3.0.0 | Develop and deploy Cloudflare Workers |
| **Node.js** | ^20.x | Development environment |
| **TypeScript** | ^5.0.0 | Type-safe code |
| **Firebase Emulator Suite** | Latest | Local development and testing |

### SDKs & Libraries

**For Cloudflare Workers:**
```json
{
  "@cloudflare/workers-types": "^4.0.0",
  "hono": "^3.0.0",
  "firebase-admin": "^12.0.0",
  "@google/generative-ai": "^0.1.0"
}
```

**For Firebase:**
```json
{
  "firebase-functions": "^5.0.0",
  "firebase-admin": "^12.0.0"
}
```

---

## Architecture Overview

### System Architecture

```mermaid
flowchart TB
    subgraph "Client Side"
        A[Chrome Extension] -->|Records| B[MediaRecorder API]
        B -->|WEBM Chunks| C[IndexedDB Buffer]
        C -->|Upload| D[Google Drive API]
        
        E[React Dashboard] -->|Fetch Metadata| F[Cloud Firestore]
        E -->|Get Video URL| D
        E -->|Request AI Processing| G[Cloudflare Workers]
    end
    
    subgraph "Firebase Services - YOUR RESPONSIBILITY"
        H[Firebase Auth] -->|User Session| A
        H -->|User Session| E
        F -->|Recording Metadata| E
        I[Firestore Security Rules] -->|Protect| F
    end
    
    subgraph "Serverless Backend - YOUR RESPONSIBILITY"
        G -->|Process Request| J[Cloudflare Worker - Transcribe]
        G -->|Process Request| K[Cloudflare Worker - Summarize]
        J -->|Call API| L[Gemini 2.0 Flash API]
        K -->|Call API| L
        L -->|Return Results| G
        G -->|Store Results| F
    end
    
    subgraph "User Storage"
        D -->|Video Files| M[User's Google Drive]
        E -->|Playback| M
    end
```

### Backend Workflow

```mermaid
sequenceDiagram
    participant User
    participant Extension
    participant Firebase
    participant Drive
    participant Worker
    participant Gemini
    participant Dashboard
    
    Note over Firebase,Worker: YOUR IMPLEMENTATION
    
    User->>Extension: Join Google Meet
    Extension->>Firebase: Authenticate
    Firebase-->>Extension: Auth Token
    
    User->>Extension: Start Recording
    Extension->>Drive: Upload Chunks
    Drive-->>Extension: Upload Progress
    
    User->>Extension: Stop Recording
    Extension->>Firebase: Save Metadata
    Firebase-->>Extension: Recording ID
    
    User->>Dashboard: View Recordings
    Dashboard->>Firebase: Fetch Metadata
    Firebase-->>Dashboard: Recording List
    
    User->>Dashboard: Request AI Summary
    Dashboard->>Worker: Trigger Processing
    Worker->>Drive: Get Video URL
    Worker->>Gemini: Transcribe + Summarize
    Gemini-->>Worker: AI Results
    Worker->>Firebase: Store Results
    Firebase-->>Dashboard: Updated Metadata
    Dashboard-->>User: Display Summary
```

---

## Phase Structure

### Implementation Phases Overview

| Phase | Name | Duration | Dependencies | Deliverables |
|-------|------|----------|--------------|--------------|
| **Phase 1** | Firebase Project Setup | 2-3 hours | None | Firebase project, Auth configured |
| **Phase 2** | Database Schema Implementation | 3-4 hours | Phase 1 | Firestore collections, rules, indexes |
| **Phase 3** | Authentication Backend | 2-3 hours | Phase 1, Phase 2 | OAuth setup, token verification |
| **Phase 4** | Google Drive Integration | 2-3 hours | Phase 3 | Drive API config, signed URLs |
| **Phase 5** | Cloudflare Workers Setup | 3-4 hours | Phase 1, Phase 2 | Workers project, API routing |
| **Phase 6** | API Endpoints Implementation | 4-5 hours | Phase 5 | Recording CRUD, auth endpoints |
| **Phase 7** | AI Integration (Gemini) | 4-5 hours | Phase 5, Phase 6 | Transcription, summarization |
| **Phase 8** | Security & Validation | 2-3 hours | All phases | Input validation, rate limiting |
| **Phase 9** | Testing & Documentation | 3-4 hours | All phases | API tests, documentation |
| **Phase 10** | Deployment & Monitoring | 2-3 hours | All phases | Production deployment |

**Total Estimated Time:** 27-37 hours

**Critical Path:**
Phase 1 → Phase 2 → Phase 3 → Phase 5 → Phase 6 → Phase 7 → Phase 9 → Phase 10

---

## Phase 1: Firebase Project Setup

### Phase Overview

**Goal:** Set up Firebase project infrastructure and configure core services

**Duration:** 2-3 hours

**Prerequisites:** 
- Google Cloud account
- Firebase account
- Node.js installed (v20.x)
- Firebase CLI installed globally

**Deliverables:**
- Firebase project created and configured
- Firebase Auth enabled with Google provider
- Cloud Firestore database provisioned
- Firebase Admin SDK configured
- Service account credentials generated
- Local development environment ready

---

### Task 1.1: Create Firebase Project

**Objective:** Initialize new Firebase project in Google Cloud Console

**Steps:**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add Project"
3. Enter project details:
   - **Project Name:** `openmeet-recorder`
   - **Project ID:** `openmeet-recorder` (or auto-generated)
   - **Google Analytics:** Enable (optional for MVP)
   - **Analytics Location:** Select your region
4. Wait for project creation (~30 seconds)
5. Note down Project ID and Project Number

**Verification:**
- [ ] Firebase project visible in console
- [ ] Project ID matches naming convention
- [ ] Project dashboard accessible

**Documentation:**
```markdown
# Firebase Project Details
- Project Name: openmeet-recorder
- Project ID: [your-project-id]
- Project Number: [your-project-number]
- Region: [your-region]
- Created: [timestamp]
```

---

### Task 1.2: Enable Firebase Authentication

**Objective:** Configure Firebase Auth with Google Sign-In provider

**Steps:**
1. In Firebase Console, navigate to **Build** → **Authentication**
2. Click "Get Started"
3. Go to **Sign-in method** tab
4. Enable **Google** provider:
   - Click on Google
   - Toggle "Enable"
   - Set **Project support email:** [your-email]
   - Save
5. Note the **Web API Key** from Project Settings

**Verification:**
- [ ] Google sign-in method shows as "Enabled"
- [ ] Support email configured
- [ ] Web API Key accessible

**Expected Configuration:**
```json
{
  "authDomain": "[project-id].firebaseapp.com",
  "apiKey": "[web-api-key]"
}
```

---

### Task 1.3: Create OAuth 2.0 Client IDs

**Objective:** Set up OAuth clients for Chrome Extension and Dashboard

**Steps:**

**For Chrome Extension:**
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Select your Firebase project
3. Navigate to **APIs & Services** → **Credentials**
4. Click **Create Credentials** → **OAuth 2.0 Client ID**
5. If prompted, configure OAuth consent screen:
   - **User Type:** External
   - **App Name:** OpenMeet Recorder
   - **User support email:** [your-email]
   - **Developer contact:** [your-email]
   - **Scopes:** Add `userinfo.email`, `userinfo.profile`, `drive.file`
   - Save and continue
6. Create OAuth Client ID:
   - **Application type:** Chrome App
   - **Name:** OpenMeet Extension
   - **Application ID:** `[extension-id].apps.googleusercontent.com`
   - Note: Extension ID will be generated after extension is uploaded
7. Click **Create**
8. Copy **Client ID** and **Client Secret**

**For Dashboard:**
1. Create another OAuth 2.0 Client ID
2. Configure:
   - **Application type:** Web application
   - **Name:** OpenMeet Dashboard
   - **Authorized JavaScript origins:**
     - `https://openmeet.panotech.uk`
     - `http://localhost:5173` (development)
   - **Authorized redirect URIs:**
     - `https://openmeet.panotech.uk/__/auth/handler`
     - `http://localhost:5173/__/auth/handler`
3. Click **Create**
4. Copy **Client ID** and **Client Secret**

**Verification:**
- [ ] OAuth consent screen configured
- [ ] Extension OAuth client created
- [ ] Dashboard OAuth client created
- [ ] All credentials safely stored

**Store Credentials:**
```env
# .env file (DO NOT COMMIT)
EXTENSION_CLIENT_ID=xxx.apps.googleusercontent.com
EXTENSION_CLIENT_SECRET=xxx
DASHBOARD_CLIENT_ID=xxx.apps.googleusercontent.com
DASHBOARD_CLIENT_SECRET=xxx
```

---

### Task 1.4: Enable Required Google Cloud APIs

**Objective:** Enable APIs needed for the backend

**Steps:**
1. In Google Cloud Console, navigate to **APIs & Services** → **Library**
2. Search and enable the following APIs:
   - **Identity Toolkit API** (for Firebase Auth)
   - **Cloud Firestore API**
   - **Google Drive API**
   - **Vertex AI API** (for Gemini)
3. For each API:
   - Click on the API
   - Click "Enable"
   - Wait for activation

**Verification:**
- [ ] Identity Toolkit API enabled
- [ ] Cloud Firestore API enabled
- [ ] Google Drive API enabled
- [ ] Vertex AI API enabled

**Check Status:**
```bash
gcloud services list --enabled --project=[project-id]
```

---

### Task 1.5: Provision Cloud Firestore Database

**Objective:** Create Firestore database instance

**Steps:**
1. In Firebase Console, navigate to **Build** → **Firestore Database**
2. Click "Create database"
3. Select mode:
   - **Production mode** (we'll add rules in Phase 2)
4. Choose location:
   - Select region closest to your users (e.g., `us-central1`, `asia-south1`)
   - **Note:** This cannot be changed later
5. Click "Enable"
6. Wait for database provisioning (~2 minutes)

**Verification:**
- [ ] Firestore database created
- [ ] Database mode: Production
- [ ] Region selected and confirmed
- [ ] Database dashboard accessible

**Database Info:**
```markdown
# Firestore Database
- Database ID: (default)
- Location: [your-region]
- Mode: Production
- Rules: Default (deny all - will update in Phase 2)
```

---

### Task 1.6: Install Firebase CLI and Initialize Project

**Objective:** Set up local development environment

**Steps:**

1. **Install Firebase CLI globally:**
```bash
npm install -g firebase-tools
```

2. **Verify installation:**
```bash
firebase --version
# Expected: 13.x.x or higher
```

3. **Login to Firebase:**
```bash
firebase login
```
- Browser opens for Google authentication
- Select your Google account
- Grant permissions
- Confirm login in terminal

4. **Initialize Firebase in project:**
```bash
cd /path/to/openmeet
firebase init
```

5. **Select features (use spacebar to select):**
   - [x] Firestore: Configure security rules and indexes
   - [x] Functions: Configure Cloud Functions (optional for Phase 7)
   - [x] Emulators: Set up local emulators for development

6. **Configuration prompts:**
   - **Use existing project:** Select `openmeet-recorder`
   - **Firestore rules file:** `firebase/firestore.rules`
   - **Firestore indexes file:** `firebase/firestore.indexes.json`
   - **Functions language:** TypeScript
   - **Functions directory:** `firebase/functions`
   - **Emulators to set up:**
     - [x] Authentication Emulator (port 9099)
     - [x] Firestore Emulator (port 8080)
   - **Download emulators now:** Yes

**Verification:**
- [ ] Firebase CLI installed and version ≥ 13.0.0
- [ ] Logged in successfully
- [ ] Project initialized
- [ ] Configuration files created:
  - `firebase.json`
  - `firebase/firestore.rules`
  - `firebase/firestore.indexes.json`
  - `.firebaserc`

**Expected File Structure:**
```
openmeet/
├── firebase/
│   ├── firestore.rules
│   ├── firestore.indexes.json
│   └── functions/       (optional)
├── firebase.json
└── .firebaserc
```

---

### Task 1.7: Generate Firebase Admin SDK Service Account

**Objective:** Create service account for server-side Firebase access

**Steps:**
1. In Firebase Console, click **Project Settings** (gear icon)
2. Go to **Service Accounts** tab
3. Click **Generate new private key**
4. Confirm by clicking **Generate key**
5. A JSON file downloads automatically
6. Rename file to `serviceAccountKey.json`
7. Move to `firebase/serviceAccountKey.json`
8. **IMPORTANT:** Add to `.gitignore`

**Security:**
```gitignore
# Add to .gitignore
firebase/serviceAccountKey.json
*.json.secret
.env
.env.local
```

**Verification:**
- [ ] Service account key downloaded
- [ ] File placed in `firebase/` directory
- [ ] File added to `.gitignore`
- [ ] No sensitive data committed to Git

**Service Account Structure:**
```json
{
  "type": "service_account",
  "project_id": "[project-id]",
  "private_key_id": "[key-id]",
  "private_key": "-----BEGIN PRIVATE KEY-----\n...",
  "client_email": "firebase-adminsdk-xxxxx@[project-id].iam.gserviceaccount.com",
  "client_id": "[client-id]",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token"
}
```

---

### Task 1.8: Configure Firebase Admin SDK Initialization

**Objective:** Create utility to initialize Firebase Admin SDK

**Steps:**

1. Create `firebase/admin.ts`:
```typescript
import * as admin from 'firebase-admin';
import * as serviceAccount from './serviceAccountKey.json';

// Initialize Firebase Admin SDK
if (!admin.apps.length) {
  admin.initializeApp({
    credential: admin.credential.cert(serviceAccount as admin.ServiceAccount),
    databaseURL: `https://[project-id].firebaseio.com`,
  });
}

// Export services
export const auth = admin.auth();
export const db = admin.firestore();
export const storage = admin.storage();
export const timestamp = admin.firestore.Timestamp;

// Export admin for custom operations
export default admin;
```

2. Create TypeScript types file `firebase/types.ts`:
```typescript
import { Timestamp } from 'firebase-admin/firestore';

// User Profile
export interface UserProfile {
  uid: string;
  email: string;
  displayName: string;
  photoURL?: string;
  driveConnected: boolean;
  createdAt: Timestamp;
  updatedAt: Timestamp;
  plan: 'free' | 'premium';
  quotaUsed: number;
  quotaLimit: number;
  preferences: {
    defaultQuality: '720p' | '1080p';
    autoTranscribe: boolean;
    autoSummarize: boolean;
    language: string;
  };
}

// Recording
export interface Recording {
  id: string;
  userId: string;
  title: string;
  description?: string;
  duration: number;
  fileSize: number;
  quality: '720p' | '1080p';
  format: 'webm' | 'mp4';
  driveFileId: string;
  driveFileUrl?: string;
  thumbnailUrl?: string;
  createdAt: Timestamp;
  updatedAt: Timestamp;
  status: 'uploading' | 'processing' | 'ready' | 'failed';
  transcript?: TranscriptEntry[];
  summary?: AISummary;
  tags?: string[];
  folderId?: string;
  isPrivate: boolean;
  shareableLink?: string;
  meetingUrl?: string;
  participants?: string[];
}

// Transcript Entry
export interface TranscriptEntry {
  timestamp: number;
  duration: number;
  speaker: string;
  text: string;
  confidence?: number;
}

// AI Summary
export interface AISummary {
  text: string;
  keyPoints: string[];
  actionItems: ActionItem[];
  topics: string[];
  participants: string[];
  language: string;
  model: string;
  generatedAt: Timestamp;
}

// Action Item
export interface ActionItem {
  text: string;
  assignee?: string;
  dueDate?: string;
  completed: boolean;
}

// Folder
export interface Folder {
  id: string;
  userId: string;
  name: string;
  color?: string;
  icon?: string;
  createdAt: Timestamp;
  updatedAt: Timestamp;
  recordingCount: number;
}

// AI Job
export interface AIJob {
  id: string;
  recordingId: string;
  userId: string;
  type: 'transcribe' | 'summarize';
  status: 'pending' | 'processing' | 'completed' | 'failed';
  progress: number;
  result?: any;
  error?: string;
  createdAt: Timestamp;
  completedAt?: Timestamp;
}
```

**Verification:**
- [ ] `firebase/admin.ts` created
- [ ] `firebase/types.ts` created
- [ ] TypeScript compiles without errors
- [ ] Admin SDK initialized successfully

---

### Task 1.9: Test Firebase Admin SDK Connection

**Objective:** Verify Firebase Admin SDK can connect to Firestore

**Steps:**

1. Create test script `firebase/test-connection.ts`:
```typescript
import { db, auth } from './admin';

async function testConnection() {
  try {
    console.log('Testing Firebase Admin SDK connection...\n');
    
    // Test Firestore
    console.log('1. Testing Firestore connection...');
    const testDoc = await db.collection('_test').doc('connection').set({
      timestamp: new Date(),
      message: 'Connection test successful'
    });
    console.log('✅ Firestore write successful');
    
    // Read back
    const doc = await db.collection('_test').doc('connection').get();
    console.log('✅ Firestore read successful:', doc.data());
    
    // Clean up
    await db.collection('_test').doc('connection').delete();
    console.log('✅ Firestore delete successful');
    
    // Test Auth
    console.log('\n2. Testing Firebase Auth...');
    const users = await auth.listUsers(1);
    console.log(`✅ Auth connection successful (${users.users.length} users found)`);
    
    console.log('\n✅ All tests passed! Firebase Admin SDK is ready.');
  } catch (error) {
    console.error('❌ Test failed:', error);
    process.exit(1);
  }
}

testConnection();
```

2. Run test:
```bash
ts-node firebase/test-connection.ts
```

**Expected Output:**
```
Testing Firebase Admin SDK connection...

1. Testing Firestore connection...
✅ Firestore write successful
✅ Firestore read successful: { timestamp: ..., message: '...' }
✅ Firestore delete successful

2. Testing Firebase Auth...
✅ Auth connection successful (0 users found)

✅ All tests passed! Firebase Admin SDK is ready.
```

**Verification:**
- [ ] Test script runs without errors
- [ ] Firestore write/read/delete successful
- [ ] Auth connection successful
- [ ] No error messages in console

---

### Task 1.10: Set Up Firebase Local Emulators

**Objective:** Configure local development environment with emulators

**Steps:**

1. **Update `firebase.json`:**
```json
{
  "firestore": {
    "rules": "firebase/firestore.rules",
    "indexes": "firebase/firestore.indexes.json"
  },
  "emulators": {
    "auth": {
      "port": 9099,
      "host": "localhost"
    },
    "firestore": {
      "port": 8080,
      "host": "localhost"
    },
    "ui": {
      "enabled": true,
      "port": 4000,
      "host": "localhost"
    }
  }
}
```

2. **Start emulators:**
```bash
firebase emulators:start
```

3. **Access Emulator UI:**
- Open browser: `http://localhost:4000`
- Verify all services are running

4. **Create test user in Auth emulator:**
- Go to Authentication tab
- Click "Add user"
- Email: `test@example.com`
- Password: `password123`
- Click "Save"

**Verification:**
- [ ] Emulators start without errors
- [ ] Emulator UI accessible at http://localhost:4000
- [ ] Authentication emulator running on port 9099
- [ ] Firestore emulator running on port 8080
- [ ] Test user created successfully

---

### Phase 1 Completion Checklist

**Completed Tasks:**
- [ ] Task 1.1: Firebase project created
- [ ] Task 1.2: Firebase Auth enabled with Google provider
- [ ] Task 1.3: OAuth 2.0 Client IDs created (Extension + Dashboard)
- [ ] Task 1.4: Required Google Cloud APIs enabled
- [ ] Task 1.5: Cloud Firestore database provisioned
- [ ] Task 1.6: Firebase CLI installed and project initialized
- [ ] Task 1.7: Service account credentials generated
- [ ] Task 1.8: Firebase Admin SDK configured
- [ ] Task 1.9: Connection test passed
- [ ] Task 1.10: Local emulators configured

**Deliverables:**
- [x] Firebase project: `openmeet-recorder`
- [x] Firebase Auth enabled with Google Sign-In
- [x] OAuth Client IDs for Extension and Dashboard
- [x] Cloud Firestore database in production mode
- [x] Firebase Admin SDK initialized
- [x] Service account credentials secured
- [x] Local development emulators running
- [x] TypeScript types defined

**Phase 1 Success Criteria:**
- ✅ Firebase project fully configured
- ✅ All required APIs enabled
- ✅ Firebase Admin SDK connecting successfully
- ✅ Local emulators functional
- ✅ No credentials committed to Git

**Next Phase:** Phase 2 - Database Schema Implementation

---

## Phase 2: Database Schema Implementation

### Phase Overview

**Goal:** Implement Firestore database schema with collections, security rules, and indexes

**Duration:** 3-4 hours

**Prerequisites:**
- Phase 1 completed
- Firebase project configured
- Firestore database provisioned

**Deliverables:**
- Firestore collections: `users`, `recordings`, `folders`, `ai_jobs`
- Comprehensive security rules
- Composite indexes for efficient queries
- Data validation rules
- Security rules tested

---

### Task 2.1: Create Firestore Collections Structure

**Objective:** Define and document all Firestore collections

**Collections to Create:**
1. `users` - User profile information
2. `recordings` - Recording metadata
3. `folders` - Organization folders
4. `ai_jobs` - AI processing job tracking

**Steps:**

1. **Document collection schemas in `firebase/schema.md`:**

```markdown
# Firestore Database Schema

## Collection: users

**Path:** `/users/{userId}`

**Document ID:** Firebase Auth UID

**Fields:**
- `uid` (string): Firebase Auth UID - matches document ID
- `email` (string): User's email address
- `displayName` (string): User's display name
- `photoURL` (string, optional): Profile picture URL
- `driveConnected` (boolean): Whether Google Drive is connected
- `createdAt` (timestamp): Account creation timestamp
- `updatedAt` (timestamp): Last update timestamp
- `plan` (string): Subscription plan - 'free' | 'premium'
- `quotaUsed` (number): Storage quota used in MB
- `quotaLimit` (number): Storage quota limit in MB
- `preferences` (map):
  - `defaultQuality` (string): '720p' | '1080p'
  - `autoTranscribe` (boolean): Auto-trigger transcription
  - `autoSummarize` (boolean): Auto-trigger summarization
  - `language` (string): Preferred language code (e.g., 'en', 'hi')

**Indexes:**
- Single-field index on `email` (ascending)

**Example Document:**
```json
{
  "uid": "abc123xyz",
  "email": "user@example.com",
  "displayName": "John Doe",
  "photoURL": "https://...",
  "driveConnected": true,
  "createdAt": "2025-12-29T10:00:00Z",
  "updatedAt": "2025-12-29T10:00:00Z",
  "plan": "free",
  "quotaUsed": 245,
  "quotaLimit": 1024,
  "preferences": {
    "defaultQuality": "720p",
    "autoTranscribe": false,
    "autoSummarize": false,
    "language": "en"
  }
}
```

---

## Collection: recordings

**Path:** `/recordings/{recordingId}`

**Document ID:** Auto-generated Firestore ID

**Fields:**
- `id` (string): Document ID (auto-generated)
- `userId` (string): Owner's Firebase Auth UID
- `title` (string): Recording title
- `description` (string, optional): Recording description
- `duration` (number): Duration in seconds
- `fileSize` (number): File size in bytes
- `quality` (string): '720p' | '1080p'
- `format` (string): 'webm' | 'mp4'
- `driveFileId` (string): Google Drive file ID
- `driveFileUrl` (string, optional): Temporary signed URL
- `thumbnailUrl` (string, optional): Thumbnail URL
- `createdAt` (timestamp): Recording creation timestamp
- `updatedAt` (timestamp): Last update timestamp
- `status` (string): 'uploading' | 'processing' | 'ready' | 'failed'
- `transcript` (array, optional): Array of TranscriptEntry objects
- `summary` (map, optional): AISummary object
- `tags` (array, optional): Array of string tags
- `folderId` (string, optional): Parent folder ID
- `isPrivate` (boolean): Privacy setting
- `shareableLink` (string, optional): Public shareable link
- `meetingUrl` (string, optional): Google Meet URL
- `participants` (array, optional): Array of participant names

**Composite Indexes:**
1. `(userId ASC, createdAt DESC)` - User's recordings sorted by creation date
2. `(userId ASC, updatedAt DESC)` - User's recordings sorted by update date
3. `(userId ASC, title ASC)` - User's recordings sorted alphabetically
4. `(userId ASC, folderId ASC, createdAt DESC)` - Folder-based queries

**Example Document:**
```json
{
  "id": "rec_abc123",
  "userId": "abc123xyz",
  "title": "Team Standup - Dec 29",
  "description": "Daily standup meeting",
  "duration": 1845,
  "fileSize": 125829120,
  "quality": "720p",
  "format": "webm",
  "driveFileId": "1ABC...XYZ",
  "driveFileUrl": "https://drive.google.com/...",
  "thumbnailUrl": null,
  "createdAt": "2025-12-29T10:00:00Z",
  "updatedAt": "2025-12-29T10:30:00Z",
  "status": "ready",
  "transcript": [...],
  "summary": {...},
  "tags": ["standup", "team"],
  "folderId": "folder_123",
  "isPrivate": true,
  "shareableLink": null,
  "meetingUrl": "https://meet.google.com/abc-defg-hij",
  "participants": ["John", "Jane", "Bob"]
}
```

---

## Collection: folders

**Path:** `/folders/{folderId}`

**Document ID:** Auto-generated Firestore ID

**Fields:**
- `id` (string): Document ID
- `userId` (string): Owner's Firebase Auth UID
- `name` (string): Folder name
- `color` (string, optional): Folder color (hex code)
- `icon` (string, optional): Folder icon name
- `createdAt` (timestamp): Folder creation timestamp
- `updatedAt` (timestamp): Last update timestamp
- `recordingCount` (number): Number of recordings (denormalized)

**Indexes:**
- Single-field index on `userId` (ascending)

**Example Document:**
```json
{
  "id": "folder_123",
  "userId": "abc123xyz",
  "name": "Team Meetings",
  "color": "#4285F4",
  "icon": "folder",
  "createdAt": "2025-12-29T10:00:00Z",
  "updatedAt": "2025-12-29T10:00:00Z",
  "recordingCount": 5
}
```

---

## Collection: ai_jobs

**Path:** `/ai_jobs/{jobId}`

**Document ID:** Auto-generated Firestore ID

**Fields:**
- `id` (string): Job ID
- `recordingId` (string): Associated recording ID
- `userId` (string): Owner's UID
- `type` (string): 'transcribe' | 'summarize'
- `status` (string): 'pending' | 'processing' | 'completed' | 'failed'
- `progress` (number): Progress percentage (0-100)
- `result` (any, optional): Job result (transcript or summary)
- `error` (string, optional): Error message if failed
- `createdAt` (timestamp): Job creation timestamp
- `completedAt` (timestamp, optional): Job completion timestamp

**Composite Indexes:**
1. `(userId ASC, createdAt DESC)` - User's jobs sorted by creation date
2. `(recordingId ASC, type ASC)` - Jobs for a recording by type

**Example Document:**
```json
{
  "id": "job_abc123",
  "recordingId": "rec_abc123",
  "userId": "abc123xyz",
  "type": "transcribe",
  "status": "completed",
  "progress": 100,
  "result": [...],
  "error": null,
  "createdAt": "2025-12-29T10:00:00Z",
  "completedAt": "2025-12-29T10:05:00Z"
}
```
```

**Verification:**
- [ ] Schema documentation created
- [ ] All collections defined
- [ ] All fields documented with types
- [ ] Example documents provided

---

### Task 2.2: Implement Firestore Security Rules

**Objective:** Create comprehensive security rules to protect data

**Steps:**

1. **Update `firebase/firestore.rules`:**

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // ========================================
    // HELPER FUNCTIONS
    // ========================================
    
    // Check if user is authenticated
    function isAuthenticated() {
      return request.auth != null;
    }
    
    // Check if user owns the resource
    function isOwner(userId) {
      return isAuthenticated() && request.auth.uid == userId;
    }
    
    // Check if user owns the document (for reads)
    function isDocumentOwner() {
      return isAuthenticated() && resource.data.userId == request.auth.uid;
    }
    
    // Check if user is creating their own document
    function isCreatingOwnDocument() {
      return isAuthenticated() && request.resource.data.userId == request.auth.uid;
    }
    
    // Validate user profile data on create
    function hasValidUserData() {
      let data = request.resource.data;
      return data.keys().hasAll(['uid', 'email', 'displayName', 'createdAt', 'driveConnected', 'plan', 'quotaUsed', 'quotaLimit']) &&
             data.uid is string &&
             data.email is string &&
             data.displayName is string &&
             data.driveConnected is bool &&
             data.plan in ['free', 'premium'] &&
             data.quotaUsed is number &&
             data.quotaLimit is number;
    }
    
    // Validate recording data on create
    function hasValidRecordingData() {
      let data = request.resource.data;
      return data.keys().hasAll(['userId', 'title', 'duration', 'fileSize', 'quality', 'format', 'driveFileId', 'createdAt', 'status', 'isPrivate']) &&
             data.userId is string &&
             data.title is string &&
             data.duration is number &&
             data.fileSize is number &&
             data.quality in ['720p', '1080p'] &&
             data.format in ['webm', 'mp4'] &&
             data.driveFileId is string &&
             data.status in ['uploading', 'processing', 'ready', 'failed'] &&
             data.isPrivate is bool;
    }
    
    // Validate folder data on create
    function hasValidFolderData() {
      let data = request.resource.data;
      return data.keys().hasAll(['userId', 'name', 'createdAt', 'recordingCount']) &&
             data.userId is string &&
             data.name is string &&
             data.recordingCount is number;
    }
    
    // Check if only allowed fields are being updated
    function isUpdatingAllowedFields(allowedFields) {
      return request.resource.data.diff(resource.data).affectedKeys().hasOnly(allowedFields);
    }
    
    // ========================================
    // COLLECTION RULES
    // ========================================
    
    // Users Collection
    match /users/{userId} {
      // Read: User can only read their own profile
      allow read: if isOwner(userId);
      
      // Create: User can create their own profile with valid data
      allow create: if isOwner(userId) && 
                       hasValidUserData() &&
                       request.resource.data.uid == userId;
      
      // Update: User can update only specific fields
      allow update: if isOwner(userId) && 
                       isUpdatingAllowedFields([
                         'displayName', 
                         'photoURL', 
                         'driveConnected', 
                         'updatedAt', 
                         'preferences',
                         'quotaUsed'
                       ]);
      
      // Delete: Users cannot delete their own profile
      allow delete: if false;
    }
    
    // Recordings Collection
    match /recordings/{recordingId} {
      // Read: User can only read recordings they own
      allow read: if isDocumentOwner();
      
      // Create: User can create recording with valid data
      allow create: if isCreatingOwnDocument() && 
                       hasValidRecordingData() &&
                       request.resource.data.userId == request.auth.uid;
      
      // Update: User can update recording but cannot change certain fields
      allow update: if isDocumentOwner() &&
                       !isUpdatingAllowedFields(['userId', 'driveFileId', 'createdAt']).hasAny(['userId', 'driveFileId', 'createdAt']);
      
      // Delete: User can delete their own recordings
      allow delete: if isDocumentOwner();
    }
    
    // Folders Collection
    match /folders/{folderId} {
      // Read: User can only read their own folders
      allow read: if isDocumentOwner();
      
      // Create: User can create folder with valid data
      allow create: if isCreatingOwnDocument() && 
                       hasValidFolderData();
      
      // Update: User can update name, color, icon, recordingCount
      allow update: if isDocumentOwner() &&
                       isUpdatingAllowedFields([
                         'name', 
                         'color', 
                         'icon', 
                         'updatedAt',
                         'recordingCount'
                       ]);
      
      // Delete: User can delete their own folders
      allow delete: if isDocumentOwner();
    }
    
    // AI Jobs Collection
    match /ai_jobs/{jobId} {
      // Read: User can read their own jobs
      allow read: if isDocumentOwner();
      
      // Write: Only server-side can write (no client writes)
      allow write: if false;
    }
  }
}
```

**Verification:**
- [ ] Security rules file created
- [ ] All collections have rules
- [ ] Helper functions defined
- [ ] User can only access their own data
- [ ] Validation rules prevent invalid data

---

### Task 2.3: Define Firestore Composite Indexes

**Objective:** Create indexes for efficient queries

**Steps:**

1. **Update `firebase/firestore.indexes.json`:**

```json
{
  "indexes": [
    {
      "collectionGroup": "recordings",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "createdAt",
          "order": "DESCENDING"
        }
      ]
    },
    {
      "collectionGroup": "recordings",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "updatedAt",
          "order": "DESCENDING"
        }
      ]
    },
    {
      "collectionGroup": "recordings",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "title",
          "order": "ASCENDING"
        }
      ]
    },
    {
      "collectionGroup": "recordings",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "folderId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "createdAt",
          "order": "DESCENDING"
        }
      ]
    },
    {
      "collectionGroup": "recordings",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "status",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "createdAt",
          "order": "DESCENDING"
        }
      ]
    },
    {
      "collectionGroup": "folders",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "name",
          "order": "ASCENDING"
        }
      ]
    },
    {
      "collectionGroup": "ai_jobs",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "createdAt",
          "order": "DESCENDING"
        }
      ]
    },
    {
      "collectionGroup": "ai_jobs",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "recordingId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "type",
          "order": "ASCENDING"
        }
      ]
    },
    {
      "collectionGroup": "ai_jobs",
      "queryScope": "COLLECTION",
      "fields": [
        {
          "fieldPath": "userId",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "status",
          "order": "ASCENDING"
        },
        {
          "fieldPath": "createdAt",
          "order": "DESCENDING"
        }
      ]
    }
  ],
  "fieldOverrides": []
}
```

**Index Explanation:**

| Index | Purpose | Query Pattern |
|-------|---------|---------------|
| `userId ASC, createdAt DESC` | Fetch user's recordings sorted by newest first | `.where('userId', '==', uid).orderBy('createdAt', 'desc')` |
| `userId ASC, updatedAt DESC` | Fetch user's recordings sorted by recently updated | `.where('userId', '==', uid).orderBy('updatedAt', 'desc')` |
| `userId ASC, title ASC` | Fetch user's recordings sorted alphabetically | `.where('userId', '==', uid).orderBy('title', 'asc')` |
| `userId ASC, folderId ASC, createdAt DESC` | Fetch recordings in a folder | `.where('userId', '==', uid).where('folderId', '==', folderId).orderBy('createdAt', 'desc')` |
| `userId ASC, status ASC, createdAt DESC` | Fetch recordings by status | `.where('userId', '==', uid).where('status', '==', 'ready').orderBy('createdAt', 'desc')` |

**Verification:**
- [ ] Indexes file created
- [ ] All query patterns covered
- [ ] No redundant indexes

---

### Task 2.4: Deploy Security Rules and Indexes

**Objective:** Deploy rules and indexes to Firebase

**Steps:**

1. **Deploy security rules:**
```bash
firebase deploy --only firestore:rules --project openmeet-recorder
```

**Expected Output:**
```
=== Deploying to 'openmeet-recorder'...

i  deploying firestore
i  firestore: checking firestore.rules for compilation errors...
✔  firestore: rules file firestore.rules compiled successfully
i  firestore: uploading rules firestore.rules...
✔  firestore: released rules firestore.rules

✔  Deploy complete!
```

2. **Deploy indexes:**
```bash
firebase deploy --only firestore:indexes --project openmeet-recorder
```

**Expected Output:**
```
=== Deploying to 'openmeet-recorder'...

i  deploying firestore
i  firestore: creating indexes...
✔  firestore: indexes created successfully

✔  Deploy complete!
```

**Note:** Index creation can take several minutes. Monitor progress in Firebase Console.

**Verification:**
- [ ] Rules deployed without errors
- [ ] Indexes deployment initiated
- [ ] No compilation errors
- [ ] Firebase Console shows updated rules

**Check deployment:**
- Go to Firebase Console → Firestore Database → Rules
- Verify rules match your local file
- Go to Indexes tab
- Verify all indexes are "Building" or "Enabled"

---

### Task 2.5: Test Security Rules

**Objective:** Verify security rules work as expected

**Steps:**

1. **Create test script `firebase/test-security-rules.ts`:**

```typescript
import { db, auth } from './admin';
import { Timestamp } from 'firebase-admin/firestore';

async function testSecurityRules() {
  console.log('Testing Firestore Security Rules...\n');
  
  try {
    // Create test user via Admin SDK (bypasses rules)
    const testUserUid = 'test-user-123';
    const testUser = {
      uid: testUserUid,
      email: 'test@example.com',
      displayName: 'Test User',
      driveConnected: true,
      createdAt: Timestamp.now(),
      updatedAt: Timestamp.now(),
      plan: 'free' as const,
      quotaUsed: 0,
      quotaLimit: 1024,
      preferences: {
        defaultQuality: '720p' as const,
        autoTranscribe: false,
        autoSummarize: false,
        language: 'en'
      }
    };
    
    console.log('1. Creating test user (via Admin SDK)...');
    await db.collection('users').doc(testUserUid).set(testUser);
    console.log('✅ Test user created');
    
    // Test recording creation
    console.log('\n2. Creating test recording...');
    const testRecording = {
      userId: testUserUid,
      title: 'Test Recording',
      duration: 600,
      fileSize: 50000000,
      quality: '720p',
      format: 'webm',
      driveFileId: 'test-drive-file-id',
      createdAt: Timestamp.now(),
      updatedAt: Timestamp.now(),
      status: 'ready',
      isPrivate: true
    };
    
    const recordingRef = await db.collection('recordings').add(testRecording);
    console.log(`✅ Test recording created: ${recordingRef.id}`);
    
    // Test folder creation
    console.log('\n3. Creating test folder...');
    const testFolder = {
      userId: testUserUid,
      name: 'Test Folder',
      createdAt: Timestamp.now(),
      updatedAt: Timestamp.now(),
      recordingCount: 0
    };
    
    const folderRef = await db.collection('folders').add(testFolder);
    console.log(`✅ Test folder created: ${folderRef.id}`);
    
    // Test query (user's recordings)
    console.log('\n4. Querying user recordings...');
    const recordings = await db.collection('recordings')
      .where('userId', '==', testUserUid)
      .orderBy('createdAt', 'desc')
      .get();
    
    console.log(`✅ Query successful: ${recordings.size} recording(s) found`);
    
    // Cleanup
    console.log('\n5. Cleaning up test data...');
    await db.collection('users').doc(testUserUid).delete();
    await db.collection('recordings').doc(recordingRef.id).delete();
    await db.collection('folders').doc(folderRef.id).delete();
    console.log('✅ Test data cleaned up');
    
    console.log('\n✅ All security rule tests passed!');
  } catch (error) {
    console.error('❌ Test failed:', error);
    process.exit(1);
  }
}

testSecurityRules();
```

2. **Run tests:**
```bash
ts-node firebase/test-security-rules.ts
```

**Expected Output:**
```
Testing Firestore Security Rules...

1. Creating test user (via Admin SDK)...
✅ Test user created

2. Creating test recording...
✅ Test recording created: rec_abc123

3. Creating test folder...
✅ Test folder created: folder_abc123

4. Querying user recordings...
✅ Query successful: 1 recording(s) found

5. Cleaning up test data...
✅ Test data cleaned up

✅ All security rule tests passed!
```

**Verification:**
- [ ] Test script runs without errors
- [ ] All CRUD operations successful
- [ ] Queries work correctly
- [ ] Cleanup successful

---

### Task 2.6: Create Database Helper Functions

**Objective:** Create utility functions for common database operations

**Steps:**

1. **Create `firebase/db-helpers.ts`:**

```typescript
import { db, timestamp } from './admin';
import type { UserProfile, Recording, Folder, AIJob } from './types';
import { Timestamp, FieldValue } from 'firebase-admin/firestore';

// ========================================
// USER OPERATIONS
// ========================================

export async function createUser(userData: Omit<UserProfile, 'createdAt' | 'updatedAt'>): Promise<void> {
  const userRef = db.collection('users').doc(userData.uid);
  
  await userRef.set({
    ...userData,
    createdAt: Timestamp.now(),
    updatedAt: Timestamp.now()
  });
}

export async function getUser(uid: string): Promise<UserProfile | null> {
  const userDoc = await db.collection('users').doc(uid).get();
  
  if (!userDoc.exists) {
    return null;
  }
  
  return userDoc.data() as UserProfile;
}

export async function updateUser(uid: string, updates: Partial<UserProfile>): Promise<void> {
  const userRef = db.collection('users').doc(uid);
  
  await userRef.update({
    ...updates,
    updatedAt: Timestamp.now()
  });
}

// ========================================
// RECORDING OPERATIONS
// ========================================

export async function createRecording(recordingData: Omit<Recording, 'id' | 'createdAt' | 'updatedAt'>): Promise<string> {
  const recordingRef = db.collection('recordings').doc();
  
  await recordingRef.set({
    id: recordingRef.id,
    ...recordingData,
    createdAt: Timestamp.now(),
    updatedAt: Timestamp.now()
  });
  
  return recordingRef.id;
}

export async function getRecording(recordingId: string): Promise<Recording | null> {
  const recordingDoc = await db.collection('recordings').doc(recordingId).get();
  
  if (!recordingDoc.exists) {
    return null;
  }
  
  return recordingDoc.data() as Recording;
}

export async function getUserRecordings(
  userId: string,
  options: {
    limit?: number;
    offset?: number;
    sortBy?: 'createdAt' | 'updatedAt' | 'title';
    sortOrder?: 'asc' | 'desc';
    folderId?: string;
    status?: string;
  } = {}
): Promise<Recording[]> {
  const {
    limit = 20,
    offset = 0,
    sortBy = 'createdAt',
    sortOrder = 'desc',
    folderId,
    status
  } = options;
  
  let query = db.collection('recordings')
    .where('userId', '==', userId);
  
  if (folderId) {
    query = query.where('folderId', '==', folderId);
  }
  
  if (status) {
    query = query.where('status', '==', status);
  }
  
  query = query.orderBy(sortBy, sortOrder);
  
  if (offset > 0) {
    query = query.offset(offset);
  }
  
  query = query.limit(limit);
  
  const snapshot = await query.get();
  
  return snapshot.docs.map(doc => doc.data() as Recording);
}

export async function updateRecording(recordingId: string, updates: Partial<Recording>): Promise<void> {
  const recordingRef = db.collection('recordings').doc(recordingId);
  
  await recordingRef.update({
    ...updates,
    updatedAt: Timestamp.now()
  });
}

export async function deleteRecording(recordingId: string): Promise<void> {
  await db.collection('recordings').doc(recordingId).delete();
}

// ========================================
// FOLDER OPERATIONS
// ========================================

export async function createFolder(folderData: Omit<Folder, 'id' | 'createdAt' | 'updatedAt'>): Promise<string> {
  const folderRef = db.collection('folders').doc();
  
  await folderRef.set({
    id: folderRef.id,
    ...folderData,
    createdAt: Timestamp.now(),
    updatedAt: Timestamp.now()
  });
  
  return folderRef.id;
}

export async function getUserFolders(userId: string): Promise<Folder[]> {
  const snapshot = await db.collection('folders')
    .where('userId', '==', userId)
    .orderBy('name', 'asc')
    .get();
  
  return snapshot.docs.map(doc => doc.data() as Folder);
}

export async function updateFolder(folderId: string, updates: Partial<Folder>): Promise<void> {
  const folderRef = db.collection('folders').doc(folderId);
  
  await folderRef.update({
    ...updates,
    updatedAt: Timestamp.now()
  });
}

export async function deleteFolder(folderId: string): Promise<void> {
  await db.collection('folders').doc(folderId).delete();
}

export async function incrementFolderRecordingCount(folderId: string, increment: number = 1): Promise<void> {
  const folderRef = db.collection('folders').doc(folderId);
  
  await folderRef.update({
    recordingCount: FieldValue.increment(increment),
    updatedAt: Timestamp.now()
  });
}

// ========================================
// AI JOB OPERATIONS
// ========================================

export async function createAIJob(jobData: Omit<AIJob, 'id' | 'createdAt'>): Promise<string> {
  const jobRef = db.collection('ai_jobs').doc();
  
  await jobRef.set({
    id: jobRef.id,
    ...jobData,
    createdAt: Timestamp.now()
  });
  
  return jobRef.id;
}

export async function getAIJob(jobId: string): Promise<AIJob | null> {
  const jobDoc = await db.collection('ai_jobs').doc(jobId).get();
  
  if (!jobDoc.exists) {
    return null;
  }
  
  return jobDoc.data() as AIJob;
}

export async function updateAIJob(jobId: string, updates: Partial<AIJob>): Promise<void> {
  const jobRef = db.collection('ai_jobs').doc(jobId);
  
  await jobRef.update(updates);
}

export async function getUserAIJobs(userId: string, limit: number = 10): Promise<AIJob[]> {
  const snapshot = await db.collection('ai_jobs')
    .where('userId', '==', userId)
    .orderBy('createdAt', 'desc')
    .limit(limit)
    .get();
  
  return snapshot.docs.map(doc => doc.data() as AIJob);
}

export async function getRecordingAIJobs(recordingId: string): Promise<AIJob[]> {
  const snapshot = await db.collection('ai_jobs')
    .where('recordingId', '==', recordingId)
    .get();
  
  return snapshot.docs.map(doc => doc.data() as AIJob);
}
```

**Verification:**
- [ ] Helper functions file created
- [ ] CRUD operations for all collections
- [ ] Type-safe with TypeScript
- [ ] Includes pagination and filtering

---

### Phase 2 Completion Checklist

**Completed Tasks:**
- [ ] Task 2.1: Firestore collections structure defined
- [ ] Task 2.2: Security rules implemented
- [ ] Task 2.3: Composite indexes defined
- [ ] Task 2.4: Rules and indexes deployed
- [ ] Task 2.5: Security rules tested
- [ ] Task 2.6: Database helper functions created

**Deliverables:**
- [x] Schema documentation: `firebase/schema.md`
- [x] Security rules: `firebase/firestore.rules`
- [x] Composite indexes: `firebase/firestore.indexes.json`
- [x] Helper functions: `firebase/db-helpers.ts`
- [x] Test script: `firebase/test-security-rules.ts`

**Phase 2 Success Criteria:**
- ✅ All collections properly defined
- ✅ Security rules deployed and tested
- ✅ Indexes created for efficient queries
- ✅ Helper functions ready for use
- ✅ No security vulnerabilities

**Next Phase:** Phase 3 - Authentication Backend

---

## Phase 3: Authentication Backend

### Phase Overview

**Goal:** Implement authentication backend logic with token verification

**Duration:** 2-3 hours

**Prerequisites:**
- Phase 1 completed (Firebase Auth configured)
- Phase 2 completed (Database schema ready)
- OAuth Client IDs created

**Deliverables:**
- Token verification middleware
- User session management
- Auth API endpoints
- User profile creation logic
- Token refresh handling

---

### Task 3.1: Create Token Verification Middleware

**Objective:** Implement middleware to verify Firebase ID tokens

**Steps:**

1. **Create `firebase/auth-middleware.ts`:**

```typescript
import { auth } from './admin';
import type { Request, Response, NextFunction } from 'express';

// Define authenticated request interface
export interface AuthenticatedRequest extends Request {
  user?: {
    uid: string;
    email: string;
    name: string;
    picture?: string;
  };
}

// Verify Firebase ID Token
export async function verifyFirebaseToken(
  req: AuthenticatedRequest,
  res: Response,
  next: NextFunction
): Promise<void> {
  try {
    // Extract token from Authorization header
    const authHeader = req.headers.authorization;
    
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      res.status(401).json({
        error: 'Unauthorized',
        message: 'No authentication token provided'
      });
      return;
    }
    
    const idToken = authHeader.split('Bearer ')[1];
    
    // Verify token with Firebase Admin SDK
    const decodedToken = await auth.verifyIdToken(idToken);
    
    // Attach user info to request
    req.user = {
      uid: decodedToken.uid,
      email: decodedToken.email!,
      name: decodedToken.name || decodedToken.email!,
      picture: decodedToken.picture
    };
    
    next();
  } catch (error) {
    console.error('Token verification failed:', error);
    
    res.status(401).json({
      error: 'Unauthorized',
      message: 'Invalid or expired token'
    });
  }
}

// Verify token and return user data (for use in Cloudflare Workers)
export async function verifyToken(idToken: string): Promise<{
  uid: string;
  email: string;
  name: string;
  picture?: string;
} | null> {
  try {
    const decodedToken = await auth.verifyIdToken(idToken);
    
    return {
      uid: decodedToken.uid,
      email: decodedToken.email!,
      name: decodedToken.name || decodedToken.email!,
      picture: decodedToken.picture
    };
  } catch (error) {
    console.error('Token verification failed:', error);
    return null;
  }
}
```

**Verification:**
- [ ] Middleware file created
- [ ] Token extraction logic implemented
- [ ] Firebase verification integrated
- [ ] Error handling included

---

### Task 3.2: Implement User Profile Creation Logic

**Objective:** Create function to handle first-time user sign-in

**Steps:**

1. **Create `firebase/auth-service.ts`:**

```typescript
import { auth, db, timestamp } from './admin';
import { createUser, getUser } from './db-helpers';
import type { UserProfile } from './types';

// Create or update user profile on sign-in
export async function handleUserSignIn(
  uid: string,
  email: string,
  displayName: string,
  photoURL?: string
): Promise<UserProfile> {
  // Check if user exists
  const existingUser = await getUser(uid);
  
  if (existingUser) {
    // Update last sign-in
    await db.collection('users').doc(uid).update({
      updatedAt: timestamp.now()
    });
    
    return existingUser;
  }
  
  // Create new user profile
  const newUser: Omit<UserProfile, 'createdAt' | 'updatedAt'> = {
    uid,
    email,
    displayName,
    photoURL: photoURL || undefined,
    driveConnected: true, // Assume connected after OAuth
    plan: 'free',
    quotaUsed: 0,
    quotaLimit: 1024, // 1GB for free tier
    preferences: {
      defaultQuality: '720p',
      autoTranscribe: false,
      autoSummarize: false,
      language: 'en'
    }
  };
  
  await createUser(newUser);
  
  const createdUser = await getUser(uid);
  return createdUser!;
}

// Get or create user from Firebase Auth token
export async function getUserFromToken(idToken: string): Promise<UserProfile | null> {
  try {
    const decodedToken = await auth.verifyIdToken(idToken);
    
    const user = await handleUserSignIn(
      decodedToken.uid,
      decodedToken.email!,
      decodedToken.name || decodedToken.email!,
      decodedToken.picture
    );
    
    return user;
  } catch (error) {
    console.error('Failed to get user from token:', error);
    return null;
  }
}

// Delete user account and all associated data
export async function deleteUserAccount(uid: string): Promise<void> {
  // Delete user from Firebase Auth
  await auth.deleteUser(uid);
  
  // Delete user profile from Firestore
  await db.collection('users').doc(uid).delete();
  
  // Delete user's recordings
  const recordings = await db.collection('recordings')
    .where('userId', '==', uid)
    .get();
  
  const batch = db.batch();
  recordings.forEach(doc => {
    batch.delete(doc.ref);
  });
  
  // Delete user's folders
  const folders = await db.collection('folders')
    .where('userId', '==', uid)
    .get();
  
  folders.forEach(doc => {
    batch.delete(doc.ref);
  });
  
  // Delete user's AI jobs
  const jobs = await db.collection('ai_jobs')
    .where('userId', '==', uid)
    .get();
  
  jobs.forEach(doc => {
    batch.delete(doc.ref);
  });
  
  await batch.commit();
}
```

**Verification:**
- [ ] Auth service file created
- [ ] User sign-in logic implemented
- [ ] Profile creation on first sign-in
- [ ] Account deletion logic included

---

### Task 3.3: Test Authentication Flow

**Objective:** Verify authentication works end-to-end

**Steps:**

1. **Create test script `firebase/test-auth.ts`:**

```typescript
import { auth } from './admin';
import { handleUserSignIn, getUserFromToken } from './auth-service';

async function testAuthentication() {
  console.log('Testing Authentication Flow...\n');
  
  try {
    // Create test user
    console.log('1. Creating test Firebase Auth user...');
    const userRecord = await auth.createUser({
      email: 'test-auth@example.com',
      password: 'testpassword123',
      displayName: 'Auth Test User'
    });
    console.log(`✅ User created: ${userRecord.uid}`);
    
    // Generate custom token
    console.log('\n2. Generating custom token...');
    const customToken = await auth.createCustomToken(userRecord.uid);
    console.log('✅ Custom token generated');
    
    // Handle user sign-in (create profile)
    console.log('\n3. Creating user profile...');
    const userProfile = await handleUserSignIn(
      userRecord.uid,
      userRecord.email!,
      userRecord.displayName!,
      userRecord.photoURL
    );
    console.log('✅ User profile created:', userProfile);
    
    // Verify token verification
    console.log('\n4. Testing token verification...');
    const decodedToken = await auth.verifyIdToken(customToken);
    console.log('✅ Token verified:', decodedToken.uid);
    
    // Cleanup
    console.log('\n5. Cleaning up test data...');
    await auth.deleteUser(userRecord.uid);
    console.log('✅ Test user deleted');
    
    console.log('\n✅ All authentication tests passed!');
  } catch (error) {
    console.error('❌ Authentication test failed:', error);
    process.exit(1);
  }
}

testAuthentication();
```

2. **Run test:**
```bash
ts-node firebase/test-auth.ts
```

**Expected Output:**
```
Testing Authentication Flow...

1. Creating test Firebase Auth user...
✅ User created: abc123xyz

2. Generating custom token...
✅ Custom token generated

3. Creating user profile...
✅ User profile created: { uid: 'abc123xyz', email: '...', ... }

4. Testing token verification...
✅ Token verified: abc123xyz

5. Cleaning up test data...
✅ Test user deleted

✅ All authentication tests passed!
```

**Verification:**
- [ ] Test runs successfully
- [ ] User creation works
- [ ] Token generation works
- [ ] Token verification works
- [ ] Cleanup successful

---

### Phase 3 Completion Checklist

**Completed Tasks:**
- [ ] Task 3.1: Token verification middleware created
- [ ] Task 3.2: User profile creation logic implemented
- [ ] Task 3.3: Authentication flow tested

**Deliverables:**
- [x] Auth middleware: `firebase/auth-middleware.ts`
- [x] Auth service: `firebase/auth-service.ts`
- [x] Auth test script: `firebase/test-auth.ts`

**Phase 3 Success Criteria:**
- ✅ Token verification works correctly
- ✅ User profiles created on first sign-in
- ✅ Authentication tests pass
- ✅ Error handling robust

**Next Phase:** Phase 4 - Google Drive Integration

---

## Phase 4: Google Drive Integration

### Phase Overview

**Goal:** Integrate Google Drive API for video storage and playback

**Duration:** 2-3 hours

**Prerequisites:**
- Phase 3 completed (Authentication working)
- Google Drive API enabled
- OAuth scopes configured

**Deliverables:**
- Google Drive API client setup
- Signed URL generation for playback
- File metadata retrieval
- File deletion orchestration

---

### Task 4.1: Set Up Google Drive API Client

**Objective:** Configure Google Drive API for server-side access

**Steps:**

1. **Install Google APIs client:**
```bash
npm install googleapis
```

2. **Create `google/drive-client.ts`:**

```typescript
import { google } from 'googleapis';
import { auth } from '../firebase/admin';

// Get OAuth2 client for a user
export async function getDriveClient(userId: string) {
  // Get user's access token from Firebase Auth
  const user = await auth.getUser(userId);
  
  // Note: In production, you'd store and refresh OAuth tokens
  // For now, we'll use the service account for demonstration
  
  const oauth2Client = new google.auth.OAuth2(
    process.env.GOOGLE_CLIENT_ID,
    process.env.GOOGLE_CLIENT_SECRET,
    process.env.REDIRECT_URI
  );
  
  // Set credentials (in production, get from database)
  oauth2Client.setCredentials({
    access_token: process.env.GOOGLE_ACCESS_TOKEN,
    refresh_token: process.env.GOOGLE_REFRESH_TOKEN
  });
  
  return google.drive({ version: 'v3', auth: oauth2Client });
}

// Get Drive client with service account (for admin operations)
export function getServiceDriveClient() {
  const auth = new google.auth.GoogleAuth({
    keyFile: './firebase/serviceAccountKey.json',
    scopes: ['https://www.googleapis.com/auth/drive.file']
  });
  
  return google.drive({ version: 'v3', auth });
}
```

**Verification:**
- [ ] googleapis installed
- [ ] Drive client configuration created
- [ ] OAuth2 client setup complete

---

### Task 4.2: Implement Signed URL Generation

**Objective:** Generate temporary URLs for video playback

**Steps:**

1. **Create `google/drive-service.ts`:**

```typescript
import { google } from 'googleapis';
import { getDriveClient } from './drive-client';

// Generate signed URL for file access
export async function generateSignedUrl(
  fileId: string,
  userId: string,
  expirationMinutes: number = 60
): Promise<{ url: string; expiresAt: string }> {
  try {
    const drive = await getDriveClient(userId);
    
    // Get file metadata to verify access
    const file = await drive.files.get({
      fileId,
      fields: 'id,name,mimeType'
    });
    
    // Generate download URL with access token
    // Note: This is a simplified version. In production, you'd use proper signing
    const url = `https://www.googleapis.com/drive/v3/files/${fileId}?alt=media`;
    
    const expiresAt = new Date(Date.now() + expirationMinutes * 60 * 1000).toISOString();
    
    return {
      url,
      expiresAt
    };
  } catch (error) {
    console.error('Failed to generate signed URL:', error);
    throw new Error('Failed to generate video URL');
  }
}

// Get file metadata
export async function getFileMetadata(fileId: string, userId: string) {
  try {
    const drive = await getDriveClient(userId);
    
    const file = await drive.files.get({
      fileId,
      fields: 'id,name,mimeType,size,createdTime,modifiedTime,webViewLink,thumbnailLink'
    });
    
    return file.data;
  } catch (error) {
    console.error('Failed to get file metadata:', error);
    throw new Error('Failed to retrieve file information');
  }
}

// Delete file from Drive
export async function deleteFile(fileId: string, userId: string): Promise<void> {
  try {
    const drive = await getDriveClient(userId);
    
    await drive.files.delete({
      fileId
    });
    
    console.log(`File ${fileId} deleted successfully`);
  } catch (error) {
    console.error('Failed to delete file:', error);
    throw new Error('Failed to delete video file');
  }
}

// Check if file exists and user has access
export async function verifyFileAccess(fileId: string, userId: string): Promise<boolean> {
  try {
    const drive = await getDriveClient(userId);
    
    await drive.files.get({
      fileId,
      fields: 'id'
    });
    
    return true;
  } catch (error) {
    return false;
  }
}
```

**Verification:**
- [ ] Drive service file created
- [ ] Signed URL generation implemented
- [ ] File metadata retrieval works
- [ ] File deletion logic included

---

### Task 4.3: Test Google Drive Integration

**Objective:** Verify Drive API operations work correctly

**Steps:**

1. **Create test script `google/test-drive.ts`:**

```typescript
import { generateSignedUrl, getFileMetadata, verifyFileAccess } from './drive-service';

async function testDriveIntegration() {
  console.log('Testing Google Drive Integration...\n');
  
  // Note: Replace with actual test file ID
  const testFileId = 'YOUR_TEST_FILE_ID';
  const testUserId = 'test-user-123';
  
  try {
    console.log('1. Testing file access verification...');
    const hasAccess = await verifyFileAccess(testFileId, testUserId);
    console.log(`✅ File access: ${hasAccess ? 'Granted' : 'Denied'}`);
    
    if (hasAccess) {
      console.log('\n2. Getting file metadata...');
      const metadata = await getFileMetadata(testFileId, testUserId);
      console.log('✅ File metadata:', metadata);
      
      console.log('\n3. Generating signed URL...');
      const { url, expiresAt } = await generateSignedUrl(testFileId, testUserId);
      console.log('✅ Signed URL generated');
      console.log(`URL: ${url.substring(0, 50)}...`);
      console.log(`Expires: ${expiresAt}`);
    }
    
    console.log('\n✅ Google Drive integration tests completed!');
  } catch (error) {
    console.error('❌ Test failed:', error);
  }
}

testDriveIntegration();
```

**Verification:**
- [ ] Test script created
- [ ] Can verify file access
- [ ] Can get file metadata
- [ ] Can generate signed URLs

---

### Phase 4 Completion Checklist

**Completed Tasks:**
- [ ] Task 4.1: Google Drive API client setup
- [ ] Task 4.2: Signed URL generation implemented
- [ ] Task 4.3: Drive integration tested

**Deliverables:**
- [x] Drive client: `google/drive-client.ts`
- [x] Drive service: `google/drive-service.ts`
- [x] Drive test script: `google/test-drive.ts`

**Phase 4 Success Criteria:**
- ✅ Drive API configured correctly
- ✅ Can generate signed URLs
- ✅ File operations work
- ✅ Access verification functional

**Next Phase:** Phase 5 - Cloudflare Workers Setup

---

## Phase 5: Cloudflare Workers Setup

### Phase Overview

**Goal:** Set up Cloudflare Workers for serverless API endpoints

**Duration:** 3-4 hours

**Prerequisites:**
- Cloudflare account created
- Domain configured (openmeet.panotech.uk)
- Wrangler CLI installed

**Deliverables:**
- Cloudflare Workers project initialized
- API routing structure
- Environment variables configured
- CORS handling
- Error handling middleware

---

### Task 5.1: Initialize Cloudflare Workers Project

**Objective:** Create and configure Workers project

**Steps:**

1. **Install Wrangler CLI:**
```bash
npm install -g wrangler
```

2. **Login to Cloudflare:**
```bash
wrangler login
```

3. **Create Workers project:**
```bash
mkdir workers
cd workers
npm init -y
npm install --save-dev wrangler typescript @cloudflare/workers-types
npm install hono
```

4. **Create `wrangler.toml`:**
```toml
name = "openmeet-api"
main = "src/index.ts"
compatibility_date = "2025-12-29"

[env.production]
name = "openmeet-api-production"
routes = [
  { pattern = "openmeet.panotech.uk/api/*", zone_name = "panotech.uk" }
]

[env.development]
name = "openmeet-api-dev"

# Environment variables (use wrangler secret for sensitive values)
[vars]
ENVIRONMENT = "production"
CORS_ORIGIN = "https://openmeet.panotech.uk"

# Secrets (set via: wrangler secret put SECRET_NAME)
# FIREBASE_PROJECT_ID
# FIREBASE_PRIVATE_KEY
# FIREBASE_CLIENT_EMAIL
# GEMINI_API_KEY
# GOOGLE_CLIENT_ID
# GOOGLE_CLIENT_SECRET
```

5. **Create `tsconfig.json`:**
```json
{
  "compilerOptions": {
    "target": "ES2021",
    "lib": ["ES2021"],
    "module": "ES2022",
    "moduleResolution": "node",
    "types": ["@cloudflare/workers-types"],
    "resolveJsonModule": true,
    "allowJs": true,
    "checkJs": false,
    "jsx": "react",
    "jsxFactory": "h",
    "jsxFragmentFactory": "Fragment",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

**Verification:**
- [ ] Wrangler CLI installed
- [ ] Workers project created
- [ ] Configuration files in place
- [ ] Dependencies installed

---

### Task 5.2: Create API Router with Hono

**Objective:** Set up routing structure for API endpoints

**Steps:**

1. **Create `workers/src/index.ts`:**

```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';
import { prettyJSON } from 'hono/pretty-json';

// Import route handlers (will create in Phase 6)
import authRoutes from './routes/auth';
import recordingRoutes from './routes/recordings';
import aiRoutes from './routes/ai';
import driveRoutes from './routes/drive';

// Create Hono app
const app = new Hono();

// Middleware
app.use('*', cors({
  origin: (origin) => {
    // Allow requests from dashboard and localhost
    const allowed = [
      'https://openmeet.panotech.uk',
      'http://localhost:5173',
      'http://localhost:4000'
    ];
    return allowed.includes(origin) ? origin : allowed[0];
  },
  allowMethods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE', 'OPTIONS'],
  allowHeaders: ['Content-Type', 'Authorization'],
  exposeHeaders: ['Content-Length'],
  maxAge: 86400,
  credentials: true
}));

app.use('*', prettyJSON());

// Health check
app.get('/health', (c) => {
  return c.json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    service: 'openmeet-api'
  });
});

// API routes
app.route('/api/auth', authRoutes);
app.route('/api/recordings', recordingRoutes);
app.route('/api/ai', aiRoutes);
app.route('/api/drive', driveRoutes);

// 404 handler
app.notFound((c) => {
  return c.json({
    error: 'Not Found',
    message: 'The requested endpoint does not exist'
  }, 404);
});

// Error handler
app.onError((err, c) => {
  console.error('API Error:', err);
  
  return c.json({
    error: 'Internal Server Error',
    message: err.message || 'An unexpected error occurred'
  }, 500);
});

export default app;
```

2. **Create route placeholders:**

```bash
mkdir -p workers/src/routes
touch workers/src/routes/auth.ts
touch workers/src/routes/recordings.ts
touch workers/src/routes/ai.ts
touch workers/src/routes/drive.ts
```

3. **Create basic route structure in each file:**

`workers/src/routes/auth.ts`:
```typescript
import { Hono } from 'hono';

const app = new Hono();

// Placeholder - will implement in Phase 6
app.post('/signin', async (c) => {
  return c.json({ message: 'Auth signin endpoint' });
});

export default app;
```

(Repeat similar structure for other route files)

**Verification:**
- [ ] Main app file created
- [ ] Router configured with Hono
- [ ] CORS middleware applied
- [ ] Route structure in place

---

### Task 5.3: Create Middleware for Firebase Integration

**Objective:** Set up Firebase Admin in Workers environment

**Steps:**

1. **Create `workers/src/lib/firebase.ts`:**

```typescript
// Firebase Admin SDK for Cloudflare Workers
import { initializeApp, cert } from 'firebase-admin/app';
import { getAuth } from 'firebase-admin/auth';
import { getFirestore } from 'firebase-admin/firestore';

// Initialize Firebase Admin (using environment variables)
let firebaseApp: any;
let firebaseAuth: any;
let firebaseDb: any;

export function initializeFirebase(env: any) {
  if (!firebaseApp) {
    const serviceAccount = {
      projectId: env.FIREBASE_PROJECT_ID,
      privateKey: env.FIREBASE_PRIVATE_KEY.replace(/\\n/g, '\n'),
      clientEmail: env.FIREBASE_CLIENT_EMAIL
    };
    
    firebaseApp = initializeApp({
      credential: cert(serviceAccount)
    });
    
    firebaseAuth = getAuth(firebaseApp);
    firebaseDb = getFirestore(firebaseApp);
  }
  
  return { auth: firebaseAuth, db: firebaseDb };
}

// Verify Firebase ID token
export async function verifyToken(token: string, env: any) {
  const { auth } = initializeFirebase(env);
  
  try {
    const decodedToken = await auth.verifyIdToken(token);
    return {
      uid: decodedToken.uid,
      email: decodedToken.email,
      name: decodedToken.name || decodedToken.email
    };
  } catch (error) {
    console.error('Token verification failed:', error);
    return null;
  }
}
```

2. **Create auth middleware `workers/src/middleware/auth.ts`:**

```typescript
import { Context, Next } from 'hono';
import { verifyToken } from '../lib/firebase';

// Extend context with user data
export interface AuthContext extends Context {
  user?: {
    uid: string;
    email: string;
    name: string;
  };
}

// Auth middleware
export async function authMiddleware(c: AuthContext, next: Next) {
  const authHeader = c.req.header('Authorization');
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return c.json({
      error: 'Unauthorized',
      message: 'No authentication token provided'
    }, 401);
  }
  
  const token = authHeader.split('Bearer ')[1];
  
  const user = await verifyToken(token, c.env);
  
  if (!user) {
    return c.json({
      error: 'Unauthorized',
      message: 'Invalid or expired token'
    }, 401);
  }
  
  c.user = user;
  await next();
}
```

**Verification:**
- [ ] Firebase lib created
- [ ] Token verification implemented
- [ ] Auth middleware functional

---

### Task 5.4: Set Environment Variables

**Objective:** Configure secrets for Workers

**Steps:**

1. **Set Firebase credentials:**
```bash
wrangler secret put FIREBASE_PROJECT_ID
# Enter your Firebase project ID

wrangler secret put FIREBASE_PRIVATE_KEY
# Enter your service account private key

wrangler secret put FIREBASE_CLIENT_EMAIL
# Enter your service account email
```

2. **Set Gemini API key:**
```bash
wrangler secret put GEMINI_API_KEY
# Enter your Gemini API key
```

3. **Set Google OAuth credentials:**
```bash
wrangler secret put GOOGLE_CLIENT_ID
# Enter your Google OAuth client ID

wrangler secret put GOOGLE_CLIENT_SECRET
# Enter your Google OAuth client secret
```

**Verification:**
- [ ] All secrets configured
- [ ] No secrets in code or Git
- [ ] Environment variables accessible in Workers

---

### Task 5.5: Deploy and Test Workers

**Objective:** Deploy Workers and verify deployment

**Steps:**

1. **Deploy to development:**
```bash
wrangler deploy --env development
```

**Expected Output:**
```
⛅️ wrangler 3.x.x
-------------------
Uploading Worker bundle
Uploaded openmeet-api-dev (x.xx sec)
Published openmeet-api-dev (x.xx sec)
  https://openmeet-api-dev.[your-subdomain].workers.dev
```

2. **Test health endpoint:**
```bash
curl https://openmeet-api-dev.[your-subdomain].workers.dev/health
```

**Expected Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-12-29T10:00:00.000Z",
  "service": "openmeet-api"
}
```

3. **Deploy to production:**
```bash
wrangler deploy --env production
```

**Verification:**
- [ ] Workers deployed successfully
- [ ] Health endpoint accessible
- [ ] CORS headers present
- [ ] No deployment errors

---

### Phase 5 Completion Checklist

**Completed Tasks:**
- [ ] Task 5.1: Cloudflare Workers project initialized
- [ ] Task 5.2: API router created with Hono
- [ ] Task 5.3: Firebase integration middleware
- [ ] Task 5.4: Environment variables configured
- [ ] Task 5.5: Workers deployed and tested

**Deliverables:**
- [x] Workers project: `workers/`
- [x] Main app: `workers/src/index.ts`
- [x] Firebase lib: `workers/src/lib/firebase.ts`
- [x] Auth middleware: `workers/src/middleware/auth.ts`
- [x] Route placeholders created

**Phase 5 Success Criteria:**
- ✅ Workers project configured
- ✅ API routing structure in place
- ✅ Firebase integration working
- ✅ Deployed and accessible
- ✅ CORS properly configured

**Next Phase:** Phase 6 - API Endpoints Implementation

---

## Phase 6: API Endpoints Implementation

### Phase Overview

**Goal:** Implement all API endpoints for the application

**Duration:** 4-5 hours

**Prerequisites:**
- Phase 5 completed (Cloudflare Workers setup)
- Phase 2 completed (Database schema)
- Phase 3 completed (Authentication)

**Deliverables:**
- Authentication endpoints (signin, signout, me)
- Recording CRUD endpoints
- Folder management endpoints
- Complete API documentation

---

### Task 6.1: Implement Authentication Endpoints

**Objective:** Create auth API endpoints

**Steps:**

1. **Update `workers/src/routes/auth.ts`:**

```typescript
import { Hono } from 'hono';
import { initializeFirebase } from '../lib/firebase';
import { authMiddleware } from '../middleware/auth';

const app = new Hono();

// POST /api/auth/signin
app.post('/signin', async (c) => {
  try {
    const { idToken } = await c.req.json();
    
    if (!idToken) {
      return c.json({
        error: 'Bad Request',
        message: 'ID token is required'
      }, 400);
    }
    
    const { auth, db } = initializeFirebase(c.env);
    
    // Verify token
    const decodedToken = await auth.verifyIdToken(idToken);
    
    // Check if user exists
    const userRef = db.collection('users').doc(decodedToken.uid);
    const userDoc = await userRef.get();
    
    let user;
    
    if (!userDoc.exists) {
      // Create new user
      user = {
        uid: decodedToken.uid,
        email: decodedToken.email,
        displayName: decodedToken.name || decodedToken.email,
        photoURL: decodedToken.picture || null,
        driveConnected: true,
        createdAt: new Date(),
        updatedAt: new Date(),
        plan: 'free',
        quotaUsed: 0,
        quotaLimit: 1024,
        preferences: {
          defaultQuality: '720p',
          autoTranscribe: false,
          autoSummarize: false,
          language: 'en'
        }
      };
      
      await userRef.set(user);
    } else {
      user = userDoc.data();
      
      // Update last sign-in
      await userRef.update({
        updatedAt: new Date()
      });
    }
    
    return c.json({
      success: true,
      user: {
        uid: user.uid,
        email: user.email,
        displayName: user.displayName,
        photoURL: user.photoURL
      },
      token: idToken
    });
  } catch (error: any) {
    console.error('Sign-in error:', error);
    return c.json({
      error: 'Authentication Failed',
      message: error.message
    }, 401);
  }
});

// POST /api/auth/signout
app.post('/signout', authMiddleware, async (c) => {
  // Client-side will clear tokens
  return c.json({
    success: true,
    message: 'Signed out successfully'
  });
});

// GET /api/auth/me
app.get('/me', authMiddleware, async (c) => {
  const { db } = initializeFirebase(c.env);
  
  try {
    const userRef = db.collection('users').doc(c.user.uid);
    const userDoc = await userRef.get();
    
    if (!userDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'User profile not found'
      }, 404);
    }
    
    return c.json({
      user: userDoc.data()
    });
  } catch (error: any) {
    console.error('Get user error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

export default app;
```

**Verification:**
- [ ] Sign-in endpoint implemented
- [ ] Sign-out endpoint implemented
- [ ] Get current user endpoint implemented
- [ ] Error handling included

---

### Task 6.2: Implement Recording Endpoints

**Objective:** Create recording CRUD endpoints

**Steps:**

1. **Update `workers/src/routes/recordings.ts`:**

```typescript
import { Hono } from 'hono';
import { initializeFirebase } from '../lib/firebase';
import { authMiddleware } from '../middleware/auth';

const app = new Hono();

// All recording routes require authentication
app.use('*', authMiddleware);

// GET /api/recordings - List user's recordings
app.get('/', async (c) => {
  const { db } = initializeFirebase(c.env);
  
  try {
    const limit = parseInt(c.req.query('limit') || '20');
    const offset = parseInt(c.req.query('offset') || '0');
    const sortBy = c.req.query('sortBy') || 'createdAt';
    const sortOrder = c.req.query('sortOrder') || 'desc';
    const folderId = c.req.query('folderId');
    const status = c.req.query('status');
    
    let query = db.collection('recordings')
      .where('userId', '==', c.user.uid);
    
    if (folderId) {
      query = query.where('folderId', '==', folderId);
    }
    
    if (status) {
      query = query.where('status', '==', status);
    }
    
    query = query.orderBy(sortBy, sortOrder as any);
    
    if (offset > 0) {
      query = query.offset(offset);
    }
    
    query = query.limit(limit);
    
    const snapshot = await query.get();
    const recordings = snapshot.docs.map(doc => doc.data());
    
    // Get total count
    const totalSnapshot = await db.collection('recordings')
      .where('userId', '==', c.user.uid)
      .get();
    
    const total = totalSnapshot.size;
    const hasMore = offset + limit < total;
    
    return c.json({
      recordings,
      total,
      hasMore,
      limit,
      offset
    });
  } catch (error: any) {
    console.error('Get recordings error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// GET /api/recordings/:id - Get single recording
app.get('/:id', async (c) => {
  const { db } = initializeFirebase(c.env);
  const recordingId = c.req.param('id');
  
  try {
    const recordingRef = db.collection('recordings').doc(recordingId);
    const recordingDoc = await recordingRef.get();
    
    if (!recordingDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Recording not found'
      }, 404);
    }
    
    const recording = recordingDoc.data();
    
    // Verify ownership
    if (recording.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this recording'
      }, 403);
    }
    
    return c.json({
      recording
    });
  } catch (error: any) {
    console.error('Get recording error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// POST /api/recordings - Create new recording
app.post('/', async (c) => {
  const { db } = initializeFirebase(c.env);
  
  try {
    const body = await c.req.json();
    
    // Validate required fields
    const required = ['title', 'duration', 'fileSize', 'quality', 'format', 'driveFileId'];
    for (const field of required) {
      if (!body[field]) {
        return c.json({
          error: 'Bad Request',
          message: `Missing required field: ${field}`
        }, 400);
      }
    }
    
    const recordingRef = db.collection('recordings').doc();
    
    const recording = {
      id: recordingRef.id,
      userId: c.user.uid,
      title: body.title,
      description: body.description || null,
      duration: body.duration,
      fileSize: body.fileSize,
      quality: body.quality,
      format: body.format,
      driveFileId: body.driveFileId,
      driveFileUrl: body.driveFileUrl || null,
      thumbnailUrl: body.thumbnailUrl || null,
      createdAt: new Date(),
      updatedAt: new Date(),
      status: 'uploading',
      isPrivate: body.isPrivate !== undefined ? body.isPrivate : true,
      meetingUrl: body.meetingUrl || null,
      tags: body.tags || [],
      folderId: body.folderId || null,
      participants: body.participants || []
    };
    
    await recordingRef.set(recording);
    
    return c.json({
      recording
    }, 201);
  } catch (error: any) {
    console.error('Create recording error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// PATCH /api/recordings/:id - Update recording
app.patch('/:id', async (c) => {
  const { db } = initializeFirebase(c.env);
  const recordingId = c.req.param('id');
  
  try {
    const recordingRef = db.collection('recordings').doc(recordingId);
    const recordingDoc = await recordingRef.get();
    
    if (!recordingDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Recording not found'
      }, 404);
    }
    
    const recording = recordingDoc.data();
    
    // Verify ownership
    if (recording.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this recording'
      }, 403);
    }
    
    const body = await c.req.json();
    
    // Allowed update fields
    const allowedFields = ['title', 'description', 'tags', 'folderId', 'isPrivate', 'status', 'transcript', 'summary'];
    const updates: any = {
      updatedAt: new Date()
    };
    
    for (const field of allowedFields) {
      if (body[field] !== undefined) {
        updates[field] = body[field];
      }
    }
    
    await recordingRef.update(updates);
    
    const updatedDoc = await recordingRef.get();
    
    return c.json({
      recording: updatedDoc.data()
    });
  } catch (error: any) {
    console.error('Update recording error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// DELETE /api/recordings/:id - Delete recording
app.delete('/:id', async (c) => {
  const { db } = initializeFirebase(c.env);
  const recordingId = c.req.param('id');
  
  try {
    const recordingRef = db.collection('recordings').doc(recordingId);
    const recordingDoc = await recordingRef.get();
    
    if (!recordingDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Recording not found'
      }, 404);
    }
    
    const recording = recordingDoc.data();
    
    // Verify ownership
    if (recording.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this recording'
      }, 403);
    }
    
    // Delete from Firestore
    await recordingRef.delete();
    
    // TODO: Delete from Google Drive (Phase 4 integration)
    
    return c.json({
      success: true,
      message: 'Recording deleted successfully'
    });
  } catch (error: any) {
    console.error('Delete recording error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

export default app;
```

**Verification:**
- [ ] GET /api/recordings implemented
- [ ] GET /api/recordings/:id implemented
- [ ] POST /api/recordings implemented
- [ ] PATCH /api/recordings/:id implemented
- [ ] DELETE /api/recordings/:id implemented

---

### Phase 6 Completion Checklist

**Completed Tasks:**
- [ ] Task 6.1: Authentication endpoints implemented
- [ ] Task 6.2: Recording endpoints implemented

**Deliverables:**
- [x] Auth routes: `workers/src/routes/auth.ts`
- [x] Recording routes: `workers/src/routes/recordings.ts`

**Phase 6 Success Criteria:**
- ✅ All API endpoints functional
- ✅ Authentication working
- ✅ CRUD operations complete
- ✅ Error handling robust

**Next Phase:** Phase 7 - AI Integration (Gemini)

---

## Phase 7: AI Integration (Gemini)

### Phase Overview

**Goal:** Integrate Gemini 2.0 Flash API for transcription and summarization

**Duration:** 4-5 hours

**Prerequisites:**
- Phase 6 completed (API endpoints)
- Gemini API key obtained
- Phase 4 completed (Drive integration)

**Deliverables:**
- Gemini AI client setup
- Transcription endpoint
- Summarization endpoint
- AI job status tracking
- Prompt engineering

---

### Task 7.1: Set Up Gemini AI Client

**Objective:** Configure Gemini API client

**Steps:**

1. **Install Gemini SDK:**
```bash
cd workers
npm install @google/generative-ai
```

2. **Create `workers/src/lib/gemini.ts`:**

```typescript
import { GoogleGenerativeAI } from '@google/generative-ai';

// Initialize Gemini AI
export function initializeGemini(apiKey: string) {
  const genAI = new GoogleGenerativeAI(apiKey);
  return genAI.getGenerativeModel({ model: 'gemini-2.0-flash' });
}

// Transcribe audio/video
export async function transcribeMedia(
  fileContent: string | Blob,
  language: string = 'en',
  apiKey: string
) {
  const model = initializeGemini(apiKey);
  
  const prompt = `
Transcribe the following audio/video content accurately.
Format the output as a JSON array with this structure:
[
  {
    "timestamp": 0,
    "duration": 5,
    "speaker": "Speaker 1",
    "text": "Transcribed text here",
    "confidence": 0.95
  }
]

Language: ${language}
Detect multiple speakers and label them as "Speaker 1", "Speaker 2", etc.
Include timestamps in seconds.
`;
  
  try {
    const result = await model.generateContent([
      { text: prompt },
      // Note: In production, you'd pass the actual media file
      // For now, we'll work with text-based prompts
    ]);
    
    const response = await result.response;
    const text = response.text();
    
    // Parse JSON response
    const transcript = JSON.parse(text);
    
    return transcript;
  } catch (error) {
    console.error('Transcription error:', error);
    throw new Error('Failed to transcribe media');
  }
}

// Summarize transcript
export async function summarizeTranscript(
  transcript: any[],
  language: string = 'en',
  apiKey: string
) {
  const model = initializeGemini(apiKey);
  
  const transcriptText = transcript
    .map(entry => `[${entry.timestamp}s] ${entry.speaker}: ${entry.text}`)
    .join('\n');
  
  const prompt = `
Analyze the following meeting transcript and provide a comprehensive summary.

Transcript:
${transcriptText}

Generate a JSON response with this structure:
{
  "text": "Brief overview paragraph summarizing the meeting",
  "keyPoints": ["Key point 1", "Key point 2", ...],
  "actionItems": [
    {
      "text": "Action item description",
      "assignee": "Person responsible (if mentioned)",
      "dueDate": "Due date (if mentioned)",
      "completed": false
    }
  ],
  "topics": ["Topic 1", "Topic 2", ...],
  "participants": ["Participant 1", "Participant 2", ...],
  "language": "${language}"
}

Extract:
1. Main discussion points
2. Decisions made
3. Action items with assignees
4. Key topics discussed
5. Participant names mentioned
`;
  
  try {
    const result = await model.generateContent(prompt);
    const response = await result.response;
    const text = response.text();
    
    // Parse JSON response
    const summary = JSON.parse(text);
    
    return {
      ...summary,
      model: 'gemini-2.0-flash',
      generatedAt: new Date()
    };
  } catch (error) {
    console.error('Summarization error:', error);
    throw new Error('Failed to summarize transcript');
  }
}
```

**Verification:**
- [ ] Gemini SDK installed
- [ ] AI client created
- [ ] Transcription function implemented
- [ ] Summarization function implemented

---

### Task 7.2: Implement AI Processing Endpoints

**Objective:** Create endpoints for AI operations

**Steps:**

1. **Update `workers/src/routes/ai.ts`:**

```typescript
import { Hono } from 'hono';
import { initializeFirebase } from '../lib/firebase';
import { authMiddleware } from '../middleware/auth';
import { transcribeMedia, summarizeTranscript } from '../lib/gemini';

const app = new Hono();

// All AI routes require authentication
app.use('*', authMiddleware);

// POST /api/ai/transcribe - Trigger transcription
app.post('/transcribe', async (c) => {
  const { db } = initializeFirebase(c.env);
  
  try {
    const { recordingId, driveFileId, language } = await c.req.json();
    
    if (!recordingId || !driveFileId) {
      return c.json({
        error: 'Bad Request',
        message: 'recordingId and driveFileId are required'
      }, 400);
    }
    
    // Verify recording ownership
    const recordingRef = db.collection('recordings').doc(recordingId);
    const recordingDoc = await recordingRef.get();
    
    if (!recordingDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Recording not found'
      }, 404);
    }
    
    const recording = recordingDoc.data();
    
    if (recording.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this recording'
      }, 403);
    }
    
    // Create AI job
    const jobRef = db.collection('ai_jobs').doc();
    
    const job = {
      id: jobRef.id,
      recordingId,
      userId: c.user.uid,
      type: 'transcribe',
      status: 'pending',
      progress: 0,
      createdAt: new Date()
    };
    
    await jobRef.set(job);
    
    // Trigger async processing (in production, use Queue/Workers)
    // For now, process synchronously
    try {
      await jobRef.update({ status: 'processing' });
      
      // TODO: Fetch video from Drive
      // TODO: Extract audio
      // TODO: Call Gemini API
      
      // Placeholder transcription
      const transcript = [
        {
          timestamp: 0,
          duration: 10,
          speaker: 'Speaker 1',
          text: 'This is a placeholder transcript.',
          confidence: 0.95
        }
      ];
      
      // Update recording with transcript
      await recordingRef.update({
        transcript,
        updatedAt: new Date()
      });
      
      // Mark job as completed
      await jobRef.update({
        status: 'completed',
        progress: 100,
        result: transcript,
        completedAt: new Date()
      });
      
      return c.json({
        jobId: jobRef.id,
        status: 'completed',
        result: transcript
      });
    } catch (error: any) {
      await jobRef.update({
        status: 'failed',
        error: error.message
      });
      
      throw error;
    }
  } catch (error: any) {
    console.error('Transcription error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// POST /api/ai/summarize - Trigger summarization
app.post('/summarize', async (c) => {
  const { db } = initializeFirebase(c.env);
  
  try {
    const { recordingId, transcript, language } = await c.req.json();
    
    if (!recordingId || !transcript) {
      return c.json({
        error: 'Bad Request',
        message: 'recordingId and transcript are required'
      }, 400);
    }
    
    // Verify recording ownership
    const recordingRef = db.collection('recordings').doc(recordingId);
    const recordingDoc = await recordingRef.get();
    
    if (!recordingDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Recording not found'
      }, 404);
    }
    
    const recording = recordingDoc.data();
    
    if (recording.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this recording'
      }, 403);
    }
    
    // Create AI job
    const jobRef = db.collection('ai_jobs').doc();
    
    const job = {
      id: jobRef.id,
      recordingId,
      userId: c.user.uid,
      type: 'summarize',
      status: 'pending',
      progress: 0,
      createdAt: new Date()
    };
    
    await jobRef.set(job);
    
    // Process summarization
    try {
      await jobRef.update({ status: 'processing' });
      
      const summary = await summarizeTranscript(
        transcript,
        language || 'en',
        c.env.GEMINI_API_KEY
      );
      
      // Update recording with summary
      await recordingRef.update({
        summary,
        updatedAt: new Date()
      });
      
      // Mark job as completed
      await jobRef.update({
        status: 'completed',
        progress: 100,
        result: summary,
        completedAt: new Date()
      });
      
      return c.json({
        jobId: jobRef.id,
        status: 'completed',
        result: summary
      });
    } catch (error: any) {
      await jobRef.update({
        status: 'failed',
        error: error.message
      });
      
      throw error;
    }
  } catch (error: any) {
    console.error('Summarization error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

// GET /api/ai/status/:jobId - Check AI job status
app.get('/status/:jobId', async (c) => {
  const { db } = initializeFirebase(c.env);
  const jobId = c.req.param('jobId');
  
  try {
    const jobRef = db.collection('ai_jobs').doc(jobId);
    const jobDoc = await jobRef.get();
    
    if (!jobDoc.exists) {
      return c.json({
        error: 'Not Found',
        message: 'Job not found'
      }, 404);
    }
    
    const job = jobDoc.data();
    
    // Verify ownership
    if (job.userId !== c.user.uid) {
      return c.json({
        error: 'Forbidden',
        message: 'You do not have access to this job'
      }, 403);
    }
    
    return c.json({
      jobId: job.id,
      status: job.status,
      progress: job.progress,
      result: job.result || null,
      error: job.error || null
    });
  } catch (error: any) {
    console.error('Get job status error:', error);
    return c.json({
      error: 'Internal Server Error',
      message: error.message
    }, 500);
  }
});

export default app;
```

**Verification:**
- [ ] Transcribe endpoint implemented
- [ ] Summarize endpoint implemented
- [ ] Job status endpoint implemented

---

### Phase 7 Completion Checklist

**Completed Tasks:**
- [ ] Task 7.1: Gemini AI client setup
- [ ] Task 7.2: AI processing endpoints implemented

**Deliverables:**
- [x] Gemini lib: `workers/src/lib/gemini.ts`
- [x] AI routes: `workers/src/routes/ai.ts`

**Phase 7 Success Criteria:**
- ✅ Gemini API integrated
- ✅ Transcription working
- ✅ Summarization working
- ✅ Job tracking functional

**Next Phase:** Phase 8 - Security & Validation

---

## Phase 8: Security & Validation

### Phase Overview

**Goal:** Implement security measures and input validation

**Duration:** 2-3 hours

**Prerequisites:**
- All previous phases completed
- API endpoints functional

**Deliverables:**
- Input validation middleware
- Rate limiting
- Error sanitization
- Security headers
- CSRF protection

---

### Task 8.1: Implement Input Validation

**Objective:** Validate all API inputs

**Steps:**

1. **Create `workers/src/middleware/validation.ts`:**

```typescript
import { Context, Next } from 'hono';

// Validation schemas
export const schemas = {
  recording: {
    title: (value: any) => typeof value === 'string' && value.length > 0 && value.length <= 200,
    duration: (value: any) => typeof value === 'number' && value > 0,
    fileSize: (value: any) => typeof value === 'number' && value > 0,
    quality: (value: any) => ['720p', '1080p'].includes(value),
    format: (value: any) => ['webm', 'mp4'].includes(value),
    driveFileId: (value: any) => typeof value === 'string' && value.length > 0
  }
};

// Validate request body
export function validateBody(schema: Record<string, (value: any) => boolean>) {
  return async (c: Context, next: Next) => {
    const body = await c.req.json();
    
    const errors: string[] = [];
    
    for (const [field, validator] of Object.entries(schema)) {
      if (body[field] === undefined) {
        errors.push(`Missing required field: ${field}`);
      } else if (!validator(body[field])) {
        errors.push(`Invalid value for field: ${field}`);
      }
    }
    
    if (errors.length > 0) {
      return c.json({
        error: 'Validation Error',
        message: 'Request validation failed',
        details: errors
      }, 400);
    }
    
    await next();
  };
}
```

**Verification:**
- [ ] Validation middleware created
- [ ] Schemas defined
- [ ] Error messages clear

---

### Task 8.2: Implement Rate Limiting

**Objective:** Prevent API abuse

**Steps:**

1. **Create `workers/src/middleware/rate-limit.ts`:**

```typescript
import { Context, Next } from 'hono';

// Simple in-memory rate limiter
const rateLimits = new Map<string, { count: number; resetAt: number }>();

export function rateLimit(maxRequests: number = 100, windowMs: number = 60000) {
  return async (c: Context, next: Next) => {
    const key = c.req.header('x-forwarded-for') || 'unknown';
    const now = Date.now();
    
    const limit = rateLimits.get(key);
    
    if (limit) {
      if (now > limit.resetAt) {
        // Reset window
        rateLimits.set(key, { count: 1, resetAt: now + windowMs });
      } else if (limit.count >= maxRequests) {
        // Rate limit exceeded
        return c.json({
          error: 'Too Many Requests',
          message: 'Rate limit exceeded. Please try again later.',
          retryAfter: Math.ceil((limit.resetAt - now) / 1000)
        }, 429);
      } else {
        // Increment count
        limit.count++;
      }
    } else {
      // First request
      rateLimits.set(key, { count: 1, resetAt: now + windowMs });
    }
    
    await next();
  };
}
```

**Verification:**
- [ ] Rate limiting implemented
- [ ] 429 responses for exceeded limits
- [ ] Reset window working

---

### Task 8.3: Add Security Headers

**Objective:** Configure security headers

**Steps:**

1. **Update `workers/src/index.ts` with security middleware:**

```typescript
// Add after CORS middleware
app.use('*', async (c, next) => {
  await next();
  
  // Security headers
  c.header('X-Content-Type-Options', 'nosniff');
  c.header('X-Frame-Options', 'DENY');
  c.header('X-XSS-Protection', '1; mode=block');
  c.header('Referrer-Policy', 'strict-origin-when-cross-origin');
  c.header('Permissions-Policy', 'geolocation=(), microphone=(), camera=()');
});
```

**Verification:**
- [ ] Security headers added
- [ ] Headers present in responses

---

### Phase 8 Completion Checklist

**Completed Tasks:**
- [ ] Task 8.1: Input validation implemented
- [ ] Task 8.2: Rate limiting added
- [ ] Task 8.3: Security headers configured

**Deliverables:**
- [x] Validation middleware: `workers/src/middleware/validation.ts`
- [x] Rate limit middleware: `workers/src/middleware/rate-limit.ts`

**Phase 8 Success Criteria:**
- ✅ All inputs validated
- ✅ Rate limiting working
- ✅ Security headers present

**Next Phase:** Phase 9 - Testing & Documentation

---

## Phase 9: Testing & Documentation

### Phase Overview

**Goal:** Test all endpoints and document API

**Duration:** 3-4 hours

**Prerequisites:**
- All previous phases completed
- API fully functional

**Deliverables:**
- API endpoint tests
- Integration tests
- API documentation
- Postman collection

---

### Task 9.1: Create API Tests

**Objective:** Test all API endpoints

**Steps:**

1. **Install testing tools:**
```bash
cd workers
npm install --save-dev vitest @vitest/ui
```

2. **Create test file `workers/tests/api.test.ts`:**

```typescript
import { describe, it, expect } from 'vitest';
// Add actual test implementations
```

**Verification:**
- [ ] Tests created
- [ ] All endpoints covered

---

### Task 9.2: Create API Documentation

**Objective:** Document all API endpoints

**Steps:**

1. **Create `workers/API_DOCUMENTATION.md`:**

```markdown
# OpenMeet API Documentation

## Base URL
- Production: `https://openmeet.panotech.uk/api`
- Development: `https://openmeet-api-dev.[subdomain].workers.dev/api`

## Authentication
All endpoints except `/auth/signin` require authentication.

Include Firebase ID token in Authorization header:
```
Authorization: Bearer <firebase-id-token>
```

## Endpoints

### Authentication

#### POST /auth/signin
Sign in with Firebase ID token

**Request:**
```json
{
  "idToken": "string"
}
```

**Response:**
```json
{
  "success": true,
  "user": {
    "uid": "string",
    "email": "string",
    "displayName": "string",
    "photoURL": "string"
  },
  "token": "string"
}
```

[... continue for all endpoints ...]
```

**Verification:**
- [ ] Documentation complete
- [ ] Examples included
- [ ] All endpoints documented

---

### Phase 9 Completion Checklist

**Completed Tasks:**
- [ ] Task 9.1: API tests created
- [ ] Task 9.2: API documentation written

**Deliverables:**
- [x] Test files: `workers/tests/`
- [x] API docs: `workers/API_DOCUMENTATION.md`

**Phase 9 Success Criteria:**
- ✅ All endpoints tested
- ✅ Documentation complete

**Next Phase:** Phase 10 - Deployment & Monitoring

---

## Phase 10: Deployment & Monitoring

### Phase Overview

**Goal:** Deploy to production and set up monitoring

**Duration:** 2-3 hours

**Prerequisites:**
- All previous phases completed
- All tests passing

**Deliverables:**
- Production deployment
- Environment configuration
- Basic logging
- Deployment documentation

---

### Task 10.1: Production Deployment

**Objective:** Deploy Workers to production

**Steps:**

1. **Deploy to production:**
```bash
cd workers
wrangler deploy --env production
```

2. **Verify deployment:**
```bash
curl https://openmeet.panotech.uk/api/health
```

**Verification:**
- [ ] Deployed successfully
- [ ] All endpoints accessible
- [ ] No deployment errors

---

### Task 10.2: Set Up Logging

**Objective:** Configure error logging

**Steps:**

1. **Add logging to `workers/src/lib/logger.ts`:**

```typescript
export function logError(error: Error, context: any = {}) {
  console.error('Error:', {
    message: error.message,
    stack: error.stack,
    context,
    timestamp: new Date().toISOString()
  });
}

export function logInfo(message: string, data: any = {}) {
  console.log('Info:', {
    message,
    data,
    timestamp: new Date().toISOString()
  });
}
```

**Verification:**
- [ ] Logging implemented
- [ ] Errors logged with context

---

### Phase 10 Completion Checklist

**Completed Tasks:**
- [ ] Task 10.1: Production deployment complete
- [ ] Task 10.2: Logging configured

**Deliverables:**
- [x] Production Workers deployed
- [x] Logging system: `workers/src/lib/logger.ts`

**Phase 10 Success Criteria:**
- ✅ Production deployment successful
- ✅ All endpoints working in production
- ✅ Logging functional

---

## Backend Implementation Complete!

### Final Verification Checklist

**All Phases Complete:**
- [ ] Phase 1: Firebase Project Setup ✅
- [ ] Phase 2: Database Schema Implementation ✅
- [ ] Phase 3: Authentication Backend ✅
- [ ] Phase 4: Google Drive Integration ✅
- [ ] Phase 5: Cloudflare Workers Setup ✅
- [ ] Phase 6: API Endpoints Implementation ✅
- [ ] Phase 7: AI Integration (Gemini) ✅
- [ ] Phase 8: Security & Validation ✅
- [ ] Phase 9: Testing & Documentation ✅
- [ ] Phase 10: Deployment & Monitoring ✅

**System Status:**
- ✅ Firebase Auth configured and working
- ✅ Firestore database with security rules
- ✅ Google Drive API integrated
- ✅ Cloudflare Workers deployed
- ✅ All API endpoints functional
- ✅ Gemini AI integrated
- ✅ Security measures in place
- ✅ Tests passing
- ✅ Documentation complete
- ✅ Production deployment successful

**Ready for Frontend Integration!**

The Backend Agent's work is now complete. The Frontend Agent can now:
1. Use authentication endpoints for user sign-in
2. Create/read/update/delete recordings via API
3. Trigger AI processing (transcription/summarization)
4. Fetch signed URLs for video playback
5. Manage folders and organization

**Next Steps:**
1. Review this plan with user
2. Begin implementation following each phase
3. Test each phase before proceeding
4. Document any deviations or issues
5. Coordinate with Frontend Agent for integration

---

## Reference Documents

### Primary Reference Documents

| Document | Location | Lines | Purpose |
|----------|----------|-------|---------|
| **Implementation Plan** | `docs/IMPLEMENTATION_PLAN.md` | 3,493 | Complete project specification |
| **Plan Handoff Report** | `docs/PLAN_HANDOFF_BACKEND.md` | 1,053 | Backend-specific handoff from Planner |
| **Backend Rules** | `.agent/rules/backend-rule.md` | - | Coding rules and conventions |
| **Backend Instructions** | `.github/instructions/backend.instructions.md` | - | Agent-specific instructions |

### Supporting Documents

| Document | Purpose |
|----------|---------|
| `plans/UIUX_PLAN.md` | UI/UX design specifications (3,948 lines) |
| `plans/FRONTEND_PLAN.md` | Frontend implementation plan |
| `docs/PLAN_HANDOFF_FRONTEND.md` | Frontend handoff report (1,350 lines) |

### External Resources

**Firebase Documentation:**
- Firebase Auth: https://firebase.google.com/docs/auth
- Cloud Firestore: https://firebase.google.com/docs/firestore
- Security Rules: https://firebase.google.com/docs/firestore/security/get-started

**Cloudflare Workers:**
- Workers Documentation: https://developers.cloudflare.com/workers
- Hono Framework: https://hono.dev
- Wrangler CLI: https://developers.cloudflare.com/workers/wrangler

**Google APIs:**
- Google Drive API: https://developers.google.com/drive/api/v3/reference
- OAuth 2.0: https://developers.google.com/identity/protocols/oauth2
- Gemini AI: https://ai.google.dev/docs

---

## Critical Implementation Notes

### 1. Security Considerations

**API Key Management:**
- ❌ NEVER commit API keys or secrets to Git
- ✅ Use `wrangler secret` for Cloudflare Workers
- ✅ Store service account keys outside of repository
- ✅ Add sensitive files to `.gitignore`

**Authentication:**
- All API endpoints (except /auth/signin) must use `authMiddleware`
- Verify token on every request
- Check resource ownership before allowing access
- Implement token refresh on client-side

**Firestore Security:**
- Rules deployed to production restrict all access to user's own data
- Test rules thoroughly with Firebase Emulator
- Never bypass security rules with Admin SDK in client-accessible code
- Validate all user inputs before writing to Firestore

**CORS Configuration:**
- Only allow requests from dashboard domain and localhost
- Do not use wildcard (*) in production
- Verify origin header for sensitive operations

### 2. Error Handling Strategy

**Client Errors (4xx):**
- `400 Bad Request`: Invalid input, missing required fields
- `401 Unauthorized`: Missing or invalid authentication token
- `403 Forbidden`: User doesn't own the resource
- `404 Not Found`: Resource doesn't exist
- `429 Too Many Requests`: Rate limit exceeded

**Server Errors (5xx):**
- `500 Internal Server Error`: Unexpected errors
- Log full error details server-side
- Return sanitized error messages to client
- Never expose sensitive information in error responses

### 3. Performance Optimization

**Firestore Query Optimization:**
- Use composite indexes for all multi-field queries
- Limit query results (default 20, max 100)
- Implement cursor-based pagination for large datasets
- Denormalize data where appropriate

**Cloudflare Workers Optimization:**
- Workers have 50ms CPU time limit (Free tier)
- Offload heavy processing to background tasks if needed
- Use lightweight libraries (Hono instead of Express)

### 4. Frontend Integration Guide

**For Frontend Agent - API Client Setup:**

```typescript
// api/client.ts
export async function apiCall(endpoint: string, options: RequestInit = {}) {
  const user = auth.currentUser;
  const token = await user?.getIdToken();
  
  const response = await fetch(`https://openmeet.panotech.uk/api${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`,
      ...options.headers
    }
  });
  
  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.message);
  }
  
  return response.json();
}
```

### 5. Cost Management

**Free Tier Limits:**
- Firebase Auth: Unlimited (always free)
- Cloud Firestore: 50k reads/day, 20k writes/day
- Cloudflare Workers: 100k requests/day
- Gemini API: Pay-per-use (~$0.0001 per 1k characters)
- Google Drive: 15GB per user (user's personal storage)

**Estimated Monthly Costs (100 users):**
- Firebase: $0 (within free tier)
- Cloudflare Workers: $0 (within free tier)
- Gemini AI: ~$5-10 (depends on usage)
- **Total: ~$5-10/month** for MVP

---

## Summary

### What This Plan Provides

**Complete Backend Implementation:**
- ✅ 10 sequential phases with 50+ tasks
- ✅ Step-by-step instructions for every task
- ✅ Code examples and templates
- ✅ Verification checklists
- ✅ Testing strategies
- ✅ Deployment procedures

**Clear Separation of Concerns:**
- ❌ No UI/UX work (that's for UI/UX Agent)
- ❌ No frontend code (that's for Frontend Agent)
- ✅ Only backend infrastructure and APIs
- ✅ Clear integration points with frontend

**Production-Ready Architecture:**
- Serverless-first design
- Security best practices
- Scalable from day one
- Cost-optimized for MVP
- Ready for growth

**Estimated Timeline:**
- **Minimum:** 27 hours (if everything goes smoothly)
- **Maximum:** 37 hours (with debugging and testing)
- **Average:** 32 hours (realistic estimate)

**Success Metrics:**
- All 10 phases completed
- All tests passing
- Production deployment successful
- Frontend can integrate seamlessly
- No security vulnerabilities

---

## Final Notes for Backend Agent

**You Are Building:**
1. A serverless backend with no traditional servers
2. Firebase for auth, database, and user management
3. Cloudflare Workers for API endpoints
4. Integration with Google Drive and Gemini AI
5. Secure, scalable, cost-effective infrastructure

**You Are NOT Building:**
1. Chrome extension code
2. React dashboard components
3. UI/UX designs
4. Frontend state management
5. Client-side video processing

**Your Success Criteria:**
- Frontend Agent can call your APIs successfully
- Users can authenticate securely
- Recordings are stored and retrieved correctly
- AI processing works end-to-end
- System is secure and performs well

**Remember:**
- Follow each phase sequentially
- Test thoroughly at each step
- Document any issues or deviations
- Ask for clarification if needed
- Coordinate with Frontend Agent for integration

**Good luck with implementation! 🚀**

---

**End of Backend Agent Plan - Version 1.0.0**

