# AI Coding Agent Instructions - Real Estate Payment Trackers

## Project Overview
This codebase manages payment tracking for real estate properties (Siniya Island & Jacob&Co Beachfront). Each property has standalone HTML payment trackers with embedded JavaScript for real-time calculations and collapsible receipt sections.

## Architecture Pattern

### Two Tracker Architectures

**1. Delphine (Legacy Pattern)** - `Delphine_B906/DelphineB906.html`
- Single CONFIG object with totalPaid, overpayment calculations
- Dual currency (AED/USD) display throughout
- Manual receipt tracking in HTML tables
- Extensive inline comments in CONFIG for client self-service
- Manual calculation: Update `totalPaid` and `overpayment` fields directly

**2. Jacob&Co (Modern Pattern)** - `JacobCo_C422_branded.html` / `JacobCo_J307_branded.html` ⭐ **Preferred**
- Payment array with category-based grouping (`booking`, `registration`)
- Dynamic table rendering via `populateCollapsibleSection()`
- Conditional rendering: Registration fees get special 3-column layout
- **Preferred Font:** "arboria" from Adobe Fonts (Typekit)
  - Link: `https://use.typekit.net/ogu0ybk.css`
  - CSS: `font-family: "arboria", sans-serif;`
- Single-file with embedded base64 brand assets (logos, background images)
- **Auto-calculation system** - totals derived from payment array, not manual entry
- Cleaner CONFIG object focused on data, not manual totals

### Directory Structure
```
PropertyName_UnitNumber/
├── PropertyNameUnitNumber_branded.html  # Main tracker (Jacob&Co pattern)
├── PropertyNameUnitNumber.html          # Alternative/backup version
├── README.md                            # Property documentation
├── Downpayment/                         # Receipt PDFs organized by installment
├── First Installment/
├── Second Installment/
└── Third Installment/
```

## Critical Patterns

### 1. CONFIG Object - Jacob&Co Pattern (Recommended)
```javascript
const CONFIG = {
  purchasePrice: 3235796,
  
  payments: [
    { category: 'booking', amount: 323580, date: '07-Mar-2025', receipt: '001-MAK-1BR' },
    { category: 'registration', amount: 67554, date: '06-Nov-2025', receipt: 'REG-C-422', 
      regFee: 64716, adminFee: 2838 }  // Special: breakdown for registration
  ],
  
  nextPayment: {
    amount: 291222,
    date: "10 December 2025",
    dueDate: new Date("2025-12-10T00:00:00+04:00")
  }
};
```

**Key insight:** Registration fees use `regFee` and `adminFee` properties to trigger special 3-column table rendering.

### 2. Registration Fee Calculations
**CRITICAL RULE:** Registration fee = **2% of purchase price** (not manually entered)
- C422 example: 3,235,796 × 0.02 = 64,715.92 ≈ 64,716 AED
- Admin fee: Fixed 2,838 AED
- Total = regFee + adminFee

**Pattern in code:**
```javascript
{ category: 'registration', amount: 67554, regFee: 64716, adminFee: 2838 }
```

### 3. Auto-Calculation Logic (Jacob&Co Pattern)
**CRITICAL:** All totals are auto-calculated from CONFIG.payments array - never manually set.

```javascript
// Filter payments by category
const bookingPayments = getPaymentsByCategory('booking');
const registrationPayments = getPaymentsByCategory('registration');

// Auto-calculate totals using reduce
const totalBookingPaid = bookingPayments.reduce((sum, p) => sum + p.amount, 0);
const totalRegistrationPaid = registrationPayments.reduce((sum, p) => sum + p.amount, 0);

// Separate registration from property payments
const paidTowardsProperty = totalBookingPaid;
const totalPaid = paidTowardsProperty + totalRegistrationPaid;

// Calculate remaining and percentage
const remainingBalance = CONFIG.purchasePrice - paidTowardsProperty;
const bookingPercentage = (paidTowardsProperty / CONFIG.purchasePrice) * 100;
```

**Key principle:** Update only `CONFIG.payments` array - everything else recalculates automatically.

### 4. Conditional Table Rendering
The `populateCollapsibleSection()` function handles two layouts:

**Registration table** (when `category === 'registration' && p.regFee && p.adminFee`):
```html
<th>Date</th><th>Registration Fee (2%)</th><th>Admin Fee</th><th>Total (AED)</th><th>Status</th><th>Receipt</th>
```

**Standard table** (booking/installments):
```html
<th>Date</th><th>Amount (AED)</th><th>Status</th><th>Receipt Reference</th>
```

### 5. Receipt Reference Format
**Jacob&Co:** Simple receipt codes (e.g., `REG-C-422`, `001-MAK-1BR`)
**Delphine:** Full path format `De-BR-DE-B906/2025-11-04/R-674446`
### 6. Collapsible Payment Sections
Each installment section uses this pattern:
```html
<div class="payment-section">
  <div class="payment-header" onclick="toggleSection('sectionId')">
    <h3>Icon Installment Name</h3>
    <div class="payment-summary">
      <!-- Summary items with labels and values -->
    </div>
    <span class="expand-icon">▼</span>
  </div>
  <div class="payment-content" id="sectionId-content">
    <!-- Detail table with receipts -->
  </div>
</div>
```

### 7. Payment Status Indicators
- `✅ Paid` - Fully paid installments
- `✅ Paid (Covered by Overpayment Credit)` - When overpayment covers future installment
- `⏳ Upcoming` / `⏳ Pending` - Future installments

## Common Tasks

### Adding New Payment
1. **Get actual receipt PDF** from client - check file name for receipt number
2. **Save PDF** in correct installment folder (e.g., `Third Installment/R-674446_50K.pdf`)
3. **Update Payment Summary table** - add new row or update existing installment total
4. **Update collapsible section header** - summary values (Total AED, Total USD, receipt count, percentage)
5. **Add detail row** in payment content table with exact receipt reference
6. **Update CONFIG.totalPaid** - add new payment amount
7. **Recalculate overpayment** if needed
8. **Update overview section** - Total Paid, Remaining Balance, Overpayment Credit

### Creating New Property Tracker
1. **Copy** `JacobCo_C422_branded.html` as template (modern pattern)
2. **Search/replace** property identifiers (unit number, tower, property name)
3. **Update CONFIG** with new property's purchase price and payment plan
4. **Modify payment plan table** to match SPA (Sales & Purchase Agreement)
   - **Reference SPA document** for exact installment percentages, amounts, and due dates
   - Payment milestones must match SPA terms (e.g., "6 months from booking", "On Completion")
5. **Calculate registration fee:** Always 2% of purchase price + 2,838 AED admin fee
6. **Create installment folders** matching the payment plan
7. **Update receipt references** as payments come in
8. **Create README.md** documenting the property

### Backup Strategy
Create backups before major changes using this naming convention:
- `PropertyNameUnitNumber_backup_modern.html` - Full feature backup
- Make backup when:
  - Adding new collapsible sections or major structural changes
  - Before bulk updates to multiple installment sections
  - After completing a working version with all current payments
- Keep at least one working backup per property

### Color Scheme Variations
Two standard themes:
- **Green theme:** `#1a4c5d` (teal) to `#2c5f2d` (green) gradients - main version
- **Teal/Purple theme:** `#009688` (teal) to `#3f51b5` (indigo) gradients - reverted version

## File Naming Conventions
- **HTML:** `PropertyNameUnitNumber.html` (e.g., `DelphineB906.html`)
- **Receipt PDFs:** `R-[number]_[amount]K.pdf` (e.g., `R-674446_50K.pdf`)
- **Folders:** Use spaces in folder names (e.g., `First Installment/`)

## Data Validation Rules
- **Always cross-check** receipt numbers against actual PDF files before updating HTML
- **Verify dates match:** Receipt date in HTML must exactly match the date in the PDF filename and receipt reference
  - Example: `R-674446_50K.pdf` → HTML shows `4-Nov-25` → Receipt ref: `De-BR-DE-B906/2025-11-04/R-674446`
- **Verify calculations:** Total Paid = sum of all installment payments (excluding registration fees)
- **Overpayment formula:** `(totalPaid - registrationFee) - (purchasePrice × percentagePaid)`
- **Receipt count must match** actual PDFs in folders
- **Registration fees are separate:** Always exclude from installment percentage calculations and track separately

## Critical Date Formats
- **HTML display:** `4-Nov-25` (DD-MMM-YY format)
- **Receipt reference:** `2025-11-04` (YYYY-MM-DD format)
- **JavaScript Date:** `new Date("2026-03-24T00:00:00+04:00")` (ISO format with UAE timezone)

## Preview & Testing
Use PowerShell to open in browser:
```powershell
Start-Process "path\to\file.html"
```

## Key Implementation Details
- **Countdown timer:** Updates every second using `setInterval(updateCountdown, 1000)`
- **Date format:** Use UAE timezone `+04:00` for due dates
- **Percentage calculations:** Round to 2 decimal places for display
- **Mobile responsive:** Tables collapse, summaries stack vertically on small screens

## DO NOT
- ❌ Invent placeholder receipt numbers - always get real receipt number from PDF
- ❌ Mix installments - each payment belongs to specific installment per payment plan
- ❌ Forget to update both main and reverted versions when making changes
- ❌ Modify CONFIG.purchasePrice without client confirmation
- ❌ Count registration fees toward installment percentages

## Project Context
Created by: SANOOP SYAMALAN, Associate Director, Kennedy Property Brokers LLC
Client-facing tools designed for transparency and self-service payment tracking.

---

## 🚀 FUTURE DEVELOPMENT: Generator Application

### Vision
Transform static HTML trackers into a dynamic generator application with client self-service capabilities.

### Architecture (Planned)
- **Admin Interface:** Form-based wizard (MVP) → Drag-drop builder (Phase 2)
- **Technology Stack:** Python Flask + SQLite + React/Vue
- **Template System:** Multi-project templates (Jacob&Co, Delphine, custom)
- **Client Access:** Dual-mode (View-only → Unlockable self-service)

### Key Features (Roadmap)
1. **Property Generator Wizard**
   - Step 1: Select template
   - Step 2: Property details (unit, price, client mobile)
   - Step 3: Payment plan editor
   - Step 4: Branding customization
   - Step 5: Generate HTML + PDF guide

2. **Client Self-Service Mode**
   - Unlock code: `{last4_mobile}{unit_number}`
   - Smart payment recording (auto-match installments)
   - localStorage persistence
   - "Download Updated Tracker" feature

3. **Template Library**
   - Pre-built templates per project
   - Reusable assets (logos, color schemes)
   - Version tracking for template evolution

### Development Status
- **Phase:** Planning Complete ✅
- **Next:** Upgrade Delphine to modern pattern
- **Timeline:** 1 week for MVP
- **Documentation:** See `.github/BRAINSTORM_SESSION.md` for full details

### Reference Files
- Modern Pattern: `JacobCo_C422_branded.html`
- Legacy Pattern (to be upgraded): `Delphine_B906/DelphineB906.html`
- Brainstorm Session: `.github/BRAINSTORM_SESSION.md`
