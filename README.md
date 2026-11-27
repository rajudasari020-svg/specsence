# SpecSense – BestBuy Timing Hack

> **Cross-Retailer Price Intelligence & Technical Specification Benchmarking Platform**  
> Built with React, Vite, Tailwind CSS, Node.js, Express, and persistent SQLite.

---

## 🏆 Live Hackathon Demonstration Walkthrough

SpecSense is pre-configured with a live demonstration scenario comparing **OnePlus 12** and **Google Pixel 8**.

### Demo Execution Script (3 Minutes):

1. **Launch the Application**:
   ```bash
   node backend/server.js
   ```
   Open **`http://localhost:5000`** in any browser.

2. **Launch the 1-Click Comparison Demo**:
   - In the top header bar, click **`Demo: OnePlus 12 vs Pixel 8`** (or the blue banner on the homepage).
   - This opens the side-by-side comparison of **OnePlus 12 (256GB)** and **Google Pixel 8 (128GB)**.

3. **Demonstrate Specification & Timing Intelligence**:
   - **Specification Difference Highlighting**: Click the **"Differences Only"** switch to dim identical specs and highlight variances.
   - **Performance Contrast**: Note the benchmark score (OnePlus 12: `2,110,000` with Snapdragon 8 Gen 3 vs. Pixel 8: `1,180,000` with Tensor G3).
   - **Value-for-Money Index**: OnePlus 12 scores **84/100 (Exceptional Value)**; Google Pixel 8 scores **66/100 (Moderate Value)**.
   - **Recommendation Contrast**:
     - **OnePlus 12**: Classified as **`GREAT DEAL – BUY NOW`** (Current price ₹64,999 is **6.3% below its 90-day average** of ₹69,349 and within 2.4% of its 90-day low).
     - **Google Pixel 8**: Classified as **`FAIR PRICE`** (Current price ₹61,999 is hovering **-0.9%** within its normal baseline).

4. **Demonstrate 90-Day Interactive Financial Charts**:
   - Navigate to **`Price Tracker`** (or click "Price Trend" under OnePlus 12).
   - Show the 3 retailer series: **Amazon** (orange), **Flipkart** (blue), **Direct Brand Store** (green).
   - Toggle retailer lines on/off.
   - Switch date horizons between **30D**, **60D**, and **90D**.
   - Hover to inspect exact daily verified prices against the **90-Day Moving Average reference line** and the **Historical Low marker**.

5. **Demonstrate Live Price Drop Alert Triggering**:
   - Click **`Price Alerts`** in the top navigation.
   - Show the pre-configured **Active Alert** on **Google Pixel 8** with a target threshold of **₹58,000** (Current: ₹61,999).
   - Click the blue button: **`Trigger Pixel 8 Flash Drop`**.
   - **Watch Real-Time Execution**:
     - The backend applies a 10% flash price reduction to ₹55,799.
     - The alert condition `current_price <= target_price` evaluates to true.
     - Alert status updates from `ACTIVE` to `TRIGGERED`.
     - An instant **notification banner** toasts onto the screen.
     - The top navigation **bell notification counter** updates in real time.

6. **Demonstrate PDF Export**:
   - Return to **`Compare Specs`** and click **`Export PDF Report`** to generate a formatted printable report.

---

## 1. Project Overview

**SpecSense – BestBuy Timing Hack** helps consumers make smarter electronics purchasing decisions by eliminating guesswork and countering retailer surge pricing:
- Comparing technical specifications across 14 hardware parameters.
- Tracking 90 days of empirical daily price points across Amazon, Flipkart, and Direct Brand Stores.
- Calculating 30-day and 90-day moving averages (MA).
- Formulating rule-based purchase recommendations (**GREAT DEAL – BUY NOW**, **FAIR PRICE**, **WAIT – LIKELY TO DROP**).
- Monitoring target-price alerts with live simulated trigger testing.
- Scoring devices with a category-weighted Value-for-Money (VFM) model (0–100).

---

## 2. Technology Stack

### Frontend
- **Framework**: React 18 with Vite
- **Styling**: Tailwind CSS ("Clean Commerce" visual language: `#F8FAFC` background, `#FFFFFF` cards, `#0F172A` text, `#1D4ED8` primary accent, `#15803D` deal green, `#B45309` warning amber)
- **Routing**: React Router v7
- **Charts**: Recharts (multi-series line charts, custom tooltips, moving average reference lines)
- **Icons**: Lucide React
- **Exporting**: html2canvas + jsPDF (Formatted PDF comparison export)

### Backend
- **Runtime**: Node.js (v20+ / v26)
- **Web Framework**: Express.js REST APIs
- **Database**: SQLite with `sql.js` (WebAssembly-backed persistent on-disk database)
- **Validation**: Zod schema validation
- **HTTP Client**: Axios

---

## 3. Mathematical Formulas & Algorithmic Design

### A. Moving Average Calculation
$$\text{MA}(n) = \frac{1}{n} \sum_{i=1}^{n} \text{Price}_i$$
Calculated for $n = 30$ days and $n = 90$ days across multi-retailer daily records.

### B. Price Deviation Percentage
$$\text{Deviation}_{90} (\%) = \left(\frac{\text{Current Price} - \text{MA}_{90}}{\text{MA}_{90}}\right) \times 100$$

### C. Rule-Based Purchase Timing Algorithm
1. **GREAT DEAL – BUY NOW**:
   $$\text{Deviation}_{90} \le -5.0\% \quad \text{AND} \quad \frac{\text{Current Price} - \text{Low}_{90}}{\text{Low}_{90}} \le 10.0\%$$
2. **WAIT – LIKELY TO DROP**:
   $$\text{Deviation}_{90} > +5.0\%$$
3. **FAIR PRICE**:
   $$-5.0\% < \text{Deviation}_{90} \le +5.0\%$$

### D. Value-For-Money (VFM) Index (0–100)
Normalizes hardware parameters against category median price points:
$$\text{Score} = w_{\text{cpu}} S_{\text{cpu}} + w_{\text{disp}} S_{\text{disp}} + w_{\text{cam}} S_{\text{cam}} + w_{\text{batt}} S_{\text{batt}} + w_{\text{eff}} S_{\text{eff}}$$
- **Smartphones**: CPU 25%, Display 20%, Camera 20%, Battery 15%, Price Efficiency 20%
- **Laptops**: CPU 35%, Display 20%, Price Efficiency 25%, Battery 15%, Webcam 5%
- **Headphones**: Audio/ANC 20%, Battery 25%, Chip 20%, Price Efficiency 30%
- **Smartwatches**: Display 25%, Sensors 15%, Battery 20%, Chip 20%, Price Efficiency 20%

---

## 4. Database Schema

- **`products`**: 16 devices across Smartphones, Laptops, Headphones, Smartwatches with all 14 specification parameters.
- **`retailers`**: Amazon, Flipkart, Direct Brand Store.
- **`price_history`**: 4,320 records (16 products x 3 retailers x 90 daily records).
- **`price_alerts`**: Persistent target price alerts with `ACTIVE` and `TRIGGERED` status.
- **`sales_events`**: Upcoming scheduled and historically recurring sales calendar events.

---

## 5. Quick Start Instructions

```bash
# 1. Install Dependencies
npm install --prefix backend
npm install --prefix frontend

# 2. Seed SQLite Database
node backend/database/seed.js

# 3. Build Production Frontend
npm run build --prefix frontend

# 4. Start Unified Server
node backend/server.js
```
Open **`http://localhost:5000`** in your browser.
