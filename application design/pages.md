Frontend Flow

The ReactJS application will follow this structure:

```text
React Application
│
├── Public Pages
│   ├── Home
│   ├── Workspace Listing
│   ├── Workspace Details
│   ├── Login
│   └── Register
│
├── Member Pages
│   ├── Slot Selection
│   ├── Booking
│   ├── Booking Confirmation
│   ├── My Bookings
│   └── Profile
│
├── Workspace Admin Pages
│   ├── Dashboard
│   ├── Hub Management
│   ├── Workspace Management
│   └── Booking Management
│
└── Admin Pages
    ├── Dashboard
    ├── User Management
    ├── Workspace Management
    ├── Category Management
    └── Booking Management
```

---
Customer / Member Complete Flow
                    HOME PAGE
                        │
                        ↓
                Login / Register
                        │
                        ↓
                 Search Workspace
                        │
                        ↓
        Filter Location / Type / Capacity
                  / Price / Amenities
                        │
                        ↓
              Workspace Listing
                        │
                        ↓
             Workspace Details
                        │
                        ↓
              Select Date & Time
                        │
                        ↓
               Check Availability
                        │
                  ┌─────┴─────┐
                  ↓           ↓
              Available      Booked
                  │           │
                  ↓           └──→ Select another slot
             Booking Details
                  │
                  ↓
             Confirm Booking
                  │
                  ↓
             Booking Created
                  │
                  ↓
          Booking Confirmation
                  │
                  ↓
              My Bookings
                  │
             ┌────┴────┐
             ↓         ↓
          View       Cancel /
        Booking     Reschedule
        
 Workspace Admin Complete Flow
                  LOGIN
                    │
                    ↓
            ADMIN DASHBOARD
                    │
                    ↓
             MANAGE HUB
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
     Hub Details          Amenities
          │
          ↓
     Manage Workspaces
          │
    ┌─────┼─────┬─────┐
    ↓     ↓     ↓     ↓
   Add   Edit  Delete Price
    │
    └──────────┬──────────┐
               ↓          ↓
          Availability   Slots
               │
               ↓
         View Bookings
               │
               ↓
       Update Booking Status
       
 Admin Complete Flow
                 ADMIN LOGIN
                      │
                      ↓
               ADMIN DASHBOARD
                      │
       ┌──────────────┼───────────────┐
       ↓              ↓               ↓
     Users           Hubs          Workspaces
       │              │               │
       ↓              ↓               ↓
   Manage Users   Manage Hubs    Manage Inventory
       │
       └──────────────┬───────────────┘
                      ↓
              Workspace Categories
                      │
                      ↓
                 All Bookings
Final System Architecture
                    ReactJS Frontend
                           │
                           │ REST / JSON
                           ↓
                  Spring Boot Backend
                           │
             ┌─────────────┼─────────────┐
             ↓             ↓             ↓
        Controllers      Security      Exception
             │          JWT / RBAC      Handler
             ↓
          Services
             │
             ↓
        Repositories
             │
             ↓
        PostgreSQL
