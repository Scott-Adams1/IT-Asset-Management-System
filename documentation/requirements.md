# Database Requirements

## 1. Purpose

Cat Technologies needs a centralized database to support IT asset inventory, employee assignments, software licensing, equipment maintenance, and help desk operations for approximately 500 employees.

This document defines the conceptual data model and business rules. It intentionally contains no SQL.

## 2. Scope

The system must support:

- Employees, technicians, departments, and office locations
- Laptops, desktops, monitors, printers, phones, and network equipment
- Current and historical asset assignments
- Software products, purchased licenses, and license allocations
- Help desk tickets and technician assignments
- Asset maintenance and repair history
- Operational and management reporting

## 3. Entities and Attributes

### Departments

- `department_id` — unique identifier
- `department_name` — unique department name

### Locations

- `location_id` — unique identifier
- `building`
- `floor`
- `room`
- `city`
- `state`

### Employees

- `employee_id` — unique identifier
- `first_name`
- `last_name`
- `email` — unique company email
- `hire_date`
- `department_id` — employee's department
- `location_id` — employee's primary location
- `employment_status` — active, leave, or terminated

### Technicians

Technician is an employee role, not a duplicate employee record.

- `technician_id` — unique identifier
- `employee_id` — associated employee
- `skill_level` — junior, intermediate, senior, or lead
- `active` — whether the technician currently accepts work

### Asset Types

- `asset_type_id` — unique identifier
- `asset_type_name` — laptop, desktop, monitor, printer, phone, or network equipment
- `description`

### Assets

- `asset_id` — unique identifier
- `asset_tag` — unique inventory tag
- `serial_number` — manufacturer serial number
- `asset_type_id`
- `manufacturer`
- `model`
- `purchase_date`
- `purchase_cost`
- `warranty_expiration`
- `status` — available, assigned, maintenance, retired, lost, or disposed
- `location_id`

### Asset Assignments

Preserves the history of assets issued to employees.

- `assignment_id` — unique identifier
- `asset_id`
- `employee_id`
- `assigned_date`
- `returned_date` — empty while active
- `assigned_by_technician_id`
- `notes`

### Software Products

- `software_id` — unique identifier
- `software_name`
- `vendor`
- `version`
- `category`

### Software Licenses

- `license_id` — unique identifier
- `software_id`
- `license_key` — protected identifier when applicable
- `purchase_date`
- `expiration_date`
- `seat_count`
- `purchase_cost`
- `status` — active, expired, suspended, or retired

### License Assignments

Records software seats allocated to an employee or an asset.

- `license_assignment_id` — unique identifier
- `license_id`
- `employee_id` — optional employee allocation
- `asset_id` — optional device allocation
- `assigned_date`
- `revoked_date`

### Tickets

- `ticket_id` — unique identifier
- `requester_employee_id`
- `assigned_technician_id` — optional until assigned
- `related_asset_id` — optional
- `category`
- `priority` — low, medium, high, or critical
- `status` — open, assigned, in progress, waiting, resolved, or closed
- `subject`
- `issue_description`
- `opened_at`
- `resolved_at`
- `closed_at`

### Maintenance Records

- `maintenance_id` — unique identifier
- `asset_id`
- `technician_id`
- `maintenance_type`
- `description`
- `start_date`
- `completion_date`
- `cost`
- `vendor_name` — optional outside repair vendor

## 4. Relationships

- One department has many employees; each employee belongs to one department.
- One location may contain many employees and assets.
- One employee may optionally be a technician.
- One asset type categorizes many assets.
- Employees and assets have a many-to-many relationship over time through asset assignments.
- One software product may have many purchased licenses.
- Software licenses may be allocated many times through license assignments.
- One employee may submit many tickets.
- One technician may be assigned many tickets.
- One asset may be referenced by many tickets.
- One asset may have many maintenance records.
- One technician may perform many maintenance activities.

## 5. Business Rules

1. Department names, employee emails, asset tags, and license keys must be unique where applicable.
2. Every employee must belong to one department.
3. Every asset must have one asset type and one current status.
4. An asset may have no more than one active employee assignment.
5. An active assignment has no return date.
6. A returned assignment must have a return date on or after its assigned date.
7. Only active technicians may receive new ticket assignments.
8. Resolved and closed times cannot precede a ticket's opening time.
9. Closed tickets must include a closed time.
10. Purchase and maintenance costs cannot be negative.
11. Warranty and license expiration dates cannot precede purchase dates.
12. Active license allocations cannot exceed the purchased seat count.
13. A license allocation must target either one employee or one asset, but not both.
14. Historical assignments, tickets, and maintenance records should be retained.
15. Terminated employees cannot receive new assets or software allocations.

## 6. Planned Reports

- Laptops with warranties expiring in the next 30 days
- Employees with more than three assigned devices
- Average ticket resolution time by technician
- Departments submitting the most tickets
- Software licenses with unused seats
- Duplicate serial numbers
- Assets with the highest maintenance costs
- Tickets remaining open beyond their service target

## 7. Assumptions

- Cat Technologies owns all stored assets.
- Employees have one primary department and location at a time.
- Department-transfer history is outside the initial scope.
- Asset and license assignment history is retained.
- Authentication, permissions, and UI design are outside this phase.
