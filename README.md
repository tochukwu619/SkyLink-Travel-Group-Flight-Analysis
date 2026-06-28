# SkyLink Travel Group — Flight Booking Analysis

**Tool:** Power BI (Power Query + DAX) · **Records:** 10,000 bookings · **Period:** Jan 2018 – Dec 2019 · **Total Revenue:** $8,370,986.68

A Power BI–based analysis of two years of flight booking data, built to give SkyLink Travel Group leadership visibility into route profitability, traveller demand, airline partner performance, and booking channel effectiveness.


[Interact with the live dashboard here](https://app.powerbi.com/view?r=eyJrIjoiNzgyZmVjODQtYzBlYi00ZDU1LWIyZGQtYzA4N2E5MTJiMGE1IiwidCI6ImRlYWNhOWI3LWUzMTktNDI1MS1iZTQ2LTU4MWNmODI5ODFkMiJ9&pageName=0b82239a43d1720bff34)

---

## Table of Contents
1. [Objectives](#objectives)
2. [Data Cleaning & Preparation](#data-cleaning--preparation)
3. [Key Findings & Insights](#key-findings--insights)
4. [Conclusion](#conclusion)
5. [Recommendations](#recommendations)

---

## Objectives

This analysis was commissioned to answer four leadership questions:

| # | Objective | Business Question |
|---|---|---|
| 1 | **Route profitability** | Which routes and connection patterns drive (or drag down) revenue? |
| 2 | **Airline partner performance** | Which carriers contribute the most revenue and yield, and which deserve renegotiation? |
| 3 | **Traveller demand & preferences** | Who is flying, why, in what class, and how far in advance? |
| 4 | **Booking channel effectiveness** | Which agencies/channels contribute the most revenue, and how evenly is that contribution spread? |

The end goal is a set of data-backed recommendations for **route optimisation, revenue growth, and demand planning**.

---

## Data Cleaning & Preparation

**Source structure:** a single flat table of 10,000 booking records, 22 attributes wide (booking metadata, traveller identity, airline/class/route detail, trip-type flags, and ticket amount).

**Modelling steps performed in Power BI:**
- Split the flat table into a star-style schema: `FctFlights` (fact) plus `DimOrigin`, `DimDestination`, and `DimCalendarDate` (dimensions), related at **city grain**.
- Added two calculated columns to support analysis of the raw fields that were not exposed directly:
  - **`Route`** — concatenated Origin City → Destination City label, for route-level grouping.
  - **`Lead Time`** — days between `Booking Date` and `Travel Start Date`, for booking-behaviour analysis.
- Built a dedicated `_Measures` table holding 20+ DAX measures, organised into folders (*Core Metrics*, *Time Intelligence*, *Trip Type & Class*) so every report page draws from the same governed calculation logic.

**Validation checks run against the live model:**

| Check | Result |
|---|---|
| Total rows vs. distinct rows | 10,000 vs. 10,000 — **no duplicate records** |
| Blank/null check — Airlines, Booking Agency, Ticket Amount, Origin/Destination City, Traveller name | **0 blanks** across all five fields |
| Ticket Amount range | $250.04 – $5,999.88 — **no negative or zero-value fares** |
| Lead Time range | 0 – 9 days — **no negative lead times** (i.e., no record where travel precedes booking) |
| Categorical completeness — Seat class, Travel Purpose, Stops, One-way/Return | All confirmed clean, mutually exclusive where expected (One-way and Return never both "Yes") |

---

## Key Findings & Insights

### 1. Route & Revenue Performance
- The network is **highly fragmented**: 4,576 distinct routes across 10,000 bookings, an average of just **2.19 bookings per route**.
- There is **no dominant route**. The top 10 routes by revenue contribute only **$243,502 — 2.9% of total revenue**. Even the single busiest pair (Atlanta → Mascot, $32,676 across 8 bookings) is statistically immaterial on its own.
- Connection count barely moves revenue: nonstop flights edge out the rest on both volume and yield, but the gap is narrow.

| Stops | Revenue | Bookings | Avg Ticket |
|---|---|---|---|
| 0 (Direct) | $2,875,002 | 3,409 | $843.36 |
| 1 | $2,759,017 | 3,328 | $829.03 |
| 2 | $2,736,968 | 3,263 | $838.79 |

### 2. Airline Partner Performance
- 22 airlines operate in the network. Revenue is **concentrated in four major carriers**, with a clear drop-off after that:

| Airline | Revenue | Bookings | Avg Ticket |
|---|---|---|---|
| Southwest Airlines | $688,904 | 775 | $888.91 |
| American Airlines | $643,770 | 779 | $826.41 |
| Delta Airlines | $633,890 | 778 | $814.77 |
| United Airlines | $628,538 | 731 | $859.83 |
| Hawaiian | $372,677 | 420 | $887.33 |

- **Southwest leads on both volume and yield** — highest total revenue *and* a top-tier average ticket price — making it the strongest partner by this dataset's measures.
- **Seat class shows almost no price differentiation.** Business, Economy, First, and Premium Economy all post average fares within a tight $826–$851 band:

| Seat Class | Revenue | Bookings | Avg Ticket |
|---|---|---|---|
| Business Class | $2,134,029 | 2,547 | $837.86 |
| Economy | $2,121,671 | 2,494 | $850.71 |
| First Class | $2,074,828 | 2,512 | $825.97 |
| Premium Economy | $2,040,458 | 2,447 | $833.86 |

  This runs counter to typical airline economics, where premium cabins usually command a significant fare premium over Economy. It's worth validating with whoever owns the source pricing logic before using cabin mix as a revenue lever.

### 3. Traveller Demand & Preferences
- Trip type is split almost exactly down the middle: **50.4% one-way vs. 49.6% return** — no dominant pattern to plan around.
- **Business travel is the highest-value segment** in the dataset for both the highest total revenue and the highest average fare:

| Travel Purpose | Revenue | Bookings | Avg Ticket |
|---|---|---|---|
| Business | $2,165,404 | 2,473 | $875.62 |
| Personal non-leisure | $2,099,317 | 2,553 | $822.29 |
| Other | $2,055,774 | 2,471 | $831.96 |
| Personal leisure | $2,050,491 | 2,503 | $819.21 |

  Business travel's average ticket runs **~6.9% above** Personal Leisure, which is the clearest "high-value segment" signal in the data.
- **Booking lead time is extremely short**: 0–9 days, averaging **3.3 days**. Travellers are booking on-demand rather than planning ahead — there's no advance-purchase behaviour to build early-bird pricing or long-range forecasting around.

### 4. Booking Channel Effectiveness
- Revenue is **unusually even across all 8 agencies**. The spread from highest (Expedia, $1.062M) to lowest (Travelocity, $1.013M) is under 5%:

| Agency | Revenue | Bookings | Avg Ticket |
|---|---|---|---|
| Expedia | $1,061,787 | 1,268 | $837.37 |
| Orbitz | $1,058,658 | 1,234 | $857.91 |
| United Airlines | $1,056,011 | 1,239 | $852.31 |
| Cheap Flights | $1,052,737 | 1,236 | $851.73 |
| Travel Sites | $1,049,676 | 1,240 | $846.51 |
| American Airlines | $1,048,189 | 1,236 | $848.05 |
| Cheapoair | $1,030,721 | 1,285 | $802.12 |
| Travelocity | $1,013,209 | 1,262 | $802.86 |

- **United Airlines and American Airlines appear as both carriers and booking agencies**, meaning some "agency" volume is actually direct-to-airline traffic, not third-party OTA traffic. This is legitimate, but it should be split out so direct-channel performance isn't blended with true OTA effectiveness.
- **Revenue is up 84.1% year-over-year**, which shows strong overall growth, though this reflects a two-year (2018–2019) dataset rather than a long historical trend.

---

## Conclusion

SkyLink's booking business is growing fast (+84.1% YoY), but its revenue is **broadly distributed rather than concentrated** across 4,576 routes, 22 airlines, and 8 nearly-equal booking channels. That's good for resilience (no single point of failure), but it also means there's no obvious "hero route" or "underperforming agency" to act on; the growth lever has to come from **segment-level** strategy rather than route- or channel-level triage.

The strongest, most actionable signal in the data is **Business travel**, which is the one segment that's both higher-revenue and higher-yield than its peers. The shortest path to margin improvement is also visible: seat classes are priced almost identically, leaving a cabin-pricing gap that doesn't show up in typical airline data. And two structural gaps, no persistent traveller ID and blended direct/OTA channel reporting, are currently limiting how far this analysis can go on loyalty and channel-strategy questions.

---

## Recommendations

1. **Rationalise the long tail of routes.** At an average of 2.19 bookings per route, most of the 4,576 routes are not commercially meaningful individually. Identify routes with 1–2 bookings/year for seasonal-only or codeshare service, and reinvest capacity into corridors with repeat, provable demand.
2. **Deepen the Southwest partnership.** Southwest is the only carrier combining the highest revenue and a top-tier average fare. Prioritise capacity commitments and joint marketing spend here before extending similar terms to American, Delta, or United.
3. **Audit cabin pricing logic.** A $25 spread in average fare between First Class and Economy is not typical for air travel and likely understates premium-cabin revenue potential. Review pricing/yield rules and consider class-based dynamic pricing to capture the willingness-to-pay gap.
4. **Build a dedicated corporate travel program.** Business travel is the highest-revenue, highest-yield purpose segment — formalise negotiated fares, dedicated agency support, and loyalty perks to grow it further.
5. **Add a persistent traveller/loyalty ID upstream.** Today, every booking looks like a first-time traveller, which blocks any repeat-customer, retention, or lifetime-value analysis. This is a data-collection fix, not a modelling one.
6. **Separate direct-airline bookings from true OTA channels in agency reporting.** United and American currently show up as both carriers and "agencies" — split this out so channel-effectiveness comparisons are apples-to-apples.
7. **Plan inventory and staffing around near-real-time demand.** With an average 3.3-day (max 9-day) booking lead time, demand planning should weight last-minute capacity and pricing flexibility far more heavily than long-range forecasting.

---

[Interact with the live dashboard here](https://app.powerbi.com/view?r=eyJrIjoiNzgyZmVjODQtYzBlYi00ZDU1LWIyZGQtYzA4N2E5MTJiMGE1IiwidCI6ImRlYWNhOWI3LWUzMTktNDI1MS1iZTQ2LTU4MWNmODI5ODFkMiJ9&pageName=0b82239a43d1720bff34)
