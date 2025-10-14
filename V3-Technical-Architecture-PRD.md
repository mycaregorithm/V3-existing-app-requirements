# MyCaregorithm V3 - Technical Architecture PRD
## Product Requirements Document for V4 Migration

**Document Version:** 1.0  
**Last Updated:** October 14, 2025  
**Source:** mycare-frontend-v3 Codebase Analysis  
**Purpose:** Document existing V3 technical architecture to ensure zero functionality loss during V4 migration

---

## Executive Summary

This PRD documents the complete technical architecture of MyCaregorithm V3 frontend application. It serves as the single source of truth for V4 development, ensuring all current functionality, integrations, and architectural patterns are preserved or intentionally evolved.

**Key Statistics:**
- **Version:** 3.9.0
- **Build Tool:** Vite 5.4.19
- **Framework:** React 18.2.0
- **State Management:** Redux Toolkit 2.2.1
- **Authentication:** Azure MSAL (Microsoft Authentication Library)
- **UI Library:** Material-UI 5.15.10
- **Total Dependencies:** 100+ npm packages
- **Cancer Modules:** 19 distinct cancer types
- **Deployment:** Azure Static Web Apps

---

## Table of Contents

1. [Technology Stack](#1-technology-stack)
2. [Application Architecture](#2-application-architecture)
3. [Authentication & Authorization](#3-authentication--authorization)
4. [Routing & Navigation](#4-routing--navigation)
5. [State Management](#5-state-management)
6. [Case Workflow Implementation](#6-case-workflow-implementation)
7. [API Integration](#7-api-integration)
8. [UI Component System](#8-ui-component-system)
9. [Analytics & Tracking](#9-analytics--tracking)
10. [Internationalization](#10-internationalization)
11. [Build & Deployment](#11-build--deployment)
12. [Data Models](#12-data-models)
13. [Migration Recommendations](#13-migration-recommendations)

---

## 1. Technology Stack

### 1.1 Core Framework & Build
**React Ecosystem:**
- `react: 18.2.0` - Core framework
- `react-dom: 18.2.0` - DOM rendering
- `react-router-dom: 6.22.1` - Client-side routing
- `vite: 5.4.19` - Build tool (HMR, fast builds)

**Type Safety:**
- TypeScript support (partial - `.ts`, `.tsx` files)
- `vite-env.d.js` for type definitions

### 1.2 State Management
**Redux Stack:**
- `@reduxjs/toolkit: 2.2.1` - State management
- `react-redux: 9.1.0` - React bindings
- `redux: 5.0.1` - Core Redux
- `redux-persist: 6.0.0` - State persistence

**Context API:**
- ApplicationContext - Global app state
- ModuleProvider - Module-specific context
- DirtyGuardProvider - Unsaved changes detection

### 1.3 UI Framework
**Material-UI Ecosystem:**
- `@mui/material: 5.15.10` - Core components
- `@mui/icons-material: 5.15.10` - Icon library
- `@mui/x-data-grid: 6.19.5` - Data tables
- `@mui/x-date-pickers: 6.11.2` - Date/time pickers
- `@mui/x-tree-view: 6.17.0` - Tree components
- `@emotion/react: 11.11.3` - CSS-in-JS
- `@emotion/styled: 11.11.0` - Styled components

**Additional UI Libraries:**
- `@tabler/icons-react: 2.47.0` - Tabler icons
- `material-react-table: 2.13.0` - Advanced tables
- `notistack: 3.0.1` - Toast notifications

### 1.4 Authentication
**Azure AD Integration:**
- `@azure/msal-browser: 3.17.0` - MSAL core
- `@azure/msal-react: 2.0.19` - React integration
- `@azure/identity: 4.4.1` - Azure identity
- `jwt-decode: 4.0.0` - JWT token decoding

### 1.5 Form Management
**Form Libraries:**
- `formik: 2.4.5` - Form state management
- `react-hook-form: 7.50.1` - Hook-based forms
- `yup: 1.3.3` - Schema validation
- `@hookform/resolvers: 3.3.4` - Validation resolvers

### 1.6 Data Visualization
**Charts & Graphics:**
- `apexcharts: 3.46.0` - Charting library
- `react-apexcharts: 1.4.1` - React wrapper
- `three: 0.180.0` - 3D graphics (liver demo)
- `@fullcalendar/*` - Calendar components

### 1.7 Rich Content
**Editors & Media:**
- `react-quill: 2.0.0` - Rich text editor
- `draft-js: 0.11.7` - Text editor framework
- `react-sketch-canvas: 7.0.0-next.2` - Canvas drawing
- `@hello-pangea/dnd: 16.5.0` - Drag and drop

### 1.8 Utilities
**Data Handling:**
- `axios: 1.8.4` - HTTP client
- `date-fns: 2.30.0` - Date utilities
- `lodash-es: 4.17.21` - Utility functions
- `uuid: 9.0.1` - UUID generation

**Security:**
- `dompurify: 3.2.5` - XSS protection
- `html-react-parser: 5.2.3` - Safe HTML parsing

---

## 2. Application Architecture

### 2.1 Project Structure

```
mycare-frontend-v3/
├── src/
│   ├── analytics.ts                # Freshpaint analytics
│   ├── App.jsx                     # Root component
│   ├── index.jsx                   # Entry point
│   ├── config.js                   # App configuration
│   ├── authConfig.js               # Azure AD config
│   │
│   ├── components/                 # Reusable components
│   │   ├── DiagnosisStaging/      # D&S component
│   │   ├── HomeScreen/            # Dashboard
│   │   ├── PatientInformation/    # Patient ID
│   │   ├── PlaceholderPage/       # Placeholder
│   │   └── ProgressBar/           # Progress indicator
│   │
│   ├── views/                      # Page components
│   │   ├── case/                  # Case workflow
│   │   │   ├── index.jsx          # Case stepper (main)
│   │   │   ├── PatientID.jsx      # Step 1
│   │   │   ├── Diagnosis.jsx      # Step 2
│   │   │   ├── TreatmentPlan.jsx  # Step 3
│   │   │   ├── CompareOptions.jsx # Step 4
│   │   │   ├── Resources.jsx      # Step 5
│   │   │   ├── NextSteps.jsx      # Step 6
│   │   │   ├── Share.jsx          # Step 7
│   │   │   ├── Notes.jsx          # Step 8
│   │   │   └── utils/             # Helpers
│   │   ├── dashboard/             # Home dashboard
│   │   ├── allCases/              # Case listing
│   │   ├── organizations/         # Org management
│   │   └── users/                 # User management
│   │
│   ├── store/                      # Redux store
│   │   ├── store.js               # Store configuration
│   │   ├── formSlice.js           # Form state
│   │   ├── diagnosisReducers.js   # Diagnosis state
│   │   ├── customizationReducer.js # UI state
│   │   └── slices/                # Feature slices
│   │
│   ├── contexts/                   # React contexts
│   │   ├── ApplicationContext.jsx  # Global state
│   │   ├── Auth0Context.jsx       # Auth (legacy)
│   │   └── JWTContext.jsx         # JWT auth
│   │
│   ├── guards/                     # Route protection
│   │   └── DirtyGuardProvider.jsx # Unsaved changes
│   │
│   ├── routes/                     # Routing
│   │   ├── index.jsx              # Route renderer
│   │   ├── MainRoutes.jsx         # Main routes
│   │   ├── AuthenticationRoutes.jsx # Auth routes
│   │   └── RequireAuth.jsx        # Auth guard
│   │
│   ├── layout/                     # Layouts
│   │   ├── MainLayout/            # Main app layout
│   │   ├── MinimalLayout/         # Auth layout
│   │   └── NavigationScroll.jsx   # Scroll handler
│   │
│   ├── ui-component/               # UI library
│   │   ├── cards/                 # Card components
│   │   ├── extended/              # Extended MUI
│   │   └── third-party/           # Integrations
│   │
│   ├── utils/                      # Utilities
│   │   ├── axios.js               # HTTP client
│   │   ├── authConstants.js       # Role constants
│   │   └── route-guard/           # Auth guards
│   │
│   ├── themes/                     # MUI themes
│   ├── locales/                    # i18n
│   ├── constants/                  # Constants
│   └── assets/                     # Static files
│
├── public/                         # Public assets
│   ├── assets/locale/             # Locale images
│   └── models/                    # 3D models
│
├── index.html                      # HTML template
├── vite.config.mjs                # Vite config
├── package.json                   # Dependencies
└── staticwebapp.*.json            # Azure configs
```

### 2.2 Core Application Flow

**App.jsx - Root Component:**
```javascript
Provider (Redux Store)
  └── ModuleProvider
      └── StyledEngineProvider (MUI)
          └── ThemeProvider
              └── CssBaseline
                  └── MsalProvider (Azure AD)
                      └── IntlProvider (i18n)
                          └── ApplicationContext
                              └── DirtyGuardProvider
                                  └── NavigationScroll
                                      └── Routes
```

**Key Features:**
- Multi-level provider nesting for separation of concerns
- Theme customization via Redux state
- Internationalization (en/es)
- MSAL navigation client integration
- Dirty state tracking across app

### 2.3 Design Patterns

**Component Patterns:**
- Functional components with hooks
- Lazy loading for route components
- Compound components for complex UI
- Custom hooks for reusable logic

**State Patterns:**
- Redux Toolkit for global state
- Context API for cross-cutting concerns
- Local state for component-specific data
- URL state for navigation/filters

**Data Flow:**
- Unidirectional data flow (React pattern)
- API integration via Axios interceptors
- Centralized error handling
- Debounced auto-save (500ms baseline)

---

## 3. Authentication & Authorization

### 3.1 Azure AD Configuration

**authConfig.js:**
```javascript
{
  auth: {
    clientId: VITE_CLIENT_ID,
    authority: `https://login.microsoftonline.com/${VITE_TENANT_ID}`,
    redirectUri: "/"
  },
  cache: {
    cacheLocation: "localStorage",
    storeAuthStateInCookie: true (IE/Edge/Firefox)
  }
}
```

**MSAL Instance:**
- Browser detection for cookie storage
- Logger with PII protection
- Native broker disabled
- Select account prompt

### 3.2 Authorization Scopes

**SCOPES Object:**
```javascript
{
  USER_READ: ["User.Read"],
  USER_READ_WRITE: ["User.ReadWrite.All"],
  USER_AUTH_READ_WRITE: ["UserAuthenticationMethod.ReadWrite.All"],
  DEFAULT: [`${VITE_CLIENT_ID}/.default`]
}
```

**MS Graph APIs:**
- `/v1.0/me` - Current user
- `/v1.0/users` - User management

**Internal APIs:**
- `https://mcgdevbackend.azurewebsites.net/api/users`
- `https://mcgdevbackend.azurewebsites.net/api/organizations`
- `https://mcgdevbackend.azurewebsites.net/api/users/me`

### 3.3 Role-Based Access Control

**Role Constants (ROLES):**
```javascript
{
  APP: 1,
  MCG_Administrator: 2,
  Auditor: 3,
  Nurse: 4,
  Administrator: 5,
  Physician: 6,
  Scribe: 7,
  Patient: 8,
  CareGiver: 9
}
```

**User Data Structure:**
```javascript
{
  userID: number,
  userEmail: string,
  firstName: string,
  lastName: string,
  roleID: number,
  organizationID: number,
  orgAKA: string,
  userNPI: string
}
```

### 3.4 Route Protection

**RequireAuth Component:**
- Wraps protected routes
- Redirects to login if unauthenticated
- Uses MSAL authentication state

**DirtyGuard Protection:**
- Prevents navigation with unsaved changes
- Disabled for Patient role
- Modal with 3 options:
  1. Go to Share
  2. Discard & Leave
  3. Stay Here

---

## 4. Routing & Navigation

### 4.1 Route Structure

**Main Routes:**
```javascript
// New Figma Design (no MainLayout)
'/' → AllCasesModules (Dashboard)
'/patient-information' → PatientInformation
'/diagnosis-staging' → DiagnosisStaging
'/treatment-plan' → PlaceholderPage
'/compare-options' → PlaceholderPage
'/resources' → PlaceholderPage
'/next-steps' → PlaceholderPage
'/share' → PlaceholderPage
'/notes' → PlaceholderPage

// Legacy Routes (with MainLayout)
'/organizations' → OrganizationsPage
'/users' → UsersPage
'/cases' → AllCasesPage
'/case/:moduleID' → CaseStepper
'/voka-liver-demo' → VokaLiverDemo
```

**Key Observations:**
- New Figma design pages bypass MainLayout
- Placeholder pages for unimplemented workflow steps
- Case stepper is the current production implementation
- Module-based routing (`:moduleID` param)

### 4.2 Case Workflow Navigation

**CaseStepper Component:**
- Arrow-based navigation UI
- 8-step workflow visualization
- Active/completed state tracking
- Responsive chevron display (narrow < 1024px)

**Step Order:**
1. Patient ID
2. Diagnosis & Staging
3. Treatment Plan
4. Compare Options
5. Resources
6. Next Steps
7. Share
8. Notes

**Navigation Features:**
- Dirty state checking before navigation
- Debounced auto-save on navigation
- Custom navigation client for MSAL

### 4.3 NavigationScroll Component

**Functionality:**
- Scrolls to top on route change
- Handles SPA navigation smoothly
- Prevents scroll restoration issues

---

## 5. State Management

### 5.1 Redux Store Configuration

**Store Structure:**
```javascript
configureStore({
  reducer: {
    form: formReducer,              // Form state
    customization: customizationReducer,  // UI customization
    diagnosis: diagnosisReducer     // Diagnosis data
  }
})
```

**Key Slices:**
1. **formSlice.js** - Form state management
2. **customizationReducer.js** - Theme, menu orientation, language
3. **diagnosisReducers.js** - Diagnosis and staging data

**Additional Slices (in slices/):**
- calendar, cart, chat, contact
- customer, kanban, mail
- product, snackbar, user

### 5.2 ApplicationContext

**Context Structure:**
```javascript
{
  appData: {
    userData: {...},           // User information
    patientInformation: {...}, // Patient data
    caseInitialDiagnosis: {}, // Diagnosis
    drawingImages: {},         // Canvas drawings
    canvasStrokes: [],         // Drawing strokes
    nodesTumors: {},          // Tumor/node annotations
    resources: {              // Resource favorites
      favorites: []
    },
    treatmentPlan: {          // Treatment planning
      optionStates: {A, B, C},
      currentOption: 'A',
      selectedOption: ''
    },
    dataLoaded: false,
    dataError: false,
    isDirty: false           // Unsaved changes flag
  },
  setAppData: function
}
```

### 5.3 Treatment Plan State

**Option State Structure:**
```javascript
freshOptionState() {
  return {
    stepsContent: Array(15).fill(null).map(() => []),
    stepsEOPID: Array(15).fill(null).map(() => []),
    stepNotes: {},
    selectedVariations: {}
  }
}
```

**Constants:**
- `TP_STEPS = 15` - Maximum treatment steps
- 3 options: A, B, C
- Support for step notes and variations

### 5.4 Dirty State Management

**Tracked Keys:**
```javascript
DIRTY_KEYS = [
  'patientInformation',
  'caseInitialDiagnosis',
  'treatmentPlan',
  'notes',
  'compareOptions',
  'drawingImages',
  'canvasStrokes',
  'nodesTumors'
]
```

**Dirty Detection:**
- Deep sorting for consistent comparison
- JSON stringify for equality check
- Debounced updates (500ms baseline)
- Global `window.__setDirty()` function

---

## 6. Case Workflow Implementation

### 6.1 Case Stepper Architecture

**CaseStepper Component Features:**
- Module ID from URL params
- Step state management
- Auto-save with debouncing
- Data loading from API
- Dirty state tracking
- Responsive navigation

**Data Loading Functions:**
- `loadPatientInformationGlobally()`
- `loadCaseDiagnosisGlobally()`
- `loadTreatmentPlanGlobally()`
- `loadResourcesGlobally()`
- `loadNextStepsGlobally()`

### 6.2 Workflow Pages

**1. PatientID.jsx**
- Patient demographics
- Physician information
- MRN, DOB, anatomic site
- PHI/Non-PHI field variations

**2. Diagnosis.jsx**
- Image gallery
- Canvas drawing (react-sketch-canvas)
- Tumor/node annotations
- AJCC staging
- Group stage selection

**3. TreatmentPlan.jsx**
- Up to 7 treatment options (A-G)
- Up to 15 steps per option
- Multi-select treatments
- Step notes (PHI only)
- Preferred option selection

**4. CompareOptions.jsx**
- Side-by-side option comparison
- Displays all steps
- Shows preferred option
- Patient read-only view

**5. Resources.jsx**
- Categorized resources
- Highlighted/favorite resources
- Search within categories
- Video playback
- Print capability

**6. NextSteps.jsx**
- Standardized categories
- Multi-select appointments
- Notes per selection (PHI)
- Scheduled treatment, tests, referrals

**7. Share.jsx**
- Email input
- Organization branding
- Save vs Share permissions
- Role-based button display

**8. Notes.jsx**
- Auto-generated summary
- Patient discussion record
- Treatment plan display
- Download as Word
- Print capability

### 6.3 Utility Functions

**Case Loading:**
- `loadDiagnosis.jsx` - Load diagnosis data
- `loadedTreatmentPlan.jsx` - Load treatment plan
- `loadNextSteps.jsx` - Load next steps
- `loadPatientInformation.jsx` - Load patient data
- `loadResources.jsx` - Load resources

**Email Services:**
- `emailService.jsx` - Provider email
- `patientemailservice.jsx` - Patient email
- `generateCaseNoteBody.jsx` - Notes generation

---

## 7. API Integration

### 7.1 Axios Configuration

**Base Setup (axios.js):**
```javascript
const axiosServices = axios.create({ 
  baseURL: azureHost 
});
```

**Request Interceptor:**
- Bearer token from `localStorage.getItem('serviceToken')`
- StaticWebAppsAuthCookie from cookies
- Accept-Language header from localStorage
- Authorization header injection

**Response Interceptor:**
- 401 redirect to /login
- Error handling and transformation
- "Wrong Services" fallback message

### 7.2 API Hosts

**Environment-based URLs:**
```javascript
{
  'mcg3xdevfd.guidingpatientssmartly.net': 'https://mcg3xdevfd...',
  'mcg3xtestfd.guidingpatientssmartly.net': 'https://mcg3xtestfd...',
  'mcg3xdemofd.guidingpatientssmartly.net': 'https://mcg3xdemofd...',
  'localhost': 'http://localhost:4280'
}
```

**Host Selection:**
- Hostname detection
- Localhost fallback for development
- Default URL for unknown hosts

### 7.3 Key API Endpoints

**User Management:**
- `GET /api/users` - All users
- `GET /api/users/me` - Current user
- Graph API: `/v1.0/me`, `/v1.0/users`

**Case Management:**
- `GET /api/getcase` - Case data
- `GET /api/geteop` - Treatment options
- `GET /api/getstaging` - Staging info
- `GET /api/getResources` - Resources

**Organization:**
- `GET /api/organizations` - Org list
- Organization-specific endpoints

### 7.4 SWR Integration

**Fetcher Function:**
```javascript
const fetcher = async (args) => {
  const [url, config] = Array.isArray(args) ? args : [args];
  const res = await axiosServices.get(url, { ...config });
  return res.data;
};
```

**Usage:**
- `swr: 2.2.5` package
- Data fetching with caching
- Automatic revalidation

---

## 8. UI Component System

### 8.1 Material-UI Theme

**Configuration (config.js):**
```javascript
{
  menuOrientation: 'vertical',
  miniDrawer: false,
  fontFamily: `'Roboto', sans-serif`,
  borderRadius: 8,
  outlinedFilled: true,
  mode: 'light',
  presetColor: 'default',
  i18n: 'en',
  themeDirection: 'ltr',
  container: false
}
```

**Theme Modes:**
- Light/Dark mode support
- LTR/RTL direction
- Custom color presets
- Customization via Redux

### 8.2 Custom Components

**UI Component Library:**
- **AgeBox.jsx** - Age display component
- **CircularLoader.jsx** - Loading spinner
- **DynamicDiagnosisFields.jsx** - Dynamic form fields
- **Locales.jsx** - Language switcher
- **Logo.jsx** - Application logo
- **UserManagement.jsx** - User admin

**Card Components (ui-component/cards/):**
- MainCard, SubCard - Base cards
- AnalyticsChartCard - Chart display
- RevenueCard, ReportCard - Data cards
- UserProfileCard, UserDetailsCard - User cards
- Skeleton components for loading states

**Extended Components (ui-component/extended/):**
- Accordion, AppBar, Avatar
- Breadcrumbs, Chip
- Snackbar, Transitions
- Form controls (FormControl, InputLabel)
- Notistack integration

### 8.3 Third-Party Integrations

**Dropzone Components:**
- Avatar, MultiFile, SingleFile
- File preview and rejection handling
- Placeholder content

**Map Components:**
- MapControl, MapMarker, MapPopup
- Styled map containers
- Control panel

**Other:**
- SyntaxHighlighter for code display
- Notistack for notifications

---

## 9. Analytics & Tracking

### 9.1 Freshpaint Integration

**analytics.ts:**
```typescript
const ANALYTICS_ON = true;

type EventName = 'page_view' | 'cta_click';

track(event: EventName, props: SafeProps)
```

**PHI Protection:**
```javascript
FORBIDDEN_KEYS = [
  'email','name','first_name','last_name','phone','mrn',
  'appointment','diagnosis','notes','visit_id','dob','ssn'
]
```

**Implementation:**
- Freshpaint script in `index.html`
- Dynamic script injection
- Environment key: `VITE_FRESHPAINT_WRITE_KEY`
- Auto page tracking
- Safe props validation

### 9.2 Tracked Events

**Page Views:**
- Automatic SPA navigation tracking
- Page path, title, referrer

**CTA Clicks:**
- Call-to-action interactions
- CTA ID tracking

**Safety:**
- Defensive PHI blocklist
- Key validation before sending
- Development/production separation

---

## 10. Internationalization

### 10.1 React Intl Implementation

**Supported Locales:**
- English (en) - Default
- Spanish (es)

**Locale Files:**
- `src/locales/en.json`
- `src/locales/es.json`

**Integration:**
```javascript
<IntlProvider locale={locale} messages={messages} defaultLocale="en">
  {children}
</IntlProvider>
```

**Storage:**
- Locale stored in `localStorage.getItem("lang")`
- Persists across sessions
- API requests include Accept-Language header

### 10.2 Locale Resources

**Image Localization:**
- `public/assets/locale/` - Locale-specific images
- 616 files (510 PNG, 106 JPG)
- Module-specific imagery

**Utility Functions:**
- `getLocaleImageUrl.js` - Get localized image
- `getImageUrl.js` - Standard image URLs

### 10.3 Table Localization

**tableLocalization.js:**
- Custom table text for MUI Data Grid
- Pagination labels
- Filter labels
- Localized column headers

---

## 11. Build & Deployment

### 11.1 Vite Configuration

**vite.config.mjs:**
```javascript
{
  plugins: [react(), jsconfigPaths()],
  build: { 
    chunkSizeWarningLimit: 1600 
  },
  server: {
    port: 3000,
    host: true,
    hmr: { overlay: true },
    watch: { usePolling: true, interval: 100 }
  }
}
```

**Features:**
- Fast HMR (Hot Module Replacement)
- Path aliasing via jsconfig
- 1600KB chunk size limit
- File watching with polling

### 11.2 Azure Static Web Apps

**Environment Configs:**
- `staticwebapp.dev.json` - Development
- `staticwebapp.test.json` - Test environment
- `staticwebapp.demo.json` - Demo environment  
- `staticwebapp.prod.json` - Production

**Test Environment:**
- URL: `mcg3xtestfd.guidingpatientssmartly.net`
- Azure Front Door ID: `df4332b5-9537-451e-9c76-8146fc1cda5e`
- Azure AD Tenant: `bd8ab10e-e46e-45dc-9881-6459ecb7d128`

### 11.3 Environment Variables

**Required Variables:**
```
VITE_CLIENT_ID          # Azure AD client ID
VITE_TENANT_ID          # Azure AD tenant ID
VITE_APP_API_URL        # Backend API URL
VITE_FRESHPAINT_WRITE_KEY  # Analytics key
```

**Usage:**
- Vite env prefix: `VITE_`
- Access via `import.meta.env.VITE_*`
- Environment-specific configuration

### 11.4 Build Scripts

**package.json scripts:**
```json
{
  "start": "vite",
  "build": "vite build",
  "preview": "vite preview",
  "server": "node server.js"
}
```

**Build Process:**
- Development: Vite dev server (port 3000)
- Production: Optimized build with code splitting
- Preview: Test production build locally

---

## 12. Data Models

### 12.1 Case Data Model

**Complete Case Structure:**
```javascript
{
  // Patient Information
  patientInformation: {
    anatomicDiseaseSite: [],
    caseId: string,
    diagnosis: [],
    mrn: string,
    pathologyType: [],
    physicianUserID: string,
    physicianNPI: string,
    physicianFullName: string,
    firstName: string,
    lastName: string,
    email: string,
    dob: string,
    age: string,
    sex: string,
    gender: string,
    race: string,
    nextStepsID: [],
    nextStepsNotes: {},
    nextStepsAppointments: {},
    datePatientInvited: Date
  },
  
  // Diagnosis Data
  caseInitialDiagnosis: {},
  drawingImages: {},
  canvasStrokes: [],
  nodesTumors: {},
  
  // Treatment Plan
  treatmentPlan: {
    optionStates: {
      A: {
        stepsContent: Array(15)[],
        stepsEOPID: Array(15)[],
        stepNotes: {},
        selectedVariations: {}
      },
      B: {...},
      C: {...}
    },
    currentOption: 'A' | 'B' | 'C',
    selectedOption: string
  },
  
  // Resources
  resources: {
    favorites: []
  },
  
  // Meta
  dataLoaded: boolean,
  dataError: boolean,
  isDirty: boolean
}
```

### 12.2 User Data Model

```javascript
userData: {
  userID: number,
  userEmail: string,
  firstName: string,
  lastName: string,
  roleID: number,
  organizationID: number,
  orgAKA: string,
  userNPI: string
}
```

### 12.3 Treatment Plan Constants

```javascript
TP_STEPS = 15                    // Max steps per option
OPTIONS = ['A', 'B', 'C']        // Treatment options
BASELINE_DEBOUNCE_MS = 500       // Auto-save debounce
```

---

## 13. Migration Recommendations

### 13.1 Critical Preservation Requirements

**MUST Preserve:**
1. **Authentication Flow**
   - Azure AD/MSAL integration
   - Role-based access control
   - Token management
   - Session handling

2. **State Management**
   - ApplicationContext structure
   - Treatment plan state (15 steps, 3 options)
   - Dirty state tracking
   - Redux store schema

3. **Case Workflow**
   - 8-step navigation
   - Data loading utilities
   - Auto-save with debouncing
   - Dirty guard protection

4. **API Integration**
   - Axios interceptors
   - Bearer token injection
   - StaticWebAppsAuthCookie handling
   - Error handling patterns

5. **Analytics & PHI Protection**
   - Freshpaint integration
   - Forbidden keys filtering
   - Safe prop validation

### 13.2 Enhancement Opportunities

**Consider for V4:**
1. **TypeScript Migration**
   - Currently partial TS implementation
   - Full type safety would improve maintainability
   - Better IDE support and error catching

2. **Component Modernization**
   - Consolidate duplicate components
   - Standardize on single form library (React Hook Form recommended)
   - Reduce third-party dependencies

3. **State Management Optimization**
   - Consider React Query/TanStack Query for API state
   - Reduce Redux boilerplate
   - Better cache management

4. **Build Optimization**
   - Code splitting improvements
   - Lazy loading optimization
   - Bundle size reduction (currently 1600KB chunks)

5. **Testing Infrastructure**
   - Add comprehensive test coverage
   - Unit tests for utilities
   - Integration tests for workflows
   - E2E testing for critical paths

### 13.3 Architectural Patterns to Maintain

**Keep These Patterns:**
1. **Provider Nesting Order:**
   - Redux → Module → Styled → Theme → MSAL → Intl → App → DirtyGuard → Nav → Routes

2. **Dirty State Management:**
   - Deep comparison with sorted JSON
   - Debounced updates
   - Multi-key tracking
   - Global setter function

3. **Lazy Loading:**
   - Route-level code splitting
   - Loadable wrapper component
   - Dynamic imports

4. **Environment Configuration:**
   - Hostname-based API selection
   - Environment variable usage
   - Multi-environment deployment configs

### 13.4 Dependencies to Review

**High-Risk Dependencies:**
- `react-sketch-canvas: 7.0.0-next.2` (pre-release version)
- Multiple form libraries (Formik + React Hook Form)
- Duplicate icon libraries (MUI + Tabler)
- Legacy Auth0 context (unused?)

**Recommended Actions:**
- Audit and remove unused dependencies
- Consolidate overlapping functionality
- Update pre-release packages
- Remove deprecated libraries

### 13.5 Migration Checklist

**Phase 1 - Foundation:**
- [ ] Set up V4 project with same tech stack
- [ ] Migrate authentication (Azure AD/MSAL)
- [ ] Implement state management structure
- [ ] Configure build system (Vite)

**Phase 2 - Core Features:**
- [ ] Migrate ApplicationContext and data models
- [ ] Implement dirty state tracking
- [ ] Port API integration layer
- [ ] Set up routing structure

**Phase 3 - Workflows:**
- [ ] Migrate case workflow components
- [ ] Implement data loading utilities
- [ ] Port auto-save functionality
- [ ] Migrate all 8 workflow steps

**Phase 4 - UI/UX:**
- [ ] Migrate custom component library
- [ ] Implement theme system
- [ ] Port internationalization
- [ ] Migrate analytics integration

**Phase 5 - Testing & Deployment:**
- [ ] Cross-reference with functional PRD
- [ ] Implement comprehensive testing
- [ ] Configure Azure deployment
- [ ] Perform UAT with stakeholders

---

## Appendix A: File Inventory

### Critical Files for Migration

**Core Application:**
- `src/App.jsx` - Root component with provider setup
- `src/index.jsx` - Entry point
- `src/config.js` - App configuration
- `src/authConfig.js` - Azure AD configuration

**State Management:**
- `src/store/store.js` - Redux store
- `src/contexts/ApplicationContext.jsx` - Global context
- `src/constants/AppConstants.js` - Data models
- `src/guards/DirtyGuardProvider.jsx` - Dirty tracking

**Case Workflow:**
- `src/views/case/index.jsx` - Case stepper
- `src/views/case/*.jsx` - All 8 workflow pages
- `src/views/case/utils/*.jsx` - Loading utilities

**API Layer:**
- `src/utils/axios.js` - HTTP client
- `src/analytics.ts` - Analytics tracking

**Build Configuration:**
- `vite.config.mjs` - Build config
- `package.json` - Dependencies
- `staticwebapp.*.json` - Deployment configs

---

## Appendix B: Dependency Matrix

### Core Dependencies (Must Migrate)

| Package | Version | Purpose | Migration Priority |
|---------|---------|---------|-------------------|
| react | 18.2.0 | Framework | Critical |
| @azure/msal-browser | 3.17.0 | Auth | Critical |
| @mui/material | 5.15.10 | UI | Critical |
| @reduxjs/toolkit | 2.2.1 | State | Critical |
| axios | 1.8.4 | HTTP | Critical |
| vite | 5.4.19 | Build | Critical |
| react-router-dom | 6.22.1 | Routing | Critical |

### Feature Dependencies (Review for V4)

| Package | Version | Purpose | Action |
|---------|---------|---------|--------|
| formik | 2.4.5 | Forms | Consolidate |
| react-hook-form | 7.50.1 | Forms | Keep |
| @tabler/icons-react | 2.47.0 | Icons | Consider removing |
| react-quill | 2.0.0 | Editor | Keep |
| three | 0.180.0 | 3D | Keep (liver demo) |
| react-sketch-canvas | 7.0.0-next.2 | Drawing | Update to stable |

---

## Document Control

**Revision History:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 14, 2025 | Thomas Teruel | Initial technical PRD from codebase analysis |

**Cross-References:**
- See `Current-State-Application-PRD.md` for functional requirements (273 requirements)
- See `Module-Specific-Requirements-PRD.md` for all 19 cancer module configurations
  - 📊 [Module Dashboard (HTML)](Module-Requirements-Dashboard.html) - Interactive view
  - 📋 [Quick Reference](Module-Requirements-Quick-Reference.md) - Fast lookup
- See `Renee demo of MCG.md` for user workflow documentation
- See `mycare-frontend-v3-reference.md` for quick reference

**Next Steps:**
1. Review technical architecture with development team
2. Create V4 migration plan based on both PRDs
3. Identify technical debt to address in V4
4. Plan TypeScript migration strategy
5. Design testing strategy for migration validation

---

**End of Technical Architecture PRD**

