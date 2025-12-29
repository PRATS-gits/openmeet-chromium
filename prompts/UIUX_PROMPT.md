# OpenMeet Recorder - UI/UX Agent Prompts

> **Purpose:** Task-by-task prompts for the UI/UX Agent  
> **Reference Document:** [plans/UIUX_PLAN.md](../plans/UIUX_PLAN.md)  
> **Created:** 2025-12-29  
> **Version:** 1.0.0

---

## Table of Contents
1. [How to Use This Document](#how-to-use-this-document)
2. [Phase 1: Design System Setup](#phase-1-design-system-setup)
3. [Phase 2: Extension UI Components](#phase-2-extension-ui-components)
4. [Phase 3: Dashboard Core Components](#phase-3-dashboard-core-components)
5. [Phase 4: Dashboard Detail View](#phase-4-dashboard-detail-view)
6. [Phase 5: Interaction & Animation Polish](#phase-5-interaction--animation-polish)
7. [Phase 6: Documentation & Handoff](#phase-6-documentation--handoff)

---

## How to Use This Document

### For Users:
1. **Sequential Execution:** Execute prompts in order from top to bottom
2. **One Prompt at a Time:** Complete each task before moving to the next
3. **Review Deliverables:** Check the output against the deliverables list after each task
4. **Provide Context:** When giving a prompt to the UI/UX Agent, reference the UIUX_PLAN.md for full specifications
5. **Ask for Clarification:** If the agent's output doesn't match expectations, refer back to UIUX_PLAN.md section

### Prompt Structure:
Each prompt contains:
- **Task ID:** Unique identifier (e.g., UIUX-1.1)
- **Task Name:** Clear description of what needs to be done
- **Dependencies:** Prerequisites that must be completed first
- **Prompt Text:** The exact text to provide to the UI/UX Agent
- **Expected Deliverables:** What you should receive after task completion
- **Verification:** How to verify the task was completed correctly

### Important Notes:
- 🔴 **Critical Path Tasks:** Must be completed; cannot skip
- 🟡 **High Priority Tasks:** Important for full functionality
- 🟢 **Medium Priority Tasks:** Nice-to-have features
- ⚪ **Optional Tasks:** Can be skipped if time is limited

---

## Phase 1: Design System Setup

**Duration:** 3-4 days  
**Goal:** Establish the complete design system foundation including colors, typography, spacing, shadows, and animations.

---

### Task UIUX-1.1: Create CSS Variables for Color System 🔴

**Dependencies:** None (Starting task)

**Prompt:**
```
Create a comprehensive CSS variables file named `design-system.css` for the OpenMeet Recorder project. 

Define the following color systems using CSS custom properties:

1. **Brand Colors (Primary):** Create a 10-step scale from primary-50 (lightest) to primary-900 (darkest) using blue as the base color (#3b82f6 for primary-500).

2. **Secondary Colors:** Create a 10-step gray-blue scale for secondary elements.

3. **Semantic Colors:** Create color scales for:
   - Success (green) - 7 steps (50, 100, 500, 600, 700)
   - Warning (orange/yellow) - 7 steps
   - Error (red) - 7 steps  
   - Info (blue) - 7 steps

4. **Neutral/Grayscale:** Create an 11-step grayscale from white to black (gray-50 through gray-900).

5. **Special Colors:**
   - Recording state: #ef4444 (red)
   - Paused state: #f59e0b (orange)
   - Uploading state: #3b82f6 (blue)
   - Overlay colors with alpha transparency

Format as CSS variables (--color-primary-500, etc.) with clear comments explaining usage for each color group.

Reference: UIUX_PLAN.md Section 3.1
```

**Expected Deliverables:**
- [ ] `design-system.css` file created
- [ ] All color variables defined with proper naming convention
- [ ] Comments explaining usage for each color group
- [ ] Color values match the specifications in UIUX_PLAN.md

**Verification:**
```css
/* Verify these variables exist in design-system.css */
--color-primary-500: #3b82f6;
--color-success-500: #22c55e;
--color-error-500: #ef4444;
--color-recording: #ef4444;
```

---

### Task UIUX-1.2: Define Typography System Variables 🔴

**Dependencies:** UIUX-1.1

**Prompt:**
```
Extend the `design-system.css` file with a complete typography system.

Define the following using CSS custom properties:

1. **Font Families:**
   - Primary sans-serif stack: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Helvetica Neue', Arial, sans-serif
   - Monospace stack for code/timestamps: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', 'Courier New', monospace

2. **Font Sizes:** Create a scale from xs (12px) to 5xl (48px):
   - xs: 0.75rem (12px)
   - sm: 0.875rem (14px)
   - base: 1rem (16px)
   - lg: 1.125rem (18px)
   - xl: 1.25rem (20px)
   - 2xl: 1.5rem (24px)
   - 3xl: 1.875rem (30px)
   - 4xl: 2.25rem (36px)
   - 5xl: 3rem (48px)

3. **Line Heights:**
   - none: 1
   - tight: 1.25
   - snug: 1.375
   - normal: 1.5
   - relaxed: 1.625
   - loose: 2

4. **Font Weights:**
   - Define weights from 100 (thin) to 900 (black)
   - Include semantic names (normal: 400, medium: 500, semibold: 600, bold: 700)

Add usage comments for each typography element explaining when to use each size/weight combination.

Reference: UIUX_PLAN.md Section 3.2
```

**Expected Deliverables:**
- [ ] Typography variables added to `design-system.css`
- [ ] Font family stacks defined
- [ ] Complete font size scale (9 sizes)
- [ ] Line height scale defined
- [ ] Font weight scale defined
- [ ] Usage comments included

**Verification:**
```css
/* Verify these exist */
--font-family-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI'...;
--text-base: 1rem;
--leading-normal: 1.5;
--font-semibold: 600;
```

---

### Task UIUX-1.3: Create Spacing System Variables 🔴

**Dependencies:** UIUX-1.2

**Prompt:**
```
Extend the `design-system.css` file with a comprehensive spacing system based on a 4px base unit.

Define spacing variables from 0 to 64 (following the 4px grid):
- 0: 0
- px: 1px (for borders)
- 0.5: 0.125rem (2px)
- 1: 0.25rem (4px)
- 1.5: 0.375rem (6px)
- 2: 0.5rem (8px)
- 2.5: 0.625rem (10px)
- 3: 0.75rem (12px)
- 3.5: 0.875rem (14px)
- 4: 1rem (16px)
- 5: 1.25rem (20px)
- 6: 1.5rem (24px)
- 7: 1.75rem (28px)
- 8: 2rem (32px)
- 9: 2.25rem (36px)
- 10: 2.5rem (40px)
- 11: 2.75rem (44px)
- 12: 3rem (48px)
- 14: 3.5rem (56px)
- 16: 4rem (64px)
- 20: 5rem (80px)
- 24: 6rem (96px)
- 32: 8rem (128px)
- 40: 10rem (160px)
- 48: 12rem (192px)
- 56: 14rem (224px)
- 64: 16rem (256px)

Format as CSS variables: --spacing-0, --spacing-px, --spacing-0_5 (use underscore for decimals), --spacing-1, etc.

Include a usage guide table showing common use cases:
- Component internal padding
- Gap between elements
- Margin between sections
- Container widths

Reference: UIUX_PLAN.md Section 3.3
```

**Expected Deliverables:**
- [ ] All spacing variables defined in `design-system.css`
- [ ] Consistent naming convention used
- [ ] Usage guide included as CSS comments
- [ ] Values follow the 4px grid system

**Verification:**
```css
/* Verify key spacing values */
--spacing-4: 1rem;        /* 16px */
--spacing-6: 1.5rem;      /* 24px */
--spacing-8: 2rem;        /* 32px */
```

---

### Task UIUX-1.4: Define Shadow and Elevation System 🔴

**Dependencies:** UIUX-1.3

**Prompt:**
```
Extend the `design-system.css` file with a shadow system for depth perception.

Define the following shadow levels:
1. --shadow-xs: Subtle elevation for inputs (1px blur)
2. --shadow-sm: Small cards, dropdowns (3px blur)
3. --shadow-md: Default cards, button hover (6px blur)
4. --shadow-lg: Modals, popovers (15px blur)
5. --shadow-xl: Large modals, drawers (25px blur)
6. --shadow-2xl: Maximum elevation (50px blur)
7. --shadow-inner: Inset shadow for pressed states
8. --shadow-focus: Focus ring for keyboard navigation (blue glow)

Use rgba(0, 0, 0, 0.1) for shadows with varying offsets and blur radii.

For each shadow level, include:
- The complete box-shadow CSS value
- Usage comment explaining when to use it
- Examples of components that should use this shadow

Reference: UIUX_PLAN.md Section 3.4
```

**Expected Deliverables:**
- [ ] 8 shadow variables defined in `design-system.css`
- [ ] Shadow values include proper offset, blur, and spread
- [ ] Usage comments for each shadow level
- [ ] Focus ring shadow for accessibility

**Verification:**
```css
/* Verify shadow definitions */
--shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
--shadow-focus: 0 0 0 3px rgba(59, 130, 246, 0.5);
```

---

### Task UIUX-1.5: Create Border Radius System 🔴

**Dependencies:** UIUX-1.4

**Prompt:**
```
Extend the `design-system.css` file with a border radius system.

Define the following radius values:
- --radius-none: 0
- --radius-sm: 0.125rem (2px) - Subtle rounding
- --radius-base: 0.25rem (4px) - Default rounding
- --radius-md: 0.375rem (6px) - Medium rounding
- --radius-lg: 0.5rem (8px) - Large rounding
- --radius-xl: 0.75rem (12px) - Extra large
- --radius-2xl: 1rem (16px) - Very large
- --radius-3xl: 1.5rem (24px) - Huge
- --radius-full: 9999px - Full circle/pill

For each radius value, include usage comments with examples:
- Buttons (use md)
- Input fields (use md)
- Cards (use lg)
- Badges/Pills (use full)
- Modals (use lg)
- Avatars (use full)

Reference: UIUX_PLAN.md Section 3.5
```

**Expected Deliverables:**
- [ ] All border radius variables defined
- [ ] Usage comments for each radius level
- [ ] Examples of which components use which radius

**Verification:**
```css
/* Verify radius values */
--radius-md: 0.375rem;   /* 6px - buttons, inputs */
--radius-lg: 0.5rem;     /* 8px - cards, modals */
--radius-full: 9999px;   /* circles, pills */
```

---

### Task UIUX-1.6: Define Animation and Transition System 🔴

**Dependencies:** UIUX-1.5

**Prompt:**
```
Extend the `design-system.css` file with animation and transition specifications.

Create the following:

1. **Transition Durations:**
   - --duration-instant: 0ms
   - --duration-75: 75ms
   - --duration-100: 100ms
   - --duration-150: 150ms
   - --duration-200: 200ms (default)
   - --duration-300: 300ms
   - --duration-500: 500ms
   - --duration-700: 700ms
   - --duration-1000: 1000ms

2. **Easing Functions:**
   - --ease-linear: linear
   - --ease-in: cubic-bezier(0.4, 0, 1, 1)
   - --ease-out: cubic-bezier(0, 0, 0.2, 1)
   - --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1)
   - --ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55)

3. **CSS Keyframe Animations:**
   - @keyframes fadeIn (opacity 0 to 1)
   - @keyframes slideInUp (translateY + fade)
   - @keyframes slideInRight (translateX + fade)
   - @keyframes scaleUp (scale 0.95 to 1 + fade)
   - @keyframes pulse (opacity animation for recording indicator)
   - @keyframes spin (360deg rotation for loading)

4. **Common Transition Classes:**
   - .transition-standard (for colors)
   - .transition-transform (for scale/position)
   - .transition-opacity (for fades)
   - .transition-all (use sparingly)

Reference: UIUX_PLAN.md Section 3.6
```

**Expected Deliverables:**
- [ ] Duration variables defined
- [ ] Easing function variables defined
- [ ] 6 keyframe animations created
- [ ] 4 transition utility classes defined
- [ ] Usage comments for each animation

**Verification:**
```css
/* Verify animations exist */
@keyframes pulse { ... }
@keyframes spin { ... }
--duration-200: 200ms;
--ease-out: cubic-bezier(0, 0, 0.2, 1);
```

---

### Task UIUX-1.7: Define Z-Index Layering System 🔴

**Dependencies:** UIUX-1.6

**Prompt:**
```
Extend the `design-system.css` file with a z-index system to prevent stacking conflicts.

Define the following z-index layers in ascending order:
- --z-base: 0 (default layer)
- --z-dropdown: 1000 (dropdown menus)
- --z-sticky: 1020 (sticky headers)
- --z-fixed: 1030 (fixed elements like control bar)
- --z-modal-backdrop: 1040 (modal overlay backdrop)
- --z-modal: 1050 (modal content)
- --z-popover: 1060 (popovers and tooltips)
- --z-toast: 1070 (toast notifications)
- --z-tooltip: 1080 (tooltips - highest)

Include usage comments explaining what each layer is for and examples of components that should use each z-index value.

Create a usage table showing the hierarchy from lowest to highest.

Reference: UIUX_PLAN.md Section 3.9
```

**Expected Deliverables:**
- [ ] 9 z-index variables defined
- [ ] Proper hierarchy maintained (incremental values)
- [ ] Usage comments for each layer
- [ ] Examples of components for each z-index

**Verification:**
```css
/* Verify z-index values */
--z-base: 0;
--z-fixed: 1030;
--z-modal: 1050;
--z-tooltip: 1080;
```

---

### Task UIUX-1.8: Configure TailwindCSS with Design Tokens 🔴

**Dependencies:** UIUX-1.1 through UIUX-1.7

**Prompt:**
```
Create a complete `tailwind.config.js` file for the OpenMeet Recorder project.

Configure the following in the theme.extend section:

1. **Colors:** Map all CSS color variables to Tailwind color utilities:
   - primary: { 50, 100, 200, 300, 400, 500, 600, 700, 800, 900 }
   - secondary: { 50-900 }
   - success: { 50, 100, 500, 600, 700 }
   - warning: { 50, 100, 500, 600, 700 }
   - error: { 50, 100, 500, 600, 700 }
   - gray: { 50-900 }

2. **Font Families:**
   - sans: system font stack
   - mono: monospace stack

3. **Font Sizes:** Map all text sizes (xs through 5xl) with line heights as tuples

4. **Spacing:** Extend with custom spacing values if needed

5. **Box Shadow:** Map all shadow variables

6. **Border Radius:** Map all radius variables

7. **Z-Index:** Add custom z-index values

8. **Breakpoints:** Define responsive breakpoints:
   - sm: '640px'
   - md: '768px'
   - lg: '1024px'
   - xl: '1280px'
   - 2xl: '1536px'

Use the values from the design-system.css file. Format properly as a JavaScript module.

Reference: UIUX_PLAN.md Section 3.8
```

**Expected Deliverables:**
- [ ] Complete `tailwind.config.js` file created
- [ ] All color scales mapped
- [ ] Typography configuration complete
- [ ] Shadow and radius systems configured
- [ ] Responsive breakpoints defined
- [ ] File is valid JavaScript

**Verification:**
```javascript
// Verify structure exists
module.exports = {
  theme: {
    extend: {
      colors: { primary: {...}, ... },
      fontSize: { xs: [...], ... },
      ...
    }
  }
}
```

---

### Task UIUX-1.9: Initialize ShadcnUI Component Library 🔴

**Dependencies:** UIUX-1.8

**Prompt:**
```
Set up the ShadcnUI component library for the OpenMeet Recorder project.

Perform the following steps:

1. Create a `components.json` configuration file with these settings:
   - Style: "default"
   - Base color: "blue" (to match our primary-500: #3b82f6)
   - CSS variables: true
   - TypeScript: true
   - Tailwind CSS: true
   - Tailwind config: "tailwind.config.js"
   - Components directory: "@/components"
   - Utils directory: "@/lib/utils"
   - React Server Components: false
   - Aliases:
     - components: "@/components"
     - utils: "@/lib/utils"

2. List the initial components to install:
   - Button
   - Card
   - Dialog
   - Input
   - Select
   - Dropdown Menu
   - Tabs
   - Avatar
   - Badge

3. Provide the exact commands to run:
   ```bash
   npx shadcn-ui@latest init
   npx shadcn-ui@latest add button
   npx shadcn-ui@latest add card
   # ... etc
   ```

4. Document the component installation process

Reference: UIUX_PLAN.md Section 3.7 and Phase 1 Task 3
```

**Expected Deliverables:**
- [ ] `components.json` configuration file created
- [ ] List of commands to install base components
- [ ] Installation instructions documented
- [ ] Component directory structure defined

**Verification:**
```json
// Verify components.json exists with:
{
  "style": "default",
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "src/index.css",
    "baseColor": "blue",
    "cssVariables": true
  }
}
```

---

### Task UIUX-1.10: Create Base CSS and Global Styles 🔴

**Dependencies:** UIUX-1.9

**Prompt:**
```
Create a global styles file `globals.css` that imports and applies the design system.

Include the following:

1. **Imports:**
   - Import design-system.css
   - Import Tailwind directives (@tailwind base, components, utilities)

2. **CSS Reset/Normalize:**
   - Box-sizing: border-box for all elements
   - Remove default margins and padding where appropriate
   - Set default font smoothing

3. **Base Element Styles:**
   - Body: font-family, color, background-color, line-height
   - Headings (h1-h6): font-weight, line-height, color
   - Links: text-decoration, color, hover states
   - Buttons: reset button styles, cursor pointer
   - Inputs: border, padding, focus states

4. **Utility Classes:**
   - .sr-only (screen reader only)
   - .container (max-width, padding)
   - Focus-visible styles for accessibility

5. **Scrollbar Styling (optional):**
   - Custom scrollbar for webkit browsers

Format as a complete CSS file that can be imported into the application.

Reference: UIUX_PLAN.md Section 3
```

**Expected Deliverables:**
- [ ] `globals.css` file created
- [ ] Tailwind directives included
- [ ] Base element styles defined
- [ ] Utility classes created
- [ ] Accessibility styles included

**Verification:**
```css
/* Verify file includes */
@import './design-system.css';
@tailwind base;
@tailwind components;
@tailwind utilities;

body { font-family: var(--font-family-sans); }
.sr-only { position: absolute; width: 1px; ... }
```

---

## Phase 2: Extension UI Components

**Duration:** 4-5 days  
**Goal:** Design all Chrome extension UI components including popup, control bar, and options page.

---

### Task UIUX-2.1: Design Extension Popup Container Structure 🔴

**Dependencies:** Phase 1 Complete

**Prompt:**
```
Design the Chrome extension popup interface structure with HTML and CSS.

Create the following:

1. **Popup Container:**
   - Fixed width: 400px
   - Max-height: 600px, Min-height: 450px
   - Background: white
   - Border-radius: 8px (--radius-lg)
   - Box-shadow: --shadow-xl
   - Overflow: hidden

2. **Section Structure:**
   - Header section (padding: 20px, border-bottom)
   - Body section (padding: 20px, scrollable if needed)
   - Footer section (padding: 16px 20px, background: gray-50, border-top)

3. **HTML Structure:**
Provide complete HTML structure with proper semantic tags and BEM-style class naming.

4. **CSS Specifications:**
Use CSS variables from design-system.css and Tailwind classes where appropriate.

Include all states and responsive considerations.

Reference: UIUX_PLAN.md Section 4.1.1
```

**Expected Deliverables:**
- [ ] HTML structure for popup container
- [ ] CSS for popup layout (container, header, body, footer)
- [ ] Proper semantic HTML tags used
- [ ] Class naming follows conventions

**Verification:**
- Popup width is 400px
- Sections have proper padding
- Overflow is handled correctly

---

### Task UIUX-2.2: Design Extension Popup Header Component 🔴

**Dependencies:** UIUX-2.1

**Prompt:**
```
Design the popup header section with title, subtitle, and settings button.

Create:

1. **Header Layout:**
   - Flexbox with space-between alignment
   - Left side: Title and subtitle
   - Right side: Settings icon button

2. **Title:**
   - Text: "OpenMeet Recorder"
   - Font-size: --text-xl (20px)
   - Font-weight: --font-semibold (600)
   - Color: --color-gray-900

3. **Subtitle:**
   - Text: "Configure recording settings"
   - Font-size: --text-sm (14px)
   - Color: --color-gray-500
   - Margin-top: 4px

4. **Settings Button:**
   - Icon-only button (20px Settings icon)
   - Width/height: 36px
   - Hover state with background change
   - ARIA label: "Settings"

Provide complete HTML and CSS with all hover states and interactions specified.

Reference: UIUX_PLAN.md Section 4.1.2
```

**Expected Deliverables:**
- [ ] HTML for popup header
- [ ] CSS for header layout and elements
- [ ] Settings button with hover state
- [ ] ARIA labels included

**Verification:**
- Header has proper spacing
- Settings button is visible and styled
- Hover states work correctly

---

### Task UIUX-2.3: Design Source Selector Component 🔴

**Dependencies:** UIUX-2.2

**Prompt:**
```
Design the recording source selector with three options: Desktop, This Tab, and Canvas Only.

Create:

1. **Container:**
   - Label: "Choose Recording Source" (font-sm, font-medium, gray-700)
   - Grid layout: 3 columns with 12px gap

2. **Source Option Buttons (3):**
   Each button should have:
   - Icon (32px): Monitor, Square, Maximize2
   - Label below icon
   - Padding: 16px 12px
   - Border: 2px solid gray-200
   - Border-radius: --radius-lg (8px)
   - Background: white
   - Cursor: pointer

3. **States:**
   - Default: Gray border, white background
   - Hover: Blue border (primary-300), light blue background (primary-50), lift effect (translateY(-2px)), shadow-sm
   - Active/Selected: Strong blue border (primary-500), primary-50 background, icon and label change to primary-600/700
   - Disabled: Opacity 0.5, cursor not-allowed

4. **Icons:**
   - Use 32px icons from Lucide React
   - Color: gray-600 (default), primary-600 (active)

Provide complete HTML, CSS, and specify the exact animations for hover effects.

Reference: UIUX_PLAN.md Section 4.1.3
```

**Expected Deliverables:**
- [ ] HTML for source selector with 3 buttons
- [ ] CSS for grid layout
- [ ] Button styles with all states
- [ ] Hover animation specified (lift + shadow)
- [ ] Active state clearly differentiated

**Verification:**
- 3 buttons in a grid layout
- Hover effect includes lift and shadow
- Active state has blue border and background

---

### Task UIUX-2.4: Design Settings Checkboxes Component 🔴

**Dependencies:** UIUX-2.3

**Prompt:**
```
Design the settings section with checkboxes for Tab Sound, Microphone, and Annotation tools.

Create:

1. **Settings Section:**
   - Section title: "Recording Settings" (text-sm, font-semibold, gray-900)
   - Margin-bottom: 20px

2. **Checkbox Items (3):**
   Each item should have:
   - Flexbox layout (checkbox + label)
   - Padding: 12px 0
   - Cursor: pointer
   - User-select: none

3. **Checkbox Input:**
   - Size: 18x18px
   - Border: 2px solid gray-300
   - Border-radius: --radius-base (4px)
   - Margin-right: 12px
   - Accent-color: primary-600 (modern checkbox color)
   - Checked state: background primary-600, border primary-600
   - Focus state: blue ring shadow

4. **Checkbox Labels:**
   - "Tab Sound"
   - "Microphone"
   - "Control bar & Annotation tools"
   - Font-size: text-sm (14px)
   - Color: gray-700
   - Hover: color changes to gray-900

Provide complete HTML and CSS with custom checkbox styling.

Reference: UIUX_PLAN.md Section 4.1.4
```

**Expected Deliverables:**
- [ ] HTML for 3 checkbox items
- [ ] CSS for checkbox styling
- [ ] Custom checkbox appearance
- [ ] Focus states for accessibility
- [ ] Hover effects on labels

**Verification:**
- Checkboxes are 18x18px
- Blue accent color when checked
- Focus ring visible on keyboard focus

---

### Task UIUX-2.5: Design Info Note Component 🔴

**Dependencies:** UIUX-2.4

**Prompt:**
```
Design an informational note component to display microphone and echo settings information.

Create:

1. **Info Note Container:**
   - Flexbox layout with gap: 12px
   - Padding: 12px
   - Background: primary-50
   - Border: 1px solid primary-200
   - Border-left: 4px solid primary-500 (accent)
   - Border-radius: --radius-md (6px)
   - Margin-bottom: 16px

2. **Info Icon:**
   - Info icon (16px) from Lucide React
   - Color: primary-600
   - Flex-shrink: 0
   - Margin-top: 2px (align with text)

3. **Info Text:**
   - Font-size: 13px
   - Line-height: 1.5
   - Color: primary-900
   - Strong tags: font-weight 600, color primary-700

4. **Content:**
"The Mic is <strong>off</strong> & Tab Sound is <strong>on</strong> to prevent echoes during recording. Turn on the Mic if you need to record your voice."

5. **Variants (for future use):**
Document CSS classes for:
- .info-note-success (green colors)
- .info-note-warning (yellow colors)
- .info-note-error (red colors)

Provide complete HTML and CSS with all variants.

Reference: UIUX_PLAN.md Section 4.1.5
```

**Expected Deliverables:**
- [ ] HTML for info note component
- [ ] CSS for primary variant (blue)
- [ ] CSS for 3 additional variants (success, warning, error)
- [ ] Proper icon alignment

**Verification:**
- Background is light blue (primary-50)
- Left border accent is visible
- Icon is aligned with text

---

### Task UIUX-2.6: Design Action Buttons (Start/Close) 🔴

**Dependencies:** UIUX-2.5

**Prompt:**
```
Design the primary action buttons for the popup footer: Start Recording and Close buttons.

Create:

1. **Footer Button Container:**
   - Display: flex
   - Gap: 12px
   - Justify-content: stretch (buttons take equal width)

2. **Primary Button (Start Recording):**
   - Width: flex-grow (50%)
   - Height: 44px
   - Background: primary-600
   - Color: white
   - Font-size: text-base (16px)
   - Font-weight: semibold (600)
   - Border-radius: --radius-md (6px)
   - Border: none
   - Cursor: pointer
   - Box-shadow: --shadow-sm
   - Transition: all 200ms ease-out
   
   **Hover State:**
   - Background: primary-700
   - Box-shadow: --shadow-md
   - Transform: translateY(-1px)
   
   **Active/Pressed:**
   - Background: primary-800
   - Transform: translateY(0px)
   - Box-shadow: --shadow-xs
   
   **Disabled:**
   - Background: gray-300
   - Color: gray-500
   - Cursor: not-allowed
   - No hover effects

3. **Secondary Button (Close):**
   - Same dimensions as primary
   - Background: transparent
   - Color: gray-700
   - Border: 2px solid gray-300
   - Box-shadow: none
   
   **Hover State:**
   - Background: gray-50
   - Border: gray-400
   - Color: gray-900
   
   **Active/Pressed:**
   - Background: gray-100

4. **Focus State (Both):**
   - Outline: none
   - Box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5) (blue focus ring)

Provide complete HTML and CSS with all interactive states.

Reference: UIUX_PLAN.md Section 4.1.6
```

**Expected Deliverables:**
- [ ] HTML for button container with 2 buttons
- [ ] CSS for primary button (all states)
- [ ] CSS for secondary button (all states)
- [ ] Focus rings for accessibility
- [ ] Smooth transitions specified

**Verification:**
- Buttons are equal width
- Primary button is blue with white text
- Hover effects include lift on primary
- Focus ring is visible on keyboard navigation

---

### Task UIUX-2.7: Design Loading Spinner Component 🟡

**Dependencies:** UIUX-2.6

**Prompt:**
```
Design a loading spinner component to display during initialization and processing states.

Create:

1. **Spinner Container:**
   - Display: inline-block or flex
   - Width/height: 40px (default size)
   - Animation: spin 1s linear infinite (use @keyframes spin from design-system.css)

2. **Spinner SVG:**
   - Circle with dashed stroke
   - Stroke: 4px
   - Color: primary-600
   - Opacity: 0.25 for background circle
   - Opacity: 0.75 for foreground spinning arc

3. **Size Variants:**
   - .spinner-sm: 20px
   - .spinner-md: 40px (default)
   - .spinner-lg: 60px

4. **Overlay Spinner:**
   When loading overlays the entire popup:
   - Fixed position overlay
   - Background: rgba(255, 255, 255, 0.9)
   - Spinner centered absolutely
   - Z-index: --z-modal

Provide complete HTML, CSS, and SVG code for the spinner.

Reference: UIUX_PLAN.md Section 4.1 (implied loading states)
```

**Expected Deliverables:**
- [ ] HTML/SVG for spinner component
- [ ] CSS for spinner animation
- [ ] 3 size variants defined
- [ ] Overlay spinner variant
- [ ] Smooth rotation animation

**Verification:**
- Spinner rotates smoothly (1s per rotation)
- Centering works in overlay mode
- Size variants render correctly

---

### Task UIUX-2.8: Design Recording States UI 🔴

**Dependencies:** UIUX-2.7

**Prompt:**
```
Design the UI state changes when recording is active, paused, or stopped.

Create CSS and HTML for the following recording states:

1. **Recording State (Active):**
   - Primary button changes:
     - Background: error-600 (red)
     - Text: "Stop Recording"
     - Icon: Square (stop icon)
   - Add pulsing red indicator dot:
     - Size: 10px circle
     - Background: red
     - Animation: pulse 1.5s ease-in-out infinite
     - Position: absolute top-right of popup header

2. **Paused State:**
   - Primary button changes:
     - Background: warning-500 (orange)
     - Text: "Resume Recording"
     - Icon: Play (resume icon)
   - Pause indicator:
     - Orange dot (not pulsing)
     - Text overlay: "Recording Paused"

3. **Processing/Uploading State:**
   - Show progress bar:
     - Height: 4px
     - Width: 100%
     - Background: gray-200
     - Fill: primary-600 (animated progress)
     - Position: bottom of popup (absolute)
   - Show status text:
     - "Uploading... 47%" (dynamic)
     - Font-size: sm
     - Color: gray-600

4. **Error State:**
   - Show error message banner:
     - Background: error-50
     - Border-left: 4px solid error-500
     - Padding: 12px
     - Error icon + message
     - Close button

Provide complete CSS classes for each state and HTML examples showing the transformations.

Reference: UIUX_PLAN.md Section 4.1 (Recording States)
```

**Expected Deliverables:**
- [ ] CSS for all recording states
- [ ] HTML examples for each state
- [ ] Pulsing animation for recording indicator
- [ ] Progress bar component
- [ ] Error banner component

**Verification:**
- Recording state shows red stop button
- Pulsing dot animates smoothly
- Progress bar fills from left to right
- Error banner is clearly visible

---

### Task UIUX-2.9: Design Control Bar Overlay Component 🔴

**Dependencies:** UIUX-2.8

**Prompt:**
```
Design the floating control bar that appears during recording sessions.

Create:

1. **Control Bar Container:**
   - Position: fixed
   - Bottom: 24px
   - Left: 50% (transform: translateX(-50%) for centering)
   - Width: auto (min-width: 300px, max-width: 500px)
   - Height: 64px
   - Background: rgba(0, 0, 0, 0.9) (dark with transparency)
   - Backdrop-filter: blur(10px)
   - Border: 1px solid rgba(255, 255, 255, 0.1)
   - Border-radius: --radius-2xl (16px)
   - Box-shadow: --shadow-2xl
   - Z-index: --z-fixed (1030)
   - Display: flex
   - Align-items: center
   - Justify-content: space-between
   - Padding: 0 20px
   - Gap: 16px

2. **Recording Indicator Section (Left):**
   - Flex display with gap: 12px
   - Pulsing red dot: 12px circle
   - Timer display:
     - Font: monospace
     - Color: white
     - Font-size: text-lg (18px)
     - Format: "00:05:23"

3. **Control Buttons Section (Center):**
   - Flex display with gap: 8px
   - Button specs:
     - Width/height: 44px
     - Background: rgba(255, 255, 255, 0.1)
     - Border: 1px solid rgba(255, 255, 255, 0.2)
     - Border-radius: --radius-lg (8px)
     - Icon: 20px (white)
     - Hover: background rgba(255, 255, 255, 0.2)
   - Buttons:
     - Pause/Resume (Play/Pause icon)
     - Annotation (Pen tool icon)
     - Microphone (Mic icon with toggle visual)

4. **Stop Button (Right):**
   - Width/height: 44px
   - Background: error-600 (red)
   - Border-radius: --radius-lg
   - Icon: Square (stop icon, 20px, white)
   - Hover: background error-700, slight lift

5. **Drag Handle:**
   - Visual indicator that bar is draggable
   - 6 dots in 2 rows (grip icon)
   - Position: absolute left (before recording indicator)
   - Color: rgba(255, 255, 255, 0.3)

6. **Animations:**
   - Entrance: slideInUp + fadeIn (300ms)
   - Exit: slideOutDown + fadeOut (200ms)
   - Hover: slight lift (2px) + shadow increase

Provide complete HTML and CSS with all states and animations.

Reference: UIUX_PLAN.md Section 4.2
```

**Expected Deliverables:**
- [ ] HTML structure for control bar
- [ ] CSS for dark glassmorphism effect
- [ ] All button components with hover states
- [ ] Recording timer with monospace font
- [ ] Pulsing red indicator
- [ ] Entrance/exit animations
- [ ] Drag handle visual

**Verification:**
- Control bar is centered horizontally
- Dark background with blur effect
- All buttons visible and properly spaced
- Timer displays correctly
- Red dot pulses
- Hover effects work on all buttons

---

### Task UIUX-2.10: Design Annotation Toolbar Component 🟡

**Dependencies:** UIUX-2.9

**Prompt:**
```
Design the annotation toolbar that appears when the user clicks the annotation button in the control bar.

Create:

1. **Annotation Toolbar Container:**
   - Position: fixed
   - Right: 24px
   - Top: 50% (transform: translateY(-50%) for vertical centering)
   - Width: 60px
   - Background: white
   - Border-radius: --radius-xl (12px)
   - Box-shadow: --shadow-xl
   - Padding: 12px 8px
   - Z-index: --z-fixed (1030)
   - Display: flex
   - Flex-direction: column
   - Gap: 8px

2. **Tool Buttons:**
   Each button:
   - Width/height: 44px
   - Background: transparent
   - Border: 1px solid gray-200
   - Border-radius: --radius-md (6px)
   - Icon: 20px, gray-600
   - Hover: background primary-50, border primary-300, icon primary-600
   - Active/Selected: background primary-100, border primary-500, icon primary-700

3. **Tools List:**
   - Pen tool
   - Highlighter
   - Eraser
   - Text tool
   - Divider (1px line)
   - Color picker (current color preview)
   - Stroke width selector

4. **Color Picker Popover:**
   When color button clicked:
   - Popover: 200px wide
   - Background: white
   - Shadow: --shadow-lg
   - Padding: 12px
   - Color swatches: 6 colors in 2 rows
   - Each swatch: 32x32px circle
   - Selected: blue ring around swatch

5. **Stroke Width Selector:**
   - 3 options: Thin, Medium, Thick
   - Visual preview of stroke width
   - Similar popover style as color picker

Provide complete HTML and CSS with all interactive states.

Reference: UIUX_PLAN.md Section 4.2 (Annotation Tools)
```

**Expected Deliverables:**
- [ ] HTML for annotation toolbar
- [ ] CSS for vertical toolbar layout
- [ ] All tool buttons with states
- [ ] Color picker popover design
- [ ] Stroke width selector design
- [ ] Active tool indication

**Verification:**
- Toolbar is vertically centered on right side
- Tool buttons have proper hover effects
- Active tool is clearly highlighted
- Color picker shows 6 color options
- Stroke width preview is visible

---

### Task UIUX-2.11: Design Options Page Layout 🔴

**Dependencies:** UIUX-2.10

**Prompt:**
```
Design the Chrome extension options/settings page for advanced configuration.

Create:

1. **Page Container:**
   - Full viewport height (min-height: 100vh)
   - Max-width: 800px
   - Margin: 0 auto
   - Padding: 40px 24px
   - Background: gray-50

2. **Page Header:**
   - Title: "OpenMeet Recorder Settings"
   - Font-size: 3xl (30px)
   - Font-weight: bold (700)
   - Color: gray-900
   - Margin-bottom: 8px
   - Subtitle: "Configure default recording preferences"
   - Font-size: base (16px)
   - Color: gray-600
   - Margin-bottom: 32px

3. **Settings Card:**
   - Background: white
   - Border-radius: --radius-lg (8px)
   - Box-shadow: --shadow-md
   - Padding: 32px

4. **Setting Sections:**
   Each section should have:
   - Section title (text-lg, font-semibold, gray-900)
   - Section description (text-sm, gray-600)
   - Margin-bottom: 24px between sections
   - Divider line (1px, gray-200) between sections

5. **Settings Categories:**
   - Recording Quality
   - Default Source
   - Audio Settings
   - Storage & Upload
   - Keyboard Shortcuts
   - Privacy & Permissions

6. **Footer:**
   - Save Changes button (primary)
   - Reset to Defaults button (secondary)
   - Positioned at bottom of card
   - Fixed to bottom on scroll (sticky)

Provide complete HTML structure and CSS for the options page layout.

Reference: UIUX_PLAN.md Section 4.3.1
```

**Expected Deliverables:**
- [ ] HTML structure for options page
- [ ] CSS for page layout and container
- [ ] Settings card design
- [ ] Section structure with dividers
- [ ] Footer with action buttons

**Verification:**
- Page is centered with max-width 800px
- Settings card has proper padding and shadow
- Sections are clearly separated
- Footer buttons are accessible

---

### Task UIUX-2.12: Design Settings Input Components 🔴

**Dependencies:** UIUX-2.11

**Prompt:**
```
Design various input components for the options page settings.

Create the following input types:

1. **Select Dropdown:**
   - Height: 44px
   - Padding: 0 16px
   - Border: 2px solid gray-300
   - Border-radius: --radius-md (6px)
   - Font-size: base (16px)
   - Color: gray-900
   - Background: white
   - Dropdown arrow: custom SVG icon
   - Hover: border-color primary-400
   - Focus: border-color primary-500, shadow-focus ring

2. **Toggle Switch:**
   - Width: 48px, height: 24px
   - Background: gray-300 (off), primary-600 (on)
   - Border-radius: 9999px (pill)
   - Sliding circle: 20x20px
   - Transition: 200ms ease-out
   - Label beside toggle (left or right)

3. **Radio Button Group:**
   - Custom styled radio buttons
   - Size: 20x20px
   - Border: 2px solid gray-300
   - Border-radius: full (circle)
   - Selected: blue border + blue center dot (10px)
   - Label spacing: 12px from radio
   - Vertical or horizontal layout options

4. **Text Input:**
   - Height: 44px
   - Padding: 0 16px
   - Border: 2px solid gray-300
   - Border-radius: --radius-md (6px)
   - Font-size: base (16px)
   - Placeholder: gray-400
   - Focus: border primary-500, shadow-focus

5. **Keyboard Shortcut Input:**
   - Special input for recording keyboard shortcuts
   - Displays key combination (e.g., "Ctrl + Shift + R")
   - Click to record new shortcut
   - Background: gray-100 when listening
   - Monospace font for display

Provide complete HTML and CSS for each input type with all interactive states.

Reference: UIUX_PLAN.md Section 4.3.2
```

**Expected Deliverables:**
- [ ] HTML/CSS for select dropdown
- [ ] HTML/CSS for toggle switch
- [ ] HTML/CSS for radio button group
- [ ] HTML/CSS for text input
- [ ] HTML/CSS for keyboard shortcut input
- [ ] All focus states for accessibility

**Verification:**
- All inputs have consistent height (44px)
- Focus rings are visible
- Toggle switch animates smoothly
- Keyboard shortcut input displays keys correctly

---

### Task UIUX-2.13: Design Help Text and Validation Messages 🟡

**Dependencies:** UIUX-2.12

**Prompt:**
```
Design help text and validation message components for settings inputs.

Create:

1. **Help Text:**
   - Font-size: sm (14px)
   - Color: gray-600
   - Line-height: 1.5
   - Margin-top: 8px
   - Icon: Info (16px) optional, gray-500
   - Max-width: 600px

2. **Success Message:**
   - Color: success-700
   - Background: success-50
   - Border-left: 3px solid success-500
   - Padding: 12px 16px
   - Border-radius: --radius-md
   - Icon: CheckCircle (16px)
   - Font-size: sm
   - Margin-top: 8px

3. **Error Message:**
   - Color: error-700
   - Background: error-50
   - Border-left: 3px solid error-500
   - Padding: 12px 16px
   - Border-radius: --radius-md
   - Icon: AlertCircle (16px)
   - Font-size: sm
   - Margin-top: 8px
   - Shake animation on appearance

4. **Warning Message:**
   - Color: warning-700
   - Background: warning-50
   - Border-left: 3px solid warning-500
   - Similar structure to error
   - Icon: AlertTriangle (16px)

5. **Input Validation States:**
   - .input-error: Red border (error-500)
   - .input-success: Green border (success-500)
   - .input-warning: Orange border (warning-500)

6. **Toast Notification (for saved settings):**
   - Position: fixed, top-right
   - Width: 320px
   - Background: white
   - Shadow: --shadow-xl
   - Border-radius: --radius-lg
   - Padding: 16px
   - Icon + message + close button
   - Slide-in animation from right
   - Auto-dismiss after 3 seconds

Provide complete HTML and CSS for all message types.

Reference: UIUX_PLAN.md Section 4.3 (implied validation states)
```

**Expected Deliverables:**
- [ ] CSS for help text
- [ ] HTML/CSS for success message
- [ ] HTML/CSS for error message with shake animation
- [ ] HTML/CSS for warning message
- [ ] Input validation state classes
- [ ] Toast notification component

**Verification:**
- Messages have proper color coding
- Icons are aligned correctly
- Error messages shake on appearance
- Toast slides in from right and auto-dismisses

---

## Phase 3: Dashboard Core Components

**Duration:** 5-6 days  
**Goal:** Design the dashboard layout, navigation, and core UI components.

---

### Task UIUX-3.1: Design Dashboard Layout Structure 🔴

**Dependencies:** Phase 2 Complete

**Prompt:**
```
Design the overall dashboard layout with sidebar navigation and main content area.

Create:

1. **Layout Container:**
   - Display: flex
   - Height: 100vh (full viewport height)
   - Overflow: hidden

2. **Sidebar:**
   - Width: 280px (expanded), 80px (collapsed)
   - Height: 100vh
   - Background: white
   - Border-right: 1px solid gray-200
   - Position: fixed (on mobile) or static (on desktop)
   - Z-index: --z-sticky (1020)
   - Transition: width 300ms ease-in-out
   - Display: flex
   - Flex-direction: column

3. **Main Content Area:**
   - Flex: 1 (takes remaining space)
   - Margin-left: 280px (when sidebar expanded)
   - Background: gray-50
   - Overflow-y: auto
   - Display: flex
   - Flex-direction: column

4. **Responsive Behavior:**
   - Desktop (>1024px): Sidebar always visible, 280px wide
   - Tablet (768px-1024px): Sidebar collapsible, toggle button
   - Mobile (<768px): Sidebar as overlay drawer, closed by default

5. **Sidebar Sections:**
   - Logo area (top)
   - Navigation links (middle, flex-grow)
   - User profile (bottom)

Provide complete HTML structure and CSS for the dashboard layout with responsive behavior.

Reference: UIUX_PLAN.md Section 5.1
```

**Expected Deliverables:**
- [ ] HTML structure for dashboard layout
- [ ] CSS for flexbox layout
- [ ] Sidebar with fixed width
- [ ] Main content area with flex-grow
- [ ] Responsive breakpoints defined
- [ ] Sidebar collapse animation

**Verification:**
- Layout is full viewport height
- Sidebar is fixed on left
- Main content scrolls independently
- Responsive behavior works at different breakpoints

---

### Task UIUX-3.2: Design Sidebar Logo and Branding 🔴

**Dependencies:** UIUX-3.1

**Prompt:**
```
Design the logo and branding area at the top of the sidebar.

Create:

1. **Logo Container:**
   - Height: 72px
   - Padding: 20px 24px
   - Border-bottom: 1px solid gray-200
   - Display: flex
   - Align-items: center
   - Gap: 12px

2. **Logo Icon:**
   - Size: 32x32px
   - SVG or image
   - Color: primary-600 (if SVG)
   - Placeholder: Video camera icon from Lucide

3. **Logo Text:**
   - Font-size: xl (20px)
   - Font-weight: semibold (600)
   - Color: gray-900
   - Text: "OpenMeet"
   - Transition: opacity 200ms (fades out when sidebar collapses)

4. **Collapsed State:**
   - Logo text hidden (opacity 0, width 0)
   - Only icon visible, centered
   - Container padding adjusts: 20px 12px

5. **Hover Effect:**
   - Slight scale on icon: scale(1.05)
   - Cursor: pointer (if logo is clickable link to home)

Provide HTML and CSS for the logo area with collapsed state.

Reference: UIUX_PLAN.md Section 5.2.1
```

**Expected Deliverables:**
- [ ] HTML for logo container
- [ ] CSS for logo and text
- [ ] Collapsed state styling
- [ ] Smooth transition animation
- [ ] Hover effect on logo

**Verification:**
- Logo is visible and properly sized
- Text disappears smoothly when sidebar collapses
- Icon remains centered in collapsed state

---

### Task UIUX-3.3: Design Sidebar Navigation Menu 🔴

**Dependencies:** UIUX-3.2

**Prompt:**
```
Design the navigation menu in the sidebar with links to different dashboard sections.

Create:

1. **Navigation Container:**
   - Flex-grow: 1 (takes available space)
   - Padding: 16px 12px
   - Display: flex
   - Flex-direction: column
   - Gap: 4px

2. **Navigation Link Item:**
   Each link should have:
   - Height: 44px
   - Padding: 0 16px
   - Border-radius: --radius-lg (8px)
   - Display: flex
   - Align-items: center
   - Gap: 12px
   - Cursor: pointer
   - Transition: all 150ms ease-out
   
   **Default State:**
   - Background: transparent
   - Color: gray-700
   
   **Hover State:**
   - Background: gray-100
   - Color: gray-900
   
   **Active/Selected State:**
   - Background: primary-50
   - Color: primary-700
   - Border-left: 3px solid primary-600 (indicator)
   
   **Focus State:**
   - Outline: 2px solid primary-500

3. **Link Content:**
   - Icon: 20x20px (left)
   - Label: text-base (16px), font-medium (500)
   - Badge (optional, right): pill with count

4. **Navigation Items:**
   - Home (Home icon)
   - Recordings (Video icon) - with badge showing count
   - Starred (Star icon)
   - Shared (Share2 icon)
   - Trash (Trash icon)
   - Divider (1px line, margin: 8px 0)
   - Settings (Settings icon)

5. **Collapsed State:**
   - Labels hidden (display: none)
   - Icons centered
   - Tooltip appears on hover showing label

6. **Badge Component:**
   - Size: 20x20px (min)
   - Background: primary-600
   - Color: white
   - Border-radius: full
   - Font-size: xs (12px)
   - Font-weight: semibold
   - Padding: 2px 6px

Provide complete HTML and CSS for navigation menu with all states.

Reference: UIUX_PLAN.md Section 5.2.2
```

**Expected Deliverables:**
- [ ] HTML for navigation menu with 6+ links
- [ ] CSS for link items (all states)
- [ ] Active state visual indicator
- [ ] Badge component for counts
- [ ] Collapsed state with tooltips
- [ ] Icon and label alignment

**Verification:**
- Navigation items have proper spacing
- Active state shows blue background + left border
- Hover effects work smoothly
- Badges display counts correctly
- Icons are properly aligned

---

### Task UIUX-3.4: Design Sidebar User Profile Section 🔴

**Dependencies:** UIUX-3.3

**Prompt:**
```
Design the user profile section at the bottom of the sidebar.

Create:

1. **Profile Container:**
   - Height: auto
   - Padding: 16px
   - Border-top: 1px solid gray-200
   - Display: flex
   - Align-items: center
   - Gap: 12px
   - Cursor: pointer

2. **User Avatar:**
   - Size: 40x40px
   - Border-radius: full (circle)
   - Background: primary-100 (if no image)
   - Border: 2px solid primary-300
   - Display user initials if no image:
     - Text: user initials (2 letters)
     - Font-size: base (16px)
     - Font-weight: semibold
     - Color: primary-700
     - Text-align: center

3. **User Info (visible when expanded):**
   - Display: flex
   - Flex-direction: column
   - Gap: 2px
   - Flex: 1
   - Overflow: hidden
   
   **User Name:**
   - Font-size: sm (14px)
   - Font-weight: medium (500)
   - Color: gray-900
   - Text-overflow: ellipsis
   - White-space: nowrap
   
   **User Email:**
   - Font-size: xs (12px)
   - Color: gray-500
   - Text-overflow: ellipsis
   - White-space: nowrap

4. **Dropdown Icon:**
   - ChevronUp icon (16px)
   - Color: gray-400
   - Transition: transform 200ms
   - Rotate 180deg when menu open

5. **Hover State:**
   - Background: gray-50
   - Border-radius: --radius-lg

6. **Collapsed State:**
   - Only avatar visible, centered
   - User info hidden

7. **Profile Dropdown Menu:**
   When clicked, show dropdown:
   - Position: absolute, bottom: 80px, left: 12px
   - Width: 256px (same as sidebar)
   - Background: white
   - Border-radius: --radius-lg
   - Shadow: --shadow-xl
   - Padding: 8px
   - Z-index: --z-dropdown
   
   Menu items:
   - My Account
   - Settings
   - Help & Support
   - Divider
   - Sign Out (text-error-600)

Provide complete HTML and CSS for user profile section with dropdown.

Reference: UIUX_PLAN.md Section 5.2.3
```

**Expected Deliverables:**
- [ ] HTML for profile section
- [ ] CSS for avatar, name, and email
- [ ] Collapsed state styling
- [ ] Dropdown menu component
- [ ] Hover and active states
- [ ] Avatar with initials fallback

**Verification:**
- Avatar is circular and properly sized
- User info is truncated with ellipsis if too long
- Dropdown appears above profile section
- Hover effect works on profile container

---

### Task UIUX-3.5: Design Dashboard Header Bar 🔴

**Dependencies:** UIUX-3.4

**Prompt:**
```
Design the header bar at the top of the main content area.

Create:

1. **Header Container:**
   - Height: 72px
   - Width: 100%
   - Background: white
   - Border-bottom: 1px solid gray-200
   - Position: sticky
   - Top: 0
   - Z-index: --z-sticky (1020)
   - Display: flex
   - Align-items: center
   - Justify-content: space-between
   - Padding: 0 32px
   - Gap: 24px

2. **Left Section:**
   - Display: flex
   - Align-items: center
   - Gap: 16px
   
   **Sidebar Toggle Button (mobile):**
   - Size: 40x40px
   - Background: transparent
   - Border: 1px solid gray-300
   - Border-radius: --radius-md
   - Icon: Menu icon (20px)
   - Display: none on desktop, flex on mobile
   - Hover: background gray-100
   
   **Page Title:**
   - Font-size: 2xl (24px)
   - Font-weight: semibold (600)
   - Color: gray-900

3. **Center Section:**
   - Flex: 1
   - Max-width: 500px
   
   **Search Bar:**
   - Width: 100%
   - Height: 44px
   - Background: gray-100
   - Border: 2px solid transparent
   - Border-radius: --radius-lg
   - Padding: 0 16px 0 44px (space for icon)
   - Position: relative
   
   **Search Icon:**
   - Position: absolute
   - Left: 16px
   - Top: 50%, transform: translateY(-50%)
   - Size: 20px
   - Color: gray-400
   
   **Search Input:**
   - Background: transparent
   - Border: none
   - Outline: none
   - Font-size: base (16px)
   - Color: gray-900
   - Placeholder: "Search recordings..."
   
   **Focus State:**
   - Border-color: primary-500
   - Background: white
   - Shadow: --shadow-sm

4. **Right Section:**
   - Display: flex
   - Align-items: center
   - Gap: 12px
   
   **Icon Buttons (3):**
   Each button:
   - Size: 40x40px
   - Background: transparent
   - Border: 1px solid gray-200
   - Border-radius: --radius-md
   - Icon: 20px, gray-600
   - Hover: background gray-100, border gray-300
   - Position: relative (for notification badge)
   
   Buttons:
   - Notifications (Bell icon) - with red dot badge if unread
   - Help (HelpCircle icon)
   - Profile (User icon with dropdown)

5. **Notification Badge:**
   - Size: 8x8px
   - Background: error-600 (red)
   - Border: 2px solid white
   - Border-radius: full
   - Position: absolute, top: 6px, right: 6px

Provide complete HTML and CSS for the header bar with all components.

Reference: UIUX_PLAN.md Section 5.3
```

**Expected Deliverables:**
- [ ] HTML for header container with 3 sections
- [ ] CSS for sticky header
- [ ] Sidebar toggle button (mobile only)
- [ ] Search bar with icon
- [ ] Icon buttons with hover states
- [ ] Notification badge indicator

**Verification:**
- Header sticks to top when scrolling
- Search bar is centered and responsive
- Icon buttons have proper hover effects
- Notification badge is visible on bell icon

---

### Task UIUX-3.6: Design Recording Card Component 🔴

**Dependencies:** UIUX-3.5

**Prompt:**
```
Design the recording card component that displays each recording in a grid or list view.

Create:

1. **Card Container:**
   - Width: 100% (in grid: auto-fit with min 320px)
   - Background: white
   - Border: 1px solid gray-200
   - Border-radius: --radius-lg (8px)
   - Box-shadow: --shadow-sm
   - Overflow: hidden
   - Cursor: pointer
   - Transition: all 200ms ease-out
   
   **Hover State:**
   - Border-color: primary-300
   - Box-shadow: --shadow-md
   - Transform: translateY(-2px)

2. **Thumbnail Section:**
   - Aspect-ratio: 16/9
   - Background: gray-900 (video background)
   - Position: relative
   - Overflow: hidden
   
   **Thumbnail Image:**
   - Width: 100%
   - Height: 100%
   - Object-fit: cover
   - Transition: transform 300ms
   - Hover: scale(1.05)
   
   **Duration Badge:**
   - Position: absolute
   - Bottom: 12px, right: 12px
   - Background: rgba(0, 0, 0, 0.8)
   - Color: white
   - Font-size: xs (12px)
   - Font-family: monospace
   - Padding: 4px 8px
   - Border-radius: --radius-base (4px)
   - Backdrop-filter: blur(4px)

3. **Content Section:**
   - Padding: 16px
   - Display: flex
   - Flex-direction: column
   - Gap: 8px
   
   **Title:**
   - Font-size: base (16px)
   - Font-weight: semibold (600)
   - Color: gray-900
   - Line-clamp: 2 (max 2 lines)
   - Text-overflow: ellipsis
   
   **Date:**
   - Font-size: sm (14px)
   - Color: gray-500
   - Display: flex
   - Align-items: center
   - Gap: 6px
   - Icon: Calendar (14px)

4. **Footer Section:**
   - Padding: 12px 16px
   - Border-top: 1px solid gray-100
   - Display: flex
   - Align-items: center
   - Justify-content: space-between
   
   **Left: File Size:**
   - Font-size: sm (14px)
   - Color: gray-600
   
   **Right: Action Buttons:**
   - Display: flex
   - Gap: 8px
   - Icon buttons (24x24px):
     - Star/Favorite (Star icon)
     - Share (Share2 icon)
     - More options (MoreVertical icon)
   - Opacity: 0 normally, 1 on card hover
   - Transition: opacity 200ms

5. **Status Indicator:**
   - Position: absolute
   - Top: 12px, left: 12px
   - Padding: 4px 8px
   - Font-size: xs (12px)
   - Font-weight: medium
   - Border-radius: --radius-base
   - Backdrop-filter: blur(4px)
   
   **Processing:**
   - Background: rgba(59, 130, 246, 0.9)
   - Color: white
   - Icon: rotating spinner
   
   **Completed:**
   - Background: rgba(34, 197, 94, 0.9)
   - Color: white
   - Icon: CheckCircle

6. **Starred Indicator:**
   - Position: absolute
   - Top: 12px, right: 12px
   - Star icon (20px, filled)
   - Color: warning-500 (gold)
   - Filter: drop-shadow

Provide complete HTML and CSS for the recording card with all states and variations.

Reference: UIUX_PLAN.md Section 5.4.1
```

**Expected Deliverables:**
- [ ] HTML structure for recording card
- [ ] CSS for card layout and sections
- [ ] Thumbnail with 16:9 aspect ratio
- [ ] Duration badge overlay
- [ ] Action buttons that appear on hover
- [ ] Status indicators (processing, completed)
- [ ] Hover effects on entire card

**Verification:**
- Card has proper hover lift effect
- Thumbnail scales on hover
- Action buttons fade in on hover
- Duration badge is visible on thumbnail
- Title truncates after 2 lines

---

### Task UIUX-3.7: Design Grid and List View Layouts 🔴

**Dependencies:** UIUX-3.6

**Prompt:**
```
Design the grid and list view layouts for displaying multiple recording cards.

Create:

1. **View Toggle Controls:**
   - Container at top of content area
   - Display: flex
   - Justify-content: space-between
   - Align-items: center
   - Padding: 24px 32px
   - Background: transparent
   
   **Left Side:**
   - Section title: "All Recordings"
   - Results count: "(24 recordings)"
   
   **Right Side:**
   - View toggle buttons group:
     - Grid view button (Grid3x3 icon)
     - List view button (List icon)
   - Button group:
     - Display: flex
     - Border: 1px solid gray-300
     - Border-radius: --radius-md
     - Overflow: hidden
   - Each button:
     - Size: 36x36px
     - Background: white (inactive), primary-50 (active)
     - Icon: 18px, gray-600 (inactive), primary-600 (active)
     - Border-right: 1px solid gray-300 (except last)
     - Hover: background gray-100 (if inactive)

2. **Grid View Layout:**
   - Display: grid
   - Grid-template-columns: repeat(auto-fill, minmax(320px, 1fr))
   - Gap: 24px
   - Padding: 0 32px 32px
   
   **Responsive:**
   - Desktop (>1280px): 3-4 columns
   - Tablet (768px-1280px): 2-3 columns
   - Mobile (<768px): 1 column

3. **List View Layout:**
   - Display: flex
   - Flex-direction: column
   - Gap: 16px
   - Padding: 0 32px 32px
   
   **List Card Modifications:**
   - Display: flex (horizontal)
   - Flex-direction: row
   - Align-items: center
   
   **Thumbnail:**
   - Width: 200px
   - Flex-shrink: 0
   - Aspect-ratio: 16/9
   
   **Content:**
   - Flex: 1
   - Padding: 16px 20px
   - Display: flex
   - Flex-direction: row
   - Align-items: center
   - Justify-content: space-between
   
   **Info Section:**
   - Display: flex
   - Flex-direction: column
   - Gap: 6px
   
   **Actions Section:**
   - Display: flex
   - Gap: 12px
   - Always visible (not hidden like grid)

4. **Empty State:**
   When no recordings:
   - Display: flex
   - Flex-direction: column
   - Align-items: center
   - Justify-content: center
   - Min-height: 400px
   - Padding: 40px
   
   **Empty Icon:**
   - Size: 80px
   - Color: gray-300
   - Icon: Video or FolderOpen
   
   **Empty Title:**
   - Font-size: xl (20px)
   - Font-weight: semibold
   - Color: gray-900
   - Margin-top: 16px
   
   **Empty Description:**
   - Font-size: base (16px)
   - Color: gray-600
   - Max-width: 400px
   - Text-align: center
   - Line-height: 1.6

5. **Loading State:**
   - Show 6-8 skeleton cards
   - Animated shimmer effect
   - Gray placeholder shapes

Provide complete HTML and CSS for both view layouts with all states.

Reference: UIUX_PLAN.md Section 5.4
```

**Expected Deliverables:**
- [ ] HTML for view toggle controls
- [ ] CSS for grid layout (responsive)
- [ ] CSS for list layout with horizontal cards
- [ ] Empty state design
- [ ] Loading state with skeletons
- [ ] Active view indicator

**Verification:**
- Grid shows 3-4 columns on desktop
- List view shows horizontal cards
- View toggle buttons highlight active view
- Empty state is centered and informative
- Loading skeletons animate

---

### Task UIUX-3.8: Design Filter and Sort Controls 🟡

**Dependencies:** UIUX-3.7

**Prompt:**
```
Design filter and sort controls for the recordings view.

Create:

1. **Filter Bar Container:**
   - Position: below view toggle, above recordings grid
   - Display: flex
   - Align-items: center
   - Gap: 16px
   - Padding: 16px 32px
   - Background: white
   - Border-top: 1px solid gray-200
   - Border-bottom: 1px solid gray-200

2. **Filter Button:**
   - Height: 40px
   - Padding: 0 16px
   - Background: white
   - Border: 1px solid gray-300
   - Border-radius: --radius-md
   - Display: flex
   - Align-items: center
   - Gap: 8px
   - Icon: Filter (18px)
   - Text: "Filters"
   - Font-size: sm (14px)
   - Hover: background gray-50, border gray-400
   
   **Active State (filters applied):**
   - Background: primary-50
   - Border: primary-500
   - Color: primary-700
   - Badge showing filter count

3. **Sort Dropdown:**
   - Same dimensions as filter button
   - Icon: ArrowUpDown (18px)
   - Text: "Sort: [option]"
   - Chevron down icon (14px) on right
   
   **Dropdown Menu:**
   - Position: absolute
   - Top: 48px
   - Width: 200px
   - Background: white
   - Border-radius: --radius-lg
   - Shadow: --shadow-lg
   - Padding: 8px
   - Z-index: --z-dropdown
   
   **Menu Items:**
   - Date (newest first)
   - Date (oldest first)
   - Name (A-Z)
   - Name (Z-A)
   - Duration (shortest)
   - Duration (longest)
   - File size
   
   Each item:
   - Padding: 8px 12px
   - Border-radius: --radius-md
   - Hover: background gray-100
   - Active: background primary-50, color primary-700, checkmark icon

4. **Date Range Filter:**
   - Button: "Date: All time"
   - Dropdown with preset ranges:
     - Today
     - Last 7 days
     - Last 30 days
     - Last 3 months
     - Custom range
   - If custom: show date picker modal

5. **Active Filter Tags:**
   - Display below filter bar
   - Padding: 12px 32px
   - Display: flex
   - Gap: 8px
   - Flex-wrap: wrap
   
   **Tag Chip:**
   - Height: 32px
   - Padding: 0 12px
   - Background: primary-100
   - Color: primary-700
   - Border-radius: --radius-full (pill)
   - Font-size: sm (14px)
   - Display: flex
   - Align-items: center
   - Gap: 8px
   
   **Close Button:**
   - Size: 16px
   - Icon: X
   - Cursor: pointer
   - Hover: color primary-900
   
   **Clear All Button:**
   - Text button: "Clear all"
   - Color: error-600
   - Font-size: sm
   - Hover: underline

6. **Filter Panel (sidebar):**
   Opens when filter button clicked:
   - Position: fixed
   - Right: 0
   - Top: 0
   - Width: 320px
   - Height: 100vh
   - Background: white
   - Shadow: --shadow-2xl
   - Z-index: --z-modal-backdrop
   - Padding: 24px
   - Overflow-y: auto
   - Slide-in animation from right
   
   **Panel Header:**
   - Title: "Filters"
   - Close button (X)
   - Margin-bottom: 24px
   
   **Filter Sections:**
   - Date Range
   - Duration
   - File Size
   - Status (Processing, Completed)
   - Starred Only (toggle)
   
   **Apply/Reset Buttons:**
   - At bottom of panel
   - Sticky to bottom

Provide complete HTML and CSS for all filter and sort components.

Reference: UIUX_PLAN.md Section 5.4 (implied filtering functionality)
```

**Expected Deliverables:**
- [ ] HTML for filter bar
- [ ] CSS for filter and sort buttons
- [ ] Sort dropdown menu
- [ ] Active filter tags
- [ ] Filter panel sidebar
- [ ] All interactive states

**Verification:**
- Filter button shows active state when filters applied
- Sort dropdown opens and closes correctly
- Filter tags display with close buttons
- Filter panel slides in from right
- Clear all removes all filter tags

---

### Task UIUX-3.9: Design Skeleton Loading Components 🟡

**Dependencies:** UIUX-3.8

**Prompt:**
```
Design skeleton loading states for all major components.

Create:

1. **Base Skeleton Classes:**
   
   **.skeleton:**
   - Background: linear-gradient shimmer effect
   - Animation: shimmer 2s infinite
   - Border-radius: --radius-base (4px)
   
   **@keyframes shimmer:**
   - Background-position animation
   - Gradient: gray-200 → gray-100 → gray-200
   - Direction: left to right

2. **Skeleton Card (for recordings):**
   - Same dimensions as actual card
   - Thumbnail: rectangular skeleton (16:9 aspect)
   - Title: 2 lines of skeleton text
     - Width: 100% first line
     - Width: 70% second line
     - Height: 16px each
   - Date line: width 40%, height 14px
   - Footer: skeleton elements for file size and actions

3. **Skeleton Text:**
   - .skeleton-text-lg: height 20px, width varies
   - .skeleton-text-md: height 16px
   - .skeleton-text-sm: height 14px
   - .skeleton-text-xs: height 12px
   - Border-radius: 4px

4. **Skeleton Avatar:**
   - .skeleton-avatar: circle, 40x40px
   - .skeleton-avatar-sm: 32x32px
   - .skeleton-avatar-lg: 64x64px

5. **Skeleton Button:**
   - Same dimensions as actual buttons
   - Border-radius preserved

6. **Grid Skeleton Layout:**
   - Show 6-8 skeleton cards in grid layout
   - Stagger animation delays:
     - Card 1: 0ms
     - Card 2: 100ms
     - Card 3: 200ms
     - etc.

7. **List Skeleton Layout:**
   - Horizontal skeleton cards
   - Same staggered animation

8. **Header Skeleton:**
   - Skeleton search bar
   - Skeleton icon buttons
   - Skeleton logo/title

Provide complete HTML and CSS for all skeleton components with shimmer animation.

Reference: UIUX_PLAN.md Section 5 (implied loading states)
```

**Expected Deliverables:**
- [ ] CSS for base skeleton classes
- [ ] Shimmer animation keyframes
- [ ] HTML/CSS for skeleton card
- [ ] Skeleton text variants
- [ ] Skeleton avatar variants
- [ ] Staggered animation delays

**Verification:**
- Shimmer animates smoothly left to right
- Skeleton cards match actual card dimensions
- Animation delays create wave effect
- All skeleton variants render correctly

---

### Task UIUX-3.10: Design Common Button Components 🔴

**Dependencies:** UIUX-3.9

**Prompt:**
```
Design a comprehensive button component system with all variants and sizes.

Create:

1. **Base Button Styles:**
   - Font-family: inherit
   - Font-weight: medium (500)
   - Border-radius: --radius-md (6px)
   - Cursor: pointer
   - Transition: all 200ms ease-out
   - User-select: none
   - Display: inline-flex
   - Align-items: center
   - Justify-content: center
   - Gap: 8px
   - White-space: nowrap

2. **Size Variants:**
   
   **.btn-sm:**
   - Height: 32px
   - Padding: 0 12px
   - Font-size: sm (14px)
   - Icon: 16px
   
   **.btn-md (default):**
   - Height: 40px
   - Padding: 0 16px
   - Font-size: base (16px)
   - Icon: 18px
   
   **.btn-lg:**
   - Height: 48px
   - Padding: 0 24px
   - Font-size: lg (18px)
   - Icon: 20px

3. **Style Variants:**
   
   **.btn-primary:**
   - Background: primary-600
   - Color: white
   - Border: none
   - Shadow: --shadow-sm
   - Hover: background primary-700, shadow-md, translateY(-1px)
   - Active: background primary-800, shadow-xs
   - Focus: shadow-focus ring
   
   **.btn-secondary:**
   - Background: transparent
   - Color: gray-700
   - Border: 2px solid gray-300
   - Hover: background gray-50, border gray-400
   - Active: background gray-100
   
   **.btn-success:**
   - Background: success-600
   - Color: white
   - Similar hover/active as primary
   
   **.btn-error:**
   - Background: error-600
   - Color: white
   - Similar hover/active as primary
   
   **.btn-ghost:**
   - Background: transparent
   - Color: gray-700
   - Border: none
   - Hover: background gray-100
   - Active: background gray-200
   
   **.btn-link:**
   - Background: transparent
   - Color: primary-600
   - Border: none
   - Padding: 0
   - Hover: color primary-700, underline

4. **State Modifiers:**
   
   **.btn-disabled:**
   - Opacity: 0.5
   - Cursor: not-allowed
   - Pointer-events: none
   
   **.btn-loading:**
   - Position: relative
   - Cursor: wait
   - Text color: transparent
   - Spinner: absolute centered overlay

5. **Icon-Only Buttons:**
   
   **.btn-icon:**
   - Square dimensions (32x32, 40x40, 48x48)
   - Padding: 0
   - Only icon, no text

6. **Button Group:**
   
   **.btn-group:**
   - Display: flex
   - Gap: 0
   - First button: border-radius left only
   - Last button: border-radius right only
   - Middle buttons: no border-radius
   - Adjacent borders: collapse

Provide complete CSS for all button variants with examples in HTML.

Reference: UIUX_PLAN.md Section 5 (buttons used throughout)
```

**Expected Deliverables:**
- [ ] CSS for base button styles
- [ ] 3 size variants (sm, md, lg)
- [ ] 6 style variants (primary, secondary, success, error, ghost, link)
- [ ] Loading and disabled states
- [ ] Icon-only button variant
- [ ] Button group styling
- [ ] HTML examples for each variant

**Verification:**
- All size variants have correct heights
- Primary button has hover lift effect
- Disabled buttons cannot be clicked
- Loading state shows spinner
- Button groups have proper border-radius

---

## Phase 4: Dashboard Detail View

**Duration:** 3-4 days  
**Goal:** Design the recording detail page with video player, transcript, and summary views.

---

### Task UIUX-4.1: Design Detail Page Layout 🔴

**Dependencies:** Phase 3 Complete

**Prompt:**
```
Design the recording detail page layout with video player and sidebar.

Create:

1. **Page Container:**
   - Display: flex
   - Min-height: 100vh
   - Background: gray-50

2. **Main Content Area:**
   - Flex: 1
   - Padding: 32px
   - Max-width: 1400px
   - Margin: 0 auto

3. **Top Section (Breadcrumb & Actions):**
   - Display: flex
   - Justify-content: space-between
   - Align-items: center
   - Margin-bottom: 24px
   
   **Breadcrumb:**
   - Display: flex
   - Gap: 8px
   - Font-size: sm (14px)
   - Color: gray-600
   - Items separated by chevron icons
   - Example: "Recordings / Meeting Recording"
   - Links hover: color primary-600
   
   **Action Buttons:**
   - Display: flex
   - Gap: 12px
   - Buttons: Download, Share, Delete, More

4. **Video Player Section:**
   - Width: 100%
   - Aspect-ratio: 16/9
   - Background: black
   - Border-radius: --radius-lg (8px)
   - Overflow: hidden
   - Box-shadow: --shadow-xl
   - Margin-bottom: 32px

5. **Recording Info Header:**
   - Display: flex
   - Justify-content: space-between
   - Align-items: flex-start
   - Padding: 24px
   - Background: white
   - Border-radius: --radius-lg
   - Box-shadow: --shadow-sm
   - Margin-bottom: 24px
   
   **Left Side:**
   - Recording title (editable)
   - Recording date and duration
   - File size and format
   
   **Right Side:**
   - Star/Favorite button
   - Status badge

6. **Tabs Section:**
   - Background: white
   - Border-radius: --radius-lg
   - Box-shadow: --shadow-sm
   - Overflow: hidden
   
   **Tab List:**
   - Display: flex
   - Border-bottom: 2px solid gray-200
   - Padding: 0 24px
   
   **Tabs:**
   - Transcript
   - AI Summary
   - Details
   - Comments

7. **Tab Content Area:**
   - Padding: 24px
   - Min-height: 400px

Provide complete HTML structure and CSS for the detail page layout.

Reference: UIUX_PLAN.md Section 5.5.1
```

**Expected Deliverables:**
- [ ] HTML structure for detail page
- [ ] CSS for main content layout
- [ ] Breadcrumb navigation
- [ ] Action buttons row
- [ ] Video player container (16:9)
- [ ] Recording info header
- [ ] Tabs navigation structure

**Verification:**
- Video player maintains 16:9 aspect ratio
- Breadcrumb shows proper navigation hierarchy
- Action buttons are visible and accessible
- Tabs are horizontally aligned

---

### Task UIUX-4.2: Design Video Player Controls 🔴

**Dependencies:** UIUX-4.1

**Prompt:**
```
Design custom video player controls overlay.

Create:

1. **Controls Container:**
   - Position: absolute
   - Bottom: 0, left: 0, right: 0
   - Height: auto
   - Background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, transparent 100%)
   - Padding: 24px 20px
   - Transform: translateY(100%)
   - Transition: transform 300ms ease-out
   - Visible on hover: transform translateY(0)

2. **Progress Bar Section:**
   - Width: 100%
   - Margin-bottom: 16px
   
   **Progress Track:**
   - Height: 6px
   - Background: rgba(255, 255, 255, 0.3)
   - Border-radius: 3px
   - Position: relative
   - Cursor: pointer
   
   **Progress Fill:**
   - Height: 100%
   - Background: primary-500
   - Border-radius: 3px
   - Position: absolute
   - Left: 0
   - Transition: width 100ms linear
   
   **Progress Thumb:**
   - Size: 14x14px
   - Background: white
   - Border-radius: full
   - Position: absolute
   - Right: -7px (centered on end of fill)
   - Top: 50%, transform: translateY(-50%)
   - Box-shadow: 0 2px 4px rgba(0,0,0,0.3)
   - Opacity: 0 (default), 1 (on hover)
   - Scale: 0.8 (default), 1 (on hover)
   
   **Buffered Progress:**
   - Height: 100%
   - Background: rgba(255, 255, 255, 0.5)
   - Position: absolute
   - Left: 0
   - Z-index: below fill

3. **Control Buttons Row:**
   - Display: flex
   - Justify-content: space-between
   - Align-items: center
   
   **Left Controls:**
   - Display: flex
   - Gap: 12px
   - Align-items: center
   
   **Play/Pause Button:**
   - Size: 40x40px
   - Background: transparent
   - Border: none
   - Icon: 24px, white
   - Hover: scale(1.1)
   
   **Volume Control:**
   - Display: flex
   - Gap: 8px
   - Align-items: center
   - Icon button: 32x32px (Volume2/VolumeX)
   - Slider: width 80px, height 4px
     - Track: rgba(255, 255, 255, 0.3)
     - Fill: white
     - Thumb: 12x12px white circle
   
   **Time Display:**
   - Font-family: monospace
   - Color: white
   - Font-size: sm (14px)
   - Format: "2:34 / 15:42"
   
   **Middle Controls:**
   - Display: flex
   - Gap: 8px
   
   **Skip Buttons:**
   - Rewind 10s (RotateCcw icon)
   - Forward 10s (RotateCw icon)
   - Size: 32x32px
   - Icon: 20px
   - Hover: background rgba(255, 255, 255, 0.2)
   
   **Right Controls:**
   - Display: flex
   - Gap: 8px
   
   **Playback Speed:**
   - Button: "1x" text
   - Dropdown: 0.5x, 0.75x, 1x, 1.25x, 1.5x, 2x
   - Size: 32x32px
   - Font-size: sm
   
   **Quality Selector:**
   - Button: "720p" (or "Auto")
   - Dropdown: Auto, 1080p, 720p, 480p, 360p
   
   **Settings:**
   - Icon button (Settings icon)
   - Opens settings menu
   
   **Picture-in-Picture:**
   - Icon button (PictureInPicture icon)
   
   **Fullscreen:**
   - Icon button (Maximize icon)
   - Exit fullscreen: Minimize icon

4. **Loading Spinner:**
   When video buffering:
   - Spinner overlay
   - Center of video
   - Size: 60px
   - White color
   - Rotating animation

5. **Play Button Overlay:**
   When video paused:
   - Large play button
   - Center of video
   - Size: 80x80px
   - Background: rgba(59, 130, 246, 0.9)
   - Icon: 40px, white
   - Border-radius: full
   - Hover: scale(1.1), background primary-600

Provide complete HTML and CSS for video player controls.

Reference: UIUX_PLAN.md Section 5.5.2
```

**Expected Deliverables:**
- [ ] HTML for video controls overlay
- [ ] CSS for all control elements
- [ ] Progress bar with thumb
- [ ] Play/pause, skip, volume controls
- [ ] Playback speed and quality dropdowns
- [ ] Fullscreen and PIP buttons
- [ ] Loading spinner overlay
- [ ] Center play button for paused state

**Verification:**
- Controls appear on video hover
- Progress bar is interactive
- Volume slider works
- All buttons have hover effects
- Time display shows current/total time
- Fullscreen button toggles correctly

---

### Task UIUX-4.3: Design Transcript View Component 🔴

**Dependencies:** UIUX-4.2

**Prompt:**
```
Design the transcript view tab with timestamped text and speaker labels.

Create:

1. **Transcript Container:**
   - Padding: 24px
   - Max-height: 600px
   - Overflow-y: auto
   - Custom scrollbar styling

2. **Transcript Header:**
   - Display: flex
   - Justify-content: space-between
   - Align-items: center
   - Margin-bottom: 20px
   
   **Title:**
   - Font-size: lg (18px)
   - Font-weight: semibold
   - Color: gray-900
   
   **Actions:**
   - Search transcript button
   - Copy transcript button
   - Download transcript button
   - Size: 32x32px icon buttons

3. **Search Bar (toggleable):**
   - Height: 40px
   - Width: 100%
   - Background: gray-100
   - Border: 2px solid gray-300
   - Border-radius: --radius-md
   - Padding: 0 16px 0 40px
   - Margin-bottom: 16px
   - Search icon: absolute left
   - Focus: border primary-500

4. **Transcript Entry:**
   Each entry should have:
   - Display: flex
   - Gap: 16px
   - Padding: 16px 0
   - Border-bottom: 1px solid gray-100
   - Cursor: pointer
   - Hover: background gray-50
   
   **Timestamp:**
   - Width: 80px
   - Flex-shrink: 0
   - Font-family: monospace
   - Font-size: sm (14px)
   - Color: primary-600
   - Font-weight: medium
   - Format: "00:02:34"
   - Hover: color primary-700, underline
   - Click: jump to video time
   
   **Speaker Label:**
   - Width: 100px
   - Flex-shrink: 0
   - Font-size: sm (14px)
   - Font-weight: semibold
   - Color: gray-700
   - Format: "Speaker 1", "You", etc.
   
   **Text Content:**
   - Flex: 1
   - Font-size: base (16px)
   - Color: gray-900
   - Line-height: 1.6
   - Text-align: justify

5. **Active Entry:**
   When video playing at this timestamp:
   - Background: primary-50
   - Border-left: 3px solid primary-600
   - Padding-left: 13px (adjust for border)
   - Smooth scroll into view

6. **Highlight Search Results:**
   When searching:
   - Matched text: background warning-200, font-weight semibold
   - Active match: background warning-400
   - Navigation: Previous/Next buttons

7. **Empty State:**
   When no transcript:
   - Center text: "Transcript not available"
   - Icon: FileText (48px, gray-300)
   - Padding: 60px 20px

8. **Loading State:**
   When generating transcript:
   - Show 8-10 skeleton transcript entries
   - Animated shimmer

Provide complete HTML and CSS for transcript view with all states.

Reference: UIUX_PLAN.md Section 5.5.3
```

**Expected Deliverables:**
- [ ] HTML for transcript container and entries
- [ ] CSS for transcript layout
- [ ] Timestamp, speaker, and text sections
- [ ] Active entry highlighting
- [ ] Search functionality UI
- [ ] Search result highlighting
- [ ] Empty and loading states

**Verification:**
- Transcript entries are properly aligned
- Active entry has blue highlight
- Timestamps are clickable
- Search highlights matched text
- Scrollbar is styled
- Entries have hover effect

---

### Task UIUX-4.4: Design AI Summary View Component 🔴

**Dependencies:** UIUX-4.3

**Prompt:**
```
Design the AI Summary tab with key points, action items, and insights.

Create:

1. **Summary Container:**
   - Padding: 24px
   - Max-width: 800px (for readability)

2. **Summary Header:**
   - Margin-bottom: 24px
   
   **Title:**
   - Font-size: xl (20px)
   - Font-weight: semibold
   - Color: gray-900
   - Icon: Sparkles (for AI indication)
   
   **Description:**
   - Font-size: sm (14px)
   - Color: gray-600
   - Margin-top: 4px

3. **Summary Sections:**
   
   **Section Container:**
   - Margin-bottom: 32px
   
   **Section Title:**
   - Font-size: lg (18px)
   - Font-weight: semibold
   - Color: gray-900
   - Margin-bottom: 12px
   - Border-left: 4px solid primary-500
   - Padding-left: 16px

4. **Overview Section:**
   - Paragraph text
   - Font-size: base (16px)
   - Color: gray-800
   - Line-height: 1.7
   - Margin-bottom: 16px

5. **Key Points Section:**
   - Unordered list
   
   **List Items:**
   - Padding: 12px 0
   - Border-bottom: 1px dashed gray-200
   - Display: flex
   - Gap: 12px
   
   **Bullet:**
   - Custom: checkmark icon or blue dot
   - Size: 20px
   - Color: primary-600
   - Flex-shrink: 0
   
   **Text:**
   - Font-size: base (16px)
   - Color: gray-800
   - Line-height: 1.6

6. **Action Items Section:**
   - Similar to key points
   
   **Item:**
   - Checkbox (unchecked by default)
   - Text with assigned person (if any)
   - Due date (if any)
   
   **Checkbox:**
   - Size: 20x20px
   - Border: 2px solid gray-300
   - Border-radius: --radius-base
   - Hover: border primary-500
   - Checked: background primary-600, checkmark white

7. **Topics/Tags Section:**
   - Display: flex
   - Flex-wrap: wrap
   - Gap: 8px
   
   **Tag Chip:**
   - Height: 32px
   - Padding: 0 12px
   - Background: gray-100
   - Color: gray-700
   - Border-radius: --radius-full
   - Font-size: sm (14px)
   - Icon: Hash (14px)
   - Hover: background gray-200

8. **Participants Section:**
   - Display: flex
   - Gap: 12px
   - Flex-wrap: wrap
   
   **Participant Card:**
   - Display: flex
   - Align-items: center
   - Gap: 8px
   - Padding: 8px 12px
   - Background: white
   - Border: 1px solid gray-200
   - Border-radius: --radius-md
   
   **Avatar:**
   - Size: 32x32px
   - Border-radius: full
   
   **Name:**
   - Font-size: sm (14px)
   - Font-weight: medium
   - Color: gray-900

9. **Actions Bar:**
   At bottom of summary:
   - Display: flex
   - Gap: 12px
   - Padding-top: 20px
   - Border-top: 1px solid gray-200
   
   Buttons:
   - Regenerate Summary
   - Copy to Clipboard
   - Export as PDF
   - Share Summary

10. **Loading State:**
    When generating:
    - Skeleton blocks for each section
    - Animated gradient with AI sparkle effect

11. **Error State:**
    If summary fails:
    - Error message card
    - Retry button

Provide complete HTML and CSS for AI summary view.

Reference: UIUX_PLAN.md Section 5.5.4
```

**Expected Deliverables:**
- [ ] HTML for summary sections
- [ ] CSS for summary layout
- [ ] Key points list with custom bullets
- [ ] Action items with checkboxes
- [ ] Topic tags chips
- [ ] Participant cards
- [ ] Actions bar at bottom
- [ ] Loading and error states

**Verification:**
- Section titles have left border accent
- Key points have custom checkmark bullets
- Action items have interactive checkboxes
- Topic tags are pill-shaped
- Participant cards show avatars
- Actions bar is visible at bottom

---

### Task UIUX-4.5: Design Details Tab View 🟡

**Dependencies:** UIUX-4.4

**Prompt:**
```
Design the Details tab showing recording metadata and properties.

Create:

1. **Details Container:**
   - Padding: 24px
   - Max-width: 600px

2. **Property Row:**
   Each property should have:
   - Display: flex
   - Justify-content: space-between
   - Padding: 16px 0
   - Border-bottom: 1px solid gray-200
   
   **Label (left):**
   - Font-size: sm (14px)
   - Font-weight: medium
   - Color: gray-600
   - Width: 40%
   
   **Value (right):**
   - Font-size: sm (14px)
   - Color: gray-900
   - Width: 60%
   - Text-align: right

3. **Properties to Display:**
   - Recording Name (editable inline)
   - Created Date & Time
   - Duration
   - File Size
   - Format (MP4, WebM, etc.)
   - Resolution
   - Frame Rate
   - Bit Rate
   - Audio Channels
   - Storage Location (Google Drive link)
   - Recording ID
   - Last Modified

4. **Editable Fields:**
   - Recording Name: click to edit
     - Input appears in place
     - Save/Cancel buttons
   
   - Description: textarea field
     - Border: 1px solid gray-300
     - Border-radius: --radius-md
     - Padding: 12px
     - Min-height: 100px
     - Focus: border primary-500

5. **Tags Section:**
   - Label: "Tags"
   - Tag input field:
     - Type and press Enter to add
     - Height: 40px
     - Border: 1px solid gray-300
     - Border-radius: --radius-md
   - Tag chips display below:
     - Same style as in summary view
     - Close button (X) to remove

6. **Privacy Section:**
   - Label: "Sharing & Privacy"
   - Radio options:
     - Private (only you)
     - Shared (with specific people)
     - Public (anyone with link)
   - Share button if not private

7. **Danger Zone:**
   - Section at bottom
   - Background: error-50
   - Border: 1px solid error-200
   - Border-radius: --radius-md
   - Padding: 16px
   
   **Title:**
   - Font-size: sm
   - Font-weight: semibold
   - Color: error-700
   
   **Delete Button:**
   - Background: error-600
   - Color: white
   - Size: sm
   - Hover: background error-700

Provide complete HTML and CSS for details view.

Reference: UIUX_PLAN.md Section 5.5 (implied details view)
```

**Expected Deliverables:**
- [ ] HTML for property rows
- [ ] CSS for label-value layout
- [ ] Editable name field
- [ ] Description textarea
- [ ] Tags input and chips
- [ ] Privacy radio options
- [ ] Danger zone section with delete button

**Verification:**
- Property rows are evenly spaced
- Labels and values are aligned
- Editable fields show input on click
- Tags can be added and removed
- Danger zone is clearly distinguished with red color

---

### Task UIUX-4.6: Design Comments Section 🟡

**Dependencies:** UIUX-4.5

**Prompt:**
```
Design the comments section for collaborative feedback on recordings.

Create:

1. **Comments Container:**
   - Padding: 24px

2. **Comment Input (New Comment):**
   - Display at top
   - Margin-bottom: 32px
   
   **User Avatar:**
   - Size: 40x40px
   - Float left or flex layout
   
   **Input Field:**
   - Width: calc(100% - 56px) (space for avatar)
   - Min-height: 80px
   - Border: 2px solid gray-300
   - Border-radius: --radius-md
   - Padding: 12px
   - Font-size: base (16px)
   - Focus: border primary-500
   - Placeholder: "Add a comment..."
   
   **Action Buttons:**
   - Display: flex
   - Justify-content: flex-end
   - Gap: 8px
   - Margin-top: 8px
   - Cancel button (ghost)
   - Comment button (primary, disabled if empty)

3. **Comment Thread:**
   - Display: flex
   - Flex-direction: column
   - Gap: 24px

4. **Comment Item:**
   Each comment should have:
   - Display: flex
   - Gap: 12px
   
   **Avatar:**
   - Size: 40x40px
   - Flex-shrink: 0
   - Border-radius: full
   
   **Content Section:**
   - Flex: 1
   
   **Header:**
   - Display: flex
   - Align-items: center
   - Gap: 8px
   - Margin-bottom: 8px
   
   **Author Name:**
   - Font-size: sm (14px)
   - Font-weight: semibold
   - Color: gray-900
   
   **Timestamp:**
   - Font-size: xs (12px)
   - Color: gray-500
   - Format: "2 hours ago"
   
   **Comment Body:**
   - Font-size: base (16px)
   - Color: gray-800
   - Line-height: 1.6
   - White-space: pre-wrap (preserve line breaks)
   
   **Actions Row:**
   - Display: flex
   - Gap: 16px
   - Margin-top: 8px
   
   **Action Buttons:**
   - Like button (ThumbsUp icon + count)
   - Reply button
   - Edit button (only for author)
   - Delete button (only for author)
   - Font-size: xs
   - Color: gray-500
   - Hover: color primary-600

5. **Nested Replies:**
   - Margin-left: 52px (align with parent comment text)
   - Border-left: 2px solid gray-200
   - Padding-left: 20px

6. **Liked Comments:**
   - Like button active: color primary-600, filled icon
   - Like count: font-weight semibold

7. **Edit State:**
   When editing:
   - Comment text becomes textarea
   - Show Save/Cancel buttons
   - Border: primary-500

8. **Empty State:**
   When no comments:
   - Center text: "No comments yet"
   - Icon: MessageSquare (48px, gray-300)
   - Subtitle: "Be the first to comment"

9. **Loading State:**
   - Skeleton comment items

Provide complete HTML and CSS for comments section.

Reference: UIUX_PLAN.md Section 5.5 (implied collaboration features)
```

**Expected Deliverables:**
- [ ] HTML for comment input and thread
- [ ] CSS for comment layout
- [ ] Avatar and content alignment
- [ ] Action buttons (like, reply, edit, delete)
- [ ] Nested replies with indentation
- [ ] Edit state styling
- [ ] Empty and loading states

**Verification:**
- Comment input is at top
- Comments are properly spaced
- Nested replies are indented
- Like button changes state when clicked
- Edit mode shows textarea
- Empty state is centered

---

## Phase 5: Interaction & Animation Polish

**Duration:** 2-3 days  
**Goal:** Add micro-interactions, animations, and polish all interactive states.

---

### Task UIUX-5.1: Define Micro-Interactions Specifications 🟡

**Dependencies:** Phase 4 Complete

**Prompt:**
```
Document all micro-interactions and animation specifications for the application.

Create a comprehensive specification document including:

1. **Button Interactions:**
   - Click ripple effect:
     - Circular ripple from click point
     - Expansion from 0 to button width
     - Opacity: 0.3 to 0
     - Duration: 600ms
     - Easing: ease-out
   
   - Hover lift:
     - TranslateY: -2px
     - Shadow: increase from sm to md
     - Duration: 200ms
     - Easing: ease-out
   
   - Active press:
     - TranslateY: 0
     - Shadow: decrease to xs
     - Duration: 100ms
     - Scale: 0.98

2. **Card Interactions:**
   - Hover elevation:
     - TranslateY: -4px
     - Shadow: sm to lg
     - Border: gray-200 to primary-300
     - Duration: 300ms
     - Easing: ease-out
   
   - Thumbnail zoom:
     - Scale: 1 to 1.05
     - Duration: 300ms
     - Transform-origin: center

3. **Input Focus:**
   - Border transition:
     - From: gray-300
     - To: primary-500
     - Duration: 200ms
   
   - Focus ring:
     - Box-shadow: 0 0 0 3px primary-500 with 50% opacity
     - Duration: 200ms
     - Easing: ease-out

4. **Loading States:**
   - Spinner rotation:
     - 360deg continuous
     - Duration: 1s per rotation
     - Easing: linear
   
   - Skeleton shimmer:
     - Background position animation
     - Duration: 2s
     - Easing: ease-in-out
     - Infinite loop
   
   - Progress bar fill:
     - Width transition
     - Duration: 300ms
     - Easing: ease-out

5. **Modal/Drawer Animations:**
   - Modal entrance:
     - Backdrop: opacity 0 to 0.5 (200ms)
     - Content: scale 0.95 to 1 + opacity 0 to 1 (300ms)
     - Easing: ease-out
     - Stagger: backdrop first, then content
   
   - Drawer slide:
     - From: translateX(100%)
     - To: translateX(0)
     - Duration: 300ms
     - Easing: cubic-bezier(0.4, 0, 0.2, 1)

6. **Toast Notifications:**
   - Entrance (slide-in):
     - From: translateX(100%) + opacity 0
     - To: translateX(0) + opacity 1
     - Duration: 300ms
     - Easing: ease-out
   
   - Exit (slide-out):
     - To: translateX(100%) + opacity 0
     - Duration: 200ms
     - Easing: ease-in

7. **Dropdown Menus:**
   - Entrance:
     - From: opacity 0 + translateY(-10px) + scale(0.95)
     - To: opacity 1 + translateY(0) + scale(1)
     - Duration: 200ms
     - Easing: ease-out
   
   - Exit:
     - To: opacity 0 + scale(0.95)
     - Duration: 150ms
     - Easing: ease-in

8. **List Item Transitions:**
   - Staggered entrance:
     - Each item: fadeIn + slideUp
     - Delay between items: 50ms
     - First item: 0ms delay
     - Second item: 50ms delay
     - Third item: 100ms delay
     - etc.

9. **Success/Error Feedback:**
   - Success:
     - Checkmark icon: scale 0 to 1.2 to 1 (bounce)
     - Green background: fade in
     - Duration: 400ms
   
   - Error shake:
     - TranslateX: 0 → -10px → 10px → -10px → 0
     - Duration: 400ms
     - Easing: ease-in-out

10. **Recording Indicator:**
    - Pulse animation:
      - Opacity: 1 to 0.3 to 1
      - Scale: 1 to 1.2 to 1
      - Duration: 1.5s
      - Easing: ease-in-out
      - Infinite

For each interaction, provide:
- CSS code or specification
- When it should trigger
- Fallback for reduced-motion preference

Reference: UIUX_PLAN.md Section 3.6
```

**Expected Deliverables:**
- [ ] Complete micro-interaction specifications document
- [ ] CSS code for each animation
- [ ] Timing and easing functions defined
- [ ] Reduced-motion fallbacks

**Verification:**
- All interactions have clear specifications
- Durations are consistent with design system
- Easing functions are appropriate
- Reduced-motion alternatives provided

---

### Task UIUX-5.2: Implement Reduced Motion Support 🟡

**Dependencies:** UIUX-5.1

**Prompt:**
```
Implement reduced motion support for accessibility.

Create:

1. **Media Query:**
```css
@media (prefers-reduced-motion: reduce) {
  /* Reduced motion styles */
}
```

2. **Animation Overrides:**
   For users with motion sensitivity, override:
   
   - All transitions: duration 0.01ms (instant)
   - All animations: none or very subtle
   - Transforms: remove or minimal
   - Opacity transitions: keep (no motion)
   
   Exceptions (keep minimal animation):
   - Loading spinners: keep but slow down
   - Recording pulse: keep but more subtle
   - Focus indicators: keep opacity changes

3. **CSS Implementation:**
```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  
  /* Keep necessary animations but make subtle */
  .spinner {
    animation-duration: 1.5s !important;
  }
  
  .pulse {
    animation: pulse-subtle 2s infinite;
  }
}

@keyframes pulse-subtle {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}
```

4. **Documentation:**
   Create a comment block explaining:
   - Why reduced motion is important
   - What gets disabled
   - What remains (and why)
   - How to test (browser dev tools)

5. **Testing Checklist:**
   - [ ] Enable "Reduce motion" in OS settings
   - [ ] Verify animations are minimal
   - [ ] Check that UI is still functional
   - [ ] Ensure loading states are visible
   - [ ] Confirm focus indicators work

Provide complete CSS for reduced motion support.

Reference: UIUX_PLAN.md Section 7.1 (Accessibility)
```

**Expected Deliverables:**
- [ ] CSS media query for reduced motion
- [ ] Animation overrides implemented
- [ ] Documentation comments
- [ ] Testing checklist

**Verification:**
- All animations respect reduced motion
- Essential animations still function
- UI remains functional without animation
- Loading indicators are still visible

---

### Task UIUX-5.3: Polish Responsive Design 🔴

**Dependencies:** UIUX-5.2

**Prompt:**
```
Polish and verify responsive design across all breakpoints.

Create a testing and refinement specification:

1. **Breakpoint Review:**
   
   **Mobile (< 640px):**
   - Sidebar: drawer overlay (closed by default)
   - Header: hamburger menu, search icon only
   - Cards: single column, full width
   - Popup: 100% width, bottom sheet style
   - Tables: horizontal scroll or card conversion
   - Font sizes: scale down by 2px if needed
   
   **Tablet (640px - 1024px):**
   - Sidebar: collapsible, icon mode default
   - Header: full search bar, limited icons
   - Cards: 2 columns in grid
   - Comfortable touch targets (44px min)
   
   **Desktop (> 1024px):**
   - Sidebar: full width (280px)
   - All features visible
   - Optimal layout

2. **Touch Target Sizes:**
   Ensure all interactive elements:
   - Minimum: 44x44px
   - Comfortable: 48x48px
   - Buttons, checkboxes, radios: adequate padding
   - Links: adequate spacing

3. **Mobile Optimizations:**
   - Reduce animations on mobile (performance)
   - Simplify shadows (1-2 levels)
   - Use system fonts on mobile
   - Optimize images (smaller thumbnails)
   - Bottom navigation consideration

4. **Tablet-Specific:**
   - Two-column layouts where possible
   - Larger touch targets
   - Optimized for landscape and portrait

5. **Testing Matrix:**
   Create a table documenting:
   - Component name
   - Mobile behavior
   - Tablet behavior
   - Desktop behavior
   - Issues found
   - Resolution

6. **Common Responsive Patterns:**
   - Stack vertically on mobile
   - Side-by-side on desktop
   - Hide labels, show icons on mobile
   - Full-width on mobile, max-width on desktop
   - Adjust padding/spacing by breakpoint

7. **CSS Utilities:**
   Create responsive utility classes:
   - .mobile-only (display on < 640px)
   - .tablet-up (display on >= 640px)
   - .desktop-only (display on >= 1024px)
   - .mobile-hidden (hide on < 640px)

Provide:
- Testing matrix template
- Responsive utility classes CSS
- List of components to test
- Common issues and solutions

Reference: UIUX_PLAN.md Section 7.2 (Responsive Design)
```

**Expected Deliverables:**
- [ ] Breakpoint specifications reviewed
- [ ] Touch target sizes verified
- [ ] Testing matrix template created
- [ ] Responsive utility classes
- [ ] Component testing checklist

**Verification:**
- All interactive elements are 44px minimum
- Mobile layouts work on small screens
- Tablet layouts optimize space
- Desktop layouts use full features
- No horizontal scroll on mobile

---

### Task UIUX-5.4: Create Animation Utility Classes 🟡

**Dependencies:** UIUX-5.3

**Prompt:**
```
Create reusable animation utility classes for common interactions.

Create the following utility classes:

1. **Fade Animations:**
```css
.fade-in {
  animation: fadeIn 300ms ease-out forwards;
}

.fade-out {
  animation: fadeOut 200ms ease-in forwards;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}
```

2. **Slide Animations:**
```css
.slide-in-up {
  animation: slideInUp 300ms ease-out forwards;
}

.slide-in-down {
  animation: slideInDown 300ms ease-out forwards;
}

.slide-in-left {
  animation: slideInLeft 300ms ease-out forwards;
}

.slide-in-right {
  animation: slideInRight 300ms ease-out forwards;
}

/* Define all @keyframes */
```

3. **Scale Animations:**
```css
.scale-up {
  animation: scaleUp 200ms ease-out forwards;
}

.scale-down {
  animation: scaleDown 200ms ease-in forwards;
}

.bounce {
  animation: bounce 400ms ease-in-out;
}
```

4. **Shake/Wiggle:**
```css
.shake {
  animation: shake 400ms ease-in-out;
}

.wiggle {
  animation: wiggle 500ms ease-in-out;
}
```

5. **Spin/Rotate:**
```css
.spin {
  animation: spin 1s linear infinite;
}

.rotate-180 {
  transform: rotate(180deg);
  transition: transform 200ms ease-out;
}
```

6. **Pulse/Ping:**
```css
.pulse {
  animation: pulse 1.5s ease-in-out infinite;
}

.ping {
  animation: ping 1s ease-in-out infinite;
}
```

7. **Transition Utilities:**
```css
.transition-all {
  transition: all 200ms ease-out;
}

.transition-colors {
  transition: color 200ms ease-out, background-color 200ms ease-out, border-color 200ms ease-out;
}

.transition-transform {
  transition: transform 200ms ease-out;
}

.transition-opacity {
  transition: opacity 200ms ease-out;
}
```

8. **Duration Modifiers:**
```css
.duration-75 { transition-duration: 75ms; animation-duration: 75ms; }
.duration-100 { transition-duration: 100ms; animation-duration: 100ms; }
.duration-150 { transition-duration: 150ms; animation-duration: 150ms; }
.duration-200 { transition-duration: 200ms; animation-duration: 200ms; }
.duration-300 { transition-duration: 300ms; animation-duration: 300ms; }
.duration-500 { transition-duration: 500ms; animation-duration: 500ms; }
.duration-700 { transition-duration: 700ms; animation-duration: 700ms; }
.duration-1000 { transition-duration: 1000ms; animation-duration: 1000ms; }
```

9. **Delay Utilities:**
```css
.delay-75 { transition-delay: 75ms; animation-delay: 75ms; }
.delay-100 { transition-delay: 100ms; animation-delay: 100ms; }
/* ... through delay-1000 */
```

10. **Easing Utilities:**
```css
.ease-linear { transition-timing-function: linear; }
.ease-in { transition-timing-function: cubic-bezier(0.4, 0, 1, 1); }
.ease-out { transition-timing-function: cubic-bezier(0, 0, 0.2, 1); }
.ease-in-out { transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1); }
```

For each utility:
- Define the class
- Define associated @keyframes if needed
- Add reduced-motion override
- Provide usage example

Reference: UIUX_PLAN.md Section 3.6
```

**Expected Deliverables:**
- [ ] Complete animation utility classes CSS file
- [ ] All @keyframes defined
- [ ] Reduced-motion overrides
- [ ] Usage examples for each utility

**Verification:**
- All utility classes work correctly
- Keyframes are smooth
- Reduced-motion disables animations
- Classes can be combined (fade-in + slide-in-up)

---

## Phase 6: Documentation & Handoff

**Duration:** 1-2 days  
**Goal:** Create comprehensive documentation for Frontend Agent handoff.

---

### Task UIUX-6.1: Create Component Style Guide 🔴

**Dependencies:** Phase 5 Complete

**Prompt:**
```
Create a comprehensive component style guide documenting all UI components.

Create a style guide document with the following structure:

1. **Document Header:**
   - Title: "OpenMeet Recorder - UI Component Style Guide"
   - Version: 1.0.0
   - Date
   - Purpose statement

2. **Design Tokens Reference:**
   - Colors (with hex codes and variable names)
   - Typography (all font sizes, weights, families)
   - Spacing (all spacing values)
   - Shadows (all shadow levels)
   - Border radius (all radius values)
   - Z-index layers

3. **Component Categories:**

   **A. Buttons:**
   - All button variants (primary, secondary, success, error, ghost, link)
   - All sizes (sm, md, lg)
   - All states (default, hover, active, disabled, loading)
   - Icon buttons
   - Button groups
   - Code examples (HTML + CSS classes)
   - Visual examples (screenshots or descriptions)
   - Usage guidelines (when to use each variant)

   **B. Input Components:**
   - Text inputs
   - Textareas
   - Select dropdowns
   - Checkboxes
   - Radio buttons
   - Toggle switches
   - All states (default, focus, error, success, disabled)
   - Code examples
   - Usage guidelines

   **C. Cards:**
   - Recording cards (grid view)
   - Recording cards (list view)
   - Info cards
   - All states (default, hover, active)
   - Code examples

   **D. Navigation:**
   - Sidebar navigation
   - Sidebar items (default, hover, active)
   - Breadcrumbs
   - Tabs
   - Code examples

   **E. Modals & Overlays:**
   - Modal structure
   - Drawer/sidebar panels
   - Toast notifications
   - Tooltips
   - Popovers
   - Code examples

   **F. Feedback Components:**
   - Loading spinners
   - Skeleton loaders
   - Progress bars
   - Success/error messages
   - Info notes
   - Code examples

   **G. Layout Components:**
   - Page container
   - Section wrappers
   - Grid layouts
   - Flexbox patterns
   - Code examples

4. **Component Anatomy:**
   For each major component, provide:
   - Visual breakdown (parts labeled)
   - HTML structure
   - CSS classes
   - Required props/attributes
   - Optional props/attributes
   - Default values
   - Variants

5. **Interaction Patterns:**
   - Hover states
   - Focus states
   - Active/pressed states
   - Disabled states
   - Loading states
   - Error states
   - Animation specs

6. **Accessibility Notes:**
   For each component:
   - ARIA attributes required
   - Keyboard navigation support
   - Screen reader considerations
   - Focus management
   - Color contrast ratios

7. **Responsive Behavior:**
   For each component:
   - Mobile (<640px) behavior
   - Tablet (640-1024px) behavior
   - Desktop (>1024px) behavior

8. **Code Conventions:**
   - Class naming convention (BEM recommended)
   - CSS organization
   - File structure
   - Import order

9. **Common Patterns:**
   - Form layouts
   - List/grid patterns
   - Navigation patterns
   - Content hierarchy

10. **Don'ts (Anti-patterns):**
    - What NOT to do
    - Common mistakes
    - Accessibility violations

Format as Markdown with clear sections, code blocks, and tables where appropriate.

Reference: UIUX_PLAN.md entire document
```

**Expected Deliverables:**
- [ ] Complete style guide document (Markdown)
- [ ] All components documented
- [ ] Code examples for each component
- [ ] Usage guidelines
- [ ] Accessibility notes
- [ ] Responsive behavior documented

**Verification:**
- Style guide is comprehensive
- All components from UIUX_PLAN.md are included
- Code examples are accurate
- Accessibility information is complete
- Document is well-organized

---

### Task UIUX-6.2: Create Asset Preparation Checklist 🔴

**Dependencies:** UIUX-6.1

**Prompt:**
```
Create a checklist of all assets that need to be prepared for Frontend Agent.

Create a comprehensive checklist document:

1. **Icons:**
   - [ ] List all icons needed (from Lucide React library)
   - [ ] Document icon sizes used (16px, 18px, 20px, 24px, 32px, etc.)
   - [ ] Color variants (gray-600, primary-600, white, etc.)
   - [ ] Icons by component/page

2. **Images:**
   - [ ] Logo files (SVG, PNG)
     - Full logo (with text)
     - Icon only (for collapsed sidebar, favicon)
     - Different sizes
   - [ ] Empty state illustrations
   - [ ] Error state illustrations
   - [ ] Placeholder thumbnails

3. **CSS Files:**
   - [ ] design-system.css (design tokens)
   - [ ] globals.css (global styles and resets)
   - [ ] animations.css (animation utilities)
   - [ ] components/*.css (component-specific styles)

4. **Configuration Files:**
   - [ ] tailwind.config.js (complete configuration)
   - [ ] components.json (ShadcnUI configuration)
   - [ ] postcss.config.js (if needed)

5. **Font Files:**
   - [ ] System fonts stack defined
   - [ ] Monospace fonts stack defined
   - [ ] Web fonts (if custom fonts used)
     - WOFF2 format
     - Font face declarations

6. **Documentation Files:**
   - [ ] Style guide (from Task 6.1)
   - [ ] README.md (setup instructions)
   - [ ] Component documentation
   - [ ] Design decision rationale

7. **Color Exports:**
   - [ ] Design tokens as JSON
   - [ ] Color palette reference
   - [ ] Semantic color mapping

8. **Spacing & Typography Exports:**
   - [ ] Spacing scale as JSON
   - [ ] Typography scale as JSON
   - [ ] Line heights, font weights

9. **Component Specs:**
   For each component:
   - [ ] HTML structure
   - [ ] CSS classes
   - [ ] States (hover, focus, active, disabled)
   - [ ] Responsive behavior
   - [ ] Accessibility requirements

10. **Animation Specifications:**
    - [ ] All keyframe animations
    - [ ] Transition specifications
    - [ ] Easing functions
    - [ ] Duration values

11. **Testing Artifacts:**
    - [ ] Responsive testing checklist
    - [ ] Accessibility testing checklist
    - [ ] Browser compatibility list
    - [ ] Component testing checklist

12. **Reference Materials:**
    - [ ] UIUX_PLAN.md (this document)
    - [ ] IMPLEMENTATION_PLAN.md (overall project)
    - [ ] Link to design inspiration (if any)
    - [ ] Brand guidelines (if any)

Format as a Markdown checklist that can be tracked.

Reference: UIUX_PLAN.md Section 10.1
```

**Expected Deliverables:**
- [ ] Complete asset checklist document
- [ ] All asset categories listed
- [ ] Clear organization by type
- [ ] Actionable checkbox items

**Verification:**
- All assets from UIUX_PLAN.md are included
- Checklist is comprehensive
- Items are specific and actionable
- Organization makes sense

---

### Task UIUX-6.3: Write Frontend Handoff Notes 🔴

**Dependencies:** UIUX-6.2

**Prompt:**
```
Write comprehensive handoff notes for the Frontend Agent.

Create a handoff document with the following sections:

1. **Executive Summary:**
   - Brief overview of what has been completed in UI/UX phase
   - What Frontend Agent needs to implement
   - Timeline estimate for frontend implementation

2. **Design System Status:**
   - ✅ Complete: Design tokens, colors, typography, spacing
   - ✅ Complete: TailwindCSS configuration
   - ✅ Complete: ShadcnUI setup
   - ✅ Complete: Animation system
   - 📦 Delivered: All CSS files ready

3. **Component Inventory:**
   Table with columns:
   - Component Name
   - Location in Style Guide
   - Priority (Critical, High, Medium, Low)
   - Implementation Notes
   - Dependencies

   Examples:
   - Button (All variants) - Critical - Base component
   - Recording Card - High - Depends on Button
   - Video Player Controls - High - Complex interactions
   - Transcript View - Medium - Depends on layout
   etc.

4. **Implementation Order Recommendation:**
   Suggested sequence:
   
   Phase 1: Foundation (1-2 days)
   - Set up design system CSS
   - Configure TailwindCSS
   - Install ShadcnUI components
   - Create base layout structure
   
   Phase 2: Core Components (2-3 days)
   - Button component (all variants)
   - Input components (text, checkbox, radio, toggle)
   - Card components
   - Navigation components
   
   Phase 3: Extension UI (2-3 days)
   - Popup interface
   - Control bar
   - Options page
   
   Phase 4: Dashboard UI (3-4 days)
   - Sidebar navigation
   - Header bar
   - Recording grid/list views
   - Detail page layout
   
   Phase 5: Interactive Components (2-3 days)
   - Video player controls
   - Transcript view
   - AI summary view
   - Comments section
   
   Phase 6: Polish & Testing (2-3 days)
   - Animations and transitions
   - Responsive design
   - Accessibility testing
   - Cross-browser testing

5. **Critical Implementation Notes:**
   
   **Responsive Design:**
   - Mobile-first approach recommended
   - Test at all breakpoints (< 640px, 640-1024px, > 1024px)
   - Use responsive utility classes
   - Touch targets minimum 44x44px
   
   **Accessibility:**
   - All interactive elements must be keyboard accessible
   - ARIA labels on icon buttons
   - Focus indicators visible
   - Color contrast WCAG AA compliant
   - Reduced-motion support implemented
   
   **Performance:**
   - Lazy load components not immediately visible
   - Optimize images (use WebP, proper sizing)
   - Code splitting for routes
   - Minimize bundle size
   
   **Browser Support:**
   - Chrome/Edge (latest 2 versions)
   - Firefox (latest 2 versions)
   - Safari (latest 2 versions)
   - Mobile Safari (iOS 14+)
   - Chrome Mobile (latest)

6. **Known Challenges & Considerations:**
   - Video player: May need custom controls library
   - Control bar: Draggable positioning needs JS
   - Transcript: Auto-scroll to active entry
   - Responsive video player: Maintain aspect ratio
   - Dark mode: Considered for future, not in scope

7. **Dependencies & Prerequisites:**
   - Node.js 18+ and pnpm
   - React 18.2+
   - TypeScript 5.3+
   - Vite 5.0+
   - TailwindCSS 3.4+
   - ShadcnUI components
   - Lucide React icons

8. **Testing Requirements:**
   - Component unit tests (optional but recommended)
   - Visual regression testing (optional)
   - Manual testing checklist provided
   - Accessibility testing with screen reader
   - Responsive design testing on real devices

9. **Reference Documents:**
   - UIUX_PLAN.md - Complete UI/UX specifications
   - Component Style Guide - All component details
   - Asset Checklist - All assets to be implemented
   - IMPLEMENTATION_PLAN.md - Overall project scope

10. **Communication Protocol:**
    - Questions about design: refer to UIUX_PLAN.md first
    - Ambiguities: document and request clarification
    - Design changes: should go through user approval
    - Implementation blockers: escalate immediately

11. **Definition of Done (Frontend Implementation):**
    - [ ] All components from style guide implemented
    - [ ] All pages/views functional
    - [ ] Responsive design working across breakpoints
    - [ ] Accessibility requirements met
    - [ ] Animations and transitions implemented
    - [ ] Cross-browser tested
    - [ ] Code reviewed and documented
    - [ ] Ready for Backend Agent integration

Format as a clear, structured Markdown document.

Reference: UIUX_PLAN.md Section 10
```

**Expected Deliverables:**
- [ ] Complete frontend handoff document
- [ ] Component inventory with priorities
- [ ] Implementation order recommendations
- [ ] Critical notes and considerations
- [ ] Definition of done criteria

**Verification:**
- Document is comprehensive
- Implementation order is logical
- All critical information included
- Clear and actionable
- Ready for Frontend Agent to use

---

### Task UIUX-6.4: Create Design Decision Log 🟡

**Dependencies:** UIUX-6.3

**Prompt:**
```
Create a design decision log documenting key decisions made during UI/UX phase.

Create a decision log with the following format:

For each major decision:

**Decision ID:** DD-001  
**Date:** 2025-12-29  
**Category:** Design System  
**Decision:** Use TailwindCSS as the primary styling framework  
**Context:** Need a consistent, scalable styling approach for both extension and dashboard  
**Options Considered:**
1. Plain CSS with CSS variables
2. TailwindCSS
3. CSS-in-JS (styled-components)
4. Sass/SCSS

**Chosen Option:** TailwindCSS  
**Rationale:**
- Utility-first approach provides consistency
- Excellent with ShadcnUI components
- Good performance (PurgeCSS)
- Large community and ecosystem
- Responsive design made easy

**Trade-offs:**
- Learning curve for team unfamiliar with utility classes
- HTML can look verbose
- Mitigation: Use @apply for common patterns, component classes

**Impact:**
- All components will use Tailwind classes
- CSS bundle size will be optimized
- Consistent spacing and colors guaranteed

**Status:** ✅ Implemented

---

Document at least 10-15 major decisions such as:
- DD-001: TailwindCSS choice
- DD-002: ShadcnUI component library
- DD-003: Blue as primary brand color
- DD-004: 4px spacing grid system
- DD-005: 16:9 video player aspect ratio
- DD-006: Sidebar width 280px (expanded)
- DD-007: Lucide React for icons
- DD-008: Dark control bar for recording
- DD-009: Glassmorphism effect for control bar
- DD-010: Card hover lift effect
- DD-011: Transcript timestamped by entry
- DD-012: Grid vs List view toggle
- DD-013: Reduced motion support
- DD-014: Mobile drawer sidebar
- DD-015: WCAG AA as accessibility target (optional)

For each, provide:
- Clear context
- Options considered
- Rationale for choice
- Trade-offs and mitigations
- Impact on implementation

Format as Markdown.

Reference: UIUX_PLAN.md (decisions implied throughout)
```

**Expected Deliverables:**
- [ ] Design decision log document
- [ ] 10-15 major decisions documented
- [ ] Clear rationale for each decision
- [ ] Trade-offs acknowledged
- [ ] Impact on implementation noted

**Verification:**
- All major decisions captured
- Rationale is clear and logical
- Trade-offs are honestly assessed
- Document is useful for future reference
- Decisions align with UIUX_PLAN.md

---

### Task UIUX-6.5: Final Review and Handoff Preparation 🔴

**Dependencies:** UIUX-6.1, UIUX-6.2, UIUX-6.3, UIUX-6.4

**Prompt:**
```
Conduct a final review of all UI/UX deliverables and prepare for handoff.

Create a final review checklist and handoff package:

1. **Deliverables Review Checklist:**

   **Design System:**
   - [ ] All CSS variables defined in design-system.css
   - [ ] TailwindCSS configuration complete
   - [ ] ShadcnUI configured correctly
   - [ ] Animation system documented
   - [ ] All design tokens exported

   **Component Specifications:**
   - [ ] All extension components specified
   - [ ] All dashboard components specified
   - [ ] All interactive states documented
   - [ ] All responsive behaviors defined
   - [ ] All accessibility requirements listed

   **Documentation:**
   - [ ] Style guide complete and accurate
   - [ ] Asset checklist comprehensive
   - [ ] Handoff notes clear and actionable
   - [ ] Design decision log complete
   - [ ] README with setup instructions

   **Code Deliverables:**
   - [ ] design-system.css file
   - [ ] globals.css file
   - [ ] animations.css file
   - [ ] tailwind.config.js file
   - [ ] components.json file

   **Visual Specifications:**
   - [ ] All colors documented with hex codes
   - [ ] All spacing values specified
   - [ ] All typography specs provided
   - [ ] All shadow levels defined
   - [ ] All border radius values listed

   **Interaction Specifications:**
   - [ ] All animations documented
   - [ ] All transitions specified
   - [ ] All hover states defined
   - [ ] All focus states defined
   - [ ] All loading states specified

2. **Quality Assurance:**
   - [ ] All specifications are clear and unambiguous
   - [ ] No contradictions in documentation
   - [ ] All references to other documents are accurate
   - [ ] Code examples are syntactically correct
   - [ ] Terminology is consistent throughout

3. **Handoff Package Contents:**
   Create a folder structure:
   ```
   uiux-handoff/
   ├── docs/
   │   ├── STYLE_GUIDE.md
   │   ├── HANDOFF_NOTES.md
   │   ├── DESIGN_DECISIONS.md
   │   └── ASSET_CHECKLIST.md
   ├── css/
   │   ├── design-system.css
   │   ├── globals.css
   │   └── animations.css
   ├── config/
   │   ├── tailwind.config.js
   │   └── components.json
   └── README.md
   ```

4. **README.md for Handoff Package:**
   Include:
   - Overview of the package
   - What's included
   - How to use the documentation
   - Setup instructions
   - Where to start (recommended reading order)
   - Contact information for questions

5. **Handoff Meeting Agenda:**
   Prepare talking points:
   - Overview of design system
   - Key design decisions
   - Component priorities
   - Implementation recommendations
   - Known challenges
   - Q&A

6. **Follow-up Plan:**
   - Availability for questions during implementation
   - Process for design clarifications
   - Review checkpoints
   - Design QA during implementation

7. **Sign-off Document:**
   Create a simple sign-off checklist:
   - [ ] User has reviewed all UI/UX deliverables
   - [ ] User approves the design system
   - [ ] User approves component specifications
   - [ ] User approves interaction patterns
   - [ ] User approves handoff package
   - [ ] Ready to proceed to Frontend implementation

   Signatures:
   - UI/UX Agent: [Date]
   - User: [Date]

Provide:
- Complete review checklist
- Handoff package structure
- README content
- Sign-off document

Reference: UIUX_PLAN.md Section 10
```

**Expected Deliverables:**
- [ ] Final review checklist completed
- [ ] Handoff package structure defined
- [ ] README for handoff package
- [ ] Handoff meeting agenda
- [ ] Sign-off document prepared

**Verification:**
- All deliverables are complete
- Quality assurance checks pass
- Handoff package is well-organized
- Documentation is clear and comprehensive
- Ready for user review and Frontend Agent handoff

---

## Appendix: Quick Reference

### Color Variables Quick Reference
```css
--color-primary-500: #3b82f6
--color-success-500: #22c55e
--color-warning-500: #f59e0b
--color-error-500: #ef4444
--color-gray-900: #111827
```

### Typography Quick Reference
```css
--text-sm: 0.875rem (14px)
--text-base: 1rem (16px)
--text-lg: 1.125rem (18px)
--text-xl: 1.25rem (20px)
--text-2xl: 1.5rem (24px)
```

### Spacing Quick Reference
```css
--spacing-4: 1rem (16px)
--spacing-6: 1.5rem (24px)
--spacing-8: 2rem (32px)
```

### Animation Duration Quick Reference
```css
--duration-200: 200ms (default interactions)
--duration-300: 300ms (modals, slides)
--duration-500: 500ms (complex animations)
```

### Common Patterns
- Button hover: lift 2px + shadow increase
- Card hover: lift 4px + border color change
- Input focus: blue border + focus ring
- Loading: spinner or skeleton shimmer
- Error: shake animation + red color

---

## End of UIUX_PROMPT.md

**Total Tasks:** 47 tasks across 6 phases
**Estimated Duration:** 18-25 days

**Next Steps:**
1. User reviews this prompt file
2. User executes prompts sequentially with UI/UX Agent
3. Upon completion, move to FRONTEND_PROMPT.md
4. Frontend Agent implements designs
5. Backend Agent adds functionality

**Questions or Issues:**
Refer to UIUX_PLAN.md for detailed specifications or contact the project owner.

