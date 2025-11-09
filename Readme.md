# 📚 BookStore - Online Bookstore System

A full-stack e-commerce web application for selling books online, built with Node.js, Express, PostgreSQL, and Stripe payment integration.

## 🌟 Features

### Customer Features
- 🔐 **User Authentication** - Register, login, and secure session management with JWT
- 📖 **Browse Books** - Search and filter books by category
- 🛒 **Shopping Cart** - Add, update, and remove items from cart
- 💳 **Payment Processing** - Secure payment via Stripe integration
- 📦 **Order History** - View past orders with tracking information
- 👤 **Profile Management** - Update personal information and shipping address

### Admin Features
- 📊 **Dashboard Overview** - View statistics (total books, orders, users, revenue)
- 📚 **Product Management** - Add, edit, and delete books
- 📋 **Order Management** - View and update order status
- 🚚 **Shipping Management** - Add tracking numbers and mark orders as shipped

## 🛠️ Tech Stack

### Backend
- **Node.js** with Express.js
- **PostgreSQL** database
- **JWT** for authentication
- **Stripe** for payment processing
- **bcrypt** for password hashing

### Frontend
- **HTML5/CSS3**
- **Bootstrap 5.3** for UI components
- **Vanilla JavaScript**
- **Axios** for HTTP requests

## 📋 Prerequisites

Before running this project, make sure you have:

- Node.js (v14 or higher)
- PostgreSQL (v12 or higher)
- Stripe account for payment integration
- npm or yarn package manager

## 🚀 Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd bookstore
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**

Create a `.env` file in the root directory:
```env
# Stripe Keys
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret

# Database Credentials
DB_USER=postgres
DB_HOST=localhost
DB_DATABASE=bookstore
DB_PASSWORD=your_password
DB_PORT=5432

# JWT Secret
JWT_SECRET=your_jwt_secret
```

4. **Set up the database**

Create a PostgreSQL database named `bookstore` and run the following schema:

```sql
-- Users table
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(50) DEFAULT 'customer',
    firstname VARCHAR(100),
    lastname VARCHAR(100),
    phone VARCHAR(20),
    address TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products table
CREATE TABLE products (
    book_id SERIAL PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    author VARCHAR(255) NOT NULL,
    description TEXT,
    book_type VARCHAR(100),
    book_price DECIMAL(10, 2) NOT NULL,
    old_price DECIMAL(10, 2),
    image_url TEXT,
    stock INTEGER DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Cart items table
CREATE TABLE cart_item (
    user_id INTEGER REFERENCES users(user_id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(book_id) ON DELETE CASCADE,
    cart_item_quantity INTEGER DEFAULT 1,
    PRIMARY KEY (user_id, product_id)
);

-- Orders table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(user_id),
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50) DEFAULT 'pending',
    stripe_payment_intent_id VARCHAR(255),
    tracking_number VARCHAR(100),
    recipient_name VARCHAR(255),
    recipient_phone VARCHAR(20),
    shipping_address TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Order items table
CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES orders(order_id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(book_id),
    quantity INTEGER NOT NULL,
    price_at_purchase DECIMAL(10, 2) NOT NULL
);

-- Refresh tokens table
CREATE TABLE refresh_tokens (
    token_id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(user_id) ON DELETE CASCADE,
    token TEXT NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

5. **Start the server**
```bash
node index.js
```

The server will start on `http://localhost:8000`

## 📁 Project Structure

```
bookstore/
├── public/              # Frontend files
│   ├── index.html       # Homepage/Product listing
│   ├── login.html       # Login page
│   ├── register.html    # Registration page
│   ├── checkout.html    # Checkout page
│   ├── successpay.html  # Payment success page
│   ├── order.html       # Order history
│   ├── userprofile.html # User profile
│   ├── admin.html       # Admin product management
│   ├── manage_order.html# Admin order management
│   └── overview.html    # Admin dashboard
├── index.js             # Main server file
├── .env                 # Environment variables
├── .gitignore          # Git ignore file
├── API.md              # API documentation
└── package.json        # Project dependencies
```

## 🔑 Default Admin Account

After setting up the database, you can create an admin account by:

1. Register a normal user through the website
2. Manually update the user role in the database:
```sql
UPDATE users SET role = 'admin' WHERE email = 'your_email@example.com';
```

## 📡 API Endpoints

For detailed API documentation, see [API.md](API.md)

### Key Endpoints:
- `POST /api/register` - User registration
- `POST /api/login` - User login
- `GET /api/books` - Get all books
- `POST /api/cart` - Add to cart
- `POST /api/create-payment-intent` - Create Stripe payment
- `GET /api/admin/products` - Get all products (Admin)
- `PUT /api/admin/order/:orderId/tracking` - Update tracking (Admin)

## 🔒 Security Features

- **Password Hashing** - Using bcrypt
- **JWT Authentication** - Access & refresh token system
- **Rate Limiting** - Login attempt limits (5 attempts per 15 minutes)
- **CORS Protection** - Configured for specific origins
- **HTTP-only Cookies** - Secure token storage
- **Input Validation** - Server-side validation for all inputs

## 💳 Stripe Integration

This project uses Stripe for payment processing:

1. **Test Mode** - Currently configured for test mode
2. **Webhook** - Listening on `/api/stripe-webhook` for payment events
3. **Test Cards**:
   - Success: `4242 4242 4242 4242`
   - Decline: `4000 0000 0000 0002`

## 🎨 Frontend Features

- **Responsive Design** - Works on desktop and mobile
- **Real-time Cart Updates** - Dynamic cart management
- **Search & Filter** - Easy product discovery
- **Toast Notifications** - User feedback for actions
- **Loading States** - Better UX with loading indicators

## 🐛 Troubleshooting

### Common Issues:

1. **Database Connection Error**
   - Check PostgreSQL is running
   - Verify credentials in `.env` file

2. **Stripe Webhook Not Working**
   - Use Stripe CLI for local testing
   - Verify webhook secret in `.env`

3. **CORS Errors**
   - Check origin configuration in `index.js`
   - Ensure cookies are enabled

## 📝 License

This project is for educational purposes.

## 👥 Contributors

- APIVIT YINGYAITHANASAK
- POKPONG PADJUNGREED

## 🙏 Acknowledgments

- Bootstrap for UI components
- Stripe for payment processing
- Express.js community

---
## 🙏 command stripe
stripe listen --forward-to localhost:8000/api/stripe-webhook
**Note**: Remember to never commit your `.env` file or expose your Stripe secret keys!
