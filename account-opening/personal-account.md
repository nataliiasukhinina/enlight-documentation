# Personal Account Opening User Flow Documentation

This document describes the step-by-step user flow when users select the "Open Account" → "Personal" → any state → "Personal: Individual/Single Party" (or similar single-party ownership type) option on the landing page.

---

## Flow Overview

The Personal Account Opening flow allows users to open a new personal bank account for an individual customer. The flow guides users through customer information entry, profile validation, product selection, service configuration, document signing, and account confirmation.

---

## Step 1: Landing Page - Account Opening Setup

**Route:** `/` (root)

**Component:** `LandingPageComponent`

### Purpose
The landing page serves as the entry point where users configure the basic parameters for the new account.

### User Interface
- Welcome message: "Welcome to Your Spend Life Wisely Journey"
- A card with sequential form options

### User Actions
1. User selects task toggle: **"OPEN AN ACCOUNT"**
2. User selects account type toggle: **"Personal"**
3. User selects community bank location: **"Texas"** or **"Oklahoma"** (with state map icons)
4. User selects ownership type from dropdown (e.g., "Single Party", "Single Party W/ POD", "Joint", etc.)
5. User clicks **"Continue"** button

### What Happens on Continue
- A fullscreen customer search dialog opens

---

## Step 2: Customer Search Dialog

**Component:** `CustomerManagementDialogComponent` (fullscreen dialog)

### Purpose
A fullscreen dialog that allows users to search for an existing customer or create a new one for the account.

### User Interface
- **Title:** "Search for the Primary Owner"
- **Search Component:** A search table for finding individual customers
- **Action Button:** "CREATE NEW CUSTOMER"

### User Actions
1. User can search for an existing customer by:
   - Name
   - CIF (Customer Information File)
   - SSN (Social Security Number)

2. User views search results in a table

3. User has two options:
   - **Select an existing customer** from search results and click **"Continue"**
   - **Click "CREATE NEW CUSTOMER"** to enter a new customer's information

### If Creating New Customer:
- Dialog transitions to a CIF Edit form
- User enters customer personal information (name, SSN, date of birth, address, etc.)
- User clicks **"Continue"** to save and proceed

### What Happens on Continue
- System creates an engagement and selects/creates the customer profile
- Dialog closes and user navigates to the Customer Information page

---

## Step 3: Customer Information Page

**Route:** `/new-personal-account/customer-information`

**Component:** `CustomerInformationPageComponent`

### Purpose
Displays and manages all customers associated with the account (primary owner, joint owners, signers, etc.) based on the selected ownership type.

### User Interface
- **Page Title:** "Let's get to know you."
- **Stepper:** Shows 6 steps - "Customer Information" is highlighted (Step 1)
- **Customer Cards:** Lists all customers added to the account
- **Add Customer Actions:** Buttons to add additional customers based on ownership type

### User Actions
1. User can view the primary customer information
2. User can add additional customers:
   - Joint Owners (for joint ownership types)
   - Convenience Signers
3. User can edit existing customer information
4. User can remove added customers (except primary)
5. User clicks **"Continue"** when all required customers are added

### Navigation
- **"GO BACK"** button - Returns to the search dialog
- **"Continue"** button - Proceeds to Profile Validation Results
- **"CANCEL APPLICATION"** button - Cancels the application (with confirmation)

---

## Step 4: Profile Validation Results Page

**Route:** `/new-personal-account/profile-validation-results`

**Component:** `ProfileValidationResultsComponent`

### Purpose
Validates all customer profiles through FIS (Financial Information System) checks and displays the results. This includes identity verification, fraud checks, and other regulatory requirements.

### User Interface
- **Page Title:** "Validating Customer Information..."
- **Validation Cards:** One card per customer showing validation status
- Each card shows:
  - Customer name
  - Validation status (Pending, In Progress, Passed, Failed)
  - Any adverse action notices

### When Validation is NOT Shown for a Customer

A customer's validation card is **skipped** (not displayed) on this page when **both** of the following conditions are true:

1. **The customer has an existing CIF** (`customerInformationFile` exists)
2. **The customer already has deposit accounts** at the bank (`hasDepositAccounts` is `true`)

This is because existing customers who already have deposit accounts have been previously validated by the bank, so re-validation is unnecessary.

**Examples:**
- **New customer** (no CIF) → Validation is shown
- **Existing customer without deposit accounts** → Validation is shown
- **Existing customer with deposit accounts** → Validation is **NOT** shown (skipped)

### User Actions
1. User waits for validations to complete (loading state)
2. User reviews validation results for each customer
3. If validations pass, user clicks **"Continue"**
4. If validations fail, user may need to address issues or print adverse action letters

### Navigation
- **"GO BACK"** button - Returns to Customer Information page
- **"Continue"** button - Proceeds to Spend Life Wisely Pillars

---

## Step 5: Spend Life Wisely Pillars Page

**Route:** `/new-personal-account/slw-pillars`

**Component:** `SlwPillarsComponent`

### Purpose
Introduces the customer to First United Bank's "Spend Life Wisely" philosophy with four pillars of financial well-being.

### User Interface
- **Page Title:** "Spend Life Wisely Pillars."
- **Four Pillar Cards displayed in a grid:**
  1. **Faith** - "Treasure your faith." - Spiritual guidance and purpose
  2. **Financial Well-Being** - "Care for your money." - Intentional saving and spending
  3. **Health** - "Invest in your wellness." - Physical, mental, and spiritual health
  4. **Personal Growth** - "Enrich your mind." - Learning and growing

### User Actions
1. User reviews the Spend Life Wisely philosophy
2. User clicks **"Continue"** to proceed

### Navigation
- **"GO BACK"** button - Returns to Profile Validation Results
- **"Continue"** button - Proceeds to Account Solutions

---

## Step 6: Account Solutions Page

**Route:** `/new-personal-account/account-solutions`

**Component:** `AccountSolutionsComponent`

### Purpose
Allows users to select banking products (accounts) to open for the customer.

### User Interface
- **Page Title:** "Which checking solutions best meet [Customer Name]'s needs?" (dynamic based on active tab)
- **Stepper:** Step 2 - "Account Details" is highlighted
- **Tabbed Interface with 3 tabs:**
  1. **CHECKING** - Checking account products
  2. **SAVINGS** - Savings account products (may be disabled if no products available)
  3. **CERTIFICATE OF DEPOSIT** - CD products (controlled by feature flag)

- **Product Cards:** Each available product shown as a selectable card with:
  - Product name
  - Description
  - Features/benefits

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

## Step 7: Account Details Review Page

**Route:** `/new-personal-account/account-details-review`

**Component:** `AccountDetailsReviewComponent`

### Purpose
Allows users to review and customize details for each selected product, including titles, authorized signers, and beneficiaries.

### User Interface
- **Page Title:** "Review account details and adjust if needed."
- **Stepper:** Step 2 - "Account Details" is highlighted
- **Account Detail Sections:** One collapsible section per selected product showing:
  - Account title/name
  - Primary owner details
  - Authorized signers selection
  - Beneficiary assignments (for POD accounts)
  - Branch/officer selection

### User Actions
1. User reviews account details for each product
2. User can customize:
   - Account title format
   - Add/remove authorized signers
   - Assign primary and contingent beneficiaries (if applicable)
   - Select branch and officer
3. User clicks **"Continue"** when details are configured

### Navigation
- **"GO BACK"** button - Returns to Account Solutions
- **"Continue"** button - Proceeds to Additional Services

---

## Step 8: Additional Services Page

**Route:** `/new-personal-account/additional-services`

**Component:** `AdditionalServicesComponent`

### Purpose
Allows users to configure additional services for the selected products.

### User Interface
- **Page Title:** "Let's choose additional services for your Financial Well-Being Journey."
- **Stepper:** Step 3 - "Additional Services" is highlighted
- **Tabbed Interface with services:**
  1. **DEBIT CARDS** - Configure debit card options
  2. **OVERDRAFT SERVICES** - Select overdraft protection options
  3. **ONLINE BANKING** - Set up online banking access
  4. **PAPER CHECKS** - Order paper checks

### User Actions
For each service tab, user configures options:

1. **Debit Cards:**
   - Select which customers need debit cards
   - Choose card options

2. **Overdraft Services:**
   - Select overdraft protection preferences
   - Choose linked accounts for overdraft

3. **Online Banking:**
   - Set up online banking enrollment
   - Configure email for digital services

4. **Paper Checks:**
   - Choose whether to order checks
   - Select check style/design

5. User progresses through tabs using **"Continue"** and **"Go Back"** buttons within each service

### Navigation
- **"GO BACK"** button - Returns to Account Details Review
- **"Continue"** button - Proceeds to Review Accounts (after completing all services)

---

## Step 9: Review Accounts Page

**Route:** `/new-personal-account/review-accounts`

**Component:** `ReviewAccountsComponent`

### Purpose
Provides a comprehensive summary of all account details before finalizing the application.

### User Interface
- **Page Title:** "Let's review for accuracy."
- **Stepper:** Step 4 - "Review" is highlighted
- **Ownership Type Card:** Shows selected ownership type
- **Account Cards:** One card per selected product showing:
  - Product name
  - Account title
  - Owner information (name, CIF)
  - Signers assigned
  - Beneficiaries (if applicable)
  - Services configured (debit cards, online banking, etc.)
  - Branch and officer information

### User Actions
1. User reviews all account details for accuracy
2. User clicks **"Continue"** to proceed to document signing

### Navigation
- **"GO BACK"** button - Returns to Additional Services
- **"Continue"** button - Proceeds to Disclose & Sign

---

## Step 10: Disclose & Sign Page

**Route:** `/new-personal-account/review-disclosures-and-sign`

**Component:** `DiscloseAndSignComponent`

### Purpose
Handles the disclosure documents and signature collection process for account opening.

### User Interface
- **Page Title:** "Disclose & Sign"
- **Stepper:** Step 5 - "Disclose & Sign" is highlighted

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

### User Interface Elements
- **Selection Card:** Choose between DocuSign or Paper signing
- **Document List:** Shows all documents that need signing
- **Signature Status:** Shows completion status for each signer
- **Print Documents:** Button to print all documents
- **Verify Signatures:** Button to check DocuSign status

### User Actions
1. User selects signing method
2. For DocuSign:
   - Wait for documents to be prepared
   - Monitor signature completion
   - Verify signatures when complete
3. For Paper:
   - Print documents
   - Confirm signatures captured
4. User clicks **"Continue"** when all signatures are complete

### Navigation
- **"GO BACK"** button - Returns to Review Accounts
- **"Continue"** button - Creates the account and proceeds to Account Details Confirmation

---

## Step 11: Account Details Confirmation Page

**Route:** `/new-personal-account/account-details-confirmation`

**Component:** `AccountDetailsConfirmationComponent`

### Purpose
Displays the created account details including account numbers and provides links for final setup steps.

### User Interface
- **Page Title:** "Your account has been created. Complete the final steps." (or "Your account has been created." if no additional steps needed)
- **Stepper:** Step 6 - "Complete" is highlighted (all steps shown as complete)

### Final Action Buttons (if applicable):
- **PRINT DEBIT CARD** - Opens debit card printing interface
- **ORDER CHECKS** - Opens check ordering interface

### Account Details Cards showing:
- **Customer Information:**
  - Customer name
  - Customer CIF
  - Customer address
  - SSN (masked)

- **Account Information (per product):**
  - Account number (with copy button)
  - Routing number (with copy button)
  - Account type
  - Services enrolled

### User Actions
1. User reviews created account details
2. User copies account/routing numbers as needed
3. User completes final steps (print debit cards, order checks) if applicable
4. User clicks **"Continue"** to proceed to final confirmation

### Navigation
- **"Continue"** button - Proceeds to Account Confirmation

---

## Step 12: Account Confirmation Page

**Route:** `/new-personal-account/account-confirmation`

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
2. User clicks **"START A NEW APPLICATION"** to return to landing page and begin a new application

### Navigation
- **"START A NEW APPLICATION"** button - Resets the flow and returns to the landing page

---

## Flow Summary Diagram

```
┌─────────────────────────────────┐
│         Landing Page            │
│  1. Select "Open an Account"    │
│  2. Select "Personal"           │
│  3. Select State (TX/OK)        │
│  4. Select Ownership Type       │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Customer Search Dialog      │
│  Search or Create Customer      │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Customer Information Page     │
│  (Step 1: Customer Information) │
│  Add/Manage Account Customers   │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│  Profile Validation Results     │
│  (Step 1: Customer Information) │
│  Validate Customer Profiles     │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Spend Life Wisely Pillars     │
│  (Step 1: Customer Information) │
│  Review SLW Philosophy          │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Account Solutions Page      │
│    (Step 2: Account Details)    │
│  Select Products (Checking/     │
│    Savings/CDs)                 │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Account Details Review Page   │
│    (Step 2: Account Details)    │
│  Configure Account Titles,      │
│  Signers, Beneficiaries         │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Additional Services Page      │
│  (Step 3: Additional Services)  │
│  Configure Debit Cards,         │
│  Overdraft, Online Banking,     │
│  Paper Checks                   │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│     Review Accounts Page        │
│       (Step 4: Review)          │
│  Review All Account Details     │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│    Disclose & Sign Page         │
│   (Step 5: Disclose & Sign)     │
│  Sign Documents (DocuSign or    │
│  Paper)                         │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│ Account Details Confirmation    │
│      (Step 6: Complete)         │
│  View Created Account Details   │
│  Print Debit Cards/Order Checks │
│         Click Continue          │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│   Account Confirmation Page     │
│      (Step 6: Complete)         │
│     "Congratulations!"          │
│  START A NEW APPLICATION        │
└─────────────────────────────────┘
```

---

## Stepper Steps Summary

The flow uses a 6-step stepper shown throughout the process:

| Step | Name | Pages Included |
|------|------|----------------|
| 1 | Customer Information | Customer Information, Profile Validation Results, SLW Pillars |
| 2 | Account Details | Account Solutions, Account Details Review |
| 3 | Additional Services | Additional Services (all tabs) |
| 4 | Review | Review Accounts |
| 5 | Disclose & Sign | Disclose & Sign |
| 6 | Complete | Account Details Confirmation, Account Confirmation |

---

## Cancel Application

At any point during steps 1-4, users can click **"CANCEL APPLICATION"** button in the bottom-left corner. This:
1. Shows a confirmation dialog
2. If confirmed, resets the flow and returns to the landing page
3. Any unsaved data is discarded

---

## Notes

- The flow supports various ownership types (Single Party, Single Party W/ POD, Joint, UTMA, Rep Payee, etc.) which may affect:
  - Which customers can be added (custodians for UTMA, rep payees, etc.)
  - Which beneficiaries can be configured
  - Document requirements

- The browser back button is intercepted on the Disclose & Sign page to prevent accidental navigation

- If the user has an existing engagement, navigating away triggers a confirmation dialog

- Products available depend on the selected ownership type and community bank location

- CD products visibility is controlled by a feature flag (`selectIsCdsEnabled`)


