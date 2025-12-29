# OpenMeet Recorder - Frontend Implementation Plan

> **Status:** Draft - Awaiting Review  
> **Created:** 2025-12-29  
> **Project Repository:** https://github.com/PRATS-gits/openmeet-chromium  
> **Agent:** Frontend Agent  
> **Version:** 1.0.0  
> **Based On:** PLAN_HANDOFF_FRONTEND.md, UIUX_PLAN.md

---

## Table of Contents
1. [Overview](#overview)
2. [Frontend Agent Scope](#frontend-agent-scope)
3. [Technology Stack](#technology-stack)
4. [Project Architecture](#project-architecture)
5. [Implementation Phases](#implementation-phases)
   - [Phase 1: Project Setup & Design System](#phase-1-project-setup--design-system)
   - [Phase 2: Shared Components Library](#phase-2-shared-components-library)
   - [Phase 3: Chrome Extension UI](#phase-3-chrome-extension-ui)
   - [Phase 4: Dashboard Layout & Navigation](#phase-4-dashboard-layout--navigation)
   - [Phase 5: Recording List & Display](#phase-5-recording-list--display)
   - [Phase 6: Recording Detail & Playback](#phase-6-recording-detail--playback)
   - [Phase 7: State Management & Integration](#phase-7-state-management--integration)
6. [Testing & Quality Assurance](#testing--quality-assurance)
7. [Performance Optimization](#performance-optimization)
8. [Deliverables Checklist](#deliverables-checklist)
9. [Handoff to Backend Agent](#handoff-to-backend-agent)

---

## 1. Overview

### 1.1 Purpose
This document defines the Frontend implementation plan for the OpenMeet Recorder project. The Frontend Agent is responsible for implementing the UI/UX designs in React + TypeScript, building both the Chrome extension and dashboard application with full functionality, state management, and API integration.

### 1.2 Project Context
**OpenMeet Recorder** is a Chrome extension with a companion dashboard that enables users to record Google Meet sessions with features including:
- Automatic mic synchronization
- Chunked concurrent uploads to Google Drive (100MB chunks)
- AI-powered multilingual meeting summarization
- Custom video player with transcript synchronization
- Recording management and organization

### 1.3 Key Deliverables
The Frontend Agent will implement:
1. **Chrome Extension (Manifest V3)**
   - Popup interface for recording controls
   - Floating control bar overlay during recording
   - Options page for settings management
   - Background service worker for recording logic
   - Content script for Meet tab integration

2. **Dashboard Application (React SPA)**
   - Layout with sidebar navigation and header
   - Recording list with grid/list views
   - Filtering, sorting, and search functionality
   - Recording detail page with video player
   - Interactive transcript with timestamp navigation
   - AI summary display
   - Settings and user profile management

3. **State Management System**
   - Zustand stores for extension and dashboard
   - API integration services (Firebase, Google Drive)
   - Error handling and loading states

4. **Testing & Documentation**
   - Component testing
   - End-to-end workflow testing
   - Technical documentation

### 1.4 Implementation Approach
- **Phase-Based Development:** 7 sequential phases with clear deliverables
- **Component-First:** Build reusable components before complex features
- **Type-Safe:** TypeScript throughout for type safety
- **Design-Driven:** Strict adherence to UIUX_PLAN.md specifications
- **Test-As-You-Go:** Validate each phase before proceeding
- **Integration-Ready:** Design for seamless Backend handoff

---

## 2. Frontend Agent Scope

### 2.1 Responsibilities (What Frontend Agent DOES)

#### 2.1.1 React Component Implementation
- ✅ Convert HTML/CSS specs from UIUX_PLAN.md to React components
- ✅ Implement TypeScript interfaces and prop types
- ✅ Follow component composition patterns
- ✅ Implement all component variants and states
- ✅ Handle component lifecycle and side effects
- ✅ Implement proper event handling

#### 2.1.2 State Management
- ✅ Design and implement Zustand stores
- ✅ Manage recording state (status, timer, settings, chunks)
- ✅ Manage authentication state (user, tokens, session)
- ✅ Manage dashboard data state (recordings, filters, sort, view)
- ✅ Manage UI state (sidebar, modals, toasts, panels)
- ✅ Implement state persistence (localStorage, IndexedDB)
- ✅ Handle state synchronization between extension and dashboard

#### 2.1.3 Routing & Navigation
- ✅ Set up React Router for dashboard
- ✅ Implement route guards for authentication
- ✅ Handle deep linking and URL parameters
- ✅ Implement breadcrumb navigation
- ✅ Handle browser history and navigation state

#### 2.1.4 API Integration
- ✅ Firebase Authentication (Google OAuth, session management)
- ✅ Cloud Firestore queries (CRUD operations for recordings)
- ✅ Google Drive API (chunk upload, video URL retrieval)
- ✅ Cloudflare Workers API (trigger AI processing, fetch results)
- ✅ Error handling and retry logic
- ✅ Request/response type definitions

#### 2.1.5 Chrome Extension Specifics
- ✅ Manifest V3 configuration
- ✅ Content script injection and communication
- ✅ Background service worker implementation
- ✅ Message passing between components (runtime.sendMessage)
- ✅ Chrome Storage API integration (chrome.storage)
- ✅ IndexedDB management via Dexie.js
- ✅ Permissions handling (tabs, storage, identity)

#### 2.1.6 Media Handling
- ✅ MediaRecorder API integration
- ✅ Video capture from Meet tabs
- ✅ Audio synchronization (mic + tab audio)
- ✅ Chunk-based recording (100MB chunks)
- ✅ Background upload during recording
- ✅ Video player implementation with custom controls
- ✅ Playback controls (play/pause, seek, volume, speed)
- ✅ Progress tracking and buffering

#### 2.1.7 Form Handling
- ✅ Input validation (client-side)
- ✅ Form state management
- ✅ Error message display
- ✅ Submit handling with loading states
- ✅ Form reset functionality

#### 2.1.8 Error Handling
- ✅ Error boundaries in React
- ✅ API error handling with user-friendly messages
- ✅ Network failure handling
- ✅ Retry logic for failed operations
- ✅ Error logging and debugging
- ✅ Graceful degradation

#### 2.1.9 Loading & Feedback States
- ✅ Skeleton loaders for data fetching
- ✅ Spinner components for actions
- ✅ Progress indicators for uploads
- ✅ Optimistic UI updates
- ✅ Toast notifications for feedback
- ✅ Empty states for no data

#### 2.1.10 Responsive Behavior
- ✅ Implement media query breakpoints
- ✅ Mobile-first responsive design
- ✅ Touch-friendly interactions
- ✅ Adaptive layouts (mobile, tablet, desktop)
- ✅ Responsive images and media

#### 2.1.11 Accessibility Implementation
- ✅ ARIA attributes for semantic HTML
- ✅ Keyboard navigation support
- ✅ Focus management and indicators
- ✅ Screen reader compatibility
- ✅ Color contrast compliance (WCAG AA)
- ✅ Reduced motion support

#### 2.1.12 Performance Optimization
- ✅ Code splitting and lazy loading
- ✅ Bundle size optimization
- ✅ Image optimization
- ✅ Debouncing and throttling
- ✅ Memoization for expensive computations
- ✅ Virtual scrolling for large lists (if needed)

### 2.2 Non-Responsibilities (What Frontend Agent DOES NOT DO)

#### 2.2.1 Backend Development
- ❌ Cloudflare Workers implementation
- ❌ Firebase Cloud Functions
- ❌ Server-side API logic
- ❌ Database schema design (uses provided structure)
- ❌ Authentication backend logic (uses Firebase Auth)

#### 2.2.2 AI & Processing
- ❌ Gemini AI model integration (Backend handles this)
- ❌ Video transcription logic
- ❌ Summary generation algorithms
- ❌ AI processing workflows

#### 2.2.3 Infrastructure
- ❌ DevOps and deployment pipelines
- ❌ Server configuration
- ❌ Database optimization and indexing
- ❌ CDN setup
- ❌ Monitoring and logging infrastructure

#### 2.2.4 Design Work
- ❌ Creating new UI designs (uses UIUX_PLAN.md)
- ❌ Modifying design system tokens
- ❌ Creating visual assets (uses provided specifications)

### 2.3 Dependencies

#### 2.3.1 From UI/UX Agent
- ✅ Complete UIUX_PLAN.md with design specifications
- ✅ Design system tokens (colors, typography, spacing)
- ✅ Component specifications with HTML/CSS
- ✅ Interaction patterns and animations
- ✅ Responsive design guidelines

#### 2.3.2 From Backend Agent (Parallel/Future)
- 🔄 Firebase project configuration
- 🔄 Google Drive API credentials
- 🔄 Cloudflare Workers API endpoints
- 🔄 Firestore database structure
- 🔄 API request/response formats

#### 2.3.3 External Dependencies
- ✅ Chrome Web Store (for extension submission)
- ✅ Firebase Console (for project setup)
- ✅ Google Cloud Console (for API credentials)
- ✅ npm registry (for package dependencies)

---

## 3. Technology Stack

### 3.1 Chrome Extension Stack

#### 3.1.1 Core Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **Manifest Version** | V3 | Chrome extension platform |
| **TypeScript** | 5.3+ | Type-safe development |
| **React** | 18.2+ | UI library |
| **Vite** | 5.0+ | Build tool and dev server |
| **TailwindCSS** | 3.4+ | Utility-first styling |
| **Zustand** | 4.5+ | State management |
| **Dexie.js** | 3.2+ | IndexedDB wrapper |
| **Lucide React** | latest | Icon library |

#### 3.1.2 Chrome APIs Used
```typescript
// Core APIs
chrome.tabs         // Tab management
chrome.storage      // Settings persistence
chrome.runtime      // Message passing
chrome.identity     // OAuth authentication
chrome.scripting    // Content script injection
chrome.offscreen    // Background recording

// Permissions Required
"permissions": [
  "tabs",
  "storage",
  "identity",
  "scripting",
  "offscreen"
],
"host_permissions": [
  "https://meet.google.com/*"
]
```

#### 3.1.3 Extension Dependencies
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "zustand": "^4.5.0",
    "dexie": "^3.2.0",
    "lucide-react": "latest"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "vite-plugin-chrome-extension": "latest",
    "tailwindcss": "^3.4.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "@types/chrome": "latest",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0"
  }
}
```

### 3.2 Dashboard Stack

#### 3.2.1 Core Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **TypeScript** | 5.3+ | Type-safe development |
| **React** | 18.2+ | UI library |
| **Vite** | 5.0+ | Build tool and dev server |
| **React Router** | 6.21+ | Client-side routing |
| **TailwindCSS** | 3.4+ | Utility-first styling |
| **ShadcnUI** | latest | Component library |
| **Zustand** | 4.5+ | State management |
| **Firebase** | 10.0+ | Backend services |
| **Lucide React** | latest | Icon library |

#### 3.2.2 ShadcnUI Components
```bash
# Components to install via shadcn CLI
npx shadcn-ui@latest add button
npx shadcn-ui@latest add card
npx shadcn-ui@latest add input
npx shadcn-ui@latest add select
npx shadcn-ui@latest add dropdown-menu
npx shadcn-ui@latest add dialog
npx shadcn-ui@latest add tabs
npx shadcn-ui@latest add avatar
npx shadcn-ui@latest add badge
npx shadcn-ui@latest add tooltip
npx shadcn-ui@latest add popover
npx shadcn-ui@latest add progress
npx shadcn-ui@latest add switch
npx shadcn-ui@latest add alert
npx shadcn-ui@latest add separator
npx shadcn-ui@latest add label
npx shadcn-ui@latest add checkbox
```

#### 3.2.3 Dashboard Dependencies
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.21.0",
    "zustand": "^4.5.0",
    "firebase": "^10.0.0",
    "lucide-react": "latest",
    "@radix-ui/react-dialog": "latest",
    "@radix-ui/react-dropdown-menu": "latest",
    "@radix-ui/react-tabs": "latest",
    "@radix-ui/react-avatar": "latest",
    "@radix-ui/react-tooltip": "latest",
    "@radix-ui/react-popover": "latest",
    "@radix-ui/react-progress": "latest",
    "@radix-ui/react-switch": "latest",
    "@radix-ui/react-alert": "latest",
    "@radix-ui/react-separator": "latest",
    "@radix-ui/react-label": "latest",
    "@radix-ui/react-checkbox": "latest"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "tailwindcss": "^3.4.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@types/react-router-dom": "^5.3.0"
  }
}
```

### 3.3 Shared Development Tools

#### 3.3.1 Code Quality
```json
{
  "devDependencies": {
    "eslint": "^8.0.0",
    "eslint-config-prettier": "^9.0.0",
    "eslint-plugin-react": "^7.33.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "prettier": "^3.0.0",
    "prettier-plugin-tailwindcss": "^0.5.0"
  }
}
```

#### 3.3.2 Testing (Optional but Recommended)
```json
{
  "devDependencies": {
    "vitest": "^1.0.0",
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.0.0",
    "@testing-library/user-event": "^14.0.0",
    "playwright": "^1.40.0"
  }
}
```

### 3.4 Package Manager
- **Primary:** pnpm (recommended for speed and disk efficiency)
- **Alternative:** npm or yarn
- **Workspace Setup:** Monorepo structure with extension/ and dashboard/

---

## 4. Project Architecture

### 4.1 Monorepo Structure

```
openmeet-chromium/
├── extension/                  # Chrome Extension
│   ├── public/
│   │   ├── manifest.json       # MV3 manifest
│   │   ├── icons/              # Extension icons (16, 48, 128)
│   │   └── _locales/           # i18n (optional)
│   ├── src/
│   │   ├── background/         # Service worker
│   │   │   ├── index.ts        # Main background script
│   │   │   ├── recording.ts    # Recording logic
│   │   │   ├── upload.ts       # Chunk upload logic
│   │   │   ├── storage.ts      # IndexedDB management
│   │   │   └── messaging.ts    # Message handlers
│   │   ├── content/            # Content scripts
│   │   │   ├── index.ts        # Main content script
│   │   │   └── ControlBar.tsx  # Injected control bar
│   │   ├── popup/              # Popup UI
│   │   │   ├── index.tsx       # Entry point
│   │   │   ├── Popup.tsx       # Main component
│   │   │   └── components/     # Popup components
│   │   │       ├── PopupHeader.tsx
│   │   │       ├── SourceSelector.tsx
│   │   │       ├── SettingsCheckboxes.tsx
│   │   │       ├── InfoNote.tsx
│   │   │       └── ActionButtons.tsx
│   │   ├── options/            # Options page
│   │   │   ├── index.tsx       # Entry point
│   │   │   ├── Options.tsx     # Main component
│   │   │   └── components/     # Options components
│   │   │       ├── OptionsLayout.tsx
│   │   │       ├── SettingsCard.tsx
│   │   │       ├── SelectDropdown.tsx
│   │   │       ├── ToggleSwitch.tsx
│   │   │       └── RadioButtonGroup.tsx
│   │   ├── components/         # Shared extension components
│   │   │   ├── ui/             # UI primitives
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Input.tsx
│   │   │   │   ├── Checkbox.tsx
│   │   │   │   ├── Spinner.tsx
│   │   │   │   └── ProgressBar.tsx
│   │   │   └── common/         # Common components
│   │   │       ├── Timer.tsx
│   │   │       ├── RecordingIndicator.tsx
│   │   │       └── AnnotationToolbar.tsx
│   │   ├── store/              # Zustand stores
│   │   │   ├── recordingStore.ts
│   │   │   ├── settingsStore.ts
│   │   │   └── authStore.ts
│   │   ├── lib/                # Utilities
│   │   │   ├── chrome-api.ts   # Chrome API wrappers
│   │   │   ├── storage.ts      # Storage utilities
│   │   │   ├── media.ts        # Media handling
│   │   │   └── utils.ts        # General utilities
│   │   ├── services/           # API services
│   │   │   ├── firebase.ts
│   │   │   ├── drive.ts
│   │   │   └── upload.ts
│   │   ├── styles/             # Global styles
│   │   │   ├── design-system.css
│   │   │   ├── globals.css
│   │   │   └── animations.css
│   │   └── types/              # TypeScript types
│   │       ├── chrome.d.ts
│   │       ├── recording.ts
│   │       └── index.d.ts
│   ├── tailwind.config.js
│   ├── vite.config.ts
│   ├── tsconfig.json
│   └── package.json
│
├── dashboard/                  # React Dashboard
│   ├── public/
│   │   ├── favicon.ico
│   │   └── assets/
│   ├── src/
│   │   ├── pages/              # Route pages
│   │   │   ├── Home.tsx        # Recordings list
│   │   │   ├── Detail.tsx      # Recording detail
│   │   │   ├── Settings.tsx    # User settings
│   │   │   ├── Starred.tsx     # Starred recordings
│   │   │   ├── Shared.tsx      # Shared recordings
│   │   │   ├── Trash.tsx       # Deleted recordings
│   │   │   └── Auth.tsx        # Authentication
│   │   │       ├── SignIn.tsx
│   │   │       └── Callback.tsx
│   │   ├── components/         # Components
│   │   │   ├── layout/         # Layout components
│   │   │   │   ├── DashboardLayout.tsx
│   │   │   │   ├── Sidebar.tsx
│   │   │   │   │   ├── SidebarLogo.tsx
│   │   │   │   │   ├── NavigationMenu.tsx
│   │   │   │   │   └── UserProfile.tsx
│   │   │   │   └── Header.tsx
│   │   │   │       ├── SearchBar.tsx
│   │   │   │       └── NotificationBadge.tsx
│   │   │   ├── recording/      # Recording components
│   │   │   │   ├── RecordingCard.tsx
│   │   │   │   ├── GridView.tsx
│   │   │   │   ├── ListView.tsx
│   │   │   │   ├── ViewToggle.tsx
│   │   │   │   ├── FilterControls.tsx
│   │   │   │   ├── FilterPanel.tsx
│   │   │   │   ├── SortDropdown.tsx
│   │   │   │   ├── ActiveFilterTags.tsx
│   │   │   │   ├── EmptyState.tsx
│   │   │   │   └── SkeletonCard.tsx
│   │   │   ├── player/         # Video player components
│   │   │   │   ├── VideoPlayer.tsx
│   │   │   │   ├── PlayerControls.tsx
│   │   │   │   ├── ProgressBar.tsx
│   │   │   │   └── VolumeControl.tsx
│   │   │   ├── transcript/     # Transcript components
│   │   │   │   ├── TranscriptView.tsx
│   │   │   │   ├── TranscriptEntry.tsx
│   │   │   │   └── TranscriptSearch.tsx
│   │   │   ├── summary/        # Summary components
│   │   │   │   ├── AISummaryView.tsx
│   │   │   │   ├── SummarySection.tsx
│   │   │   │   └── ActionItems.tsx
│   │   │   ├── details/        # Detail components
│   │   │   │   ├── DetailsTab.tsx
│   │   │   │   ├── PropertyRow.tsx
│   │   │   │   └── TagsInput.tsx
│   │   │   ├── comments/       # Comments components
│   │   │   │   ├── CommentsSection.tsx
│   │   │   │   ├── CommentItem.tsx
│   │   │   │   └── CommentInput.tsx
│   │   │   └── ui/             # UI primitives (ShadcnUI)
│   │   │       ├── button.tsx
│   │   │       ├── card.tsx
│   │   │       ├── input.tsx
│   │   │       ├── select.tsx
│   │   │       ├── dropdown-menu.tsx
│   │   │       ├── dialog.tsx
│   │   │       ├── tabs.tsx
│   │   │       ├── avatar.tsx
│   │   │       ├── badge.tsx
│   │   │       ├── tooltip.tsx
│   │   │       ├── popover.tsx
│   │   │       ├── progress.tsx
│   │   │       ├── switch.tsx
│   │   │       ├── alert.tsx
│   │   │       ├── separator.tsx
│   │   │       ├── label.tsx
│   │   │       ├── checkbox.tsx
│   │   │       ├── modal.tsx
│   │   │       ├── drawer.tsx
│   │   │       ├── toast.tsx
│   │   │       ├── skeleton.tsx
│   │   │       └── spinner.tsx
│   │   ├── store/              # Zustand stores
│   │   │   ├── authStore.ts
│   │   │   ├── recordingsStore.ts
│   │   │   └── uiStore.ts
│   │   ├── services/           # API services
│   │   │   ├── firebase.ts     # Firebase config
│   │   │   ├── auth.ts         # Auth service
│   │   │   ├── firestore.ts    # Firestore queries
│   │   │   ├── storage.ts      # Google Drive API
│   │   │   └── ai.ts           # AI processing API
│   │   ├── hooks/              # Custom React hooks
│   │   │   ├── useRecordings.ts
│   │   │   ├── useAuth.ts
│   │   │   ├── useMediaQuery.ts
│   │   │   ├── useDebounce.ts
│   │   │   └── useVideoPlayer.ts
│   │   ├── lib/                # Utilities
│   │   │   ├── utils.ts
│   │   │   ├── cn.ts           # Class name utility
│   │   │   ├── constants.ts
│   │   │   └── formatters.ts
│   │   ├── styles/             # Global styles
│   │   │   ├── design-system.css
│   │   │   ├── globals.css
│   │   │   └── animations.css
│   │   ├── types/              # TypeScript types
│   │   │   ├── recording.ts
│   │   │   ├── user.ts
│   │   │   ├── api.ts
│   │   │   └── index.d.ts
│   │   ├── App.tsx             # Root component
│   │   ├── main.tsx            # Entry point
│   │   └── router.tsx          # Route configuration
│   ├── components.json         # ShadcnUI config
│   ├── tailwind.config.js
│   ├── vite.config.ts
│   ├── tsconfig.json
│   └── package.json
│
├── shared/                     # Shared code (optional)
│   ├── types/
│   └── utils/
│
├── docs/                       # Documentation
│   ├── DESCRIPTION.md
│   ├── IMPLEMENTATION_PLAN.md
│   ├── PLAN_HANDOFF_FRONTEND.md
│   └── PLAN_HANDOFF_BACKEND.md
│
├── plans/                      # Agent plans
│   ├── UIUX_PLAN.md
│   ├── FRONTEND_PLAN.md (this file)
│   └── BACKEND_PLAN.md
│
├── prompts/                    # Agent prompts
│   ├── UIUX_PROMPT.md
│   ├── FRONTEND_PROMPT.md
│   └── BACKEND_PROMPT.md
│
├── .gitignore
├── LICENSE
├── README.md
└── pnpm-workspace.yaml         # pnpm workspace config
```

### 4.2 State Management Architecture

#### 4.2.1 Zustand Store Structure

**Recording Store (Extension)**
```typescript
// extension/src/store/recordingStore.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export type RecordingStatus = 'idle' | 'recording' | 'paused' | 'stopped' | 'uploading' | 'error';
export type RecordingSource = 'desktop' | 'tab' | 'canvas';

interface RecordingSettings {
  source: RecordingSource;
  tabAudio: boolean;
  microphone: boolean;
  annotations: boolean;
  quality: 'high' | 'medium' | 'low';
}

interface RecordingChunk {
  index: number;
  blob: Blob;
  timestamp: number;
  uploaded: boolean;
}

interface RecordingState {
  // State
  status: RecordingStatus;
  startTime: number | null;
  pauseTime: number | null;
  duration: number;
  settings: RecordingSettings;
  chunks: RecordingChunk[];
  currentRecordingId: string | null;
  uploadProgress: number;
  error: string | null;
  
  // Actions
  startRecording: (settings: RecordingSettings) => Promise<void>;
  pauseRecording: () => void;
  resumeRecording: () => void;
  stopRecording: () => Promise<void>;
  updateSettings: (settings: Partial<RecordingSettings>) => void;
  addChunk: (chunk: Blob) => void;
  updateUploadProgress: (progress: number) => void;
  setError: (error: string) => void;
  reset: () => void;
}

export const useRecordingStore = create<RecordingState>()(
  persist(
    (set, get) => ({
      // Initial state
      status: 'idle',
      startTime: null,
      pauseTime: null,
      duration: 0,
      settings: {
        source: 'desktop',
        tabAudio: true,
        microphone: true,
        annotations: false,
        quality: 'high',
      },
      chunks: [],
      currentRecordingId: null,
      uploadProgress: 0,
      error: null,
      
      // Actions implementation
      startRecording: async (settings) => {
        set({
          status: 'recording',
          startTime: Date.now(),
          settings,
          chunks: [],
          error: null,
        });
      },
      
      pauseRecording: () => {
        set({ status: 'paused', pauseTime: Date.now() });
      },
      
      resumeRecording: () => {
        const { pauseTime, duration } = get();
        const pauseDuration = pauseTime ? Date.now() - pauseTime : 0;
        set({
          status: 'recording',
          pauseTime: null,
          duration: duration + pauseDuration,
        });
      },
      
      stopRecording: async () => {
        set({ status: 'stopped' });
        // Upload remaining chunks
      },
      
      updateSettings: (settings) => {
        set((state) => ({
          settings: { ...state.settings, ...settings },
        }));
      },
      
      addChunk: (chunk) => {
        set((state) => ({
          chunks: [
            ...state.chunks,
            {
              index: state.chunks.length,
              blob: chunk,
              timestamp: Date.now(),
              uploaded: false,
            },
          ],
        }));
      },
      
      updateUploadProgress: (progress) => {
        set({ uploadProgress: progress });
      },
      
      setError: (error) => {
        set({ status: 'error', error });
      },
      
      reset: () => {
        set({
          status: 'idle',
          startTime: null,
          pauseTime: null,
          duration: 0,
          chunks: [],
          currentRecordingId: null,
          uploadProgress: 0,
          error: null,
        });
      },
    }),
    {
      name: 'recording-storage',
      partialize: (state) => ({ settings: state.settings }),
    }
  )
);
```

**Auth Store (Shared)**
```typescript
// shared/store/authStore.ts
import { create } from 'zustand';
import { User } from 'firebase/auth';

interface AuthState {
  // State
  user: User | null;
  loading: boolean;
  error: string | null;
  initialized: boolean;
  
  // Actions
  setUser: (user: User | null) => void;
  setLoading: (loading: boolean) => void;
  setError: (error: string | null) => void;
  signIn: () => Promise<void>;
  signOut: () => Promise<void>;
  refreshToken: () => Promise<void>;
  initialize: () => Promise<void>;
}

export const useAuthStore = create<AuthState>((set, get) => ({
  // Initial state
  user: null,
  loading: true,
  error: null,
  initialized: false,
  
  // Actions
  setUser: (user) => set({ user, loading: false }),
  setLoading: (loading) => set({ loading }),
  setError: (error) => set({ error, loading: false }),
  
  signIn: async () => {
    set({ loading: true, error: null });
    try {
      // Firebase auth sign in
    } catch (error) {
      set({ error: (error as Error).message, loading: false });
    }
  },
  
  signOut: async () => {
    set({ loading: true, error: null });
    try {
      // Firebase auth sign out
      set({ user: null, loading: false });
    } catch (error) {
      set({ error: (error as Error).message, loading: false });
    }
  },
  
  refreshToken: async () => {
    // Refresh authentication token
  },
  
  initialize: async () => {
    // Initialize auth state on app load
    set({ initialized: true });
  },
}));
```

**Recordings Store (Dashboard)**
```typescript
// dashboard/src/store/recordingsStore.ts
import { create } from 'zustand';

export interface Recording {
  id: string;
  userId: string;
  title: string;
  description: string;
  meetingUrl: string;
  driveFileId: string;
  videoUrl: string;
  thumbnailUrl: string;
  duration: number;
  fileSize: number;
  status: 'processing' | 'completed' | 'error';
  hasTranscript: boolean;
  hasSummary: boolean;
  starred: boolean;
  tags: string[];
  createdAt: Date;
  updatedAt: Date;
}

export type DateRange = 'all' | 'today' | 'week' | 'month' | 'year';
export type DurationFilter = 'all' | '<30min' | '30-60min' | '>60min';
export type SortOption = 'date-desc' | 'date-asc' | 'name-asc' | 'name-desc' | 'duration' | 'size';
export type ViewMode = 'grid' | 'list';

interface RecordingsFilters {
  dateRange: DateRange;
  duration: DurationFilter;
  starred: boolean;
  status: Recording['status'][];
  tags: string[];
  search: string;
}

interface RecordingsState {
  // State
  recordings: Recording[];
  loading: boolean;
  error: string | null;
  filters: RecordingsFilters;
  sort: SortOption;
  view: ViewMode;
  selectedRecording: Recording | null;
  
  // Actions
  fetchRecordings: () => Promise<void>;
  fetchRecording: (id: string) => Promise<Recording>;
  updateRecording: (id: string, data: Partial<Recording>) => Promise<void>;
  deleteRecording: (id: string) => Promise<void>;
  toggleStar: (id: string) => Promise<void>;
  addTag: (id: string, tag: string) => Promise<void>;
  removeTag: (id: string, tag: string) => Promise<void>;
  setFilters: (filters: Partial<RecordingsFilters>) => void;
  setSort: (sort: SortOption) => void;
  setView: (view: ViewMode) => void;
  setSelectedRecording: (recording: Recording | null) => void;
  clearFilters: () => void;
}

export const useRecordingsStore = create<RecordingsState>((set, get) => ({
  // Initial state
  recordings: [],
  loading: false,
  error: null,
  filters: {
    dateRange: 'all',
    duration: 'all',
    starred: false,
    status: [],
    tags: [],
    search: '',
  },
  sort: 'date-desc',
  view: 'grid',
  selectedRecording: null,
  
  // Actions
  fetchRecordings: async () => {
    set({ loading: true, error: null });
    try {
      // Fetch from Firestore
      // const recordings = await getRecordings();
      set({ recordings: [], loading: false });
    } catch (error) {
      set({ error: (error as Error).message, loading: false });
    }
  },
  
  fetchRecording: async (id) => {
    set({ loading: true, error: null });
    try {
      // Fetch single recording
      // const recording = await getRecording(id);
      return {} as Recording;
    } catch (error) {
      set({ error: (error as Error).message, loading: false });
      throw error;
    }
  },
  
  updateRecording: async (id, data) => {
    try {
      // Update in Firestore
      set((state) => ({
        recordings: state.recordings.map((r) =>
          r.id === id ? { ...r, ...data } : r
        ),
      }));
    } catch (error) {
      set({ error: (error as Error).message });
    }
  },
  
  deleteRecording: async (id) => {
    try {
      // Delete from Firestore
      set((state) => ({
        recordings: state.recordings.filter((r) => r.id !== id),
      }));
    } catch (error) {
      set({ error: (error as Error).message });
    }
  },
  
  toggleStar: async (id) => {
    const recording = get().recordings.find((r) => r.id === id);
    if (recording) {
      await get().updateRecording(id, { starred: !recording.starred });
    }
  },
  
  addTag: async (id, tag) => {
    const recording = get().recordings.find((r) => r.id === id);
    if (recording && !recording.tags.includes(tag)) {
      await get().updateRecording(id, {
        tags: [...recording.tags, tag],
      });
    }
  },
  
  removeTag: async (id, tag) => {
    const recording = get().recordings.find((r) => r.id === id);
    if (recording) {
      await get().updateRecording(id, {
        tags: recording.tags.filter((t) => t !== tag),
      });
    }
  },
  
  setFilters: (filters) => {
    set((state) => ({
      filters: { ...state.filters, ...filters },
    }));
  },
  
  setSort: (sort) => set({ sort }),
  setView: (view) => set({ view }),
  setSelectedRecording: (recording) => set({ selectedRecording: recording }),
  clearFilters: () => {
    set({
      filters: {
        dateRange: 'all',
        duration: 'all',
        starred: false,
        status: [],
        tags: [],
        search: '',
      },
    });
  },
}));
```

**UI Store (Dashboard)**
```typescript
// dashboard/src/store/uiStore.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export type ToastType = 'success' | 'error' | 'info' | 'warning';

export interface Toast {
  id: string;
  type: ToastType;
  message: string;
  duration?: number;
}

interface UIState {
  // State
  sidebarCollapsed: boolean;
  filterPanelOpen: boolean;
  activeModal: string | null;
  toasts: Toast[];
  
  // Actions
  toggleSidebar: () => void;
  setSidebarCollapsed: (collapsed: boolean) => void;
  openFilterPanel: () => void;
  closeFilterPanel: () => void;
  toggleFilterPanel: () => void;
  openModal: (modalId: string) => void;
  closeModal: () => void;
  addToast: (toast: Omit<Toast, 'id'>) => void;
  removeToast: (id: string) => void;
}

export const useUIStore = create<UIState>()(
  persist(
    (set, get) => ({
      // Initial state
      sidebarCollapsed: false,
      filterPanelOpen: false,
      activeModal: null,
      toasts: [],
      
      // Actions
      toggleSidebar: () => {
        set((state) => ({ sidebarCollapsed: !state.sidebarCollapsed }));
      },
      
      setSidebarCollapsed: (collapsed) => {
        set({ sidebarCollapsed: collapsed });
      },
      
      openFilterPanel: () => set({ filterPanelOpen: true }),
      closeFilterPanel: () => set({ filterPanelOpen: false }),
      toggleFilterPanel: () => {
        set((state) => ({ filterPanelOpen: !state.filterPanelOpen }));
      },
      
      openModal: (modalId) => set({ activeModal: modalId }),
      closeModal: () => set({ activeModal: null }),
      
      addToast: (toast) => {
        const id = `toast-${Date.now()}-${Math.random()}`;
        const newToast: Toast = {
          id,
          duration: 3000,
          ...toast,
        };
        
        set((state) => ({
          toasts: [...state.toasts, newToast],
        }));
        
        // Auto-remove after duration
        if (newToast.duration) {
          setTimeout(() => {
            get().removeToast(id);
          }, newToast.duration);
        }
      },
      
      removeToast: (id) => {
        set((state) => ({
          toasts: state.toasts.filter((t) => t.id !== id),
        }));
      },
    }),
    {
      name: 'ui-storage',
      partialize: (state) => ({
        sidebarCollapsed: state.sidebarCollapsed,
      }),
    }
  )
);
```

### 4.3 Routing Architecture (Dashboard)

#### 4.3.1 Route Structure
```typescript
// dashboard/src/router.tsx
import { createBrowserRouter, Navigate } from 'react-router-dom';
import DashboardLayout from './components/layout/DashboardLayout';
import Home from './pages/Home';
import Detail from './pages/Detail';
import Settings from './pages/Settings';
import Starred from './pages/Starred';
import Shared from './pages/Shared';
import Trash from './pages/Trash';
import SignIn from './pages/Auth/SignIn';
import Callback from './pages/Auth/Callback';
import { ProtectedRoute } from './components/ProtectedRoute';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <ProtectedRoute><DashboardLayout /></ProtectedRoute>,
    children: [
      { index: true, element: <Home /> },
      { path: 'recording/:id', element: <Detail /> },
      { path: 'settings', element: <Settings /> },
      { path: 'starred', element: <Starred /> },
      { path: 'shared', element: <Shared /> },
      { path: 'trash', element: <Trash /> },
    ],
  },
  {
    path: '/auth',
    children: [
      { path: 'signin', element: <SignIn /> },
      { path: 'callback', element: <Callback /> },
    ],
  },
  {
    path: '*',
    element: <Navigate to="/" replace />,
  },
]);
```

#### 4.3.2 Protected Route Component
```typescript
// dashboard/src/components/ProtectedRoute.tsx
import { ReactNode } from 'react';
import { Navigate, useLocation } from 'react-router-dom';
import { useAuthStore } from '../store/authStore';

interface ProtectedRouteProps {
  children: ReactNode;
}

export function ProtectedRoute({ children }: ProtectedRouteProps) {
  const { user, loading } = useAuthStore();
  const location = useLocation();
  
  if (loading) {
    return <div>Loading...</div>; // Or a proper loading component
  }
  
  if (!user) {
    return <Navigate to="/auth/signin" state={{ from: location }} replace />;
  }
  
  return <>{children}</>;
}
```

### 4.4 Chrome Extension Message Passing

#### 4.4.1 Message Types
```typescript
// extension/src/types/messages.ts
export type MessageType =
  | 'START_RECORDING'
  | 'PAUSE_RECORDING'
  | 'RESUME_RECORDING'
  | 'STOP_RECORDING'
  | 'GET_RECORDING_STATUS'
  | 'UPDATE_SETTINGS'
  | 'UPLOAD_CHUNK'
  | 'INJECT_CONTROL_BAR'
  | 'REMOVE_CONTROL_BAR';

export interface Message<T = any> {
  type: MessageType;
  payload?: T;
}

export interface MessageResponse<T = any> {
  success: boolean;
  data?: T;
  error?: string;
}
```

#### 4.4.2 Message Handlers
```typescript
// extension/src/background/messaging.ts
import { Message, MessageResponse } from '../types/messages';

export function setupMessageHandlers() {
  chrome.runtime.onMessage.addListener(
    (message: Message, sender, sendResponse) => {
      handleMessage(message)
        .then((response) => sendResponse(response))
        .catch((error) => sendResponse({ success: false, error: error.message }));
      
      return true; // Keep message channel open for async response
    }
  );
}

async function handleMessage(message: Message): Promise<MessageResponse> {
  switch (message.type) {
    case 'START_RECORDING':
      // Handle recording start
      return { success: true, data: { recordingId: 'xxx' } };
      
    case 'PAUSE_RECORDING':
      // Handle recording pause
      return { success: true };
      
    case 'RESUME_RECORDING':
      // Handle recording resume
      return { success: true };
      
    case 'STOP_RECORDING':
      // Handle recording stop
      return { success: true };
      
    case 'GET_RECORDING_STATUS':
      // Return current recording status
      return { success: true, data: { status: 'idle' } };
      
    case 'UPDATE_SETTINGS':
      // Update recording settings
      return { success: true };
      
    default:
      return { success: false, error: 'Unknown message type' };
  }
}
```

---

## 5. Implementation Phases

### Phase 1: Project Setup & Design System

**Duration:** 2-3 days  
**Priority:** 🔴 Critical  
**Dependencies:** None

#### 5.1.1 Objectives
- Set up development environment for both extension and dashboard
- Implement complete design system from UIUX_PLAN.md
- Configure build tools and development workflow
- Establish code quality standards

#### 5.1.2 Tasks

**Task 1.1: Initialize Projects**
- [ ] Create monorepo structure with pnpm workspace
- [ ] Initialize extension/ directory with Vite + React + TypeScript
- [ ] Initialize dashboard/ directory with Vite + React + TypeScript
- [ ] Set up Git repository and .gitignore
- [ ] Create README.md with setup instructions

**Task 1.2: Install Dependencies**
- [ ] Extension: Install React, TypeScript, Vite, TailwindCSS, Zustand, Dexie, Lucide React
- [ ] Extension: Install vite-plugin-chrome-extension
- [ ] Dashboard: Install React, TypeScript, Vite, TailwindCSS, Zustand, React Router, Firebase, Lucide React
- [ ] Dashboard: Install Radix UI components
- [ ] Both: Install ESLint, Prettier, and type definitions

**Task 1.3: Configure Build Tools**
- [ ] Extension: Create vite.config.ts with Chrome extension plugin
- [ ] Dashboard: Create vite.config.ts with standard SPA config
- [ ] Both: Create tsconfig.json with strict TypeScript settings
- [ ] Both: Configure path aliases (@ for src)
- [ ] pnpm-workspace.yaml for monorepo management

**Task 1.4: Implement Design System CSS**
- [ ] Create design-system.css with all CSS custom properties
  - Color palette (primary, secondary, semantic, neutrals)
  - Typography scale (font families, sizes, weights, line heights)
  - Spacing system (4px grid, 27 spacing values)
  - Shadow system (8 levels)
  - Border radius scale (9 values)
  - Animation durations and easing functions
  - Z-index layers (9 levels)
- [ ] Create globals.css with base styles
  - CSS reset
  - Base element styles
  - Scrollbar customization
  - Focus visible styles
- [ ] Create animations.css with keyframe animations
  - fadeIn, slideInUp, slideInRight, scaleUp, pulse, spin

**Task 1.5: Configure TailwindCSS**
- [ ] Create tailwind.config.js for extension
  - Extend theme with design tokens
  - Configure content paths
  - Add custom utilities
  - Configure plugins
- [ ] Create tailwind.config.js for dashboard
  - Same configuration as extension
  - Additional ShadcnUI configuration
- [ ] Create postcss.config.js for both projects

**Task 1.6: Initialize ShadcnUI (Dashboard)**
- [ ] Run `npx shadcn-ui@latest init`
- [ ] Create components.json configuration
- [ ] Install base components via CLI:
  - button, card, input, select
  - dropdown-menu, dialog, tabs
  - avatar, badge, tooltip, popover
  - progress, switch, alert
  - separator, label, checkbox

**Task 1.7: Set Up Code Quality Tools**
- [ ] Create .eslintrc.json with React + TypeScript rules
- [ ] Create .prettierrc with formatting rules
- [ ] Add prettier-plugin-tailwindcss
- [ ] Create VS Code settings.json for auto-format on save
- [ ] Add lint and format scripts to package.json

**Task 1.8: Create Project Structure**
- [ ] Extension: Create all folders (background, content, popup, options, components, store, lib, services, styles, types)
- [ ] Dashboard: Create all folders (pages, components, store, services, hooks, lib, styles, types)
- [ ] Create index.ts barrel exports where appropriate

#### 5.1.3 Deliverables
- ✅ Working Vite development servers for both projects
- ✅ Complete design-system.css implementation
- ✅ TailwindCSS configured and working
- ✅ ShadcnUI installed in dashboard
- ✅ ESLint and Prettier configured
- ✅ Project folder structure established
- ✅ Build and dev scripts working

#### 5.1.4 Validation Checklist
- [ ] `pnpm dev` runs extension dev server without errors
- [ ] `pnpm dev` runs dashboard dev server without errors
- [ ] TailwindCSS classes work in both projects
- [ ] Design system CSS variables are accessible
- [ ] TypeScript compilation succeeds with no errors
- [ ] ESLint and Prettier run without issues
- [ ] Hot module replacement works during development

#### 5.1.5 Reference
- UIUX_PLAN.md Section 3: Design System Foundation
- PLAN_HANDOFF_FRONTEND.md Section 5: Design System Reference
- PLAN_HANDOFF_FRONTEND.md Section 8.2: Phase 1 Recommended Tasks

---

### Phase 2: Shared Components Library

**Duration:** 3-4 days  
**Priority:** 🔴 Critical  
**Dependencies:** Phase 1 (Design System)

#### 5.2.1 Objectives
- Build reusable UI component library
- Implement all primitive components (Button, Input, Card, etc.)
- Create loading and feedback components
- Ensure all components follow design specifications exactly

#### 5.2.2 Tasks

**Task 2.1: Button Component**
- [ ] Create Button.tsx with TypeScript interface
- [ ] Implement all variants:
  - primary: bg-primary-600, hover:bg-primary-700
  - secondary: bg-gray-200, hover:bg-gray-300
  - success: bg-success-600, hover:bg-success-700
  - error: bg-error-600, hover:bg-error-700
  - ghost: bg-transparent, hover:bg-gray-100
  - link: text-primary-600, underline on hover
- [ ] Implement all sizes:
  - sm: h-8 (32px), text-sm, px-3
  - md: h-10 (40px), text-base, px-4
  - lg: h-12 (48px), text-lg, px-6
- [ ] Implement states:
  - default, hover, active, focus, disabled, loading
- [ ] Add icon support (left/right positioning)
- [ ] Add loading spinner integration
- [ ] Implement hover animations (lift, shadow)
- [ ] Add proper TypeScript props interface

**Task 2.2: Input Components**
- [ ] **TextInput.tsx**
  - Base input with 44px height
  - Border: 2px, gray-300, focus:primary-500
  - Focus ring: 4px, primary-500/50
  - Error state: red border + error message
  - Disabled state: gray background, cursor-not-allowed
  - Label support with htmlFor
- [ ] **Textarea.tsx**
  - Same styling as TextInput
  - Auto-resize option
  - Character count display
- [ ] **Select.tsx**
  - Dropdown with custom styling
  - Options list with hover states
  - Search/filter functionality (optional)
  - Multiple selection support (optional)
- [ ] **Checkbox.tsx**
  - Custom styled checkbox (24x24px)
  - Checkmark icon on checked
  - Indeterminate state
  - Label integration
- [ ] **Radio.tsx**
  - Custom styled radio button (24x24px)
  - Filled circle on selected
  - Radio group component
- [ ] **Toggle/Switch.tsx**
  - Toggle switch component (48x24px)
  - Smooth sliding animation
  - On/off states with color change

**Task 2.3: Card Component**
- [ ] Create Card.tsx base component
- [ ] Padding variants: p-4, p-6, p-8
- [ ] Border: 1px, gray-200
- [ ] Border radius: rounded-lg
- [ ] Shadow: shadow-sm, hover:shadow-md
- [ ] Hover effects: elevation + border color change
- [ ] Optional props: hoverable, clickable
- [ ] CardHeader, CardContent, CardFooter subcomponents

**Task 2.4: Badge & Avatar**
- [ ] **Badge.tsx**
  - Variants: default, primary, success, warning, error
  - Sizes: sm, md, lg
  - Rounded pill shape
  - Optional dot indicator
  - Optional close button
- [ ] **Avatar.tsx**
  - Circular image container
  - Fallback with initials
  - Sizes: sm (32px), md (40px), lg (48px), xl (64px)
  - Loading skeleton state
  - Status indicator (online, offline, busy)

**Task 2.5: Dropdown & Menu Components**
- [ ] **Dropdown.tsx**
  - Portal-based rendering (React Portal)
  - Trigger button integration
  - Positioning: bottom-left, bottom-right, top-left, top-right
  - Animations: fade + slide
  - Click outside to close
  - Keyboard navigation (Arrow keys, Enter, Escape)
- [ ] **DropdownMenu.tsx**
  - Menu item component with hover state
  - Divider component
  - Submenu support
  - Icon support in menu items
  - Keyboard shortcuts display

**Task 2.6: Modal & Drawer**
- [ ] **Modal.tsx**
  - Portal-based rendering
  - Backdrop with blur effect
  - Content container with animations
  - Close button (X icon)
  - Close on backdrop click (optional)
  - Close on Escape key
  - Scroll lock when open
  - Sizes: sm, md, lg, xl, full
- [ ] **Drawer.tsx**
  - Slide-in from right (or left/top/bottom)
  - Same features as Modal
  - Header with title and close button
  - Footer with action buttons

**Task 2.7: Toast Notification System**
- [ ] **Toast.tsx**
  - Toast container component
  - Toast item with auto-dismiss
  - Types: success (green), error (red), info (blue), warning (yellow)
  - Icon integration (CheckCircle, XCircle, Info, AlertTriangle)
  - Slide-in from right animation
  - Progress bar for auto-dismiss timer
  - Close button
  - Stack multiple toasts vertically
- [ ] **useToast.ts** hook
  - addToast function
  - removeToast function
  - Integration with UIStore

**Task 2.8: Tooltip & Popover**
- [ ] **Tooltip.tsx**
  - Portal-based positioning
  - Show on hover with delay (200ms)
  - Hide on mouse leave
  - Positioning: top, bottom, left, right
  - Small arrow pointing to trigger
  - Dark background with white text
- [ ] **Popover.tsx**
  - Portal-based positioning
  - Show/hide on click
  - Close on outside click
  - Positioning similar to Tooltip
  - Larger content area than Tooltip

**Task 2.9: Loading Components**
- [ ] **Spinner.tsx**
  - Rotating circular spinner
  - Sizes: sm (16px), md (24px), lg (32px), xl (48px)
  - Color variants matching theme
  - Animation: 1s linear infinite rotation
- [ ] **Skeleton.tsx**
  - Shimmer animation effect
  - Variants: text, circle, rectangle
  - Sizes matching content
  - Pulse animation: 1.5s ease-in-out infinite
- [ ] **ProgressBar.tsx**
  - Linear progress indicator
  - Percentage display
  - Determinate (with progress value)
  - Indeterminate (loading animation)
  - Color variants

#### 5.2.3 Deliverables
- ✅ Complete UI component library (15+ components)
- ✅ All components follow UIUX_PLAN.md specs exactly
- ✅ TypeScript interfaces for all props
- ✅ All variants and states implemented
- ✅ Hover, focus, and interaction animations
- ✅ Components are reusable and composable
- ✅ Storybook or component showcase (optional)

#### 5.2.4 Validation Checklist
- [ ] All components render without errors
- [ ] All variants match design specifications
- [ ] Hover and focus states work correctly
- [ ] Animations are smooth and match timing specs
- [ ] TypeScript props are properly typed
- [ ] Components are accessible (ARIA attributes)
- [ ] Components work in both extension and dashboard
- [ ] No console warnings or errors

#### 5.2.5 Reference
- UIUX_PLAN.md Section 4.3.2: Extension Input Components
- UIUX_PLAN.md Section 5: Dashboard Components
- PLAN_HANDOFF_FRONTEND.md Section 6.3: Shared/Common Components
- PLAN_HANDOFF_FRONTEND.md Section 5: Design System Reference

---

### Phase 3: Chrome Extension UI

**Duration:** 4-5 days  
**Priority:** 🔴 Critical  
**Dependencies:** Phase 1 (Design System), Phase 2 (Shared Components)

#### 5.3.1 Objectives
- Implement complete Chrome extension UI
- Build Popup interface with recording controls
- Build floating Control Bar overlay
- Build Options page for settings
- Set up content script injection
- Implement Chrome extension message passing

#### 5.3.2 Tasks

**Task 3.1: Manifest Configuration**
- [ ] Create manifest.json (Manifest V3)
- [ ] Define extension metadata (name, description, version, icons)
- [ ] Configure permissions:
  - tabs, storage, identity, scripting, offscreen
- [ ] Configure host_permissions:
  - https://meet.google.com/*
- [ ] Define popup, options_page, background, content_scripts
- [ ] Configure OAuth2 for Google Drive
- [ ] Set up icons (16x16, 48x48, 128x128)

**Task 3.2: Popup Interface**
- [ ] **PopupContainer** (Section 4.1.1)
  - Fixed width: 400px
  - Flexible height with max-height: 600px
  - Padding: 16px
  - Background: white
  - Border radius: 8px (if needed)
- [ ] **PopupHeader** (Section 4.1.2)
  - Extension logo and title
  - Settings icon button (opens options page)
  - Flexbox layout: space-between
- [ ] **SourceSelector** (Section 4.1.3)
  - 3 options in grid: Desktop, Tab, Canvas
  - Grid: 3 columns, gap: 8px
  - Each option: card with icon, label, description
  - Active state: blue border, blue background
  - Click to select
- [ ] **SettingsCheckboxes** (Section 4.1.4)
  - Tab Audio checkbox
  - Microphone checkbox
  - Annotations checkbox
  - Vertical stack with gap: 12px
  - Use shared Checkbox component
- [ ] **InfoNote** (Section 4.1.5)
  - Small info box with icon and text
  - Background: blue-50, border: blue-200
  - Icon: Info circle
  - Text: "Tab audio disabled when mic is on to prevent echo"
  - Conditional rendering based on mic state
- [ ] **ActionButtons** (Section 4.1.6)
  - Start Recording button (primary, large)
  - Close button (ghost, medium)
  - Flexbox layout: space-between
  - Start button state changes:
    - Idle: "Start Recording" (primary)
    - Recording: "Stop Recording" (error/red)
- [ ] **Loading State**
  - Show spinner when initializing
  - Overlay entire popup
  - Message: "Initializing..."
- [ ] **Recording States**
  - Idle: show source selector, settings, start button
  - Recording: show timer, control buttons, stop button
  - Uploading: show progress bar, status message

**Task 3.3: Control Bar Overlay**
- [ ] **ControlBarContainer** (Section 4.2)
  - Fixed positioning: bottom center
  - Draggable with mouse
  - Dark glassmorphism background: rgba(0, 0, 0, 0.8) + blur(10px)
  - Border radius: 999px (pill shape)
  - Padding: 12px 24px
  - Shadow: large elevation
  - Z-index: 9999 (highest layer)
- [ ] **RecordingIndicator**
  - Pulsing red dot
  - Size: 12x12px
  - Animation: pulse 1.5s infinite
  - Opacity: 1 → 0.3 → 1
  - Scale: 1 → 1.2 → 1
- [ ] **TimerDisplay**
  - Monospace font
  - Format: HH:MM:SS
  - Color: white
  - Font size: 18px, font-weight: 600
  - Update every second
- [ ] **ControlButtons**
  - Pause/Resume button (icon: Pause/Play)
  - Microphone toggle button (icon: Mic/MicOff)
  - Annotation button (icon: PenTool)
  - Buttons: circular, 40x40px
  - Icon-only with tooltip on hover
  - Flexbox layout: gap: 8px
- [ ] **StopButton**
  - Square button: 44x44px
  - Icon: Square (stop)
  - Background: red
  - Separated from control buttons with divider
- [ ] **AnnotationToolbar** (Section 4.2)
  - Vertical toolbar on right side
  - Slides in when annotation button clicked
  - Tools: Pen, Highlighter, Eraser, Text, Shapes, Color picker
  - Each tool: icon button with tooltip
  - Active tool: highlighted
  - Close button at top
- [ ] **Drag & Drop Functionality**
  - Make control bar draggable
  - Constrain to viewport boundaries
  - Save position in localStorage
  - Restore position on next recording

**Task 3.4: Options Page**
- [ ] **OptionsLayout** (Section 4.3.1)
  - Full-page layout (not popup)
  - Max-width: 800px, centered
  - Padding: 32px
  - Header with title and description
  - Settings sections with dividers
  - Footer with Save and Reset buttons
- [ ] **SettingsCard** (Section 4.3.1)
  - Card wrapper for each settings section
  - Title and description at top
  - Content area with form inputs
  - Use shared Card component
- [ ] **Recording Quality Settings** (Section 4.3.2)
  - Select dropdown: High (1080p), Medium (720p), Low (480p)
  - Bitrate info display
  - Use shared Select component
- [ ] **Audio Settings** (Section 4.3.2)
  - Toggle: Enable tab audio
  - Toggle: Enable microphone
  - Toggle: Echo cancellation
  - Radio group: Audio quality (High, Medium, Low)
  - Use shared Toggle and Radio components
- [ ] **Storage Settings** (Section 4.3.2)
  - Google Drive folder selection
  - Auto-upload toggle
  - Chunk size display (100MB)
  - Local storage info (IndexedDB usage)
- [ ] **Shortcuts Settings** (Section 4.3.2)
  - Keyboard shortcut inputs
  - Start/Stop recording
  - Pause/Resume
  - Annotation mode
  - Display current shortcuts
- [ ] **Advanced Settings** (Section 4.3.2)
  - Developer mode toggle
  - Debug logging toggle
  - Clear cache button
- [ ] **Form Validation** (Section 4.3)
  - Validate all inputs on change
  - Display error messages inline
  - Disable Save button if invalid
  - Show success toast on save
- [ ] **Save & Reset Functionality**
  - Save to chrome.storage.local
  - Reset to defaults confirmation modal
  - Show loading state during save
  - Show success/error toast after save

**Task 3.5: Content Script Integration**
- [ ] **content/index.ts**
  - Inject control bar into Meet page
  - Detect Meet page URL
  - Wait for page load
  - Create shadow DOM for isolation
  - Mount React component inside shadow DOM
- [ ] **ControlBar.tsx**
  - Same component used in content script
  - Styled to work in Meet page context
  - Communicate with background via messaging
- [ ] **Meet Page Detection**
  - Check URL: https://meet.google.com/*
  - Wait for video element to be present
  - Ensure page is ready before injection

**Task 3.6: Message Passing Implementation**
- [ ] **background/messaging.ts**
  - Set up chrome.runtime.onMessage listener
  - Handle all message types (START_RECORDING, PAUSE, RESUME, STOP, GET_STATUS, UPDATE_SETTINGS)
  - Implement message handlers
  - Send responses back to senders
- [ ] **Popup → Background**
  - Send START_RECORDING on button click
  - Send UPDATE_SETTINGS when settings change
  - Request GET_RECORDING_STATUS on popup open
  - Handle responses and update UI
- [ ] **Content Script → Background**
  - Send control commands from control bar
  - Receive status updates
  - Handle recording state changes
- [ ] **Background → Content Script**
  - Send INJECT_CONTROL_BAR message
  - Send REMOVE_CONTROL_BAR message
  - Send recording status updates

**Task 3.7: Chrome Storage Integration**
- [ ] Implement settings persistence
  - Save recording settings to chrome.storage.local
  - Load settings on extension load
  - Sync settings across popup and options page
- [ ] Implement state persistence
  - Save recording state to chrome.storage.local
  - Restore state on extension reload
  - Clear state on recording complete

**Task 3.8: Extension Build Configuration**
- [ ] Configure Vite for Chrome extension
  - Use vite-plugin-chrome-extension
  - Set up multiple entry points (popup, options, background, content)
  - Configure asset handling
  - Set up production build
- [ ] Test extension loading in Chrome
  - Load unpacked extension
  - Verify all pages load
  - Test hot reload during development
  - Fix any loading errors

#### 5.3.3 Deliverables
- ✅ Working Chrome extension (Manifest V3)
- ✅ Popup interface with recording controls
- ✅ Floating control bar overlay
- ✅ Options page with settings
- ✅ Content script injection working
- ✅ Message passing functional
- ✅ Extension loads without errors in Chrome
- ✅ All UI matches UIUX_PLAN.md specifications

#### 5.3.4 Validation Checklist
- [ ] Extension installs without errors
- [ ] Popup opens on icon click
- [ ] All popup states display correctly
- [ ] Source selector toggles work
- [ ] Settings checkboxes persist
- [ ] Control bar injects into Meet page
- [ ] Control bar is draggable
- [ ] Timer displays and updates
- [ ] Options page opens and saves settings
- [ ] Message passing works between components
- [ ] Chrome storage persists data
- [ ] No console errors in any extension page

#### 5.3.5 Reference
- UIUX_PLAN.md Section 4: Extension UI/UX Design
- PLAN_HANDOFF_FRONTEND.md Section 6.1: Chrome Extension Components
- PLAN_HANDOFF_FRONTEND.md Section 9.1: Chrome Extension Specific Notes
- Chrome Extension MV3 Documentation

---

### Phase 4: Dashboard Layout & Navigation

**Duration:** 3-4 days  
**Priority:** 🔴 Critical  
**Dependencies:** Phase 1 (Design System), Phase 2 (Shared Components)

#### 5.4.1 Objectives
- Build dashboard layout structure
- Implement sidebar navigation
- Implement header bar with search
- Set up React Router with route guards
- Implement responsive behavior for mobile/tablet/desktop

#### 5.4.2 Tasks

**Task 4.1: React Router Setup**
- [ ] Install react-router-dom
- [ ] Create router.tsx with route configuration
- [ ] Define routes:
  - / - Home (recordings list)
  - /recording/:id - Recording detail
  - /settings - User settings
  - /starred - Starred recordings
  - /shared - Shared recordings
  - /trash - Deleted recordings
  - /auth/signin - Sign in page
  - /auth/callback - OAuth callback
- [ ] Create ProtectedRoute component
- [ ] Implement auth guard logic
- [ ] Set up redirect to /auth/signin if not authenticated
- [ ] Store intended destination for redirect after login
- [ ] Implement RouterProvider in App.tsx

**Task 4.2: Dashboard Layout** (Section 5.1)
- [ ] **DashboardLayout.tsx**
  - Flexbox layout: Sidebar (left) + Main content (right)
  - Sidebar: 280px fixed width (expanded), 80px (collapsed)
  - Main content: flex-1 (fills remaining space)
  - Full viewport height: h-screen
  - Responsive behavior:
    - Desktop (>1024px): Always visible sidebar
    - Tablet (768-1024px): Collapsible sidebar
    - Mobile (<768px): Drawer overlay sidebar
  - Outlet for nested routes
- [ ] **MainContent Area**
  - Flexbox column: Header + Content area
  - Header: sticky positioning at top (sticky top-0)
  - Content: scrollable area (overflow-y-auto)
  - Padding: 24px (p-6)
  - Background: gray-50
  - Min-height to fill viewport
- [ ] **Responsive Container**
  - Create useMediaQuery custom hook
  - Detect breakpoints: mobile (<768px), tablet (768-1024px), desktop (>1024px)
  - Adjust layout based on current breakpoint
  - Show/hide elements conditionally
  - Mobile: hamburger menu, drawer sidebar, search icon
  - Tablet: toggle button, collapsible sidebar, full search
  - Desktop: full sidebar, all features visible

**Task 4.3: Sidebar Component** (Section 5.2)
- [ ] **Sidebar Container**
  - Fixed width: 280px (expanded), 80px (collapsed)
  - Full height: h-screen
  - Background: white
  - Border-right: 1px border-r border-gray-200
  - Flexbox column: flex flex-col
  - Sections: Logo (top), Navigation (middle, flex-1), User Profile (bottom)
  - Smooth transition: transition-all duration-300 ease-in-out
  - Z-index: z-40 (sidebar layer)
  - Position: fixed on mobile (with overlay)
- [ ] **SidebarLogo** (Section 5.2.1)
  - Container: padding py-6 px-4
  - Logo image: 32x32px
  - Text: "OpenMeet Recorder" (hidden when collapsed)
  - Flexbox: flex items-center gap-3
  - Border-bottom: border-b border-gray-200
  - Clickable: navigate to home on click
  - Cursor: cursor-pointer
  - Hover: subtle opacity change
- [ ] **NavigationMenu** (Section 5.2.2)
  - Container: flex-1 overflow-y-auto py-4
  - Nav items array with route config:
    ```typescript
    const navItems = [
      { path: '/', icon: Home, label: 'Home' },
      { path: '/recordings', icon: Video, label: 'Recordings', badge: count },
      { path: '/starred', icon: Star, label: 'Starred' },
      { path: '/shared', icon: Users, label: 'Shared' },
      { path: '/trash', icon: Trash, label: 'Trash' },
      { path: '/settings', icon: Settings, label: 'Settings' },
    ];
    ```
  - Each NavItem component:
    - Link wrapper with react-router-dom Link
    - Flexbox: flex items-center gap-3
    - Padding: px-4 py-3 mx-2
    - Border-radius: rounded-lg
    - Default state: text-gray-700 hover:bg-gray-100
    - Active state: bg-primary-50 text-primary-700 border-l-3 border-primary-600
    - Icon: 20x20px, strokeWidth: 2
    - Label: hidden when collapsed, show tooltip instead
    - Badge: optional, right-aligned, circle with count
  - Settings item: mt-auto (pushed to bottom before user profile)
- [ ] **UserProfile** (Section 5.2.3)
  - Container: border-t border-gray-200 p-4
  - Layout: flex items-center gap-3
  - Avatar component (40x40px)
  - User info (hidden when collapsed):
    - Name: text-sm font-medium text-gray-900
    - Email: text-xs text-gray-500 truncate
  - Dropdown trigger on click
  - DropdownMenu items:
    - My Account (icon: User)
    - Settings (icon: Settings)
    - Help & Support (icon: HelpCircle)
    - Divider
    - Sign Out (icon: LogOut, text-red-600)
  - Use shared Dropdown component
- [ ] **Collapse Toggle Button**
  - Position: absolute, right: -12px, top: 50%
  - Button: circular, 24x24px, bg-white, border, shadow
  - Icon: ChevronLeft (expanded), ChevronRight (collapsed)
  - Click handler: toggle sidebar state in UIStore
  - Smooth rotation animation
  - Hidden on mobile (use drawer instead)

**Task 4.4: Header Bar** (Section 5.3)
- [ ] **Header Container**
  - Sticky positioning: sticky top-0 z-30
  - Background: white bg-white
  - Border-bottom: border-b border-gray-200
  - Height: h-16 (64px)
  - Padding: px-6
  - Flexbox: flex items-center justify-between
  - Shadow on scroll: add shadow-sm when scrolled
- [ ] **Header Left Section**
  - Flexbox: flex items-center gap-4
  - **Sidebar Toggle (Mobile/Tablet)**:
    - Button with Menu icon
    - Only visible on mobile/tablet: lg:hidden
    - Click: open sidebar drawer
  - **Page Title**:
    - Dynamic based on current route
    - Text: text-lg font-semibold text-gray-900
    - Examples: "Recordings", "Recording Details", "Settings"
  - **Breadcrumb (Optional)**:
    - Show on detail pages
    - Format: Home / Recordings / [Title]
    - Separator: ChevronRight icon
    - Each segment clickable
- [ ] **Header Center Section**
  - SearchBar component
  - Max-width: max-w-lg (512px)
  - Hidden on mobile: hidden md:block
- [ ] **Header Right Section**
  - Flexbox: flex items-center gap-4
  - **Search Button (Mobile)**:
    - Only visible on mobile: md:hidden
    - Icon: Search
    - Click: open search modal
  - **Notification Bell**:
    - Icon button: rounded-lg p-2 hover:bg-gray-100
    - Icon: Bell (24x24px)
    - Badge: red dot if notifications exist
    - Click: open notifications dropdown
  - **Help Button**:
    - Icon: HelpCircle
    - Opens help modal or external docs
  - **User Avatar**:
    - Avatar component (32x32px)
    - Click: open user dropdown (same as sidebar)
    - Subtle ring on hover

**Task 4.5: SearchBar Component** (Section 5.3)
- [ ] **SearchBar.tsx**
  - Container: relative
  - Input element:
    - Width: w-full
    - Height: h-10 (40px)
    - Padding: pl-10 pr-10 (space for icons)
    - Border: border-2 border-gray-300
    - Border-radius: rounded-lg
    - Focus state: focus:border-primary-500 focus:ring-4 focus:ring-primary-500/20
    - Placeholder: "Search recordings..."
  - Search icon (left):
    - Position: absolute left-3 top-1/2 -translate-y-1/2
    - Icon: Search (20x20px)
    - Color: text-gray-400
  - Clear button (right):
    - Position: absolute right-3 top-1/2 -translate-y-1/2
    - Icon: X (16x16px)
    - Only show when input has value
    - Click: clear input value
    - Hover: text-gray-600
- [ ] **Debounced Search Logic**
  - Use custom useDebounce hook (500ms delay)
  - Update recordingsStore.filters.search on debounced value
  - Trigger recordings refetch
- [ ] **Keyboard Shortcuts**
  - Global shortcut: Cmd/Ctrl + K to focus search
  - Escape key: clear and blur input
  - Enter key: execute search immediately (skip debounce)
- [ ] **Search Results Dropdown (Optional)**
  - Show top 5 results below input
  - Click result: navigate to detail page
  - Keyboard navigation: Arrow keys + Enter
  - Close on outside click or Escape

**Task 4.6: Responsive Behavior Implementation**
- [ ] **Mobile Layout (<768px)**
  - Sidebar: drawer overlay (fixed position, slide from left)
  - Backdrop: dark overlay when drawer open
  - Header: hamburger menu + title + search icon + user avatar
  - Search: modal overlay when icon clicked
  - Padding: reduced to p-4
  - Touch-friendly: increase button sizes (44x44px minimum)
- [ ] **Tablet Layout (768-1024px)**
  - Sidebar: collapsible, starts collapsed
  - Toggle button visible
  - Full search bar in header
  - Adjust grid columns in content area
  - Padding: p-5
- [ ] **Desktop Layout (>1024px)**
  - Full sidebar always visible
  - Toggle to collapse available
  - All features visible
  - Optimal spacing: p-6
  - Multi-column layouts
- [ ] **useMediaQuery Hook**
  ```typescript
  export function useMediaQuery(query: string): boolean {
    const [matches, setMatches] = useState(false);
    
    useEffect(() => {
      const media = window.matchMedia(query);
      setMatches(media.matches);
      
      const listener = () => setMatches(media.matches);
      media.addEventListener('change', listener);
      return () => media.removeEventListener('change', listener);
    }, [query]);
    
    return matches;
  }
  
  // Usage
  const isMobile = useMediaQuery('(max-width: 768px)');
  const isTablet = useMediaQuery('(min-width: 768px) and (max-width: 1024px)');
  const isDesktop = useMediaQuery('(min-width: 1024px)');
  ```

**Task 4.7: Navigation State Management**
- [ ] Use useLocation hook from react-router-dom
- [ ] Track active route path
- [ ] Highlight active nav item based on current path
- [ ] Update document title based on route:
  ```typescript
  useEffect(() => {
    document.title = `${pageTitle} - OpenMeet Recorder`;
  }, [pageTitle]);
  ```
- [ ] Scroll to top on route change:
  ```typescript
  useEffect(() => {
    window.scrollTo(0, 0);
  }, [location.pathname]);
  ```
- [ ] Handle browser back/forward buttons (React Router handles automatically)
- [ ] Breadcrumb generation based on route hierarchy

**Task 4.8: Loading States & Transitions**
- [ ] **Layout Loading Skeleton**
  - Show while auth is being initialized
  - Sidebar skeleton: gray bars
  - Header skeleton
  - Content area skeleton
  - Fade in when loaded
- [ ] **Route Transition Loading**
  - Top progress bar (NProgress style)
  - Or spinner overlay
  - Show during lazy-loaded route loading
- [ ] **Smooth Animations**
  - Sidebar slide: transform translateX
  - Drawer backdrop: opacity fade
  - Nav items: hover lift
  - Search dropdown: fade + slide down

#### 5.4.3 Deliverables
- ✅ Complete dashboard layout structure
- ✅ Sidebar with navigation and user profile
- ✅ Header bar with search and actions
- ✅ React Router with protected routes
- ✅ Responsive behavior for all screen sizes
- ✅ Sidebar collapse/expand functionality
- ✅ Navigation state management
- ✅ Loading states and transitions
- ✅ Mobile drawer sidebar
- ✅ Keyboard shortcuts

#### 5.4.4 Validation Checklist
- [ ] Layout renders correctly on desktop, tablet, mobile
- [ ] Sidebar navigation works and highlights active route
- [ ] Sidebar collapse/expand animation is smooth
- [ ] Mobile drawer opens and closes with backdrop
- [ ] Header search updates filter state with debounce
- [ ] Protected routes redirect to sign in when not authenticated
- [ ] User profile dropdown opens and closes
- [ ] Breadcrumb navigation works on detail pages
- [ ] Keyboard shortcuts work (Cmd+K for search)
- [ ] Page title updates based on route
- [ ] Scroll to top on route change
- [ ] No layout shifts or flickering
- [ ] Touch targets are 44x44px minimum on mobile
- [ ] No console errors or warnings

#### 5.4.5 Reference
- UIUX_PLAN.md Section 5.1: Dashboard Layout
- UIUX_PLAN.md Section 5.2: Sidebar Components
- UIUX_PLAN.md Section 5.3: Header Bar
- PLAN_HANDOFF_FRONTEND.md Section 6.2.1: Layout Components
- PLAN_HANDOFF_FRONTEND.md Section 9.5: Responsive Design

---

### Phase 5: Recording List & Display

**Duration:** 4-5 days  
**Priority:** 🔴 Critical  
**Dependencies:** Phase 4 (Dashboard Layout)

#### 5.5.1 Objectives
- Implement recording cards for grid and list views
- Build comprehensive filtering system
- Implement sorting functionality
- Create search integration
- Build empty and loading states
- Integrate with Firestore for data fetching

#### 5.5.2 Tasks

**Task 5.1: Recording Card Component** (Section 5.4.1)
- [ ] **RecordingCard.tsx Base Structure**
  - Use shared Card component as base
  - Props interface:
    ```typescript
    interface RecordingCardProps {
      recording: Recording;
      view: 'grid' | 'list';
      onStar: (id: string) => void;
      onShare: (id: string) => void;
      onDelete: (id: string) => void;
    }
    ```
  - Click handler: navigate to `/recording/${id}`
  - Cursor: cursor-pointer
  - Transition: transition-all duration-300 ease-out

- [ ] **Grid View Card Layout**
  - Container: flex flex-col
  - Hover effects:
    - Transform: hover:-translate-y-1
    - Border: hover:border-primary-300
    - Shadow: hover:shadow-md
  - Structure: Thumbnail → Content → Footer

- [ ] **Thumbnail Section**
  - Container: relative, aspect-ratio-16/9, overflow-hidden
  - Image:
    - Object-fit: cover
    - Width: 100%
    - Height: 100%
    - Lazy loading: loading="lazy"
  - Placeholder:
    - Gray background with Video icon
    - Show when no thumbnail
    - Skeleton shimmer when loading
  - **Duration Badge** (bottom-right overlay):
    - Position: absolute bottom-2 right-2
    - Background: rgba(0, 0, 0, 0.8)
    - Color: text-white
    - Padding: px-2 py-1 (4px 8px)
    - Border-radius: rounded
    - Font: text-xs font-medium
    - Format: formatDuration(seconds) → "1:23:45" or "45:30"
  - **Status Indicator** (top-left overlay):
    - Position: absolute top-2 left-2
    - Size: w-6 h-6 (24x24px)
    - Border-radius: rounded-full
    - Status variants:
      - Processing: bg-blue-500 + Loader icon (spinning)
      - Completed: bg-green-500 + CheckCircle icon
      - Error: bg-red-500 + XCircle icon
    - Shadow: shadow-sm

- [ ] **Content Section**
  - Padding: p-4
  - **Title**:
    - Text: text-base font-semibold text-gray-900
    - Line clamp: 2 lines with ellipsis
    - Margin-bottom: mb-2
    - Hover: text-primary-700
  - **Tags** (if exists):
    - Show first 2-3 tags as small badges
    - Badge: text-xs px-2 py-1 rounded-full bg-gray-100 text-gray-700
    - If more tags: show "+N more" badge
    - Flexbox: flex flex-wrap gap-1 mb-2
  - **Metadata Row**:
    - Flexbox: flex items-center justify-between
    - Color: text-sm text-gray-600
    - **Date**:
      - Icon: Calendar (16px)
      - Text: formatted relative date ("2 days ago", "Jan 15, 2025")
      - Format: use date-fns library
    - **File Size**:
      - Icon: HardDrive (16px)
      - Text: formatted file size ("1.2 GB", "450 MB")
      - Format: formatBytes function

- [ ] **Action Buttons** (hover overlay)
  - Container: absolute top-2 right-2
  - Opacity: 0 on default, 1 on hover
  - Transition: opacity 200ms ease
  - Background: bg-white rounded-lg shadow-md p-1
  - Buttons layout: flex gap-1
  - **Star Button**:
    - Icon: Star (filled if starred, outline if not)
    - Size: w-8 h-8
    - Color: text-yellow-500 (if starred), text-gray-400 (if not)
    - Hover: bg-gray-100
    - onClick: prevent navigation, call onStar
  - **Share Button**:
    - Icon: Share2
    - Same styling as Star
    - onClick: open share modal
  - **More Button**:
    - Icon: MoreVertical
    - Opens dropdown menu with:
      - Download (icon: Download)
      - Edit details (icon: Edit)
      - Move to trash (icon: Trash, text-red-600)

- [ ] **List View Card Layout**
  - Container: flex flex-row items-center
  - Padding: p-4
  - Border: border border-gray-200 rounded-lg
  - Hover: hover:bg-gray-50
  - **Thumbnail** (left):
    - Fixed width: w-48 (192px)
    - Same 16:9 ratio and badges
    - Rounded: rounded-l-lg
  - **Content** (center, flex-1):
    - Padding: px-6
    - Title: larger text-lg
    - Description: show if exists, 2 lines clamp
    - Tags and metadata stacked vertically
  - **Actions** (right):
    - Always visible (not on hover)
    - Vertical button group
    - Fixed width: w-20
    - Gap: gap-2

**Task 5.2: Grid View Layout** (Section 5.4)
- [ ] **GridView.tsx Component**
  - Container: CSS Grid
  - Grid configuration:
    ```css
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 1.5rem; /* 24px */
    ```
  - Responsive columns:
    - Mobile (<640px): 1 column
    - Small tablet (640-768px): 2 columns
    - Tablet (768-1024px): 2-3 columns
    - Desktop (>1024px): 3-4 columns
    - Large desktop (>1536px): 4-5 columns
  - Min card width: 320px
  - Auto-fill ensures optimal column count

- [ ] **Staggered Animation on Load**
  - Use framer-motion or CSS animation
  - Each card fades in with slight delay
  - Delay calculation: index * 50ms
  - Animation: fadeInUp (opacity 0→1, translateY 20px→0px)
  - Duration: 300ms ease-out

**Task 5.3: List View Layout** (Section 5.4)
- [ ] **ListView.tsx Component**
  - Container: flex flex-col gap-4
  - Each card: full width
  - Divider between cards (optional)
  - Same staggered animation as grid

**Task 5.4: View Toggle Component** (Section 5.4)
- [ ] **ViewToggle.tsx**
  - Container: inline-flex rounded-lg bg-gray-100 p-1
  - Two buttons: Grid and List
  - Button structure:
    - Icon only: Grid (LayoutGrid icon), List (LayoutList icon)
    - Size: w-9 h-9 (36x36px)
    - Rounded: rounded-md
    - Active state: bg-white shadow text-primary-600
    - Inactive state: text-gray-600 hover:text-gray-900
    - Transition: all 200ms
  - Click handler:
    - Update UIStore view state
    - Persist to localStorage
    - Re-render with new view
  - Position: in header near filter button

**Task 5.5: Filter Controls** (Section 5.4)
- [ ] **FilterControls.tsx Button**
  - Button with icon and text
  - Icon: Filter (or SlidersHorizontal)
  - Text: "Filters"
  - Badge: show active filter count if > 0
    - Position: absolute -top-1 -right-1
    - Size: w-5 h-5
    - Background: bg-primary-600
    - Text: text-xs text-white
    - Rounded: rounded-full
  - Variant: secondary
  - onClick: open filter panel (UIStore.openFilterPanel())
  - Position: left side of page, inline with view toggle

**Task 5.6: Filter Panel Component** (Section 5.4)
- [ ] **FilterPanel.tsx Drawer**
  - Use shared Drawer component
  - Slide in from right
  - Width: w-80 (320px)
  - Controlled by UIStore.filterPanelOpen
  - Background: bg-white
  - Shadow: shadow-xl
  - Z-index: z-50

- [ ] **Panel Header**
  - Flexbox: flex items-center justify-between
  - Padding: p-6 border-b border-gray-200
  - Title: "Filters" (text-lg font-semibold)
  - Close button: X icon

- [ ] **Panel Content** (scrollable)
  - Padding: p-6
  - Sections with dividers between
  - Each section: title + controls + mb-6

- [ ] **Date Range Filter Section**
  - Title: "Date Range" (text-sm font-medium text-gray-700 mb-3)
  - Radio button group:
    - Options: All time, Today, This week, This month, This year, Custom
    - Vertical stack with gap-2
    - Use shared Radio component
  - Custom date picker (if Custom selected):
    - Start date and end date inputs
    - Use HTML date input or date picker library

- [ ] **Duration Filter Section**
  - Title: "Duration"
  - Checkbox group (multi-select):
    - Options: Less than 30 min, 30-60 min, Over 60 min
    - Vertical stack
    - Use shared Checkbox component

- [ ] **File Size Filter Section**
  - Title: "File Size"
  - Range slider:
    - Min: 0 MB, Max: dynamic based on largest file
    - Display current range: "100 MB - 2 GB"
    - Use range input or slider library
    - Alternative: checkbox options (<100 MB, 100-500 MB, 500MB-1GB, >1GB)

- [ ] **Status Filter Section**
  - Title: "Status"
  - Checkbox group:
    - Processing (with spinner icon)
    - Completed (with checkmark icon)
    - Error (with X icon)

- [ ] **Starred Only Toggle**
  - Title: "Show Only Starred"
  - Toggle switch
  - Use shared Toggle component

- [ ] **Tags Filter Section**
  - Title: "Tags"
  - Multi-select dropdown or checkbox list
  - Show all available tags from recordings
  - Search within tags
  - Selected tags: show as dismissible chips

- [ ] **Panel Footer**
  - Sticky at bottom
  - Padding: p-6 border-t border-gray-200
  - Background: bg-gray-50
  - Buttons layout: flex gap-3
  - **Clear All Button**:
    - Variant: ghost
    - Text: "Clear all"
    - onClick: reset all filters to default
  - **Apply Button**:
    - Variant: primary
    - Text: "Apply filters"
    - onClick: update recordingsStore.filters, close panel, refetch
    - Full width or flex-1

**Task 5.7: Active Filter Tags** (Section 5.4)
- [ ] **ActiveFilterTags.tsx Component**
  - Display below filter button
  - Flexbox: flex flex-wrap gap-2 mt-3
  - Show only if filters are active
  - Animation: fade in when filters applied

- [ ] **Filter Tag/Chip**
  - Structure: label + value + remove button
  - Examples:
    - "Date: This week"
    - "Duration: <30 min"
    - "Status: Completed"
    - "Tag: Meeting"
  - Badge styling:
    - Background: bg-primary-100
    - Text: text-primary-700
    - Padding: px-3 py-1
    - Rounded: rounded-full
    - Font: text-sm
  - Remove button:
    - Icon: X (12px)
    - Click: remove specific filter
    - Hover: bg-primary-200

- [ ] **Clear All Link**
  - Show if multiple filters active
  - Text: "Clear all filters"
  - Color: text-primary-600 hover:text-primary-700
  - Underline on hover
  - Click: clear all filters

**Task 5.8: Sort Dropdown** (Section 5.4)
- [ ] **SortDropdown.tsx Component**
  - Dropdown button:
    - Text: "Sort by: [Current Option]"
    - Icon: ChevronDown
    - Variant: secondary
    - Size: medium
  - Dropdown menu items:
    ```typescript
    const sortOptions = [
      { value: 'date-desc', label: 'Date (newest first)', icon: ArrowDown },
      { value: 'date-asc', label: 'Date (oldest first)', icon: ArrowUp },
      { value: 'name-asc', label: 'Name (A-Z)', icon: ArrowUp },
      { value: 'name-desc', label: 'Name (Z-A)', icon: ArrowDown },
      { value: 'duration', label: 'Duration (longest)', icon: Clock },
      { value: 'size', label: 'File size (largest)', icon: HardDrive },
    ];
    ```
  - Active option:
    - Checkmark icon before label
    - Bold text
  - Click handler:
    - Update recordingsStore.sort
    - Re-sort recordings array
    - Close dropdown
  - Position: right side of page near view toggle

**Task 5.9: Empty State** (Section 5.4)
- [ ] **EmptyState.tsx Component**
  - Container: centered, min-height: 400px
  - Flexbox: flex flex-col items-center justify-center
  - Padding: py-12
  - **Icon/Illustration**:
    - Large icon: Video or FileVideo (64x64px)
    - Color: text-gray-400
    - Or custom illustration
  - **Title**:
    - Text: "No recordings yet"
    - Font: text-xl font-semibold text-gray-900
    - Margin: mt-4
  - **Description**:
    - Text: "Record your first Google Meet session to get started"
    - Font: text-base text-gray-600
    - Max-width: max-w-md text-center
    - Margin: mt-2
  - **Action Button** (if no recordings at all):
    - Text: "Start Recording"
    - Variant: primary
    - Icon: Play
    - Click: open extension or show instructions
    - Margin: mt-6
  - **Empty Filters State** (if filters applied but no results):
    - Title: "No recordings match your filters"
    - Description: "Try adjusting your filters or clear them to see all recordings"
    - Button: "Clear filters"

**Task 5.10: Skeleton Loaders** (Section 5.4)
- [ ] **SkeletonCard.tsx Component**
  - Mimic RecordingCard structure
  - Use shared Skeleton component
  - **Thumbnail skeleton**:
    - Rectangle with 16:9 ratio
    - Shimmer animation
    - Background: bg-gray-200
  - **Content skeleton**:
    - Title: 2 lines, widths: 100%, 60%
    - Metadata: 1 line, width: 80%
    - Gap between elements
  - Shimmer effect:
    ```css
    @keyframes shimmer {
      0% { background-position: -1000px 0; }
      100% { background-position: 1000px 0; }
    }
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 1000px 100%;
    animation: shimmer 2s infinite;
    ```

- [ ] **Loading State Display**
  - Show 6-8 skeleton cards in grid
  - Or 4-6 skeleton cards in list view
  - Staggered animation: each skeleton appears with 50ms delay
  - Fade out when real data loads
  - Transition: smooth crossfade

**Task 5.11: Firestore Integration**
- [ ] **services/firestore.ts Functions**
  - **getRecordings function**:
    ```typescript
    export async function getRecordings(userId: string): Promise<Recording[]> {
      const q = query(
        collection(db, 'recordings'),
        where('userId', '==', userId),
        where('deleted', '==', false),
        orderBy('createdAt', 'desc')
      );
      const snapshot = await getDocs(q);
      return snapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data(),
        createdAt: doc.data().createdAt.toDate(),
        updatedAt: doc.data().updatedAt.toDate(),
      })) as Recording[];
    }
    ```
  - **Pagination support** (optional):
    - Add limit and startAfter parameters
    - Implement infinite scroll or load more button
  - **Real-time updates** (optional):
    ```typescript
    export function subscribeToRecordings(
      userId: string,
      callback: (recordings: Recording[]) => void
    ) {
      const q = query(
        collection(db, 'recordings'),
        where('userId', '==', userId),
        where('deleted', '==', false),
        orderBy('createdAt', 'desc')
      );
      return onSnapshot(q, (snapshot) => {
        const recordings = snapshot.docs.map(doc => ({
          id: doc.id,
          ...doc.data(),
        })) as Recording[];
        callback(recordings);
      });
    }
    ```

- [ ] **hooks/useRecordings.ts Custom Hook**
  ```typescript
  export function useRecordings() {
    const { user } = useAuthStore();
    const { recordings, loading, fetchRecordings } = useRecordingsStore();
    
    useEffect(() => {
      if (user) {
        fetchRecordings();
      }
    }, [user, fetchRecordings]);
    
    return { recordings, loading };
  }
  ```

- [ ] **Implement in Home.tsx Page**
  - Call useRecordings hook
  - Show skeleton loaders while loading
  - Apply client-side filters and sorting
  - Render GridView or ListView based on view state
  - Show EmptyState if no recordings

**Task 5.12: Search Functionality**
- [ ] **Header SearchBar Integration**
  - Debounce input: 500ms delay
  - Update recordingsStore.filters.search
  - Filter recordings by title (client-side)
  - Case-insensitive search
  - Optional: fuzzy search using fuse.js
  - Highlight search term in results (optional)

- [ ] **Search Logic in Store**
  ```typescript
  const filteredRecordings = recordings.filter(recording => {
    if (filters.search) {
      return recording.title.toLowerCase().includes(filters.search.toLowerCase());
    }
    return true;
  });
  ```

- [ ] **No Results State**
  - Show when search returns empty
  - Message: "No recordings found for '[search term]'"
  - Button: "Clear search"

#### 5.5.3 Deliverables
- ✅ Recording cards (grid and list views)
- ✅ View toggle functionality
- ✅ Comprehensive filter panel with all filter types
- ✅ Active filter tags display
- ✅ Sort dropdown with all options
- ✅ Search functionality with debounce
- ✅ Empty states (no recordings, no results)
- ✅ Skeleton loading states
- ✅ Firestore data integration
- ✅ Client-side filtering and sorting
- ✅ Real-time updates (optional)

#### 5.5.4 Validation Checklist
- [ ] Recording cards display correctly in both views
- [ ] Card hover effects are smooth
- [ ] View toggle switches between grid and list
- [ ] Filter panel opens and closes with animation
- [ ] All filter options work correctly
- [ ] Multiple filters can be applied simultaneously
- [ ] Active filter tags display and are dismissible
- [ ] Clear all filters resets to default state
- [ ] Sort dropdown changes order correctly
- [ ] Search filters recordings in real-time
- [ ] Search is debounced (doesn't trigger on every keystroke)
- [ ] Empty state shows when no recordings
- [ ] "No results" state shows when filters/search return nothing
- [ ] Skeleton loaders show during initial fetch
- [ ] Staggered animation on card load
- [ ] Data fetches from Firestore successfully
- [ ] Star/unstar functionality works
- [ ] Share and delete actions work
- [ ] Pagination works (if implemented)
- [ ] Real-time updates work (if implemented)
- [ ] No console errors or warnings
- [ ] Responsive on all screen sizes

#### 5.5.5 Reference
- UIUX_PLAN.md Section 5.4: Recording Display
- PLAN_HANDOFF_FRONTEND.md Section 6.2.2: Recording Display Components
- Firebase Firestore Documentation

---

### Phase 6: Recording Detail & Playback

**Duration:** 5-6 days  
**Priority:** 🔴 Critical  
**Dependencies:** Phase 4 (Dashboard Layout), Phase 5 (Recording List)

#### 5.6.1 Objectives
- Build complete recording detail page
- Implement custom video player with full controls
- Build interactive transcript with video synchronization
- Implement AI summary display with all sections
- Create editable details tab
- Integrate video playback from Google Drive
- Handle all data fetching and updates

#### 5.6.2 Tasks

**Task 6.1: Detail Page Layout** (Section 5.5.1)
- [ ] **Detail.tsx Page Component**
  - Get recording ID from route params: `const { id } = useParams()`
  - Fetch recording data on mount
  - State management: loading, error, recording data
  - Handle not found error (404)
  - Layout: full-width stacked sections
  - Padding: p-6

- [ ] **Breadcrumb Navigation**
  - Container: flex items-center gap-2 text-sm mb-4
  - Segments:
    - Link: Home (/) → text-gray-600 hover:text-gray-900
    - Separator: ChevronRight icon (16px, text-gray-400)
    - Link: Recordings (/) → text-gray-600 hover:text-gray-900
    - Separator: ChevronRight
    - Current: Recording title → text-gray-900 font-medium truncate
  - Each segment clickable except current
  - Max-width with ellipsis for long titles

- [ ] **Action Bar**
  - Position: absolute top-6 right-6 (or sticky top-0)
  - Flexbox: flex items-center gap-3
  - Z-index: z-10
  - Buttons (icon + label):
    - Download (icon: Download, variant: secondary)
    - Share (icon: Share2, variant: secondary)
    - More (icon: MoreVertical, opens dropdown)
      - Edit details
      - Copy link
      - Move to trash (text-red-600)
  - Responsive: stack vertically on mobile

- [ ] **Recording Info Header**
  - Container: mb-6
  - **Title**:
    - Display mode: text-2xl font-bold text-gray-900
    - Editable inline: double-click or click edit icon
    - Input mode: large input with save/cancel buttons
    - Validation: required, max 200 characters
    - Save: update Firestore, show toast
  - **Metadata Row**:
    - Flexbox: flex items-center flex-wrap gap-4 mt-3
    - Color: text-sm text-gray-600
    - Items:
      - Date: Calendar icon + formatted date
      - Duration: Clock icon + formatted duration
      - File Size: HardDrive icon + formatted size
      - Status: badge (Processing/Completed/Error)
    - Each item: flex items-center gap-1

- [ ] **Main Content Area**
  - Video player section: max-w-5xl mx-auto mb-8
  - Tabs section: full width

**Task 6.2: Video Player Component** (Section 5.5.2)
- [ ] **VideoPlayer.tsx Container**
  - Aspect ratio: aspect-video (16:9)
  - Max-width: max-w-5xl
  - Margin: mx-auto
  - Background: bg-black
  - Border-radius: rounded-lg overflow-hidden
  - Position: relative (for absolute overlays)

- [ ] **HTML5 Video Element**
  - `<video>` element
  - Controls: hide native controls (`controls={false}`)
  - Ref: useRef for direct DOM manipulation
  - Source: signed URL from Google Drive
  - Attributes:
    - playsInline
    - preload="metadata"
    - poster={thumbnailUrl}
  - Event listeners:
    - onTimeUpdate
    - onDurationChange
    - onPlay
    - onPause
    - onEnded
    - onVolumeChange
    - onWaiting
    - onCanPlay
    - onLoadedMetadata
    - onError

- [ ] **useVideoPlayer Custom Hook**
  ```typescript
  interface VideoPlayerState {
    playing: boolean;
    currentTime: number;
    duration: number;
    volume: number;
    muted: boolean;
    buffering: boolean;
    playbackRate: number;
    quality: string;
    fullscreen: boolean;
  }
  
  export function useVideoPlayer(videoRef: RefObject<HTMLVideoElement>) {
    const [state, setState] = useState<VideoPlayerState>({
      playing: false,
      currentTime: 0,
      duration: 0,
      volume: 1,
      muted: false,
      buffering: false,
      playbackRate: 1,
      quality: 'auto',
      fullscreen: false,
    });
    
    // Actions
    const play = () => videoRef.current?.play();
    const pause = () => videoRef.current?.pause();
    const togglePlay = () => (state.playing ? pause() : play());
    const seek = (time: number) => {
      if (videoRef.current) videoRef.current.currentTime = time;
    };
    const setVolume = (volume: number) => {
      if (videoRef.current) {
        videoRef.current.volume = volume;
        setState(prev => ({ ...prev, volume }));
      }
    };
    const toggleMute = () => {
      if (videoRef.current) {
        videoRef.current.muted = !state.muted;
        setState(prev => ({ ...prev, muted: !prev.muted }));
      }
    };
    const setPlaybackRate = (rate: number) => {
      if (videoRef.current) {
        videoRef.current.playbackRate = rate;
        setState(prev => ({ ...prev, playbackRate: rate }));
      }
    };
    const toggleFullscreen = () => {
      // Implement fullscreen logic
    };
    
    // Event listeners setup in useEffect
    
    return { state, play, pause, togglePlay, seek, setVolume, toggleMute, setPlaybackRate, toggleFullscreen };
  }
  ```

- [ ] **PlayerControls Overlay**
  - Position: absolute inset-0
  - Pointer events: pointer-events-none on container, pointer-events-auto on controls
  - Show/hide logic:
    - Show on mouse move
    - Show on touch
    - Hide after 3 seconds of inactivity
    - Always show when paused
  - Fade animation: transition-opacity duration-200
  - Gradient background at bottom:
    - `background: linear-gradient(to top, rgba(0,0,0,0.8), transparent)`
    - Height: bottom 120px
  - Z-index: z-10

- [ ] **Center Play Button Overlay**
  - Position: absolute inset-0 flex items-center justify-center
  - Show only when paused and not buffering
  - Button:
    - Size: w-18 h-18 (72px)
    - Border-radius: rounded-full
    - Background: bg-white/90 hover:bg-white
    - Icon: Play (48px, text-gray-900)
    - Shadow: shadow-xl
    - Transition: transform and opacity
    - Hover: scale-110
  - Click: play video
  - Fade out when playing

- [ ] **Buffering Spinner Overlay**
  - Position: absolute inset-0 flex items-center justify-center
  - Show only when buffering
  - Spinner component: large size
  - Background: bg-black/50
  - Text: "Loading..." (optional)
  - Z-index: z-20

**Task 6.3: Progress Bar Component** (Section 5.5.2)
- [ ] **ProgressBar.tsx**
  - Container: absolute bottom-0 left-0 right-0
  - Height: h-1 (4px), hover:h-2 (8px)
  - Transition: height 150ms
  - Cursor: cursor-pointer
  - **Track (background)**:
    - Width: 100%
    - Background: bg-gray-700
  - **Buffered Progress**:
    - Position: absolute, same height as track
    - Width: calculated from video.buffered
    - Background: bg-gray-500
    - Z-index: 1
  - **Played Progress**:
    - Position: absolute, same height as track
    - Width: (currentTime / duration) * 100%
    - Background: bg-primary-600
    - Z-index: 2
  - **Thumb/Scrubber**:
    - Position: absolute, right edge of played progress
    - Size: w-3 h-3 (12px), hover:w-4 hover:h-4 (16px)
    - Border-radius: rounded-full
    - Background: bg-white
    - Shadow: shadow-md
    - Opacity: 0 default, 1 on hover
    - Transition: all 150ms
  - **Time Tooltip on Hover**:
    - Position: absolute, above cursor
    - Show time at hover position
    - Format: HH:MM:SS or MM:SS
    - Background: bg-black/80 text-white
    - Padding: px-2 py-1
    - Rounded: rounded
    - Font: text-xs
    - Arrow pointing down
  - **Click/Drag to Seek**:
    - onClick: calculate time from click position, seek to time
    - onMouseDown + onMouseMove: drag to seek
    - Show time tooltip during drag

**Task 6.4: Control Buttons Layout** (Section 5.5.2)
- [ ] **Controls Container**
  - Position: absolute bottom-0 left-0 right-0
  - Padding: p-4
  - Background: gradient (handled by parent overlay)
  - Flexbox: flex items-center justify-between
  - Gap: gap-4
  - Z-index: z-20

- [ ] **Left Section**
  - Flexbox: flex items-center gap-3
  - **Play/Pause Button**:
    - Icon: Play or Pause (24px)
    - Size: w-10 h-10 (40px)
    - Background: transparent hover:bg-white/20
    - Color: text-white
    - Rounded: rounded-full
    - Transition: all 150ms
  - **Volume Control** (VolumeControl component)
  - **Time Display**:
    - Format: "00:00 / 00:00" (currentTime / duration)
    - Font: text-sm font-medium text-white
    - Monospace font (optional)
    - Example: "05:23 / 45:30"

- [ ] **Center Section**
  - Flexbox: flex items-center gap-2
  - **Skip Backward Button**:
    - Icon: RotateCcw (20px)
    - Label: "-10s" or just icon
    - Click: seek backward 10 seconds
  - **Skip Forward Button**:
    - Icon: RotateCw (20px)
    - Label: "+10s"
    - Click: seek forward 10 seconds
  - Same styling as play button

- [ ] **Right Section**
  - Flexbox: flex items-center gap-2
  - **Playback Speed Button**:
    - Text: "1x" (current speed)
    - Opens dropdown with options: 0.25x, 0.5x, 0.75x, 1x, 1.25x, 1.5x, 1.75x, 2x
    - Active option: checkmark
    - Click option: set playback rate
  - **Quality Selector Button** (if multiple qualities available):
    - Text: "1080p" (current quality)
    - Opens dropdown: Auto, 1080p, 720p, 480p, 360p
    - Not implemented if using direct Drive link
  - **Settings Button**:
    - Icon: Settings (20px)
    - Opens dropdown with:
      - Playback speed
      - Quality
      - Keyboard shortcuts info
  - **Picture-in-Picture Button**:
    - Icon: PictureInPicture2 (20px)
    - Click: enter PIP mode
    - Browser API: document.pictureInPictureElement
  - **Fullscreen Button**:
    - Icon: Maximize or Minimize (20px)
    - Toggle fullscreen on click
    - Use Fullscreen API

**Task 6.5: Volume Control Component**
- [ ] **VolumeControl.tsx**
  - Container: relative, flex items-center
  - **Volume Icon Button**:
    - Icons based on volume level:
      - volume === 0 or muted: VolumeX
      - volume < 0.5: Volume1
      - volume >= 0.5: Volume2
    - Size: w-10 h-10
    - Click: toggle mute
  - **Volume Slider** (appears on hover):
    - Position: absolute, bottom: full, left: 1/2, -translate-x-1/2
    - Orientation: vertical
    - Height: h-24 (96px)
    - Background: bg-black/80
    - Padding: p-2
    - Rounded: rounded-lg
    - Show on hover with transition
    - Input type="range":
      - Orientation: vertical (CSS transform)
      - Min: 0, Max: 100
      - Value: volume * 100
      - onChange: setVolume(value / 100)
    - Custom slider styling to match theme

**Task 6.6: Keyboard Shortcuts**
- [ ] **Implement Keyboard Event Listeners**
  - useEffect to add/remove event listeners
  - Check if video player container is focused (or global)
  - Shortcuts:
    - **Space** or **K**: Toggle play/pause
    - **Arrow Left**: Skip backward 5 seconds
    - **Arrow Right**: Skip forward 5 seconds
    - **Arrow Up**: Volume up 10%
    - **Arrow Down**: Volume down 10%
    - **M**: Toggle mute
    - **F**: Toggle fullscreen
    - **0-9**: Jump to 0%-90% of video
    - **J**: Skip backward 10 seconds
    - **L**: Skip forward 10 seconds
    - **<** or **>**: Decrease/increase playback speed
  - Prevent default behavior for these keys
  - Show toast with shortcut hint (optional)

**Task 6.7: Transcript View Component** (Section 5.5.3)
- [ ] **TranscriptView.tsx Container**
  - Card container: bg-white rounded-lg border border-gray-200
  - Padding: p-6
  - Max-height: max-h-[600px]
  - Overflow: overflow-y-auto
  - Custom scrollbar styling

- [ ] **Transcript Header**
  - Flexbox: flex items-center justify-between mb-4
  - Border-bottom: border-b border-gray-200 pb-4
  - **Title**: "Transcript" (text-lg font-semibold)
  - **Actions**:
    - Search button (icon: Search)
    - Copy button (icon: Copy, tooltip: "Copy transcript")
    - Download button (icon: Download, tooltip: "Download as .txt")
    - Gap: gap-2

- [ ] **TranscriptSearch Component** (toggleable)
  - Container: animate slide down when search button clicked
  - Padding: pb-4
  - **Search Input**:
    - Icon: Search (left)
    - Placeholder: "Search in transcript..."
    - Clear button (X) when has value
    - Auto-focus when opened
  - **Search Navigation**:
    - Result count: "3 of 10 results"
    - Previous button: ChevronUp icon
    - Next button: ChevronDown icon
    - Highlight matches in transcript entries
  - **Close Search**:
    - Escape key or close button
    - Clear highlights
    - Hide search UI

- [ ] **Transcript Entries List**
  - Container: space-y-0
  - Each entry: TranscriptEntry component
  - Dividers between entries
  - Auto-scroll to active entry

- [ ] **TranscriptEntry Component**
  - Props: entry, isActive, onClick, searchTerm (for highlighting)
  - Layout: grid grid-cols-[80px_100px_1fr] gap-4
  - Padding: py-3 px-4
  - Border-bottom: border-b border-gray-100 last:border-0
  - Hover: bg-gray-50
  - Active (synced with video):
    - Background: bg-primary-50
    - Border-left: border-l-3 border-primary-600
    - Padding-left: pl-[13px] (to account for border)
  - Cursor: cursor-pointer
  - Transition: all 150ms
  - **Timestamp Column**:
    - Width: 80px
    - Font: text-sm font-medium text-primary-600
    - Format: "00:05:23"
    - Clickable
    - Hover: underline
  - **Speaker Column**:
    - Width: 100px
    - Font: text-sm font-medium text-gray-700
    - Truncate if too long
    - Examples: "Speaker 1", "John Doe", "You"
  - **Text Column**:
    - Flex: flex-1
    - Font: text-sm text-gray-900 leading-relaxed
    - Preserve line breaks if multi-line
    - Highlight search matches: bg-yellow-200

- [ ] **Click Timestamp to Jump**
  - onClick handler: navigate video to timestamp
  - Call videoPlayer.seek(timestamp)
  - Play video if paused
  - Scroll transcript entry into view

- [ ] **Active Entry Auto-Scroll**
  - useEffect watching currentTime
  - Find entry where timestamp <= currentTime < (timestamp + duration)
  - Set as active entry
  - Scroll into view with smooth behavior:
    ```typescript
    entryRef.current?.scrollIntoView({
      behavior: 'smooth',
      block: 'center',
    });
    ```

- [ ] **Empty State**
  - Show when no transcript available
  - Icon: FileText (48px, text-gray-400)
  - Title: "Transcript not available"
  - Description: "The transcript is still being processed" or "No transcript for this recording"
  - Centered in container

- [ ] **Copy Transcript Function**
  - Format entire transcript as plain text
  - Format: "[HH:MM:SS] Speaker: Text\n"
  - Copy to clipboard: navigator.clipboard.writeText()
  - Show success toast: "Transcript copied to clipboard"

- [ ] **Download Transcript Function**
  - Format options: .txt or .srt
  - **TXT format**: Plain text with timestamps
  - **SRT format**: SubRip subtitle format
    ```
    1
    00:00:05,230 --> 00:00:08,450
    Speaker 1: This is the transcript text.
    
    2
    00:00:08,450 --> 00:00:12,100
    Speaker 2: More transcript text here.
    ```
  - Create blob and download:
    ```typescript
    const blob = new Blob([content], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `${recording.title}-transcript.txt`;
    a.click();
    URL.revokeObjectURL(url);
    ```

**Task 6.8: AI Summary View Component** (Section 5.5.4)
- [ ] **AISummaryView.tsx Container**
  - Card container: bg-white rounded-lg border border-gray-200
  - Padding: p-6
  - Space between sections: space-y-6

- [ ] **Summary Header**
  - Flexbox: flex items-center gap-3 mb-6
  - Icon: Sparkles (24px, text-primary-600)
  - Title: "AI-Generated Summary" (text-lg font-semibold)
  - Description: text-sm text-gray-600 (optional)

- [ ] **Overview Section**
  - Title: "Overview" (text-base font-semibold mb-3)
  - Content: paragraph text
  - Font: text-base text-gray-700 leading-relaxed
  - Max 3-4 sentences summarizing the meeting

- [ ] **Key Points Section**
  - Title: "Key Points" (text-base font-semibold mb-3)
  - List: ul with custom bullets
  - Each item:
    - Flexbox: flex gap-2
    - Icon: CheckCircle2 (16px, text-green-500)
    - Text: text-sm text-gray-700
  - Spacing: space-y-2

- [ ] **Action Items Section**
  - Title: "Action Items" (text-base font-semibold mb-3)
  - List of actionable tasks
  - Each item:
    - Checkbox: interactive (can mark complete)
    - Text: action description
    - Assignee: avatar + name (if available)
    - Due date: calendar icon + date (if available)
    - Layout: flex items-start gap-3
  - Completed items: line-through text-gray-500

- [ ] **Topics/Tags Section**
  - Title: "Topics Discussed" (text-base font-semibold mb-3)
  - Pills/badges for each topic
  - Badge styling:
    - Background: varied colors (blue, green, purple, orange)
    - Padding: px-3 py-1
    - Rounded: rounded-full
    - Font: text-sm
  - Layout: flex flex-wrap gap-2

- [ ] **Participants Section** (optional)
  - Title: "Participants" (text-base font-semibold mb-3)
  - Grid: grid grid-cols-2 gap-4
  - Each participant card:
    - Avatar (40px)
    - Name: text-sm font-medium
    - Role/Email: text-xs text-gray-600 (if available)
    - Layout: flex items-center gap-3

- [ ] **Action Bar at Bottom**
  - Container: flex items-center gap-3 mt-6 pt-6 border-t border-gray-200
  - Buttons:
    - Regenerate (icon: RefreshCw, variant: secondary)
    - Copy (icon: Copy, variant: secondary)
    - Export (icon: Download, variant: secondary)
    - Share (icon: Share2, variant: primary)

- [ ] **Loading State**
  - Skeleton blocks for each section
  - Shimmer animation
  - Show while summary is being generated
  - Text: "Generating AI summary..."

- [ ] **Error State**
  - Icon: AlertCircle (48px, text-red-500)
  - Title: "Failed to generate summary"
  - Description: error message
  - Button: "Try again" (calls regenerate)

**Task 6.9: Details Tab Component** (Section 5.5)
- [ ] **DetailsTab.tsx Container**
  - Card container: bg-white rounded-lg border border-gray-200
  - Padding: p-6
  - Sections divided by dividers

- [ ] **Property Row Component**
  - Layout: grid grid-cols-[40%_60%] gap-4
  - Padding: py-3
  - Border-bottom: border-b border-gray-100 last:border-0
  - **Label (left)**:
    - Font: text-sm font-medium text-gray-700
    - Examples: "Recording Name", "Date & Time", "Duration"
  - **Value (right)**:
    - Font: text-sm text-gray-900
    - Editable fields: show edit icon on hover
    - Copy icon for copyable values (IDs, URLs)

- [ ] **Editable Fields**
  - **Recording Name**:
    - Display: text with edit icon
    - Edit mode: input field with save/cancel
    - Validation: required, max length
    - Save: update Firestore
  - **Description**:
    - Display: text (multi-line if long)
    - Edit mode: textarea (4-6 rows)
    - Placeholder: "Add a description..."
    - Save: update Firestore

- [ ] **Static Properties**
  - Meeting URL: link (external icon, opens in new tab)
  - Date & Time: formatted date
  - Duration: formatted duration
  - File Size: formatted size
  - Video Quality: 1080p/720p/etc.
  - Status: badge
  - Google Drive File ID: copyable text (small, monospace)
  - Created At: relative time ("2 days ago") + absolute tooltip
  - Last Modified: same format

- [ ] **Tags Management Section**
  - Title: "Tags" (text-sm font-medium text-gray-700 mb-2)
  - Tags display: flex flex-wrap gap-2
  - Each tag: badge with remove button (X icon)
  - Add tag input:
    - Input: text input with "Add tag" placeholder
    - Enter key or + button to add
    - Validation: no duplicates, max length
    - Update Firestore on add/remove
  - Suggestions: show popular tags (optional)

- [ ] **Privacy Settings Section**
  - Title: "Privacy" (text-sm font-medium text-gray-700 mb-2)
  - Radio button group:
    - Private: only you can access
    - Shared: anyone with the link can view
    - Public: listed in public recordings (if applicable)
  - Description under each option
  - Update on change

- [ ] **Danger Zone Section**
  - Container: mt-6 p-4 bg-red-50 border border-red-200 rounded-lg
  - Title: "Danger Zone" (text-sm font-semibold text-red-900)
  - **Delete Recording Button**:
    - Variant: error (red)
    - Text: "Delete Recording"
    - Icon: Trash
    - Click: open confirmation modal
  - **Confirmation Modal**:
    - Title: "Delete Recording?"
    - Message: "This action cannot be undone. The recording will be permanently deleted."
    - Checkbox: "Also delete from Google Drive" (optional)
    - Buttons: Cancel (secondary), Delete (error)
    - On confirm: delete from Firestore, navigate to home, show toast

**Task 6.10: Data Integration**
- [ ] **Fetch Recording Data**
  - services/firestore.ts: `getRecording(id: string)`
  - Fetch from Firestore by document ID
  - Include transcript and summary subcollections/fields
  - Handle not found: throw error or return null
  - Handle errors: log and rethrow

- [ ] **Get Video URL from Google Drive**
  - services/storage.ts: `getVideoUrl(recordingId: string)`
  - Get signed URL for video playback
  - URL may expire: implement refresh logic
  - Cache URL with expiration timestamp
  - Fallback if URL fails

- [ ] **Update Recording**
  - services/firestore.ts: `updateRecording(id: string, data: Partial<Recording>)`
  - Update document in Firestore
  - Fields: title, description, tags, privacy, etc.
  - Return updated document
  - Show success toast on update
  - Handle errors gracefully

- [ ] **Delete Recording**
  - services/firestore.ts: `deleteRecording(id: string)`
  - Soft delete: set `deleted: true` field
  - Or hard delete: delete document
  - Backend should handle Google Drive deletion
  - Navigate to home after delete
  - Show success toast

- [ ] **Toggle Star**
  - Update `starred` field
  - Optimistic update in UI
  - Sync with Firestore

- [ ] **Add/Remove Tags**
  - Update `tags` array
  - Immediate UI update
  - Debounced Firestore update

#### 5.6.3 Deliverables
- ✅ Complete recording detail page
- ✅ Custom video player with all controls
- ✅ Seekable progress bar with time tooltip
- ✅ Volume control with slider
- ✅ Playback speed and quality selection
- ✅ Keyboard shortcuts
- ✅ Picture-in-Picture and fullscreen support
- ✅ Interactive transcript with video sync
- ✅ Click timestamp to jump to time
- ✅ Transcript search functionality
- ✅ AI summary with all sections
- ✅ Action items with checkboxes
- ✅ Details tab with editable fields
- ✅ Tags management
- ✅ Delete recording with confirmation
- ✅ Video playback from Google Drive
- ✅ All data integrated with Firestore

#### 5.6.4 Validation Checklist
- [ ] Detail page loads recording data correctly
- [ ] 404 page shows if recording not found
- [ ] Video player loads and plays video from Drive URL
- [ ] All player controls work (play, pause, seek, volume)
- [ ] Progress bar is seekable and shows time tooltip
- [ ] Buffered progress is visible
- [ ] Volume slider works and persists
- [ ] Playback speed changes work
- [ ] Keyboard shortcuts all function correctly
- [ ] PiP mode works
- [ ] Fullscreen mode works
- [ ] Controls hide after 3 seconds of inactivity
- [ ] Controls always show when paused
- [ ] Transcript displays all entries
- [ ] Active transcript entry highlights and auto-scrolls
- [ ] Clicking timestamp jumps video to time
- [ ] Transcript search finds and highlights matches
- [ ] Copy and download transcript work
- [ ] AI summary displays all sections correctly
- [ ] Action items can be checked/unchecked
- [ ] Details tab shows all metadata
- [ ] Title and description are editable
- [ ] Tags can be added and removed
- [ ] Delete recording works with confirmation
- [ ] Star/unstar toggle works
- [ ] All updates save to Firestore
- [ ] Loading states show during data fetch
- [ ] Error states handle failures gracefully
- [ ] No console errors
- [ ] Responsive on all screen sizes

#### 5.6.5 Reference
- UIUX_PLAN.md Section 5.5: Detail View
- PLAN_HANDOFF_FRONTEND.md Section 6.2.3: Detail View Components
- PLAN_HANDOFF_FRONTEND.md Section 9.3: Video Player Implementation
- PLAN_HANDOFF_FRONTEND.md Section 9.4: Transcript Synchronization
- HTML5 Video API Documentation
- Web Fullscreen API Documentation

---

### Phase 7: State Management & Integration

**Duration:** 6-7 days  
**Priority:** 🔴 Critical  
**Dependencies:** All previous phases

#### 5.7.1 Objectives
- Implement complete recording logic with MediaRecorder API
- Build chunked upload system for Google Drive
- Connect all UI components to Zustand stores
- Implement API service layer for Firebase/Firestore/Drive
- Build message passing system between extension scripts
- Implement error handling and retry logic
- Test end-to-end workflows
- Ensure data synchronization across components

#### 5.7.2 Tasks

**Task 7.1: Recording Store Implementation** (Section 9.1, 9.2)
- [ ] **stores/recordingStore.ts Setup**
  - Create Zustand store with immer middleware
  - TypeScript interface for state:
    ```typescript
    interface RecordingState {
      // Recording session
      isRecording: boolean;
      isPaused: boolean;
      startTime: number | null;
      pausedTime: number;
      currentDuration: number;
      
      // Recording metadata
      meetingUrl: string;
      meetingTitle: string;
      participantCount: number;
      
      // Technical details
      mediaRecorder: MediaRecorder | null;
      mediaStream: MediaStream | null;
      recordedChunks: Blob[];
      
      // Upload state
      isUploading: boolean;
      uploadProgress: number;
      uploadedChunks: number;
      totalChunks: number;
      driveFileId: string | null;
      
      // Error handling
      error: string | null;
      retryCount: number;
      
      // Actions
      startRecording: (stream: MediaStream, metadata: RecordingMetadata) => Promise<void>;
      pauseRecording: () => void;
      resumeRecording: () => void;
      stopRecording: () => Promise<void>;
      updateDuration: () => void;
      handleChunk: (chunk: Blob) => void;
      uploadChunk: (chunk: Blob, index: number) => Promise<void>;
      finalizeRecording: () => Promise<void>;
      setError: (error: string) => void;
      reset: () => void;
    }
    ```
  - Persist state: use `persist` middleware for critical fields
  - DevTools: enable Redux DevTools integration

- [ ] **startRecording Action**
  - Validate input: stream must be active
  - Initialize MediaRecorder:
    ```typescript
    const options = {
      mimeType: 'video/webm;codecs=vp9,opus',
      videoBitsPerSecond: 2500000, // 2.5 Mbps
      audioBitsPerSecond: 128000,  // 128 kbps
    };
    
    // Fallback to supported mime types
    const mimeType = MediaRecorder.isTypeSupported(options.mimeType)
      ? options.mimeType
      : 'video/webm';
    
    const recorder = new MediaRecorder(stream, { ...options, mimeType });
    ```
  - Set up event listeners:
    - `ondataavailable`: collect chunks
    - `onstop`: finalize recording
    - `onerror`: handle errors
    - `onstart`: update state
    - `onpause`: update state
    - `onresume`: update state
  - Start recording: `recorder.start(10000)` (10-second chunks)
  - Create Google Drive file immediately (get file ID)
  - Update state: isRecording = true, startTime = Date.now()
  - Start duration timer
  - Send message to background: recording started

- [ ] **Chunk Handling Logic**
  - `ondataavailable` event handler:
    ```typescript
    recorder.ondataavailable = (event) => {
      if (event.data.size > 0) {
        handleChunk(event.data);
      }
    };
    ```
  - handleChunk function:
    - Add chunk to recordedChunks array
    - Check chunk size threshold (100MB recommended)
    - If threshold reached, trigger upload
    - Update chunk counter
  - Background upload: don't block recording
  - Queue chunks if network is slow

- [ ] **Chunked Upload to Google Drive**
  - Use Google Drive Resumable Upload API
  - Initialize upload session:
    ```typescript
    const initUpload = async (fileMetadata: any) => {
      const response = await fetch(
        'https://www.googleapis.com/upload/drive/v3/files?uploadType=resumable',
        {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${accessToken}`,
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(fileMetadata),
        }
      );
      
      const uploadUrl = response.headers.get('Location');
      return uploadUrl;
    };
    ```
  - Upload chunks sequentially:
    ```typescript
    const uploadChunk = async (
      uploadUrl: string,
      chunk: Blob,
      start: number,
      total: number
    ) => {
      const end = start + chunk.size - 1;
      
      const response = await fetch(uploadUrl, {
        method: 'PUT',
        headers: {
          'Content-Length': chunk.size.toString(),
          'Content-Range': `bytes ${start}-${end}/${total}`,
        },
        body: chunk,
      });
      
      if (response.status === 200 || response.status === 201) {
        // Upload complete
        return await response.json();
      } else if (response.status === 308) {
        // Resume incomplete, continue with next chunk
        const range = response.headers.get('Range');
        return { nextByte: parseRange(range) };
      } else {
        throw new Error(`Upload failed: ${response.status}`);
      }
    };
    ```
  - Track progress: update uploadProgress state
  - Handle upload errors: retry logic with exponential backoff
  - Resume interrupted uploads: query upload status

- [ ] **pauseRecording & resumeRecording**
  - Pause: `mediaRecorder?.pause()`
  - Update state: isPaused = true, pausedTime accumulate
  - Resume: `mediaRecorder?.resume()`
  - Update state: isPaused = false
  - Send messages to background script

- [ ] **stopRecording Action**
  - Stop MediaRecorder: `mediaRecorder?.stop()`
  - Stop all media tracks: `mediaStream?.getTracks().forEach(track => track.stop())`
  - Wait for final chunks to be collected
  - Upload remaining chunks
  - Finalize upload session
  - Create Firestore document with metadata
  - Update state: isRecording = false
  - Send message to background: recording stopped
  - Navigate to detail page (dashboard)
  - Show success toast

- [ ] **Duration Timer**
  - useEffect with setInterval
  - Update every second when recording
  - Calculate: (Date.now() - startTime - pausedTime) / 1000
  - Format: HH:MM:SS
  - Stop timer when recording stops

- [ ] **Error Handling**
  - MediaRecorder errors: show error in UI, option to retry
  - Upload failures: exponential backoff retry (3 attempts)
  - Network offline: pause uploads, resume when online
  - Storage quota exceeded: show error, suggest cleanup
  - Permission denied: guide user to grant permissions
  - Log all errors to console and Firestore (for monitoring)

- [ ] **Cleanup & Reset**
  - Release media stream
  - Clear recorded chunks
  - Reset all state to initial values
  - Call on unmount or error

**Task 7.2: Auth Store Implementation** (Section 8.1)
- [ ] **stores/authStore.ts Setup**
  - State interface:
    ```typescript
    interface AuthState {
      user: User | null;
      loading: boolean;
      error: string | null;
      isAuthenticated: boolean;
      
      // Actions
      signIn: (provider: 'google') => Promise<void>;
      signOut: () => Promise<void>;
      refreshToken: () => Promise<void>;
      setUser: (user: User | null) => void;
    }
    ```
  - Persist: save user data (not tokens)
  - Hydrate on app load

- [ ] **Firebase Auth Integration**
  - Initialize Firebase: `initializeApp(firebaseConfig)`
  - Get auth instance: `getAuth(app)`
  - Set persistence: `setPersistence(auth, browserLocalPersistence)`
  - Auth state observer:
    ```typescript
    onAuthStateChanged(auth, (user) => {
      if (user) {
        setUser({
          uid: user.uid,
          email: user.email,
          displayName: user.displayName,
          photoURL: user.photoURL,
        });
      } else {
        setUser(null);
      }
    });
    ```

- [ ] **Sign In with Google**
  - Create provider: `new GoogleAuthProvider()`
  - Set scopes:
    ```typescript
    provider.addScope('https://www.googleapis.com/auth/drive.file');
    provider.addScope('https://www.googleapis.com/auth/userinfo.profile');
    ```
  - Sign in: `signInWithPopup(auth, provider)`
  - Get credential: `GoogleAuthProvider.credentialFromResult(result)`
  - Store access token (for Drive API): save to secure storage
  - Update state: user, isAuthenticated
  - Redirect to dashboard
  - Handle errors: popup blocked, network error, user cancelled

- [ ] **Sign Out**
  - Call `signOut(auth)`
  - Clear user state
  - Clear all stores (recordings, UI)
  - Redirect to landing page
  - Show toast: "Signed out successfully"

- [ ] **Token Refresh**
  - Get current user: `auth.currentUser`
  - Get fresh token: `user.getIdToken(true)`
  - Update stored token
  - Call automatically before API requests if token expired

**Task 7.3: Recordings Store (Dashboard)** (Section 8.2)
- [ ] **stores/recordingsStore.ts Setup**
  - State interface:
    ```typescript
    interface RecordingsState {
      recordings: Recording[];
      filteredRecordings: Recording[];
      loading: boolean;
      error: string | null;
      
      // Filters
      filters: {
        dateRange: { start: Date | null; end: Date | null };
        duration: { min: number | null; max: number | null };
        fileSize: { min: number | null; max: number | null };
        status: RecordingStatus[];
        starred: boolean | null;
        tags: string[];
        search: string;
      };
      
      // Sort
      sortBy: 'date' | 'duration' | 'size' | 'title';
      sortOrder: 'asc' | 'desc';
      
      // View
      viewMode: 'grid' | 'list';
      
      // Actions
      fetchRecordings: () => Promise<void>;
      addRecording: (recording: Recording) => void;
      updateRecording: (id: string, data: Partial<Recording>) => void;
      deleteRecording: (id: string) => Promise<void>;
      toggleStar: (id: string) => Promise<void>;
      setFilters: (filters: Partial<Filters>) => void;
      clearFilters: () => void;
      setSortBy: (field: string, order: string) => void;
      setViewMode: (mode: 'grid' | 'list') => void;
      applyFilters: () => void;
    }
    ```
  - Persist: filters, sortBy, sortOrder, viewMode
  - Computed values: filteredRecordings based on filters & sort

- [ ] **fetchRecordings Action**
  - Query Firestore: `collection(db, 'recordings')`
  - Filter by user: `where('userId', '==', user.uid)`
  - Filter deleted: `where('deleted', '==', false)`
  - Order by: `orderBy('createdAt', 'desc')`
  - Get documents: `getDocs(query)`
  - Map to Recording type
  - Update state: recordings array
  - Handle pagination (optional): load more on scroll
  - Cache in memory: avoid refetch on every page visit

- [ ] **Real-time Updates**
  - Use Firestore snapshot listener:
    ```typescript
    const unsubscribe = onSnapshot(
      query(collection(db, 'recordings'), where('userId', '==', userId)),
      (snapshot) => {
        snapshot.docChanges().forEach((change) => {
          if (change.type === 'added') {
            addRecording(change.doc.data() as Recording);
          }
          if (change.type === 'modified') {
            updateRecording(change.doc.id, change.doc.data());
          }
          if (change.type === 'removed') {
            removeRecording(change.doc.id);
          }
        });
      }
    );
    ```
  - Clean up listener on unmount
  - Optimistic updates: update UI immediately, sync with Firestore

- [ ] **Filter & Sort Logic**
  - applyFilters function:
    - Start with all recordings
    - Apply each filter sequentially:
      - Date range: filter by createdAt
      - Duration: filter by duration (min/max)
      - File size: filter by fileSize (min/max)
      - Status: filter by status (if array not empty)
      - Starred: filter by starred (if not null)
      - Tags: filter by tags (intersection or union)
      - Search: filter by title/description (case-insensitive)
    - Apply sort: sortBy field and sortOrder
    - Update filteredRecordings
  - Debounce search filter: wait 300ms after typing
  - Run applyFilters on any filter/sort change

- [ ] **Toggle Star Action**
  - Optimistic update: toggle starred in local state immediately
  - Update Firestore: `updateDoc(doc, { starred: !recording.starred })`
  - If error, revert optimistic update
  - Show toast on error

- [ ] **Delete Recording Action**
  - Show confirmation modal first
  - Soft delete: update Firestore `{ deleted: true, deletedAt: Date.now() }`
  - Remove from local state
  - Show toast: "Recording deleted" with undo option (optional)
  - Undo: restore within 10 seconds

**Task 7.4: UI Store (Dashboard)** (Section 8.3)
- [ ] **stores/uiStore.ts Setup**
  - State interface:
    ```typescript
    interface UIState {
      // Sidebar
      sidebarCollapsed: boolean;
      sidebarMobileOpen: boolean;
      
      // Modals
      activeModal: string | null;
      modalData: any;
      
      // Toasts
      toasts: Toast[];
      
      // Theme (future)
      theme: 'light' | 'dark' | 'system';
      
      // Actions
      toggleSidebar: () => void;
      openSidebar: () => void;
      closeSidebar: () => void;
      toggleMobileSidebar: () => void;
      openModal: (modalId: string, data?: any) => void;
      closeModal: () => void;
      addToast: (toast: Omit<Toast, 'id'>) => void;
      removeToast: (id: string) => void;
      setTheme: (theme: string) => void;
    }
    ```
  - Persist: sidebarCollapsed, theme
  - Auto-dismiss toasts after 5 seconds

- [ ] **Toast System**
  - Toast interface:
    ```typescript
    interface Toast {
      id: string;
      type: 'success' | 'error' | 'warning' | 'info';
      title: string;
      description?: string;
      duration?: number;
      action?: {
        label: string;
        onClick: () => void;
      };
    }
    ```
  - addToast: generate unique ID, add to array, set timeout to remove
  - removeToast: filter out by ID
  - Toast component consumes from store and renders all toasts

- [ ] **Modal System**
  - openModal: set activeModal ID and modalData
  - closeModal: clear activeModal and modalData
  - Modal component: renders based on activeModal
  - Multiple modal types: confirmation, form, info, etc.
  - ESC key closes modal
  - Click outside closes modal

**Task 7.5: API Service Layer** (Section 8.4)
- [ ] **services/firebase.ts**
  - Initialize Firebase app:
    ```typescript
    import { initializeApp } from 'firebase/app';
    import { getAuth } from 'firebase/auth';
    import { getFirestore } from 'firebase/firestore';
    
    const firebaseConfig = {
      apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
      authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
      projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
      storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
      messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
      appId: import.meta.env.VITE_FIREBASE_APP_ID,
    };
    
    export const app = initializeApp(firebaseConfig);
    export const auth = getAuth(app);
    export const db = getFirestore(app);
    ```
  - Export instances for use across app
  - Environment variables: load from .env files
  - Error handling: check if initialization fails

- [ ] **services/firestore.ts**
  - CRUD operations for recordings:
    ```typescript
    export const getRecordings = async (userId: string): Promise<Recording[]> => {
      const q = query(
        collection(db, 'recordings'),
        where('userId', '==', userId),
        where('deleted', '==', false),
        orderBy('createdAt', 'desc')
      );
      const snapshot = await getDocs(q);
      return snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() } as Recording));
    };
    
    export const getRecording = async (id: string): Promise<Recording | null> => {
      const docRef = doc(db, 'recordings', id);
      const docSnap = await getDoc(docRef);
      if (docSnap.exists()) {
        return { id: docSnap.id, ...docSnap.data() } as Recording;
      }
      return null;
    };
    
    export const createRecording = async (data: RecordingInput): Promise<string> => {
      const docRef = await addDoc(collection(db, 'recordings'), {
        ...data,
        createdAt: serverTimestamp(),
        updatedAt: serverTimestamp(),
        deleted: false,
      });
      return docRef.id;
    };
    
    export const updateRecording = async (
      id: string,
      data: Partial<Recording>
    ): Promise<void> => {
      const docRef = doc(db, 'recordings', id);
      await updateDoc(docRef, {
        ...data,
        updatedAt: serverTimestamp(),
      });
    };
    
    export const deleteRecording = async (id: string): Promise<void> => {
      const docRef = doc(db, 'recordings', id);
      await updateDoc(docRef, {
        deleted: true,
        deletedAt: serverTimestamp(),
      });
    };
    ```
  - Error handling: try-catch blocks, throw custom errors
  - Type safety: use TypeScript interfaces

- [ ] **services/storage.ts (Google Drive)**
  - Get access token from auth:
    ```typescript
    const getAccessToken = async (): Promise<string> => {
      const user = auth.currentUser;
      if (!user) throw new Error('Not authenticated');
      const token = await user.getIdToken();
      return token;
    };
    ```
  - Create Drive file:
    ```typescript
    export const createDriveFile = async (
      fileName: string,
      mimeType: string
    ): Promise<string> => {
      const token = await getAccessToken();
      
      const metadata = {
        name: fileName,
        mimeType: mimeType,
      };
      
      const response = await fetch(
        'https://www.googleapis.com/drive/v3/files',
        {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(metadata),
        }
      );
      
      const data = await response.json();
      return data.id;
    };
    ```
  - Get video URL (signed URL):
    ```typescript
    export const getVideoUrl = async (fileId: string): Promise<string> => {
      const token = await getAccessToken();
      
      const response = await fetch(
        `https://www.googleapis.com/drive/v3/files/${fileId}?alt=media`,
        {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${token}`,
          },
        }
      );
      
      // Return blob URL or signed URL
      return response.url;
    };
    ```
  - Delete file:
    ```typescript
    export const deleteDriveFile = async (fileId: string): Promise<void> => {
      const token = await getAccessToken();
      
      await fetch(
        `https://www.googleapis.com/drive/v3/files/${fileId}`,
        {
          method: 'DELETE',
          headers: {
            'Authorization': `Bearer ${token}`,
          },
        }
      );
    };
    ```

- [ ] **services/api.ts (Backend API)**
  - Base URL: from environment variable
  - HTTP client setup (fetch or axios):
    ```typescript
    const baseURL = import.meta.env.VITE_API_BASE_URL;
    
    export const apiClient = {
      async get<T>(endpoint: string): Promise<T> {
        const token = await getAccessToken();
        const response = await fetch(`${baseURL}${endpoint}`, {
          headers: { 'Authorization': `Bearer ${token}` },
        });
        if (!response.ok) throw new Error(`API error: ${response.statusText}`);
        return response.json();
      },
      
      async post<T>(endpoint: string, data: any): Promise<T> {
        const token = await getAccessToken();
        const response = await fetch(`${baseURL}${endpoint}`, {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
        });
        if (!response.ok) throw new Error(`API error: ${response.statusText}`);
        return response.json();
      },
      
      // Similar for PUT, PATCH, DELETE
    };
    ```
  - Request interceptors: add auth token
  - Response interceptors: handle errors globally
  - Retry logic for failed requests

**Task 7.6: Message Passing (Extension)** (Section 9.5)
- [ ] **Message Types Definition**
  - `types/messages.ts`:
    ```typescript
    export enum MessageType {
      START_RECORDING = 'START_RECORDING',
      STOP_RECORDING = 'STOP_RECORDING',
      PAUSE_RECORDING = 'PAUSE_RECORDING',
      RESUME_RECORDING = 'RESUME_RECORDING',
      RECORDING_STATUS = 'RECORDING_STATUS',
      TAB_CAPTURE_STREAM = 'TAB_CAPTURE_STREAM',
      ERROR = 'ERROR',
    }
    
    export interface Message {
      type: MessageType;
      payload?: any;
    }
    ```

- [ ] **Background Script Message Handling**
  - Listen for messages:
    ```typescript
    chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
      switch (message.type) {
        case MessageType.START_RECORDING:
          handleStartRecording(message.payload);
          sendResponse({ success: true });
          break;
        case MessageType.STOP_RECORDING:
          handleStopRecording();
          sendResponse({ success: true });
          break;
        // ... other cases
      }
      return true; // Keep channel open for async response
    });
    ```
  - Manage recording state in background
  - Coordinate between popup and content script
  - Handle tab capture: `chrome.tabCapture.capture()`

- [ ] **Content Script Communication**
  - Send messages to background:
    ```typescript
    chrome.runtime.sendMessage(
      { type: MessageType.START_RECORDING, payload: { tabId } },
      (response) => {
        if (response.success) {
          // Handle success
        }
      }
    );
    ```
  - Listen for messages from background:
    ```typescript
    chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
      if (message.type === MessageType.RECORDING_STATUS) {
        updateUIStatus(message.payload);
      }
    });
    ```
  - Inject control bar into page
  - Update control bar based on recording state

- [ ] **Popup Communication**
  - Query active tab: `chrome.tabs.query({ active: true, currentWindow: true })`
  - Send messages to content script of active tab:
    ```typescript
    chrome.tabs.sendMessage(tabId, message);
    ```
  - Send messages to background script
  - Sync state between popup and background

**Task 7.7: Error Handling & Retry Logic**
- [ ] **Global Error Handler**
  - utils/errorHandler.ts:
    ```typescript
    export class AppError extends Error {
      constructor(
        message: string,
        public code: string,
        public statusCode?: number,
        public retryable: boolean = false
      ) {
        super(message);
        this.name = 'AppError';
      }
    }
    
    export const handleError = (error: any) => {
      console.error('Error:', error);
      
      if (error instanceof AppError) {
        // Show user-friendly message
        showToast({
          type: 'error',
          title: 'Error',
          description: error.message,
        });
        
        // Log to monitoring service
        logError(error);
        
        return error;
      }
      
      // Unknown error
      showToast({
        type: 'error',
        title: 'Unexpected Error',
        description: 'Something went wrong. Please try again.',
      });
      
      return new AppError('Unknown error', 'UNKNOWN_ERROR');
    };
    ```

- [ ] **Retry Logic**
  - utils/retry.ts:
    ```typescript
    export const retry = async <T>(
      fn: () => Promise<T>,
      options: {
        maxAttempts: number;
        delay: number;
        backoff: number;
        retryableErrors?: string[];
      }
    ): Promise<T> => {
      let lastError: any;
      
      for (let attempt = 1; attempt <= options.maxAttempts; attempt++) {
        try {
          return await fn();
        } catch (error: any) {
          lastError = error;
          
          // Check if error is retryable
          if (
            options.retryableErrors &&
            !options.retryableErrors.includes(error.code)
          ) {
            throw error;
          }
          
          if (attempt < options.maxAttempts) {
            const delayMs = options.delay * Math.pow(options.backoff, attempt - 1);
            await new Promise(resolve => setTimeout(resolve, delayMs));
          }
        }
      }
      
      throw lastError;
    };
    ```
  - Use for API calls, uploads, etc.
  - Example:
    ```typescript
    const uploadWithRetry = () => retry(
      () => uploadChunk(chunk),
      {
        maxAttempts: 3,
        delay: 1000,
        backoff: 2,
        retryableErrors: ['NETWORK_ERROR', 'TIMEOUT'],
      }
    );
    ```

- [ ] **Network Status Detection**
  - Listen for online/offline events:
    ```typescript
    window.addEventListener('online', () => {
      // Resume uploads
      resumeUploads();
    });
    
    window.addEventListener('offline', () => {
      // Pause uploads
      pauseUploads();
      showToast({
        type: 'warning',
        title: 'You are offline',
        description: 'Uploads will resume when connection is restored.',
      });
    });
    ```

**Task 7.8: Integration Testing**
- [ ] **Extension Recording Flow**
  - Test sequence:
    1. Open Google Meet in tab
    2. Click extension icon
    3. Click "Start Recording"
    4. Background captures tab + audio
    5. Control bar appears on page
    6. Record for 30 seconds
    7. Click "Pause" - verify pause works
    8. Click "Resume" - verify resume works
    9. Click "Stop Recording"
    10. Chunks upload to Drive
    11. Firestore document created
    12. Dashboard shows new recording
  - Verify: no errors in console, all state updates correct

- [ ] **Dashboard Data Flow**
  - Test sequence:
    1. Sign in with Google
    2. Dashboard loads recordings from Firestore
    3. Apply filters - verify filtered list updates
    4. Change sort - verify sort applied
    5. Toggle view mode - verify UI changes
    6. Click recording - navigate to detail
    7. Video loads and plays
    8. Transcript syncs with video
    9. Edit title - verify saves to Firestore
    10. Delete recording - verify removed
  - Verify: real-time updates work, no data loss

- [ ] **Error Scenarios**
  - Test cases:
    - Network disconnects during upload
    - User denies permissions
    - Firestore write fails
    - Drive quota exceeded
    - Invalid video file
    - Expired auth token
  - Verify: errors handled gracefully, user sees clear messages

- [ ] **Cross-Component Communication**
  - Verify messages flow correctly:
    - Popup → Background
    - Background → Content Script
    - Content Script → Background
    - Extension → Dashboard (via Firestore)
  - Check: no message loss, responses received

#### 5.7.3 Deliverables
- ✅ Complete recordingStore with MediaRecorder integration
- ✅ Chunked upload system for Google Drive
- ✅ authStore with Firebase Authentication
- ✅ recordingsStore with Firestore sync
- ✅ uiStore for UI state management
- ✅ API service layer (Firestore, Drive, Backend)
- ✅ Message passing system for extension
- ✅ Global error handler and retry logic
- ✅ Network status detection
- ✅ All stores connected to UI components
- ✅ End-to-end recording workflow tested
- ✅ Dashboard data flow tested
- ✅ Error scenarios handled

#### 5.7.4 Validation Checklist
- [ ] Recording starts successfully from extension
- [ ] MediaRecorder captures video + audio
- [ ] Chunks are collected correctly
- [ ] Chunks upload to Drive in background
- [ ] Upload progress updates in UI
- [ ] Pause and resume work during recording
- [ ] Stop recording completes all uploads
- [ ] Firestore document created with correct metadata
- [ ] Auth flow works (sign in, sign out, refresh token)
- [ ] Recordings load from Firestore on dashboard
- [ ] Filters and sort work correctly
- [ ] Real-time updates reflected in UI
- [ ] Starring/unstarring recordings works
- [ ] Deleting recordings works with confirmation
- [ ] Video playback works from Drive URL
- [ ] Error messages display for failures
- [ ] Retry logic works for failed operations
- [ ] Network offline/online detection works
- [ ] Message passing between scripts works
- [ ] All stores persist correct data
- [ ] No memory leaks in long recording sessions
- [ ] No console errors during normal operation

#### 5.7.5 Reference
- PLAN_HANDOFF_FRONTEND.md Section 9: Core Implementation Details
- PLAN_HANDOFF_FRONTEND.md Section 8: State Management Architecture
- MediaRecorder API Documentation
- Chrome Extension Message Passing Documentation
- Google Drive API - Resumable Upload Documentation
- Firebase Authentication Documentation
- Cloud Firestore Documentation
- Zustand Documentation

---
