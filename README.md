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

## Currently Supported Demo Routes

The current version of the Metro Ticket Generating System uses a configured
**Metro Route Master** to calculate distance, estimated journey duration,
and fare.

The following routes are currently configured and supported:

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

The fare calculation service also checks the reverse direction automatically.

For example, if the following route exists:

`Ameerpet → Raidurg`

then the application can also calculate:

`Raidurg → Ameerpet`

without requiring a second Route Master record.

### Fare Calculation

For a supported route, the application performs the following process:

1. Finds the route in Metro Route Master.
2. Retrieves the configured distance and estimated duration.
3. Uses the route distance to find the applicable Metro Fare Rule.
4. Calculates the base fare.
5. Applies journey type and passenger count.
6. Displays the calculated fare to the passenger before booking.

Example:

`Miyapur → L B Nagar`

- Distance: 29 km
- Estimated Duration: 52 minutes
- Single Journey Fare for 1 passenger: ₹69

### Current Scope / Limitation

This is an academic ServiceNow implementation.

The current application supports the configured demo routes listed above
rather than every possible Hyderabad Metro station-to-station combination.

Station names and fare bands are based on publicly available Hyderabad Metro
information. The route distances and estimated journey durations stored in
the current Route Master are project/demo values used to demonstrate the
ServiceNow booking and fare-calculation workflow.

A future enhancement can calculate routes dynamically from station sequence,
metro line, interchange station, and route-segment information so that all
station combinations can be supported.

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

## Known Limitations / Future Enhancements

### Known Limitations

- Only routes configured in the **Metro Route Master** are currently supported.
- Route distance and duration values are project/demo values.
- The generated QR code is for academic demonstration and is not connected to the real Hyderabad Metro gate validation system.
- The project is hosted on a ServiceNow Personal Developer Instance, so availability depends on the PDI remaining active.
- Email delivery may depend on ServiceNow PDI email configuration.

### Future Enhancements

- Support all metro station combinations using dynamic route calculation.
- Add QR validation for entry and exit.
- Add payment gateway integration.
- Add stronger role-based access for passengers, approvers, and administrators.
- Add reporting and analytics for bookings, cancellations, and fares.
