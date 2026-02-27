# FlySmart – Backend

Smart credit card recommendation engine for flight bookings.

## Tech Stack

- **Language:** Go (Golang)
- **Framework:** Gin
- **Database:** PostgreSQL
- **ORM:** GORM
- **Auth:** JWT + bcrypt
- **Deployment:** AWS EC2 + RDS + Nginx

## Getting Started

### Prerequisites

- Go 1.21+
- PostgreSQL
- Git

### Installation

```bash
git clone https://github.com/your-username/flysmart-backend.git
cd flysmart-backend
go mod tidy
```

### Environment Variables

Create a `.env` file in the root directory:

```env
DB_HOST=localhost
DB_PORT=5432
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_NAME=flysmart
JWT_SECRET=your_jwt_secret
PORT=8080
```

### Run Locally

```bash
go run main.go
```

The server starts at `http://localhost:8080`.

## Project Structure

```
flysmart-backend/
├── main.go
├── config/         # DB connection, env config
├── controllers/    # Route handlers
├── middleware/     # JWT auth, CORS
├── models/         # GORM models
├── routes/         # API route definitions
├── services/       # Recommendation engine logic
└── utils/          # Helper functions
```

## Core Data Models

- `User` — registered users
- `CreditCard` — up to 4 cards per user
- `CardOffer` — offers tied to cards, airlines, and platforms
- `FlightBooking` — booking details (amount, airline, platform)
- `RewardCalculation` — computed savings and reward points

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login and get JWT token |
| GET | `/api/cards` | List user's credit cards |
| POST | `/api/cards` | Add a credit card |
| DELETE | `/api/cards/:id` | Remove a credit card |
| POST | `/api/recommend` | Get best card recommendation for a flight |

## Recommendation Engine

The core engine works in three steps:

1. **Eligibility filtering** — filters cards applicable to the given airline and booking platform
2. **Offer matching** — matches active offers against booking details
3. **Savings maximization** — ranks cards by total benefit (discount + reward points + cashback)

## Deployment

The app is deployed on AWS using EC2 + RDS (PostgreSQL) + Nginx as a reverse proxy.

```bash
# Build for production
go build -o flysmart-server main.go

# Run with production env
./flysmart-server
```
