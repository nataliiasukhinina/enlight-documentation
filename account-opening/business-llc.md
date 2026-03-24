# Business Account Opening User Flow Documentation: LLC (Limited Liability Company)

This document describes the step-by-step user flow when users select the "Open Account" → "Business" → any state → "Limited Liability Company" (or "Single Member LLC") ownership type on the landing page.

---

## Flow Overview

The Business Account Opening flow for LLC (Limited Liability Company) allows users to open a new business bank account for an LLC entity. This flow is more comprehensive than other business ownership types because it includes additional steps for ownership structure review, business resolution documentation, and regulatory compliance (beneficial owners and persons with control).

**Note:** The LLC flow includes additional steps compared to Sole Proprietorship: Review Ownership, Business Resolution, and Business Summary pages.

---

## Step 1: Landing Page - Account Opening Setup

**Route:** `/` (root)

**Component:** `LandingPageComponent`

### Purpose
The landing page serves as the entry point where users configure the basic parameters for the new business account.

### User Interface
- Welcome message: "Welcome to Your Spend Life Wisely Journey"
- A card with sequential form options

### User Actions
1. User selects task toggle: **"OPEN AN ACCOUNT"**
2. User selects account type toggle: **"Business"**
3. User selects community bank location: **"Texas"** or **"Oklahoma"** (with state map icons)
4. User selects ownership type from dropdown: **"Limited Liability Company"** or **"Single Member LLC"**
5. User clicks **"Continue"** button

### What Happens on Continue
- A fullscreen business search dialog opens

---

## Step 2: Business Search Dialog

**Component:** `BusinessManagementDialogComponent` (fullscreen dialog)

### Purpose
A fullscreen dialog that allows users to search for an existing business or create a new one for the account.

### User Interface
- **Title:** "Search for an Existing Business"
- **Search Component:** A search table for finding businesses
- **Action Button:** "CREATE NEW BUSINESS"

### User Actions
1. User can search for an existing business by:
   - Business Name
   - CIF (Customer Information File)
   - Tax ID (EIN)

2. User views search results in a table

3. User has two options:
   - **Select an existing business** from search results and click **"Continue"**
   - **Click "CREATE NEW BUSINESS"** to enter a new business's information

### If Creating New Business:
- Dialog transitions to a CIF Edit Business form
- User enters business information:
  - Legal Business Name
  - DBA (Doing Business As)
  - Tax ID (EIN)
  - Business Address
  - Business Type (auto-set to LLC)
  - Industry/NAICS Code
  - Date Business Established
- User clicks **"Continue"** to save and proceed

### What Happens on Continue
- System creates an engagement and selects/creates the business profile
- Dialog closes and user navigates to the Business Information page

---

## Step 3: Business Information Page

**Route:** `/new-business-account/business-information`

**Component:** `BusinessInformationPageComponent`

### Purpose
Displays and allows editing of the business information for the account.

### User Interface
- **Page Title:** "Let's get to know your business"
- **Stepper:** Shows 8 steps (for LLC) - "Customer Information" is highlighted (Step 1)
- **Business Card:** Displays business information with edit capability

### Stepper Steps for LLC:
1. Customer Information
2. Ownership & Resolution
3. Business Summary
4. Account Details
5. Additional Services
6. Review
7. Disclose & Sign
8. Complete

### User Actions
1. User can view and verify business information
2. User can edit business details if needed
3. User clicks **"Continue"** when business information is complete

### Navigation
- **"GO BACK"** button - Returns to the search dialog
- **"Continue"** button - Proceeds to Business Information Validations
- **"CANCEL APPLICATION"** button - Cancels the application (with confirmation)

---

## Step 4: Business Information Validations Page

**Route:** `/new-business-account/business-information-validations`

**Component:** `BusinessInformationValidationsComponent`

### Purpose
Validates the business information through FIS checks (BizChex, OFAC) and requires completion of Customer Due Diligence (CDD) questionnaire.

### User Interface
- **Page Title:** "Validating business information..."
- **Business Name Header:** Shows the legal business name
- **Validation Cards:** Two validation status cards:
  1. **BIZCHEX** - Business credit/risk check
  2. **OFAC** - Office of Foreign Assets Control sanctions check

- **CDD Section:**
  - Link to complete Customer Due Diligence questionnaire
  - Checkbox to confirm CDD completion

- **VIEW FIS REPORT** button (appears when validations complete)

### When Validation is NOT Shown for a Business

The business validation is **skipped** if the business already has deposit accounts at the bank (`hasDepositAccounts` is `true`).

### User Actions
1. User waits for validations to complete (loading state)
2. User clicks the CDD questionnaire link to complete due diligence
3. User checks the confirmation checkbox after completing CDD
4. User can view the FIS report once validations complete
5. User clicks **"Continue"** when validations pass and CDD is confirmed

### Navigation
- **"GO BACK"** button - Returns to Business Information page
- **"Continue"** button - Proceeds to Customer Information page

---

## Step 5: Customer Information Page

**Route:** `/new-business-account/customer-information`

**Component:** `CustomerInformationPageComponent`

### Purpose
Displays and manages all members/managers and signers associated with the LLC account.

### User Interface
- **Page Title:** "Let's get to know you."
- **Stepper:** Step 1 - "Customer Information" is highlighted
- **Customer Cards:** Lists all LLC managers and signers
- **Add Customer Actions:** Options to add members/managers and signers

### Required Customers for LLC:
- **LLC Manager(s)** (at least one required) - Members or managers of the LLC

### Optional Customers:
- **Additional LLC Managers** - More members/managers
- **Convenience Signers** - Additional authorized signers on the account

### User Actions
1. User can view the LLC managers information
2. User can add additional LLC managers
3. User can add convenience signers
4. User can edit customer information
5. User clicks **"Continue"** when all required customers are added

### Navigation
- **"GO BACK"** button - Returns to Business Information Validations
- **"Continue"** button - Proceeds to Review Ownership (LLC-specific)
- **"CANCEL APPLICATION"** button - Cancels the application

---

## Step 6: Review Ownership Page (LLC-Specific)

**Route:** `/new-business-account/review-ownership`

**Component:** `ReviewOwnershipComponent`

### Purpose
Collects regulatory compliance information for the LLC, including Person with Control and Beneficial Owners as required by the Corporate Transparency Act (CTA) and Bank Secrecy Act (BSA).

### User Interface
- **Page Title:** "Review Ownership"
- **Stepper:** Step 2 - "Ownership & Resolution" is highlighted
- **Two Main Sections:**
  1. **Person with Control** - Individual who has significant control over the business
  2. **Beneficial Owners** - Individuals who own 25% or more of the business

### Person with Control Section:
- Dropdown to select from existing customers
- Title field (e.g., "Manager/Designated Member", "Owner", "Partner")
- Option to add a new customer if not in the list

### Beneficial Owners Section:
- Checkbox: "Nobody owns 25% or more of this business"
- If applicable, add beneficial owners with:
  - Customer selection
  - Ownership percentage (must be 25-100%)
- Total ownership percentage displayed (cannot exceed 100%)

### User Actions
1. User selects or adds a Person with Control
2. User specifies their title/role
3. User indicates if nobody owns 25% or more, OR
4. User adds beneficial owners with ownership percentages
5. User clicks **"Continue"** when ownership is complete

### Validation Rules:
- Person with Control is required
- If beneficial owners exist, each must have 25-100% ownership
- Total beneficial ownership cannot exceed 100%

### Navigation
- **"GO BACK"** button - Returns to Customer Information page
- **"Continue"** button - Proceeds to Business Resolution

---

## Step 7: Business Resolution Page (LLC-Specific)

**Route:** `/new-business-account/business-resolution`

**Component:** `BusinessResolutionComponent`

### Purpose
Handles the Business Resolution document (BEDAR - Business Entity Deposit Account Resolution) which authorizes the account opening and designates signers.

### User Interface
- **Page Title:** "Business Resolution"
- **Stepper:** Step 2 - "Ownership & Resolution" is highlighted

### For Existing Businesses with Resolution on File:
- Displays existing Business Resolution PDF
- Question: "Do we need to generate a new form to reflect any revisions?"
- Yes/No toggle

### For New Business or New Resolution Needed:
- **Management Type Selection:** How is the business entity managed?
  - Manager-Managed
  - Member-Managed

- **Members/Managers Selection:** Select individuals to appear on the Business Resolution form
  - Checkboxes for each LLC manager

- **Additional Fields:**
  - Meeting Date
  - State of Organization
  - County of Organization

### User Actions
1. User reviews existing resolution (if applicable)
2. User decides if new resolution is needed
3. User selects management type
4. User selects members/managers for the resolution
5. User enters meeting date, state, and county
6. User clicks **"Continue"** to generate/confirm resolution

### Navigation
- **"GO BACK"** button - Returns to Review Ownership
- **"Continue"** button - Proceeds to Business Summary

---

## Step 8: Business Summary Page (LLC-Specific)

**Route:** `/new-business-account/business-summary`

**Component:** `BusinessSummaryComponent`

### Purpose
Provides a comprehensive summary of the business structure before proceeding to profile validations.

### User Interface
- **Page Title:** "Business Summary"
- **Stepper:** Step 3 - "Business Summary" is highlighted

### Summary Sections:
1. **Business Card:**
   - Business Name
   - Business CIF

2. **Members/Managers:**
   - Grid showing all LLC managers with names and CIFs

3. **Signers:**
   - Grid showing all convenience signers (if any)

4. **Person with Control:**
   - Name, CIF, and Title

5. **Beneficial Owners:**
   - Names, CIFs, and Ownership Percentages

### User Actions
1. User reviews all business structure information
2. User verifies accuracy of all parties
3. User clicks **"Continue"** to proceed to validations

### Navigation
- **"GO BACK"** button - Returns to Business Resolution
- **"Continue"** button - Proceeds to Profile Validation Results

---

## Step 9: Profile Validation Results Page

**Route:** `/new-business-account/profile-validation-results`

**Component:** `ProfileValidationResultsComponent`

### Purpose
Validates all customer profiles (LLC managers, signers, beneficial owners, person with control) through FIS checks and displays the results.

### User Interface
- **Page Title:** "Validating Customer Information..."
- **Validation Cards:** One card per customer showing validation status
- For LLC, shows combined customers including:
  - LLC Managers
  - Convenience Signers
  - Beneficial Owners
  - Person with Control (deduplicated if same person appears multiple times)

### When Validation is NOT Shown for a Customer

A customer's validation card is **skipped** when **both** conditions are true:
1. **The customer has an existing CIF** (`customerInformationFile` exists)
2. **The customer already has deposit accounts** at the bank (`hasDepositAccounts` is `true`)

### User Actions
1. User waits for validations to complete
2. User reviews validation results
3. User clicks **"Continue"** when validations pass

### Navigation
- **"GO BACK"** button - Returns to Business Summary
- **"Continue"** button - Proceeds to Spend Life Wisely Pillars

---

## Step 10: Spend Life Wisely Pillars Page

**Route:** `/new-business-account/slw-pillars`

**Component:** `SlwPillarsComponent`

### Purpose
Introduces the customer to First United Bank's "Spend Life Wisely" philosophy with four pillars of financial well-being.

### User Interface
- **Page Title:** "Spend Life Wisely Pillars."
- **Four Pillar Cards displayed in a grid:**
  1. **Faith** - "Treasure your faith."
  2. **Financial Well-Being** - "Care for your money."
  3. **Health** - "Invest in your wellness."
  4. **Personal Growth** - "Enrich your mind."

### User Actions
1. User reviews the Spend Life Wisely philosophy
2. User clicks **"Continue"** to proceed

### Navigation
- **"GO BACK"** button - Returns to Profile Validation Results
- **"Continue"** button - Proceeds to Account Solutions

---

## Step 11: Account Solutions Page

**Route:** `/new-business-account/account-solutions`

**Component:** `AccountSolutionsComponent`

### Purpose
Allows users to select banking products (accounts) to open for the business.

### User Interface
- **Page Title:** "Which checking solutions best meet [Business Name]'s needs?"
- **Stepper:** Step 4 - "Account Details" is highlighted
- **Tabbed Interface with 3 tabs:**
  1. **CHECKING** - Business checking account products
  2. **SAVINGS** - Business savings account products
  3. **CERTIFICATE OF DEPOSIT** - CD products (controlled by feature flag)

- **Product Cards:** Each available product shown as a selectable card

### User Actions
1. User browses available products in each tab
2. User selects products by clicking on product cards (up to 6 products total)
3. User can deselect products
4. User receives a warning toast when reaching 6 products limit
5. User clicks **"Continue"** when product selection is complete

### Navigation
- **"GO BACK"** button - Returns to SLW Pillars
- **"Continue"** button - Proceeds to Account Details Review

---

## Step 12: Account Details Review Page

**Route:** `/new-business-account/account-details-review`

**Component:** `AccountDetailsReviewComponent`

### Purpose
Allows users to review and customize details for each selected product.

### User Interface
- **Page Title:** "Review account details and adjust if needed."
- **Stepper:** Step 4 - "Account Details" is highlighted
- **Account Detail Sections:** One collapsible section per selected product showing:
  - Account title/name (includes business name)
  - LLC manager details
  - Authorized signers selection
  - Branch/officer selection

### User Actions
1. User reviews account details for each product
2. User can customize:
   - Account title format
   - Add/remove authorized signers
   - Select branch and officer
3. User clicks **"Continue"** when details are configured

### Navigation
- **"GO BACK"** button - Returns to Account Solutions
- **"Continue"** button - Proceeds to Additional Services

---

## Step 13: Additional Services Page

**Route:** `/new-business-account/additional-services`

**Component:** `AdditionalServicesComponent`

### Purpose
Allows users to configure additional services for the selected products.

### User Interface
- **Page Title:** "Let's choose additional services for your Financial Well-Being Journey."
- **Stepper:** Step 5 - "Additional Services" is highlighted
- **Tabbed Interface with services (Business Account has 5 tabs):**
  1. **DEBIT CARDS** - Configure debit card options
  2. **PARTNER SOLUTIONS** - Business partner integrations (Business accounts only)
  3. **OVERDRAFT** - Select overdraft protection options
  4. **ONLINE BANKING** - Set up online banking access
  5. **PAPER CHECKS** - Order paper checks

### User Actions
For each service tab, user configures options:

1. **Debit Cards:**
   - Select which individuals need debit cards
   - Choose card options

2. **Partner Solutions (Business only):**
   - Select business partner integrations
   - Configure business services

3. **Overdraft:**
   - Select overdraft protection preferences

4. **Online Banking:**
   - Set up online banking enrollment
   - Configure email for digital services

5. **Paper Checks:**
   - Choose whether to order checks
   - Select check style/design

### Navigation
- **"GO BACK"** button - Returns to Account Details Review
- **"Continue"** button - Proceeds to Review Accounts

---

## Step 14: Review Accounts Page

**Route:** `/new-business-account/review-accounts`

**Component:** `ReviewAccountsComponent`

### Purpose
Provides a comprehensive summary of all account details before finalizing the application.

### User Interface
- **Page Title:** "Let's review for accuracy."
- **Stepper:** Step 6 - "Review" is highlighted
- **Ownership Type Card:** Shows "Limited Liability Company" or "Single Member LLC"
- **Account Cards:** One card per selected product showing:
  - Product name
  - Account title (includes business name)
  - Business information
  - LLC manager information
  - Signers assigned
  - Services configured
  - Branch and officer information

### User Actions
1. User reviews all account details for accuracy
2. User clicks **"Continue"** to proceed to document signing

### Navigation
- **"GO BACK"** button - Returns to Additional Services
- **"Continue"** button - Proceeds to Disclose & Sign

---

## Step 15: Disclose & Sign Page

**Route:** `/new-business-account/review-disclosures-and-sign`

**Component:** `DiscloseAndSignComponent`

### Purpose
Handles the disclosure documents and signature collection process for account opening.

### User Interface
- **Page Title:** "Disclose & Sign"
- **Stepper:** Step 7 - "Disclose & Sign" is highlighted

### Flow Options
User is presented with signing method selection:

1. **Sign via DocuSign:**
   - System sends DocuSign email links to all signers
   - Shows status of signatures
   - User can verify signatures

2. **Sign Paper Documents:**
   - System generates printable documents
   - User can print documents
   - User confirms signatures captured on paper

### Documents Include:
- Account agreements
- Business Resolution (BEDAR)
- Beneficial ownership certification
- Other regulatory disclosures

### User Actions
1. User selects signing method
2. For DocuSign: Monitor and verify signature completion
3. For Paper: Print documents and confirm signatures captured
4. User clicks **"Continue"** when all signatures are complete

### Navigation
- **"GO BACK"** button - Returns to Review Accounts
- **"Continue"** button - Creates the account and proceeds to Account Details Confirmation

---

## Step 16: Account Details Confirmation Page

**Route:** `/new-business-account/account-details-confirmation`

**Component:** `AccountDetailsConfirmationComponent`

### Purpose
Displays the created account details including account numbers and provides links for final setup steps.

### User Interface
- **Page Title:** "Your account has been created. Complete the final steps." (or "Your account has been created." if no additional steps needed)
- **Stepper:** Step 8 - "Complete" is highlighted

### Final Action Buttons (if applicable):
- **PRINT DEBIT CARD** - Opens debit card printing interface
- **ORDER CHECKS** - Opens check ordering interface

### Account Details Cards showing:
- **Business Information:**
  - Business name
  - Business CIF
  - Business address

- **LLC Members/Managers:**
  - Names and CIFs for all managers

- **Account Information (per product):**
  - Account number (with copy button)
  - Routing number (with copy button)
  - Account type
  - Services enrolled

### User Actions
1. User reviews created account details
2. User copies account/routing numbers as needed
3. User completes final steps (print debit cards, order checks) if applicable
4. User clicks **"Continue"** to proceed

### Navigation
- **"Continue"** button - Proceeds to Financial Well-Being Journey Map

---

## Step 17: Financial Well-Being Journey Map Page

**Route:** `/new-business-account/financial-wellbeing-journey-map`

**Component:** `FinancialWellbeingJourneyMapComponent`

### Purpose
Displays the Financial Well-Being Journey Map as a discussion tool for the banker to use with the business customer.

### User Interface
- **Page Title:** "What does your customer say about their Financial Well-Being Journey?"
- **Journey Map Image:** Large visual representation of the financial well-being journey

### User Actions
1. Banker reviews the journey map with the customer
2. User clicks **"Continue"** to proceed to final confirmation

### Navigation
- **"Continue"** button - Proceeds to Account Confirmation

---

## Step 18: Account Confirmation Page

**Route:** `/new-business-account/account-confirmation`

**Component:** `AccountConfirmationComponent`

### Purpose
Final confirmation page celebrating successful account creation.

### User Interface
- **Title:** "Congratulations!"
- **Subtitle:** "Your Financial Well-Being Journey starts now."
- **Success Icon:** Large green checkmark
- **Action Button:** "START A NEW APPLICATION"

### User Actions
1. User reviews the congratulations message
2. User clicks **"START A NEW APPLICATION"** to return to landing page

### Navigation
- **"START A NEW APPLICATION"** button - Resets the flow and returns to the landing page

---

## Flow Summary Diagram

```
┌─────────────────────────────────┐
│         Landing Page            │
│  1. Select "Open an Account"    │
│  2. Select "Business"           │
│  3. Select State (TX/OK)        │
│  4. Select "LLC" or "SMLLC"     │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Business Search Dialog      │
│  Search or Create Business      │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Business Information Page     │
│  (Step 1: Customer Information) │
│  Review Business Details        │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│  Business Info Validations      │
│  (Step 1: Customer Information) │
│  BizChex + OFAC + CDD           │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Customer Information Page     │
│  (Step 1: Customer Information) │
│  Add LLC Managers/Signers       │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│    Review Ownership Page        │  ◄── LLC-SPECIFIC
│ (Step 2: Ownership & Resolution)│
│  Person with Control +          │
│  Beneficial Owners              │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Business Resolution Page      │  ◄── LLC-SPECIFIC
│ (Step 2: Ownership & Resolution)│
│  BEDAR Document Generation      │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│    Business Summary Page        │  ◄── LLC-SPECIFIC
│   (Step 3: Business Summary)    │
│  Review All Business Structure  │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│  Profile Validation Results     │
│   (Step 3: Business Summary)    │
│  Validate All Customer Profiles │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Spend Life Wisely Pillars     │
│   (Step 3: Business Summary)    │
│  Review SLW Philosophy          │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Account Solutions Page      │
│    (Step 4: Account Details)    │
│  Select Products                │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Account Details Review Page   │
│    (Step 4: Account Details)    │
│  Configure Account Details      │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Additional Services Page      │
│  (Step 5: Additional Services)  │
│  Debit Cards, Partner Solutions,│
│  Overdraft, Online Banking,     │
│  Paper Checks                   │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Review Accounts Page        │
│       (Step 6: Review)          │
│  Review All Account Details     │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│    Disclose & Sign Page         │
│   (Step 7: Disclose & Sign)     │
│  Sign Documents (includes BEDAR)│
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│ Account Details Confirmation    │
│      (Step 8: Complete)         │
│  View Account Details           │
│  Print Debit Cards/Order Checks │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│  Financial Well-Being Journey   │
│      (Step 8: Complete)         │
│  Review Journey Map with        │
│  Customer                       │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Account Confirmation Page     │
│      (Step 8: Complete)         │
│     "Congratulations!"          │
│  START A NEW APPLICATION        │
└─────────────────────────────────┘
```

---

## Stepper Steps Summary (LLC - 8 Steps)

The LLC flow uses an 8-step stepper (2 more than Sole Proprietorship):

| Step | Name | Pages Included |
|------|------|----------------|
| 1 | Customer Information | Business Information, Business Info Validations, Customer Information |
| 2 | Ownership & Resolution | Review Ownership, Business Resolution |
| 3 | Business Summary | Business Summary, Profile Validation Results, SLW Pillars |
| 4 | Account Details | Account Solutions, Account Details Review |
| 5 | Additional Services | Additional Services (all tabs including Partner Solutions) |
| 6 | Review | Review Accounts |
| 7 | Disclose & Sign | Disclose & Sign |
| 8 | Complete | Account Details Confirmation, Financial Well-Being Journey Map, Account Confirmation |

---

## Key Differences from Sole Proprietorship Flow

LLC has **additional pages** compared to Sole Proprietorship:

| Feature | Sole Proprietorship | LLC |
|---------|-------------------|-----|
| **Stepper Steps** | 6 steps | 8 steps |
| **Review Ownership** | Not required | Required (Person with Control + Beneficial Owners) |
| **Business Resolution** | Not required | Required (BEDAR document) |
| **Business Summary** | Not required | Required (comprehensive review) |
| **Required Customers** | 1 Primary Owner | 1+ LLC Managers |
| **Beneficial Owners** | Not applicable | Required if anyone owns 25%+ |
| **Person with Control** | Not applicable | Required |

---

## Regulatory Compliance Requirements (LLC-Specific)

### Person with Control
- Required for all LLCs
- Must be a natural person (not another business entity)
- Must have significant management responsibilities
- Title options: Agent, Authorized Officer, Corporate Secretary, Manager/Designated Member, Owner, Partner

### Beneficial Owners
- Required to disclose any individual owning 25% or more of the LLC
- If no one owns 25% or more, user must check "Nobody owns 25% or more"
- Each beneficial owner's percentage must be between 25-100%
- Total ownership cannot exceed 100%
- Part of Corporate Transparency Act (CTA) compliance

### Business Resolution (BEDAR)
- Required legal document authorizing account opening
- Specifies management type (Manager-Managed or Member-Managed)
- Lists authorized signers
- Includes meeting date, state, and county of organization
- Can reuse existing resolution or generate new one

---

## Cancel Application

At any point during the flow (except final steps), users can click **"CANCEL APPLICATION"** button in the bottom-left corner. This:
1. Shows a confirmation dialog
2. If confirmed, resets the flow and returns to the landing page
3. Any unsaved data is discarded

---

## Notes

- LLC flow is the most comprehensive business account opening flow
- Both "Limited Liability Company" and "Single Member LLC" ownership types use this same LLC flow
- The Person with Control and Beneficial Owners requirements are part of federal regulatory compliance
- Products available depend on the selected ownership type and community bank location
- CD products visibility is controlled by a feature flag (`selectIsCdsEnabled`)
- The browser back button is intercepted on the Disclose & Sign page to prevent accidental navigation
- Existing businesses may have a Business Resolution (BEDAR) on file that can be reused or updated

