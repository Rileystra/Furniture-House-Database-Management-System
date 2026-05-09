# 🪑 Furniture House – Database Management System

A fully normalised **relational database system** designed for a multi-branch furniture rental business — built in Oracle SQL with an Entity Relationship Model, role-based access control, and advanced query logic.

---

## 📌 What It Does

The Furniture House database centralises and streamlines operations across multiple branches by:

- Managing branch, staff, member and rental data in one secure system
- Enforcing role-based access so each user only sees what they need
- Replacing redundant paperwork with a consistent, integrated data store
- Providing real-time operational insights through SQL queries and reports
- Supporting the full rental lifecycle — from member registration to returns

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Database | Oracle SQL |
| Modelling Tool | Microsoft Visio (ERD) |
| Query Language | SQL (DDL + DML) |
| Design Method | Entity Relationship Modelling (ERM) |
| Constraints | Primary Key, Foreign Key, NOT NULL, UNIQUE, CHECK |

---

## 🗄️ Database Schema

The system is built across five core tables:

### BRANCH
```sql
CREATE TABLE Branch (
    BRANCH_ID NUMBER PRIMARY KEY,
    POSTCODE VARCHAR2(6 BYTE),
    CONTACT_NUMBER VARCHAR2(11 BYTE),
    BRANCHNAME VARCHAR2(20 BYTE) NOT NULL,
    ADDRESS VARCHAR2(20 BYTE)
);
```

### FURNITURE
```sql
CREATE TABLE Furniture (
    FURNITURETYPE VARCHAR2(20 BYTE) NOT NULL,
    CATALOGUENUMBER NUMBER(38, 0) NOT NULL,
    FURNITUREID NUMBER(38, 0) NOT NULL,
    DESCRIPTION VARCHAR2(250 BYTE) NOT NULL,
    RENTALPRICE NUMBER(25, 0) NOT NULL
);
```

### MEMBERS
```sql
CREATE TABLE Members (
    MEMBERID VARCHAR2(20 BYTE) NOT NULL,
    FIRSTNAME VARCHAR2(45 BYTE) NOT NULL,
    LASTNAME VARCHAR2(45 BYTE),
    ADDRESS VARCHAR2(250 BYTE) NOT NULL,
    PHONE VARCHAR2(15 BYTE) NOT NULL,
    BRANCH_ID NUMBER NOT NULL,   -- FK → Branch
    JOINED DATE NOT NULL
);
```

### RENTALS
```sql
CREATE TABLE Rentals (
    RENTALID NUMBER(20, 0) PRIMARY KEY,
    QUANTITY NUMBER NOT NULL,
    RENTAL_COST NUMBER(20, 0),
    MEMBER_ID_FK VARCHAR2(20 BYTE) NOT NULL,  -- FK → Members
    DATE_RENTED DATE NOT NULL,
    NUMBERRENTED NUMBER
);
```

### STAFF
```sql
CREATE TABLE Staff (
    STAFF_ID_PK NUMBER(7, 0) PRIMARY KEY,
    FIRSTNAME VARCHAR2(20 BYTE) NOT NULL,
    STARTDATE DATE NOT NULL,
    SAL FLOAT NOT NULL,
    STAFFCONTACTDETAILS NUMBER(19, 0),
    POSITION VARCHAR2(20 BYTE) NOT NULL,
    NI VARCHAR2(9 BYTE) NOT NULL,
    SUPERVISORID NUMBER,
    EMAIL VARCHAR2(50 BYTE) NOT NULL,
    BRANCH_ID NUMBER(3, 0),     -- FK → Branch
    LASTNAME VARCHAR2(20 BYTE) NOT NULL
);
```

---

## 🔗 Entity Relationships

| Relationship | Type | Description |
|---|---|---|
| Branch ↔ Furniture | One-to-Many | A branch stocks many furniture items |
| Branch ↔ Staff | One-to-Many | A branch employs many staff members |
| Branch ↔ Members | One-to-Many | A branch has many members |
| Furniture ↔ Rentals | One-to-Many | A furniture item can be rented many times |
| Members ↔ Rentals | One-to-Many | A member can have many rentals |
| Staff ↔ Staff | Self-referencing | Staff members can have supervisors |

---

## 🔐 Constraints Applied

| Constraint | Purpose |
|---|---|
| **Primary Key** | Uniquely identifies each row (Branch ID, Staff ID, Rental ID, Member ID, Furniture ID) |
| **Foreign Key** | Enforces referential integrity between tables |
| **NOT NULL** | Ensures critical fields always have a value |
| **UNIQUE** | Prevents duplicate values in key columns |
| **CHECK** | Restricts column values to a valid range |

---

## 👥 Role-Based Access

The system is designed around four user roles, each with appropriate data access:

| Role | Access Level |
|---|---|
| **Managing Director** | Full access — all branches, staff, members, stock, rentals, reports |
| **Branch Manager** | Branch-specific — staff, stock, rentals for their branch only |
| **Administrator** | Member records, rental processing, stock and staff reports |
| **Assistant** | Stock availability and member details; process rentals and returns |

---

## 🔍 Example SQL Queries

**List all furniture with rental price above £50:**
```sql
SELECT
    FURNITUREID AS "Furniture ID",
    DESCRIPTION AS "Furniture Description",
    FURNITURETYPE AS "Furniture Type",
    RENTALPRICE AS "Rental Price"
FROM Furniture
WHERE RENTALPRICE > 50
ORDER BY RENTALPRICE ASC;
```

**List all staff under a specific line manager:**
```sql
SELECT
    STAFF_ID_PK AS StaffID,
    FIRSTNAME AS FirstName,
    LASTNAME AS LastName,
    POSITION AS Position,
    EMAIL AS Email,
    SAL AS Salary,
    BRANCH_ID AS BranchID
FROM Staff
WHERE SUPERVISORID = 1;
```

**Rank furniture by number of times rented, per branch:**
```sql
SELECT
    f.BRANCH_ID,
    f.FURNITUREID,
    f.FURNITURETYPE,
    f.DESCRIPTION,
    COALESCE(r.TOTALRENTALS, 0) AS TIMES_RENTED,
    RANK() OVER (PARTITION BY f.BRANCH_ID ORDER BY COALESCE(r.TOTALRENTALS, 0) DESC) AS RANKING
FROM FURNITURE f
LEFT JOIN RENTAL r ON f.FURNITUREID = r.RENTALID
ORDER BY f.BRANCH_ID ASC, RANKING ASC;
```

**Decode rental status (Active / Inactive / Unknown):**
```sql
SELECT
    BRANCH_ID,
    FURNITUREID,
    FURNITURETYPE,
    DESCRIPTION,
    COALESCE(TOTALRENTALS, 0) AS TIMES_RENTED,
    DECODE(
        SIGN(COALESCE(TOTALRENTALS, 0)),
        1, 'Active',
        0, 'Inactive',
        'Unknown'
    ) AS RENTAL_STATUS
FROM FURNITURE f
LEFT JOIN RENTAL r ON f.FURNITUREID = r.RENTALID
ORDER BY BRANCH_ID, TIMES_RENTED DESC;
```

---

## 📂 Project Structure

```
furniture-house-database/
│
├── schema/
│   ├── branch.sql          # Branch table DDL
│   ├── furniture.sql       # Furniture table DDL
│   ├── members.sql         # Members table DDL
│   ├── rentals.sql         # Rentals table DDL
│   └── staff.sql           # Staff table DDL
├── queries/
│   ├── admin_queries.sql   # Administrator role queries
│   ├── manager_queries.sql # Branch manager queries
│   └── reports.sql         # Ranking and decode queries
├── erd/
│   └── furniture_house_erd.png  # Entity Relationship Diagram
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Oracle SQL (or Oracle SQL Developer)
- Basic SQL knowledge

### Setup

```bash
# Clone the repo
git clone https://github.com/YOUR-USERNAME/furniture-house-database.git
cd furniture-house-database

# Run schema files in order
@schema/branch.sql
@schema/furniture.sql
@schema/members.sql
@schema/rentals.sql
@schema/staff.sql
```

---

## 📈 Future Improvements

- [ ] Migrate to a cloud database (AWS RDS / Azure SQL)
- [ ] Add stored procedures for common rental operations
- [ ] Build a simple web front-end for branch staff
- [ ] Implement automated backup and recovery scripts
- [ ] Add audit logging for sensitive data access

---

## 💡 Why This Project

This project demonstrates how a well-designed relational database can replace fragmented paperwork and inconsistent manual processes across a multi-site business. The focus throughout was on data integrity, role-appropriate access, and writing queries that generate genuine business insight — not just retrieving raw data.

---

## 👤 Author

**Uriel Djantou Fanja**
📧 urieldjantou@gmail.com
🔗 [GitHub](https://github.com/YOUR-USERNAME)

---

## 📄 Licence

MIT — free to use, modify and distribute.
