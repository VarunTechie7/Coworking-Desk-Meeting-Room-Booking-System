# Coworking-Desk-Meeting-Room-Booking-System


##  Project Overview

The **Coworking Desk & Meeting Room Booking System** is a web-based workspace reservation platform designed to help professionals and remote teams easily discover, check availability, and reserve coworking spaces, desks, private pods, and meeting rooms.

The system provides a centralized platform where members can browse available workspaces, filter them based on requirements such as workspace type, capacity, price, and amenities, select available time slots, and manage their bookings.

The application also provides a **Workspace Admin** interface for managing coworking hubs, workspace inventory, pricing, availability, and customer bookings. A system **Admin** can manage users, workspace categories, workspaces, and overall bookings.

The application follows a **single-tenant architecture**, meaning the system operates for one coworking business/platform without separate tenant management or tenant switching.

### Key Features

* Member registration and login
* JWT-based authentication and authorization
* Browse and search coworking workspaces
* Filter workspaces by type, capacity, price, location, and amenities
* View workspace and coworking hub details
* View real-time workspace slot availability
* Select date and time slots
* Create workspace bookings
* Prevent double-booking of workspaces
* View booking history
* Cancel bookings
* Reschedule bookings
* Member profile management
* Workspace Admin dashboard
* Workspace and hub management
* Workspace pricing and availability management
* Booking management for Workspace Admins
* Admin user management
* Workspace category management
* Centralized exception handling
* RESTful APIs
* API validation using Postman / Swagger
* Role-based access control

### User Roles

The system supports three roles:

**Member**

* Browse workspaces
* Search and filter workspaces
* Check slot availability
* Book workspaces
* View bookings
* Cancel/reschedule bookings
* Manage profile

**Workspace Admin**

* Manage coworking hub details
* Add, update, and remove workspaces
* Manage workspace pricing
* Manage workspace availability
* View and manage bookings

**Admin**

* Manage members and Workspace Admins
* Manage workspace hubs
* Manage workspace categories
* Manage workspaces
* View all bookings
* Manage overall platform data

### Booking Workflow

```text
Register / Login
       ↓
Browse Workspaces
       ↓
Search / Filter
       ↓
View Workspace Details
       ↓
Check Available Slots
       ↓
Select Date & Time
       ↓
Confirm Booking
       ↓
Booking Created
       ↓
View Booking History
```

The system uses transactional booking logic to ensure that the same workspace cannot be booked by multiple users for the same time period.

---

## 🛠️ Tech Stack

### Frontend

* **ReactJS** — Frontend application development
* **JavaScript** — Application logic
* **React Hooks** — State and lifecycle management
* **React Router** — Client-side routing
* **Axios** — REST API communication
* **HTML5 / CSS3** — User interface

### Backend

* **Java** — Backend programming language
* **Spring Boot** — Backend application framework
* **Spring Web** — RESTful API development
* **Spring Data JPA** — Database access and ORM
* **Spring Security** — Authentication and authorization
* **JWT (JSON Web Token)** — Secure API authentication
* **Maven** — Dependency management and build automation

### Database

* **PostgreSQL** — Relational database
* **Hibernate / JPA** — Object-relational mapping

### API & Testing

* **REST API** — Frontend/backend communication
* **JSON** — API request and response format
* **Swagger / OpenAPI** — API documentation
* **Postman** — API testing and validation

### Development & Version Control

* **Git** — Version control
* **GitHub** — Source code and project documentation management

### Architecture

The backend follows a layered architecture:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
PostgreSQL Database
```

Security is implemented using:

```text
ReactJS
   ↓
REST API
   ↓
Spring Security
   ↓
JWT Authentication
   ↓
Role-Based Authorization
   ↓
Spring Boot
   ↓
PostgreSQL
```

### Project Structure

```text
coworking-booking-system/
│
├── backend/
│   ├── src/
│   └── pom.xml
│
├── frontend/
│   ├── src/
│   └── package.json
│
├── database/
│   └── schema.sql
│
├── docs/
│   ├── ER-Diagram.md
│   ├── API-Documentation.md
│   ├── Architecture.md
│   └── Workflow.md
│
├── postman/
│   └── Coworking-Booking-API.json
│
└── README.md
```
