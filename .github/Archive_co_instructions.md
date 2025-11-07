# AI Coding Agent Instructions - Real Estate Payment Tracker System

## Project Overview
Multi-property payment tracking system for real estate broker managing UAE properties. Each property gets a standalone HTML tracker with embedded JavaScript for real-time calculations, collapsible receipt sections, and offline capability.

**Key Projects:**
- **Jacob&Co Beachfront** (C422, J307) - Modern auto-calculation pattern
- **Delphine Beach Residences** (B906) - Legacy manual pattern  
- **Future:** Generator application for rapid tracker creation

---

## Architecture: Two Patterns

### Modern Pattern (Jacob&Co) ⭐ PREFERRED
**Template:** `JacobCo_C422/JacobCo_C422_branded.html`

**Key Features:**
- Payment array with category-based grouping (`booking`, `registration`)
- Auto-calculation via `getPaymentsByCategory()` and `reduce()`
- Dynamic rendering via `populateCollapsibleSection()`
- Embedded base64 assets (logos, backgrounds)
- Adobe Fonts (arboria): `https://use.typekit.net/ogu0ybk.css`

**CONFIG Pattern:**
```javascript
const CONFIG = {
  purchasePrice: 3235796,
  payments: [
    { category: 'booking', amount: 323580, date: '07-Mar-2025', receipt: '001-MAK-1BR' },
    { category: 'registration', amount: 67554, regFee: 64716, adminFee: 2838 }
  ],
  nextPayment: { amount: 291222, dueDate: new Date("2025-12-10T00:00:00+04:00") }
};
```

**Critical:** Registration fees trigger 3-column layout when `regFee` and `adminFee` properties exist.

### Legacy Pattern (Delphine)
**Template:** `Delphine_B906/DelphineB906.html`

**Characteristics:**
- Manual `totalPaid` and `overpayment` fields in CONFIG
- Receipt tracking in static HTML tables
- Extensive inline comments for client self-service
- Dual currency (AED/USD) throughout

**Upgrade Path:** Migrate to modern pattern (see `.github/BRAINSTORM_SESSION.md`)

---

## Critical Calculation Rules

### Registration Fees (Jacob&Co Pattern)
**ALWAYS:** Registration fee = 2% of purchase price + 2,838 AED admin fee
```javascript
regFee = purchasePrice * 0.02  // e.g., 3,235,796 × 0.02 = 64,716
adminFee = 2838  // Fixed
total = regFee + adminFee  // 67,554
```

**Important:** Registration fees are EXCLUDED from property payment percentages.

### Auto-Calculation Logic
```javascript
const bookingPayments = getPaymentsByCategory('booking');
const totalBookingPaid = bookingPayments.reduce((sum, p) => sum + p.amount, 0);
const paidTowardsProperty = totalBookingPaid;  // Excludes registration
const remainingBalance = CONFIG.purchasePrice - paidTowardsProperty;
const percentagePaid = (paidTowardsProperty / CONFIG.purchasePrice) * 100;
```

**Never manually set totals** - all calculations derive from `CONFIG.payments` array.

---

## File Structure & Naming

```
PropertyName_UnitNumber/
├── PropertyNameUnitNumber_branded.html  # Main (modern pattern)
├── PropertyNameUnitNumber.html          # Alternative version
├── README.md                            # Property documentation
├── Assets/                              # Logos, backgrounds (for reference)
├── Downpayment/                         # Receipt PDFs (ignored by git)
├── First Installment/
└── Second Installment/
```

**Conventions:**
- HTML: `DelphineB906.html` (no spaces)
- Receipt PDFs: `R-674446_50K.pdf` (Receipt#_Amount)
- Folders: Use spaces (`First Installment/`)
- Jacob&Co receipts: Simple codes (`REG-C-422`, `001-MAK-1BR`)
- Delphine receipts: Full path format (`De-BR-DE-B906/2025-11-04/R-674446`)

---

## Common Workflows

### Adding New Payment
1. Get actual receipt PDF from client (never invent placeholder numbers)
2. Save PDF in correct installment folder
3. Add to `CONFIG.payments` array:
   ```javascript
   { category: 'booking', amount: 50000, date: '04-Nov-2025', receipt: 'R-674446' }
   ```
4. Auto-calculation updates all totals, percentages, remaining balance
5. Test by opening HTML in browser

### Creating New Property Tracker
1. Copy `JacobCo_C422_branded.html` as template
2. Search/replace: unit number, tower, property name
3. Update `CONFIG.purchasePrice` from SPA document
4. Calculate registration: `purchasePrice * 0.02 + 2838`
5. Build payment plan from SPA milestones (e.g., "6 months from booking")
6. Create installment folders
7. Generate README.md with payment summary

### Backup Strategy
Before major changes, create: `PropertyNameUnitNumber_backup_modern.html`

Backup when:
- Adding new collapsible sections
- Bulk updates to multiple installments
- After completing working version

---

## Data Validation Rules

### Receipt Verification
- Cross-check receipt numbers against actual PDF files
- Date in HTML must match PDF filename date
- Example: `R-674446_50K.pdf` → HTML shows `4-Nov-25` → Receipt ref matches

### Calculation Checks
- Total Paid = sum of all payments (excluding registration)
- Overpayment = `(totalPaid - registrationFee) - (purchasePrice × percentagePaid)`
- Receipt count in UI must match actual PDFs in folders
- Registration fees always tracked separately

### Date Formats (UAE timezone +04:00)
- **HTML display:** `4-Nov-25` (DD-MMM-YY)
- **Receipt reference:** `2025-11-04` (YYYY-MM-DD)
- **JavaScript:** `new Date("2026-03-24T00:00:00+04:00")`

---

## Conditional Rendering Patterns

### Registration Table (3-column)
Triggered when: `category === 'registration' && p.regFee && p.adminFee`
```html
<th>Registration Fee (2%)</th><th>Admin Fee</th><th>Total (AED)</th>
```

### Standard Payment Table
```html
<th>Date</th><th>Amount (AED)</th><th>Status</th><th>Receipt Reference</th>
```

### Payment Status Icons
- `✅ Paid` - Fully paid
- `✅ Paid (Covered by Overpayment Credit)` - Overpayment applied
- `⏳ Upcoming` / `⏳ Pending` - Future

---

## Testing & Preview

```powershell
# Open in browser
Start-Process "path\to\PropertyNameUnitNumber.html"
```

**Key Features to Test:**
- Countdown timer updates every second
- Collapsible sections toggle smoothly
- All calculations accurate to 2 decimal places
- Mobile responsive (tables collapse, summaries stack)
- Dual currency display (AED primary, USD secondary)

---

## Active Development: Generator Application

**Status:** Active development (see `JacobCo_C422/.github/BRAINSTORM_SESSION.md`)

**Vision:** Form-based wizard → generates custom trackers → client self-service mode

**Tech Stack (Planned):**
- Admin: Python Flask + SQLite + React/Vue
- Templates: Multi-project library (Jacob&Co, Delphine, custom)
- Client Access: View-only → Unlockable self-service

**Active Mockups:**
- `JacobCo_C422/admin_mockup_form_based.html` - Wizard-style form interface
- `JacobCo_C422/admin_mockup_drag_drop.html` - Drag-drop builder (Phase 2)

**Reference Files:**
- Modern Pattern: `JacobCo_C422_branded.html`
- Full Planning: `JacobCo_C422/.github/BRAINSTORM_SESSION.md`

---

## DO NOT

❌ Invent placeholder receipt numbers - always verify PDFs  
❌ Mix payment categories - registration stays separate  
❌ Manually set totals in modern pattern - use payment array  
❌ Modify `CONFIG.purchasePrice` without SPA confirmation  
❌ Count registration fees toward installment percentages  
❌ Commit PDFs to git (see `.gitignore`)

---

## Project Context

**Created by:** SANOOP SYAMALAN, Associate Director, Kennedy Property Brokers LLC  
**Purpose:** Client-facing transparency tools with self-service capability  
**Clients:** Real estate investors in UAE luxury developments
