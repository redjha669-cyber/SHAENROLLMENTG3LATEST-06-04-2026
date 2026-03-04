# 🎓 Sacred Heart Academy of Santa Maria - Enrollment System
## Complete Technical Documentation

---

## 📋 Table of Contents
1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [System Architecture](#system-architecture)
4. [Database Design](#database-design)
5. [Frontend & User Interface](#frontend--user-interface)
6. [Backend API](#backend-api)
7. [Authentication System](#authentication-system)
8. [Installation & Setup](#installation--setup)
9. [How to Run the System](#how-to-run-the-system)
10. [Key Features](#key-features)
11. [File Structure](#file-structure)
12. [Deployment](#deployment)
13. [Troubleshooting](#troubleshooting)

---

## 📱 Project Overview

The **Sacred Heart Academy Student Enrollment System** is a full-stack web application designed to streamline student enrollment management for the institution. It allows authorized teachers to verify their identities, create accounts, and manage student enrollments efficiently.

### **Core Objectives:**
- ✅ Secure admin authentication system
- ✅ Student enrollment form with comprehensive data collection
- ✅ Sequential enrollment ID generation (2026-00001, 2026-00002, etc.)
- ✅ Admin dashboard for managing student records
- ✅ Enrollment records page for viewing, editing, and deleting records
- ✅ CSV export functionality for data analysis
- ✅ Print capabilities for enrollment verification
- ✅ Role-based access control via email whitelist

---

## 🛠️ Technology Stack

### **Frontend Technologies**
| Technology | Version | Purpose |
|---|---|---|
| **HTML5** | Latest | Page structure and markup |
| **CSS3** | Latest | Styling, gradients, responsive design |
| **Vanilla JavaScript** | ES6+ | Client-side logic, API calls, form validation |
| **Fetch API** | Native | HTTP requests to backend |

### **Backend Technologies**
| Technology | Version | Purpose |
|---|---|---|
| **Node.js** | v22.22.0 | Server runtime environment |
| **Express.js** | Latest | Web framework & routing |
| **Mongoose** | Latest | MongoDB ODM (Object Data Modeling) |

### **Database**
| Technology | Service | Purpose |
|---|---|---|
| **MongoDB** | MongoDB Atlas (Cloud) | Primary database |
| **Collections** | Multiple | Enrollment, AdminAccount data storage |

### **Deployment**
| Platform | Service | Purpose |
|---|---|---|
| **Railway** | Cloud Hosting | Production deployment & auto-deployment |
| **GitHub** | Git Repository | Version control & CI/CD integration |

### **Email Service (Optional)**
| Service | Purpose |
|---|---|
| **SendGrid API** | Email verification (configured but simplified) |

---

## 🏗️ System Architecture

### **High-Level Flow Diagram**

```
┌─────────────────┐
│   User/Teacher  │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────┐
│   Frontend (HTML/CSS/JavaScript)    │
│  ├─ Home Page                       │
│  ├─ Admin Login                     │
│  ├─ Admin Create Account            │
│  ├─ Enrollment Form                 │
│  ├─ Admin Dashboard                 │
│  ├─ Enrollment Records              │
│  └─ Verification Pages              │
└────────┬────────────────────────────┘
         │ (API Calls via Fetch)
         ▼
┌─────────────────────────────────────┐
│    Backend (Node.js/Express)        │
│  ├─ Authentication Routes           │
│  ├─ Enrollment Routes               │
│  ├─ Email Verification              │
│  ├─ Data Processing                 │
│  └─ API Endpoints                   │
└────────┬────────────────────────────┘
         │ (Mongoose Queries)
         ▼
┌─────────────────────────────────────┐
│   MongoDB Atlas (Cloud Database)    │
│  ├─ Enrollments Collection          │
│  ├─ AdminAccount Collection         │
│  └─ VerificationCode Collection     │
└─────────────────────────────────────┘
```

---

## 💾 Database Design

### **MongoDB Collections**

#### **1. Enrollments Collection**
Stores all student enrollment information with sequential ID generation.

```javascript
{
  _id: ObjectId,                          // MongoDB unique identifier
  enrollmentID: String,                   // Format: "2026-00001" (unique)
  enrollmentDate: Date,                   // When student enrolled
  studentInfo: {
    firstName: String,
    middleName: String,
    lastName: String,
    suffix: String,
    lrn: String,                         // Learner Reference Number (unique)
    email: String,
    contactNumber: String,
    age: Number,
    sex: String,                         // "Male" or "Female"
    dateOfBirth: Date,
    gradeLevel: Number,                  // 11 or 12
    strand: String,                      // "stem", "humss", "tvl-ict", etc.
    gwa: Number,                         // General Weighted Average
    paymentCategory: String,
    alsPasser: String,                   // "yes" or "no"
    schoolLastAttended: String,
    houseAddress: String
  },
  parentsInfo: {
    fatherName: String,
    fatherContact: String,
    fatherAlumni: String,
    motherName: String,
    motherContact: String,
    motherAlumni: String
  },
  addressInfo: {
    province: String,
    city: String,
    barangay: String
  },
  emergencyContact: {
    name: String,
    relationship: String,
    contactNumber: String
  }
}
```

#### **2. AdminAccount Collection**
Stores teacher/admin credentials and account information.

```javascript
{
  _id: ObjectId,
  email: String,                         // Unique email
  username: String,                      // Unique username
  password: String,                      // Hashed password
  verified: Boolean,                     // Account verification status
  createdDate: Date                      // Account creation date
}
```

#### **3. VerificationCode Collection** (Optional)
Stores temporary verification codes for authentication.

```javascript
{
  email: String,
  code: String,
  expiresAt: Date,
  attempts: Number
}
```

### **Key Indexes:**
- `enrollmentID`: Unique, sparse (prevents duplicates)
- `lrn`: Unique, sparse (ensures student uniqueness)
- `email` (AdminAccount): Unique (merchant login)
- `id_1`: Dropped on startup (legacy field cleanup)

---

## 🎨 Frontend & User Interface

### **HTML Pages**

| Page | Purpose | Key Features |
|---|---|---|
| **Home.html** | Landing page | Navigation, enrollment link, login option |
| **admin-login.html** | Teacher login | Email/password authentication, session storage |
| **admin-create.html** | Account creation | Email verification (whitelist), password validation |
| **verification.html** | Student email check | Email input, whitelist verification |
| **final-update.html** | Complete enrollment | Comprehensive form with all student data fields |
| **admin-dashboard.html** | Admin control center | Student list, view/edit/delete, CSV export, search |
| **enrollment-records.html** | Record management | Table view, filters, print, CSV export, delete |
| **enrollment-records.html** | Special features | Modal views, data export, inline editing |

### **UI/UX Design Highlights**
- **Color Scheme**: Teal gradient (#00A693) with complementary neutrals
- **Responsive Design**: Works on desktop, tablet, mobile
- **User Feedback**: Alerts, modals, loading states
- **Accessibility**: Clear labels, intuitive navigation, form validation
- **Security Features**: Show/hide password toggles, confirmation dialogs

### **Key UI Components**
1. **Enrollment Form**: Multi-step data collection
2. **Student Table**: Sortable, filterable, paginated display
3. **Modal Dialogs**: View details, edit records, confirm deletions
4. **Action Buttons**: Consistent styling and positioning
5. **Export Tools**: CSV download, print functionality

---

## 🔌 Backend API

### **Base URL**: `https://pr-g3-sha-sces-enrollment-production.up.railway.app`

### **Main Endpoints**

#### **Authentication Routes**

**1. Check Email (Whitelist Verification)**
```
POST /api/admin/check-email
Content-Type: application/json
Body: { "email": "teacher@example.com" }
Response: { "success": true, "message": "Email verified as authorized" }
```

**2. Create Admin Account**
```
POST /api/admin/create-account
Content-Type: application/json
Body: {
  "email": "teacher@example.com",
  "username": "teachername",
  "password": "securepassword"
}
Response: { "success": true, "message": "Account created successfully" }
```

**3. Admin Login**
```
POST /api/admin/login
Content-Type: application/json
Body: { "emailOrUsername": "teacher@example.com", "password": "password" }
Response: { 
  "success": true, 
  "user": { "email": "teacher@example.com", "username": "teachername" }
}
```

**4. Verify Session**
```
POST /api/admin/verify-session
Content-Type: application/json
Response: { "valid": true, "user": { /* user data */ } }
```

#### **Enrollment Routes**

**1. Save/Create Enrollment**
```
POST /api/enroll
Content-Type: application/json
Body: {
  "studentInfo": { /* all student fields */ },
  "parentsInfo": { /* parent fields */ },
  "addressInfo": { /* address fields */ },
  "emergencyContact": { /* emergency contact */ }
}
Response: { 
  "success": true, 
  "message": "Enrollment saved successfully",
  "enrollmentId": "2026-00001"
}
```

**2. Get All Enrollments**
```
GET /api/enrollments
Response: [ 
  { 
    "_id": "...",
    "enrollmentID": "2026-00001",
    "studentInfo": { /* all data */ },
    ...
  }, 
  ...
]
```

**3. Get Single Enrollment**
```
GET /api/enrollments/:id
Response: { /* single enrollment object */ }
```

**4. Update Enrollment**
```
PUT /api/enrollments/:id
Content-Type: application/json
Body: { /* updated fields */ }
Response: { "success": true, "message": "Enrollment updated" }
```

**5. Delete Enrollment**
```
DELETE /api/enrollments/:id
Response: { "success": true, "message": "Enrollment deleted successfully" }
```

---

## 🔐 Authentication System

### **Login Flow**

```
1. Teacher visits home page
   ↓
2. Clicks "Admin Login" or "Create Account"
   ↓
3. Enter email address
   ↓
4. System checks: admin-authorized.json whitelist
   ├─ ✅ Email found → Proceed to create account or login
   └─ ❌ Email not found → Show error message
   ↓
5. If creating account:
   ├─ Enter username and password
   ├─ Password validation (must meet criteria)
   └─ Account saved to MongoDB AdminAccount collection
   ↓
6. If logging in:
   ├─ Enter email/username and password
   ├─ Credentials verified against MongoDB
   └─ Session created (stored in localStorage)
   ↓
7. Redirected to Admin Dashboard
   ↓
8. Session verified on each page load
   └─ If invalid, redirect to login
```

### **Authorized Teachers**
The system loads authorized emails from **admin-authorized.json**:

```json
{
  "authorizedEmails": [
    "surugi64@gmail.com",
    "teacher1@gmail.com",
    "teacher2@gmail.com",
    "principal@gmail.com",
    "testadmin@gmail.com",
    "bryceedrithm@gmail.com",
    "giancarlsarmiento4@gmail.com",
    "aicellecarpio09@gmail.com",
    "mishacabrera23176@gmail.com",
    "gmarguiller@gmail.com",
    "ogkatasngbayag@gmail.com",
    "jakejakeparado@gmail.com"
  ]
}
```

### **Security Features**
- ✅ Email whitelist verification
- ✅ Case-insensitive login
- ✅ Password validation
- ✅ Session management via localStorage
- ✅ No sensitive data in client storage
- ✅ Server-side authentication checks

---

## 💿 Installation & Setup

### **Prerequisites**
- Node.js v22.22.0 or higher
- npm (Node Package Manager)
- Git
- MongoDB Atlas account (for cloud database)
- Railway account (for deployment)

### **Step 1: Clone Repository**
```bash
git clone https://github.com/tsurugi64/PR-G3--SHA-SCES--ENROLLMENT.git
cd PR-G3--SHA-SCES--ENROLLMENT
```

### **Step 2: Install Dependencies**
```bash
npm install
```

This installs:
- express (web framework)
- mongoose (MongoDB driver)
- cors (cross-origin requests)
- dotenv (environment variables)
- @sendgrid/mail (email service)

### **Step 3: Configure Environment Variables**
Create a `.env` file in the root directory:

```env
# MongoDB Connection
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/dbname

# SendGrid API (optional)
SENDGRID_API_KEY=your_sendgrid_api_key

# Server Port
PORT=3000
```

### **Step 4: Verify Files**
Ensure these key files exist:
- `server-mongodb.js` - Main server file
- `models/Enrollment.js` - Database schema
- `admin-authorized.json` - Whitelist of authorized emails
- `package.json` - Dependencies list
- All HTML files in root directory

---

## 🚀 How to Run the System

### **Local Development**

#### **Option 1: Using Node.js directly**
```bash
node server-mongodb.js
```

Output should show:
```
✅ Loaded 12 authorized teacher emails
╔════════════════════════════════════════╗
║  SHA Enrollment System Server Running  ║
║  Port: 8080                            ║
║  MongoDB: Connected ✅                ║
╚════════════════════════════════════════╝
```

#### **Option 2: Using npm start (if configured)**
```bash
npm start
```

### **Access the Application**
- **Local**: `http://localhost:3000` (or 8080 depending on PORT setting)
- **Production**: `https://pr-g3-sha-sces-enrollment-production.up.railway.app`

### **Testing the System**

#### **1. Teacher Registration**
1. Go to home page
2. Click "Create Admin Account"
3. Enter authorized email (from whitelist)
4. Create username and password
5. Account created successfully

#### **2. Student Enrollment**
1. Click "Enroll A Student" or go to verification page
2. Enter authorized teacher email
3. Fill out complete enrollment form
4. Student gets ID: 2026-00001, 2026-00002, etc.

#### **3. View Dashboard**
1. Login with teacher credentials
2. See all enrolled students
3. Manage records (view, edit, delete)

---

## ⭐ Key Features

### **1. Sequential ID Generation** 🆔
- Auto-generates enrollment IDs: 2026-00001, 2026-00002, etc.
- Format: YYYY-#####
- Never duplicates or skips
- Next ID always based on highest existing ID + 1

### **2. Email Whitelist Authentication** 📧
- Secure access control
- Only authorized emails can create accounts
- Configurable list in admin-authorized.json
- No SendGrid required for basic functionality

### **3. Comprehensive Data Collection** 📋
- Student information (name, LRN, contact, etc.)
- Parent/guardian details
- Address information
- Academic information
- Emergency contact details
- Payment category tracking

### **4. Admin Dashboard** 👨‍💼
- View all enrolled students in table format
- Search and filter functionality
- View complete student profiles
- Edit student information
- Delete student records
- CSV export for data analysis
- Real-time statistics

### **5. Enrollment Records Management** 📊
- Separate dedicated page for records
- Advanced filtering options
- Inline editing
- Print functionality for individual records
- Bulk CSV export
- Delete functionality with confirmation

### **6. Data Export Options** 📤
- CSV format for Excel/Sheets
- Print-friendly layouts
- Complete data preservation

### **7. Session Management** 🔒
- Persistent login sessions
- Automatic logout on browser close
- Session validation on each page

---

## 📁 File Structure

```
PR-G3--SHA-SCES--ENROLLMENT/
│
├── 📄 server-mongodb.js              # Main backend server
├── 📄 package.json                   # Dependencies & scripts
├── 📄 .env                           # Environment variables (NOT in repo)
├── 📄 admin-authorized.json          # Authorized teacher emails
│
├── 📁 models/
│   └── 📄 Enrollment.js              # MongoDB Enrollment schema
│
├── 📁 Frontend Pages/
│   ├── 📄 Home.html                  # Landing page
│   ├── 📄 admin-login.html           # Login page
│   ├── 📄 admin-create.html          # Account creation
│   ├── 📄 admin-dashboard.html       # Admin control center
│   ├── 📄 verification.html          # Email verification
│   ├── 📄 final-update.html          # Student enrollment form
│   ├── 📄 enrollment-records.html    # Records management
│   └── 📄 home-for-student.html      # Student home page
│
├── 📄 style.css                      # Global styling
│
├── 📁 Documentation/
│   ├── 📄 SYSTEM_DOCUMENTATION.md    # This file
│   ├── 📄 README.md                  # Quick start guide
│   └── 📄 QUICK_REFERENCE.md         # API quick reference
│
└── 📁 .git/                          # Git repository
```

---

## 🌐 Deployment

### **Current Deployment: Railway**

#### **Production URL**
```
https://pr-g3-sha-sces-enrollment-production.up.railway.app
```

#### **Deployment Process**
1. Code pushed to GitHub main branch
2. Railway automatically triggers build
3. Dependencies installed via npm
4. Server starts with `npm start`
5. Application live within minutes

#### **Deployment Commands**
```bash
# Commit changes
git add -A
git commit -m "Description of changes"

# Push to GitHub (auto-deploys to Railway)
git push origin main
```

#### **Environment Variables on Railway**
Set these in Railway dashboard:
- `MONGODB_URI` - MongoDB Atlas connection string
- `SENDGRID_API_KEY` - SendGrid API key (optional)
- `PORT` - Server port (default 8080)

#### **Monitoring Deployment**
1. Go to Railway dashboard
2. Select the PR-G3 project
3. View build logs and deployment status
4. Check HTTP logs for errors

---

## 🔧 Troubleshooting

### **Common Issues & Solutions**

#### **1. MongoDB Connection Error**
**Problem**: `Error: Could not connect to MongoDB`

**Solution**:
- Verify MONGODB_URI in .env file
- Check MongoDB Atlas whitelist includes your IP
- Ensure connection string format is correct
- Test connection separately

```bash
# Test MongoDB connection
mongosh "mongodb+srv://username:password@cluster.mongodb.net/dbname"
```

#### **2. Port Already in Use**
**Problem**: `Error: listen EADDRINUSE: address already in use :::3000`

**Solution**:
```bash
# Find process using port 3000
netstat -ano | findstr :3000

# Kill the process (replace PID with actual number)
taskkill /PID 12345 /F

# Then restart server
node server-mongodb.js
```

#### **3. Duplicate Key Error on Enrollment**
**Problem**: `E11000 duplicate key error on field: id`

**Solution**:
- Server automatically drops problematic indexes on startup
- System clears legacy `id` field from documents
- Next enrollment attempt should succeed
- If persists, check MongoDB collection for orphaned indexes

#### **4. 404 Not Found Errors**
**Problem**: `Cannot POST /api/enroll` or missing pages

**Solution**:
- Verify server is running on correct port
- Check MONGODB_URI is set correctly
- Ensure all HTML files are in root directory
- Review server console for error messages

#### **5. Enrollment ID Not Generating**
**Problem**: NULL or undefined enrollmentID

**Solution**:
- Verify `generateEnrollmentID()` function exists
- Check MongoDB enrollments collection has records
- Ensure `enrollmentID` field is unique in schema
- Test with simple enrollment first

#### **6. Delete Button Not Working**
**Problem**: Clicking delete doesn't remove record

**Solution**:
- Verify using MongoDB `_id` not legacy `id`
- Check browser console for errors
- Ensure server delete endpoint is working
- Test: `DELETE /api/enrollments/{_id}`

#### **7. Session Lost After Refresh**
**Problem**: Need to login again after page refresh

**Solution**:
- localStorage should persist sessions
- Check browser localStorage settings
- Verify session verification endpoint `/api/admin/verify-session`
- Clear browser cache and try again

---

## 📊 Performance & Database Optimization

### **Indexing Strategy**
```javascript
// Efficient queries with indexes
db.enrollments.find({ enrollmentID: "2026-00001" })  // Fast - indexed
db.enrollments.find({ "studentInfo.lrn": "123456" }) // Fast - indexed
db.adminaccounts.find({ email: "teacher@example.com" }) // Fast - indexed
```

### **Query Performance Tips**
- Always query by enrollmentID or _id for single records
- Use filters efficiently on dashboard
- Batch export operations for large datasets
- Keep MongoDB Atlas cluster properly sized

---

## 🎓 For Presentation

### **Key Talking Points**
1. **Automated ID Generation**: System creates unique, sequential IDs automatically (2026-00001, 2026-00002, etc.)
2. **Security**: Email whitelist ensures only authorized teachers can access
3. **Complete Data Collection**: Captures all necessary student information in one form
4. **Real-time Updates**: Changes reflect immediately across all pages
5. **Export Capabilities**: Data can be exported to CSV for analysis
6. **Cloud-Based**: No local server needed, accessible from anywhere
7. **Scalable**: Can handle multiple concurrent users
8. **User-Friendly**: Intuitive interface requires minimal training

### **Live Demo Script**
1. Show home page navigation
2. Create new admin account (use authorized email)
3. Login to admin dashboard
4. Show existing student records
5. Enroll a new student through form
6. View new student appearing in dashboard with auto-generated ID
7. Show CSV export functionality
8. Show deletion with confirmation
9. Highlight responsive design on different screen sizes

---

## 📞 Support & References

### **Technologies Used**
- [Node.js Documentation](https://nodejs.org/docs/)
- [Express.js Guide](https://expressjs.com/)
- [MongoDB/Mongoose Manual](https://www.mongodb.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/)

### **Repository**
- GitHub: https://github.com/tsurugi64/PR-G3--SHA-SCES--ENROLLMENT
- Production: https://pr-g3-sha-sces-enrollment-production.up.railway.app

### **Database**
- MongoDB Atlas: https://www.mongodb.com/cloud/atlas
- Connection Status: Check Railway dashboard

---

## 📝 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Mar 5, 2026 | Initial release with sequential ID generation, authentication, and full CRUD |
| 1.1 | Mar 5, 2026 | Fixed cascading delete issue, optimized performance |
| 1.2 | Mar 5, 2026 | Removed legacy id field for cleaner database |

---

## ✨ Future Enhancements

Potential improvements for future iterations:
- [ ] Role-based access control (Admin, Teacher, Secretary roles)
- [ ] Bulk student import from CSV
- [ ] Email notifications for enrollment confirmation
- [ ] Payment tracking integration
- [ ] Attendance management system
- [ ] Grade tracking
- [ ] Parent portal for viewing student progress
- [ ] Advanced reporting and analytics
- [ ] Multi-school support
- [ ] Mobile application

---

**Last Updated**: March 5, 2026  
**Status**: ✅ Production Ready  
**Documentation Version**: 1.0

