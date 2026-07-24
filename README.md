# SudaCart 🛒

SudaCart is a full-stack online grocery shopping platform with separate user and seller experiences — browse and buy groceries as a customer, or manage products and orders as a seller, all from one app.

**Live:** [suda-cart.vercel.app](https://suda-cart.vercel.app/)

## Features

**User**
- Register & login (JWT-based authentication)
- Browse products by category, view best sellers
- Add / remove products from cart
- Manage delivery addresses
- Checkout via Cash on Delivery or online payment (Stripe)
- View order history & order status

**Seller**
- Separate seller registration & login
- Switch into seller mode via the "As Seller" option on the user menu
- Add new products (with image upload via Cloudinary)
- Update product details and stock status (in-stock / out-of-stock)
- View customer orders & order details

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Tailwind CSS, Vite |
| Backend | Node.js, Express |
| Database | MongoDB (Mongoose) |
| Image storage | Cloudinary |
| Payments | Stripe |
| Auth | JWT (cookie-based) |
| Deployment | Vercel |

## Project Structure

```
SudaCart/
├── client/                 # React frontend
│   ├── src/
│   ├── vercel.json
│   └── vite.config.js
└── server/                 # Express backend
    ├── configs/             # DB, Cloudinary, Multer config
    ├── controllers/         # route handlers
    ├── middleware/          # AuthUser, AuthSeller
    ├── models/               # Mongoose schemas
    ├── routes/               # API route definitions
    ├── server.js             # app entry point
    └── vercel.json
```

## User Flow

Register/login → explore the main page → view best sellers → browse categories → add to cart → checkout → add delivery address → pay (Cash on Delivery or Stripe) → done.

**Becoming a seller:** log in/register → open the user menu → select "As Seller" → fill out the seller form → add products, manage stock, and view order details.

## Getting Started

### Prerequisites

- Node.js (v18+ recommended)
- A MongoDB instance (local or Atlas)
- A [Cloudinary](https://console.cloudinary.com/app/) account (for product image storage)
- A [Stripe](https://dashboard.stripe.com/) account (for online payments)

### 1. Clone the repo

```bash
git clone https://github.com/sudarshan9518/SudaCart.git
cd SudaCart
```

### 2. Backend setup

```bash
cd server
npm install
```

Create a `.env` file in `server/`:

```env
MongoDB_URI=your_mongodb_connection_string
NODE_ENV=development
SECRET_KEY=your_jwt_secret

# Cloudinary
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_SECRET_KEY=

# Admin / seller credentials
SELLER_EMAIL=
SELLER_PASS=

# Stripe
STRIPE_PUBLIC_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

Run the backend (defaults to port 4000):

```bash
npm run dev
```

### 3. Frontend setup

```bash
cd ../client
npm install
```

Create a `.env` file in `client/`:

```env
VITE_CURRENCY="₹"
VITE_BACKEND_URL=http://localhost:4000
```

Run the frontend:

```bash
npm run dev
```

## API Overview

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/user/register` | Register a new user |
| POST | `/api/user/login` | User login |
| GET | `/api/user/is-auth` | Check user auth status |
| GET | `/api/user/logout` | Log out |
| POST | `/api/seller/login` | Seller login |
| GET | `/api/seller/is-auth` | Check seller auth status |
| GET | `/api/seller/logout` | Seller logout |
| POST | `/api/product/add` | Add a new product (seller only, image upload) |
| GET | `/api/product/list` | List all products |
| GET | `/api/product/id` | Get a single product by ID |
| POST | `/api/product/stock` | Update product stock (seller only) |
| POST | `/api/cart/update` | Update the logged-in user's cart |
| POST | `/api/address/add` | Add a delivery address |
| GET | `/api/address/get` | Get saved addresses |
| POST | `/api/order/cod` | Place an order (Cash on Delivery) |
| POST | `/api/order/stripe` | Place an order (Stripe payment) |
| GET | `/api/order/user` | Get the logged-in user's orders |
| GET | `/api/order/seller` | Get all orders (seller only) |
| POST | `/api/email/email` | Send an email (e.g. notifications) |
| POST | `/stripe` | Stripe webhook endpoint |

## Deployment

Both `client` and `server` include a `vercel.json` and are deployed independently on Vercel. Make sure `VITE_BACKEND_URL` on the frontend points to your deployed backend URL, and that the backend's `allowedOrigins` list (in `server.js`) includes your deployed frontend URL.
