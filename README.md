# 📋 Task Management Backend API

A robust, scalable Task Management Backend built with Node.js, Express, MongoDB Atlas, and JWT Authentication. This API provides complete task management capabilities with user authentication, CRUD operations, and advanced filtering.

## ✨ Features

- 🔐 **Secure Authentication** - JWT-based user authentication with 7-day token expiry
- 📝 **Complete CRUD Operations** - Full task management with create, read, update, delete
- 🏷️ **Smart Categorization** - Organize tasks by categories (work, personal, shopping, health, education, other)
- ⏰ **Deadline Management** - Set and track task deadlines with validation
- 🎯 **Priority System** - Three-level priority system (low, medium, high)
- ✅ **Status Tracking** - Track task progress (pending, in-progress, completed)
- 📊 **Analytics** - Get task statistics and insights
- 🔒 **User Isolation** - Each user only sees their own tasks
- 🛡️ **Security** - Password hashing, input validation, and secure headers
- 🌐 **CORS Enabled** - Ready for frontend integration

## 🛠️ Tech Stack

- **Backend**: Node.js, Express.js
- **Database**: MongoDB Atlas (Cloud)
- **Authentication**: JWT (JSON Web Tokens)
- **Password Security**: bcryptjs
- **Validation**: express-validator
- **Environment**: dotenv
- **Development**: nodemon

## 📁 Project Structure

```
task-manager-backend/
├── config/
│   └── db.js                 # Database connection configuration
├── controllers/
│   ├── authController.js     # Authentication logic
│   └── taskController.js     # Task management logic
├── middleware/
│   └── authMiddleware.js     # JWT authentication middleware
├── models/
│   ├── User.js              # User schema and methods
│   └── Task.js              # Task schema and methods
├── routes/
│   ├── authRoutes.js        # Authentication routes
│   └── taskRoutes.js        # Task management routes
├── .env                     # Environment variables (not in repo)
├── .env.example             # Environment variables template
├── .gitignore               # Git ignore rules
├── server.js                # Main server file
├── package.json             # Dependencies and scripts
├── README.md               # This file
└── Task-Manager-API.postman_collection.json # API testing collection
```

## 🚀 Quick Start

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- MongoDB Atlas account (free tier available)

### Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd task-manager-backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Setup**
   ```bash
   # Copy the example environment file
   cp .env.example .env
   
   # Edit .env with your configuration
   nano .env
   ```

4. **Configure Environment Variables**
   
   Update your `.env` file with the following:
   ```env
   # Database
   MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/task-manager?retryWrites=true&w=majority
   
   # JWT Configuration
   JWT_SECRET=your-super-secret-jwt-key-here
   JWT_EXPIRE=7d
   
   # Server Configuration
   PORT=5000
   NODE_ENV=development
   ```

5. **Start the server**
   ```bash
   # Development mode (with auto-restart)
   npm run dev
   
   # Production mode
   npm start
   ```

The server will start on `http://localhost:5000`

## 📚 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Authentication

All task-related endpoints require authentication. Include the JWT token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

### Endpoints

#### 🔐 Authentication Routes

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| POST | `/auth/register` | Register a new user | Public |
| POST | `/auth/login` | Login user and get token | Public |
| GET | `/auth/me` | Get current user profile | Private |

#### 📋 Task Management Routes

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/tasks` | Get all user tasks (with filtering) | Private |
| GET | `/tasks/:id` | Get single task by ID | Private |
| POST | `/tasks` | Create a new task | Private |
| PUT | `/tasks/:id` | Update existing task | Private |
| DELETE | `/tasks/:id` | Delete task | Private |
| GET | `/tasks/stats` | Get task statistics | Private |

#### 🏥 Health Check

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/health` | API health status | Public |

### Request/Response Examples

#### User Registration
```bash
POST /api/auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "_id": "64f1a2b3c4d5e6f7g8h9i0j1",
    "name": "John Doe",
    "email": "john@example.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### Create Task
```bash
POST /api/tasks
Authorization: Bearer <jwt-token>
Content-Type: application/json

{
  "title": "Complete project documentation",
  "description": "Write comprehensive API documentation",
  "category": "work",
  "priority": "high",
  "deadline": "2024-01-15T10:00:00.000Z"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Task created successfully",
  "data": {
    "_id": "64f1a2b3c4d5e6f7g8h9i0j2",
    "title": "Complete project documentation",
    "description": "Write comprehensive API documentation",
    "category": "work",
    "priority": "high",
    "status": "pending",
    "deadline": "2024-01-15T10:00:00.000Z",
    "user": "64f1a2b3c4d5e6f7g8h9i0j1",
    "createdAt": "2024-01-01T12:00:00.000Z",
    "updatedAt": "2024-01-01T12:00:00.000Z"
  }
}
```

#### Get Tasks with Filtering
```bash
GET /api/tasks?category=work&status=pending&sortBy=deadline&sortOrder=asc
Authorization: Bearer <jwt-token>
```

### Data Models

#### User Model
```javascript
{
  _id: ObjectId,
  name: String (required, 2-50 chars),
  email: String (required, unique, valid email),
  password: String (required, min 6 chars, hashed),
  createdAt: Date,
  updatedAt: Date
}
```

#### Task Model
```javascript
{
  _id: ObjectId,
  title: String (required, max 100 chars),
  description: String (optional, max 500 chars),
  category: String (work|personal|shopping|health|education|other),
  priority: String (low|medium|high),
  status: String (pending|in-progress|completed),
  deadline: Date (optional, must be future),
  completedAt: Date (auto-set when completed),
  user: ObjectId (reference to User),
  createdAt: Date,
  updatedAt: Date
}
```

## 🧪 Testing

### Using Postman

1. Import the provided Postman collection: `Task-Manager-API.postman_collection.json`
2. Set the `base_url` variable to your server URL
3. Run the collection to test all endpoints

### Manual Testing

```bash
# Health check
curl http://localhost:5000/api/health

# Register user
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Create task (replace <token> with actual JWT token)
curl -X POST http://localhost:5000/api/tasks \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Task","category":"work","priority":"high"}'
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `MONGODB_URI` | MongoDB connection string | - | ✅ |
| `JWT_SECRET` | Secret key for JWT signing | - | ✅ |
| `JWT_EXPIRE` | JWT token expiry time | 7d | ❌ |
| `PORT` | Server port | 5000 | ❌ |
| `NODE_ENV` | Environment mode | development | ❌ |

### MongoDB Atlas Setup

1. Create a [MongoDB Atlas](https://www.mongodb.com/atlas) account
2. Create a new cluster (free tier available)
3. Create a database user
4. Whitelist your IP address (or use 0.0.0.0/0 for development)
5. Get your connection string and update `MONGODB_URI`

## 🚀 Deployment

### Production Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Use a strong, unique `JWT_SECRET`
- [ ] Configure proper MongoDB Atlas security
- [ ] Set up environment variables on your hosting platform
- [ ] Enable HTTPS
- [ ] Set up monitoring and logging

### Popular Deployment Platforms

- **Heroku**: Easy deployment with MongoDB Atlas integration
- **Railway**: Modern platform with automatic deployments
- **DigitalOcean**: VPS with full control
- **AWS**: EC2 with RDS or DocumentDB
- **Vercel**: Serverless deployment option

## 🔒 Security Features

- ✅ **Password Hashing**: bcryptjs with salt rounds
- ✅ **JWT Authentication**: Secure token-based auth
- ✅ **Input Validation**: express-validator for all inputs
- ✅ **CORS Protection**: Configurable cross-origin requests
- ✅ **Environment Variables**: Sensitive data protection
- ✅ **User Isolation**: Data separation between users
- ✅ **Error Handling**: Secure error responses

## 📊 API Response Format

### Success Response
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error description",
  "errors": [
    {
      "field": "email",
      "message": "Please provide a valid email"
    }
  ]
}
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

If you encounter any issues or have questions:

1. Check the [Issues](https://github.com/your-username/task-manager-backend/issues) page
2. Create a new issue with detailed information
3. Include error logs and steps to reproduce

