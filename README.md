# 🏨 Hotel Booking Demand Dashboard — Excel

An interactive **Excel dashboard** analyzing hotel booking demand, cancellations, pricing, and guest behavior across **City Hotel** and **Resort Hotel**.

Built using the **Hotel Booking Demand** dataset containing **119,390 booking records** from 2015–2017.

---

## 📊 Dashboard Preview

![Hotel Booking Demand Dashboard](<img width="1122" height="592" alt="report_overview" src="https://github.com/user-attachments/assets/67aa83fa-a1d1-496a-b3f8-a8a075277292" />)


---

## 📌 Project Overview

Hotel booking data can provide valuable insights into demand patterns, cancellation behavior, pricing, and guest characteristics.

This project transforms raw hotel booking data into an interactive Excel dashboard designed to answer key business questions:

- How many bookings were made, and what percentage were cancelled?
- What is the average daily rate (ADR)?
- How does ADR vary across months and hotel types?
- Which countries generate the highest booking volume?
- How does cancellation rate vary by hotel and market segment?
- How does ADR vary by deposit type?
- What proportion of bookings come from repeat guests?

The dashboard uses **PivotTables, PivotCharts, slicers, and KPI cards** to provide an interactive view of the data.

---

## 🎯 Business Questions

The dashboard focuses on five major areas:

### 1. Booking Demand
- Total number of bookings
- Top countries by booking volume
- Booking distribution across hotel types

### 2. Cancellation Behavior
- Overall cancellation rate
- Cancellation rate by hotel
- Cancellation rate by market segment

### 3. Pricing Analysis
- Average Daily Rate (ADR)
- Monthly ADR trends by hotel type
- ADR comparison by deposit type

### 4. Guest Behavior
- Repeat guest rate
- Guest and booking characteristics

### 5. Interactive Analysis
Users can filter the dashboard using slicers for:

- Hotel
- Market Segment
- Country
- Deposit Type
- Arrival Month

---

## 📈 Dashboard Components

| Component | Description |
|---|---|
| **Total Bookings** | Total number of booking records |
| **Overall Cancellation Rate** | Percentage of bookings that were cancelled |
| **Average Daily Rate (ADR)** | Average daily room rate across bookings |
| **Total Revenue Exposure** | Revenue-related exposure calculated from the available booking data |
| **Repeat Guest Rate** | Percentage of bookings made by repeat guests |
| **Top 10 Countries by Booking Volume** | Countries contributing the highest number of bookings |
| **Average Daily Rate by Hotel over Months** | Monthly ADR trend comparing City Hotel and Resort Hotel |
| **Cancellation Rate by Hotel** | Cancellation rate comparison between the two hotel types |
| **Average Daily Rate by Deposit Type** | ADR comparison across deposit categories |
| **Cancellation Rate by Market Segment** | Cancellation rate across different market segments |
| **Slicers** | Interactive filters for hotel, market segment, country, deposit type, and arrival month |

---

## 🔑 Key Insights

### 🌍 Booking Volume

**Portugal (PRT)** is the largest source country by booking volume in the dataset, followed by other major European markets such as **United Kingdom (GBR), France (FRA), Spain (ESP), and Germany (DEU)**.

### 🏨 Hotel Pricing

ADR varies considerably across months and between **City Hotel** and **Resort Hotel**, with seasonal fluctuations visible in the monthly trend.

### ❌ Cancellation Behavior

Cancellation rates differ between hotel types and market segments. The dashboard allows these differences to be explored interactively using the available slicers.

### 👥 Repeat Guests

The repeat guest rate is relatively small compared with the overall booking volume, providing a useful metric for understanding guest retention.

> **Note:** Insights are descriptive and based on the dataset used in this project. They should not be interpreted as causal relationships.

---

## 🗂️ Repository Structure

```text
Hotel-Booking-Demand-Dashboard/
│
├── Hotel_Booking_Demand_Dashboard.xlsx
├── hotel_bookings.csv
├── report_overview.png
├── README.md
└── DOCUMENTATION.md
