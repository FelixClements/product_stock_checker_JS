# Stock Checker

A Node.js application that automates checking product stock levels on bol.com. This service uses Puppeteer to navigate through product pages, add items to cart, and determine maximum available stock quantities.

## Features

- Automated stock checking for bol.com products
- Scheduled checks distributed throughout the day
- Stock history tracking in PostgreSQL database
- Docker containerization for easy deployment
- GitHub Actions workflow for CI/CD
- Error handling with screenshots for debugging

## Technologies Used

- Node.js 18
- Puppeteer for web automation
- PostgreSQL for data storage
- Docker and Docker Compose for containerization
- Winston for logging
- GitHub Actions for CI/CD

## Installation and Setup

### Prerequisites

- Node.js 18+
- Docker and Docker Compose
- PostgreSQL (if not using Docker)

### Local Development Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/stock-checker.git
   cd stock-checker
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory with the following variables:
   ```
   NODE_ENV=development
   DB_HOST=localhost
   DB_PORT=5432
   DB_NAME=stockchecker
   DB_USER=postgres
   DB_PASSWORD=yourpassword
   ```

4. Start the application:
   ```bash
   npm start
   ```

### Using Docker Compose

To run the application with Docker Compose:

```bash
docker-compose up -d
```

This will start both the application and PostgreSQL database containers.

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| NODE_ENV | Environment (development/production) | development |
| DB_HOST | PostgreSQL host | db |
| DB_PORT | PostgreSQL port | 5432 |
| DB_NAME | PostgreSQL database name | stockchecker |
| DB_USER | PostgreSQL username | postgres |
| DB_PASSWORD | PostgreSQL password | yourpassword |

## Database Schema

The application uses the following tables:

### products
- `id`: Primary key
- `url`: URL of the product to check
- `scheduled_time`: Time of day to check the product
- `last_checked`: Timestamp of the last check
- `checked_today`: Boolean flag for daily checks

### products_history
- `id`: Primary key
- `product_id`: Foreign key to products
- `stock`: Stock quantity found
- `checked_at`: Timestamp of the check

## How It Works

1. The application schedules product checks throughout the day (between 8 AM and 11 PM)
2. For each scheduled check, the app:
   - Opens a headless browser
   - Navigates to the product page
   - Handles cookie consent and location modals
   - Adds the product to cart
   - Navigates to the cart
   - Attempts to set the quantity to 500
   - Retrieves the maximum available quantity
   - Records this value in the database

3. In development mode, all pending products are checked immediately
4. In production mode, checks are distributed throughout the day based on scheduled times

## Development vs Production Mode

- **Development Mode**: All pending products are checked immediately on startup
- **Production Mode**: Products are checked based on their scheduled time throughout the day

## Project Structure

```
.
├── .github/workflows     # GitHub Actions workflows 
├── src/                  # Source code
│   ├── config/           # Database configuration
│   ├── constants/        # Application constants
│   ├── models/           # Database models
│   ├── services/         # Business logic
│   ├── utils/            # Utility functions
│   └── app.js            # Main application entry point
├── logs/                 # Application logs and screenshots
├── .env                  # Environment variables (not in repo)
├── docker-compose.yml    # Docker Compose definition
├── Dockerfile            # Docker build instructions
└── package.json          # NPM package definition
```

## Docker Image

The application is containerized with Docker. The container includes:

- Node.js 18 (slim version)
- Chromium for headless browser automation
- Application code and dependencies

The Docker image is built and published to GitHub Container Registry on each push to main.

To use the published image:

```bash
docker pull ghcr.io/yourusername/stock-checker:latest
```

## CI/CD Pipeline

This repository includes a GitHub Actions workflow that:

1. Builds the Docker image
2. Pushes it to GitHub Container Registry
3. Tags the image with branch name, PR number, or commit SHA

## Troubleshooting

Screenshots of errors during the stock checking process are saved to `logs/screenshots/` to help with debugging.

## License

ISC License

---

**Note:** Before deploying to production, make sure to change default database credentials in `docker-compose.yml` and configure appropriate environment variables.