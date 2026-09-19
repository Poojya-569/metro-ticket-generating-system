# Metro Ticket Generating System

A ServiceNow-based academic project for booking metro tickets through a Service Portal.

The application allows a passenger to select source and destination stations, calculate distance and fare, book a ticket, generate a digital QR ticket, view booked tickets, cancel eligible tickets, and calculate refunds.

---

## Project Overview

The **Metro Ticket Generating System** was developed using the ServiceNow platform to demonstrate a complete metro ticket booking workflow.

The project includes:

- Metro station master data
- Route and fare configuration
- Automatic fare calculation
- Service Catalog / Record Producer booking
- Approval workflow using Flow Designer
- Ticket number generation
- Digital QR ticket generation
- My Tickets portal
- Ticket cancellation
- Refund calculation
- Notifications
- Scheduled cancellation of expired pending bookings
- GitHub source control integration

---

## Main Features

### 1. Metro Ticket Booking

Passengers can book a metro ticket by selecting:

- Source Station
- Destination Station
- Travel Date
- Journey Type
- Passenger Count

The system validates the booking before creating the ticket.

### 2. Automatic Fare Calculation

The application automatically calculates:

- Route distance
- Estimated duration
- Base fare
- Total fare

The total fare depends on the route distance, journey type, and passenger count.

Single journey:

`Base Fare × Passenger Count`

Return journey:

`Base Fare × 2 × Passenger Count`

### 3. Approval Workflow

The project was initially implemented with a **manual approval workflow**.

Original development flow:

```text
Booking Submitted
      ↓
Pending
      ↓
Approval Request Created
      ↓
Approver Approves / Rejects
      ↓
Approved
      ↓
Ticket Number Generated
      ↓
QR Generated
```

In the original implementation, the QR ticket was generated only after the approval request was approved.

For the **final evaluator/demo configuration**, the approval step is automated in the backend using Flow Designer. This allows an evaluator to test the complete booking-to-QR workflow using a single login without requiring a separate approver account.

Final demo flow:

```text
Booking Submitted
      ↓
Pending
      ↓
Flow Designer Automatically Sets Approval = Approved
      ↓
Generate Metro Ticket Business Rule Runs
      ↓
Ticket Number Generated
      ↓
QR Generated
      ↓
Metro Status = Generated
```

The approval logic is still part of the backend workflow; only the manual approver action is automated for demonstration convenience.

### 4. Digital Ticket Generation

After approval, the system automatically generates:

- Ticket Number
- QR Data
- Generated Date/Time
- Generated status

Example ticket number:

`HMR-MBK0001027`

### 5. QR Ticket

Generated tickets display a QR code in **My Tickets**.

The QR payload contains information such as:

- Ticket Number
- Booking Number
- Passenger
- Source Station
- Destination Station
- Travel Date
- Journey Type
- Passenger Count
- Distance
- Estimated Duration
- Total Fare
- Ticket Status

The QR implementation is for academic demonstration purposes and is not connected to the real Hyderabad Metro AFC/gate validation system.

### 6. My Tickets

The **My Tickets** page allows the logged-in passenger to view their bookings.

It displays:

- Booking Number
- Ticket Number
- Source and Destination
- Travel Date
- Journey Type
- Passenger Count
- Distance
- Duration
- Base Fare
- Total Fare
- Ticket Status
- QR Ticket
- Refund information when cancelled

Only tickets belonging to the logged-in user are displayed.

### 7. Ticket Cancellation

Eligible generated tickets can be cancelled from the **My Tickets** page.

After cancellation:

- Metro Status becomes `Cancelled`
- Cancellation reason is stored
- Refund amount is calculated automatically

### 8. Refund Calculation

The project uses the following academic refund rules:

- Future travel date: **90% refund**
- Same-day cancellation: **50% refund**
- Past travel date: **No refund**

### 9. Scheduled Job

A Scheduled Script Execution checks for expired pending bookings.

If a booking is still pending and its travel date has already passed, the system automatically cancels it.

### 10. Notifications

The application includes notifications for ticket-related events.

Approval-request notification behavior was implemented during the manual approval version of the workflow. Email delivery behavior may depend on the ServiceNow Personal Developer Instance email configuration.

---

## Application Flow

```text
Passenger Login
      ↓
Metro Service Portal
      ↓
Book Metro Ticket
      ↓
Select Source and Destination
      ↓
Enter Travel Date / Journey Type / Passengers
      ↓
Distance + Duration + Fare Calculated
      ↓
Submit Booking
      ↓
Booking Created as Pending
      ↓
Flow Designer Automatically Sets Approval = Approved
      ↓
Generate Metro Ticket Business Rule Runs
      ↓
Ticket Number Generated
      ↓
QR Data Generated
      ↓
Metro Status = Generated
      ↓
My Tickets
      ↓
View Digital QR Ticket
      ↓
Cancel Ticket (if eligible)
      ↓
Refund Calculated
```

---

## Metro Booking Statuses

The application uses the following Metro Booking statuses:

- `Pending`
- `Approved`
- `Generated`
- `Rejected`
- `Cancelled`

In the current demo configuration, approval is automated, so a valid booking normally moves quickly from Pending to Generated.

---

## Technologies Used

- ServiceNow
- ServiceNow Studio
- Service Portal
- Flow Designer
- Record Producer
- Catalog Client Scripts
- Script Includes
- GlideAjax
- GlideRecord
- GlideRecordSecure
- Business Rules
- Scheduled Script Executions
- Email Notifications
- QRCode.js
- GitHub Source Control

---

## ServiceNow Application Components

### Metro Station

Stores metro station master data.

Main fields:

- Station Name
- Metro Line
- Interchange
- Active

### Metro Fare Rule

Stores fare ranges based on travel distance.

Main fields:

- Zone
- Minimum Distance
- Maximum Distance
- Fare
- Effective From
- Active

### Metro Route Master

Stores configured routes used for distance and duration calculation.

Main fields:

- Route Name
- Source Station
- Destination Station
- Distance KM
- Duration Minutes
- Active

### Metro Booking

Stores passenger booking and ticket information.

Important fields include:

- Requested For
- Source Station
- Destination Station
- Travel Date
- Journey Type
- Passenger Count
- Distance KM
- Duration Minutes
- Base Fare
- Total Fare
- Metro Status
- Ticket Number
- QR Data
- Generated At
- Cancellation Reason
- Refund Amount

---

## Fare Calculation

The fare calculation process is:

1. Passenger selects Source and Destination.
2. The application searches the Metro Route Master.
3. The route distance and duration are retrieved.
4. The distance is matched with an active Metro Fare Rule.
5. Base fare is determined.
6. Journey type and passenger count are applied.
7. Total fare is shown before booking.

Example:

### Miyapur → L B Nagar

- Distance: 29 km
- Estimated Duration: 52 minutes
- Base Fare: ₹69
- Journey Type: Single
- Passengers: 1
- Total Fare: ₹69

---

## Currently Supported Demo Routes

The current version uses configured records in **Metro Route Master**.

| Source Station | Destination Station | Distance | Estimated Duration |
|---|---|---:|---:|
| Miyapur | L B Nagar | 29 km | 52 min |
| Miyapur | Ameerpet | 13 km | 24 min |
| Miyapur | MG Bus Station | 22 km | 39 min |
| KPHB Colony | Ameerpet | 10.4 km | 20 min |
| Kukatpally | Ameerpet | 9.1 km | 18 min |
| Ameerpet | L B Nagar | 16 km | 29 min |
| Ameerpet | Nagole | 17 km | 31 min |
| Ameerpet | Raidurg | 11 km | 21 min |
| Nagole | Raidurg | 28 km | 52 min |
| Parade Ground | Ameerpet | 6.5 km | 14 min |
| Parade Ground | Raidurg | 17.5 km | 34 min |
| JBS Parade Ground | MG Bus Station | 11 km | 20 min |
| Secunderabad West | MG Bus Station | 9.6 km | 17 min |
| Dilsukhnagar | Ameerpet | 13 km | 24 min |
| MG Bus Station | Ameerpet | 9 km | 17 min |

### Reverse Direction Support

The fare service also checks the reverse direction automatically.

For example, when:

`Ameerpet → Raidurg`

is configured, the system can also calculate:

`Raidurg → Ameerpet`

without requiring another Route Master record.

---

## Service Portal

The passenger-facing Metro portal contains two main options:

### Book Metro Ticket

Allows passengers to:

- Select stations
- Select travel date
- Choose journey type
- Enter passenger count
- Preview distance, duration, and fare
- Submit the booking

### My Tickets

Allows passengers to:

- View their own bookings
- View generated ticket numbers
- View QR tickets
- Check booking status
- Cancel eligible tickets
- View refund information

There is no passenger-facing Approvals section in the final demo version.

---

## Cancellation and Refund Flow

```text
Generated Ticket
      ↓
Passenger selects Cancel Ticket
      ↓
Cancellation Reason Entered
      ↓
Metro Status = Cancelled
      ↓
Refund Business Rule Runs
      ↓
Refund Amount Calculated
      ↓
My Tickets Displays Refund Information
```

---

## Security / User Access

The current portal is designed for a logged-in passenger/demo user.

The application:

- Shows only the logged-in passenger's tickets in **My Tickets**
- Validates ticket ownership before cancellation
- Does not expose approval controls on the passenger website
- Keeps approval processing in the backend for the final demo configuration

During development, manual approver-based processing was implemented. The final evaluator version uses backend automatic approval so the complete workflow can be tested using one login.

Additional role-based access control and separate approver access can be enabled for a production implementation.

---

## Requirement Alignment

The original project requirement includes separate administrator/developer responsibilities and manual approval.

This project implements the core functional requirements including:

- Station, route, and fare master data
- Dynamic fare calculation
- Booking through Service Portal
- Pending / Approved / Generated / Cancelled status handling
- Approval workflow logic
- Ticket number generation
- QR generation
- Cancellation and refund logic
- Scheduled processing
- Notifications
- GitHub source control

For the final submitted demo, manual approver interaction is replaced with **automatic backend approval** only so the evaluator can directly verify ticket generation and QR functionality without requiring a separate approver account.

---

## Source Control

The ServiceNow scoped application is connected to GitHub using ServiceNow Source Control.

Development branch:

`sn_instances/dev268719`

Completed changes are merged into:

`main`

This repository contains ServiceNow application metadata generated by the platform.

---

## Demo Steps

An evaluator can test the project using the following flow:

1. Log in using the provided demo credentials.
2. Open the Metro Ticket Booking portal.
3. Click **Book Metro Ticket**.
4. Select one of the supported source/destination combinations.
5. Select the travel date.
6. Select the journey type.
7. Enter the passenger count.
8. Verify the calculated distance, duration, and fare.
9. Submit the booking.
10. The backend automatically completes the approval step.
11. Open **My Tickets**.
12. Verify the generated ticket number and QR code.
13. Cancel an eligible ticket if cancellation testing is required.
14. Verify the cancellation status and refund amount.

For a simple test, use:

`Miyapur → L B Nagar`

Expected demo values:

- Distance: 29 km
- Duration: 52 minutes
- Base Fare: ₹69

---

## Known Limitations / Future Enhancements

### Known Limitations

- Only routes configured in the **Metro Route Master** are currently supported.
- Route distance and duration values are project/demo values.
- The QR code is for academic demonstration and is not connected to the real Hyderabad Metro gate validation system.
- The final demonstration configuration uses automatic backend approval instead of requiring a separate approver login.
- The project is hosted on a ServiceNow Personal Developer Instance, so availability depends on the PDI remaining active.
- Email delivery may depend on ServiceNow PDI email configuration.

### Future Enhancements

- Support all station combinations using dynamic route calculation.
- Re-enable a dedicated role-based approver portal for production use.
- Add QR validation for metro entry and exit.
- Add payment gateway integration.
- Add reporting and analytics for bookings and cancellations.

---

## Project Status

Core functionality is implemented and integrated:

- Metro Station Master 
- Metro Route Master 
- Metro Fare Rules 
- Fare Calculation 
- Record Producer Booking 
- Backend Validation 
- Manual Approval Workflow Implemented During Development 
- Automatic Approval Enabled for Final Demo 
- Ticket Number Generation 
- QR Ticket Generation 
- My Tickets Portal 
- Ticket Cancellation 
- Refund Calculation 
- Scheduled Expired-Booking Cancellation 
- GitHub Source Control 

---

## Disclaimer

This project was developed for academic and demonstration purposes.

It is not an official Hyderabad Metro Rail or L&T Metro Rail Hyderabad ticketing application.

The route distance, duration, approval process, QR implementation, cancellation policy, and refund rules used in this project are part of the academic project design unless otherwise stated.
