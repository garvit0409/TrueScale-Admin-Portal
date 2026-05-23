# Device Request System

## Overview
Complete device request flow implementation with user registration, admin approval, and certificate generation.

## Features
- Device request submission with validation
- Reference number generation (DEV-{YYYY}{6-digit-rand})
- Request tracking by reference number
- Admin dashboard for request management
- User account creation upon approval
- PDF certificate generation
- Secure password hashing with bcrypt

## Setup Instructions

### 1. Install Dependencies
```bash
cd smart-weight-backend
npm install express mongoose bcrypt pdfkit dotenv
npm install --save-dev nodemon jest supertest
```

### 2. Environment Variables
Create `.env` file in smart-weight-backend:
```
MONGODB_URI=your_mongodb_connection_string
DB_NAME=smart_weight_detection
PORT=3001
```

### 3. Start Server
```bash
npm run dev
```

### 4. Run Tests
```bash
npm test
```

## API Endpoints

### Device Requests
- `POST /api/device-requests` - Submit new device request
- `GET /api/device-requests/track/:referenceNo` - Track request status
- `GET /api/device-requests` - Get all requests (admin)
- `POST /api/device-requests/:id/accept` - Accept request (admin)
- `POST /api/device-requests/:id/reject` - Reject request (admin)
- `GET /certificates/:filename` - Download certificate

## Frontend Features

### Login Page Additions
- "Request New Device" button opens registration form
- "Track Request" field for status checking
- Form validation and error handling

### Admin Dashboard
- New "Device Requests" section in navigation
- Request listing with search/filter
- Accept/Reject actions
- Certificate download links

## Database Schema

### DeviceRequest
```javascript
{
  referenceNo: String (unique),
  name: String,
  dateOfBirth: Date,
  gender: String,
  governmentId: String,
  phone: String,
  shopAddress: String,
  status: String (pending/accepted/rejected),
  createdAt: Date,
  processedAt: Date,
  userId: String,
  certificateUrl: String
}
```

### User
```javascript
{
  userId: String (unique),
  name: String,
  phone: String,
  password: String (hashed),
  requestId: ObjectId,
  createdAt: Date
}
```

## Security Features
- Password hashing with bcrypt
- Input validation and sanitization
- Unique reference number generation
- Secure file handling for certificates

## Usage Flow

1. **Request Submission**: User fills form on login page
2. **Reference Generation**: System creates unique DEV-{YYYY}{6-digit} reference
3. **Admin Review**: Admin sees request in dashboard
4. **Approval Process**: Admin accepts/rejects with one click
5. **Account Creation**: System generates userId and password
6. **Certificate Generation**: PDF certificate with credentials
7. **Tracking**: User can track status with reference number

## File Structure
```
smart-weight-backend/
├── models/
│   ├── DeviceRequest.js
│   └── User.js
├── tests/
│   └── deviceRequest.test.js
├── certificates/
│   └── (generated PDFs)
└── server.js (updated with routes)

frontend/
├── admin.html (updated with modals)
├── dashboard.html (updated with requests section)
└── assets/js/
    ├── admin.js (updated with request functions)
    └── dashboard.js (updated with admin functions)
```