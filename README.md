# Product Stock Checker

A Node.js application that automatically tracks and monitors product stock availability on bol.com. This service uses Puppeteer for web scraping, Express for the API backend, and PostgreSQL for data storage, all containerized with Docker.

## Features

- **Automated Stock Checking**: Monitors product stock levels from bol.com at scheduled intervals
- **Web Interface**: Easy-to-use UI to add and manage product URLs
- **Scheduling System**: Checks products at randomized times between 8AM and 11PM to avoid detection
- **Stock History**: Tracks historical stock levels for each product
- **Docker Containerization**: Easy deployment using Docker Compose
- **CI/CD Pipeline**: Automated builds and container publishing to GitHub Container Registry

## Architecture

The application consists of:

- **Web UI**: Simple interface to add/remove products and view status
- **Express API**: RESTful API for managing products
- **Scheduler**: Manages when products are checked based on configured schedules
- **Stock Checker**: Uses Puppeteer to automate browser interactions and retrieve stock information
- **PostgreSQL Database**: Stores product information and stock history

## Installation

### Prerequisites

- Docker and Docker Compose installed
- GitHub Container Registry access

### Using Docker Compose

1. Clone this repository:
   ```
   git clone https://github.com/felixclements/product_stock_checker_js.git
   cd product_stock_checker_js
   ```

2. Create an `.env` file with your configuration (optional):
   ```
   NODE_ENV=production
   DB_HOST=db
   DB_PORT=5432
   DB_NAME=stockchecker
   DB_USER=postgres
   DB_PASSWORD=yourpassword
   API_PORT=3000
   ```

3. Run the application using Docker Compose:
   ```
   docker-compose up -d
   ```

4. Access the web interface at: http://server_ip:3000

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| NODE_ENV | Environment (production/development) | production |
| DB_HOST | PostgreSQL host | db |
| DB_PORT | PostgreSQL port | 5432 |
| DB_NAME | Database name | stockchecker |
| DB_USER | Database username | postgres |
| DB_PASSWORD | Database password | yourpassword |
| API_PORT | Port for the API server | 3000 |

## Usage

### Adding Products

1. Open the web interface at http://server_ip:3000
2. Enter a bol.com product URL in the input field
3. Click "Add URL" to begin tracking

The application will automatically:
- Schedule the product for stock checking at a random time between 8AM and 11PM
- Check the product's stock level daily at the scheduled time
- Store historical stock data

### Development Mode

In development mode, the application will:
- Check all products immediately on startup
- Skip scheduling functionality
- Save detailed logs and screenshots to the `logs` directory

To run in development mode, set `NODE_ENV=development` in your environment variables.

## CI/CD Workflow

This project uses GitHub Actions to:
1. Build the Docker image on push to main branch or pull requests
2. Push the image to GitHub Container Registry when merged to main
3. Tag the image with the branch name, PR number, semantic version, or commit SHA

## Project Structure

```
├── .github/workflows    # GitHub Actions workflows
├── src                  # Application source code
│   ├── api              # Express API endpoints
│   ├── config           # Database configuration
│   ├── constants        # Application constants
│   ├── models           # Database models
│   ├── services         # Business logic services
│   └── utils            # Utility functions
├── public               # Static web interface files
├── Dockerfile           # Docker container definition
├── docker-compose.yml   # Docker Compose configuration
└── package.json         # Node.js dependencies
```

## Development

### Building Locally

```bash
# Install dependencies
npm install

# Run locally
npm start

# Run with debugging
npm run debug
```

### Database Schema

The application uses two main tables:
- `products`: Stores product URLs and scheduling information
- `products_history`: Stores historical stock levels

## Troubleshooting

### Common Issues

- **Screenshots**: Error screenshots are saved to `logs/screenshots` for debugging
- **Database Connection**: Ensure PostgreSQL is running and accessible
- **bol.com Changes**: The website structure may change, requiring updates to selectors
