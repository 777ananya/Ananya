# FoodBank-Manager

A C-based management system for NGOs and food banks that records donated food and recipient requests, allocates stock using FIFO by expiry date and need-based priority, tracks perishable and non-perishable inventory across multiple shelters, raises expiry alerts, creates distribution batches, and generates daily distribution, waste and donor acknowledgement reports.

**Project 30: Food Bank Donation & Distribution Management**

---

## Problem Statement

NGOs need to track donated food items, expiry dates, recipient eligibility and distribution logistics. This program provides a console-based system that:

- Records donations (item type, quantity, expiry date, donor details) and recipient requests (family size, dietary restrictions, priority).
- Allocates near-expiry items first (FIFO by expiry) and serves high-need requests first.
- Tracks inventory (perishable vs non-perishable) and alerts for expiring or expired items.
- Creates distribution batches.
- Produces reports: daily distributed quantities, waste due to expiry, and donor acknowledgements.

---

## Features

| Feature | Description |
|---|---|
| Donation ingestion | Item, perishable flag, veg/non-veg, quantity, expiry date, donor name and phone, storage location |
| Request ingestion | Family name, family size, veg-only restriction, priority (1-3), item, quantity, nearest location |
| Expiry sorting | Stock is sorted so the earliest expiry is always distributed first |
| Need-based matching | Requests are ordered by priority, then by family size |
| Dietary rule | Veg-only families are never given non-veg items |
| Multi-location | 3 shelters (Shelter A, B, C), matched by location |
| Expiry alerts | Flags items expiring within 3 days and writes off expired stock |
| Distribution batches | Each matching run creates a numbered batch with a full log |
| Reports | Daily distribution, waste due to expiry, donor acknowledgements |
| Day simulation | Advance the date to test expiry and waste tracking |

---

## Concepts Used (Syllabus Mapping)

| Topic | Where it is used |
|---|---|
| Structures | `Donation`, `Request`, `DistRecord`, `Date` |
| Dynamic memory | `malloc`, `realloc`, `free` for donations, requests and the distribution log |
| 2D arrays | `stock[location][type]` in the inventory view and the per-location daily report |
| Sorting | Insertion sort by expiry date, selection sort by priority and family size |
| Pointers | `nextDay(Date *d)`, `Donation *d`, `Request *r`, dynamic `order` array |
| Decision making | Expiry checks, dietary checks, location and item matching |
| Functions | Separate functions for ingestion, matching, distribution and reporting |

Course outcomes covered: CO1-CO5.

---

## Project Structure

```
FoodBank-Manager/
├── food_bank.c    # complete source code
└── README.md      # this file
```

---

## How to Compile and Run

**Requirements:** a C compiler such as GCC.

```bash
gcc food_bank.c -o food_bank
./food_bank
```

On Windows, run `food_bank.exe` instead of `./food_bank`.

---

## Menu Options

```
1.  Add donation
2.  Add recipient request
3.  Show inventory
4.  Expiry alerts
5.  Run matching & create distribution batch
6.  Daily distribution report
7.  Waste report
8.  Donor acknowledgements
9.  Advance to next day
10. Load sample data
0.  Exit
```

On start, enter today's date in `DD MM YYYY` format.

---

## Quick Demo

1. Start the program and enter the date `19 11 2026`.
2. Choose **10** to load sample data.
3. Choose **3** to view inventory sorted by expiry.
4. Choose **4** to see expiry alerts.
5. Choose **5** to run matching and create a distribution batch.
6. Choose **6** to view the daily report.
7. Choose **9** a few times to advance the date, then **4** and **7** to see expired stock written off as waste.
8. Choose **8** to print donor acknowledgements.

### Sample Output (matching)

```
=== Distribution Batch #1 (19-11-2026) ===
  Singh Family <-   30 x Rice         (batch 1, expires 30-06-2027, Shelter A)
  Das Family   <-   10 x Chicken      (batch 4, expires 25-11-2026, Shelter B)
  Khan Family  <-   25 x Bread        (batch 2, expires 20-11-2026, Shelter A)
  Gupta Family <-   15 x Bread        (batch 2, expires 20-11-2026, Shelter A)
  Gupta Family still needs 10 x Bread (stock unavailable)
Total distributed in this batch: 80 units
```

---

## How the Matching Works

1. Expired stock is written off and counted as waste.
2. Donations are sorted by expiry date (earliest first), giving FIFO by expiry.
3. Requests are sorted by priority (High, Medium, Low), then by larger family size.
4. For each request, the program goes through the sorted stock and gives items that match:
   - the same item name,
   - the same location,
   - the dietary restriction (veg-only requests get veg items only),
   - and that have not expired.
5. Every allocation is logged in the distribution record used by the reports.

---

## Limitations and Possible Improvements

- Data is held in memory only and is lost when the program exits. File handling (`fopen`, `fprintf`, `fscanf`) can be added to save and load records.
- Item names must match (case-insensitive) between donation and request.
- The number of locations is fixed at 3 (`MAX_LOC`).
- Possible additions: transfers between shelters, partial-expiry alerts by category, and a login for staff.

---

## Author

Ananya, B.Tech CSE (AI & ML)
