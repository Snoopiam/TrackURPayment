# AI Coding Agent Instructions - Siniya Island Real Estate Payment Trackers

## Project Overview
This codebase manages payment tracking for real estate properties on Siniya Island. Each property has standalone HTML payment trackers with embedded JavaScript for real-time calculations and collapsible receipt sections.

## Architecture Pattern

### Single-File HTML Trackers
Each property unit has self-contained HTML files with:
- **Embedded CSS** for styling (gradient backgrounds, collapsible sections, responsive design)
- **Embedded JavaScript** with CONFIG object for easy updates
- **No external dependencies** - works offline, can be emailed directly to clients

**Example:** `Delphine_B906/DelphineB906.html` is the canonical pattern to follow.

### Directory Structure
```
PropertyName_UnitNumber/
├── PropertyNameUnitNumber.html       # Main tracker
├── PropertyName_reverted.html        # Alternative color scheme version
├── PropertyName_backup_modern.html   # Backup copy
├── README.md                         # Property documentation
├── Downpayment/                      # Receipt PDFs organized by installment
├── First Installment/
├── Second Installment/
└── Third Installment/
```

## Critical Patterns

### 1. Receipt Reference Format
**Always follow this exact format:** `De-BR-DE-B906/2025-11-04/R-674446`
- Property code / Date / Receipt number
- **NEVER** make up placeholder receipt numbers (e.g., R-ADV001)
- **ALWAYS** check the actual PDF files in installment folders to get real receipt numbers

### 2. CONFIG Object (JavaScript)
Located in `<script>` section with extensive comments for client self-service:
```javascript
const CONFIG = {
  totalPaid: 1391410,              // Update when payment made
  purchasePrice: 3125008,          // Fixed - don't change
  overpayment: 140406.81,          // Auto-calculated credit
  nextInstallmentAmount: 312500.80,
  nextPaymentDate: "24 March 2026",
  nextPaymentDueDate: new Date("2026-03-24T00:00:00+04:00"),
  usdRate: 0.272                   // AED to USD conversion
};
```

### 3. Collapsible Payment Sections
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

### 4. Dual Currency Display
All amounts shown in **both AED and USD**:
- Primary: AED (bold)
- Secondary: USD in `.usd-amount` class (styled in blue, 0.9em)
- Conversion rate stored in CONFIG.usdRate

### 5. Payment Status Indicators
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
1. **Copy** `Delphine_B906/DelphineB906.html` as template
2. **Search/replace** property identifiers (unit number, tower, property name)
3. **Update CONFIG** with new property's purchase price and payment plan
4. **Modify payment plan table** to match SPA (Sales & Purchase Agreement)
   - **Reference SPA document** for exact installment percentages, amounts, and due dates
   - Payment milestones must match SPA terms (e.g., "6 months from booking", "On Completion")
5. **Create installment folders** matching the payment plan
6. **Update receipt references** as payments come in
7. **Create README.md** documenting the property

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
