# MyCaregorithm Application - Current State PRD
## Product Requirements Document - Existing Functionality

**Document Version:** 1.0  
**Last Updated:** October 14, 2025  
**Source:** Renee Application Walkthrough Transcript  
**Purpose:** Document existing application functionality to ensure complete feature parity during backend/frontend updates

---

## Table of Contents
1. [User Roles & Permissions](#user-roles--permissions)
2. [Homepage & Case Management](#homepage--case-management)
3. [Patient ID/Information Page](#patient-idinformation-page)
4. [Diagnosis & Staging Page](#diagnosis--staging-page)
5. [Treatment Plan Page](#treatment-plan-page)
6. [Resources Page](#resources-page)
7. [Next Steps Page](#next-steps-page)
8. [Compare Options Page](#compare-options-page)
9. [Notes Page](#notes-page)
10. [Share Functionality](#share-functionality)
11. [PHI vs Non-PHI Versions](#phi-vs-non-phi-versions)
12. [Patient View/Role](#patient-viewrole)
13. [Module-Specific Requirements](#module-specific-requirements)
14. [Known Issues & Bugs](#known-issues--bugs)

---

## 1. User Roles & Permissions

### 1.1 Physician Role
**Access Level:**
- Can only see their own cases
- Has access to all 19 cancer modules (see Module-Specific-Requirements-PRD.md)
- Can save AND share cases
- Full access to PHI data entry
- Auto-population of physician name and NPI fields

**Functional Requirements:**
- FR-UR-001: Physicians shall only view cases assigned to them
- FR-UR-002: Physician and NPI fields shall auto-populate for physician users
- FR-UR-003: Physicians shall have both "Save" and "Share" buttons on Share page
- FR-UR-004: System shall display physician's cases at bottom of homepage

### 1.2 Support Staff Role
**Access Level:**
- Can see ALL cases within their organization
- Has access to all 19 modules
- Currently can only save cases (not share) - **Feature request to change**
- Must select physician from dropdown (no auto-population)
- Full access to PHI data entry

**Functional Requirements:**
- FR-UR-005: Support staff shall view all cases in their organization
- FR-UR-006: Support staff shall see completely clear screen on homepage (no "My Cases" section)
- FR-UR-007: Support staff shall use search feature to find cases
- FR-UR-008: Support staff currently has "Save" only (Share restricted to physicians)
- FR-UR-009: **FUTURE:** Allow support staff to share cases on behalf of physicians

### 1.3 Patient Role
**Access Level:**
- View-only access to their own cases
- Cannot edit any information
- Can access full-screen mode
- Can print resources
- Can view highlighted resources

**Functional Requirements:**
- FR-UR-010: Patients shall have read-only access to all case data
- FR-UR-011: Patients shall access cases via email address
- FR-UR-012: Patients can view multiple cases across different physicians within same organization
- FR-UR-013: Patients cannot change organization (one patient = one organization)

### 1.4 Trainee/Scribe Role
**Access Level:**
- Can access all cases within organization
- Similar permissions to support staff

---

## 2. Homepage & Case Management

### 2.1 Case Search & Display
**Search Capabilities:**
- Search by patient name
- Search by MRN (Medical Record Number)
- Search by physician name
- Search by first name
- Filter capabilities

**Functional Requirements:**
- FR-HP-001: Homepage shall display search fields for: Patient Name, MRN, Physician Name
- FR-HP-002: Search shall work across all cases in organization (support staff) or physician's cases only
- FR-HP-003: Case grid shall display all relevant case information
- FR-HP-004: Users shall be able to reorder columns in case grid
- FR-HP-005: **ISSUE:** Column order preferences do not persist after logout
- FR-HP-006: **ENHANCEMENT:** Column preferences should persist across sessions

### 2.2 Case Grid Display
**Columns:**
- Patient information (name/MRN depending on PHI status)
- Physician name
- Module type
- Date created/modified
- Case status

**Functional Requirements:**
- FR-HP-007: Case grid shall support column reordering via drag-and-drop
- FR-HP-008: Case grid shall display appropriate fields based on PHI/Non-PHI organization
- FR-HP-009: Clicking a case shall open that case for editing/viewing

---

## 3. Patient ID/Information Page

### 3.1 PHI Version Fields
**Patient Demographics:**
- First Name (free text, alphanumeric)
- Last Name (free text, alphanumeric)
- Email (email validation)
- MRN - Medical Record Number (free text, alphanumeric)
- Specific Date of Birth (date picker)
- Sex (dropdown selection)
- Gender (dropdown selection)
- Race (dropdown selection)
- Anatomic Disease Site (dropdown, module-specific)

**Physician Information:**
- Physician Name (auto-populate for physicians, dropdown for support staff)
- Physician NPI (auto-populate for physicians, dropdown for support staff)

**Other Characteristics:**
- Free text fields (rarely used)
- Notes field (enabled in PHI version only)

**Functional Requirements:**
- FR-PI-001: System shall display all PHI fields in PHI organizations
- FR-PI-002: **BUG:** Physician and NPI fields should auto-populate for physicians but currently don't consistently
- FR-PI-003: All text fields shall accept alphanumeric input unless otherwise specified
- FR-PI-004: Email field shall validate email format
- FR-PI-005: Date of Birth shall use date picker for specific date entry
- FR-PI-006: Notes field shall be available in PHI version only
- FR-PI-007: Email can be entered on this page OR on Share page

### 3.2 Non-PHI Version Fields
**Restricted Fields:**
- NO First Name (grayed out)
- NO Last Name (grayed out)
- NO Email (grayed out)
- NO MRN (grayed out)
- NO Date of Birth
- NO Notes field

**Available Fields:**
- Age Range (dropdown instead of specific date)
- Sex (dropdown selection)
- Gender (dropdown selection)
- Race (dropdown selection)
- Anatomic Disease Site (dropdown)
- Other Characteristics (numeric only, no alphanumeric)

**Functional Requirements:**
- FR-PI-008: Non-PHI version shall gray out all PHI fields
- FR-PI-009: Age Range dropdown shall replace specific Date of Birth
- FR-PI-010: Other Characteristics shall accept numeric input only (with decimal support)
- FR-PI-011: No decimal comma allowed, only decimal point
- FR-PI-012: Notes field shall not appear in Non-PHI version

### 3.3 Module-Specific Requirements
**Myeloma Module:**
- Additional fields requested by expert physician
- More fields than standard (causing scrolling)
- Exception to "no scrolling" rule (pending Lou's review)

**Functional Requirements:**
- FR-PI-013: Myeloma module shall display extended patient information fields
- FR-PI-014: System shall support module-specific field configurations

---

## 4. Diagnosis & Staging Page

### 4.1 Full Screen Mode
**Behavior:**
- Full screen should NOT exit except when:
  - Clicking browser's exit full screen button
  - Clicking the application's full screen toggle
  - Pressing ESC key
- Should NOT exit when clicking Home or other navigation

**Functional Requirements:**
- FR-DS-001: Full screen mode shall remain active during all navigation within the module
- FR-DS-002: Full screen shall only exit via: browser exit, app toggle, or ESC key
- FR-DS-003: **FIXED:** Clicking Home button no longer exits full screen

### 4.2 Image Display & Navigation
**Default Behavior:**
- Labels should default to ON across all modules
- Default image should be Image 1 (first image) across all modules
- Image carousel should display all available images (typically 8-13 images)

**Functional Requirements:**
- FR-DS-004: Labels shall default to ON for all modules
- FR-DS-005: First image (Image 1) shall be default display across all modules
- FR-DS-006: Image carousel shall display all module images in single horizontal row
- FR-DS-007: Maximum images supported: approximately 12-13 per module
- FR-DS-008: **ISSUE:** Some modules show images stacking vertically instead of horizontal row
- FR-DS-009: Users shall be able to click through all available images

### 4.3 Tumor & Node Annotations
**Annotation Capabilities:**
- Add tumor markers
- Add node markers
- Adjust size of markers
- Position markers on images
- Annotations persist per image

**Size Requirements:**
- Default tumor/node size should be smaller than current implementation
- Default size should match previous version sizing
- Size will vary by module

**Functional Requirements:**
- FR-DS-010: System shall support adding tumor annotations to images
- FR-DS-011: System shall support adding node annotations to images
- FR-DS-012: Default annotation label should read "Tumour" not "T1"
- FR-DS-013: Default annotation for node should read "Node" (working correctly)
- FR-DS-014: Default tumor/node size should match previous version (smaller than current)
- FR-DS-015: Annotation sizes should be module-appropriate
- FR-DS-016: **BUG:** Current default sizing is too large

### 4.4 Drawings & Annotations Persistence
**Persistence Requirements:**
- Drawings should stay on the specific image where they were created
- Drawings should persist when navigating between images
- Drawings should persist when changing group stage selections

**Functional Requirements:**
- FR-DS-017: All drawings shall persist on their respective images
- FR-DS-018: Drawings shall remain visible when navigating between images
- FR-DS-019: Drawings shall remain visible when selecting different group stages
- FR-DS-020: **BUG:** Drawings currently moving between images (regression)

### 4.5 Staging Information (AJCC Box)
**Display Fields:**
- Tumor stage (T)
- Node stage (N)
- Metastasis stage (M0/M1)
- Group stage
- Additional staging information (module-specific)

**Functional Requirements:**
- FR-DS-021: AJCC box shall display all selected staging information
- FR-DS-022: Selecting any stage option shall populate in AJCC box
- FR-DS-023: Tumor and Node annotations shall display in stage section
- FR-DS-024: **OPEN QUESTION:** Should tumor/node annotations from images appear in Patient Stage section?
- FR-DS-025: M stage display based on sex selection (currently)
- FR-DS-026: **DEBATE:** M stage should be "M" regardless of sex/gender (not resolved)

### 4.6 Group Stage Selection
**Behavior:**
- Dropdown selection for group stage
- Selection should persist across image navigation
- Label should remain visible when changing images

**Functional Requirements:**
- FR-DS-027: Group stage dropdown shall offer all valid stage options
- FR-DS-028: Selected group stage shall persist when navigating images
- FR-DS-029: Group stage label shall remain visible across image changes

### 4.7 Endo/Radio Functionality
**Status:**
- Working as expected (no detailed requirements captured)

**Functional Requirements:**
- FR-DS-030: Endo/Radio functionality shall work as designed (extent already known issue)

---

## 5. Treatment Plan Page

### 5.1 Layout & Design Principles
**Design Philosophy:**
- Maximum 2 rows of 6 elements each = 12 total elements maximum
- **CRITICAL:** Avoid scrolling whenever possible
- Lou's requirement: "Physicians won't scroll - once they scroll, it gets messy"

**Functional Requirements:**
- FR-TP-001: Treatment elements shall display in 2 rows of 6 maximum
- FR-TP-002: Maximum of 12 treatment elements (lozengers/buttons) per module
- FR-TP-003: Page shall avoid requiring scrolling
- FR-TP-004: **EXCEPTION:** Myeloma module may require scrolling due to expert physician requirements

### 5.2 Treatment Options & Steps
**Structure:**
- Up to 7 treatment options (Option 1, Option 2, etc.)
- Up to 15 steps per option
- Multi-select capabilities within each step
- Cannot duplicate same treatment on same step

**Treatment Elements (Buttons/Lozengers):**
- Imaging
- Pathology Review
- Surgery
- Radiation Therapy
- Drug Therapy
- Clinical Trials
- Supportive Care
- And others (module-specific)

**Functional Requirements:**
- FR-TP-005: System shall support up to 7 treatment options
- FR-TP-006: Each option shall support up to 15 steps
- FR-TP-007: Users shall be able to select multiple treatments within one step
- FR-TP-008: System shall prevent adding duplicate treatments to same step
- FR-TP-009: Attempting to add duplicate shall display error/prevention message
- FR-TP-010: Users shall be able to add treatments to subsequent steps without restriction
- FR-TP-011: "Add Step" button shall allow creating new steps up to limit of 15

### 5.3 Notes Functionality
**PHI Version:**
- Notes box available for each step
- Can add step with only notes (no treatment selection)
- Notes persist and display in Compare Options and Notes pages
- Notes display in italics at end of step on Notes page

**Non-PHI Version:**
- NO notes box available (PHI risk)

**Functional Requirements:**
- FR-TP-012: PHI version shall display notes field for each treatment step
- FR-TP-013: Users shall be able to add notes without selecting treatments
- FR-TP-014: Blank step with only notes shall save and display properly
- FR-TP-015: Notes shall carry through to Compare Options page
- FR-TP-016: Notes shall display in italics on final Notes page
- FR-TP-017: Non-PHI version shall NOT display notes fields

### 5.4 Preference Selection
**Behavior:**
- Physician can select preferred option via checkbox
- Selection made during consultation with patient
- Patient takes information home to consider
- Preference should NOT auto-select

**Functional Requirements:**
- FR-TP-018: System shall provide checkbox to select preferred treatment option
- FR-TP-019: **BUG:** System currently auto-selects Option 1 if any step added
- FR-TP-020: Preference selection should remain blank unless explicitly selected
- FR-TP-021: Selected preference shall display on Compare Options page
- FR-TP-022: Preference should be selectable/changeable by physician only

### 5.5 Treatment Plan Display
**Sub-elements:**
- Each treatment element has sub-options
- Sub-elements display below main element
- All selections display on final Notes page

**Functional Requirements:**
- FR-TP-023: All treatment elements shall display associated sub-elements
- FR-TP-024: Sub-element selections shall persist with parent element
- FR-TP-025: **ISSUE:** Some problems with sub-element display on Notes page
- FR-TP-026: Treatment plan shall display in structured format on Notes page

---

## 6. Resources Page

### 6.1 Resource Categories
**Standard Categories:**
- Highlighted Resources (user-curated)
- General Biology
- Drug Therapy
- Radiation Therapy
- Side Effects
- Clinical Trials
- Additional categories (module-specific)

**Functional Requirements:**
- FR-RS-001: Resources page shall default to "Highlighted Resources" tab
- FR-RS-002: Each category shall display if it contains at least one resource
- FR-RS-003: Empty categories shall not display in navigation
- FR-RS-004: Categories shall be module-specific

### 6.2 Resource Types
**Static Resources (PDFs):**
- Educational materials
- Treatment information
- Printable documents

**Video Resources:**
- Educational videos
- Treatment explanation videos
- Hosted video content

**Functional Requirements:**
- FR-RS-005: Static resources shall display with Print button
- FR-RS-006: Print button shall open system print dialog
- FR-RS-007: Print dialog shall offer: Print, Save as PDF, OneNote options
- FR-RS-008: Video resources shall be playable inline
- FR-RS-009: Videos may take time to load (especially in test environment)
- FR-RS-010: Video display names and categories shall be editable via admin tool

### 6.3 Highlighting/Favoriting Resources
**Functionality:**
- Physician/care team can highlight resources
- Highlighted resources appear in "Highlighted Resources" section
- Highlights persist after save
- Highlights visible to patient

**Functional Requirements:**
- FR-RS-011: Users shall be able to highlight/favorite any resource
- FR-RS-012: Highlighted resources shall appear in "Highlighted Resources" category
- FR-RS-013: Highlights shall persist after saving case
- FR-RS-014: Highlighted resources shall be visible to patients
- FR-RS-015: Resources can be highlighted from any category
- FR-RS-016: Highlighting from Drug Therapy category shall still show in Highlighted Resources

### 6.4 Search Functionality
**Current Behavior:**
- Search works within each individual category
- Search bar clears when switching categories
- Exception: Highlighted Resources doesn't clear (different behavior)
- Cannot search across all categories simultaneously

**Functional Requirements:**
- FR-RS-017: Each category shall have its own search functionality
- FR-RS-018: Search shall filter resources within current category only
- FR-RS-019: Search shall clear when switching to different category
- FR-RS-020: **EXCEPTION:** Highlighted Resources search behaves differently
- FR-RS-021: Search example: "cisplatin" in Drug Therapy shows matching resources
- FR-RS-022: **FUTURE ENHANCEMENT:** Search across all categories (in backlog)
- FR-RS-023: **QUESTION:** Should search work in Highlighted Resources? (Low priority - expect few highlighted items)

### 6.5 Highlighted Resources Behavior
**Display:**
- Shows "Highlighted Resources" title when items exist
- Title disappears when all items removed
- Question about blank state messaging

**Functional Requirements:**
- FR-RS-024: "Highlighted Resources" section shall display title when items present
- FR-RS-025: Title shall disappear when no highlighted resources exist
- FR-RS-026: **OPEN QUESTION:** Should blank state message appear when empty?
- FR-RS-027: General Biology may show blank state (only category that could be empty while displayed)

---

## 7. Next Steps Page

### 7.1 Standardized Options
**Main Categories:**
1. Scheduled Treatment Options First
2. Additional Appointments
3. Schedule Test/Procedure
4. Referrals to Other Physicians
5. Other (catch-all)

**Each Category Has:**
- Sub-options specific to category
- "Other" option in each category for unlisted items
- Notes capability (PHI only)

**Functional Requirements:**
- FR-NS-001: Next Steps page shall display 4 main categories + Other
- FR-NS-002: Each category shall have standardized sub-options
- FR-NS-003: Each category shall include "Other" as catch-all option
- FR-NS-004: Recently updated for consistency across modules
- FR-NS-005: Lou is okay with current layout (tentative approval)

### 7.2 Multi-Select Capability
**Selection Behavior:**
- Select multiple options across different categories
- All selections save and persist
- Selections display on final Notes page

**Functional Requirements:**
- FR-NS-006: Users shall be able to select multiple sub-options across categories
- FR-NS-007: Multi-select shall work in PHI version
- FR-NS-008: All selections shall save and update properly
- FR-NS-009: Selections shall display on final Notes page

### 7.3 Notes Functionality (PHI Only)
**Use Cases:**
- Referral to specific doctor: "Dr. Smith"
- Specific instructions: "Obtain outside medical records from PCP"
- Test details: "CT imaging required"
- Appointment specifics: "Social worker - nutrition consultation"
- Dates and times

**Functional Requirements:**
- FR-NS-010: PHI version shall display notes popup for each selection
- FR-NS-011: Notes shall support free-text entry
- FR-NS-012: Notes can include: names, dates, times, specific instructions
- FR-NS-013: Non-PHI version shall NOT display notes fields at all
- FR-NS-014: Notes shall persist with associated selections
- FR-NS-015: Notes shall display on final Notes page

---

## 8. Compare Options Page

### 8.1 Display Requirements
**Content:**
- Side-by-side comparison of all treatment options
- Shows all steps for each option
- Displays preferred option (if selected)
- Shows treatment notes

**Functional Requirements:**
- FR-CO-001: Compare Options page shall display all treatment options side-by-side
- FR-CO-002: Each option shall show all steps in order
- FR-CO-003: Preferred option selection shall be visible
- FR-CO-004: Treatment notes shall display with associated steps
- FR-CO-005: **EXCEPTION:** Organization logo/message does NOT appear on this page

### 8.2 Patient View
**Interaction:**
- Patient cannot select or change anything
- Can view all options
- Can see physician's recommended option
- Read-only display

**Functional Requirements:**
- FR-CO-006: Patients shall have read-only view of Compare Options
- FR-CO-007: Physician's preference shall be visible to patient
- FR-CO-008: Patient cannot change preference selection

---

## 9. Notes Page

### 9.1 Page Structure
**Header Section:**
- Record ID (populates after save)
- "Information below was discussed with [Patient Name]" (or "patient" if no name)
- Date of discussion
- Patient demographics summary

**Content Sections:**
1. Initial Diagnosis
2. Stage
3. Treatment Plan Options
4. Resources (highlighted)
5. Next Steps
6. Standard closing statements

**Functional Requirements:**
- FR-NP-001: Record ID shall populate only after case is saved
- FR-NP-002: Header shall include patient name if available, otherwise "the patient"
- FR-NP-003: Header shall include discussion date
- FR-NP-004: Demographics summary includes: age/age range, sex, race, diagnosis, anatomic site

### 9.2 Diagnosis Section
**Content:**
- Age (specific) or age range (non-PHI)
- Sex
- Race
- Diagnosis: pathology type
- Anatomic disease site (may vary in wording)

**Wording Variations:**
- May show: "of the left tonsel"
- May show: "tonsel - left"
- May show: "of the left" (when organ assumed like breast/brain)

**Functional Requirements:**
- FR-NP-005: Diagnosis section shall auto-generate from patient ID data
- FR-NP-006: Age shall display as specific age (PHI) or range (non-PHI)
- FR-NP-007: Anatomic site wording varies by module configuration
- FR-NP-008: **KNOWN ISSUE:** Anatomic disease site wording can be "funky" - low priority fix

### 9.3 Initial Diagnosis Section
**Content:**
- Additional diagnosis fields from Diagnosis & Staging page
- Fields that didn't fit on left side (to avoid scrolling)
- Module-specific questions and answers

**Functional Requirements:**
- FR-NP-009: Initial Diagnosis shall display right-side fields from Diagnosis page
- FR-NP-010: Fields moved here to prevent scrolling on Diagnosis page
- FR-NP-011: Content is module-specific

### 9.4 Stage Section
**Content:**
- Tumor stage (T)
- Node stage (N)
- Metastasis stage (M)
- Group stage
- Gender-specific display (current implementation)

**Functional Requirements:**
- FR-NP-012: Stage section shall display all AJCC staging information
- FR-NP-013: Tumor and Node staging shall appear in stage section
- FR-NP-014: **CURRENT:** M stage display depends on sex selection
- FR-NP-015: **DISCUSSION POINT:** Should be "M" for all (not resolved)
- FR-NP-016: This wording "should never change" (referring to current format)

### 9.5 Treatment Plan Section
**Display Format:**
- All options shown
- All steps listed under each option
- Treatment elements with sub-elements
- Notes appear in italics at end of each step

**Functional Requirements:**
- FR-NP-017: Treatment plan shall show exactly as configured on Treatment Plan page
- FR-NP-018: Each option shall list all steps in order
- FR-NP-019: Sub-elements shall display after parent elements
- FR-NP-020: **ISSUE:** Some sub-element display problems exist
- FR-NP-021: Step notes shall appear in italics after step details
- FR-NP-022: Format shall match Compare Options page

### 9.6 Next Steps Section
**Display:**
- Lists all selected next steps
- Shows by category
- Includes any notes (same format as treatment plan notes)

**Functional Requirements:**
- FR-NP-023: Next Steps section shall list all selections
- FR-NP-024: Notes shall display in same format as treatment plan notes
- FR-NP-025: All categories with selections shall appear

### 9.7 Standard Footer Statements
**Content:**
- "We answered all the patient's questions to the best of our ability"
- "Visual content was made available to the patient"
- Email/Save status statement (context-dependent)
- Preferred option statement (if applicable)

**Email/Save Statement:**
- BEFORE sharing: "This information was shown to the patient on [date]"
- AFTER sharing: "This information was emailed to [patient email] on [date]"
- Non-PHI save: "This information was saved to [record] on [date]"

**Preference Statement:**
- Appears when preference selected
- "After discussion, we expressed our preference for Option [X]"
- Should display numeric option number (1, 2, 3) not letters (A, B, C)

**Functional Requirements:**
- FR-NP-026: Standard questions statement shall always appear
- FR-NP-027: Visual content statement shall always appear
- FR-NP-028: **CRITICAL:** Email statement should say "shown to patient" BEFORE sharing
- FR-NP-029: Email statement should say "emailed to [email]" AFTER sharing
- FR-NP-030: **BUG:** Currently says "emailed" before actually sharing
- FR-NP-031: Preference statement shall appear as new sentence when option selected
- FR-NP-032: **BUG:** Currently shows letters (A, B, C) instead of numbers (1, 2, 3)
- FR-NP-033: Preference bug thought to be fixed but regressed in recent push

### 9.8 Download & Print Functionality
**Capabilities:**
- Download as Word document
- Print to PDF
- Print to physical printer
- Download includes branding and logo

**Functional Requirements:**
- FR-NP-034: Notes page shall be downloadable as Word document
- FR-NP-035: Notes page shall be printable
- FR-NP-036: Downloaded/printed version shall include organization logo
- FR-NP-037: Downloaded/printed version shall include MyCaregorithm branding

---

## 10. Share Functionality

### 10.1 Share Page Elements
**Display:**
- Organization logo (customizable)
- Standard message: "Please verify the patient's email address. Thank you."
- Email input field
- Two buttons: "Save this case" and "Share this case"

**Branding:**
- Logo changes by organization
- Message can be customized per organization
- Example: OSU organization shows OSU logo
- MyCaregorithm organizations show MyCaregorithm logo

**Functional Requirements:**
- FR-SH-001: Share page shall display organization-specific logo
- FR-SH-002: Logo and message can be customized per organization
- FR-SH-003: Default message: "Please verify the patient's email address. Thank you."
- FR-SH-004: Email input field shall validate email format
- FR-SH-005: Email can be entered here OR on Patient ID page (both locations sync)

### 10.2 Save vs. Share Permissions
**Physician:**
- Can Save
- Can Share

**Support Staff (Current):**
- Can Save only
- Cannot Share

**Support Staff (Requested):**
- Should be able to Share on behalf of physician
- Physicians want support staff to handle sharing
- Don't want to log in again just to share

**Functional Requirements:**
- FR-SH-006: Physicians shall see both "Save" and "Share" buttons
- FR-SH-007: Support staff currently sees only "Save" button
- FR-SH-008: **FEATURE REQUEST:** Support staff should be able to Share cases
- FR-SH-009: Multiple customers have requested support staff share capability

### 10.3 Unsaved Changes Modal
**Behavior:**
- Modal should appear when navigating away with unsaved changes
- Should NOT appear when no changes made

**Functional Requirements:**
- FR-SH-010: **BUG:** "Unsaved changes" modal appearing when no changes made
- FR-SH-011: Modal intermittently triggers when navigating from Share to Home
- FR-SH-012: Dirty check logic needs review
- FR-SH-013: Issue difficult to reproduce consistently

### 10.4 Email Delivery
**Process:**
- Patient receives email with link to case
- Email contains patient-specific access link
- Patient clicks link to view their case
- Access tied to email address

**Functional Requirements:**
- FR-SH-014: Share action shall email case link to patient
- FR-SH-015: Email shall contain unique access link
- FR-SH-016: Link shall grant patient read-only access to case
- FR-SH-017: Patient access tied to email address used

---

## 11. PHI vs Non-PHI Versions

### 11.1 Version Purpose
**PHI Version:**
- Full featured application
- Allows Protected Health Information
- Can email to patients
- Best version of product
- Most organizations use PHI

**Non-PHI Version:**
- Restricted feature set
- NO Protected Health Information allowed
- Cannot email to patients (in-clinic use only)
- For organizations with strict security regulations
- Much more difficult to use

**Functional Requirements:**
- FR-VER-001: System shall support both PHI and Non-PHI organization types
- FR-VER-002: Version type is organization-level setting (not user toggle)
- FR-VER-003: Same environments exist for both: test, prod, etc.
- FR-VER-004: Users have same roles across both versions
- FR-VER-005: PHI version is preferred/recommended version

### 11.2 PHI Version Features
**Patient ID Page:**
- All fields available (first, last, email, MRN)
- Specific date of birth
- Alphanumeric text entry
- Notes field enabled

**Throughout Application:**
- Notes fields on Treatment Plan
- Notes fields on Next Steps
- Email sharing capability
- Full demographic data

**Functional Requirements:**
- FR-VER-006: PHI version shall allow all patient identifying information
- FR-VER-007: PHI version shall support free-text alphanumeric entry
- FR-VER-008: PHI version shall enable notes fields throughout application
- FR-VER-009: PHI version shall support email sharing to patients

### 11.3 Non-PHI Version Restrictions
**Patient ID Page:**
- First name: GRAYED OUT
- Last name: GRAYED OUT
- Email: GRAYED OUT
- MRN: GRAYED OUT
- Date of Birth: REPLACED with Age Range dropdown
- Notes: DISABLED
- Other Characteristics: NUMERIC ONLY (no alphanumeric)

**Throughout Application:**
- NO notes fields on Treatment Plan
- NO notes fields on Next Steps
- NO email sharing capability
- Case search by physician only (no patient name/MRN)

**Notes Page Differences:**
- Header: "discussed with the patient" (no name)
- Age range instead of specific age
- Footer: "This information was saved" (not "emailed")

**Functional Requirements:**
- FR-VER-010: Non-PHI version shall gray out all PHI fields on Patient ID
- FR-VER-011: Non-PHI version shall use age range instead of date of birth
- FR-VER-012: Non-PHI version shall restrict text fields to numeric only
- FR-VER-013: Non-PHI version shall disable all notes fields
- FR-VER-014: Non-PHI version shall disable email sharing
- FR-VER-015: Non-PHI version case search limited to physician name
- FR-VER-016: Non-PHI Notes page shall not include patient name
- FR-VER-017: Non-PHI Notes page footer shall say "saved" not "emailed"

### 11.4 Non-PHI Workarounds
**Recommended Practices:**
- Print Notes page for patient to take home
- Print resources for patient
- Use for anonymized pilot studies
- In-clinic review only

**Functional Requirements:**
- FR-VER-018: Non-PHI version shall support printing all pages
- FR-VER-019: Printed materials can be provided to patients physically
- FR-VER-020: Non-PHI suitable for pilot studies with anonymized data

### 11.5 Organization Assignment
**Access Control:**
- Users assigned to specific organizations
- Organization determines PHI/Non-PHI status
- Same user can have accounts in different organizations
- Cannot change organization for existing case

**Test Organizations:**
- Expert Faculty (Non-PHI prod testing)
- Ohio State Test (PHI test)
- MyCaregorithm (PHI prod)
- And others

**Functional Requirements:**
- FR-VER-021: Users shall be assigned to specific organization(s)
- FR-VER-022: Organization assignment determines PHI/Non-PHI features
- FR-VER-023: Admin can change user's organization assignment
- FR-VER-024: Same email can exist in different organizations (separate accounts)
- FR-VER-025: Cases belong to one organization (cannot transfer)

---

## 12. Patient View/Role

### 12.1 Access & Authentication
**Access Method:**
- Receive email with case link
- Click link to access case
- Access tied to email address
- Can view multiple cases if shared

**Case Organization:**
- Homepage shows "Cases" (not "Home")
- Lists all cases shared with that email
- Cases can be from different physicians within same organization
- Cannot access cases from different organizations

**Functional Requirements:**
- FR-PV-001: Patients shall access cases via emailed link
- FR-PV-002: Patient access shall be tied to email address
- FR-PV-003: Patient homepage shall display all cases for that email
- FR-PV-004: Multiple cases from different physicians shall display (same org only)
- FR-PV-005: **LIMITATION:** One patient email = one organization only
- FR-PV-006: **FUTURE CONSIDERATION:** Allow same patient across different organizations

### 12.2 Read-Only Restrictions
**Cannot Edit:**
- Cannot change any patient information
- Cannot modify diagnosis or staging
- Cannot add/remove tumor or node annotations
- Cannot change treatment plan
- Cannot select preferences
- Cannot highlight resources (can view highlighted)
- Cannot modify next steps

**Can View:**
- All patient information
- All images and annotations
- All staging information
- All treatment options
- All highlighted resources
- Next steps

**Functional Requirements:**
- FR-PV-007: Patients shall have READ-ONLY access to all case data
- FR-PV-008: Patients cannot edit any fields
- FR-PV-009: Patients cannot modify annotations or drawings
- FR-PV-010: Patients cannot change treatment preferences
- FR-PV-011: Patients shall view all information as configured by care team

### 12.3 Patient ID Page (Patient View)
**Display:**
- All fields visible but not editable
- Shows MRN (if available)
- Shows all demographics

**Functional Requirements:**
- FR-PV-012: Patient shall view all Patient ID information
- FR-PV-013: All fields shall be read-only (not grayed out, just uneditable)
- FR-PV-014: **ISSUE:** MRN sometimes missing from patient view (need to verify)

### 12.4 Diagnosis & Staging (Patient View)
**Display Issues:**
- Images should load properly
- Tumor and node annotations should be visible
- Should be able to view in full screen
- Should be able to navigate between images

**Known Issues:**
- Blank canvas on initial load
- Annotations may be missing
- Image navigation problems when not in full screen

**Functional Requirements:**
- FR-PV-015: Patients shall view all images in Diagnosis & Staging
- FR-PV-016: Patients shall see all tumor and node annotations
- FR-PV-017: Patients shall access full screen mode
- FR-PV-018: **BUG:** Canvas appears blank until image navigation
- FR-PV-019: **BUG:** Tumor/node annotations missing (may be regression from updates)
- FR-PV-020: **BUG:** Cannot navigate images when not in full screen
- FR-PV-021: AJCC staging box shall display all staging information
- FR-PV-022: Initial diagnosis fields shall be visible

### 12.5 Treatment Plan (Patient View)
**Display:**
- All treatment options visible
- All steps visible
- No editing buttons (Add Step, etc. removed)
- Can see all dropdowns (read-only)
- Can see all notes
- Blank options shouldn't display

**Functional Requirements:**
- FR-PV-023: Patients shall view all treatment options and steps
- FR-PV-024: Edit controls shall be hidden in patient view
- FR-PV-025: Dropdown content shall be visible but not editable
- FR-PV-026: All notes shall be visible to patient
- FR-PV-027: **BUG:** Empty Option 4 appearing when it shouldn't

### 12.6 Compare Options (Patient View)
**Display:**
- Can see all options side-by-side
- Can see physician's recommended option (checked box)
- Read-only view

**Functional Requirements:**
- FR-PV-028: Patients shall view Compare Options page
- FR-PV-029: Physician's preferred option shall be visible
- FR-PV-030: Cannot change preference selection

### 12.7 Resources (Patient View)
**Capabilities:**
- View all highlighted resources
- Search within each category
- Open and view static resources
- Print resources for themselves
- Play video resources
- Cannot highlight additional resources

**Functional Requirements:**
- FR-PV-031: Patients shall view all highlighted resources
- FR-PV-032: Patients shall search within each resource category
- FR-PV-033: Patients shall open and view static resources (PDFs)
- FR-PV-034: Patients shall print resources
- FR-PV-035: Patients shall play video resources
- FR-PV-036: Videos may load slowly (acceptable behavior)
- FR-PV-037: Patients cannot add new highlighted resources

### 12.8 Next Steps (Patient View)
**Display:**
- Can view all selected next steps
- Cannot click or modify selections
- Read-only view

**Functional Requirements:**
- FR-PV-038: Patients shall view all next steps
- FR-PV-039: Patients cannot modify next step selections
- FR-PV-040: Next steps display in read-only format

### 12.9 Notes Page (Patient View)
**Access:**
- Full access to view Notes page
- Can see all information formatted for sharing
- Can print for themselves

**Functional Requirements:**
- FR-PV-041: Patients shall access and view Notes page
- FR-PV-042: Notes page shall display all case information
- FR-PV-043: Patients shall be able to print Notes page

---

## 13. Module-Specific Requirements

### 13.1 Myeloma Module
**Partner Branding:**
- LLS (Leukemia & Lymphoma Society) logo - now "Blood Cancers United"
- Both logos display together in header
- MyCaregorithm logo + LLS logo side-by-side
- Branding requirement due to partnership

**Patient ID Page:**
- Extended fields requested by expert physician
- More fields than standard modules
- Requires scrolling (exception to no-scroll rule)
- Lou hasn't reviewed this scrolling yet
- Visible in test environment

**Functional Requirements:**
- FR-MOD-001: Myeloma module shall display LLS/Blood Cancers United logo
- FR-MOD-002: Both organization logos shall appear side-by-side in header
- FR-MOD-003: Partnership branding shall persist across all organizations using Myeloma
- FR-MOD-004: Myeloma Patient ID page shall include extended field set
- FR-MOD-005: Myeloma Patient ID may require scrolling (pending final review)

### 13.2 Image Count by Module
**Standard:**
- Most modules: 8-13 images
- Varies by body part/disease site

**Layout:**
- Should display in single horizontal row
- All images accessible via carousel

**Functional Requirements:**
- FR-MOD-006: Image count shall be configurable per module
- FR-MOD-007: Standard range: 8-13 images per module
- FR-MOD-008: Image carousel shall accommodate module-specific image counts

### 13.3 Module Count
**Current State:**
- 19 modules currently in production
- All modules follow same basic structure
- Module-specific variations in fields and options

**Functional Requirements:**
- FR-MOD-009: System supports 19 distinct cancer modules
- FR-MOD-010: Each module follows standardized page flow
- FR-MOD-011: Modules can have unique field configurations

---

## 14. Known Issues & Bugs

### 14.1 Critical Bugs (Need Immediate Attention)

**BUG-001: Physician/NPI Auto-Population**
- **Location:** Patient ID page
- **Issue:** Physician and NPI fields not auto-populating for physician users
- **Expected:** Should auto-fill for physicians, dropdown for support staff
- **Current:** Not working consistently
- **Priority:** HIGH

**BUG-002: Tumor Default Label**
- **Location:** Diagnosis & Staging page
- **Issue:** Tumor annotation defaults to "T1" instead of "Tumour"
- **Expected:** Should read "Tumour" as default label
- **Current:** Shows "T1"
- **Priority:** HIGH

**BUG-003: Drawings Moving Between Images**
- **Location:** Diagnosis & Staging page
- **Issue:** Drawings/annotations moving from original image to other images
- **Expected:** Drawings should stay on specific image where created
- **Current:** Moving between images
- **Priority:** CRITICAL
- **Note:** This is a regression

**BUG-004: Default Tumor/Node Size Too Large**
- **Location:** Diagnosis & Staging page
- **Issue:** Default sizing of tumors and nodes too large
- **Expected:** Should match previous version sizing (smaller)
- **Current:** Significantly larger than before
- **Priority:** MEDIUM

**BUG-005: Treatment Plan Preference Auto-Select**
- **Location:** Treatment Plan page
- **Issue:** System auto-selecting Option 1 when any step added
- **Expected:** Should remain blank unless explicitly selected
- **Current:** Defaults to Option 1
- **Priority:** HIGH

**BUG-006: Preference Display Shows Letters Instead of Numbers**
- **Location:** Notes page
- **Issue:** Preference shows as "Option A, B, C" instead of "Option 1, 2, 3"
- **Expected:** Should display numeric options (1, 2, 3)
- **Current:** Shows letters (A, B, C)
- **Priority:** MEDIUM
- **Note:** Thought to be fixed but regressed in recent push

**BUG-007: Notes Page Email Statement Premature**
- **Location:** Notes page footer
- **Issue:** Says "information was emailed" before case is actually shared
- **Expected:** Should say "shown to patient on [date]" until shared, then "emailed to [email] on [date]"
- **Current:** Says "emailed" prematurely
- **Priority:** HIGH
- **Note:** Will has ticket for this

**BUG-008: Unsaved Changes Modal False Positive**
- **Location:** Share page navigation
- **Issue:** "Unsaved changes" modal appearing when no changes were made
- **Expected:** Should only appear when changes exist
- **Current:** Appearing intermittently without changes
- **Priority:** MEDIUM
- **Note:** Difficult to reproduce consistently

**BUG-009: Non-PHI Patient ID Fields Not Grayed**
- **Location:** Patient ID page (Non-PHI version)
- **Issue:** PHI fields appear editable instead of grayed out
- **Expected:** First name, last name, email, MRN, notes should be grayed out
- **Current:** Appear active but are not typeable
- **Priority:** HIGH

**BUG-010: Images Not Displaying in Single Row**
- **Location:** Diagnosis & Staging page (Myeloma and others)
- **Issue:** Images stacking vertically instead of single horizontal row
- **Expected:** All images in one horizontal line
- **Current:** Vertical stacking/wrapping
- **Priority:** MEDIUM

**BUG-011: Patient View - Missing Annotations**
- **Location:** Diagnosis & Staging (Patient role)
- **Issue:** Tumor and node annotations not visible to patients
- **Expected:** All annotations should be visible
- **Current:** Missing from patient view
- **Priority:** CRITICAL
- **Note:** May have been deleted during recent updates

**BUG-012: Patient View - Blank Canvas**
- **Location:** Diagnosis & Staging (Patient role)
- **Issue:** Canvas blank on initial load
- **Expected:** Should display image immediately
- **Current:** Remains blank until navigation
- **Priority:** HIGH

**BUG-013: Patient View - Image Navigation Broken**
- **Location:** Diagnosis & Staging (Patient role)
- **Issue:** Cannot navigate through images when not in full screen
- **Expected:** Should be able to click through images
- **Current:** Navigation non-functional outside full screen
- **Priority:** HIGH

**BUG-014: Empty Treatment Option Displaying**
- **Location:** Treatment Plan (Patient view)
- **Issue:** Option 4 showing when it's empty/blank
- **Expected:** Empty options should not display
- **Current:** Blank Option 4 visible
- **Priority:** LOW

**BUG-015: Treatment Plan Sub-Elements Display Issues**
- **Location:** Notes page
- **Issue:** Sub-elements not displaying properly below parent elements
- **Expected:** Sub-elements should appear correctly formatted
- **Current:** Display problems (specifics not detailed)
- **Priority:** MEDIUM

**BUG-016: Patient MRN Missing in Patient View**
- **Location:** Patient ID page (Patient role)
- **Issue:** Patient MRN sometimes not displaying in patient view
- **Expected:** Should show MRN if it was entered
- **Current:** Missing (needs verification)
- **Priority:** MEDIUM
- **Note:** Uncertain if actual bug

### 14.2 Enhancement Requests

**ENH-001: Support Staff Share Permission**
- **Request:** Allow support staff to share cases on behalf of physicians
- **Rationale:** Multiple customers requested; physicians want staff to handle sharing
- **Current:** Support staff can only save, not share
- **Priority:** HIGH
- **Status:** Feature request pending

**ENH-002: Column Preferences Persistence**
- **Request:** Save user's column order preferences across sessions
- **Rationale:** Users reorder columns but settings reset on logout
- **Current:** Column order resets each session
- **Priority:** MEDIUM
- **Status:** Enhancement request

**ENH-003: Cross-Category Resource Search**
- **Request:** Search across all resource categories simultaneously
- **Rationale:** More efficient than searching each category individually
- **Current:** Search limited to current category only
- **Priority:** LOW
- **Status:** In backlog

**ENH-004: Highlighted Resources Search**
- **Request:** Add search capability to Highlighted Resources section
- **Rationale:** Consistency with other resource categories
- **Current:** No search in Highlighted Resources
- **Priority:** LOW
- **Status:** Uncertain - expect few highlighted items anyway

**ENH-005: Multi-Organization Patient Support**
- **Request:** Allow same patient email to access cases from multiple organizations
- **Rationale:** Patients may receive care from different organizations
- **Current:** One patient email = one organization only
- **Priority:** MEDIUM
- **Status:** Exploring options ("can of worms")

**ENH-006: M Stage Display Simplification**
- **Request:** Display "M" for metastasis stage regardless of sex/gender
- **Rationale:** Simplification; avoid gender-based logic
- **Current:** M stage display depends on sex selection
- **Priority:** LOW
- **Status:** Not resolved; political/medical debate

**ENH-007: Anatomic Disease Site Wording Standardization**
- **Request:** Standardize how anatomic sites display in sentences on Notes page
- **Rationale:** Current wording can be "funky" ("tonsel-left" vs "of the left tonsel")
- **Current:** Inconsistent wording across modules
- **Priority:** LOW
- **Status:** Further down priority list

### 14.3 Extent/Known Issue (No Details)
- **Location:** Diagnosis & Staging - Endo/Radio
- **Status:** Already being tracked
- **Priority:** Unknown

---

## 15. General UX Principles

### 15.1 No Scrolling Mandate
**Lou's Rule:** "Physicians won't scroll - once they have to scroll, it gets messy and they won't do it"

**Application:**
- Treatment Plan: Maximum 12 elements (2 rows of 6)
- Diagnosis & Staging: All images in one row
- Patient ID: All fields visible without scrolling (except Myeloma exception)

**Functional Requirements:**
- FR-UX-001: Pages shall be designed to avoid vertical scrolling
- FR-UX-002: Maximum 12 treatment elements to prevent scrolling
- FR-UX-003: Exception: Myeloma module Patient ID page
- FR-UX-004: Exception: Next Steps page (Lou tentatively okay with current layout)

### 15.2 User Interface Standards
**Logo Display:**
- MyCaregorithm logo always present in header
- Organization-specific logos can co-exist
- Branding consistent across all pages (except Compare Options)

**Navigation:**
- Tab-based navigation through workflow
- Home button returns to case grid
- Full screen accessible from Diagnosis & Staging

**Functional Requirements:**
- FR-UX-005: MyCaregorithm logo shall appear on all pages
- FR-UX-006: Organization logos shall be configurable
- FR-UX-007: Tab navigation shall follow standard workflow order
- FR-UX-008: Home button shall return to case grid/homepage

### 15.3 Data Persistence
**Save Behavior:**
- Save button commits all changes
- Record ID generated upon first save
- All changes persist across sessions
- Refreshing page should retain saved data

**Functional Requirements:**
- FR-UX-009: Save shall commit all page changes
- FR-UX-010: Record ID generated on first save
- FR-UX-011: Saved data shall persist across sessions
- FR-UX-012: Unsaved changes shall trigger warning modal on navigation

---

## 16. Testing & Quality Assurance

### 16.1 Test Environments
**Available Environments:**
- Test (test environment)
- Prod (production environment)

**Organization Types in Each:**
- PHI organizations
- Non-PHI organizations
- Pilot study organizations

**Functional Requirements:**
- FR-QA-001: All features shall be testable in test environment before prod
- FR-QA-002: Test and prod shall have separate organizations for testing
- FR-QA-003: Both PHI and Non-PHI organizations shall exist in each environment

### 16.2 Cross-Role Testing Needs
**Required Test Accounts:**
- Physician account
- Support staff account
- Patient account (different email)
- Trainee/Scribe account
- Accounts in both PHI and Non-PHI organizations

**Functional Requirements:**
- FR-QA-004: QA shall have test accounts for all roles
- FR-QA-005: QA shall have access to both PHI and Non-PHI test organizations
- FR-QA-006: Patient testing shall use separate email from staff accounts

### 16.3 Browser Testing
**Requirements:**
- Full screen functionality
- Print functionality
- Download functionality
- Video playback

**Functional Requirements:**
- FR-QA-007: All features shall be tested in primary browsers
- FR-QA-008: Full screen shall work across browsers
- FR-QA-009: Print/download shall work across browsers

---

## Appendix A: Quick Reference - Page Flow

**Standard Workflow:**
1. Homepage/Case Grid → Search or select case
2. Patient ID/Information → Demographics and basic info
3. Diagnosis & Staging → Images, annotations, staging
4. Treatment Plan → Options and steps
5. Compare Options → Side-by-side view
6. Resources → Educational materials
7. Next Steps → Follow-up actions
8. Notes → Final summary document
9. Share → Email to patient or save

---

## Appendix B: Quick Reference - Role Permissions Matrix

| Feature | Physician | Support Staff | Patient | Trainee/Scribe |
|---------|-----------|---------------|---------|----------------|
| View own cases | ✓ | ✓ | ✓ | ✓ |
| View all org cases | ✗ | ✓ | ✗ | ✓ |
| Edit cases | ✓ | ✓ | ✗ | ✓ |
| Save cases | ✓ | ✓ | ✗ | ✓ |
| Share cases | ✓ | ✗ (requested ✓) | ✗ | ✗ |
| Auto-populate MD info | ✓ | ✗ | N/A | ✗ |
| Access PHI fields | ✓ | ✓ | ✓ (view only) | ✓ |
| Print resources | ✓ | ✓ | ✓ | ✓ |

---

## Appendix C: Quick Reference - PHI vs Non-PHI Features

| Feature | PHI Version | Non-PHI Version |
|---------|-------------|-----------------|
| Patient name | ✓ Full name | ✗ Grayed out |
| Patient email | ✓ | ✗ Grayed out |
| MRN | ✓ | ✗ Grayed out |
| Date of birth | ✓ Specific date | Age range only |
| Notes fields | ✓ All pages | ✗ No notes anywhere |
| Text entry | ✓ Alphanumeric | Numeric only |
| Email sharing | ✓ | ✗ |
| Case search | All fields | Physician only |
| Print capability | ✓ | ✓ |

---

## Appendix D: Transcript Source Reference

**Source File:** `Renee demo of MCG.md`
**Participants:**
- Speaker 1: Samantha
- Speaker 2: Renee (primary demonstrator)
- Speaker 3: Thomas (documenter)

**Key Moments with "Capture That" Annotations:**
- Line 31: Physician/NPI auto-population issue
- Line 49: Full screen fix status
- Line 55: Default image requirements
- Line 79: Drawing persistence bug
- Line 91: Tumor/node stage display question
- Line 106: No scrolling requirement
- Line 112: Myeloma module scrolling exception
- Line 133: Treatment plan duplicate prevention
- Line 139: Step/option limits
- Line 145: Notes following to compare options
- Line 205: Moving to next steps
- Line 223: Search functionality
- Line 289: Share page logo/branding
- Line 301: Support staff share request
- Line 361: Notes page email statement logic
- Line 397: LLS logo partnership requirement
- And more throughout document...

---

## Document Control

**Revision History:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 14, 2025 | Thomas Teruel | Initial PRD creation from transcript |

**Approval Status:** DRAFT - Pending Review

**Related Documents:**
- `V3-Technical-Architecture-PRD.md` - Complete technical architecture documentation
- `Module-Specific-Requirements-PRD.md` - All 19 cancer module configurations and unique requirements
  - 📊 [Module Dashboard (HTML)](Module-Requirements-Dashboard.html) - Interactive view
  - 📋 [Quick Reference](Module-Requirements-Quick-Reference.md) - Fast lookup
- `Renee demo of MCG.md` - Source transcript

**Next Steps:**
1. Review PRD with development team
2. Cross-reference with existing tickets in Jira
3. Prioritize bug fixes vs. enhancements
4. Validate requirements with Renee and Lou
5. Create test plans for each functional requirement
6. Update PRD based on stakeholder feedback

---

**End of Document**

