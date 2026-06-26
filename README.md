# Auction Platform

A full-stack real-time auction platform enabling users to participate in live auctions, place bids, manage items and wishlists, with comprehensive admin controls and real-time notifications.

**Repository:** [group-project-team-03](https://github.com/ShaimaaAliECE/group-project-team-03)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites & Installation](#prerequisites--installation)
- [Environment Configuration](#environment-configuration)
- [Running the Application](#running-the-application)
- [API Endpoints Overview](#api-endpoints-overview)
- [Database Models](#database-models)
- [Authentication & Authorization](#authentication--authorization)
- [Frontend Routes & Components](#frontend-routes--components)
- [Key Features Explained](#key-features-explained)
- [Development Workflow](#development-workflow)
- [Docker Deployment](#docker-deployment)
- [Troubleshooting](#troubleshooting)
- [Team Members](#team-members)

---

## Overview

The Auction Platform is a comprehensive e-commerce solution designed for conducting real-time auctions. It provides an intuitive user interface for bidders, robust admin tools for auction management, and a sophisticated backend supporting live bidding, notifications, and payment management through a virtual currency system called "Kogbucks."

### Key Statistics
- **Users:** Self-registration with email verification via OTP
- **Auctions:** Support for live, scheduled, and completed auctions
- **Items:** Categorized auction items with detailed information
- **Transactions:** Complete bid history and payment tracking
- **Real-time:** Live bidding updates and notifications

---

## Features

### User Features
- **Authentication:** User registration, login, and email OTP verification
- **Live Auctions:** Participate in real-time auctions with live bidding
- **Bidding System:** Place, outbid, and track bids with validation rules
  - Minimum one bid required in first 30 minutes to continue bidding
- **Wishlist Management:** Add, view, and manage favorite items
- **Notifications:** Real-time alerts for outbids, wins, and auction updates
- **Chat & Messaging:** Communication during auctions and direct messaging
- **User Profile:** View and manage account information and bid history
- **Kogbucks System:** Virtual currency for auction participation

### Admin Features
- **Admin Dashboard:** Overview of auctions, users, and system statistics
- **Auction Management:** Create, edit, schedule, and close auctions
- **Item Management:** Add, categorize, and manage auction items
- **User Management:** View user information and activity
- **Auction Reports:** Detailed analytics and reports on auction performance
- **Chat Management:** Monitor and manage user communications
- **Notifications:** Send system-wide or targeted notifications to users
- **Prize Draw:** Automated prize distribution for auction winners

---

## Tech Stack

### Backend
| Technology | Purpose |
|-----------|---------|
| **Node.js** | Server runtime |
| **Express.js v5.2** | Web framework and routing |
| **MongoDB + Mongoose v9.1** | NoSQL database and ODM |
| **JWT (jsonwebtoken)** | Token-based authentication |
| **bcryptjs v2.4** | Password hashing and encryption |
| **Nodemailer v6.10** | Email notifications and OTP delivery |
| **CORS** | Cross-origin resource sharing |
| **dotenv** | Environment variable management |

### Frontend
| Technology | Purpose |
|-----------|---------|
| **Angular v21.1** | Frontend framework |
| **TypeScript v5.9** | Type-safe JavaScript |
| **RxJS v7.8** | Reactive programming |
| **Angular Forms** | Form handling and validation |
| **Angular Router** | Client-side routing |

### Development Tools
| Tool | Purpose |
|------|---------|
| **npm v10.9** | Package manager |
| **nodemon** | Development server auto-reload |
| **Docker** | Containerization for deployment |
| **Nginx** | Web server and reverse proxy |
| **Vitest** | Testing framework |

---

## Project Structure

```
group-project-team-03/
├── README.md                          # Project documentation
│
├── backend/                           # Node.js/Express API server
│   ├── server.js                      # Main server entry point
│   ├── db.js                          # MongoDB connection setup
│   ├── Dockerfile                     # Backend Docker configuration
│   ├── package.json                   # Backend dependencies
│   │
│   ├── middleware/                    # Authentication middleware
│   │   ├── userAuth.js                # JWT user authentication
│   │   └── adminAuth.js               # JWT admin authentication
│   │
│   ├── models/                        # Mongoose database schemas
│   │   ├── User.js                    # User profile and credentials
│   │   ├── Admin.js                   # Admin credentials
│   │   ├── Auction.js                 # Auction details and status
│   │   ├── Items.js                   # Auction items
│   │   ├── Transaction.js             # Bid and payment records
│   │   ├── AuctionChatMessage.js      # Auction-specific chat
│   │   ├── AuctionDirectMessage.js    # Direct user messages
│   │   ├── Notification.js            # User notifications
│   │   ├── EmailOtp.js                # OTP for email verification
│   │   ├── Wishlist.js                # User wishlist items
│   │   └── Note: Additional models exist
│   │
│   ├── routes/                        # API route handlers
│   │   ├── admin.js                   # Admin operations API
│   │   ├── auctions.js                # Auction management API
│   │   ├── items.js                   # Item management API
│   │   ├── authenticationProcess.js   # User auth and OTP API
│   │   ├── notificationRoutes.js      # Notification API
│   │   ├── wishlist.js                # Wishlist API
│   │   ├── kogbucks.js                # Virtual currency API
│   │   ├── auctionTimer.js            # Auction timing API
│   │   ├── userInfoRoute.js           # User info API
│   │   └── More route files...
│   │
│   ├── utils/                         # Utility functions
│   │   ├── auction.js                 # Auction business logic
│   │   ├── auctionNotification.js     # Notification helper
│   │   ├── finalizeAuction.js         # Auction completion logic
│   │   ├── prizeDraw.js               # Prize distribution
│   │   └── wishlistSync.js            # Wishlist synchronization
│   │
│   └── scripts/                       # Database seeding scripts
│       ├── backfill-item-categories.js    # Seed item categories
│       └── backfill-auction-invited-reps.js # Seed auction data
│
└── frontend/                          # Angular application
    ├── angular.json                   # Angular CLI configuration
    ├── tsconfig.json                  # TypeScript configuration
    ├── Dockerfile                     # Frontend Docker image
    ├── nginx.conf                     # Nginx configuration
    ├── proxy.conf.json                # Dev server proxy config
    ├── package.json                   # Frontend dependencies
    │
    ├── src/
    │   ├── index.html                 # Main HTML file
    │   ├── main.ts                    # Angular bootstrap entry
    │   ├── styles.css                 # Global styles
    │   │
    │   └── app/
    │       ├── app.ts                 # Root component
    │       ├── app.routes.ts          # Route definitions
    │       ├── app.config.ts          # App configuration
    │       │
    │       ├── components/            # Reusable components
    │       │   ├── admin-navbar/      # Admin navigation bar
    │       │   └── user-navbar/       # User navigation bar
    │       │
    │       ├── pages/                 # Page components
    │       │   ├── login/             # Login page
    │       │   ├── user-home/         # User dashboard
    │       │   ├── auctions/          # Auctions listing
    │       │   ├── wishlist/          # Wishlist view
    │       │   ├── admin-home/        # Admin dashboard
    │       │   ├── admin-auctions/    # Admin auction management
    │       │   ├── admin-auction-items/   # Admin item management
    │       │   ├── admin-chats/       # Admin chat monitoring
    │       │   └── admin-auction-report/  # Auction reports
    │       │
    │       ├── interceptors/          # HTTP interceptors
    │       │   └── admin-auth.interceptor.ts  # Admin auth handling
    │       │
    │       └── service/               # API services
    │           ├── api.service.ts     # Core API client
    │           └── wishlist.service.ts    # Wishlist management
    │
    └── environments/                  # Environment configs
        ├── environment.ts             # Development environment
        └── environment.prod.ts        # Production environment
```

---

## Prerequisites & Installation

### Requirements
- **Node.js:** v16 or higher
- **npm:** v10.9 or higher
- **MongoDB:** v4.4+ (Local or MongoDB Atlas)
- **Git:** For cloning the repository

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/ShaimaaAliECE/group-project-team-03
   cd group-project-team-03
   ```

2. **Backend Installation**
   ```bash
   cd backend
   npm install
   ```

3. **Frontend Installation**
   ```bash
   cd frontend
   npm install
   ```

---

## Environment Configuration

### Backend Environment Variables

Create a `.env` file in the `backend/` directory with the following variables:

```env
# Server
PORT=8080

# Database
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/auction-platform

# JWT Secret
JWT_SECRET=your_jwt_secret_key_here_min_32_chars

# Email Configuration (Nodemailer)
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-specific-password
EMAIL_SERVICE=gmail

# Default Admin (auto-seeding)
ADMIN_EMAIL=admin@example.com
ADMIN_NAME=Admin

# Default User (auto-seeding, optional)
DEFAULT_USER_EMAIL=user@example.com
DEFAULT_USER_FIRST_NAME=John
DEFAULT_USER_LAST_NAME=Doe

# Allowed Origins (CORS)
ALLOWED_ORIGINS=http://localhost:4200,http://localhost:3000
```

### Frontend Environment Variables

Files are located in `frontend/src/environments/`:

**environment.ts** (Development)
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api'
};
```

**environment.prod.ts** (Production)
```typescript
export const environment = {
  production: true,
  apiUrl: 'https://api.example.com/api'
};
```

### MongoDB Setup

#### Option 1: Local MongoDB
```bash
# Start MongoDB locally
mongod

# Or with Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest
```

#### Option 2: MongoDB Atlas (Cloud)
1. Create account at [mongodb.com](https://www.mongodb.com/cloud/atlas)
2. Create a cluster and database
3. Get connection string and add to `.env` file

---

## Running the Application

### Development Mode

#### Terminal 1 - Backend Server
```bash
cd backend
npm run dev
```
- Starts on `http://localhost:8080`
- Automatically restarts on file changes (nodemon)
- Database seeding runs automatically

#### Terminal 2 - Frontend Development Server
```bash
cd frontend
npm start
```
- Starts on `http://localhost:4200`
- Hot module reloading enabled
- Automatically opens in browser

### Production Build

**Backend:**
```bash
cd backend
npm start
```

**Frontend:**
```bash
cd frontend
npm run build
# Output in dist/frontend
```

### Testing

**Backend:**
```bash
cd backend
npm test
```

**Frontend:**
```bash
cd frontend
npm test
```

---

## API Endpoints Overview

### Authentication (`/api/auth/otp`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/send-otp` | Send OTP to email |
| POST | `/verify-otp` | Verify OTP and create/login user |

### Users (`/api`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/:email` | Get user information |
| PUT | `/users/:email` | Update user profile |
| GET | `/users/:email/bid-history` | Get user bid history |

### Auctions (`/api/auctions`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all auctions |
| GET | `/:auctionId` | Get auction details |
| POST | `/` | Create auction (Admin) |
| PUT | `/:auctionId` | Update auction (Admin) |
| POST | `/:auctionId/close` | Close auction (Admin) |
| GET | `/:auctionId/finalize` | Finalize auction results |

### Items (`/api/items`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | List all items |
| POST | `/` | Create item (Admin) |
| PUT | `/:itemId` | Update item (Admin) |
| DELETE | `/:itemId` | Delete item (Admin) |

### Bidding & Transactions (`/api/auctions/:auctionId/bid`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/` | Place a bid |
| GET | `/` | Get auction bids |

### Wishlist (`/api/wishlist`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/:userEmail` | Get user wishlist |
| POST | `/add` | Add item to wishlist |
| DELETE | `/:itemId` | Remove item from wishlist |

### Notifications (`/api/notifications`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/:userEmail` | Get user notifications |
| POST | `/` | Create notification (Admin) |
| DELETE | `/:notificationId` | Delete notification |

### Kogbucks (`/api/kogbucks`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/:userEmail` | Get user balance |
| POST | `/transfer` | Transfer kogbucks (Admin) |

### Admin (`/api/admin`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/dashboard` | Admin dashboard stats |
| GET | `/users` | List all users |
| GET | `/auctions/reports` | Auction reports |

---

## Database Models

### User
```javascript
{
  email: String (unique, lowercase),
  firstName: String,
  lastName: String,
  dateOfBirth: Date,
  kogbucks: Number (default: 0),
  hold: Boolean (default: false),
  emailNotificationsEnabled: Boolean (default: false),
  // Additional fields for profile and settings
}
```

### Auction
```javascript
{
  name: String,
  description: String,
  startTime: Date,
  endTime: Date,
  status: String (enum: 'scheduled', 'live', 'closed'),
  items: [ObjectId],
  startingPrice: Number,
  minimumBid: Number,
  // Winner determination and finalization fields
}
```

### Items
```javascript
{
  name: String,
  description: String,
  category: String,
  auctionId: ObjectId,
  images: [String],
  estimatedValue: Number,
  // Auction item specific details
}
```

### Transaction (Bids)
```javascript
{
  bidderId: ObjectId,
  auctionId: ObjectId,
  itemId: ObjectId,
  bidAmount: Number,
  timestamp: Date,
  // Bid history and payment tracking
}
```

### Notification
```javascript
{
  userId: ObjectId,
  type: String (enum: 'outbid', 'won', 'update', etc.),
  message: String,
  relatedAuctionId: ObjectId,
  isRead: Boolean,
  timestamp: Date
}
```

### Wishlist
```javascript
{
  userEmail: String,
  items: [ObjectId],
  // Synced with user preferences
}
```

---

## Authentication & Authorization

### JWT Authentication Flow

1. **User Registration/Login**
   - User provides email
   - OTP sent to email via Nodemailer
   - User verifies OTP
   - JWT token generated and returned

2. **Token Structure**
   ```javascript
   {
     userId: ObjectId,
     email: String,
     isAdmin: Boolean,
     iat: timestamp,
     exp: timestamp
   }
   ```

3. **Request Authorization**
   - Token sent in `Authorization: Bearer {token}` header
   - Middleware validates JWT signature and expiration
   - Authenticated user info attached to request

### Middleware Files
- **userAuth.js:** Validates user JWT tokens
- **adminAuth.js:** Validates admin JWT tokens with elevated permissions

### Protected Routes
- All `/api/auctions/*` endpoints (authenticated users)
- All `/api/wishlist/*` endpoints (authenticated users)
- All `/api/admin/*` endpoints (authenticated admins)
- All `/api/notifications/*` endpoints (authenticated users)

---

## Frontend Routes & Components

### User Routes
| Path | Component | Purpose |
|------|-----------|---------|
| `/` | LoginComponent | User login/registration |
| `/user` | Redirects to `/` | User home |
| `/auctions` | AuctionsComponent | Browse live auctions |
| `/wishlist` | WishlistComponent | View saved items |
| `/user-home` | UserHomeComponent | User dashboard |

### Admin Routes
| Path | Component | Purpose |
|------|-----------|---------|
| `/admin-home` | AdminHomeComponent | Admin dashboard |
| `/admin-auctions` | AdminAuctionsComponent | Manage auctions |
| `/admin-auction-items` | AdminAuctionItemsComponent | Manage items |
| `/admin-auction-items/:auctionId` | AdminAuctionItemsDetailComponent | Item details |
| `/admin-chats` | AdminChatsComponent | Monitor communications |
| `/admin-auction-report/:auctionId` | AdminAuctionReportComponent | Auction analytics |

### Components Structure
- **Navbar Components:** Navigation and user menu
- **Page Components:** Full-page views with layout
- **Shared Components:** Reusable UI elements
- **Services:** API communication and state management

---

## Key Features Explained

### 1. Real-time Bidding System
- Users can place bids during live auctions
- Minimum bid requirement: ≥1 bid in first 30 minutes
- Automatic outbid notifications sent
- Transaction history tracked

### 2. Virtual Currency (Kogbucks)
- Users earn/receive kogbucks
- Used for auction participation
- Admin can transfer kogbucks
- Balance checked before bid placement

### 3. Notification System
- **Types:** Outbid, Win, Auction Update
- **Delivery:** Email + In-app notifications
- **Triggers:** Bid events, auction status changes
- **Preferences:** User can enable/disable email notifications

### 4. Wishlist Synchronization
- Add items to wishlist for later
- Synced across user sessions
- Integrated with auction browsing
- Automatic creation on user signup

### 5. Admin Prize Draw
- Automated winner selection
- Prize distribution logic
- Fairness and randomization
- Transaction finalization

### 6. Chat & Messaging
- Auction-specific chat rooms
- Direct user-to-user messaging
- Admin monitoring capabilities
- Message history persistence

---

## Development Workflow

### Code Organization
- Backend: MVC-like pattern with models, routes, middleware
- Frontend: Component-based architecture with services
- Separation of concerns with utility functions

### Adding a New Feature

1. **Create Backend Route** (`backend/routes/feature.js`)
   ```javascript
   const express = require('express');
   const router = express.Router();
   const userAuth = require('../middleware/userAuth');

   router.get('/', userAuth, (req, res) => {
     // Handler logic
   });

   module.exports = router;
   ```

2. **Create Model** if needed (`backend/models/Feature.js`)
   ```javascript
   const mongoose = require('mongoose');
   const featureSchema = new mongoose.Schema({
     // Schema definition
   });
   module.exports = mongoose.model('Feature', featureSchema);
   ```

3. **Register Route** in `backend/server.js`
   ```javascript
   app.use('/api/feature', featureRoute);
   ```

4. **Create Angular Service** (`frontend/src/app/service/feature.service.ts`)
   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpClient } from '@angular/common/http';

   @Injectable({ providedIn: 'root' })
   export class FeatureService {
     constructor(private http: HttpClient) {}
     getFeature() {
       return this.http.get('/api/feature');
     }
   }
   ```

5. **Create Angular Component** (`frontend/src/app/pages/feature/feature.component.ts`)
   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { FeatureService } from '../../service/feature.service';

   @Component({
     selector: 'app-feature',
     templateUrl: './feature.component.html',
     styleUrls: ['./feature.component.css']
   })
   export class FeatureComponent implements OnInit {
     constructor(private service: FeatureService) {}
     ngOnInit() { }
   }
   ```

6. **Add Route** in `frontend/src/app/app.routes.ts`

### Code Style Guidelines
- **Backend:** ES6+ syntax, consistent naming conventions
- **Frontend:** TypeScript strict mode, Angular style guide
- **Database:** Schema validation, indexes for performance
- **Error Handling:** Try-catch with proper HTTP status codes

---

## Docker Deployment

### Building Docker Images

**Backend Dockerfile**
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install --only=production
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]
```

**Frontend Dockerfile**
```dockerfile
FROM node:20-alpine as builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist/frontend /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Running with Docker Compose
```bash
docker-compose up -d
# Backend: http://localhost:8080
# Frontend: http://localhost:80
```

---

## Troubleshooting

### Common Issues

#### Backend Won't Start
```bash
# Check MongoDB connection
# Verify MONGODB_URI in .env
# Check port 8080 is available

# Kill process on port 8080
lsof -ti:8080 | xargs kill -9
```

#### Frontend Build Errors
```bash
# Clear node_modules and reinstall
rm -rf frontend/node_modules
cd frontend && npm install

# Clear Angular build cache
rm -rf frontend/.angular
```

#### JWT Token Errors
```
"Unauthorized" responses
→ Check JWT_SECRET matches backend .env
→ Verify token expiration
→ Ensure Authorization header format: "Bearer {token}"
```

#### MongoDB Connection Issues
```bash
# Verify connection string format
# Check network access (for Atlas)
# Confirm database and user permissions
```

#### CORS Errors
```bash
# Update ALLOWED_ORIGINS in backend .env
# Verify frontend URL matches CORS config
# Check proxy.conf.json in frontend
```

### Debug Mode
```bash
# Backend with debug logs
DEBUG=* npm run dev

# Frontend with detailed output
ng serve --verbose
```

---

## Team Members

| Name | Role |
|------|------|
| **Aaron Siby** | Full Stack Developer |
| **Etan Feidelberg** | Full Stack Developer |
| **Khilan Desai** | Full Stack Developer |
| **Parth Valand** | Full Stack Developer |
| **Shehbaaz Virk** | Full Stack Developer |

---

## License

This project is licensed under the ISC License.

---

## Support & Questions

For issues, questions, or contributions:
1. Check existing GitHub issues
2. Create a new issue with detailed description
3. Submit pull requests for bug fixes or features

---

**Last Updated:** June 2026  
**Version:** 1.0.0
