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
- Automated backend approval
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

### 3. Automated Approval

For the current project demonstration, approval is handled automatically in the backend.

After a valid booking is created:

1. The booking is initially created with `Pending` status.
2. Flow Designer automatically updates the inherited Approval field to `Approved`.
3. The ticket-generation Business Rule detects the approval.
4. A ticket number and QR data are generated.
5. Metro Status changes to `Generated`.

The approval process is not exposed on the passenger website.

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

Email delivery behavior can depend on the ServiceNow Personal Developer Instance email configuration.

---

## Application Flow

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

There is no passenger-facing Approvals section in the current version.

---

## Cancellation and Refund Flow

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

## Security / User Access

The current portal is designed for a passenger login.

The application:

- Shows only the logged-in passenger's tickets in **My Tickets**
- Validates ticket ownership before cancellation
- Does not expose approval controls on the passenger website
- Keeps approval processing in the backend for the current demonstration configuration

Additional role-based access control can be added for a production implementation.

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

1. Open the Metro Ticket Booking portal.
2. Click **Book Metro Ticket**.
3. Select one of the supported source/destination combinations.
4. Select the travel date.
5. Select the journey type.
6. Enter the passenger count.
7. Verify the calculated distance, duration, and fare.
8. Submit the booking.
9. Open **My Tickets**.
10. Verify the generated ticket number and QR code.
11. Cancel an eligible ticket if cancellation testing is required.
12. Verify the cancellation status and refund amount.

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
- The current demonstration configuration uses automatic backend approval instead of a separate approver login.
- The project is hosted on a ServiceNow Personal Developer Instance, so availability depends on the PDI remaining active.
- Email delivery may depend on ServiceNow PDI email configuration.

### Future Enhancements

- Support all station combinations using dynamic route calculation.
- Add a dedicated role-based approver portal for production use.
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
- Automated Approval 
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
