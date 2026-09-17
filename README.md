# Metro Ticket Generating System

A ServiceNow-based metro ticket booking application developed as an academic project.

## Features

- Metro station selection
- Source and destination validation
- Automatic fare calculation
- Distance and estimated duration calculation
- Single and return journey support
- Passenger count validation
- Approval workflow
- Automatic metro ticket generation
- Digital QR ticket generation
- My Tickets Service Portal page
- Ticket cancellation
- Automatic refund calculation
- Cancellation notification
- Scheduled cancellation of expired pending bookings

## Technologies Used

- ServiceNow
- ServiceNow Studio
- Service Portal
- Flow Designer / Workflow Studio
- Business Rules
- Script Includes
- GlideAjax
- GlideRecord
- GlideRecordSecure
- Record Producers
- Catalog Client Scripts
- Scheduled Script Executions
- Email Notifications
- QRCode.js
- GitHub Source Control

## Application Flow

User books metro ticket

→ Fare is calculated

→ Booking created with Pending status

→ Approval request generated

→ Approver approves booking

→ Ticket number generated

→ Metro Status becomes Generated

→ QR ticket generated

→ Passenger views ticket in My Tickets portal

→ Passenger can cancel eligible ticket

→ Refund calculated automatically

## Metro Booking Statuses

- Pending
- Approved
- Generated
- Rejected
- Cancelled

## Fare Calculation

Fare is calculated based on configured route distance and Metro Fare Rules.

Example:

Miyapur → L B Nagar

Distance: 29 KM

Base Fare: ₹69

## QR Ticket

Generated tickets contain QR data including:

- Ticket Number
- Booking Number
- Source Station
- Destination Station
- Travel Date
- Journey Type
- Passenger Count
- Distance
- Fare

The QR implementation is for academic demonstration purposes and is not connected to Hyderabad Metro's production AFC system.

## Cancellation and Refund

Academic refund rules used in this project:

- Future travel date: 90% refund
- Same-day cancellation: 50% refund
- Past travel date: No refund

## Service Portal

The application contains a passenger-facing Service Portal with:

- Book Metro Ticket
- My Tickets
- Digital QR Ticket
- Cancel Ticket

## Source Control

The ServiceNow scoped application is integrated with GitHub using ServiceNow Source Control.

Development branch:

`sn_instances/dev268719`

Changes are merged into:

`main`
