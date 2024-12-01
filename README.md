
# Bring4Me Platform Documentation

## Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [User Flow](#user-flow)
4. [Technologies](#technologies)
5. [Environments](#environments)
6. [Database Schema](#database-schema)
7. [API Endpoints](#api-endpoints)
8. [Setup and Installation](#setup-and-installation)
9. [Deployment Recommendations](#deployment-recommendations)
10. [Basic Authentication for E2E Testing](#basic-authentication-for-e2e-testing)

---

## Overview

**Bring4Me** is a community-based web application that connects individuals who need to send packages with travelers willing to transport them. It provides a secure, cost-effective, and sustainable way to send items by leveraging travelers' available luggage space. The platform caters primarily to the France-Tunisia corridor, enabling users to arrange transportation for goods.

---

## Features

1. **User Authentication**: Secure registration and login system.
2. **User Profiles**: Both senders and travelers can create detailed profiles, including contact information and ratings.
3. **Package Requests**: Senders can create requests to send items, detailing the item, destination, and timeframe.
4. **Trip Offers**: Travelers can post available trips with details on space availability, destinations, and dates.
5. **Matching System**: The app automatically matches senders with travelers based on routes and availability.
6. **In-App Messaging**: Secure, real-time messaging system to facilitate communication between senders and travelers.
7. **Transaction Management**: Securely handle payments, including escrow and release upon delivery confirmation.
8. **Notifications**: Users receive notifications for new matches, messages, and status updates.
9. **Review and Rating System**: Senders and travelers can rate each other after a transaction is completed.
10. **Admin Dashboard**: For managing user activities, transactions, and handling disputes.

---

## User Flow

1. **Registration/Login**: Users create an account and set up their profile.
2. **Requesting a Package Transfer**: A sender posts a package transfer request.
3. **Offering a Trip**: A traveler lists a trip they will take, specifying available space.
4. **Matching**: The system matches package requests with available trips.
5. **Messaging**: Matched users communicate via in-app messaging.
6. **Payment Processing**: Sender pays for the transport, and funds are held until confirmation.
7. **Package Handover**: Sender and traveler arrange the exchange.
8. **Completion and Review**: Sender confirms receipt, and users rate each other.

---

## Technologies

- **Frontend**: React, CSS/SCSS, and component libraries like Material UI for responsive design.
- **Backend**: Node.js with Express, PostgreSQL for the database.
- **Real-Time Messaging**: WebSockets (Socket.IO).
- **Authentication**: JSON Web Tokens (JWT) with bcrypt for password hashing.
- **Payments**: Stripe API integration for secure transactions.
- **Cloud Provider**: AWS or Google Cloud Platform for hosting, storage, and scalability.

---

## Environments

### 1. Development
- Used for local development and feature testing.
- Database: Local instance of PostgreSQL.
- Cloud/Hosting: Local server or cloud development environment.
- Environment Variables: Development-specific settings (e.g., `DEV_API_URL`).

### 2. Testing
- Isolated environment for running tests.
- Supports Basic Authentication for automated testing (see section on Basic Authentication).
- Database: Separate testing instance of PostgreSQL.
- Environment Variables: Testing-specific settings (e.g., `TEST_API_URL`, `TEST_BASIC_AUTH`).

### 3. Production
- Live environment accessible to end-users.
- Database: Cloud-hosted PostgreSQL instance.
- Environment Variables: Production-specific settings (e.g., `PROD_API_URL`).
- Security: HTTPS, data encryption, production API keys.

---

## Database Schema

| Table          | Fields                                                        |
|----------------|---------------------------------------------------------------|
| Users          | id, name, email, password_hash, role, profile_picture, bio, ratings, created_at, updated_at |
| Packages       | id, user_id (sender), title, description, weight, destination, origin, status, created_at, updated_at |
| Trips          | id, user_id (traveler), destination, origin, available_space, departure_date, return_date, status, created_at, updated_at |
| Messages       | id, sender_id, receiver_id, message_body, sent_at, read_status |
| Transactions   | id, sender_id, traveler_id, package_id, amount, status, created_at, updated_at |
| Ratings        | id, from_user_id, to_user_id, rating_score, comments, created_at |
| Notifications  | id, user_id, message, status, created_at |
| Admins         | id, name, email, password_hash, role, created_at, updated_at |

---

## API Endpoints

### Authentication
- `POST /api/auth/register`: Register a new user.
- `POST /api/auth/login`: Login and receive an authentication token.

### User Profile
- `GET /api/users/:id`: Get user profile details.
- `PUT /api/users/:id`: Update profile information.

### Packages
- `POST /api/packages`: Create a new package request.
- `GET /api/packages`: List all package requests.
- `GET /api/packages/:id`: Get details of a package.

### Trips
- `POST /api/trips`: Create a new trip offer.
- `GET /api/trips`: List all trip offers.
- `GET /api/trips/:id`: Get details of a specific trip.

### Transactions
- `POST /api/transactions`: Create a new transaction.
- `GET /api/transactions/:id`: Get details of a transaction.

### Messages
- `POST /api/messages`: Send a message.
- `GET /api/messages/:user_id`: Retrieve messages for a user.

---

## Setup and Installation

### Prerequisites
- **Node.js** (version 14 or higher)
- **PostgreSQL**
- **Stripe account** (for payment processing)

### Installation Steps
1. Clone the repository.
2. Run `npm install` to install dependencies.
3. Configure environment variables for development, testing, and production:
    - `.env.development`
    - `.env.test`
    - `.env.production`
4. Run database migrations:
    ```bash
    npx sequelize-cli db:migrate
    ```
5. Start the development server:
    ```bash
    npm run dev
    ```

---

## Deployment Recommendations

- **Cloud Hosting**: AWS, Google Cloud, or DigitalOcean.
- **Containerization**: Docker for consistent deployment.
- **SSL Certificate**: HTTPS for production.
- **CI/CD Pipeline**: Use GitHub Actions or CircleCI for automated testing and deployment.
- **Monitoring and Logging**: Implement monitoring (Datadog, New Relic) and logging (Winston, LogDNA).

---

## Basic Authentication for E2E Testing

To support controlled access during E2E tests, Basic Authentication can be enabled in the **test environment**. This provides an additional layer of security for testing purposes.

### Enabling Basic Authentication

In the `.env.test` file, add the following credentials for the test environment:
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=bring4me
DB_USER=postgres
DB_PASSWORD=postgres
JWT_SECRET=your-secret-key
STRIPE_KEY=your-stripe-key

Bring4MeV0/
├── .gitignore
├── package.json
├── bring4me-backend/
│   ├── src/
│   │   ├── config/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── services/
│   │   └── server.js
│   ├── .env.example
│   └── package.json
└── bring4me-frontend/
    └── (frontend files)