# Airbnb Clone Backend Requirements

This document outlines the technical and functional requirements for key backend features of the Airbnb Clone project.

---

## 1. User Authentication & Management

**Description:**  
Handles user signup, login, logout, profile management, and role-based access control.

**API Endpoints:**  
- `POST /api/users/register` – Create a new user  
- `POST /api/users/login` – Authenticate user and return token  
- `GET /api/users/:id` – Retrieve user profile  
- `PUT /api/users/:id` – Update user profile  
- `DELETE /api/users/:id` – Delete user account (admin only)

**Input Specifications:**  
- `first_name` (string, required)  
- `last_name` (string, required)  
- `email` (string, required, unique, valid email format)  
- `password` (string, required, min 8 characters)  
- `role` (enum: guest, host, admin, required)

**Output Specifications:**  
- Success: User object with `user_id`, timestamps, and role  
- Error: Validation errors or authentication errors

**Validation Rules:**  
- Email format validation  
- Password strength (minimum 8 characters)  
- Role must be one of the predefined enums

**Performance Criteria:**  
- Response time < 500ms for login/register endpoints  
- Handle at least 100 concurrent requests per endpoint

---

## 2. Property Management

**Description:**  
Allows hosts to create, read, update, and delete property listings.

**API Endpoints:**  
- `POST /api/properties` – Add a new property  
- `GET /api/properties` – Retrieve all properties  
- `GET /api/properties/:id` – Retrieve property details  
- `PUT /api/properties/:id` – Update property details  
- `DELETE /api/properties/:id` – Delete a property

**Input Specifications:**  
- `name` (string, required)  
- `description` (text, required)  
- `location` (string, required)  
- `price_per_night` (decimal, required, > 0)  
- `host_id` (UUID, required)

**Output Specifications:**  
- Success: Property object with `property_id`, timestamps, host info  
- Error: Validation or authorization errors

**Validation Rules:**  
- Price must be > 0  
- Host must exist in the system  
- Non-empty required fields

**Performance Criteria:**  
- Retrieve property listings in < 1 second for 1000 properties  
- Support up to 50 concurrent property creation requests

---

## 3. Booking System

**Description:**  
Manages property bookings, including creation, modification, cancellation, and availability checks.

**API Endpoints:**  
- `POST /api/bookings` – Create a booking  
- `GET /api/bookings/:id` – Get booking details  
- `PUT /api/bookings/:id` – Update booking dates/status  
- `DELETE /api/bookings/:id` – Cancel a booking  

**Input Specifications:**  
- `property_id` (UUID, required)  
- `user_id` (UUID, required)  
- `start_date` (date, required)  
- `end_date` (date, required, must be after start_date)  
- `status` (enum: pending, confirmed, canceled, default: pending)

**Output Specifications:**  
- Success: Booking object with `booking_id`, total price, timestamps  
- Error: Validation errors, conflicts, or authorization errors

**Validation Rules:**  
- Start date < End date  
- Property must be available for selected dates  
- User must exist and be authenticated

**Performance Criteria:**  
- Check availability and create booking < 500ms  
- Support up to 100 concurrent booking requests

---

## ⚡ Notes
- All endpoints must return JSON responses  
- Authentication via JWT tokens for protected routes  
- All timestamps stored in UTC  
- Proper error handling with standardized HTTP status codes
