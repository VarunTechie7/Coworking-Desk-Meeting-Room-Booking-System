#API Endpoint List

Base URL:

http://localhost:8080/api

All protected APIs require a JWT token:

Authorization: Bearer <JWT_TOKEN>
1. Authentication APIs
1.1 Register Member

POST /auth/register

Registers a new member.

Input
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "Password@123",
  "phone": "9876543210",
  "location": "Chennai"
}
Output — 201 Created
{
  "message": "Member registered successfully",
  "userId": 1
}
1.2 Login

POST /auth/login

Authenticates a user and returns a JWT token.

Input
{
  "email": "john@example.com",
  "password": "Password@123"
}
Output — 200 OK
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "userId": 1,
  "name": "John Doe",
  "role": "MEMBER"
}
1.3 Get Profile

GET /auth/profile

Authentication: Required

Input

No request body.

Output — 200 OK
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "9876543210",
  "location": "Chennai",
  "role": "MEMBER"
}
1.4 Update Profile

PUT /auth/profile

Authentication: Required

Input
{
  "name": "John Smith",
  "phone": "9876543210",
  "location": "Chennai"
}
Output — 200 OK
{
  "message": "Profile updated successfully",
  "user": {
    "id": 1,
    "name": "John Smith",
    "email": "john@example.com",
    "phone": "9876543210",
    "location": "Chennai",
    "role": "MEMBER"
  }
}
2. Workspace Category APIs
2.1 Get Workspace Categories

GET /workspace-categories

Returns all available workspace categories.

Input

No request body.

Output — 200 OK
[
  {
    "id": 1,
    "name": "HOT_DESK",
    "description": "Flexible shared desk"
  },
  {
    "id": 2,
    "name": "DEDICATED_DESK",
    "description": "Dedicated individual desk"
  },
  {
    "id": 3,
    "name": "MEETING_ROOM",
    "description": "Private meeting room"
  },
  {
    "id": 4,
    "name": "CONFERENCE_ROOM",
    "description": "Large conference room"
  }
]
3. Workspace Browse & Search APIs
3.1 Get All Workspaces

GET /workspaces

Returns all available workspaces.

Input

No request body.

Optional query parameters:

?page=0&size=10
Output — 200 OK
[
  {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM",
    "capacity": 8,
    "floor": 2,
    "hourlyRate": 500,
    "dailyRate": 2500,
    "location": "Chennai",
    "status": "AVAILABLE"
  }
]
3.2 Get Workspace Details

GET /workspaces/{id}

Returns detailed information about a workspace.

Input

Path parameter:

id = 1
Output — 200 OK
{
  "id": 1,
  "name": "Meeting Room A",
  "category": {
    "id": 3,
    "name": "MEETING_ROOM"
  },
  "hub": {
    "id": 1,
    "name": "WorkHub Chennai",
    "location": "Chennai",
    "address": "Anna Nagar, Chennai"
  },
  "capacity": 8,
  "floor": 2,
  "hourlyRate": 500,
  "dailyRate": 2500,
  "description": "Private meeting room with projector",
  "amenities": [
    "High-Speed Wi-Fi",
    "Projector",
    "Whiteboard"
  ],
  "status": "AVAILABLE"
}
3.3 Search Workspaces

GET /workspaces/search

Searches workspaces by location or keyword.

Input

Query parameters:

?location=Chennai

Example:

GET /api/workspaces/search?location=Chennai
Output — 200 OK
[
  {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM",
    "capacity": 8,
    "hourlyRate": 500,
    "location": "Chennai",
    "status": "AVAILABLE"
  }
]
3.4 Filter Workspaces

GET /workspaces/filter

Filters workspaces based on different criteria.

Input

Example:

GET /api/workspaces/filter?location=Chennai&type=MEETING_ROOM&capacity=8&minPrice=300&maxPrice=1000

Supported parameters:

Parameter	Description
location	Workspace location
type	Workspace category
capacity	Minimum capacity
minPrice	Minimum hourly price
maxPrice	Maximum hourly price
amenity	Required amenity
Output — 200 OK
[
  {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM",
    "capacity": 8,
    "hourlyRate": 500,
    "location": "Chennai",
    "amenities": [
      "Projector",
      "Whiteboard"
    ],
    "status": "AVAILABLE"
  }
]
4. Time Slot APIs
4.1 Get Available Slots

GET /workspaces/{workspaceId}/slots

Returns the available time slots for a workspace on a specific date.

Input

Query parameter:

?date=2026-09-25

Example:

GET /api/workspaces/1/slots?date=2026-09-25
Output — 200 OK
[
  {
    "slotId": 101,
    "date": "2026-09-25",
    "startTime": "09:00",
    "endTime": "10:00",
    "status": "AVAILABLE"
  },
  {
    "slotId": 102,
    "date": "2026-09-25",
    "startTime": "10:00",
    "endTime": "11:00",
    "status": "BOOKED"
  },
  {
    "slotId": 103,
    "date": "2026-09-25",
    "startTime": "11:00",
    "endTime": "12:00",
    "status": "AVAILABLE"
  }
]
4.2 Check Slot Availability

GET /slots/availability

Checks whether a workspace is available for the requested period.

Input
GET /api/slots/availability?workspaceId=1&date=2026-09-25&startTime=10:00&endTime=12:00
Output — Available
{
  "workspaceId": 1,
  "date": "2026-09-25",
  "startTime": "10:00",
  "endTime": "12:00",
  "available": true,
  "message": "Workspace is available"
}
Output — Not Available
{
  "workspaceId": 1,
  "date": "2026-09-25",
  "startTime": "10:00",
  "endTime": "12:00",
  "available": false,
  "message": "Workspace is already booked"
}
5. Member Booking APIs
5.1 Create Booking

POST /bookings

Authentication: Required — MEMBER

Creates a workspace reservation.

Input
{
  "workspaceId": 1,
  "bookingDate": "2026-09-25",
  "startTime": "10:00",
  "endTime": "12:00",
  "numberOfGuests": 5
}
Output — 201 Created
{
  "message": "Booking created successfully",
  "booking": {
    "id": 1001,
    "bookingNumber": "BK-20260925-1001",
    "workspaceId": 1,
    "workspaceName": "Meeting Room A",
    "bookingDate": "2026-09-25",
    "startTime": "10:00",
    "endTime": "12:00",
    "numberOfGuests": 5,
    "totalAmount": 1000,
    "status": "CONFIRMED"
  }
}
5.2 Get My Bookings

GET /bookings

Authentication: Required — MEMBER

Returns the logged-in member's booking history.

Input

Optional:

?status=CONFIRMED
Output — 200 OK
[
  {
    "id": 1001,
    "bookingNumber": "BK-20260925-1001",
    "workspaceName": "Meeting Room A",
    "bookingDate": "2026-09-25",
    "startTime": "10:00",
    "endTime": "12:00",
    "totalAmount": 1000,
    "status": "CONFIRMED"
  }
]
5.3 Get Booking Details

GET /bookings/{id}

Authentication: Required — MEMBER

Input

Path parameter:

id = 1001
Output — 200 OK
{
  "id": 1001,
  "bookingNumber": "BK-20260925-1001",
  "member": {
    "id": 1,
    "name": "John Doe"
  },
  "workspace": {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM"
  },
  "hub": {
    "id": 1,
    "name": "WorkHub Chennai",
    "location": "Chennai"
  },
  "bookingDate": "2026-09-25",
  "startTime": "10:00",
  "endTime": "12:00",
  "numberOfGuests": 5,
  "totalAmount": 1000,
  "status": "CONFIRMED"
}
5.4 Cancel Booking

PUT /bookings/{id}/cancel

Authentication: Required — MEMBER

Input

Path parameter:

id = 1001

No request body required.

Output — 200 OK
{
  "message": "Booking cancelled successfully",
  "bookingId": 1001,
  "status": "CANCELLED"
}

The associated time slot becomes available again.

5.5 Reschedule Booking

PUT /bookings/{id}/reschedule

Authentication: Required — MEMBER

Input
{
  "bookingDate": "2026-09-26",
  "startTime": "14:00",
  "endTim:"12:00"
}
### Output — 200 OK

```json
{
  "message": "Booking rescheduled successfully",
  "bookingId": 1001,
  "bookingDate": "2026-09-26",
  "startTime": "14:00",
  "endTime": "16:00",
  "status": "CONFIRMED"
}
```

### Output — 409 Conflict

```json
{
  "status": 409,
  "error": "BOOKING_CONFLICT",
  "message": "The selected workspace is not available for the requested time"
}
```

---

# 6. Workspace Admin APIs

All Workspace Admin APIs require:

```http
Authorization: Bearer <WORKSPACE_ADMIN_JWT>
```

Required role:

```text
WORKSPACE_ADMIN
```

---

## 6.1 Workspace Admin Login

**POST** `/workspace-admin/auth/login`

### Input

```json
{
  "email": "admin@workhub.com",
  "password": "Admin@123"
}
```

### Output — 200 OK

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "userId": 10,
  "name": "Workspace Manager",
  "role": "WORKSPACE_ADMIN"
}
```

---

# 7. Workspace Hub Management

## 7.1 Get Hub Details

**GET** `/workspace-admin/hubs/{id}`

### Input

```text
id = 1
```

### Output — 200 OK

```json
{
  "id": 1,
  "name": "WorkHub Chennai",
  "description": "Premium coworking space",
  "location": "Chennai",
  "address": "Anna Nagar, Chennai",
  "contactNumber": "9876543210",
  "status": "ACTIVE"
}
```

---

## 7.2 Update Hub

**PUT** `/workspace-admin/hubs/{id}`

### Input

```json
{
  "name": "WorkHub Chennai",
  "description": "Premium coworking and meeting space",
  "location": "Chennai",
  "address": "Anna Nagar, Chennai",
  "contactNumber": "9876543210"
}
```

### Output — 200 OK

```json
{
  "message": "Workspace hub updated successfully",
  "hub": {
    "id": 1,
    "name": "WorkHub Chennai",
    "location": "Chennai",
    "address": "Anna Nagar, Chennai",
    "status": "ACTIVE"
  }
}
```

---

# 8. Workspace Management APIs

## 8.1 Get All Workspaces

**GET** `/workspace-admin/hubs/{hubId}/workspaces`

### Input

```text
hubId = 1
```

### Output — 200 OK

```json
[
  {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM",
    "capacity": 8,
    "floor": 2,
    "hourlyRate": 500,
    "dailyRate": 2500,
    "status": "AVAILABLE"
  },
  {
    "id": 2,
    "name": "Hot Desk 01",
    "category": "HOT_DESK",
    "capacity": 1,
    "floor": 1,
    "hourlyRate": 100,
    "dailyRate": 600,
    "status": "AVAILABLE"
  }
]
```

---

## 8.2 Add Workspace

**POST** `/workspace-admin/hubs/{hubId}/workspaces`

### Input

```json
{
  "name": "Meeting Room B",
  "categoryId": 3,
  "workspaceNumber": "MR-B-02",
  "capacity": 10,
  "floor": 2,
  "hourlyRate": 600,
  "dailyRate": 3000,
  "description": "Large meeting room with projector",
  "status": "AVAILABLE",
  "amenityIds": [
    1,
    2,
    3
  ]
}
```

### Output — 201 Created

```json
{
  "message": "Workspace created successfully",
  "workspace": {
    "id": 5,
    "name": "Meeting Room B",
    "category": "MEETING_ROOM",
    "capacity": 10,
    "floor": 2,
    "hourlyRate": 600,
    "dailyRate": 3000,
    "status": "AVAILABLE"
  }
}
```

---

## 8.3 Get Workspace

**GET** `/workspace-admin/workspaces/{id}`

### Input

```text
id = 5
```

### Output — 200 OK

```json
{
  "id": 5,
  "name": "Meeting Room B",
  "workspaceNumber": "MR-B-02",
  "category": "MEETING_ROOM",
  "capacity": 10,
  "floor": 2,
  "hourlyRate": 600,
  "dailyRate": 3000,
  "description": "Large meeting room with projector",
  "amenities": [
    "High-Speed Wi-Fi",
    "Projector",
    "Whiteboard"
  ],
  "status": "AVAILABLE"
}
```

---

## 8.4 Update Workspace

**PUT** `/workspace-admin/workspaces/{id}`

### Input

```json
{
  "name": "Meeting Room B",
  "categoryId": 3,
  "capacity": 12,
  "floor": 2,
  "hourlyRate": 650,
  "dailyRate": 3200,
  "description": "Large conference room",
  "status": "AVAILABLE"
}
```

### Output — 200 OK

```json
{
  "message": "Workspace updated successfully",
  "workspace": {
    "id": 5,
    "name": "Meeting Room B",
    "capacity": 12,
    "hourlyRate": 650,
    "dailyRate": 3200,
    "status": "AVAILABLE"
  }
}
```

---

## 8.5 Delete Workspace

**DELETE** `/workspace-admin/workspaces/{id}`

### Input

```text
id = 5
```

### Output — 200 OK

```json
{
  "message": "Workspace deleted successfully",
  "workspaceId": 5
}
```

> Recommended implementation: deactivate the workspace instead of physically deleting it if historical bookings exist.

---

# 9. Workspace Price APIs

## 9.1 Update Workspace Price

**PUT** `/workspace-admin/workspaces/{id}/price`

### Input

```json
{
  "hourlyRate": 700,
  "dailyRate": 3500
}
```

### Output — 200 OK

```json
{
  "message": "Workspace price updated successfully",
  "workspaceId": 5,
  "hourlyRate": 700,
  "dailyRate": 3500
}
```

---

# 10. Workspace Availability APIs

## 10.1 Update Workspace Availability

**PUT** `/workspace-admin/workspaces/{id}/availability`

### Input

```json
{
  "status": "AVAILABLE"
}
```

Possible values:

```text
AVAILABLE
UNAVAILABLE
MAINTENANCE
```

### Output — 200 OK

```json
{
  "message": "Workspace availability updated successfully",
  "workspaceId": 5,
  "status": "AVAILABLE"
}
```

---

# 11. Workspace Admin Booking APIs

## 11.1 Get All Bookings

**GET** `/workspace-admin/bookings`

### Input

Optional query parameters:

```text
?status=CONFIRMED
```

or:

```text
?date=2026-09-25
```

or:

```text
?workspaceId=5
```

### Output — 200 OK

```json
[
  {
    "id": 1001,
    "bookingNumber": "BK-20260925-1001",
    "memberName": "John Doe",
    "workspaceName": "Meeting Room A",
    "bookingDate": "2026-09-25",
    "startTime": "10:00",
    "endTime": "12:00",
    "totalAmount": 1000,
    "status": "CONFIRMED"
  }
]
```

---

## 11.2 Get Booking Details

**GET** `/workspace-admin/bookings/{id}`

### Input

```text
id = 1001
```

### Output — 200 OK

```json
{
  "id": 1001,
  "bookingNumber": "BK-20260925-1001",
  "member": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "workspace": {
    "id": 1,
    "name": "Meeting Room A",
    "category": "MEETING_ROOM"
  },
  "bookingDate": "2026-09-25",
  "startTime": "10:00",
  "endTime": "12:00",
  "numberOfGuests": 5,
  "totalAmount": 1000,
  "status": "CONFIRMED"
}
```

---

## 11.3 Update Booking Status

**PUT** `/workspace-admin/bookings/{id}/status`

### Input

```json
{
  "status": "COMPLETED"
}
```

Possible values:

```text
PENDING
CONFIRMED
CANCELLED
COMPLETED
```

### Output — 200 OK

```json
{
  "message": "Booking status updated successfully",
  "bookingId": 1001,
  "status": "COMPLETED"
}
```

---

# 12. Admin APIs

All Admin APIs require:

```http
Authorization: Bearer <ADMIN_JWT>
```

Required role:

```text
ADMIN
```

---

## 12.1 Admin Login

**POST** `/admin/auth/login`

### Input

```json
{
  "email": "admin@coworking.com",
  "password": "Admin@123"
}
```

### Output — 200 OK

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "userId": 100,
  "name": "System Admin",
  "role": "ADMIN"
}
```

---

# 13. Admin User Management

## 13.1 Get All Users

**GET** `/admin/users`

### Input

Optional:

```text
?role=MEMBER
```

### Output — 200 OK

```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "9876543210",
    "role": "MEMBER",
    "status": "ACTIVE",
    "createdAt": "2026-09-20T10:30:00"
  },
  {
    "id": 10,
    "name": "Workspace Manager",
    "email": "manager@workhub.com",
    "phone": "9876543211",
    "role": "WORKSPACE_ADMIN",
    "status": "ACTIVE",
    "createdAt": "2026-09-20T11:00:00"
  }
]
```

---

## 13.2 Get User

**GET** `/admin/users/{id}`

### Input

```text
id = 1
```

### Output — 200 OK

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "9876543210",
  "location": "Chennai",
  "role": "MEMBER",
  "status": "ACTIVE",
  "createdAt": "2026-09-20T10:30:00"
}
```

---

## 13.3 Update User

**PUT** `/admin/users/{id}`

### Input

```json
{
  "name": "John Smith",
  "phone": "9876543210",
  "location": "Chennai",
  "status": "ACTIVE"
}
```

### Output — 200 OK

```json
{
  "message": "User updated successfully",
  "user": {
    "id": 1,
    "name": "John Smith",
    "phone": "9876543210",
    "location": "Chennai",
    "status": "ACTIVE"
  }
}
```

---

## 13.4 Deactivate User

**DELETE** `/admin/users/{id}`

### Input

```text
id = 1
```

### Output — 200 OK

```json
{
  "message": "User deactivated successfully",
  "userId": 1
}
```

---

# 14. Admin Workspace Hub APIs

## 14.1 Get All Hubs

**GET** `/admin/hubs`

### Input

No request body.

### Output — 200 OK

```json
[
  {
    "id": 1,
    "name": "WorkHub Chennai",
    "location": "Chennai",
    "address": "Anna Nagar, Chennai",
    "status": "ACTIVE"
  }
]
```


