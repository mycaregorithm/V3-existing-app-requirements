# MyCaregorithm - Module-Specific Requirements PRD
## Product Requirements Document - Cancer Module Configurations

**Document Version:** 1.0  
**Last Updated:** October 14, 2025  
**Source:** V3 Codebase Analysis  
**Purpose:** Document unique requirements and configurations for each of the 19 cancer modules

---

> 💡 **Quick Access:**  
> 📊 [Interactive Dashboard (HTML)](Module-Requirements-Dashboard.html) - Best for browsing and filtering  
> 📋 [Quick Reference Guide](Module-Requirements-Quick-Reference.md) - Best for fast lookup

---

## Table of Contents

1. [Overview](#overview)
2. [Module Architecture](#module-architecture)
3. [Module-Specific Field System](#module-specific-field-system)
4. [Module Directory](#module-directory)
5. [Special Module Configurations](#special-module-configurations)
6. [API Integration Per Module](#api-integration-per-module)
7. [Image Requirements](#image-requirements)
8. [Data Model Requirements](#data-model-requirements)

---

## 1. Overview

MyCaregorithm currently supports **19 distinct cancer modules**, each with unique:
- Patient ID fields (anatomic sites, pathology types)
- Diagnosis & staging requirements
- Treatment options (Elements of Plan - EOPs)
- Educational resources
- Image sets

### 1.1 Complete Module List

| Module ID | Module Name | Key |
|-----------|-------------|-----|
| 1 | Anal | `anal` |
| 2 | Bladder | `bladder` |
| 3 | Brain | `brain` |
| 4 | Breast | `breast` |
| 5 | Cervix/Uterus | `cervix` |
| 6 | Colon | `colon` |
| 7 | Esophagus/Stomach | `stomach` |
| 8 | Kidney | `kidney` |
| 9 | Lung | `lung` |
| 10 | Melanoma | `melanoma` |
| 11 | Oropharynx/Nasopharynx | `nasopharynx` |
| 12 | Ovarian | `ovarian` |
| 13 | Pancreas | `pancreas` |
| 14 | Prostate | `prostate` |
| 15 | Rectal | `rectal` |
| 16 | Sarcoma | `sarcoma` |
| 17 | Thyroid | `thyroid` |
| 18 | Larynx/Oral Cavity | `larynx` |
| 19 | Myeloma | `myeloma` |

---

## 2. Module Architecture

### 2.1 Dynamic Field Loading

All module-specific fields are loaded dynamically from the backend via API calls:

```javascript
// Patient ID Page - Module-specific data fetching
await Promise.all([
  axiosServices.get('/api/getPathology', { params: { moduleID } }),
  axiosServices.get('/api/getanatomicsites', { params: { moduleID } }),
  axiosServices.get('/api/getCaseInitialDiagnosis', { params: { moduleID } })
]);

// Diagnosis & Staging - Module-specific staging
await axiosServices.post('/api/getstaging', { moduleID });

// Treatment Plan - Module-specific treatment options
await axiosServices.post('/api/geteop', { moduleID });
```

### 2.2 Module-Based Routing

**URL Pattern:** `/case/{moduleID}?caseId={caseID}`

Examples:
- Brain Cancer: `/case/3?caseId=12345`
- Prostate Cancer: `/case/14?caseId=67890`

---

## 3. Module-Specific Field System

### 3.1 Common Fields (All Modules)

**Patient Information (Standard):**
- MRN (Medical Record Number)
- First Name / Last Name
- Email
- Date of Birth / Age Range
- Sex
- Gender
- Race
- ECOG Performance Status
- Physician Name / NPI

**These fields are consistent across all 19 modules.**

### 3.2 Module-Specific Fields

#### 3.2.1 Pathology Types
**API:** `GET /api/getPathology?moduleID={id}`

**Response Structure:**
```javascript
[
  {
    PathologyID: number,
    PathologyType: string,
    ModuleID: number
  }
]
```

**Purpose:** Defines available pathology types for each cancer module
- Brain: Glioblastoma, Astrocytoma, Oligodendroglioma, etc.
- Breast: Invasive Ductal Carcinoma, Invasive Lobular Carcinoma, etc.
- Lung: Adenocarcinoma, Squamous Cell, Small Cell, etc.

#### 3.2.2 Anatomic Disease Sites
**API:** `GET /api/getanatomicsites?moduleID={id}`

**Response Structure:**
```javascript
[
  {
    AnatomicSiteID: number,
    AnatomicSiteT: string,      // Primary site
    SubSiteT: string,            // Sub-site (optional)
    ModuleID: number
  }
]
```

**Hierarchical Structure:**
- **Primary Sites:** Grouped categories (e.g., "Left Lung", "Right Lung")
- **Sub-Sites:** Specific locations within primary site (e.g., "Upper Lobe", "Lower Lobe")

**UI Rendering:**
- Grouped dropdown by primary site
- Sub-site selection within each group
- Multi-select capability

#### 3.2.3 Initial Diagnosis Fields
**API:** `GET /api/getCaseInitialDiagnosis?moduleID={id}`

**Response Structure:**
```javascript
[
  {
    InitialDiagnosisID: number,
    FieldNameT: string,          // Field label (e.g., "Histology", "Gleason")
    value: string,               // Option value
    valueT: string,              // Translated value
    displayInPatientID: boolean  // Show on Patient ID page vs Diagnosis page
  }
]
```

**Dynamic Field Rendering:**
- Fields grouped by `FieldNameT`
- Multi-select dropdowns for most fields
- Special handling for "Other" fields (text input)
- PHI/Non-PHI field restrictions

#### 3.2.4 Staging Systems
**API:** `POST /api/getstaging` with `{ moduleID }`

**Response Structure:**
```javascript
[
  {
    StagingSystemID: number,
    ValueType: string,  // "T", "N", "M", or custom (e.g., "Group Stage")
    Value: string,      // Stage value (e.g., "T1", "N0", "Stage IIA")
    ModuleID: number
  }
]
```

**Staging Categories:**
- **TNM Staging:** T (Tumor), N (Node), M (Metastasis)
- **Custom Staging:** Module-specific (e.g., Gleason Score for Prostate, Clark Level for Melanoma)

#### 3.2.5 Treatment Options (EOPs)
**API:** `POST /api/geteop` with `{ moduleID }`

**Response Structure:**
```javascript
[
  {
    ElementOfPlanID: number,
    ElementOfPlanT: string,       // Treatment name
    ElementOfPlanCategoryT: string, // Category (Drug, Radiation, Surgery, etc.)
    ModuleID: number,
    variations: string[]          // Treatment variations/sub-options
  }
]
```

**Treatment Categories:**
1. Drug Therapy
2. Radiation Therapy
3. Surgery
4. Re-evaluation
5. Imaging
6. Pathology Review
7. Clinical Trial
8. Active Surveillance
9. Lab Work
10. Supportive Care
11. Follow-up

---

## 4. Module Directory

### 4.1 Module 1: Anal Cancer

**Pathology Types:**
- Squamous Cell Carcinoma
- Adenocarcinoma
- Others

**Key Features:**
- Male/Female image variations
- 6 anatomic images (front wide, front mid, left/right mid, left/right close)
- Right-side diagnosis display
- TNM staging

**Image Count:** 6 images × 2 genders = 12 total images

---

### 4.2 Module 2: Bladder Cancer

**Pathology Types:**
- Urothelial Carcinoma
- Squamous Cell Carcinoma
- Adenocarcinoma
- Others

**Key Features:**
- Male/Female image variations
- 6 anatomic images
- Combined Bladder/Kidney visualization
- TNM staging

**Image Count:** 6 images × 2 genders = 12 total images

---

### 4.3 Module 3: Brain Cancer

**Pathology Types:**
- Glioblastoma
- Astrocytoma
- Oligodendroglioma
- Meningioma
- Others

**Key Features:**
- 12 brain anatomy images (different views/cross-sections)
- No gender differentiation
- WHO grading system
- Specialized molecular markers (IDH, MGMT)

**Image Count:** 12 images

---

### 4.4 Module 4: Breast Cancer

**Pathology Types:**
- Invasive Ductal Carcinoma (IDC)
- Invasive Lobular Carcinoma (ILC)
- Ductal Carcinoma In Situ (DCIS)
- Others

**Key Features:**
- 3 anatomic images (front, left, right)
- Right-side diagnosis display
- Hormone receptor status (ER, PR, HER2)
- Ki-67 proliferation index
- Oncotype DX score
- TNM staging

**Image Count:** 3 images

---

### 4.5 Module 5: Cervix/Uterus Cancer

**Pathology Types:**
- Squamous Cell Carcinoma
- Adenocarcinoma
- Adenosquamous
- Others

**Key Features:**
- Female-only anatomy
- Multiple anatomic views
- FIGO staging system
- HPV status

**Image Count:** 18+ images

---

### 4.6 Module 6: Colon Cancer

**Pathology Types:**
- Adenocarcinoma
- Mucinous Adenocarcinoma
- Signet Ring Cell
- Others

**Key Features:**
- Male/Female image variations
- Colon anatomy (labeled/unlabeled)
- Microsatellite instability (MSI) status
- TNM staging
- Tumor location (ascending, transverse, descending, sigmoid)

**Image Count:** 2 images × 2 genders = 4 total images

---

### 4.7 Module 7: Esophagus/Stomach Cancer

**Pathology Types:**
- Adenocarcinoma
- Squamous Cell Carcinoma
- GIST (Gastrointestinal Stromal Tumor)
- Others

**Key Features:**
- Combined esophagus/stomach visualization
- Multiple anatomic views
- Siewert classification (for GE junction)
- TNM staging

**Image Count:** 13+ images

---

### 4.8 Module 8: Kidney Cancer

**Pathology Types:**
- Renal Cell Carcinoma (Clear Cell)
- Papillary RCC
- Chromophobe RCC
- Oncocytoma
- Others

**Key Features:**
- Male/Female variations
- Combined bladder/kidney views
- Multiple cross-sections
- TNM staging

**Image Count:** 18+ images

---

### 4.9 Module 9: Lung Cancer

**Pathology Types:**
- Adenocarcinoma
- Squamous Cell Carcinoma
- Small Cell Lung Cancer (SCLC)
- Large Cell Carcinoma
- Others

**Key Features:**
- 12 anatomic images
- Left/right lung differentiation
- Lobe-specific locations (upper, middle, lower)
- Molecular markers (EGFR, ALK, ROS1, PD-L1)
- TNM staging

**Image Count:** 12 images

---

### 4.10 Module 10: Melanoma

**Pathology Types:**
- Superficial Spreading Melanoma
- Nodular Melanoma
- Lentigo Maligna Melanoma
- Acral Lentiginous Melanoma
- Others

**Key Features:**
- Right-side diagnosis display
- Full body mapping (multiple views)
- Breslow depth
- Clark level
- Ulceration status
- AJCC staging

**Image Count:** 16+ images (body mapping)

---

### 4.11 Module 11: Oropharynx/Nasopharynx Cancer

**Pathology Types:**
- Squamous Cell Carcinoma
- Adenocarcinoma
- Others

**Key Features:**
- Head and neck anatomy
- Multiple anatomic views
- HPV status (important for oropharynx)
- TNM staging

**Image Count:** 8+ images

---

### 4.12 Module 12: Ovarian Cancer

**Pathology Types:**
- High-Grade Serous Carcinoma
- Low-Grade Serous Carcinoma
- Endometrioid
- Clear Cell
- Mucinous
- Others

**Key Features:**
- Female-only anatomy
- Pelvic anatomic views
- CA-125 levels
- BRCA mutation status
- FIGO staging

**Image Count:** 12+ images

---

### 4.13 Module 13: Pancreas Cancer

**Pathology Types:**
- Ductal Adenocarcinoma
- Neuroendocrine Tumor
- Others

**Key Features:**
- Pancreas anatomy (head, body, tail)
- CA 19-9 levels
- Bilirubin levels
- Resectability status
- TNM staging

**Image Count:** Variable

---

### 4.14 Module 14: Prostate Cancer

**Pathology Types:**
- Adenocarcinoma
- Others

**Key Features:**
- **SPECIAL:** Gleason Score fields (Primary, Secondary, Sampled, Positive)
- Right-side diagnosis display
- PSA levels
- Number of cores sampled/positive
- TNM staging
- Risk stratification (D'Amico)

**Unique Field Configuration:**
```javascript
// Module 14 specific: Gleason scoring layout
if (parseInt(moduleID) === 14) {
  // Primary Gleason field
  // Secondary Gleason field
  // Cores Sampled field
  // Cores Positive field
}
```

**Image Count:** 12 images

---

### 4.15 Module 15: Rectal Cancer

**Pathology Types:**
- Adenocarcinoma
- Mucinous Adenocarcinoma
- Others

**Key Features:**
- Right-side diagnosis display
- Distance from anal verge
- Mesorectal fascia involvement
- TNM staging
- Neoadjuvant therapy response

**Image Count:** 25+ images

---

### 4.16 Module 16: Sarcoma

**Pathology Types:**
- Liposarcoma
- Leiomyosarcoma
- Undifferentiated Pleomorphic Sarcoma
- Synovial Sarcoma
- Others

**Key Features:**
- Right-side diagnosis display
- Full body mapping (extremity, trunk, retroperitoneal)
- Tumor grade (FNCLCC)
- Tumor size
- TNM staging for soft tissue sarcoma

**Image Count:** 21+ images

---

### 4.17 Module 17: Thyroid Cancer

**Pathology Types:**
- Papillary Thyroid Carcinoma
- Follicular Thyroid Carcinoma
- Medullary Thyroid Carcinoma
- Anaplastic Thyroid Carcinoma
- Others

**Key Features:**
- Neck anatomy
- Multiple views
- BRAF mutation status
- Thyroglobulin levels
- TNM staging

**Image Count:** 13+ images

---

### 4.18 Module 18: Larynx/Oral Cavity Cancer

**Pathology Types:**
- Squamous Cell Carcinoma
- Others

**Key Features:**
- Head and neck anatomy
- Larynx-specific views
- Oral cavity subsites
- HPV status
- TNM staging

**Image Count:** 10+ images

---

### 4.19 Module 19: Myeloma (Multiple Myeloma)

**Pathology Types:**
- IgG Myeloma
- IgA Myeloma
- Light Chain Myeloma
- Others

**Key Features:**
- **SPECIAL:** Partnership with LLS (Leukemia & Lymphoma Society) - now Blood Cancers United
- Dual logo display (MyCaregorithm + LLS)
- Extended patient information fields (exception to no-scrolling rule)
- M-protein levels
- Free light chain ratio
- Cytogenetics
- ISS staging (International Staging System)
- R-ISS staging

**Special Branding Requirement:**
```javascript
// Myeloma module MUST display both logos
<Logo src={myCaregorithmLogo} />
<Logo src={llsLogo} /> // Blood Cancers United
```

**Image Count:** 22 images

---

## 5. Special Module Configurations

### 5.1 Right-Side Diagnosis Display

**Modules with Right-Side Diagnosis Fields:**
```javascript
const MODULES_WITH_RIGHT_SIDE_DIAGNOSIS = [1, 4, 10, 14, 15, 16];
```

These modules display additional diagnosis fields on the right side of the Diagnosis & Staging page instead of at the bottom.

**Affected Modules:**
- Module 1: Anal
- Module 4: Breast
- Module 10: Melanoma
- Module 14: Prostate
- Module 15: Rectal
- Module 16: Sarcoma

### 5.2 Prostate-Specific (Module 14)

**Gleason Score Configuration:**

The Prostate module has unique field labels and layout:

| Field Name | Display Label | Layout |
|------------|---------------|--------|
| Primary | "Gleason" header | Left column |
| Secondary | (blank header) | Right column |
| Sampled | "Cores" header | Left column |
| Positive | (blank header) | Right column |

**Implementation:**
```javascript
if (parseInt(moduleID) === 14) {
  // Special 2-column layout for Gleason scoring
  // Primary + Secondary Gleason scores
  // Sampled cores + Positive cores
}
```

### 5.3 Myeloma-Specific (Module 19)

**Partnership Branding:**
- Requires LLS (Blood Cancers United) logo alongside MyCaregorithm logo
- Applies to ALL organizations using Myeloma module
- Logo must persist across all pages of the workflow

**Extended Fields:**
- More patient information fields than standard modules
- Only module with scrolling on Patient ID page (pending Lou's review)
- Visible in test environment

**Special Requirements:**
- ISS Staging system
- Cytogenetic risk stratification
- Laboratory values (M-protein, free light chains)

### 5.4 Gender-Specific Images

**Modules with Male/Female Variations:**
- Module 1: Anal (6 images × 2 = 12)
- Module 2: Bladder (6 images × 2 = 12)
- Module 6: Colon (2 images × 2 = 4)
- Module 7: Esophagus/Stomach (varies)

**Female-Only Modules:**
- Module 5: Cervix/Uterus
- Module 12: Ovarian

**Gender-Neutral Modules:**
- Module 3: Brain
- Module 9: Lung
- Module 17: Thyroid
- Others

---

## 6. API Integration Per Module

### 6.1 Required API Calls for Each Module

**Initial Load Sequence:**
```javascript
// 1. Get module metadata
GET /api/getmodules

// 2. Load patient-specific fields (moduleID required)
GET /api/getPathology?moduleID={id}
GET /api/getanatomicsites?moduleID={id}
GET /api/getCaseInitialDiagnosis?moduleID={id}

// 3. Load staging systems (moduleID required)
POST /api/getstaging { moduleID }

// 4. Load treatment options (moduleID required)
POST /api/geteop { moduleID }

// 5. Load resources (moduleID required)
GET /api/getResources?moduleID={id}
```

### 6.2 Case-Specific Data Loading

**When opening existing case:**
```javascript
// Get case details
GET /api/getCase?caseID={id}

// Get case-specific selections
GET /api/getCasePathology?caseID={id}
GET /api/getCaseAnatomicSite?caseID={id}
GET /api/getCaseDiagnosis?caseID={id}
GET /api/getCaseStaging?caseID={id}
GET /api/getTumorsNodes?caseID={id}
```

### 6.3 Module Validation

**Organization Module Access:**
```javascript
GET /api/getOrgModulesForUser
```

Returns array of moduleIDs that the user's organization has access to.

**Access Control:**
- If user tries to access unpurchased module → Show modal: "This module has not yet been purchased"
- Redirect to contact MyCaregorithm for purchase

---

## 7. Image Requirements

### 7.1 Image Folder Structure

```
src/assets/images/modules/
├── anal/
│   ├── 1_Female_frontwide.png
│   ├── 1_Female_frontwide_labels.png
│   ├── 1_Male_frontwide.png
│   ├── 1_Male_frontwide_labels.png
│   ├── ... (6 views × 2 genders × 2 versions)
│   └── icons/ (12 SVG icons)
├── bladder/
├── brain/
├── breast/
├── cervix/
├── colon/
├── esophagus/
├── kidney/
├── larynx-oral/
├── lung/
├── melanoma/
├── myeloma/
├── oropharynx/
├── ovarian/
├── pancreas/
├── prostate/
├── rectal/
├── sarcoma/
└── thyroid/
```

### 7.2 Image Naming Conventions

**Standard Format:**
- `{number}_{Gender}_{view}.png` - Unlabeled anatomy
- `{number}_{Gender}_{view}_labels.png` - Labeled anatomy
- `{modulename}_anatomy_{number}.jpg` - Alternative format

**Label Toggle:**
- Default: Labels ON
- User can toggle labels on/off
- Label state persists across image navigation

### 7.3 Image Count by Module

| Module | Total Images | Notes |
|--------|--------------|-------|
| Anal | 12 | 6 views × 2 genders |
| Bladder | 12 | 6 views × 2 genders |
| Brain | 12 | Cross-sections |
| Breast | 3 | Front, left, right |
| Cervix/Uterus | 18+ | Multiple pelvic views |
| Colon | 4 | 2 views × 2 genders |
| Esophagus/Stomach | 13+ | GI tract views |
| Kidney | 18+ | Multiple cross-sections |
| Larynx/Oral | 10+ | Head/neck views |
| Lung | 12 | Thorax views |
| Melanoma | 16+ | Body mapping |
| Myeloma | 22 | Skeletal system |
| Oropharynx/Nasopharynx | 8+ | Head/neck views |
| Ovarian | 12+ | Pelvic views |
| Pancreas | Variable | Abdominal views |
| Prostate | 12 | Pelvic views |
| Rectal | 25+ | Rectum/pelvis views |
| Sarcoma | 21+ | Body mapping |
| Thyroid | 13+ | Neck views |

### 7.4 Locale-Specific Images

**Image Localization Path:**
```
public/assets/locale/
├── {moduleFolder}/
│   └── {locale}/
│       └── {imageName}.png
```

**Supported Locales:**
- English (en)
- Spanish (es)

**Image Loading Function:**
```javascript
getLocaleImageUrl('modules/{module}/{imageName}', lang)
```

---

## 8. Data Model Requirements

### 8.1 Patient Information Structure

```javascript
patientInformation: {
  // Standard fields (all modules)
  caseId: string,
  mrn: string,
  firstName: string,
  lastName: string,
  email: string,
  dob: string,              // ISO date
  age: number,
  ageRange: string,         // For non-PHI
  sex: string,
  gender: string,
  race: string,
  ecog: number,             // 0-5
  
  // Physician info
  physicianUserID: number,
  physicianNPI: string,
  physicianFullName: string,
  
  // Module-specific arrays
  pathologyType: number[],        // PathologyIDs
  anatomicDiseaseSite: number[],  // AnatomicSiteIDs
  diagnosis: [                     // Initial diagnosis fields
    {
      id: number,                  // InitialDiagnosisID
      field: string,               // FieldNameT
      note: string                 // For "Other" or special fields
    }
  ],
  
  // Meta
  notes: string,
  datePatientInvited: string,    // ISO datetime
  PatientID: number
}
```

### 8.2 Diagnosis Structure

```javascript
caseInitialDiagnosis: {
  [FieldNameT]: [
    {
      id: number,              // InitialDiagnosisID
      value: string,
      valueT: string,          // Translated
      displayInPatientID: boolean,
      field: string            // FieldNameT
    }
  ]
}
```

### 8.3 Staging Structure

```javascript
// Redux store
diagnosis: {
  characterization: string,    // First staging type value
  characterizationID: number,  // First staging type ID
  characterization2: string,   // Second staging type value
  characterizationID2: number, // Second staging type ID
  age: number
}

// Application context
{
  nodesTumors: {
    [imageNumber]: string      // SVG path data for annotations
  },
  lastTumorStage: string,      // Most recent tumor stage
  lastNodeStage: string        // Most recent node stage
}
```

### 8.4 Treatment Plan Structure

```javascript
treatmentPlan: {
  optionStates: {
    A: {
      stepsContent: Array(15)[],      // Array of arrays
      stepsEOPID: Array(15)[],        // Element of Plan IDs
      stepNotes: {},                  // Notes per step
      selectedVariations: {}          // Sub-variations selected
    },
    B: { /* same structure */ },
    C: { /* same structure */ }
  },
  currentOption: 'A' | 'B' | 'C',
  selectedOption: string              // Preferred option
}
```

---

## 9. V4 Migration Requirements

### 9.1 Module Data Preservation

**CRITICAL: Each module MUST maintain:**

1. **Existing Pathology Types**
   - All current pathology options
   - Exact naming/spelling
   - IDs must remain consistent

2. **Existing Anatomic Sites**
   - All current anatomic locations
   - Hierarchical structure (site/subsite)
   - IDs must remain consistent

3. **Existing Diagnosis Fields**
   - All custom fields per module
   - Field names and labels
   - "Other" field configurations

4. **Existing Staging Systems**
   - TNM staging where applicable
   - Module-specific staging (Gleason, FIGO, ISS, etc.)
   - All stage values and IDs

5. **Existing Treatment Options**
   - All EOPs (Elements of Plan)
   - Treatment categories
   - Sub-variations

6. **Image Sets**
   - All current images
   - Labeled/unlabeled versions
   - Male/female variations
   - Locale-specific versions

### 9.2 Module-Specific Testing Requirements

**For Each Module, Test:**
- [ ] All pathology types load correctly
- [ ] All anatomic sites display hierarchically
- [ ] All diagnosis fields render properly
- [ ] Staging systems work correctly
- [ ] Treatment options load with correct variations
- [ ] Images display with correct labels
- [ ] Gender-specific images (if applicable)
- [ ] Right-side diagnosis display (modules 1, 4, 10, 14, 15, 16)
- [ ] Special configurations (Prostate Gleason, Myeloma branding)

### 9.3 API Contract Validation

**Backend APIs Must Return:**

```typescript
// Example: GET /api/getPathology?moduleID=14
interface PathologyResponse {
  PathologyID: number;
  PathologyType: string;
  PathologyTypeT?: string;  // Translated
  ModuleID: number;
}

// Example: GET /api/getanatomicsites?moduleID=9
interface AnatomicSiteResponse {
  AnatomicSiteID: number;
  AnatomicSiteT: string;
  SubSiteT: string | null;
  ModuleID: number;
}

// Example: GET /api/getCaseInitialDiagnosis?moduleID=4
interface InitialDiagnosisResponse {
  InitialDiagnosisID: number;
  FieldNameT: string;
  value: string;
  valueT?: string;
  displayInPatientID: boolean;
  ModuleID: number;
}
```

---

## 10. Cross-Reference

### 10.1 Related Documents
- **Current-State-Application-PRD.md** - Functional requirements
- **V3-Technical-Architecture-PRD.md** - Technical architecture
- **Renee demo of MCG.md** - User workflow walkthrough

### 10.2 Key Code References

**Module Configuration:**
- `src/mocks.js` - Module metadata and images
- `src/ui-component/DynamicDiagnosisFields.jsx` - Dynamic field rendering
- `src/views/case/PatientID.jsx` - Module-specific field loading

**Special Configurations:**
- Line 39 in `PatientID.jsx`: `MODULES_WITH_RIGHT_SIDE_DIAGNOSIS = [1, 4, 10, 14, 15, 16]`
- Lines 168-173 in `DynamicDiagnosisFields.jsx`: Prostate Gleason layout
- Module 19 (Myeloma): Partnership branding requirements

---

## Appendix A: Module Quick Reference

| Module | Special Features | Image Count | Gender Specific | Right-Side Diagnosis |
|--------|------------------|-------------|-----------------|----------------------|
| 1 - Anal | - | 12 | Yes | ✓ |
| 2 - Bladder | - | 12 | Yes | - |
| 3 - Brain | WHO grading | 12 | No | - |
| 4 - Breast | Hormone receptors | 3 | No | ✓ |
| 5 - Cervix/Uterus | FIGO staging | 18+ | Female only | - |
| 6 - Colon | MSI status | 4 | Yes | - |
| 7 - Esophagus/Stomach | Siewert class | 13+ | No | - |
| 8 - Kidney | - | 18+ | Yes | - |
| 9 - Lung | Molecular markers | 12 | No | - |
| 10 - Melanoma | Breslow/Clark | 16+ | No | ✓ |
| 11 - Oropharynx/Nasopharynx | HPV status | 8+ | No | - |
| 12 - Ovarian | BRCA status | 12+ | Female only | - |
| 13 - Pancreas | CA 19-9, Bili | Variable | No | - |
| 14 - Prostate | **Gleason score** | 12 | No | ✓ |
| 15 - Rectal | Distance from AV | 25+ | No | ✓ |
| 16 - Sarcoma | FNCLCC grading | 21+ | No | ✓ |
| 17 - Thyroid | BRAF status | 13+ | No | - |
| 18 - Larynx/Oral | HPV status | 10+ | No | - |
| 19 - Myeloma | **LLS branding, ISS** | 22 | No | - |

---

## Appendix B: API Endpoints Summary

| Endpoint | Method | Purpose | Module-Specific |
|----------|--------|---------|-----------------|
| `/api/getmodules` | GET | List all modules | No |
| `/api/getPathology` | GET | Get pathology types | Yes (moduleID) |
| `/api/getanatomicsites` | GET | Get anatomic sites | Yes (moduleID) |
| `/api/getCaseInitialDiagnosis` | GET | Get diagnosis fields | Yes (moduleID) |
| `/api/getstaging` | POST | Get staging systems | Yes (moduleID) |
| `/api/geteop` | POST | Get treatment options | Yes (moduleID) |
| `/api/getResources` | GET | Get resources | Yes (moduleID) |
| `/api/getOrgModulesForUser` | GET | Get user's module access | No |

---

**Document Status:** COMPLETE  
**Next Steps:**
1. Validate all module configurations in test environment
2. Create module-by-module migration checklist
3. Build automated tests for module-specific field loading
4. Verify backend API contracts for all modules

---

**End of Module-Specific Requirements PRD**

