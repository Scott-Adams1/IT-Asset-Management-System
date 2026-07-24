# Enterprise IT Asset & Help Desk Management System

## Project Overview

This project designs and implements an enterprise IT asset and help desk management database for **Cat Technologies**, a fictional company with approximately 500 employees.

The system will track company hardware, software licenses, employee asset assignments, maintenance history, and help desk tickets. It is a portfolio project demonstrating requirements analysis, relational database design, PostgreSQL, data generation, reporting, and future Power BI and Python integration.

## Business Goals

- Track laptops, desktops, monitors, printers, phones, and network equipment
- Record which assets are assigned to employees
- Preserve asset assignment and return history
- Track repairs and maintenance costs
- Track software licenses, seat availability, and expiration dates
- Manage help desk tickets and assign technicians
- Measure ticket resolution time and technician performance
- Report on warranties, assets, licenses, and help desk activity

## Company Departments

- Human Resources
- Finance
- Information Technology
- Marketing
- Sales
- Operations
- Executive

## Planned Data Volume

- 500 employees
- 2,500 IT assets
- 3,000 help desk tickets
- 200 software licenses
- 7 departments

## Project Structure

```text
.
├── README.md
├── LICENSE
├── database/
├── diagrams/
├── documentation/
├── sample_data/
├── screenshots/
└── queries/
```

## Project Phases

1. Requirements gathering and database design
2. PostgreSQL schema implementation
3. Realistic sample-data generation
4. Reporting and analytical SQL queries
5. Power BI and Python integration

## Current Status

**Phase 1 — Requirements gathering and database design**

See [Database Requirements](documentation/requirements.md) and the [Entity-Relationship Diagram](diagrams/erd.md).
