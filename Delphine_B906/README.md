# Delphine Beach Residences - Tower B Unit 906

## 🏢 Property Information

**Project:** Delphine Beach Residences - Siniya Island  
**Unit:** Tower B - Unit 906  
**Purchase Price:** AED 3,125,008 (USD 850,000)  
**SPA Registration Fee:** AED 1,000 (USD 272)

---

## 📊 Payment Summary

| Installment | Status | Amount (AED) | Amount (USD) | Receipts | Percentage |
|-------------|--------|--------------|--------------|----------|------------|
| **Down Payment** | ✅ Paid | 623,650 | 169,633 | 8 | 20.00% |
| **First Installment** | ✅ Paid | 318,000 | 86,496 | 4 | 10.20% |
| **Second Installment** | ✅ Paid | 398,760 | 108,463 | 5 | 12.79% |
| **Third Installment** | 🟡 Partial | 50,000 | 13,600 | 1 | 1.60% |
| **SPA Registration** | ✅ Paid | 1,000 | 272 | 1 | - |
| **Total Paid** | - | **1,391,410** | **378,464** | **19** | **44.59%** |

### Current Status (as of Nov 4, 2025)
- **Total Paid:** AED 1,391,410 (USD 378,464)
- **Remaining Balance:** AED 1,734,598 (USD 471,811)
- **Overpayment Credit:** AED 140,406.80 (USD 38,191)
- **Next Payment Due:** AED 172,094 (March 24, 2026)

---

## 📁 Payment Receipt Files

### Down Payment (8 receipts)
Located in: `Downpayment/`

| Date | Amount (AED) | Receipt Number | File |
|------|--------------|----------------|------|
| 29-Sep-24 | 65,000 | R-236837 | R-236837_65K.pdf |
| 02-Oct-24 | 110,000 | R-237467 | R-237467_110K.pdf |
| 03-Oct-24 | 102,000 | R-238196 | R-238196_102K.pdf |
| 04-Oct-24 | 37,000 | R-238479 | R-238479_37K.pdf |
| 08-Oct-24 | 124,500 | R-238503 | R-238503_124,5K.pdf |
| 08-Oct-24 | 92,000 | R-238507 | R-238507_92K.pdf |
| 09-Oct-24 | 38,050 | R-238677 | R-238677_38.5k.pdf |
| 10-Oct-24 | 55,100 | R-238696 | R-238696_55,1K.pdf |
| **Total** | **623,650** | - | - |

### First Installment (4 receipts)
Located in: `First Installment/`

| Date | Amount (AED) | Receipt Number | File |
|------|--------------|----------------|------|
| 15-Oct-24 | 110,000 | R-240527 | R-240527_110K.pdf |
| 26-Nov-24 | 54,000 | R-443077 | R-443077_54K.pdf |
| 26-Nov-24 | 54,000 | R-443081 | R-443081_54K.pdf |
| 07-Jan-25 | 100,000 | R-456514 | R-456514_100K.pdf |
| **Total** | **318,000** | - | - |

### Second Installment (5 receipts)
Located in: `Second Installment/`

| Date | Amount (AED) | Receipt Number | File |
|------|--------------|----------------|------|
| 04-Feb-25 | 100,000 | R-480841 | R-480841_100K.pdf |
| 15-Feb-25 | 58,760 | R-486342 | R-486342_58.5K.pdf |
| 05-Mar-25 | 65,000 | R-493793 | R-493793_65K.pdf |
| 23-Apr-25 | 75,000 | R-527323 | R-527323_75K.pdf |
| 03-Jun-25 | 100,000 | R-596092 | R-596092_100K.pdf |
| **Total** | **398,760** | - | - |

### Third Installment (1 receipt)
Located in: `Third Installment/`

| Date | Amount (AED) | Receipt Number | File |
|------|--------------|----------------|------|
| 04-Nov-25 | 50,000 | R-674446 | R-674446_50K.pdf |
| **Total** | **50,000** | - | - |

### SPA Registration Fee (1 receipt)
Located in: `Second Installment/` *(Administrative fee - separate from installments)*

| Date | Amount (AED) | Receipt Number | File |
|------|--------------|----------------|------|
| 08-May-25 | 1,000 | R-598157 | R-598157_1K_Registration.pdf |

---

## 📅 Payment Plan Schedule

| Installment | Percentage | Amount (AED) | Due Date | Milestone | Status |
|-------------|------------|--------------|----------|-----------|--------|
| Booking Amount | 20% | 625,001.59 | 30/09/2024 | On Booking | ✅ Paid |
| First Installment | 10% | 312,500.80 | 29/03/2025 | 6 months from booking | ✅ Paid |
| Second Installment | 10% | 312,500.80 | 25/09/2025 | 12 months from booking | ✅ Paid |
| **Third Installment** | 10% | 312,500.80 | **24/03/2026** | 18 months from booking | 🟡 **Partial** |
| Fourth Installment | 10% | 312,500.80 | 20/09/2026 | 24 months from booking | ⏳ Pending |
| Final Installment | 40% | 1,250,003.18 | TBD | On Completion | ⏳ Pending |

---

## 📄 HTML Tracker Files

### Main Files
- **`DelphineB906.html`** - Primary payment tracker with collapsible sections and real-time countdown
- **`DelphineB906_reverted.html`** - Alternative version with different color scheme (teal/purple)
- **`DelphineB906_backup_modern.html`** - Backup version

### Features
- ✅ Real-time countdown to next payment deadline
- ✅ Collapsible payment detail sections
- ✅ Automatic overpayment calculation
- ✅ Complete receipt reference tracking
- ✅ AED/USD dual currency display
- ✅ Responsive mobile-friendly design

---

## 🔄 How to Update Payment Information

### Method 1: Manual Update (Easy)
Open the HTML file and locate the **EASY UPDATE SECTION** in the JavaScript:

```javascript
const CONFIG = {
  totalPaid: 1391410,        // Update this when new payment is made
  purchasePrice: 3125008,    // Don't change
  overpayment: 140406.81,    // Update if overpayment changes
  nextInstallmentAmount: 312500.80,
  nextPaymentDate: "24 March 2026",
  nextPaymentDueDate: new Date("2026-03-24T00:00:00+04:00"),
  usdRate: 0.272
};
```

### Method 2: Adding New Payment Receipt
1. Save the PDF receipt in the appropriate folder (e.g., `Third Installment/`)
2. Update the HTML payment section with the new receipt details
3. Update the `totalPaid` amount in CONFIG
4. Recalculate overpayment if needed

---

## 💡 Important Notes

1. **Overpayment Credit:** The current overpayment of AED 140,406.80 will be applied toward the Third Installment, reducing the remaining balance to AED 172,094.
2. **Registration Fee:** The AED 1,000 SPA registration fee is separate and does not count toward the property purchase price milestones.
3. **USD Conversion:** All USD amounts are calculated at the rate of 1 AED = 0.272 USD (approximate).
4. **Next Due Date:** Third Installment balance due on March 24, 2026.

---

## 📞 Contact Information

**Created by:** SANOOP SYAMALAN  
**Position:** ASSOCIATE DIRECTOR  
**Company:** KENNEDY PROPERTY BROKERS LLC

---

## 🗂️ Folder Structure

```
Delphine_B906/
├── DelphineB906.html (Main tracker)
├── DelphineB906_reverted.html (Alternative version)
├── DelphineB906_backup_modern.html (Backup)
├── README.md (This file)
├── Downpayment/
│   └── [8 receipt PDFs]
├── First Installment/
│   └── [4 receipt PDFs]
├── Second Installment/
│   ├── [5 payment receipt PDFs]
│   └── [1 registration fee PDF]
└── Third Installment/
    └── [1 receipt PDF]
```

---

**Last Updated:** November 4, 2025
