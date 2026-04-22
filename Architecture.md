# E-Commerce Application Architecture

## Table of Contents
1. [System Overview](#system-overview)
2. [Architecture Pattern](#architecture-pattern)
3. [System Components](#system-components)
4. [Data Flow](#data-flow)
5. [Database Design](#database-design)
6. [API Architecture](#api-architecture)
7. [Security Architecture](#security-architecture)
8. [Scalability Considerations](#scalability-considerations)

---

## System Overview

The E-Commerce application follows a three-tier architecture pattern with a clear separation between presentation, business logic, and data layers. The system is built using the MERN stack (MongoDB, Express.js, React, Node.js) and follows modern software engineering principles including microservices readiness, RESTful API design, and component-based frontend architecture.

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        Client Layer                          │
│  ┌────────────────────────────────────────────────────────┐ │
│  │            React Application (SPA)                      │ │
│  │  ┌──────────┐  ┌──────────┐  ┌─────────────────────┐  │ │
│  │  │   UI     │  │  Redux   │  │   React Router      │  │ │
│  │  │Components│  │  Store   │  │   (Navigation)      │  │ │
│  │  └──────────┘  └──────────┘  └─────────────────────┘  │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                        HTTPS/REST API
                              │
┌─────────────────────────────────────────────────────────────┐
│                      Application Layer                       │
│  ┌────────────────────────────────────────────────────────┐ │
│  │              Express.js Server                          │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐ │ │
│  │  │Middleware│  │Controller│  │    Business Logic    │ │ │
│  │  │  Layer   │  │  Layer   │  │      & Services      │ │ │
│  │  └──────────┘  └──────────┘  └──────────────────────┘ │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                         Mongoose ORM
                              │
┌─────────────────────────────────────────────────────────────┐
│                        Data Layer                            │
│  ┌────────────────────────────────────────────────────────┐ │
│  │                  MongoDB Database                       │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐ │ │
│  │  │  Users   │  │ Products │  │   Orders/Cart/...    │ │ │
│  │  │Collection│  │Collection│  │    Collections       │ │ │
│  │  └──────────┘  └──────────┘  └──────────────────────┘ │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘

        ┌─────────────────────────────────────┐
        │      External Services              │
        │  ┌─────────┐  ┌─────────────────┐  │
        │  │ Stripe  │  │   Cloudinary    │  │
        │  │ PayPal  │  │   SMTP Server   │  │
        │  └─────────┘  └─────────────────┘  │
        └─────────────────────────────────────┘
```

---

## Architecture Pattern

### MVC (Model-View-Controller) Pattern

The application implements a modified MVC architecture:

- **Model**: MongoDB schemas defined with Mongoose ODM
- **View**: React components (frontend)
- **Controller**: Express.js route controllers (backend)

### Component-Based Architecture (Frontend)

```
React Application
│
├── Presentational Components (Stateless)
│   ├── ProductCard
│   ├── Button
│   ├── Input
│   └── Header
│
├── Container Components (Stateful)
│   ├── ProductList
│   ├── CartContainer
│   ├── CheckoutFlow
│   └── UserProfile
│
└── Page Components
    ├── HomePage
    ├── ProductDetailPage
    ├── CheckoutPage
    └── AdminDashboard
```

---

## System Components

### 1. Frontend Architecture (React)

#### Component Hierarchy

```
App
│
├── Layout
│   ├── Header
│   │   ├── Navigation
│   │   ├── SearchBar
│   │   └── UserMenu
│   ├── Footer
│   └── Sidebar
│
├── Pages
│   ├── Home
│   │   ├── HeroSection
│   │   ├── FeaturedProducts
│   │   └── CategoryGrid
│   │
│   ├── Products
│   │   ├── ProductFilters
│   │   ├── ProductGrid
│   │   └── Pagination
│   │
│   ├── ProductDetail
│   │   ├── ImageGallery
│   │   ├── ProductInfo
│   │   ├── AddToCart
│   │   └── Reviews
│   │
│   ├── Cart
│   │   ├── CartItems
│   │   ├── CartSummary
│   │   └── CouponInput
│   │
│   ├── Checkout
│   │   ├── ShippingForm
│   │   ├── PaymentForm
│   │   └── OrderSummary
│   │
│   ├── UserDashboard
│   │   ├── OrderHistory
│   │   ├── Wishlist
│   │   └── AccountSettings
│   │
│   └── AdminDashboard
│       ├── ProductManagement
│       ├── OrderManagement
│       ├── UserManagement
│       └── Analytics
│
└── Common Components
    ├── Modal
    ├── Toast
    ├── Loader
    └── ErrorBoundary
```

#### State Management (Redux)

```javascript
Store Structure:
{
  auth: {
    user: {},
    token: '',
    isAuthenticated: false,
    loading: false
  },
  products: {
    items: [],
    currentProduct: {},
    loading: false,
    filters: {},
    pagination: {}
  },
  cart: {
    items: [],
    totalPrice: 0,
    itemCount: 0
  },
  orders: {
    list: [],
    currentOrder: {},
    loading: false
  },
  ui: {
    theme: 'light',
    notifications: [],
    modals: {}
  }
}
```

### 2. Backend Architecture (Node.js/Express)

#### Layered Architecture

```
Server Application
│
├── API Layer (Routes)
│   ├── /api/auth
│   ├── /api/products
│   ├── /api/orders
│   ├── /api/users
│   ├── /api/cart
│   └── /api/admin
│
├── Controller Layer
│   ├── authController.js
│   ├── productController.js
│   ├── orderController.js
│   ├── userController.js
│   └── adminController.js
│
├── Service Layer (Business Logic)
│   ├── authService.js
│   ├── productService.js
│   ├── orderService.js
│   ├── paymentService.js
│   └── emailService.js
│
├── Middleware Layer
│   ├── authMiddleware.js
│   ├── errorHandler.js
│   ├── validator.js
│   ├── rateLimiter.js
│   └── uploadMiddleware.js
│
└── Data Access Layer (Models)
    ├── User.js
    ├── Product.js
    ├── Order.js
    ├── Cart.js
    ├── Review.js
    └── Category.js
```

---

## Data Flow

### 1. User Registration Flow

```
User Input → React Form
     ↓
Validation (Formik/Yup)
     ↓
Redux Action (registerUser)
     ↓
API Call (POST /api/auth/register)
     ↓
Express Route Handler
     ↓
Controller (validate input)
     ↓
Service Layer (hash password)
     ↓
MongoDB (save user)
     ↓
Generate JWT Token
     ↓
Response → Redux Store → Update UI
```

### 2. Product Purchase Flow

```
User Adds to Cart
     ↓
Update Local Cart State (Redux)
     ↓
Sync with Backend (POST /api/cart)
     ↓
User Proceeds to Checkout
     ↓
Shipping Information Form
     ↓
Payment Method Selection
     ↓
Process Payment (Stripe/PayPal API)
     ↓
Create Order (POST /api/orders)
     ↓
Update Inventory
     ↓
Send Confirmation Email
     ↓
Redirect to Order Success Page
```

### 3. Admin Product Management Flow

```
Admin Login → Dashboard
     ↓
Navigate to Products
     ↓
Create/Edit Product Form
     ↓
Upload Images → Cloudinary
     ↓
Submit Product Data
     ↓
Validation Layer
     ↓
Save to MongoDB
     ↓
Update Product Cache (if applicable)
     ↓
Notify Admin → Update Product List
```

---

## Database Design

### Entity Relationship Diagram

```
┌─────────────────┐
│     User        │
│─────────────────│
│ _id (PK)        │
│ name            │
│ email (unique)  │
│ password (hash) │
│ role            │
│ addresses []    │
│ createdAt       │
└─────────────────┘
        │ 1
        │
        │ N
┌─────────────────┐       N  ┌─────────────────┐
│     Order       │◄──────────┤   OrderItem     │
│─────────────────│            │─────────────────│
│ _id (PK)        │            │ _id (PK)        │
│ user (FK)       │            │ order (FK)      │
│ orderItems []   │            │ product (FK)    │
│ shippingAddress │            │ quantity        │
│ paymentMethod   │            │ price           │
│ paymentResult   │            └─────────────────┘
│ totalPrice      │                    │ N
│ status          │                    │
│ paidAt          │                    │ 1
│ deliveredAt     │            ┌─────────────────┐
└─────────────────┘            │    Product      │
        │ 1                     │─────────────────│
        │                       │ _id (PK)        │
        │ N                     │ name            │
┌─────────────────┐            │ description     │
│      Cart       │            │ price           │
│─────────────────│            │ category (FK)   │
│ _id (PK)        │            │ brand           │
│ user (FK)       │            │ images []       │
│ items []        │            │ stock           │
│ totalPrice      │            │ rating          │
└─────────────────┘            │ numReviews      │
        │ 1                     │ createdAt       │
        │                       └─────────────────┘
        │ N                             │ 1
┌─────────────────┐                    │
│    CartItem     │                    │ N
│─────────────────│            ┌─────────────────┐
│ _id (PK)        │            │     Review      │
│ cart (FK)       │            │─────────────────│
│ product (FK)    │────────────┤ _id (PK)        │
│ quantity        │      N     │ user (FK)       │
│ price           │            │ product (FK)    │
└─────────────────┘            │ rating          │
                               │ comment         │
                               │ createdAt       │
                               └─────────────────┘
                                       │ N
                                       │
                                       │ 1
                               ┌─────────────────┐
                               │    Category     │
                               │─────────────────│
                               │ _id (PK)        │
                               │ name            │
                               │ description     │
                               │ image           │
                               │ parent (FK)     │
                               └─────────────────┘
```

### MongoDB Collections Schema

#### User Collection
```javascript
{
  _id: ObjectId,
  name: String (required),
  email: String (required, unique, lowercase),
  password: String (required, hashed),
  role: String (enum: ['user', 'admin'], default: 'user'),
  avatar: String (URL),
  phone: String,
  addresses: [{
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String,
    isDefault: Boolean
  }],
  wishlist: [ObjectId (Product)],
  resetPasswordToken: String,
  resetPasswordExpire: Date,
  createdAt: Date,
  updatedAt: Date
}
```

#### Product Collection
```javascript
{
  _id: ObjectId,
  name: String (required),
  slug: String (unique),
  description: String (required),
  price: Number (required),
  compareAtPrice: Number,
  category: ObjectId (Category, required),
  brand: String,
  images: [{
    url: String,
    publicId: String (Cloudinary),
    isDefault: Boolean
  }],
  stock: Number (required, default: 0),
  sku: String (unique),
  rating: Number (default: 0),
  numReviews: Number (default: 0),
  specifications: [{
    key: String,
    value: String
  }],
  isFeatured: Boolean,
  isActive: Boolean (default: true),
  tags: [String],
  createdAt: Date,
  updatedAt: Date
}
```

#### Order Collection
```javascript
{
  _id: ObjectId,
  orderNumber: String (unique),
  user: ObjectId (User, required),
  orderItems: [{
    product: ObjectId (Product),
    name: String,
    quantity: Number,
    image: String,
    price: Number
  }],
  shippingAddress: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  paymentMethod: String (required),
  paymentResult: {
    id: String,
    status: String,
    updateTime: String,
    emailAddress: String
  },
  taxPrice: Number,
  shippingPrice: Number,
  totalPrice: Number (required),
  status: String (enum: ['pending', 'processing', 'shipped', 'delivered', 'cancelled']),
  isPaid: Boolean (default: false),
  paidAt: Date,
  isDelivered: Boolean (default: false),
  deliveredAt: Date,
  trackingNumber: String,
  notes: String,
  createdAt: Date,
  updatedAt: Date
}
```

#### Cart Collection
```javascript
{
  _id: ObjectId,
  user: ObjectId (User, required, unique),
  items: [{
    product: ObjectId (Product),
    quantity: Number (required, min: 1),
    price: Number
  }],
  totalPrice: Number,
  updatedAt: Date
}
```

#### Review Collection
```javascript
{
  _id: ObjectId,
  user: ObjectId (User, required),
  product: ObjectId (Product, required),
  rating: Number (required, min: 1, max: 5),
  title: String,
  comment: String (required),
  images: [String],
  isVerifiedPurchase: Boolean,
  helpfulCount: Number (default: 0),
  createdAt: Date,
  updatedAt: Date
}
```

#### Category Collection
```javascript
{
  _id: ObjectId,
  name: String (required, unique),
  slug: String (unique),
  description: String,
  image: String,
  parent: ObjectId (Category, null for root categories),
  level: Number,
  isActive: Boolean (default: true),
  order: Number,
  createdAt: Date,
  updatedAt: Date
}
```

---

## API Architecture

### RESTful API Design Principles

#### Endpoint Structure

```
Base URL: https://api.ecommerce.com/v1

Authentication:
  POST   /auth/register
  POST   /auth/login
  GET    /auth/me
  POST   /auth/logout
  POST   /auth/forgot-password
  POST   /auth/reset-password/:token

Users:
  GET    /users/profile
  PUT    /users/profile
  POST   /users/addresses
  PUT    /users/addresses/:id
  DELETE /users/addresses/:id
  GET    /users/wishlist
  POST   /users/wishlist/:productId
  DELETE /users/wishlist/:productId

Products:
  GET    /products
  GET    /products/:id
  GET    /products/slug/:slug
  GET    /products/featured
  GET    /products/search?q=query
  POST   /products (Admin)
  PUT    /products/:id (Admin)
  DELETE /products/:id (Admin)
  POST   /products/:id/reviews
  GET    /products/:id/reviews

Categories:
  GET    /categories
  GET    /categories/:id
  GET    /categories/:id/products
  POST   /categories (Admin)
  PUT    /categories/:id (Admin)
  DELETE /categories/:id (Admin)

Cart:
  GET    /cart
  POST   /cart/items
  PUT    /cart/items/:itemId
  DELETE /cart/items/:itemId
  DELETE /cart

Orders:
  POST   /orders
  GET    /orders
  GET    /orders/:id
  PUT    /orders/:id/cancel
  GET    /orders/:id/invoice
  PUT    /orders/:id/pay
  PUT    /orders/:id/deliver (Admin)
  GET    /admin/orders (Admin)

Payments:
  POST   /payments/stripe/create-intent
  POST   /payments/stripe/webhook
  POST   /payments/paypal/create-order
  POST   /payments/paypal/capture-order

Analytics (Admin):
  GET    /admin/analytics/dashboard
  GET    /admin/analytics/sales
  GET    /admin/analytics/products
  GET    /admin/analytics/customers
```

#### Request/Response Format

**Standard Success Response:**
```json
{
  "success": true,
  "data": {
    // response data
  },
  "message": "Operation successful",
  "timestamp": "2026-04-22T10:30:00Z"
}
```

**Standard Error Response:**
```json
{
  "success": false,
  "error": {
    "code": "PRODUCT_NOT_FOUND",
    "message": "Product not found",
    "details": []
  },
  "timestamp": "2026-04-22T10:30:00Z"
}
```

**Pagination Response:**
```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "totalPages": 10,
    "totalItems": 200,
    "hasNext": true,
    "hasPrev": false
  }
}
```

### API Authentication

**JWT Token Flow:**

1. User logs in → Receives access token + refresh token
2. Access token stored in httpOnly cookie (secure)
3. Refresh token stored in httpOnly cookie (longer expiry)
4. Client sends requests with token in Authorization header
5. Server validates token on protected routes
6. Token refresh mechanism for expired tokens

**Authorization Header:**
```
Authorization: Bearer <jwt_token>
```

---

## Security Architecture

### Authentication & Authorization

1. **Password Security**
   - Bcrypt hashing (10 rounds)
   - Password strength validation
   - Account lockout after failed attempts

2. **JWT Security**
   - Short-lived access tokens (15 minutes)
   - Longer-lived refresh tokens (7 days)
   - Token rotation on refresh
   - Blacklist for revoked tokens

3. **Session Management**
   - httpOnly cookies
   - Secure flag in production
   - SameSite attribute
   - CSRF protection

### Input Validation & Sanitization

```javascript
// Example validation middleware
const validateProduct = [
  body('name').trim().notEmpty().isLength({ min: 3, max: 100 }),
  body('price').isFloat({ min: 0 }),
  body('description').trim().notEmpty(),
  body('category').isMongoId(),
  sanitizeBody('name').escape(),
  sanitizeBody('description').escape()
];
```

### API Security Measures

1. **Rate Limiting**
   ```javascript
   // 100 requests per 15 minutes per IP
   const limiter = rateLimit({
     windowMs: 15 * 60 * 1000,
     max: 100
   });
   ```

2. **CORS Configuration**
   ```javascript
   const corsOptions = {
     origin: process.env.CLIENT_URL,
     credentials: true,
     optionsSuccessStatus: 200
   };
   ```

3. **Helmet.js Headers**
   - Content Security Policy
   - X-Content-Type-Options
   - X-Frame-Options
   - Strict-Transport-Security

4. **Data Encryption**
   - HTTPS/TLS in production
   - Encrypted sensitive data in database
   - Secure payment processing

### Payment Security

- PCI DSS compliance
- Tokenization for card data
- No storage of sensitive payment information
- Webhook signature verification
- 3D Secure authentication support

---

## Scalability Considerations

### Horizontal Scaling

1. **Load Balancing**
   ```
   ┌─────────┐
   │  Nginx  │ (Load Balancer)
   └────┬────┘
        │
   ┌────┴────────────────┐
   │                     │
   ▼                     ▼
   Node Server 1    Node Server 2
   ```

2. **Stateless Architecture**
   - JWT tokens (no server-side sessions)
   - Shared MongoDB instance
   - Centralized caching layer

### Database Optimization

1. **Indexing Strategy**
   ```javascript
   // Product indexes
   productSchema.index({ name: 'text', description: 'text' });
   productSchema.index({ category: 1, price: 1 });
   productSchema.index({ createdAt: -1 });
   productSchema.index({ rating: -1 });
   
   // User indexes
   userSchema.index({ email: 1 }, { unique: true });
   
   // Order indexes
   orderSchema.index({ user: 1, createdAt: -1 });
   orderSchema.index({ status: 1 });
   ```

2. **Query Optimization**
   - Use projections to limit fields
   - Implement pagination
   - Use aggregation pipeline for complex queries
   - Avoid N+1 queries with population

3. **Database Sharding** (Future consideration)
   - Shard by user ID for user data
   - Shard by category for products

### Caching Strategy

1. **Redis Caching**
   ```javascript
   // Cache frequently accessed data
   - Product listings (5 minutes TTL)
   - Category tree (1 hour TTL)
   - User sessions (7 days TTL)
   - Shopping cart (1 hour TTL)
   ```

2. **Client-Side Caching**
   - React Query for data caching
   - Service Workers for offline support
   - LocalStorage for cart persistence

### CDN Integration

- Static assets (images, CSS, JS) → CDN
- Cloudinary for image optimization
- Edge caching for faster delivery

### Microservices Migration Path

**Future architecture for scale:**

```
API Gateway
    │
    ├─ User Service
    ├─ Product Service
    ├─ Order Service
    ├─ Payment Service
    ├─ Notification Service
    └─ Analytics Service
```

### Performance Monitoring

1. **Application Monitoring**
   - Error tracking (Sentry)
   - Performance monitoring (New Relic)
   - Logging (Winston + ELK Stack)

2. **Database Monitoring**
   - MongoDB Atlas monitoring
   - Slow query analysis
   - Index usage statistics

3. **Key Metrics**
   - Response time
   - Throughput (requests/second)
   - Error rate
   - Database query time
   - Cache hit ratio

---

## Deployment Architecture

### Development Environment
```
Local Machine
├── MongoDB (Docker)
├── Node.js Server (localhost:5000)
└── React Dev Server (localhost:3000)
```

### Production Environment
```
┌──────────────┐
│    Users     │
└──────┬───────┘
       │
   ┌───▼────┐
   │  CDN   │ (Static Assets)
   └───┬────┘
       │
┌──────▼──────────┐
│  Load Balancer  │
└──────┬──────────┘
       │
   ┌───┴───────────────┐
   │                   │
┌──▼───────┐   ┌──────▼────┐
│  Server  │   │  Server   │
│ Instance │   │ Instance  │
└──┬───────┘   └──────┬────┘
   │                  │
   └────────┬─────────┘
            │
    ┌───────▼────────┐
    │  MongoDB       │
    │  Replica Set   │
    └────────────────┘
```

### CI/CD Pipeline

```
Git Push → GitHub
    ↓
GitHub Actions
    ↓
Run Tests
    ↓
Build Docker Image
    ↓
Push to Registry
    ↓
Deploy to Production
    ↓
Health Check
```

---

## Technology Decisions & Rationale

### Why MERN Stack?

1. **MongoDB**: Flexible schema for e-commerce, easy to scale
2. **Express.js**: Minimal, flexible web framework
3. **React**: Component-based UI, rich ecosystem
4. **Node.js**: JavaScript everywhere, non-blocking I/O

### Alternative Considerations

- **PostgreSQL** instead of MongoDB for ACID compliance
- **Next.js** for SSR and better SEO
- **GraphQL** instead of REST for flexible queries
- **TypeScript** for type safety

---

## Conclusion

This architecture provides a solid foundation for an e-commerce application with room for growth. The modular design allows for incremental improvements and scaling as the business grows. Key principles maintained throughout:

- **Separation of Concerns**: Clear boundaries between layers
- **Scalability**: Horizontal scaling capability
- **Security**: Multiple layers of protection
- **Performance**: Caching and optimization strategies
- **Maintainability**: Clean code and documentation

---

**Document Version**: 1.0  
**Last Updated**: April 22, 2026  
---
