# OpenMeet Recorder - Frontend Agent Prompts

> **Purpose:** Phase-based implementation prompts for the Frontend Agent  
> **Reference Document:** [plans/FRONTEND_PLAN.md](../plans/FRONTEND_PLAN.md)  
> **Created:** 2025-12-29  
> **Version:** 1.0.0  
> **Prompt Type:** Phase-Based (Not Task-by-Task)

---

## Table of Contents
1. [How to Use This Document](#how-to-use-this-document)
2. [Prerequisites & Setup](#prerequisites--setup)
3. [Phase 1: Project Setup & Design System](#phase-1-project-setup--design-system)
4. [Phase 2: Shared Components Library](#phase-2-shared-components-library)
5. [Phase 3: Chrome Extension UI](#phase-3-chrome-extension-ui)
6. [Phase 4: Dashboard Layout & Navigation](#phase-4-dashboard-layout--navigation)
7. [Phase 5: Recording List & Display](#phase-5-recording-list--display)
8. [Phase 6: Recording Detail & Playback](#phase-6-recording-detail--playback)
9. [Phase 7: State Management & Integration](#phase-7-state-management--integration)
10. [Final Validation](#final-validation)

---

## How to Use This Document

### Prompt Philosophy
Unlike the UI/UX phase which used 47 task-by-task prompts, the Frontend phase uses **7 comprehensive phase-based prompts**. Each prompt encompasses a complete implementation phase with multiple interconnected tasks.

### Why Phase-Based Prompts?
- **Contextual Cohesion:** Frontend work involves interconnected components that benefit from being implemented together
- **Reduced Context Switching:** Agents can maintain better context when working on a complete phase
- **Natural Dependencies:** Phases naturally group tasks that depend on each other
- **Efficiency:** Fewer but more substantial prompts reduce overhead
- **Better Results:** Agents produce more cohesive implementations when given phase-level objectives

### Execution Guidelines

**For Users:**
1. **Execute Phases Sequentially:** Complete Phase 1 before Phase 2, etc.
2. **Provide Full Context:** Always reference FRONTEND_PLAN.md when giving prompts
3. **Review Phase Deliverables:** Check all deliverables at phase completion
4. **Validate Before Proceeding:** Run validation checklist before next phase
5. **Ask for Clarifications:** If output doesn't match, point agent to specific FRONTEND_PLAN.md sections

**For AI Agents:**
1. **Read the Full Phase:** Review the entire phase in FRONTEND_PLAN.md before starting
2. **Understand Dependencies:** Check what previous phases provide
3. **Follow Tech Stack:** Use specified versions and patterns
4. **Reference Design System:** Always consult UIUX_PLAN.md for styling
5. **Implement Completely:** Don't leave TODOs or placeholders
6. **Test As You Go:** Validate each component before proceeding
7. **Document Decisions:** Note any deviations or architectural choices

### Priority Indicators
- 🔴 **Critical:** Must complete; project cannot proceed without this
- 🟡 **High:** Important for core functionality
- 🟢 **Medium:** Enhances user experience
- ⚪ **Optional:** Nice-to-have, can defer to later iterations

---

## Prerequisites & Setup

### Before Starting Phase 1

**Required Access:**
- [ ] Repository access: https://github.com/PRATS-gits/openmeet-chromium
- [ ] Branch: `pratham` (current development branch)
- [ ] Read access to all plan documents

**Required Reading:**
- [ ] FRONTEND_PLAN.md (complete, 4352 lines)
- [ ] PLAN_HANDOFF_FRONTEND.md (sections 1-9)
- [ ] UIUX_PLAN.md (sections 3-5 for design tokens and components)

**Environment Setup:**
- [ ] Node.js 18+ installed
- [ ] pnpm package manager installed
- [ ] VS Code with recommended extensions
- [ ] Chrome browser for extension testing

**Repository Structure:**
```
openmeet/
├── extension/          # Chrome extension code
│   ├── src/
│   │   ├── popup/      # Extension popup UI
│   │   ├── content/    # Content scripts
│   │   ├── background/ # Service worker
│   │   ├── options/    # Options page
│   │   └── components/ # Shared UI components
│   ├── public/         # Static assets (icons, manifest)
│   └── vite.config.ts  # Vite config for extension
│
├── dashboard/          # React dashboard SPA
│   ├── src/
│   │   ├── pages/      # Route pages
│   │   ├── components/ # React components
│   │   ├── stores/     # Zustand stores
│   │   ├── services/   # API services
│   │   ├── hooks/      # Custom React hooks
│   │   ├── utils/      # Utility functions
│   │   ├── types/      # TypeScript types
│   │   └── styles/     # Global styles
│   └── vite.config.ts  # Vite config for dashboard
│
├── shared/             # Code shared between extension and dashboard
│   ├── types/          # Shared TypeScript interfaces
│   ├── utils/          # Shared utility functions
│   └── constants/      # Shared constants
│
├── docs/               # Documentation
├── plans/              # Planning documents
└── prompts/            # Agent prompts (this file)
```

---

## Phase 1: Project Setup & Design System

**Duration:** 1-2 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.1

### Phase Objective
Initialize the complete monorepo project structure with Vite, TypeScript, and React. Establish the design system by implementing CSS custom properties from UIUX_PLAN.md and configuring TailwindCSS. Set up all development tools, linting, and build processes for both extension and dashboard.

### Context & Background
This phase lays the foundation for all frontend work. The design system must precisely match the UIUX_PLAN.md specifications, using CSS custom properties for:
- 10-step color scales (primary, secondary, semantic colors)
- 9 typography sizes with line heights and letter spacing
- 27 spacing values on a 4px grid system
- 8 shadow levels
- 6 animation keyframes with timing functions

Both the extension and dashboard will share the same design tokens but have separate build outputs. The extension uses Manifest V3 with Vite for bundling.

### Full Prompt

```
You are implementing Phase 1 of the OpenMeet Recorder frontend development. This phase establishes the project foundation and design system.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.1 (Phase 1 detailed tasks)
- UIUX_PLAN.md Section 3 (Complete design system specifications)
- PLAN_HANDOFF_FRONTEND.md Section 4 (Technology stack)

**Phase Scope - Complete ALL of the following:**

1. **Monorepo Structure Setup**
   - Initialize pnpm workspace with three packages: extension/, dashboard/, shared/
   - Configure workspace package.json with proper dependencies
   - Set up pnpm-workspace.yaml
   - Create .gitignore, .editorconfig, .prettierrc

2. **Extension Directory (Vite + React + TypeScript)**
   - Initialize Vite project in extension/ with React + TypeScript template
   - Install vite-plugin-chrome-extension (v1.0+)
   - Configure vite.config.ts for extension build (separate bundles for popup, content, background, options)
   - Create tsconfig.json with strict mode enabled
   - Set up directory structure: src/popup/, src/content/, src/background/, src/options/, src/components/
   - Install dependencies: react 18.2+, react-dom, zustand 4.5+, dexie 3.2+, lucide-react
   - Configure Chrome extension manifest.json (Manifest V3) with all required permissions

3. **Dashboard Directory (Vite + React + TypeScript)**
   - Initialize Vite project in dashboard/ with React + TypeScript template
   - Configure vite.config.ts for SPA build with path aliases (@/)
   - Create tsconfig.json with strict mode
   - Set up directory structure: src/pages/, src/components/, src/stores/, src/services/, src/hooks/, src/utils/, src/types/, src/styles/
   - Install dependencies: react 18.2+, react-dom, react-router-dom 6.21+, zustand 4.5+, firebase 10.0+, lucide-react
   - Configure environment variables with .env.example

4. **Shared Package**
   - Create shared/types/ for TypeScript interfaces (Recording, User, RecordingMetadata, etc.)
   - Create shared/utils/ for common utility functions
   - Create shared/constants/ for app constants
   - Export all from shared/index.ts
   - Configure tsconfig.json for shared types

5. **Design System Implementation**
   - Create shared/styles/design-tokens.css with ALL CSS custom properties from UIUX_PLAN.md:
     * Brand colors (primary-50 through primary-900, 10 steps)
     * Secondary colors (gray-blue scale, 10 steps)
     * Semantic colors (success, warning, error, info - each 7 steps)
     * Grayscale (gray-50 through gray-950, 11 steps)
     * Special colors (recording, paused, uploading states)
   - Create shared/styles/typography.css:
     * 9 typography sizes (text-xs through text-4xl) with line-height and letter-spacing
     * Font families (Inter for UI, JetBrains Mono for code/monospace)
   - Create shared/styles/spacing.css:
     * 27 spacing values (space-0 through space-96) following 4px grid
   - Create shared/styles/shadows.css:
     * 8 shadow levels (shadow-xs through shadow-2xl)
   - Create shared/styles/animations.css:
     * 6 keyframe animations (fade-in, slide-up, slide-down, scale-in, shimmer, spin)

6. **TailwindCSS Configuration**
   - Install tailwindcss 3.4+ and configure for both extension and dashboard
   - Create tailwind.config.js that extends with custom theme:
     * Import design token values
     * Configure colors using CSS variables
     * Configure spacing, shadows, typography
     * Add custom animations
   - Configure PostCSS
   - Create base styles that apply design tokens globally

7. **Development Tools Setup**
   - ESLint configuration (extends airbnb-typescript, react, react-hooks)
   - Prettier configuration (consistent formatting)
   - TypeScript strict mode with path aliases
   - VS Code settings.json with recommended settings
   - Install dev dependencies: @types/chrome, @types/react, typescript 5.3+

8. **Build & Scripts Configuration**
   - Root package.json scripts:
     * dev:extension - run extension in dev mode
     * dev:dashboard - run dashboard in dev mode
     * build:extension - production build for extension
     * build:dashboard - production build for dashboard
     * build:all - build both
     * lint - run ESLint
     * type-check - run TypeScript compiler check
   - Configure hot module replacement for development
   - Set up Chrome extension reloading on code changes

**Critical Requirements:**
- Use exact versions specified in FRONTEND_PLAN.md Section 3
- All color values must match UIUX_PLAN.md Section 3.1 exactly
- Typography system must match UIUX_PLAN.md Section 3.2 exactly
- Spacing values must follow 4px grid from UIUX_PLAN.md Section 3.3
- TypeScript must be in strict mode with no `any` types
- All imports must use path aliases (@components, @stores, etc.)
- Manifest V3 must include all required permissions: tabs, storage, identity, scripting

**Validation Checklist:**
Before marking this phase complete, verify:
- [ ] pnpm install succeeds with no errors
- [ ] Both dev:extension and dev:dashboard scripts run successfully
- [ ] Hot reload works in development mode
- [ ] TypeScript compiles with no errors
- [ ] ESLint passes with no warnings
- [ ] All design tokens are accessible via CSS variables
- [ ] TailwindCSS utility classes work (test with a simple component)
- [ ] Extension can be loaded in Chrome (chrome://extensions)
- [ ] Dashboard opens in browser at http://localhost:5173
- [ ] No console errors in either environment

**Deliverables:**
- Complete monorepo structure with all three packages
- Fully configured build systems (Vite, TypeScript, Tailwind)
- Complete design system in CSS (design-tokens.css, typography.css, spacing.css, shadows.css, animations.css)
- Working development environment for both extension and dashboard
- All configuration files (package.json, tsconfig.json, vite.config.ts, tailwind.config.js, .eslintrc.js, manifest.json)
- README.md with setup instructions

**If You Encounter Issues:**
- Design token values: Reference UIUX_PLAN.md Section 3 for exact values
- Build configuration: Check PLAN_HANDOFF_FRONTEND.md Section 4.2 for Vite setup
- TypeScript errors: Ensure all paths are properly aliased in tsconfig.json
- Extension not loading: Verify manifest.json matches Manifest V3 schema

Output the complete file structure, all configuration files, and design system CSS. Test that the environment runs successfully before marking complete.
```

### Phase 1 Completion Criteria
- ✅ All packages install and build successfully
- ✅ Dev servers run without errors
- ✅ Design system CSS matches UIUX_PLAN.md specifications
- ✅ TypeScript strict mode enabled with no errors
- ✅ Extension loads in Chrome
- ✅ Dashboard renders in browser

**Estimated Time:** 1-2 days  
**Next Phase:** Phase 2 - Shared Components Library

---

## Phase 2: Shared Components Library

**Duration:** 2-3 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.2

### Phase Objective
Build a complete library of reusable React components following ShadcnUI patterns. These foundational components will be used across both the extension and dashboard. Each component must be fully typed, accessible (WCAG 2.1 Level AA), and match the UIUX_PLAN.md specifications exactly.

### Context & Background
This phase creates the building blocks for all UI development. Components should:
- Use Radix UI primitives for accessibility and behavior
- Follow compound component patterns for flexibility
- Accept className props for customization via Tailwind
- Include all variants specified in UIUX_PLAN.md
- Handle all interactive states (hover, active, focus, disabled)
- Be fully typed with TypeScript interfaces

The components will be placed in shared/components/ so both extension and dashboard can import them. This avoids code duplication and ensures consistent UI across the application.

### Full Prompt

```
You are implementing Phase 2 of the OpenMeet Recorder frontend development. This phase builds the shared component library that both extension and dashboard will use.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.2 (Phase 2 detailed tasks)
- UIUX_PLAN.md Section 4 (Component specifications with HTML/CSS examples)
- PLAN_HANDOFF_FRONTEND.md Section 6.1 (Component requirements)

**Phase Scope - Implement ALL of the following components:**

1. **Button Component** (shared/components/Button.tsx)
   - Variants: primary, secondary, ghost, error
   - Sizes: small (sm), medium (md), large (lg)
   - States: default, hover, active, focus, disabled, loading
   - Props: variant, size, disabled, loading, leftIcon, rightIcon, fullWidth, onClick
   - Loading state: show spinner icon, disable interaction
   - Icon support: accept icon components on left or right
   - Focus: visible focus ring using ring utilities
   - Reference: UIUX_PLAN.md Section 4.1.1

2. **Input Component** (shared/components/Input.tsx)
   - Types: text, email, password, number, search, url
   - Sizes: small, medium, large
   - States: default, focus, error, disabled
   - Props: type, size, error, errorMessage, disabled, leftIcon, rightIcon, placeholder, value, onChange
   - Label: optional label prop with proper htmlFor association
   - Helper text: display below input
   - Error state: red border, error message display
   - Icon slots: support icons on left or right side
   - Password toggle: show/hide password functionality
   - Reference: UIUX_PLAN.md Section 4.1.2

3. **Card Component** (shared/components/Card.tsx)
   - Compound component pattern: Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter
   - Variants: default, hover (with shadow elevation), clickable (pointer cursor)
   - Props: variant, onClick, className
   - Clickable: add hover effects and cursor when onClick provided
   - Header: optional with title and description
   - Footer: optional actions section
   - Reference: UIUX_PLAN.md Section 4.2.1

4. **Badge Component** (shared/components/Badge.tsx)
   - Variants: default, success, warning, error, info, recording, paused
   - Sizes: small, medium, large
   - Props: variant, size, icon, removable, onRemove
   - Icon: optional icon on left
   - Removable: show X button, call onRemove when clicked
   - Recording variant: use recording color (#ef4444) with pulse animation
   - Reference: UIUX_PLAN.md Section 4.1.8

5. **Avatar Component** (shared/components/Avatar.tsx)
   - Sizes: xs (24px), sm (32px), md (40px), lg (48px), xl (64px)
   - Props: src, alt, size, fallback, status
   - Image: load from src, show fallback on error
   - Fallback: display initials or icon if no image
   - Status indicator: online/offline dot (optional)
   - Border: ring utility for emphasis
   - Reference: UIUX_PLAN.md Section 4.1.9

6. **Dropdown Component** (shared/components/Dropdown.tsx)
   - Use Radix UI DropdownMenu primitive
   - Compound components: Dropdown, DropdownTrigger, DropdownContent, DropdownItem, DropdownSeparator
   - Props: open, onOpenChange, side, align
   - Animation: slide and fade in/out
   - Item states: default, hover, active, disabled
   - Item variants: default, danger (red text)
   - Separator: divider between groups
   - Keyboard navigation: arrow keys, enter, escape
   - Reference: UIUX_PLAN.md Section 4.1.5

7. **Modal Component** (shared/components/Modal.tsx)
   - Use Radix UI Dialog primitive
   - Compound components: Modal, ModalTrigger, ModalContent, ModalHeader, ModalTitle, ModalDescription, ModalFooter
   - Props: open, onOpenChange, size (sm, md, lg, full)
   - Overlay: semi-transparent backdrop
   - Animation: fade in overlay, scale up content
   - Close button: X in top-right corner
   - ESC key: closes modal
   - Click outside: closes modal
   - Focus trap: keeps focus within modal
   - Reference: UIUX_PLAN.md Section 4.1.6

8. **Toast Component** (shared/components/Toast.tsx)
   - Use Radix UI Toast primitive
   - Variants: success, error, warning, info
   - Props: variant, title, description, duration, action
   - Position: bottom-right corner, stack vertically
   - Animation: slide in from right, fade out
   - Auto-dismiss: after duration (default 5 seconds)
   - Action button: optional action with callback
   - Close button: X to dismiss manually
   - Queue: multiple toasts stack and dismiss in order
   - Reference: UIUX_PLAN.md Section 4.1.7

9. **Skeleton Component** (shared/components/Skeleton.tsx)
   - Variants: text, circular, rectangular
   - Props: variant, width, height, count (for multiple lines)
   - Animation: shimmer effect using CSS keyframes
   - Text variant: rounded rectangle with text height
   - Circular: for avatar placeholders
   - Rectangular: for cards, images
   - Reference: UIUX_PLAN.md Section 4.1.10

**Implementation Guidelines:**

**TypeScript:**
- Define props interface for each component
- Use React.FC or explicit return types
- Export both component and props interface
- Use discriminated unions for variants

**Accessibility:**
- All interactive elements keyboard accessible
- Proper ARIA attributes (aria-label, aria-describedby, aria-expanded)
- Focus indicators visible
- Semantic HTML elements
- Screen reader announcements for status changes

**Styling:**
- Use Tailwind utility classes
- Use design tokens (text-primary-600, bg-gray-50, etc.)
- Support className prop for customization (use clsx or cn utility)
- Responsive: adjust sizes on mobile if needed
- Dark mode ready: use CSS variables (future-proof)

**State Management:**
- Use React hooks (useState, useEffect, useRef)
- Handle controlled/uncontrolled patterns where applicable
- Forward refs for imperative access (forwardRef)

**Testing During Development:**
- Create a dev page (shared/components/ComponentShowcase.tsx) that renders all components with all variants
- Test all interactive states (hover, click, focus, disabled)
- Test keyboard navigation
- Test with screen reader (optional but recommended)

**File Structure:**
```
shared/components/
├── Button.tsx
├── Input.tsx
├── Card.tsx
├── Badge.tsx
├── Avatar.tsx
├── Dropdown.tsx
├── Modal.tsx
├── Toast.tsx
├── Skeleton.tsx
├── index.ts (export all components)
└── utils/
    └── cn.ts (classnames utility using clsx)
```

**Critical Requirements:**
- Every component must match UIUX_PLAN.md specifications exactly
- All variants and sizes must be implemented
- TypeScript strict mode with no `any` types
- Radix UI primitives must be used for Dropdown, Modal, Toast
- All components must be accessible (WCAG 2.1 AA)
- Components must accept and merge className prop
- Focus states must be visible

**Validation Checklist:**
Before marking this phase complete, verify:
- [ ] All 9 components implemented and exported
- [ ] ComponentShowcase page shows all variants working
- [ ] TypeScript compiles with no errors
- [ ] All components accept className prop and merge correctly
- [ ] Keyboard navigation works for interactive components
- [ ] Focus indicators visible on all interactive elements
- [ ] Loading states work (Button, Input if applicable)
- [ ] Error states display correctly (Input)
- [ ] Toasts auto-dismiss and can be manually closed
- [ ] Modal traps focus and closes on ESC/outside click
- [ ] Dropdown keyboard navigation works
- [ ] No console warnings or errors

**Deliverables:**
- 9 complete, tested components in shared/components/
- TypeScript interfaces exported for all component props
- ComponentShowcase.tsx demonstrating all variants
- index.ts barrel export file
- cn.ts utility for className merging

**If You Encounter Issues:**
- Component styling: Reference exact Tailwind classes from UIUX_PLAN.md Section 4
- Radix UI: Check Radix UI documentation for primitive API
- TypeScript errors: Ensure proper typing of event handlers and refs
- Animation issues: Verify keyframes defined in animations.css from Phase 1

Output all component code, showcase page, and demonstrate working examples before marking complete.
```

### Phase 2 Completion Criteria
- ✅ All 9 components implemented with full TypeScript typing
- ✅ All variants and sizes working
- ✅ Accessibility features implemented
- ✅ ComponentShowcase renders all examples
- ✅ No TypeScript or runtime errors

**Estimated Time:** 2-3 days  
**Next Phase:** Phase 3 - Chrome Extension UI

---

## Phase 3: Chrome Extension UI

**Duration:** 3-4 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.3

### Phase Objective
Implement the complete Chrome extension user interface including the popup, floating control bar, options page, and all extension scripts. Integrate with Chrome APIs for tab capture, storage, and message passing. This phase makes the extension functional for recording control.

### Context & Background
The Chrome extension has four main UI components:
1. **Popup** (16px icon click): Main entry point, shows recording status and start/stop controls
2. **Control Bar** (injected overlay): Floating bar during recording with pause/resume/stop
3. **Options Page** (chrome://extensions): Full settings management
4. **Background Service Worker**: Coordinates recording logic and API calls

The extension must communicate between these components using Chrome's message passing API. State synchronization is critical - when recording starts in popup, the control bar must reflect that state.

### Full Prompt

```
You are implementing Phase 3 of the OpenMeet Recorder frontend development. This phase builds the complete Chrome extension UI and integration with Chrome APIs.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.3 (Phase 3 detailed tasks)
- UIUX_PLAN.md Section 4.3 (Extension UI specifications)
- PLAN_HANDOFF_FRONTEND.md Section 7 (Extension architecture)

**Phase Scope - Complete ALL of the following:**

1. **Manifest V3 Configuration** (extension/public/manifest.json)
   - Update manifest with complete metadata:
     * name: "OpenMeet Recorder"
     * version: "1.0.0"
     * description: "Record Google Meet sessions with AI summarization"
     * icons: 16, 48, 128 sizes
   - Permissions: tabs, storage, identity, scripting, activeTab
   - Host permissions: https://meet.google.com/*
   - Background service worker: src/background/index.ts
   - Content scripts: inject into meet.google.com
   - Action: default popup (src/popup/index.html)
   - Options page: src/options/index.html
   - Web accessible resources: control bar assets
   - OAuth2: configure for Google Sign-In
   - Reference: FRONTEND_PLAN.md Task 3.1

2. **Popup Interface** (extension/src/popup/)
   - Create popup/index.html (minimal, loads React app)
   - Create popup/Popup.tsx (main component):
     * Get current tab URL to detect if on Meet
     * If on Meet: show recording controls
     * If not on Meet: show message "Navigate to Google Meet to start recording"
     * Recording states: idle, recording, paused, uploading
     * Display: current recording duration (MM:SS format)
     * Buttons: Start Recording, Pause, Resume, Stop, Settings (opens options)
     * Status indicator: badge showing state (idle/recording/paused)
   - State sync: listen for messages from background to update UI
   - Styling: compact design (400px width, ~600px height max)
   - Reference: FRONTEND_PLAN.md Task 3.2, UIUX_PLAN.md Section 4.3.1

3. **Control Bar Overlay** (extension/src/content/ControlBar.tsx)
   - Inject floating control bar into Meet page DOM
   - Position: fixed, bottom-center, z-index 9999
   - Container: semi-transparent black background, rounded
   - Controls layout:
     * Recording indicator: pulsing red dot + "Recording" text
     * Duration timer: MM:SS format, updates every second
     * Pause/Resume button: toggle between states
     * Stop button: ends recording
     * Minimize button: collapse to small badge
   - Draggable: allow user to reposition (save position to chrome.storage)
   - Minimize state: show only red dot + duration, expand on hover
   - State management: receive updates from background script
   - Prevent Meet interference: do not capture control bar in recording
   - Reference: FRONTEND_PLAN.md Task 3.3, UIUX_PLAN.md Section 4.3.2

4. **Content Script** (extension/src/content/index.ts)
   - Inject control bar when recording starts
   - Remove control bar when recording ends
   - Listen for messages from background:
     * START_RECORDING: show control bar
     * STOP_RECORDING: hide control bar
     * UPDATE_DURATION: update timer
     * UPDATE_STATUS: update pause/resume button
   - Send messages to background:
     * PAUSE_RECORDING: user clicked pause
     * RESUME_RECORDING: user clicked resume
     * STOP_RECORDING: user clicked stop
   - Detect Meet page: check URL pattern
   - Handle SPA navigation: detect when user navigates away from Meet
   - Reference: FRONTEND_PLAN.md Task 3.4

5. **Options Page** (extension/src/options/)
   - Create options/index.html (full page, loads React app)
   - Create options/Options.tsx (main component):
     * Tabbed interface: General, Recording, Storage, About
     * **General Tab:**
       - Auto-start recording on Meet join (toggle)
       - Show notifications (toggle)
       - Recording quality (dropdown: 1080p, 720p, 480p)
     * **Recording Tab:**
       - Microphone source (dropdown of available mics)
       - Include tab audio (toggle)
       - Include webcam (toggle, future feature)
       - Chunk size (slider: 50-200MB, default 100MB)
     * **Storage Tab:**
       - Google Drive folder location (text input)
       - Storage usage display (progress bar)
       - Clear cache button
     * **About Tab:**
       - Version info
       - Links to privacy policy, terms, support
       - Sign out button
   - Save settings: write to chrome.storage.sync
   - Load settings: read on mount
   - Validate inputs: show errors for invalid values
   - Reference: FRONTEND_PLAN.md Task 3.5, UIUX_PLAN.md Section 4.3.4

6. **Background Service Worker** (extension/src/background/index.ts)
   - Initialize on extension install
   - Message handler: chrome.runtime.onMessage.addListener
   - Handle messages:
     * START_RECORDING: get tab stream, initialize MediaRecorder, start capture
     * STOP_RECORDING: stop MediaRecorder, finalize upload, create Firestore doc
     * PAUSE_RECORDING: pause MediaRecorder
     * RESUME_RECORDING: resume MediaRecorder
     * GET_STATUS: return current recording state
   - State management: maintain recording state in memory
   - Tab capture: use chrome.tabCapture.capture() for screen + audio
   - Storage: use chrome.storage.local for temporary data
   - Broadcast state: send messages to popup and content script when state changes
   - Error handling: catch and display errors to user
   - Reference: FRONTEND_PLAN.md Task 3.6, PLAN_HANDOFF_FRONTEND.md Section 7.3

7. **Authentication Flow** (extension/src/popup/Auth.tsx)
   - Sign in screen: Google Sign-In button
   - Use chrome.identity.launchWebAuthFlow for OAuth
   - Store access token in chrome.storage.local
   - Check auth status on popup open
   - If not authenticated: show sign-in screen
   - If authenticated: show main interface
   - Sign out: clear tokens, reset state
   - Reference: FRONTEND_PLAN.md Task 3.7

8. **Message Passing System** (extension/src/utils/messaging.ts)
   - Create typed message interfaces:
     ```typescript
     enum MessageType {
       START_RECORDING = 'START_RECORDING',
       STOP_RECORDING = 'STOP_RECORDING',
       PAUSE_RECORDING = 'PAUSE_RECORDING',
       RESUME_RECORDING = 'RESUME_RECORDING',
       UPDATE_STATUS = 'UPDATE_STATUS',
       UPDATE_DURATION = 'UPDATE_DURATION',
       GET_STATUS = 'GET_STATUS',
     }
     
     interface Message<T = any> {
       type: MessageType;
       payload?: T;
     }
     ```
   - Helper functions:
     * sendMessage(type, payload): send to background
     * sendToTab(tabId, type, payload): send to content script
     * onMessage(callback): typed listener
   - Error handling: handle message failures
   - Reference: FRONTEND_PLAN.md Task 3.8

**Implementation Guidelines:**

**Chrome APIs:**
- chrome.runtime: message passing, extension info
- chrome.storage: settings and state persistence
- chrome.tabs: query active tab, inject scripts
- chrome.tabCapture: capture Meet tab video/audio
- chrome.identity: OAuth authentication
- chrome.notifications: show system notifications (optional)

**State Synchronization:**
- Background worker is source of truth for recording state
- Popup and content script listen for state updates
- Use chrome.storage.onChanged for settings sync
- Debounce frequent updates (duration timer)

**Error Handling:**
- Permission denied: guide user to grant permissions
- Tab capture failed: show error message
- Storage full: notify user, stop recording
- Authentication failed: show sign-in screen

**Testing:**
- Load extension in Chrome (chrome://extensions)
- Test popup on Meet tab vs non-Meet tab
- Test recording start/pause/resume/stop flow
- Test control bar positioning and dragging
- Test options page save/load
- Test authentication flow
- Test message passing between components

**File Structure:**
```
extension/
├── public/
│   ├── manifest.json
│   └── icons/
│       ├── icon16.png
│       ├── icon48.png
│       └── icon128.png
├── src/
│   ├── popup/
│   │   ├── index.html
│   │   ├── index.tsx
│   │   ├── Popup.tsx
│   │   └── Auth.tsx
│   ├── content/
│   │   ├── index.ts
│   │   ├── ControlBar.tsx
│   │   └── styles.css
│   ├── background/
│   │   └── index.ts
│   ├── options/
│   │   ├── index.html
│   │   ├── index.tsx
│   │   └── Options.tsx
│   └── utils/
│       └── messaging.ts
└── vite.config.ts
```

**Critical Requirements:**
- Manifest V3 compliance (no Manifest V2 features)
- All UI must match UIUX_PLAN.md specifications
- Message passing must be typed and reliable
- State must sync across all components
- Extension must not interfere with Meet UI
- Control bar must not appear in recording
- All Chrome API calls must handle errors

**Validation Checklist:**
Before marking this phase complete, verify:
- [ ] Extension loads without errors in Chrome
- [ ] Popup opens and displays correct state
- [ ] Popup shows different UI on Meet vs non-Meet tabs
- [ ] Start recording button initiates recording
- [ ] Control bar appears when recording starts
- [ ] Control bar shows correct duration (updates every second)
- [ ] Pause/Resume buttons work
- [ ] Stop button ends recording
- [ ] Control bar is draggable
- [ ] Options page loads and displays settings
- [ ] Settings save and persist
- [ ] Authentication flow works
- [ ] Message passing works between all components
- [ ] No console errors during normal operation
- [ ] Extension icon shows correct badge state

**Deliverables:**
- Complete popup interface with auth and recording controls
- Floating control bar overlay
- Full-featured options page
- Background service worker with recording logic
- Content script for control bar injection
- Typed message passing system
- Updated manifest.json

**If You Encounter Issues:**
- Manifest errors: Verify Manifest V3 schema compliance
- Tab capture not working: Check permissions and host_permissions
- Messages not delivering: Ensure listeners are set up before sending
- Control bar positioning: Use fixed position with high z-index
- State sync issues: Use background as single source of truth

Load the extension in Chrome and test the complete recording workflow before marking complete.
```

### Phase 3 Completion Criteria
- ✅ Extension loads in Chrome without errors
- ✅ Popup UI functional with all states
- ✅ Control bar injects and displays correctly
- ✅ Options page saves/loads settings
- ✅ Message passing works across components
- ✅ Authentication flow complete

**Estimated Time:** 3-4 days  
**Next Phase:** Phase 4 - Dashboard Layout & Navigation

---

## Phase 4: Dashboard Layout & Navigation

**Duration:** 2-3 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.4

### Phase Objective
Build the complete dashboard application structure with React Router, responsive layout with sidebar navigation and header, protected routes, and user profile management. Establish the shell that all subsequent dashboard features will plug into.

### Context & Background
The dashboard is a React SPA that displays recordings, handles playback, and manages user settings. The layout consists of:
- **Sidebar** (left): Navigation links, user profile, collapse toggle
- **Header** (top): Page title, search, notifications, user menu
- **Main Content Area**: Route-specific pages (Home, Recordings, Settings, Detail)

Authentication is required for all routes except the landing page. Firebase Authentication manages user sessions.

### Full Prompt

```
You are implementing Phase 4 of the OpenMeet Recorder frontend development. This phase builds the dashboard layout, navigation, and routing infrastructure.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.4 (Phase 4 detailed tasks)
- UIUX_PLAN.md Section 5.1 (Dashboard layout specifications)
- PLAN_HANDOFF_FRONTEND.md Section 6.2.1 (Layout architecture)

**Phase Scope - Complete ALL of the following:**

1. **React Router Setup** (dashboard/src/App.tsx)
   - Install and configure react-router-dom v6.21+
   - Define routes:
     * "/" - Landing page (public)
     * "/dashboard" - Home (redirects to /dashboard/recordings)
     * "/dashboard/recordings" - Recordings list
     * "/dashboard/recordings/:id" - Recording detail
     * "/dashboard/settings" - Settings page
     * "/dashboard/profile" - User profile
     * "*" - 404 Not Found page
   - Protected routes: require authentication for /dashboard/*
   - Redirect: authenticated users accessing "/" redirect to "/dashboard/recordings"
   - Reference: FRONTEND_PLAN.md Task 4.1

2. **DashboardLayout Component** (dashboard/src/components/DashboardLayout.tsx)
   - Layout structure:
     ```
     <div className="flex h-screen overflow-hidden">
       <Sidebar />
       <div className="flex-1 flex flex-col">
         <Header />
         <main className="flex-1 overflow-auto p-6">
           <Outlet /> {/* React Router outlet */}
         </main>
       </div>
     </div>
     ```
   - Responsive: sidebar collapses to mobile menu on screens < 768px
   - Sidebar state: manage collapsed state with UI store
   - Background: bg-gray-50
   - Reference: FRONTEND_PLAN.md Task 4.2, UIUX_PLAN.md Section 5.1

3. **Sidebar Component** (dashboard/src/components/Sidebar.tsx)
   - Width: 240px (expanded), 64px (collapsed)
   - Background: bg-white with border-r border-gray-200
   - Sections:
     * **Logo/Brand** (top):
       - Logo icon + "OpenMeet" text (hide text when collapsed)
       - Click: navigate to /dashboard/recordings
     * **Navigation Links** (middle):
       - Home (icon: Home, path: /dashboard/recordings)
       - Settings (icon: Settings, path: /dashboard/settings)
       - Profile (icon: User, path: /dashboard/profile)
       - Each link: icon + label, active state highlighting
       - Active: bg-primary-50, text-primary-600, border-l-3 border-primary-600
       - Hover: bg-gray-50
     * **User Profile** (bottom):
       - Avatar (user photo or initials)
       - Name and email (hide when collapsed)
       - Dropdown on click: View Profile, Settings, Sign Out
     * **Collapse Toggle** (bottom):
       - Button: ChevronLeft/ChevronRight icon
       - Click: toggle sidebar collapsed state
   - Mobile: slide-in overlay (fixed, z-50) with backdrop
   - Keyboard: close on ESC key
   - Reference: FRONTEND_PLAN.md Task 4.3, UIUX_PLAN.md Section 5.1.1

4. **Header Component** (dashboard/src/components/Header.tsx)
   - Height: 64px
   - Background: bg-white with border-b border-gray-200
   - Layout: flex items-center justify-between px-6
   - Left section:
     * Mobile menu button (icon: Menu, shows only on mobile)
     * Page title: dynamic based on current route
       - "Recordings" for /dashboard/recordings
       - "Recording Detail" for /dashboard/recordings/:id
       - "Settings" for /dashboard/settings
       - "Profile" for /dashboard/profile
   - Center section:
     * Search bar (icon: Search, placeholder: "Search recordings...")
     * Input: trigger search, update recordings filter
     * Keyboard: focus on CMD/CTRL + K
   - Right section:
     * Notifications bell (icon: Bell, badge if unread)
     * User menu: Avatar + dropdown
       - Dropdown items: Profile, Settings, Sign Out
   - Reference: FRONTEND_PLAN.md Task 4.4, UIUX_PLAN.md Section 5.1.2

5. **Protected Route Wrapper** (dashboard/src/components/ProtectedRoute.tsx)
   - Higher-order component or route guard
   - Check authentication: use authStore
   - If authenticated: render children (Outlet)
   - If not authenticated: redirect to "/" with state for redirect after login
   - Loading: show spinner while checking auth
   - Implementation:
     ```tsx
     function ProtectedRoute() {
       const { isAuthenticated, loading } = useAuthStore();
       const location = useLocation();
       
       if (loading) return <LoadingScreen />;
       if (!isAuthenticated) {
         return <Navigate to="/" state={{ from: location }} replace />;
       }
       return <Outlet />;
     }
     ```
   - Reference: FRONTEND_PLAN.md Task 4.5

6. **Landing Page** (dashboard/src/pages/Landing.tsx)
   - Public page: not behind authentication
   - Hero section:
     * Headline: "Record and Summarize Your Google Meet Sessions"
     * Subheadline: description of features
     * CTA button: "Get Started" (opens Google Sign-In)
   - Features section: 3 key features with icons
   - Footer: links to privacy policy, terms, contact
   - If user is authenticated: redirect to /dashboard/recordings
   - Reference: FRONTEND_PLAN.md Task 4.6

7. **Loading & Error Pages** (dashboard/src/pages/)
   - **LoadingScreen.tsx**: full-screen spinner with logo
   - **NotFound.tsx**: 404 page with message and link to home
   - **ErrorPage.tsx**: generic error page for unexpected errors
   - Reference: FRONTEND_PLAN.md Task 4.7

8. **Navigation Utilities** (dashboard/src/hooks/useNavigation.ts)
   - Custom hook for programmatic navigation
   - Functions:
     * navigateToRecordings()
     * navigateToDetail(recordingId)
     * navigateToSettings()
     * navigateBack()
   - Use useNavigate from react-router-dom
   - Type-safe navigation with TypeScript

**Implementation Guidelines:**

**Routing:**
- Use React Router v6 API (createBrowserRouter or BrowserRouter)
- Lazy load route components with React.lazy and Suspense
- Use Outlet for nested routes
- Handle 404s with catch-all route

**State Management:**
- UI store: sidebar collapsed state, mobile menu open
- Auth store: user, isAuthenticated, loading
- Sync sidebar state across refreshes (persist to localStorage)

**Responsive Behavior:**
- Breakpoint: 768px (Tailwind md:)
- Mobile: sidebar becomes slide-in drawer
- Mobile: show hamburger menu button in header
- Tablet/Desktop: sidebar always visible, can collapse

**Keyboard Navigation:**
- CMD/CTRL + K: focus search
- ESC: close mobile menu, close dropdowns
- Arrow keys: navigate through dropdown items

**Accessibility:**
- Sidebar: nav element with aria-label="Main navigation"
- Skip to content link (optional)
- Focus management: trap focus in mobile menu
- Announce route changes to screen readers

**Testing:**
- Navigate through all routes
- Test authentication redirect flow
- Test sidebar collapse/expand
- Test mobile menu open/close
- Test search bar focus and input
- Test user menu dropdown
- Verify active link highlighting

**File Structure:**
```
dashboard/src/
├── App.tsx (router setup)
├── components/
│   ├── DashboardLayout.tsx
│   ├── Sidebar.tsx
│   ├── Header.tsx
│   └── ProtectedRoute.tsx
├── pages/
│   ├── Landing.tsx
│   ├── LoadingScreen.tsx
│   ├── NotFound.tsx
│   └── ErrorPage.tsx
├── hooks/
│   └── useNavigation.ts
└── routes/
    └── index.tsx (route definitions)
```

**Critical Requirements:**
- All routes must be properly defined and working
- Protected routes must redirect unauthenticated users
- Layout must be fully responsive (mobile, tablet, desktop)
- Sidebar collapse state must persist
- Active navigation link must highlight correctly
- Search bar must be functional (even if filter logic comes in Phase 5)
- User dropdown must work with sign out

**Validation Checklist:**
Before marking this phase complete, verify:
- [ ] All routes navigate correctly
- [ ] Protected routes redirect when not authenticated
- [ ] Sidebar displays navigation links with icons
- [ ] Sidebar collapses and expands correctly
- [ ] Sidebar state persists across page refreshes
- [ ] Mobile menu opens and closes
- [ ] Header displays correct page title for each route
- [ ] Search bar is focusable and accepts input
- [ ] User menu dropdown opens and has all items
- [ ] Active navigation link highlights correctly
- [ ] 404 page shows for invalid routes
- [ ] No console errors during navigation
- [ ] Responsive: layout works on mobile, tablet, desktop

**Deliverables:**
- Complete routing configuration with all routes
- DashboardLayout with Sidebar and Header
- Protected route wrapper
- Landing page with authentication CTA
- Loading and error pages
- Navigation utilities hook

**If You Encounter Issues:**
- Routing not working: Check BrowserRouter is wrapping App
- Protected routes not redirecting: Verify authStore isAuthenticated logic
- Layout breaking on mobile: Test responsive Tailwind classes (md:, lg:)
- Sidebar state not persisting: Check UI store persist middleware

Navigate through all routes and test responsive behavior before marking complete.
```

### Phase 4 Completion Criteria
- ✅ All routes defined and navigating
- ✅ Protected routes enforcing authentication
- ✅ Responsive layout working on all screen sizes
- ✅ Sidebar and header fully functional
- ✅ Active link highlighting correct
- ✅ Mobile menu working

**Estimated Time:** 2-3 days  
**Next Phase:** Phase 5 - Recording List & Display

---

## Phase 5: Recording List & Display

**Duration:** 3-4 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.5

### Phase Objective
Implement the recordings list page with comprehensive filtering, sorting, and search functionality. Build recording cards that display in both grid and list views. Integrate with Firestore to fetch and display user's recordings.

### Context & Background
This is the main dashboard page where users see all their recordings. The interface must support:
- Two view modes: Grid (cards) and List (table-like rows)
- Comprehensive filtering: date range, duration, file size, status, starred, tags
- Sorting: by date, duration, size, title (ascending/descending)
- Real-time search across titles and descriptions
- Empty states for no recordings or no results

### Full Prompt

```
You are implementing Phase 5 of the OpenMeet Recorder frontend development. This phase builds the complete recordings list with filtering, sorting, and search.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.5 (Phase 5: 12 detailed tasks)
- UIUX_PLAN.md Section 5.2 (List view specifications)
- PLAN_HANDOFF_FRONTEND.md Section 6.2.2 (List requirements)

**IMPORTANT:** This phase has 12 tasks. Reference FRONTEND_PLAN.md Section 5.5 for complete specifications of each component. Implement ALL tasks listed there including:
- RecordingCard (grid & list variants)
- GridView & ListView layouts
- ViewToggle button
- FilterPanel (7 filter types: date range, duration, file size, status, starred, tags, search)
- ActiveFilterTags display
- Sort Dropdown (6 sort options)
- EmptyState component
- SkeletonCard loading state
- Firestore integration (getRecordings function)
- useRecordings custom hook
- Recordings page component integration

**Phase Summary:**
Build the recordings list page (/dashboard/recordings) with grid/list view toggle, comprehensive filtering (date, duration, size, status, starred, tags), sorting (date, duration, size, title), search functionality, and Firestore integration. Each recording card shows thumbnail, title, duration, date, file size, status badge, starred toggle, and action menu.

**Critical Implementation Requirements:**
1. **RecordingCard Component** - Two variants (grid and list) with thumbnail, metadata, badges, star toggle, and actions menu
2. **View Toggle** - Switch between grid (3-4 columns) and list (table-like) views, persist preference
3. **FilterPanel** - Collapsible panel with 7 filter types, apply/reset buttons, active filter count badge
4. **Search** - Debounced search across title and description fields
5. **Sorting** - Dropdown with 6 options (date, duration, size, title × asc/desc)
6. **Firestore Integration** - Fetch recordings for current user, real-time updates, loading/error states
7. **Empty States** - Different messages for "no recordings" vs "no results found"
8. **Skeleton Loading** - Show 6-8 skeleton cards while loading

**File Structure:**
```
dashboard/src/pages/Recordings.tsx (main page)
dashboard/src/components/recordings/
  ├── RecordingCard.tsx
  ├── GridView.tsx
  ├── ListView.tsx
  ├── ViewToggle.tsx
  ├── FilterPanel.tsx
  ├── ActiveFilterTags.tsx
  ├── SortDropdown.tsx
  ├── EmptyState.tsx
  └── SkeletonCard.tsx
dashboard/src/hooks/useRecordings.ts
dashboard/src/services/firestore.ts (getRecordings function)
```

**Validation Checklist:**
- [ ] Both grid and list views render correctly
- [ ] View toggle switches views and persists preference
- [ ] All 7 filters work and can be combined
- [ ] Search filters results with debouncing (300ms)
- [ ] Sort dropdown applies sorting correctly
- [ ] Active filter tags display and can be removed
- [ ] Star toggle works with optimistic updates
- [ ] Action menu opens with Edit/Share/Delete options
- [ ] Empty state shows when no recordings
- [ ] Skeleton cards show while loading
- [ ] Firestore integration fetches user's recordings
- [ ] No console errors or warnings

For complete implementation details of each component, refer to FRONTEND_PLAN.md Section 5.5 Tasks 5.1 through 5.12.
```

### Phase 5 Completion Criteria
- ✅ Recording cards display in grid and list views
- ✅ All filters, sorting, and search working
- ✅ Firestore integration fetching recordings
- ✅ Loading and empty states implemented
- ✅ View preference persists

**Estimated Time:** 3-4 days  
**Next Phase:** Phase 6 - Recording Detail & Playback

---

## Phase 6: Recording Detail & Playback

**Duration:** 5-6 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.6

### Phase Objective
Build the complete recording detail page with custom video player (including controls, progress bar, volume, playback speed), interactive transcript synchronized with video, AI summary display, and editable details tab. Integrate video playback from Google Drive and all data from Firestore.

### Context & Background
This is the most complex UI page, featuring:
- **Custom Video Player**: HTML5 video with custom controls overlay (play/pause, seek, volume, speed, PiP, fullscreen)
- **Interactive Transcript**: Click timestamps to jump in video, auto-scroll active entry, search functionality
- **AI Summary**: Display overview, key points, action items, topics, participants
- **Details Tab**: Editable metadata, tags management, privacy settings, delete action

The page must handle video streaming from Google Drive (signed URLs), real-time transcript synchronization, and all CRUD operations for recording metadata.

### Full Prompt

```
You are implementing Phase 6 of the OpenMeet Recorder frontend development. This phase builds the recording detail page with video player, transcript, AI summary, and details management.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.6 (Phase 6: 10 detailed tasks)
- UIUX_PLAN.md Section 5.5 (Detail view specifications)
- PLAN_HANDOFF_FRONTEND.md Section 9.3-9.4 (Video player and transcript implementation)

**IMPORTANT:** This phase has 10 complex tasks. Reference FRONTEND_PLAN.md Section 5.6 for complete specifications including:
- Detail page layout with breadcrumbs and action bar
- VideoPlayer component with custom controls
- ProgressBar (seekable, time tooltip, buffered progress)
- Control buttons (play/pause, volume, skip, speed, quality, PiP, fullscreen)
- VolumeControl with slider
- Keyboard shortcuts (space, arrows, F, M, J/L, 0-9)
- TranscriptView with interactive entries and search
- AISummaryView with all sections (overview, key points, action items, topics, participants)
- DetailsTab with editable fields and tags management
- Data integration (Firestore and Google Drive)

**Phase Summary:**
Build the recording detail page (/dashboard/recordings/:id) with full-featured video player (custom controls, seekable progress, volume, speed, PiP, fullscreen, keyboard shortcuts), interactive transcript (click-to-jump, auto-scroll, search), AI summary display (all sections), and editable details (title, description, tags, privacy, delete). Integrate video playback from Google Drive signed URLs and all data from Firestore.

**Critical Implementation Requirements:**
1. **Custom Video Player** - HTML5 video with custom controls overlay, all playback controls, keyboard shortcuts, responsive
2. **Progress Bar** - Seekable with click/drag, time tooltip on hover, buffered progress indicator
3. **Transcript Synchronization** - Active entry highlights and auto-scrolls as video plays, click timestamp to jump
4. **AI Summary** - Display all sections with proper formatting, copy and export functionality
5. **Editable Details** - Inline editing for title/description, tags management, privacy settings
6. **Video Streaming** - Fetch signed URL from Google Drive, handle URL expiration/refresh
7. **State Management** - useVideoPlayer hook for player state, sync transcript with currentTime
8. **Data Integration** - getRecording, updateRecording, deleteRecording Firestore functions

**File Structure:**
```
dashboard/src/pages/Detail.tsx (main page)
dashboard/src/components/detail/
  ├── VideoPlayer.tsx
  ├── ProgressBar.tsx
  ├── PlayerControls.tsx
  ├── VolumeControl.tsx
  ├── TranscriptView.tsx
  ├── TranscriptEntry.tsx
  ├── AISummaryView.tsx
  ├── DetailsTab.tsx
  └── PropertyRow.tsx
dashboard/src/hooks/useVideoPlayer.ts
dashboard/src/services/
  ├── firestore.ts (getRecording, updateRecording, deleteRecording)
  └── storage.ts (getVideoUrl from Drive)
```

**Validation Checklist:**
- [ ] Video player loads and plays from Google Drive URL
- [ ] All player controls work (play, pause, seek, volume)
- [ ] Progress bar is seekable with time tooltip
- [ ] Keyboard shortcuts function correctly
- [ ] PiP and fullscreen modes work
- [ ] Transcript displays and syncs with video
- [ ] Clicking transcript timestamp jumps video
- [ ] Transcript search highlights matches
- [ ] AI summary displays all sections
- [ ] Title and description are editable inline
- [ ] Tags can be added/removed
- [ ] Delete recording works with confirmation
- [ ] All updates save to Firestore
- [ ] Loading/error states handled

For complete implementation details of each component (including exact specifications for controls layout, styling, event handlers, and state management), refer to FRONTEND_PLAN.md Section 5.6 Tasks 6.1 through 6.10.
```

### Phase 6 Completion Criteria
- ✅ Custom video player fully functional
- ✅ Transcript synchronized with video
- ✅ AI summary displaying all sections
- ✅ Details editable and saving to Firestore
- ✅ Video streaming from Google Drive
- ✅ All interactions working

**Estimated Time:** 5-6 days  
**Next Phase:** Phase 7 - State Management & Integration

---

## Phase 7: State Management & Integration

**Duration:** 6-7 days  
**Priority:** 🔴 Critical  
**Reference:** FRONTEND_PLAN.md Section 5.7

### Phase Objective
Implement complete state management with Zustand stores, integrate MediaRecorder API for recording logic, build chunked upload system to Google Drive, create API service layer for Firebase/Firestore/Drive, implement message passing between extension scripts, add error handling and retry logic, and test end-to-end workflows.

### Context & Background
This is the final and most critical phase that ties everything together. It implements:
- **Recording Store**: MediaRecorder API integration, chunk-based recording, background uploads to Google Drive (100MB chunks)
- **Auth Store**: Firebase Authentication with Google OAuth, token management
- **Recordings Store**: Firestore integration, real-time updates, filtering/sorting logic
- **UI Store**: Global UI state (sidebar, modals, toasts)
- **API Services**: Abstraction layer for all external API calls
- **Message Passing**: Typed communication between extension components
- **Error Handling**: Global error handler, retry logic with exponential backoff

This phase makes the entire application functional end-to-end.

### Full Prompt

```
You are implementing Phase 7 (FINAL PHASE) of the OpenMeet Recorder frontend development. This phase implements all state management, recording logic, API integrations, and connects everything together.

**Reference Documents:**
- FRONTEND_PLAN.md Section 5.7 (Phase 7: 8 detailed tasks)
- PLAN_HANDOFF_FRONTEND.md Section 8 (State management architecture)
- PLAN_HANDOFF_FRONTEND.md Section 9 (Core implementation details)

**IMPORTANT:** This is the most complex phase with 8 major tasks. Reference FRONTEND_PLAN.md Section 5.7 for complete specifications including:
- Recording Store (MediaRecorder, chunk handling, upload logic)
- Auth Store (Firebase Auth, Google OAuth, token refresh)
- Recordings Store (Firestore queries, real-time updates, filters, sorting)
- UI Store (sidebar, modals, toasts)
- API Service Layer (Firebase, Firestore, Google Drive clients)
- Message Passing System (typed messages between extension scripts)
- Error Handling (global handler, retry logic with exponential backoff)
- Integration Testing (end-to-end recording and dashboard workflows)

**Phase Summary:**
Implement all Zustand stores (recording, auth, recordings, UI), integrate MediaRecorder API with chunked upload to Google Drive (100MB chunks), build complete API service layer (Firebase Auth, Firestore CRUD, Drive upload/fetch), implement typed message passing between extension components, add global error handling with retry logic, and test complete end-to-end workflows (extension recording → upload → dashboard display → playback).

**Critical Implementation Requirements:**
1. **Recording Store** - MediaRecorder initialization, start/pause/resume/stop actions, chunk collection (10s intervals), resumable upload to Google Drive (100MB chunks), progress tracking, error handling
2. **Chunk Upload System** - Google Drive Resumable Upload API, sequential chunk uploads, progress updates, handle network failures, resume interrupted uploads
3. **Auth Store** - Firebase Auth initialization, Google OAuth with popup, token storage and refresh, auth state observer, sign in/out actions
4. **Recordings Store** - Firestore queries (fetch user recordings), real-time snapshot listeners, filter/sort logic, optimistic updates for star/delete
5. **UI Store** - Sidebar collapse state, modal management, toast system (auto-dismiss, queue), theme preference (future)
6. **API Services** - firebase.ts (init), firestore.ts (CRUD operations), storage.ts (Drive upload/fetch), api.ts (backend client with interceptors)
7. **Message Passing** - Typed message interfaces, sendMessage helpers, typed listeners, error handling for message failures
8. **Error Handling** - AppError class, global handleError function, retry utility with exponential backoff, network status detection

**File Structure:**
```
extension/src/stores/recordingStore.ts
dashboard/src/stores/
  ├── authStore.ts
  ├── recordingsStore.ts
  └── uiStore.ts
dashboard/src/services/
  ├── firebase.ts
  ├── firestore.ts
  ├── storage.ts
  └── api.ts
extension/src/utils/
  ├── messaging.ts
  └── errorHandler.ts
dashboard/src/utils/
  ├── errorHandler.ts
  └── retry.ts
```

**Validation Checklist:**
- [ ] Recording starts from extension popup
- [ ] MediaRecorder captures video + audio
- [ ] Chunks collected every 10 seconds
- [ ] Chunks upload to Drive in background
- [ ] Upload progress updates in UI
- [ ] Pause/resume work during recording
- [ ] Stop completes all uploads and creates Firestore doc
- [ ] Auth flow works (sign in, sign out, token refresh)
- [ ] Dashboard loads user's recordings from Firestore
- [ ] Real-time updates reflected when recordings added/modified
- [ ] Filters and sort work correctly
- [ ] Star/delete work with optimistic updates
- [ ] Errors display user-friendly messages
- [ ] Retry logic works for failed API calls
- [ ] Network offline/online detection works
- [ ] Message passing between extension scripts works
- [ ] No memory leaks during long sessions

For complete implementation details of each store, service, and system (including exact state interfaces, action implementations, API functions, and error handling patterns), refer to FRONTEND_PLAN.md Section 5.7 Tasks 7.1 through 7.8.
```

### Phase 7 Completion Criteria
- ✅ All stores implemented and working
- ✅ Recording with MediaRecorder functional
- ✅ Chunked uploads to Google Drive working
- ✅ Auth flow complete with Firebase
- ✅ Firestore integration working
- ✅ Message passing functional
- ✅ Error handling comprehensive
- ✅ End-to-end workflows tested

**Estimated Time:** 6-7 days  
**Next Section:** Final Validation

---

## Final Validation

### Complete System Testing

After completing all 7 phases, perform comprehensive end-to-end testing before marking the Frontend implementation complete.

**Extension Workflow Test:**
1. Open Chrome and load extension
2. Navigate to Google Meet
3. Open extension popup
4. Sign in with Google account
5. Start recording from popup
6. Verify control bar appears on Meet page
7. Record for 2 minutes
8. Pause recording - verify pause works
9. Resume recording - verify resume works
10. Stop recording
11. Verify chunks upload (check console logs)
12. Verify Firestore document created
13. Check no console errors

**Dashboard Workflow Test:**
1. Open dashboard in browser
2. Sign in (should use same Google account)
3. Verify recordings list loads
4. Test grid view - all cards display correctly
5. Test list view - all rows display correctly
6. Apply filters - verify results update
7. Apply sorting - verify order changes
8. Search for recording - verify search works
9. Click recording - navigate to detail page
10. Verify video loads and plays
11. Test all player controls
12. Verify transcript displays and syncs
13. Click transcript timestamp - verify video jumps
14. Verify AI summary displays
15. Edit title - verify saves
16. Add tags - verify saves
17. Star recording - verify updates
18. Delete recording - verify confirmation and deletion
19. Check no console errors

**Cross-Component Testing:**
- Extension and dashboard show same recordings
- Real-time updates: recording added in extension appears in dashboard immediately
- Auth state synced: sign out in dashboard, extension reflects this
- Settings sync: changes in extension options reflected across app

**Performance Testing:**
- Dashboard loads within 2 seconds
- Recordings list renders smoothly with 100+ recordings
- Video playback starts within 1 second
- No memory leaks during extended use
- No frame drops during recording

**Accessibility Testing:**
- All interactive elements keyboard accessible
- Tab order logical
- Focus indicators visible
- Screen reader announces status changes (optional)
- Color contrast meets WCAG AA standards

**Error Scenarios Testing:**
- Network disconnects during upload - uploads resume when reconnected
- User denies permissions - error message displayed with guidance
- Firestore write fails - error displayed, retry option available
- Drive quota exceeded - clear error message
- Invalid video file - graceful error handling
- Expired auth token - auto-refresh or prompt to re-authenticate

### Success Criteria

The Frontend implementation is complete when:
- ✅ All 7 phases implemented according to FRONTEND_PLAN.md
- ✅ Extension fully functional (recording, uploading, state management)
- ✅ Dashboard fully functional (display, playback, editing)
- ✅ All components match UIUX_PLAN.md specifications
- ✅ TypeScript compiles with no errors
- ✅ ESLint passes with no warnings
- ✅ All end-to-end workflows tested successfully
- ✅ No critical console errors
- ✅ Accessibility standards met
- ✅ Performance targets achieved

### Handoff to Backend Agent

Once Frontend implementation is validated and complete:
1. Document all API endpoints used (for Backend to implement)
2. Document Firestore data model (collections, fields, indexes)
3. Document Google Drive API usage (upload, fetch)
4. Document authentication flow (Firebase Auth with Google)
5. Note any mock data or stubs that need real backend implementation
6. Create handoff document: PLAN_HANDOFF_BACKEND.md (if not exists)

### Final Deliverables

- Complete Chrome extension (all UI and logic)
- Complete dashboard application (all pages and features)
- All Zustand stores implemented
- All API services implemented
- Complete test coverage of workflows
- Documentation of known issues or limitations
- README with setup and development instructions

---

## Appendix: Quick Reference

### Phase Duration Estimates
- Phase 1: 1-2 days (Setup)
- Phase 2: 2-3 days (Components)
- Phase 3: 3-4 days (Extension)
- Phase 4: 2-3 days (Layout)
- Phase 5: 3-4 days (List)
- Phase 6: 5-6 days (Detail)
- Phase 7: 6-7 days (Integration)
- **Total: 23-33 days (~5-7 weeks)**

### Key Dependencies
- Phase 2 depends on Phase 1 (components need design system)
- Phase 3 depends on Phase 2 (extension uses shared components)
- Phase 4 depends on Phase 2 (layout uses shared components)
- Phase 5 depends on Phase 4 (list page within layout)
- Phase 6 depends on Phase 4 (detail page within layout)
- Phase 7 depends on all phases (integrates everything)

### Critical Documents
- **FRONTEND_PLAN.md** - Complete implementation specifications
- **UIUX_PLAN.md** - Design system and UI specifications
- **PLAN_HANDOFF_FRONTEND.md** - Architecture and handoff details

### Technology Stack
- **Extension**: React 18.2+, TypeScript 5.3+, Vite 5.0+, Zustand 4.5+, Dexie.js 3.2+
- **Dashboard**: React 18.2+, TypeScript 5.3+, Vite 5.0+, React Router 6.21+, Zustand 4.5+, Firebase 10.0+
- **Shared**: TailwindCSS 3.4+, Radix UI primitives, Lucide icons

---

**End of Frontend Agent Prompts**

*For detailed task-by-task specifications, always refer to FRONTEND_PLAN.md*




