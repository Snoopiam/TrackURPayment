# TrackURPayment - AI Coding Agent Instructions

## Project Overview
TrackURPayment is a **multi-property off-plan real estate payment tracking system** built as standalone HTML applications. Each off-plan property purchase has its own independent HTML file with embedded CSS/JS, designed for offline capability and simple deployment.

**Key Properties:**
- **Delphine_B906/**: Older codebase (2023-2024) with jQuery/Bootstrap 3, includes versioned files showing evolution
- **JacobCo_C422/**: Modern property with admin management interfaces (drag-drop + form-based)
- **JacobCo_J307/**: Standard property implementation

## Architecture & Design Philosophy

### Standalone HTML Pattern
Each property is a **complete single-file application** (~2000-4000 lines) with embedded CSS and JavaScript. This architectural choice prioritizes:
- Zero build process or dependencies
- Offline-first functionality
- Simple deployment (copy file anywhere)
- Property isolation (no shared state between buildings)

### Code Structure Within Each File
```html
<!DOCTYPE html>
<html>
<head>
  <!-- CSS: Inline <style> blocks (~500-800 lines) -->
  <!-- External: Font Awesome, Google Fonts via CDN -->
</head>
<body>
  <!-- HTML Structure: Modal-heavy UI with data tables -->
  <script>
    // JavaScript: Inline (~1200-2000 lines)
    // Libraries: jQuery + jsPDF (via CDN)
  </script>
</body>
</html>
```

### Architecture Variants: CONFIG vs localStorage

**Two distinct patterns exist across properties:**

#### Pattern 1: CONFIG Object (Single-Client Off-Plan Property Tracker)
**Used in:** Delphine_B906

**Purpose:** Track one client's off-plan property purchase with SPA payment schedule

**Data Structure:**
```javascript
const CONFIG = {
  purchasePrice: 3125008,
  usdRate: 0.272,
  registrationFee: 1000,
  
  payments: [
    { category: 'booking', amount: 65000, date: '29-Sep-24', receipt: 'De-BR-DE-B906/...' },
    { category: 'first', amount: 110000, date: '15-Oct-24', receipt: 'De-BR-DE-B906/...' },
    // ... more payments
  ],
  
  nextPayment: {
    amount: 312500.80,
    date: "24 March 2026",
    dueDate: new Date("2026-03-24T00:00:00+04:00")
  }
};
```

**Update Workflow:**
1. Edit CONFIG object directly in HTML file
2. Calculations auto-run on page load (no save button)
3. Payments array stores complete receipt history
4. All data embedded in HTML (no external storage)

**Best For:** Off-plan property purchases with SPA payment schedules

#### Pattern 2: localStorage (Currently Unused)
**Reserved for:** Future multi-unit or portfolio tracking features

**Note:** While localStorage infrastructure exists in codebase, **all current properties (Jacob C422, J307, and Delphine) use the CONFIG pattern** for single off-plan property payment tracking.

**Potential Future Use:**
```javascript
const STORAGE_KEY = 'unitDataJacobCo_C422';
localStorage.setItem(STORAGE_KEY, JSON.stringify([
  {
    unitNumber: "C422",
    clientName: "Client Name",
    purchasePrice: 3235796,
    paymentHistory: [
      { category: "booking", amount: 647159, date: "2024-09-30" }
    ]
  }
]));
```

**Would Enable:**
1. Dynamic CRUD operations via modals
2. Client-side persistence without code editing
3. Import/export (JSON/CSV) functionality
4. Multi-property portfolio tracking

**Current Reality:** All properties use CONFIG object pattern (static, code-based)

## Critical Patterns & Conventions

### 1. Data Storage Architecture

#### CONFIG Object Pattern (All Properties)
**Universal Pattern:** All current properties (Jacob C422, J307, Delphine B906) use this approach

**Jacob Properties (Numbered Payments):**
```javascript
const CONFIG = {
  payment001: 647159,  // Booking Amount
  payment002: 323580,  // First Installment
  payment003: 323580,  // Second Installment
  purchasePrice: 3235796,
  registrationFee: 1000
};
```

**Delphine Property (Categorized Payments):**
- **Embedded JavaScript object** as single source of truth
- Data format: Payments array with categories (booking, first, second, third, registration)
- Auto-calculated totals on page load
- Example structure:
```javascript
const CONFIG = {
  purchasePrice: 3125008,
  payments: [
    { category: 'booking', amount: 65000, date: '29-Sep-24', receipt: 'De-BR-DE-B906/...' }
  ],
  nextPayment: { amount: 312500.80, date: "24 March 2026", dueDate: new Date("2026-03-24...") }
};
```

**When editing Delphine:** Update CONFIG object, not localStorage. Calculations run automatically.

### 2. Branding System
Each property has TWO versions:
- **Base version** (`JacobCo_C422.html`): Generic "TrackURPayment" branding
- **Branded version** (`JacobCo_C422_branded.html`): Property-specific logos/colors

**When editing:** Apply functional changes to BOTH versions, keeping only branding differences.

### 3. Modal-Centric UI Pattern
All user interactions happen through Bootstrap modals:
- Add/Edit Unit
- Add/Edit Payment
- View Payment History
- Settings & Backup/Restore
- Export (PDF/CSV)

**Key convention:** Modal IDs follow pattern `{action}{EntityName}Modal` (e.g., `addUnitModal`, `viewHistoryModal`)

### 4. Admin Interfaces (JacobCo_C422 Only)
Two admin tools for managing unit data:
- **`admin_mockup_form_based.html`**: Traditional form interface
- **`admin_mockup_drag_drop.html`**: Drag-and-drop file uploader for CSV imports

Both read/write to the SAME localStorage key as the main property file.

### 5. Technology Stack Per Property

**Modern Properties (C422, J307):**
- Bootstrap 5.3
- jQuery 3.7.1
- Font Awesome 6.4.0
- jsPDF 2.5.1 (PDF generation)
- CONFIG object with numbered payments (payment001, payment002, etc.)
- Simple calculation logic

**Legacy Property (Delphine_B906):**
- Bootstrap 3.4.1
- jQuery 3.5.1
- Adobe Fonts (arboria)
- CONFIG object with categorized payments array
- Advanced payment logic (categories, overpayments, advance credits)
- Collapsible payment sections with detailed tables

**Key Architectural Differences:**

| Feature | Jacob Properties (C422, J307) | Delphine Property (B906) |
|---------|-----------------|-------------------|
| **Data Storage** | CONFIG object (static) | CONFIG object (static) |
| **Update Method** | Direct code editing | Direct code editing |
| **Use Case** | Off-plan property tracker | Off-plan property tracker |
| **Payments** | Milestone-based installments | Milestone-based installments |
| **Calculations** | Simple sum (payment001 + payment002 + ...) | Category-based with advance credits |
| **Bootstrap Version** | 5.3 (modern) | 3.4.1 (legacy) |
| **UI Style** | Card-based overview, simple layout | Collapsible sections, detailed tables |
| **Payment Categories** | Numbered (payment001, payment002, ...) | Named (booking, first, second, third) |
| **Currency Display** | AED only | AED + USD dual display |
| **Complexity** | Basic tracking | Advanced (overpayments, advance credits) |

## Development Workflows

### Updating Delphine CONFIG-Based Tracker

**When to use:** Adding new payments, correcting dates, fixing calculations

**Step-by-Step:**

1. **Locate CONFIG object** (~line 820 in DelphineB906.html):
```javascript
const CONFIG = {
  purchasePrice: 3125008,
  usdRate: 0.272,
  registrationFee: 1000,
  payments: [ /* payment array */ ],
  nextPayment: { /* next payment details */ }
};
```

2. **Add new payment to array:**
```javascript
{ category: 'third', amount: 50000, date: '31-Oct-25', receipt: 'De-BR-DE-B906/2025-10-31/R-674446' }
```

**Categories:** `booking`, `first`, `second`, `third`, `registration`

3. **Update nextPayment object** if payment affects next due date:
```javascript
nextPayment: {
  amount: 312500.80,
  date: "24 March 2026",  // Display format
  dueDate: new Date("2026-03-24T00:00:00+04:00")  // Countdown timer
}
```

4. **Verify calculations auto-update** on page load:
- `populateOverview()` - Updates summary cards
- `populateNextPayment()` - Updates next payment section
- `populateCollapsibleSection()` - Populates payment tables

5. **Check payment status table rows:**
- Mark installment as `✅ Paid` only if fully paid
- Use `⏰ Upcoming` if partially paid (has advance credit)
- Row class: `class="highlight"` for paid, `class="upcoming"` for next

**Important:** No save button exists. All calculations run on page load from CONFIG data.

### Adding a New Property
1. Copy an existing modern property file (e.g., `JacobCo_J307.html`)
2. Update localStorage key in JavaScript: `const STORAGE_KEY = 'unitData{PropertyName}'`
3. Create branded version with property-specific:
   - Logo in header (`<img>` in navbar)
   - CSS variables or inline styles for brand colors
   - Footer text with property name

### Modifying Functionality
1. **Identify scope:** Does this affect all properties or just one?
2. **Update base versions first:** Make changes to non-branded files
3. **Sync to branded versions:** Copy functional changes, preserve branding
4. **Test localStorage:** Use browser DevTools → Application → Local Storage

### PDF Generation Pattern
Uses jsPDF library with custom formatting:
```javascript
function generatePDF() {
  const doc = new jsPDF();
  doc.setFontSize(16);
  doc.text("Payment History - Unit " + unitNumber, 14, 20);
  // Table generation with payment history data
  doc.save(`unit_${unitNumber}_history.pdf`);
}
```

## File Organization

### Property Folders
```
{PropertyName}/
  ├── {PropertyName}.html              # Base version
  ├── {PropertyName}_branded.html      # Branded version
  ├── admin_mockup_*.html              # Admin tools (if needed)
  └── Assets/                          # Logos, images for branding
```

### Version Control in Delphine_B906
- `DelphineB906.html` - Current version
- `DelphineB906_backup_modern.html` - Backup before major changes
- `DelphineB906_reverted.html` - Historical rollback version

Shows evolutionary development; useful for understanding design decisions.

## Key Integration Points

### External Dependencies (CDN)
All properties load these from CDN (NO npm/build process):
- Bootstrap CSS/JS
- jQuery
- Font Awesome
- jsPDF
- Google Fonts (Montserrat, Roboto)

**Internet required:** First load only (browser caching handles offline after)

### Data Import/Export
- **Import:** JSON restore from backup (Settings modal)
- **Export:** 
  - PDF per unit (payment history)
  - CSV for all units (admin interfaces or Settings)
- **Backup:** JSON download of entire localStorage

## Common Tasks

### Embedding Background Pattern Across Properties

**Goal:** Apply consistent geometric background pattern to all properties

**Workflow:**
1. **Extract base64 from source file** (usually Jacob branded version):
```powershell
$jacobContent = Get-Content "JacobCo_C422\JacobCo_C422_branded.html" -Raw
if ($jacobContent -match "background:\s*url\('(data:image/png;base64,[^']+)'\)") {
    $base64Pattern = $matches[1]
}
```

2. **Locate target CSS** in destination file (look for `body::before` pseudo-element):
```css
body::before {
  content: '';
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: url('data:image/png;base64,[INSERT_HERE]');
  background-size: cover;
  opacity: 0.05;  /* Always use 5% opacity for subtle effect */
  z-index: -1;
}
```

3. **Insert via PowerShell replacement** (if structure exists but data missing):
```powershell
$targetContent = Get-Content "Delphine_B906\DelphineB906.html" -Raw
$targetContent = $targetContent -replace "background:\s*url\('data:image/png;base64,[^']+'\)", "background: url('$base64Pattern')"
Set-Content "Delphine_B906\DelphineB906.html" -Value $targetContent
```

4. **Verify** opacity is 0.05 and positioning is `fixed` with `z-index: -1`

**Critical:** Always use same base64 source for visual consistency across all properties.

### Payment Calculation Patterns

**Scenario:** Complex payment schedules with advance payments and overpayment credits

**Example from Delphine CONFIG:**
```javascript
// STEP 1: Calculate category totals
const totalBookingPaid = bookingPayments.reduce((sum, p) => sum + p.amount, 0);
const totalFirstPaid = firstPayments.reduce((sum, p) => sum + p.amount, 0);
const totalSecondPaid = secondPayments.reduce((sum, p) => sum + p.amount, 0);
const totalThirdPaid = thirdPayments.reduce((sum, p) => sum + p.amount, 0);

// STEP 2: Calculate overpayment from specific installment
const expectedForSecond = CONFIG.purchasePrice * 0.10; // 10% = 312,500.80
const overpaymentFromSecond = totalSecondPaid > expectedForSecond 
  ? totalSecondPaid - expectedForSecond 
  : 0;

// STEP 3: Calculate total advance credit (available for future payments)
const totalAdvanceCredit = overpaymentFromSecond + totalThirdPaid;

// STEP 4: Calculate remaining amount due for next installment
const expectedForThird = CONFIG.purchasePrice * 0.10;
const remainingForThird = expectedForThird - totalAdvanceCredit;
```

**Key Principles:**
- Separate property payments from administrative fees (SPA registration)
- Track overpayments per installment to apply as credits
- Advance payments count toward future installments
- Display both total credit and remaining balance clearly

**Common Gotcha:** Don't mark installment as "Paid" unless fully paid. Use "⏰ Upcoming" if advance payment exists but doesn't cover full amount.

### Visual Consistency Updates

**Icons & Emojis:**
- Payment Plan headers: `💳 Payment Plan (as per SPA)`
- Next Payment sections: `⏰ Next Payment 💳`
- Installment headers: `🏗️ Third Installment`, `📅 Date`
- Status indicators: `✅ Paid`, `⏰ Upcoming`, `⏳ Pending`

**Date Formatting:**
- Display format: "24 March 2026" (DD Month YYYY)
- JavaScript dates: `new Date("2026-03-24T00:00:00+04:00")` (ISO 8601 with timezone)
- Table dates: "24/03/2026" (DD/MM/YYYY) or "3-Jun-25" (D-Mon-YY)

**Status CSS Classes:**
```css
.highlight { background: #d4edda; } /* Paid installments */
.upcoming { background: #fff3cd; } /* Next due installment */
```

**Apply consistently:** When fixing one property, check others for same pattern.

### Collapsible Payment Sections Pattern

**Structure:** All modern properties use expandable sections for payment details

**HTML Pattern (Delphine as reference):**
```html
<div class="payment-section">
  <div class="payment-header" onclick="toggleSection('sectionId')">
    <h3>💰 Section Title</h3>
    <div class="payment-summary">
      <div class="summary-item">
        <span class="summary-label">Total AED</span>
        <span class="summary-value" id="section-total-aed"></span>
      </div>
      <!-- More summary items -->
      <span class="click-hint">Click to view details</span>
      <span class="expand-icon">▼</span>
    </div>
  </div>
  <div class="payment-content" id="sectionId-content">
    <!-- Table with payment details -->
  </div>
</div>
```

**Critical Elements:**
- `click-hint` must be INSIDE `payment-summary` div, BEFORE `expand-icon`
- Each section needs unique ID for `toggleSection()` function
- Summary items show totals calculated from CONFIG data
- Content div starts hidden, expands on click

**CSS Requirements:**
```css
.click-hint { font-size: 0.85em; opacity: 0.8; color: #fff; margin-left: 10px; }
.expand-icon { font-size: 1.5em; transition: transform 0.3s ease; }
.expanded .expand-icon { transform: rotate(180deg); }
.payment-content { max-height: 0; overflow: hidden; transition: max-height 0.3s ease; }
.payment-content.expanded { max-height: 1000px; }
```

**JavaScript Function:**
```javascript
function toggleSection(sectionId) {
  const content = document.getElementById(sectionId + '-content');
  const header = content.previousElementSibling;
  content.classList.toggle('expanded');
  header.classList.toggle('expanded');
}
```

**Consistency Rule:** When adding collapsible sections, match Delphine's exact structure for visual uniformity.

### Add New Payment Method
1. Locate payment form modal in HTML
2. Add `<option>` to payment method `<select>`
3. Update validation logic in JavaScript if needed
4. Apply to all property files requiring this feature

### Customize PDF Layout
1. Find `generatePDF()` function in JavaScript section
2. Modify jsPDF coordinates and formatting
3. Test with sample data from localStorage

### Add Calculated Field (e.g., Late Fees)
1. Update unit object schema in data storage logic
2. Add UI elements in modals (forms + display tables)
3. Update calculation functions (e.g., `calculateTotalPaid()`)
4. Modify PDF export to include new field

## Testing & Debugging

### Manual Testing Checklist
- Create unit → Add payment → View history → Export PDF
- Backup data → Clear localStorage → Restore from backup
- Test all modals open/close without errors
- Verify calculations (totals, balances, etc.)

### Browser DevTools
- **Console:** Check for JavaScript errors
- **Application → Local Storage:** Inspect/modify data directly
- **Network:** Verify CDN resources load (if online)

### Cross-Property Consistency
When changing core logic, verify consistency across:
1. JacobCo_C422 (base + branded)
2. JacobCo_J307 (base + branded)
3. Delphine_B906 (if applicable to legacy code)

## Conventions to Follow

1. **Inline everything:** No external CSS/JS files (except CDN libraries)
2. **Property isolation:** Each property's data is independent (different localStorage keys)
3. **Responsive design:** All tables and modals must work on mobile
4. **localStorage sync:** Always save after any data mutation
5. **Error handling:** Alert users for storage quota exceeded or parse errors
6. **Date formats:** Use `YYYY-MM` for month selectors, `YYYY-MM-DD` for specific dates

## Quick Reference

**Find property-specific code:**
- Data storage key: Search for `localStorage.getItem`
- Modal definitions: Search for `<div class="modal"`
- Event handlers: Search for `.on('click'` or `addEventListener`

**Common function names:**
- `loadUnits()` - Load data from localStorage
- `saveUnits()` - Persist data to localStorage
- `addUnit()` / `editUnit()` - CRUD operations
- `generatePDF()` - PDF export logic
- `exportCSV()` - CSV export logic

---

## Future Vision: In-Context Editor Architecture

### Current State → Future State
**Today:** Manually edit HTML files to create property trackers  
**Vision:** WYSIWYG in-context editor (like WordPress visual editor or Webflow)

**Core Concept:** Edit the actual tracker itself, not a separate admin interface
- Open existing tracker → Enable edit mode → Hover & click → Changes happen live → Save

### Component Architecture & Rules

#### Component Types with Isolation

**1. Single-Instance Components** (One per tracker)
- Logo, Header, Property Overview Dashboard, Footer
- Once added, can only edit (not duplicate)
- Hover shows: `[✏️ Edit]` or `[↑ Upload New]`

**2. Editable-Only Components** (Click to edit inline)
- Unit Number, Purchase Price, Property Title, Client Name
- Direct text editing, no structural changes
- No + button (not repeatable)

**3. Repeatable Groups** (Unlimited additions)
- Payment installment sections, collapsible payment categories
- **CRITICAL:** Each group is an **isolated container**
- Hover shows: `[+ Add Payment Group]`

**4. Row-Level Items** (Within groups only)
- Table rows for payment entries, receipt references
- Can add/delete/reorder **within parent group only**
- ❌ CANNOT drag rows between different groups

#### Group Isolation Rules

```
📦 Group: "Booking Amount" (Isolated Container)
├── Columns (customizable per this group)
│   ├── Add unlimited columns with type definitions
│   ├── Each column has type: Date, Currency-AED, Status, Text, etc.
│   └── Types enable smart features (auto-sum, countdown, icons)
├── Rows (data entries within this group)
│   ├── Add/edit/delete rows
│   ├── Reorder within THIS group only
│   └── ❌ Cannot move to other groups
└── Calculations (group-scoped)
    ✅ Sum amounts in this group
    ✅ Count rows in this group
    ❌ Groups remain structurally independent
```

### Column Type System

When adding columns to any group, user defines type to enable smart features:

| Column Type | Smart Features Enabled |
|-------------|----------------------|
| **Date** | Countdown timers, date calculations |
| **Currency (AED)** | Auto-sum, totals, percentage calculations |
| **Currency (USD)** | Conversion rates, dual currency display |
| **Percentage** | Visual progress bars, ratio calculations |
| **Status** | Conditional icons (✅ ⏳), color coding |
| **Receipt Reference** | Special formatting, link generation |
| **Text** | Display only, no calculations |

**Example:** Add "Amount (AED)" column with type Currency-AED → System automatically sums all values in that column for group totals.

### Client Self-Service Mode

**Dual-Mode System:**
- **View Mode** (default): Beautiful read-only display
- **Edit Mode** (unlockable): Client can record payments

**Unlock Mechanism:**
- Code format: `{last4_mobile}{unit_number}` (e.g., `5678C422`)
- Stored encrypted in generated HTML
- Help guide appears on unlock

**Smart Payment Recording:**
1. Client enters payment amount (e.g., "323,580 AED")
2. System matches to payment plan installment
3. Auto-identifies: "This matches First Payment / Deposit (10%)"
4. Updates status, recalculates totals, updates next payment countdown

### Implementation Status

**Current:** Manual HTML editing for each property  
**In Development:** In-context editor (see `BRAINSTORM_SESSION.md` at root)  
**Mockups:** `admin_mockup_form_based.html`, `admin_mockup_drag_drop.html` (at root)

**Next Steps:**
1. Create `admin_mockup_in_context.html` - Interactive demo
2. Implement edit mode overlay on existing trackers
3. Build column type system with auto-calculations
4. Add client self-service unlock mechanism

---

*Last updated: November 8, 2025*

**Recent Updates (Nov 8, 2025):**
- Added comprehensive documentation on CONFIG vs localStorage architecture patterns
- Documented Delphine property's CONFIG-based workflow (off-plan property tracker)
- Added background pattern embedding workflow with PowerShell examples
- Documented payment calculation patterns for advance payments and overpayment credits
- Added visual consistency guidelines (icons, emojis, status indicators, date formats)
- Created architectural comparison table between Jacob and Delphine properties
- Added step-by-step guide for updating Delphine CONFIG object
- Added collapsible payment sections pattern with exact HTML/CSS/JS structure
- Documented click-hint positioning requirements for UI consistency

