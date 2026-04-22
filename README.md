# E-Commerce MERN Stack Application

A production-ready, full-stack e-commerce platform built with the MERN stack (MongoDB, Express.js, React, and Node.js). This project emphasizes solid system design principles with carefully architected Low-Level Design (LLD) and High-Level Design (HLD).

## 🚀 Features

### User Features
- User registration and authentication (JWT-based)
- Browse products by categories
- Advanced product search and filtering
- Product detail pages with reviews and ratings
- Shopping cart management
- Wishlist functionality
- Order placement and tracking
- Multiple payment options (Stripe, PayPal)
- Order history and invoice generation
- User profile management
- Address book management

### Admin Features
- Product management (CRUD operations)
- Category management
- Inventory tracking
- Order management and status updates
- User management
- Sales analytics and reporting
- Dashboard with key metrics
- Bulk product upload
- Discount and coupon management

### Additional Features
- Responsive design for mobile and desktop
- Image optimization and lazy loading
- Real-time notifications
- Email notifications for orders
- Product recommendations
- Review and rating system
- Advanced search with autocomplete
- Multi-language support (i18n)
- Dark/Light theme toggle

## 🛠️ Technology Stack

### Frontend
- **React** 18+ with Hooks
- **Redux Toolkit** for state management
- **React Router** for navigation
- **Material-UI / Tailwind CSS** for styling
- **Axios** for API calls
- **Formik & Yup** for form handling and validation
- **React Query** for data fetching and caching

### Backend
- **Node.js** with Express.js
- **MongoDB** with Mongoose ODM
- **JWT** for authentication
- **Bcrypt** for password hashing
- **Multer** for file uploads
- **Cloudinary** for image storage
- **Nodemailer** for email services
- **Express Validator** for input validation

### Payment Integration
- Stripe API
- PayPal SDK

### DevOps & Tools
- **Docker** for containerization
- **Git** for version control
- **Jest & React Testing Library** for testing
- **ESLint & Prettier** for code quality
- **GitHub Actions** for CI/CD

## 📁 Project Structure

```
2026_MERN_Stack_Application/
├── client/                 # React frontend
│   ├── public/
│   ├── src/
│   │   ├── components/    # Reusable components
│   │   ├── pages/         # Page components
│   │   ├── redux/         # Redux store and slices
│   │   ├── services/      # API service calls
│   │   ├── utils/         # Utility functions
│   │   ├── hooks/         # Custom React hooks
│   │   ├── assets/        # Images, fonts, etc.
│   │   └── App.js
│   └── package.json
├── server/                # Node.js backend
│   ├── config/           # Configuration files
│   ├── controllers/      # Route controllers
│   ├── models/          # Mongoose models
│   ├── routes/          # API routes
│   ├── middleware/      # Custom middleware
│   ├── utils/           # Helper functions
│   ├── uploads/         # Uploaded files
│   └── server.js
├── docs/                # Documentation
│   └── Architecture.md
├── docker-compose.yml
└── README.md
```

## 🚦 Getting Started

### Prerequisites
- Node.js (v18 or higher)
- MongoDB (v6 or higher)
- npm or yarn
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd 2026_MERN_Stack_Application
   ```

2. **Install server dependencies**
   ```bash
   cd server
   npm install
   ```

3. **Install client dependencies**
   ```bash
   cd ../client
   npm install
   ```

4. **Environment Variables**
   
   Create `.env` file in the server directory:
   ```env
   NODE_ENV=development
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/ecommerce
   JWT_SECRET=your_jwt_secret_key
   JWT_EXPIRE=7d
   
   # Cloudinary
   CLOUDINARY_CLOUD_NAME=your_cloud_name
   CLOUDINARY_API_KEY=your_api_key
   CLOUDINARY_API_SECRET=your_api_secret
   
   # Stripe
   STRIPE_SECRET_KEY=your_stripe_secret_key
   STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
   
   # PayPal
   PAYPAL_CLIENT_ID=your_paypal_client_id
   PAYPAL_CLIENT_SECRET=your_paypal_client_secret
   
   # Email
   SMTP_HOST=smtp.gmail.com
   SMTP_PORT=587
   SMTP_USER=your_email@gmail.com
   SMTP_PASS=your_email_password
   
   # Frontend URL
   CLIENT_URL=http://localhost:3000
   ```
   
   Create `.env` file in the client directory:
   ```env
   REACT_APP_API_URL=http://localhost:5000/api
   REACT_APP_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
   ```

5. **Run the application**
   
   Run server (from server directory):
   ```bash
   npm run dev
   ```
   
   Run client (from client directory):
   ```bash
   npm start
   ```
   
   The client will run on `http://localhost:3000` and server on `http://localhost:5000`

### Using Docker

```bash
docker-compose up --build
```

## 🧪 Testing

```bash
# Run server tests
cd server
npm test

# Run client tests
cd client
npm test

# Run tests with coverage
npm test -- --coverage
```

## 📚 API Documentation

### Authentication Endpoints
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - User login
- `GET /api/auth/me` - Get current user
- `PUT /api/auth/updateprofile` - Update user profile
- `POST /api/auth/forgotpassword` - Forgot password

### Product Endpoints
- `GET /api/products` - Get all products
- `GET /api/products/:id` - Get single product
- `POST /api/products` - Create product (Admin)
- `PUT /api/products/:id` - Update product (Admin)
- `DELETE /api/products/:id` - Delete product (Admin)
- `POST /api/products/:id/reviews` - Add review

### Order Endpoints
- `POST /api/orders` - Create new order
- `GET /api/orders/myorders` - Get user orders
- `GET /api/orders/:id` - Get order by ID
- `PUT /api/orders/:id/pay` - Update order to paid
- `PUT /api/orders/:id/deliver` - Update order to delivered (Admin)

### Cart Endpoints
- `GET /api/cart` - Get user cart
- `POST /api/cart` - Add item to cart
- `PUT /api/cart/:id` - Update cart item
- `DELETE /api/cart/:id` - Remove item from cart

For detailed API documentation, visit `/api-docs` after running the server.

## 🔒 Security Features

- JWT authentication with httpOnly cookies
- Password hashing with bcrypt
- Rate limiting on API endpoints
- Input validation and sanitization
- XSS protection
- CORS configuration
- Helmet.js for security headers
- MongoDB injection prevention

## 🎨 UI/UX Features

- Modern and clean design
- Smooth animations and transitions
- Loading states and skeletons
- Error boundaries
- Toast notifications
- Infinite scroll for products
- Image zoom on product pages
- Mobile-first responsive design

## 🚀 Deployment

### Frontend (Vercel/Netlify)
```bash
cd client
npm run build
# Deploy build folder
```

### Backend (Heroku/Railway/DigitalOcean)
```bash
cd server
# Configure environment variables
# Deploy using platform-specific commands
```

### Database (MongoDB Atlas)
- Create cluster on MongoDB Atlas
- Update MONGODB_URI in environment variables

## 📈 Future Enhancements

- [ ] Social media authentication (Google, Facebook)
- [ ] Real-time chat support
- [ ] Product comparison feature
- [ ] Advanced analytics dashboard
- [ ] Mobile app (React Native)
- [ ] Multi-vendor marketplace support
- [ ] Subscription-based products
- [ ] Gift cards and vouchers
- [ ] Loyalty points system
- [ ] Advanced SEO optimization

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- Your Name - Initial work

## 🙏 Acknowledgments

- [MERN Stack documentation](https://www.mongodb.com/mern-stack)
- [Stripe Documentation](https://stripe.com/docs)
- [Material-UI](https://mui.com/)
- Community contributors

## 📞 Support

For support, email support@ecommerce.com or join our Slack channel.

---

**Built with ❤️ using the MERN Stack**
