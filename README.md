# Hotel Reservation System

A desktop hotel booking application built with **Java Swing** and **SQLite**. Guests search for available rooms by date, book them, pay (simulated), view their booking details and cancel. An admin manages users and rooms and sees all reservations and payments.

Created as a Java programming task for CodeAlpha.

## Features

**Guest**
- Log in with a user name and password
- Search available rooms by check-in date, check-out date and room category (Standard, Deluxe, Suite)
- Book a room, with the total price calculated from the number of nights
- Pay for a reservation by card or cash (simulated)
- View all own reservations, with full booking details: room, dates, total, status, and payment method, reference and time
- Cancel a reservation that has not started yet
- Dashboard with the number of own reservations

**Admin**
- Same login screen, with the admin role set by the `isAdmin` flag
- Add and update users (user name, password, full name)
- Add and update rooms (room number, category, price per night, active or inactive)
- View all reservations with user name, room number, dates, total, status and whether they are paid
- View all payments
- Dashboard with totals for users, rooms, reservations, payments and income

## How It Works

- **Availability:** a room can be booked if it is active and no `BOOKED` reservation overlaps the chosen dates. A stay may start on the day another stay ends. Cancelled reservations do not block a room.
- **Price:** total = number of nights × price per night. The total is saved with the reservation, so a later price change does not alter existing bookings.
- **Payment simulation:** a card payment needs a 16-digit card number and a 3-digit CVV. A card number ending in `0000` is declined. Cash is accepted without extra details. A successful payment gets a generated reference such as `PAY-261028190035563`. The card number is not stored. A reservation can be paid only once.
- **Cancellation:** a reservation can be cancelled while it is `BOOKED` and its check-in date is today or later. The record stays in the database with the status `CANCELLED`. Payments are not refunded in this simulation.
- **No deletion:** rooms are marked inactive and reservations are marked cancelled, so the history stays consistent.
- User names are stored in lower case.

## Technologies

- Java 17
- Swing (NetBeans GUI Builder for the login, dashboards and admin screens; the four guest screens are written in code)
- SQLite with the `sqlite-jdbc` driver (included in `lib/`)
- NetBeans (Ant project)

## Project Structure

```
HotelReservationSystem/
├── lib/                              # sqlite-jdbc driver
├── hotelreservation.db               # SQLite database file with sample data
└── src/hotelreservationsystem/
    ├── HotelReservationSystem.java   # entry point, opens the login screen
    ├── models/                       # User, Room, RoomCategory, Reservation, ReservationStatus, Payment, PaymentMethod
    ├── db/                           # DataBase connection and UserDao, RoomDao, ReservationDao, PaymentDao
    └── views/
        ├── LoginView, UserDashboard, AdminDashboard
        ├── users/                    # UserBookRoomView, UserMakePaymentView, UserCancelReservationView, UserReservationHistoryView
        └── admin/                    # AdminUserView, AdminRoomView, AdminReservationView, AdminPaymentView
```

## Database

| Table | Purpose | Main columns |
|-------|---------|--------------|
| `users` | Accounts | id, userName (unique), password, fullName, isAdmin |
| `rooms` | Rooms | id, roomNumber (unique), category, pricePerNight, active |
| `reservations` | Bookings | id, userId, roomId, checkIn, checkOut, totalPrice, status (BOOKED / CANCELLED), createdAt |
| `payments` | Payments | id, reservationId, amount, method (CARD / CASH), reference, paidAt |

Dates are stored as text such as `2026-10-25`. The database file `hotelreservation.db` is included in the repository with an admin account and three sample rooms.

## How to Run

1. Clone the repository:
   ```
   git clone https://github.com/MMRSheikh2001/CodeAlpha_Hotel_Reservation_System.git
   ```
2. Open the `HotelReservationSystem` folder as a project in NetBeans (JDK 17 or later).
3. Run the project (F6). The main class is `hotelreservationsystem.HotelReservationSystem`.
4. Log in with the demo admin account, `admin` / `1234`.

The application must be started from the `HotelReservationSystem` folder so that it finds `hotelreservation.db`. NetBeans does this by default. After a Clean and Build, the jar in `dist/` can also be started from that folder with `java -jar dist/HotelReservationSystem.jar`.

## Usage

1. Log in as admin. Add guests in **Users** and rooms in **Rooms**.
2. Log out and log in as a guest.
3. Open **Book Room**, choose the dates, click **Search Rooms**, select a room and click **Book Selected Room**.
4. Open **Make Payment**, select the reservation, choose card or cash and pay.
5. Open **All Reservations** and select a row to see the booking details.
6. Open **Cancel Reservations** to cancel a booking that has not started.
7. Log in as admin again to see the reservations, payments and total income.

## Limitations

- Passwords are stored as plain text, and the demo admin password is shown on the login screen.
- Payments are only simulated, and cancelled reservations are not refunded.
- Table creation and the first admin are not created automatically. The database file in the repository already contains them.
- Dates use the system time zone and are chosen with date fields, not a calendar picker.
- Data is local to one SQLite file, so there is no network or multi-machine use.

## Author

MD Mahbubur Rahman Sheikh — [@MMRSheikh2001](https://github.com/MMRSheikh2001)
