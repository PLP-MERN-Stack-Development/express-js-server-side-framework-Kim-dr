# 🚂 Express.js Product API

A fully functional RESTful API built with Express.js that manages a product catalog. This API demonstrates proper routing, middleware implementation, error handling, and advanced features like filtering, pagination, and search.

## 📋 Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Setup](#environment-setup)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Middleware](#middleware)
- [Error Handling](#error-handling)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Examples](#examples)
- [Troubleshooting](#troubleshooting)

## ✨ Features

- **RESTful API Design**: Standard CRUD operations for product management
- **Custom Middleware**: Logging, authentication, and error handling
- **Advanced Filtering**: Filter products by category
- **Search Functionality**: Search products by name
- **Pagination Support**: Navigate through large product lists
- **Error Handling**: Comprehensive error responses with appropriate HTTP status codes
- **In-Memory Database**: Simple data storage for development/testing
- **UUID Generation**: Unique identifiers for all products

## 📦 Prerequisites

Before running this project, make sure you have the following installed:

- **Node.js** (v18.0.0 or higher) - [Download here](https://nodejs.org/)
- **npm** (comes with Node.js) or **yarn**
- **Postman** or **Insomnia** (optional, for API testing)
- **Git** (for cloning the repository)

### Check your installations:
```bash
node --version  # Should show v18.0.0 or higher
npm --version   # Should show 8.0.0 or higher
```

## 🚀 Installation

### 1. Clone the repository
```bash
git clone https://github.com/PLP-MERN-Stack-Development/express-js-server-side-framework-Kim-dr.git
cd express-js-server-side-framework-Kim-dr
```

### 2. Install dependencies
```bash
npm install
```

This will install:
- `express` - Web framework
- `body-parser` - Parse JSON request bodies
- `uuid` - Generate unique identifiers
- `dotenv` - Load environment variables
- `nodemon` (dev dependency) - Auto-restart server during development

## 🔧 Environment Setup

### 1. Create environment file
Create a `.env` file in the root directory:

```bash
# Copy the example file
cp .env.example .env
```

### 2. Configure environment variables
Edit the `.env` file with your settings:

```env
# Server Configuration
PORT=3000
NODE_ENV=development

# API Security
API_KEY=your-secret-api-key-here-change-this-in-production

# Optional: Database connection (for future MongoDB integration)
# MONGODB_URI=mongodb://localhost:27017/products_db
```

**⚠️ Important Security Notes:**
- Never commit your `.env` file to version control
- Change the default `API_KEY` to a secure random string
- For production, use environment variables from your hosting platform

### 3. Example `.env.example` file
```env
# Server Configuration
PORT=3000
NODE_ENV=development

# API Security
API_KEY=your-secret-api-key-here

# Database (Optional - for future use)
# MONGODB_URI=your-mongodb-connection-string
```

## 🏃 Running the Application

### Development Mode (with auto-restart)
```bash
npm run dev
```
This uses `nodemon` to automatically restart the server when you make changes.

### Production Mode
```bash
npm start
```

### Expected Output
```
Server is running on http://localhost:3000
```

### Verify the server is running
Open your browser and visit: `http://localhost:3000`

You should see: **"Welcome to the Product API! Go to /api/products to see all products."**

## 📚 API Documentation

### Base URL
```
http://localhost:3000
```

### Authentication
All API endpoints require an API key in the request headers:
```
x-api-key: your-secret-api-key-here
```

---

## 📡 API Endpoints

### 1. Get All Products
**Endpoint:** `GET /api/products`

**Description:** Retrieve all products with optional filtering, search, and pagination.

**Headers:**
```
x-api-key: your-secret-api-key-here
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `category` | string | No | Filter by category (e.g., electronics, kitchen) |
| `search` | string | No | Search by product name (partial match) |
| `page` | number | No | Page number for pagination (default: 1) |
| `limit` | number | No | Number of items per page (default: 10) |

**Example Requests:**
```bash
# Get all products
curl -X GET http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here"

# Filter by category
curl -X GET "http://localhost:3000/api/products?category=electronics" \
  -H "x-api-key: your-secret-api-key-here"

# Search by name
curl -X GET "http://localhost:3000/api/products?search=laptop" \
  -H "x-api-key: your-secret-api-key-here"

# Pagination
curl -X GET "http://localhost:3000/api/products?page=1&limit=5" \
  -H "x-api-key: your-secret-api-key-here"
```

**Success Response (200):**
```json
[
  {
    "id": "1",
    "name": "Laptop",
    "description": "High-performance laptop with 16GB RAM",
    "price": 1200,
    "category": "electronics",
    "inStock": true
  },
  {
    "id": "2",
    "name": "Smartphone",
    "description": "Latest model with 128GB storage",
    "price": 800,
    "category": "electronics",
    "inStock": true
  }
]
```

---

### 2. Get Product by ID
**Endpoint:** `GET /api/products/:id`

**Description:** Retrieve a specific product by its unique ID.

**Headers:**
```
x-api-key: your-secret-api-key-here
```

**URL Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | Unique product identifier |

**Example Request:**
```bash
curl -X GET http://localhost:3000/api/products/1 \
  -H "x-api-key: your-secret-api-key-here"
```

**Success Response (200):**
```json
{
  "id": "1",
  "name": "Laptop",
  "description": "High-performance laptop with 16GB RAM",
  "price": 1200,
  "category": "electronics",
  "inStock": true
}
```

**Error Response (404):**
```json
{
  "error": "Product not found"
}
```

---

### 3. Create New Product
**Endpoint:** `POST /api/products`

**Description:** Add a new product to the catalog.

**Headers:**
```
x-api-key: your-secret-api-key-here
Content-Type: application/json
```

**Request Body:**
```json
{
  "name": "Gaming Mouse",
  "description": "RGB gaming mouse with 12 programmable buttons",
  "price": 79.99,
  "category": "electronics",
  "inStock": true
}
```

**Field Requirements:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Product name |
| `description` | string | Yes | Product description |
| `price` | number | Yes | Product price (must be a number) |
| `category` | string | Yes | Product category |
| `inStock` | boolean | Yes | Stock availability (true/false) |

**Example Request:**
```bash
curl -X POST http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Gaming Mouse",
    "description": "RGB gaming mouse with 12 programmable buttons",
    "price": 79.99,
    "category": "electronics",
    "inStock": true
  }'
```

**Success Response (201):**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Gaming Mouse",
  "description": "RGB gaming mouse with 12 programmable buttons",
  "price": 79.99,
  "category": "electronics",
  "inStock": true
}
```

**Error Response (400):**
```json
{
  "error": "Invalid product data"
}
```

---

### 4. Update Product
**Endpoint:** `PUT /api/products/:id`

**Description:** Update an existing product's information.

**Headers:**
```
x-api-key: your-secret-api-key-here
Content-Type: application/json
```

**URL Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | Unique product identifier |

**Request Body:**
```json
{
  "name": "Laptop Pro",
  "description": "Updated high-performance laptop with 32GB RAM",
  "price": 1500,
  "category": "electronics",
  "inStock": true
}
```

**Example Request:**
```bash
curl -X PUT http://localhost:3000/api/products/1 \
  -H "x-api-key: your-secret-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Laptop Pro",
    "description": "Updated high-performance laptop with 32GB RAM",
    "price": 1500,
    "category": "electronics",
    "inStock": true
  }'
```

**Success Response (200):**
```json
{
  "id": "1",
  "name": "Laptop Pro",
  "description": "Updated high-performance laptop with 32GB RAM",
  "price": 1500,
  "category": "electronics",
  "inStock": true
}
```

**Error Responses:**
- **404:** `{"error": "Product not found"}`
- **400:** `{"error": "Invalid product data"}`

---

### 5. Delete Product
**Endpoint:** `DELETE /api/products/:id`

**Description:** Remove a product from the catalog.

**Headers:**
```
x-api-key: your-secret-api-key-here
```

**URL Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | Unique product identifier |

**Example Request:**
```bash
curl -X DELETE http://localhost:3000/api/products/1 \
  -H "x-api-key: your-secret-api-key-here"
```

**Success Response (204):**
No content returned (successful deletion)

**Error Response (404):**
```json
{
  "error": "Product not found"
}
```

---

### 6. Get Product Statistics by Category
**Endpoint:** `GET /api/products/stats/category`

**Description:** Get count of products by category.

**Headers:**
```
x-api-key: your-secret-api-key-here
```

**Example Request:**
```bash
curl -X GET http://localhost:3000/api/products/stats/category \
  -H "x-api-key: your-secret-api-key-here"
```

**Success Response (200):**
```json
{
  "electronics": 2,
  "kitchen": 1
}
```

---

## 🛡️ Middleware

### 1. Logger Middleware
**Location:** `middleware/logger.js`

**Purpose:** Logs all incoming requests with timestamp, method, and URL.

**Output Example:**
```
[2025-08-30T14:23:45.123Z] GET /api/products
[2025-08-30T14:23:50.456Z] POST /api/products
```

### 2. Authentication Middleware
**Location:** `middleware/auth.js`

**Purpose:** Validates API key in request headers.

**Header Required:** `x-api-key`

**Error Response (401):**
```json
{
  "error": "Unauthorized"
}
```

### 3. Error Handler Middleware
**Location:** `middleware/errorHandler.js`

**Purpose:** Centralized error handling with appropriate status codes.

**Handles:**
- 404 Not Found errors
- 400 Validation errors
- 500 Internal Server errors

---

## ❌ Error Handling

### Custom Error Classes
**Location:** `errors/customErrors.js`

#### NotFoundError (404)
```javascript
throw new NotFoundError('Product not found');
```

#### ValidationError (400)
```javascript
throw new ValidationError('Invalid product data');
```

### HTTP Status Codes Used
| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful GET, PUT requests |
| 201 | Created | Successful POST request |
| 204 | No Content | Successful DELETE request |
| 400 | Bad Request | Invalid data provided |
| 401 | Unauthorized | Missing or invalid API key |
| 404 | Not Found | Resource doesn't exist |
| 500 | Internal Server Error | Unexpected server error |

---

## 🧪 Testing

### Using Postman

1. **Import Collection:**
   - Create a new collection called "Product API"
   - Add environment variable `API_KEY` with your key

2. **Test Endpoints:**
   - Set header `x-api-key: {{API_KEY}}`
   - Test each endpoint with sample data

### Using cURL

#### Test GET all products:
```bash
curl -X GET http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here"
```

#### Test POST (create product):
```bash
curl -X POST http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test Product",
    "description": "This is a test",
    "price": 99.99,
    "category": "test",
    "inStock": true
  }'
```

#### Test Authentication (without API key):
```bash
curl -X GET http://localhost:3000/api/products
# Should return: {"error": "Unauthorized"}
```

---

## 📁 Project Structure

```
express-js-server-side-framework-Kim-dr/
│
├── middleware/
│   ├── auth.js              # Authentication middleware
│   ├── errorHandler.js      # Error handling middleware
│   └── logger.js            # Request logging middleware
│
├── errors/
│   └── customErrors.js      # Custom error classes
│
├── node_modules/            # Dependencies (not committed)
│
├── .env                     # Environment variables (not committed)
├── .env.example             # Example environment file
├── .gitignore               # Git ignore rules
├── package.json             # Project dependencies and scripts
├── package-lock.json        # Locked dependency versions
├── README.md                # This file
├── server.js                # Main application file
└── Week2-Assignment.md      # Assignment instructions
```

---

## 💡 Examples

### Complete Workflow Example

#### 1. Start the server
```bash
npm run dev
```

#### 2. Get all products
```bash
curl -X GET http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here"
```

#### 3. Create a new product
```bash
curl -X POST http://localhost:3000/api/products \
  -H "x-api-key: your-secret-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Wireless Headphones",
    "description": "Noise-cancelling bluetooth headphones",
    "price": 150,
    "category": "electronics",
    "inStock": true
  }'
```

#### 4. Update the product
```bash
curl -X PUT http://localhost:3000/api/products/[product-id] \
  -H "x-api-key: your-secret-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Wireless Headphones Pro",
    "description": "Premium noise-cancelling bluetooth headphones",
    "price": 199,
    "category": "electronics",
    "inStock": true
  }'
```

#### 5. Delete the product
```bash
curl -X DELETE http://localhost:3000/api/products/[product-id] \
  -H "x-api-key: your-secret-api-key-here"
```

---

## 🐛 Troubleshooting

### Common Issues

#### 1. "Cannot find module 'express'"
```bash
# Solution: Install dependencies
npm install
```

#### 2. "Port 3000 is already in use"
```bash
# Solution 1: Change port in .env
PORT=3001

# Solution 2: Kill the process using port 3000
# On Mac/Linux:
lsof -ti:3000 | xargs kill -9

# On Windows:
netstat -ano | findstr :3000
taskkill /PID [PID_NUMBER] /F
```

#### 3. "Unauthorized" error
```bash
# Solution: Check your API key
# Make sure x-api-key header matches .env API_KEY
```

#### 4. Server not restarting with nodemon
```bash
# Solution: Manually restart
# Stop server: Ctrl + C
# Start again: npm run dev
```

#### 5. "Invalid product data" error
```bash
# Solution: Check all required fields
# All fields must be present: name, description, price, category, inStock
# price must be a number
# inStock must be boolean (true/false)
```

---

## 🚀 Next Steps / Future Enhancements

- [ ] Connect to MongoDB database instead of in-memory storage
- [ ] Add user authentication with JWT tokens
- [ ] Implement product images upload
- [ ] Add product reviews and ratings
- [ ] Create API documentation with Swagger
- [ ] Add unit and integration tests
- [ ] Implement rate limiting
- [ ] Add CORS configuration
- [ ] Deploy to production (Heroku, Railway, etc.)

---

## 📝 Assignment Requirements Checklist

- [x] Express.js server setup
- [x] RESTful API routes (GET, POST, PUT, DELETE)
- [x] Custom logger middleware
- [x] Authentication middleware
- [x] Error handling middleware
- [x] Custom error classes
- [x] Query parameters for filtering
- [x] Pagination support
- [x] Search functionality
- [x] Product statistics endpoint
- [x] Comprehensive documentation

---

## 👤 Author

**Kim-dr**
- GitHub: [@Kim-dr](https://github.com/Kim-dr)
- Assignment Repository: [express-js-server-side-framework-Kim-dr](https://github.com/PLP-MERN-Stack-Development/express-js-server-side-framework-Kim-dr)

---

## 📄 License

This project is part of the PLP MERN Stack Development course assignment.

---

## 🙏 Acknowledgments

- Express.js Documentation
- PLP Academy MERN Stack Development Course
- Node.js Community

---

## 📞 Support

If you encounter any issues or have questions:

1. Check the [Troubleshooting](#troubleshooting) section
2. Review the [Examples](#examples)
3. Contact your instructor or teaching assistant
4. Create an issue in the GitHub repository

---

**Happy Coding! 🚀**