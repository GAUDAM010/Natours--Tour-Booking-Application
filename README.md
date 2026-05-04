# 🌿 Natours — Tour Booking Application

A full-stack, production-ready tour booking web application built with **Node.js**, **Express**, **MongoDB**, and **Pug** templates. Browse exciting nature tours, manage your account, leave reviews, and book tours with integrated **Stripe** payments.

---

## ✨ Features

### 🗺️ Tours
- Browse all available tours with details like duration, difficulty, price, and ratings
- View tour locations on an interactive map
- Get detailed tour information including itinerary, guides, and user reviews
- Filter, sort, and paginate tours via a powerful API
- Access tour statistics and monthly plans via aggregation pipelines

### 👤 Users & Authentication
- Secure user signup and login with **JWT**-based authentication
- Role-based access control — `user`, `guide`, `lead-guide`, `admin`
- Password reset via email with token expiration
- Update user profile, photo, and password
- Account deactivation (soft delete)

### ⭐ Reviews
- Authenticated users can create, read, update, and delete reviews for tours
- Reviews are linked to both users and tours with nested routing support

### 💳 Bookings & Payments
- Book tours with **Stripe** checkout integration
- View your booked tours in a dedicated "My Tours" section
- Stripe webhook support for production payment processing

### 🔒 Security
- HTTP security headers via **Helmet**
- Rate limiting to prevent brute-force attacks
- Data sanitization against **NoSQL injection** and **XSS**
- Prevention of HTTP parameter pollution
- CORS enabled for cross-origin access

### 📧 Email
- Transactional emails using **Nodemailer** with Pug-based HTML templates
- Welcome emails on signup
- Password reset emails
- **SendGrid** integration for production

---

## 🛠️ Tech Stack

| Layer          | Technology                                       |
| -------------- | ------------------------------------------------ |
| **Runtime**    | Node.js                                          |
| **Framework**  | Express.js                                       |
| **Database**   | MongoDB Atlas + Mongoose ODM                     |
| **Templating** | Pug                                              |
| **Auth**       | JSON Web Tokens (JWT) + bcrypt.js                |
| **Payments**   | Stripe                                           |
| **Email**      | Nodemailer / SendGrid                            |
| **Bundler**    | Parcel                                           |
| **Linting**    | ESLint (Airbnb config) + Prettier                |

---

## 📁 Project Structure

```
Natours/
├── controllers/           # Route handler logic
│   ├── authController.js       # Authentication & authorization
│   ├── bookingController.js    # Stripe checkout & bookings
│   ├── errorController.js      # Global error handling
│   ├── handlerFactory.js       # Generic CRUD factory functions
│   ├── reviewController.js     # Review CRUD
│   ├── tourController.js       # Tour CRUD & aggregations
│   ├── userController.js       # User profile management
│   └── viewsController.js      # Server-rendered page controllers
├── dev-data/              # Seed data & sample images
│   ├── data/                   # JSON seed files
│   ├── img/                    # Sample images
│   └── templates/              # Email & page templates
├── models/                # Mongoose schemas & models
│   ├── bookingModel.js
│   ├── reviewModel.js
│   ├── tourModel.js
│   └── userModel.js
├── public/                # Static assets (CSS, JS, images)
│   ├── css/
│   ├── img/
│   └── js/
├── routes/                # Express route definitions
│   ├── bookingRoutes.js
│   ├── reviewRoutes.js
│   ├── tourRoutes.js
│   ├── userRoutes.js
│   └── viewRoutes.js
├── utils/                 # Helper utilities
│   ├── apiFeatures.js          # Filtering, sorting, pagination
│   ├── appError.js             # Custom error class
│   ├── catchAsync.js           # Async error wrapper
│   └── email.js                # Email transporter & templates
├── views/                 # Pug templates
│   ├── email/                  # Email HTML templates
│   ├── base.pug                # Layout template
│   ├── overview.pug            # Tour listing page
│   ├── tour.pug                # Single tour detail page
│   ├── login.pug               # Login page
│   ├── account.pug             # User account settings
│   └── error.pug               # Error page
├── app.js                 # Express app configuration & middleware
├── server.js              # Server entry point & DB connection
├── config.env             # Environment variables (not committed)
├── package.json
└── .eslintrc.json
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v16 or higher
- **npm** v8 or higher
- A **MongoDB Atlas** cluster (or local MongoDB instance)
- A **Stripe** account (for payments)

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/natours.git
cd natours
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `config.env` file in the project root with the following variables:

```env
NODE_ENV=development
PORT=3000

DATABASE=<your-mongodb-connection-string>
DATABASE_PASSWORD=<your-db-password>

JWT_SECRET=<your-jwt-secret-min-32-chars>
JWT_EXPIRES_IN=90d
JWT_COOKIE_EXPIRES_IN=90

EMAIL_USERNAME=<mailtrap-username>
EMAIL_PASSWORD=<mailtrap-password>
EMAIL_HOST=smtp.mailtrap.io
EMAIL_PORT=25
EMAIL_FROM=you@example.com

SENDGRID_USERNAME=<sendgrid-username>
SENDGRID_PASSWORD=<sendgrid-api-key>

STRIPE_SECRET_KEY=<your-stripe-secret-key>
```

### 4. Run the Application

**Development mode** (with hot-reload via Nodemon):

```bash
npm run dev
```

**Production mode:**

```bash
npm run start:prod
```

The server starts at **http://localhost:3000**.

---

## 📡 API Reference

Base URL: `/api/v1`

### Tours

| Method   | Endpoint                      | Access          | Description                |
| -------- | ----------------------------- | --------------- | -------------------------- |
| `GET`    | `/tours`                      | Public          | Get all tours              |
| `GET`    | `/tours/:id`                  | Public          | Get a single tour          |
| `POST`   | `/tours`                      | Admin, Lead Guide | Create a new tour        |
| `PATCH`  | `/tours/:id`                  | Admin, Lead Guide | Update a tour            |
| `DELETE` | `/tours/:id`                  | Admin, Lead Guide | Delete a tour            |
| `GET`    | `/tours/top-5-cheap`          | Public          | Top 5 cheapest tours       |
| `GET`    | `/tours/tour-stats`           | Public          | Tour statistics            |
| `GET`    | `/tours/monthly-plan/:year`   | Protected       | Monthly plan for a year    |

### Users

| Method   | Endpoint                      | Access          | Description                |
| -------- | ----------------------------- | --------------- | -------------------------- |
| `POST`   | `/users/signup`               | Public          | Register a new user        |
| `POST`   | `/users/login`                | Public          | Log in                     |
| `GET`    | `/users/logout`               | Public          | Log out                    |
| `POST`   | `/users/forgotPassword`       | Public          | Request password reset     |
| `PATCH`  | `/users/resetPassword/:token` | Public          | Reset password with token  |
| `PATCH`  | `/users/updateMyPassword`     | Protected       | Update current password    |
| `PATCH`  | `/users/updateMe`             | Protected       | Update profile info        |
| `DELETE` | `/users/deleteMe`             | Protected       | Deactivate account         |

### Reviews

| Method   | Endpoint                          | Access          | Description            |
| -------- | --------------------------------- | --------------- | ---------------------- |
| `GET`    | `/tours/:tourId/reviews`          | Protected       | Get reviews for a tour |
| `POST`   | `/tours/:tourId/reviews`          | User            | Create a review        |
| `PATCH`  | `/reviews/:id`                    | User, Admin     | Update a review        |
| `DELETE` | `/reviews/:id`                    | User, Admin     | Delete a review        |

### Bookings

| Method   | Endpoint                              | Access          | Description                  |
| -------- | ------------------------------------- | --------------- | ---------------------------- |
| `GET`    | `/bookings/checkout-session/:tourId`  | Protected       | Get Stripe checkout session  |
| `GET`    | `/bookings`                           | Admin, Lead Guide | Get all bookings           |
| `POST`   | `/bookings`                           | Admin, Lead Guide | Create a booking           |
| `GET`    | `/bookings/:id`                       | Admin, Lead Guide | Get a booking              |
| `PATCH`  | `/bookings/:id`                       | Admin, Lead Guide | Update a booking           |
| `DELETE` | `/bookings/:id`                       | Admin, Lead Guide | Delete a booking           |

---

## 🧪 Available Scripts

| Script              | Command                  | Description                              |
| ------------------- | ------------------------ | ---------------------------------------- |
| **Start**           | `npm start`              | Run in production mode                   |
| **Dev**             | `npm run dev`            | Run with Nodemon (auto-restart)          |
| **Start (Prod)**    | `npm run start:prod`     | Run with `NODE_ENV=production`           |
| **Debug**           | `npm run debug`          | Launch with `ndb` debugger               |
| **Watch JS**        | `npm run watch:js`       | Parcel — watch & bundle client JS        |
| **Build JS**        | `npm run build:js`       | Parcel — production build of client JS   |

---

## 🔑 Key Concepts & Patterns

- **MVC Architecture** — Clean separation of Models, Views (Pug), and Controllers
- **Factory Functions** — `handlerFactory.js` provides reusable `getAll`, `getOne`, `createOne`, `updateOne`, and `deleteOne` handlers
- **API Features Class** — Chainable filtering, sorting, field limiting, and pagination
- **Mongoose Middleware** — Document and query middleware for slugs, password hashing, and query filtering
- **Virtual Populate** — Tour reviews populated without persisting on the tour document
- **Geospatial Indexing** — `2dsphere` index on tour start locations
- **Centralized Error Handling** — Global error middleware with operational vs. programming error distinction

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **ISC License**.

---

## 🙏 Acknowledgments

- Built as part of the [Node.js, Express, MongoDB & More](https://www.udemy.com/course/nodejs-express-mongodb-bootcamp/) course by Jonas Schmedtmann
- [Stripe](https://stripe.com/) for payment processing
- [MongoDB Atlas](https://www.mongodb.com/atlas) for cloud database hosting
- [Mailtrap](https://mailtrap.io/) for development email testing
