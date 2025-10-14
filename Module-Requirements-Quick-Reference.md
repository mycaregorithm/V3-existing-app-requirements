# Module Requirements - Quick Reference Guide
**Companion to:** [Module-Specific-Requirements-PRD.md](Module-Specific-Requirements-PRD.md)

> 📋 This is a quick reference guide for MyCaregorithm's 19 cancer modules. For detailed requirements, see the [full Module-Specific-Requirements-PRD](Module-Specific-Requirements-PRD.md).

---

## Table of Contents
- [All 19 Modules at a Glance](#all-19-modules-at-a-glance)
- [Module Groupings](#module-groupings)
- [Special Configurations](#special-configurations)
- [API Quick Reference](#api-quick-reference)
- [Image Assets Summary](#image-assets-summary)
- [Field Types by Module](#field-types-by-module)

---

## All 19 Modules at a Glance

| # | Module | Images | Gender | Special Features | Staging |
|---|--------|--------|--------|------------------|---------|
| 1 | **Anal** | 12 | M/F | Right-side diagnosis | TNM |
| 2 | **Bladder** | 12 | M/F | Bladder/Kidney combined | TNM |
| 3 | **Brain** | 12 | Neutral | WHO grading, IDH/MGMT | WHO |
| 4 | **Breast** | 3 | Neutral | Right-side, ER/PR/HER2, Oncotype | TNM |
| 5 | **Cervix/Uterus** | 18+ | Female | HPV status | FIGO |
| 6 | **Colon** | 4 | M/F | MSI status, location | TNM |
| 7 | **Esophagus/Stomach** | 13+ | Neutral | Siewert classification | TNM |
| 8 | **Kidney** | 18+ | M/F | RCC subtypes | TNM |
| 9 | **Lung** | 12 | Neutral | EGFR/ALK/ROS1/PD-L1 | TNM |
| 10 | **Melanoma** | 16+ | Neutral | Right-side, Breslow/Clark | AJCC |
| 11 | **Oropharynx/Nasopharynx** | 8+ | Neutral | HPV status | TNM |
| 12 | **Ovarian** | 12+ | Female | CA-125, BRCA | FIGO |
| 13 | **Pancreas** | Var. | Neutral | CA 19-9, Bilirubin | TNM |
| 14 | **Prostate** | 12 | Neutral | ⭐ Gleason scoring, PSA, Right-side | TNM |
| 15 | **Rectal** | 25+ | Neutral | Right-side, distance from AV | TNM |
| 16 | **Sarcoma** | 21+ | Neutral | Right-side, FNCLCC grade | TNM |
| 17 | **Thyroid** | 13+ | Neutral | BRAF, Thyroglobulin | TNM |
| 18 | **Larynx/Oral Cavity** | 10+ | Neutral | HPV status | TNM |
| 19 | **Myeloma** | 22 | Neutral | ⭐ LLS branding, M-protein | ISS/R-ISS |

**Legend:**
- M/F = Male/Female image variations
- Neutral = Gender-neutral images
- Female = Female-only anatomy
- ⭐ = Special configuration required

---

## Module Groupings

### By Gender Specificity

**Male/Female Variations (4 modules):**
```
Module 1:  Anal               (6 views × 2 = 12 images)
Module 2:  Bladder            (6 views × 2 = 12 images)  
Module 6:  Colon              (2 views × 2 = 4 images)
Module 8:  Kidney             (varies)
```

**Female-Only (2 modules):**
```
Module 5:  Cervix/Uterus
Module 12: Ovarian
```

**Gender-Neutral (13 modules):**
```
All remaining modules (3, 4, 7, 9, 10, 11, 13-19)
```

### By Image Count

**High Image Count (>15 images):**
- Module 5: Cervix/Uterus (18+)
- Module 8: Kidney (18+)
- Module 10: Melanoma (16+ body mapping)
- Module 15: Rectal (25+)
- Module 16: Sarcoma (21+)
- Module 19: Myeloma (22)

**Standard Count (10-13 images):**
- Modules 1, 2, 3, 7, 9, 11, 12, 14, 17, 18

**Low Image Count (<10 images):**
- Module 4: Breast (3)
- Module 6: Colon (4)

---

## Special Configurations

### Right-Side Diagnosis Display
**6 modules with special layout:**

| Module | Name | Why Special |
|--------|------|-------------|
| 1 | Anal | Additional diagnosis fields |
| 4 | Breast | Hormone receptors, Oncotype |
| 10 | Melanoma | Breslow depth, Clark level |
| 14 | Prostate | **Gleason scoring** |
| 15 | Rectal | Distance from anal verge |
| 16 | Sarcoma | FNCLCC grading |

**Code Reference:**
```javascript
const MODULES_WITH_RIGHT_SIDE_DIAGNOSIS = [1, 4, 10, 14, 15, 16];
```

### Module 14: Prostate - Gleason Scoring

**Special 2×2 Layout:**
```
┌─────────────┬─────────────┐
│ Primary     │ Secondary   │
│ (Gleason)   │ (Gleason)   │
├─────────────┼─────────────┤
│ Sampled     │ Positive    │
│ (Cores)     │ (Cores)     │
└─────────────┴─────────────┘
```

**Fields:**
- Primary Gleason (numeric)
- Secondary Gleason (numeric)
- Cores Sampled (numeric)
- Cores Positive (numeric)

### Module 19: Myeloma - Partnership Branding

**Special Requirements:**
- ✅ Dual logo display: MyCaregorithm + LLS (Blood Cancers United)
- ✅ Extended patient information fields
- ✅ Exception to "no scrolling" rule on Patient ID page
- ✅ ISS and R-ISS staging systems

**Logo Requirement:**
```
All pages must show: [MCG Logo] [LLS Logo]
```

---

## API Quick Reference

### Module-Specific APIs (Requires moduleID)

| API Endpoint | Method | Returns | Purpose |
|--------------|--------|---------|---------|
| `/api/getPathology` | GET | Pathology types | Cancer subtypes for module |
| `/api/getanatomicsites` | GET | Anatomic sites | Body locations (hierarchical) |
| `/api/getCaseInitialDiagnosis` | GET | Diagnosis fields | Dynamic form fields |
| `/api/getstaging` | POST | Staging systems | TNM, FIGO, ISS, etc. |
| `/api/geteop` | POST | Treatment options | Elements of Plan (EOPs) |
| `/api/getResources` | GET | Educational resources | PDFs, videos |

### Example API Call Sequence

```javascript
// 1. User selects module (e.g., Lung Cancer = moduleID: 9)
navigate('/case/9')

// 2. Load module-specific data
const [pathology, sites, diagnosis, staging, treatments] = await Promise.all([
  GET /api/getPathology?moduleID=9,
  GET /api/getanatomicsites?moduleID=9,
  GET /api/getCaseInitialDiagnosis?moduleID=9,
  POST /api/getstaging { moduleID: 9 },
  POST /api/geteop { moduleID: 9 }
])

// 3. Render dynamic fields based on responses
```

---

## Image Assets Summary

### Total Image Inventory

| Module | Count | Type | Notes |
|--------|-------|------|-------|
| Anal | 12 | M/F | 6 views × 2 genders |
| Bladder | 12 | M/F | 6 views × 2 genders |
| Brain | 12 | Static | Cross-sections |
| Breast | 3 | Static | Front, left, right |
| Cervix/Uterus | 18+ | Female | Pelvic views |
| Colon | 4 | M/F | 2 views × 2 genders |
| Esophagus/Stomach | 13+ | Static | GI tract |
| Kidney | 18+ | M/F | Cross-sections |
| Larynx/Oral | 10+ | Static | Head/neck |
| Lung | 12 | Static | Thorax |
| Melanoma | 16+ | Static | Body mapping |
| Myeloma | 22 | Static | Skeletal |
| Oropharynx/Nasopharynx | 8+ | Static | Head/neck |
| Ovarian | 12+ | Female | Pelvic |
| Pancreas | Variable | Static | Abdominal |
| Prostate | 12 | Static | Pelvic |
| Rectal | 25+ | Static | Rectum/pelvis |
| Sarcoma | 21+ | Static | Body mapping |
| Thyroid | 13+ | Static | Neck |

### Image Naming Convention

```
Labeled:   {number}_{Gender}_{view}_labels.png
Unlabeled: {number}_{Gender}_{view}.png
Alt:       {module}_anatomy_{number}.jpg
```

**Label Toggle:**
- Default state: Labels ON
- User can toggle labels on/off per image
- State persists during navigation

---

## Field Types by Module

### Common Fields (All Modules)

✅ **Standard Patient Information:**
- MRN, First/Last Name, Email
- Date of Birth / Age Range
- Sex, Gender, Race
- ECOG Performance Status (0-5)
- Physician Name / NPI

### Module-Specific Fields Overview

| Module | Unique Fields | Example Values |
|--------|---------------|----------------|
| 3 - Brain | Molecular markers | IDH mutation, MGMT methylation |
| 4 - Breast | Receptors, genomics | ER+, PR+, HER2+, Oncotype DX score |
| 6 - Colon | Molecular testing | MSI-High, MSI-Low, MSS |
| 9 - Lung | Biomarkers | EGFR, ALK, ROS1, PD-L1 % |
| 12 - Ovarian | Tumor markers, genetics | CA-125, BRCA1/2 |
| 13 - Pancreas | Lab values | CA 19-9, Bilirubin |
| 14 - Prostate | Gleason, PSA | Primary/Secondary Gleason, PSA level |
| 17 - Thyroid | Genetics, markers | BRAF V600E, Thyroglobulin |
| 19 - Myeloma | Proteins, cytogenetics | M-protein, Free light chains, ISS |

---

## Staging Systems by Module

### TNM Staging (12 modules)
Modules: 1, 2, 4, 6, 7, 8, 9, 11, 13, 14, 15, 16, 17, 18

**Components:**
- T = Tumor size/extent
- N = Node involvement
- M = Metastasis

### FIGO Staging (2 modules)
- Module 5: Cervix/Uterus
- Module 12: Ovarian

### AJCC Staging (1 module)
- Module 10: Melanoma (with Breslow/Clark)

### ISS/R-ISS Staging (1 module)
- Module 19: Myeloma

### WHO Grading (1 module)
- Module 3: Brain (Grade I-IV)

---

## Treatment Categories (All Modules)

**Standard Treatment Options (EOPs):**
1. Drug Therapy (chemo, immunotherapy, targeted)
2. Radiation Therapy (EBRT, SBRT, proton, etc.)
3. Surgery (varies by module)
4. Re-evaluation (tumor board)
5. Imaging (CT, MRI, PET-CT, ultrasound)
6. Pathology Review
7. Clinical Trial
8. Active Surveillance
9. Lab Work
10. Supportive Care
11. Follow-up

**Module-Specific Treatment Variations:**
- Loaded dynamically via `/api/geteop` with moduleID
- Each module has unique sub-variations
- Example: Lung module may have specific targeted therapies (Osimertinib for EGFR+)

---

## Migration Checklist (V4)

### Per-Module Validation

For **each of 19 modules**, verify:

- [ ] Pathology types match exactly
- [ ] Anatomic site hierarchy preserved  
- [ ] All diagnosis fields render correctly
- [ ] Staging system functional
- [ ] Treatment options load with variations
- [ ] Images display (labeled/unlabeled)
- [ ] Gender-specific images (if applicable)
- [ ] Special configurations work:
  - [ ] Right-side diagnosis (modules 1, 4, 10, 14, 15, 16)
  - [ ] Gleason scoring (module 14)
  - [ ] LLS branding (module 19)

### API Contract Testing

Validate all 6 module-specific APIs return correct structure:

```typescript
✓ GET  /api/getPathology?moduleID={id}
✓ GET  /api/getanatomicsites?moduleID={id}  
✓ GET  /api/getCaseInitialDiagnosis?moduleID={id}
✓ POST /api/getstaging { moduleID }
✓ POST /api/geteop { moduleID }
✓ GET  /api/getResources?moduleID={id}
```

---

## Related Documentation

- 📄 [**Module-Specific-Requirements-PRD.md**](Module-Specific-Requirements-PRD.md) - Complete detailed requirements
- 📄 [Current-State-Application-PRD.md](Current-State-Application-PRD.md) - Functional requirements (273 items)
- 📄 [V3-Technical-Architecture-PRD.md](V3-Technical-Architecture-PRD.md) - Technical architecture
- 🌐 [Module-Requirements-Dashboard.html](Module-Requirements-Dashboard.html) - Interactive HTML view

---

**Last Updated:** October 14, 2025  
**Status:** Active Reference Document  
**Maintained By:** Product/Engineering Team

