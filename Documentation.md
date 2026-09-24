# 📘 Documentation — Hotel Booking Demand Dashboard

This document provides a detailed technical reference for the project: dataset structure, data cleaning notes, KPI definitions, chart logic, and known limitations. It is meant to accompany `README.md` for anyone (recruiters, collaborators, or future-you) who wants to understand exactly how the dashboard was built and what each number means.

---

## 1. Dataset Overview

| Property | Value |
|---|---|
| File | `hotel_bookings.csv` |
| Rows | 119,390 bookings |
| Columns | 32 fields |
| Time span | 2015 – 2017 |
| Hotels covered | City Hotel, Resort Hotel (both located in Portugal) |
| Granularity | One row = one booking/reservation |

### 1.1 Data Dictionary

| Column | Type | Description |
|---|---|---|
| `hotel` | Categorical | Hotel type: `City Hotel` or `Resort Hotel` |
| `is_canceled` | Binary (0/1) | 1 if the booking was cancelled |
| `lead_time` | Numeric | Days between booking date and arrival date |
| `arrival_date_year` | Numeric | Year of arrival (2015–2017) |
| `arrival_date_month` | Categorical | Month name of arrival |
| `arrival_date_week_number` | Numeric | ISO week number of arrival |
| `arrival_date_day_of_month` | Numeric | Day of month of arrival |
| `stays_in_weekend_nights` | Numeric | Number of weekend nights booked |
| `stays_in_week_nights` | Numeric | Number of weeknights booked |
| `adults` / `children` / `babies` | Numeric | Guest counts |
| `meal` | Categorical | Meal plan booked (BB, HB, FB, SC, Undefined) |
| `country` | Categorical | Guest's country of origin (ISO code) |
| `market_segment` | Categorical | e.g. Direct, Corporate, Online TA, Offline TA/TO, Groups, Complementary, Aviation |
| `distribution_channel` | Categorical | Booking distribution channel |
| `is_repeated_guest` | Binary (0/1) | 1 if the guest has stayed before |
| `previous_cancellations` | Numeric | Count of prior cancellations by this guest |
| `previous_bookings_not_canceled` | Numeric | Count of prior completed bookings |
| `reserved_room_type` / `assigned_room_type` | Categorical | Room type requested vs. actually assigned |
| `booking_changes` | Numeric | Number of changes made to the booking |
| `deposit_type` | Categorical | `No Deposit`, `Non Refund`, `Refundable` |
| `agent` / `company` | Categorical (ID) | Travel agent / company ID (many nulls — see §2) |
| `days_in_waiting_list` | Numeric | Days the booking spent on a waiting list |
| `customer_type` | Categorical | Transient, Transient-Party, Contract, Group |
| `adr` | Numeric | **Average Daily Rate** — the average revenue per occupied room-night for that booking |
| `required_car_parking_spaces` | Numeric | Parking spaces requested |
| `total_of_special_requests` | Numeric | Count of special requests made |
| `reservation_status` | Categorical | Check-Out, Canceled, No-Show |
| `reservation_status_date` | Date | Date of the final reservation status |

---

## 2. Data Cleaning Notes

The raw dataset contains known data-quality issues that should be accounted for before analysis:

| Field | Issue | Recommended Handling |
|---|---|---|
| `country` | ~488 missing values | Excluded/grouped as "Unknown" in country-level charts |
| `agent` | ~16,340 missing values | Null = booking made without a travel agent (not an error) |
| `company` | ~112,593 missing values | Null = booking not made through a company (expected — most guests are individuals) |
| `children` | 4 missing values | Treated as 0 / dropped |
| `adr` | Contains a small number of 0 or negative outliers | Reviewed and filtered where they would distort ADR averages (e.g., complimentary stays) |
| Duplicate rows | **31,994 full-row duplicates** confirmed in the raw file (119,390 → 87,396 unique rows) | **Removed** before building the PivotTables |

> ℹ️ **Note on KPI totals:** The dashboard's headline "Total Bookings" is **86,009**, after removing the 31,994 duplicate records (119,390 → 87,396) plus a small number of additional exclusions (e.g., the 488 rows with missing `country` and/or a handful of invalid `adr` records), bringing the final clean count to 86,009. This is a deliberate data-quality step, not a data-loading error.

---

## 3. KPI Definitions & Formulas

| KPI | Formula (logic) |
|---|---|
| **Total Bookings** | Count of booking records after cleaning |
| **Overall Cancellation Rate** | `COUNT(is_canceled = 1) / COUNT(all bookings)` |
| **Average Daily Rate (ADR)** | `AVERAGE(adr)` across all bookings (or filtered subset via slicers) |
| **Total Revenue Exposure** | `SUM(adr × total nights booked)` — approximates total potential room revenue represented by all bookings, whether or not they were ultimately cancelled |
| **Repeat Guest Rate** | `COUNT(is_repeated_guest = 1) / COUNT(all bookings)` |
| **Cancellation Rate by Hotel** | `COUNT(is_canceled = 1)` grouped by `hotel` |
| **Cancellation Rate by Market Segment** | `COUNT(is_canceled = 1)` grouped by `market_segment`, shown as % of segment total |
| **ADR by Deposit Type** | `SUM(adr)` grouped by `deposit_type`, shown as a share of total ADR exposure |
| **ADR by Hotel over Months** | `AVERAGE(adr)` grouped by `hotel` and `arrival_date_month` |

---

## 4. Dashboard Architecture (Excel)

The workbook is built entirely in native Excel using:

- **PivotTables** as the underlying data engine for every chart (one pivot per visual, sourced from a single cleaned data range/table).
- **PivotCharts** for all bar, line, and pie visuals, so every chart responds live to slicer selections.
- **Slicers** (Hotel, Market Segment, Country, Deposit Type, Arrival Month) connected to all relevant PivotTables via **Report Connections**, enabling synchronized cross-filtering across the whole dashboard from a single click.
- **KPI cards** built as formatted cells/shapes referencing summary formulas (or pivot GETPIVOTDATA references) rather than static text, so they update as filters change.
- A single dashboard sheet for presentation, with supporting pivot/data sheets behind it (7 sheets total in the workbook).

---

## 5. Chart-by-Chart Reference

1. **Top 10 Countries by Booking Volume** — Bar chart, sourced from a pivot of `country` × booking count, filtered/sorted to the top 10. PRT (Portugal) dominates as the hotels' home market.
2. **Average Daily Rate by Hotel over Months** — Line chart of average `adr` per `arrival_date_month`, split by `hotel`, showing seasonality (summer peak for City Hotel).
3. **Cancellation Rate by Hotel** — Bar chart comparing `is_canceled` volume between City Hotel and Resort Hotel.
4. **Average Daily Rate by Deposit Type** — Pie chart showing the proportional ADR exposure split across `No Deposit`, `Non Refund`, and `Refundable` deposit types.
5. **Cancellation Rate by Market Segment** — Horizontal bar chart ranking `market_segment` categories by cancellation percentage, highlighting Groups/Online TA as higher-risk segments.

---

## 6. Key Findings (Detailed)

- **Cancellation risk is not evenly distributed.** Market segment and deposit type are much stronger differentiators of cancellation likelihood than hotel type alone — this is actionable for revenue management (e.g., stricter deposit policies for higher-risk segments).
- **Seasonality is hotel-specific.** City Hotel ADR is more volatile and peaks sharply in summer, while Resort Hotel pricing is comparatively flatter — suggesting different demand drivers (business/city travel vs. leisure/resort travel).
- **Geographic concentration.** A large share of demand originates domestically (Portugal) or from a small set of Western European countries, which has implications for marketing spend allocation.
- **Loyalty gap.** A low repeat guest rate suggests limited guest retention — a candidate area for a loyalty program or CRM-driven remarketing.

---

## 7. Limitations

- The dataset covers a single City Hotel and a single Resort Hotel in Portugal (2015–2017); findings are illustrative and not necessarily generalizable to other markets or time periods.
- `Total Revenue Exposure` is a modeled estimate (rate × nights), not confirmed actual collected revenue, and does not net out cancellations.
- Some fields (`agent`, `company`) have very high null rates by nature of the business process, which limits granular channel-level analysis.
- The dashboard is static/file-based (Excel); it is not connected to a live booking system.

---

## 8. Future Enhancements

- Build a cancellation-prediction model (logistic regression / gradient boosting) using `lead_time`, `deposit_type`, `market_segment`, and guest history features.
- Add a lead-time distribution analysis and correlate it with cancellation probability.
- Rebuild the same dashboard in Power BI or Tableau for cloud-based, shareable access.
- Add year-over-year comparison once more recent data becomes available.

---

## 9. Contact / Attribution

Dataset: *Hotel Booking Demand*, originally compiled by Antonio, Almeida & Nunes (2019), widely distributed via Kaggle for educational/research use.

Dashboard design, KPI logic, and analysis: this repository's author.
