Inventory Reservation System

A backend-focused inventory reservation system built using Node.js, Express, MySQL, and Postman.

This project solves a real-world e-commerce concurrency problem where multiple users attempt to reserve the same product simultaneously during checkout.

The system temporarily reserves inventory during checkout and prevents overselling using MySQL transactions and row-level locking.

Tech Stack:

Node.js
Express.js
MySQL
Postman
HTML/CSS/JavaScript
VS Code
GitHub

Features:
Product listing API
Warehouse listing API
Inventory reservation system
Reservation confirmation
Reservation release/cancellation
Reservation expiry handling
Concurrency-safe stock reservation
Frontend pages for reservation flow
API testing with Postman

Problem Statement

In e-commerce systems, payment confirmation may take several minutes.

If stock is reduced only after payment succeeds:

multiple customers may purchase the same unit
overselling occurs

If stock is reduced immediately:

abandoned carts reduce available inventory unnecessarily

Solution:

temporarily reserve stock during checkout
confirm reservation after payment
release reservation if payment fails or expires.

Reservation Workflow:
Reserve
Lock inventory row
Check available stock
Increase reserved_stock
Create reservation
Commit transaction
Confirm
Verify reservation not expired
Update status to CONFIRMED
Release
Lock reservation row
Reduce reserved_stock
Mark reservation RELEASED
Commit transaction
Reservation Expiry

Reservations expire after 10 minutes.

Expired reservations:

cannot be confirmed
should be released automatically

Current implementation:

lazy expiry checking during confirmation

Production improvement:

scheduled cron jobs
background workers
Frontend Features
Product listing page
Reserve button
Checkout page
Live countdown timer
Confirm purchase button
Cancel reservation button
Error handling
Error Handling
409 Conflict

Returned when stock unavailable.

Example:

{
  "message": "Not enough stock available"
}
410 Gone

Returned when reservation expired.

Example:

{
  "message": "Reservation expired"
}
Installation & Setup
1. Clone Repository
git clone <repository-url>
2. Install Dependencies
npm install
3. Configure Database

Create MySQL database:

CREATE DATABASE inventory_system;

Run:

sql/schema.sql
4. Configure db.js

Update:

MySQL username
password
database name
5. Start Server
node server.js

Server runs on:

http://localhost:5000
API Testing

All APIs were tested using Postman.

Tested scenarios:

successful reservation
insufficient stock
reservation confirmation
reservation release
expired reservation
Concurrency Testing

Two simultaneous reservation requests were sent for the last available unit.

Expected behavior:

one request succeeds
second request returns 409 Conflict

This confirms:

race conditions are prevented
inventory consistency maintained
Future Improvements
JWT authentication
Redis distributed locking
Automatic expiry workers
WebSocket real-time inventory updates
React frontend
Docker deployment
Idempotency keys

Trade-offs:
Used simple HTML frontend for faster development
Used lazy expiry handling instead of cron jobs
Focused primarily on concurrency correctness and API reliability


Author
Sunanda Tallam
