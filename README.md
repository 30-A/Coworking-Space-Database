# Coworking Space Database Management System

This project is a relational database system to manage the daily operations of a coworking space. It handles Member Management, Space Booking, Billing/Payments and Availability Scheduling. The system ensures data integrity using ACID-compliant transactions to prevent double-booking and uses Triggers for audit logging.

### Project Structure

```text
coworking_space/
├── queries/
│   ├── 01_create_tables.sql
│   ├── 02_import_data.sql
│   ├── 03_create_indexes.sql
│   ├── 04_crud_operations.sql
│   ├── 05_joins_and_aggregations.sql
│   ├── 06_stored_procedures.sql
│   ├── 07_triggers.sql
│   └── 08_transactions.sql
├── data/
│   ├── membership_plans.csv
│   ├── spaces.csv
│   ├── space_availability.csv
│   ├── members.csv
│   ├── bookings.csv
│   ├── payments.csv
│   └── booking_log.csv
├── ERD/
│   └── er_diagram.png
└── README.md
```

### SQL Files Overview

| File | Description |
|---|---|
| 01_create_tables.sql | Creates database and all tables with primary/foreign keys |
| 02_import_data.sql | Imports data from CSV files |
| 03_create_indexes.sql | Creates indexes for better query performance |
| 04_crud_operations.sql | INSERT, SELECT, UPDATE, DELETE operations |
| 05_joins_and_aggregations.sql | INNER JOIN, LEFT JOIN, GROUP BY, HAVING |
| 06_stored_procedures.sql | Two stored procedures with parameters |
| 07_triggers.sql | Triggers that log booking changes |
| 08_transactions.sql | Transaction using COMMIT |

### Database Schema

#### MembershipPlans (5 records)

Stores the available membership options.

| Column | Type | Description |
|---|---|---|
| plan_id | INT | Primary key |
| plan_name | VARCHAR(50) | Name (Day Pass, Weekly, Monthly, Quarterly, Annual) |
| price | DECIMAL(10,2) | Cost of the plan |
| duration_days | INT | Length of membership in days |

#### Spaces (17 records)

Stores all bookable workspaces.

| Column | Type | Description |
|---|---|---|
| space_id | INT | Primary key |
| space_name | VARCHAR(50) | Name of the space |
| space_type | ENUM | Hot Desk, Dedicated Desk, Meeting Room, Private Office |
| capacity | INT | Number of people |
| hourly_rate | DECIMAL(10,2) | Price per hour |
| is_active | BOOLEAN | Whether space is available |

#### SpaceAvailability (89 records)

Stores operating hours for each space by day of week.

| Column | Type | Description |
|---|---|---|
| space_id | INT | Foreign key to Spaces |
| day_of_week | TINYINT | 1 (Monday) to 7 (Sunday) |
| open_time | TIME | Opening time |
| close_time | TIME | Closing time |

#### Members (1,000 records)

Stores registered members.

| Column | Type | Description |
|---|---|---|
| member_id | INT | Primary key |
| first_name | VARCHAR(50) | First name |
| last_name | VARCHAR(50) | Last name |
| email | VARCHAR(100) | Email (unique) |
| phone | VARCHAR(20) | Phone number |
| plan_id | INT | Foreign key to MembershipPlans |
| registration_date | DATE | Date joined |
| membership_end | DATE | Membership expiry date |
| status | ENUM | Active, Expired, Cancelled |

#### Bookings (15,001 records)

Stores space reservations.

| Column | Type | Description |
|---|---|---|
| booking_id | INT | Primary key |
| member_id | INT | Foreign key to Members |
| space_id | INT | Foreign key to Spaces |
| booking_date | DATE | Date of booking |
| start_time | TIME | Start time |
| end_time | TIME | End time |
| total_price | DECIMAL(10,2) | Total cost |
| status | ENUM | Confirmed, Cancelled, Completed |
| created_at | TIMESTAMP | When booking was created |

#### Payments (13,487 records)

Stores all payment transactions.

| Column | Type | Description |
|---|---|---|
| payment_id | INT | Primary key |
| member_id | INT | Foreign key to Members |
| booking_id | INT | Foreign key to Bookings (NULL for membership payments) |
| amount | DECIMAL(10,2) | Payment amount |
| payment_date | DATE | Date of payment |
| payment_method | ENUM | Cash, Card, Bank Transfer |
| payment_type | ENUM | Booking, Membership |

#### BookingLog (200 records)

Audit log that tracks booking changes (populated by triggers).

| Column | Type | Description |
|---|---|---|
| log_id | INT | Primary key |
| booking_id | INT | Related booking |
| member_id | INT | Related member |
| space_id | INT | Related space |
| action | VARCHAR(10) | INSERT or UPDATE |
| old_status | VARCHAR(20) | Previous status |
| new_status | VARCHAR(20) | New status |
| changed_at | TIMESTAMP | When change occurred |

### Business Questions Answered

The following business questions are answered via queries in the SQL files:

* **Q1:** How many members are currently active? _(file 04)_
* **Q2:** What are the 10 most recent bookings? _(file 04)_
* **Q3:** Find all "Meeting Room" spaces? _(file 04)_
* **Q4:** What bookings are scheduled for December 20th? _(file 04)_
* **Q5:** What is the total revenue by space type? _(file 05)_
* **Q6:** Which Payment Method is most preferred? _(file 05)_
* **Q7:** Which members have never made a booking? _(file 05)_
* **Q8:** How many members are on each plan? _(file 05)_
* **Q9:** Who are the top 5 highest-spending members? _(file 05)_
* **Q10:** Which members have booked more than 5 times? _(file 05)_
* **Q11:** What is the full booking history for a specific member? _(file 06)_
* **Q12:** How much revenue was generated between two dates? _(file 06)_

### Tools

 * MySQL 9.5.0
 * DataGrip 2025.2.4

### Author

 * Mohamed Amine Aouini
