
# Product Stock Checker

A web application that automates stock monitoring for bol.com products, tracks sales history, and provides a dashboard for analysis.

## Overview

This application allows you to:

- Track stock levels for multiple bol.com product URLs
- Automatically calculate sales based on stock changes
- View historical sales data for 30/60/90 day periods
- Schedule automatic stock checks at different times

## Features

- **Stock Monitoring**: Automated browser-based stock checking using Puppeteer
- **Sales Tracking**: Calculates sales by comparing stock changes between checks
- **Scheduling System**: Automatically checks products at scheduled times throughout the day
- **User Interface**: Clean, responsive dashboard to manage products and view sales data
- **Docker Containerization**: Easy deployment with Docker

## Technology Stack

- **Frontend**: HTML, CSS, JavaScript (vanilla)
- **Backend**: Node.js, Express
- **Database**: PostgreSQL
- **Web Scraping**: Puppeteer
- **Logging**: Winston
- **Containerization**: Docker
- **CI/CD**: GitHub Actions

## Screenshots

- Product Management Dashboard
- Sales Analytics View

## Requirements

- Docker and Docker Compose
- Node.js 18+ (for development)
- PostgreSQL 14+ (handled by Docker for production)

## Installation & Setup

### Using Docker (Recommended)

1. Clone the repository:
   ```bash
   git clone https://github.com/FelixClements/product_stock_checker_js.git
   cd product_stock_checker_js
   ```

2. Configure environment variables in `docker-compose.yml` if needed:
   ```yaml
   environment:
     - NODE_ENV=production
     - DB_HOST=db
     - DB_PORT=5432
     - DB_NAME=stockchecker
     - DB_USER=postgres
     - DB_PASSWORD=yourpassword  # Change this
     - API_PORT=3000
   ```

3. Start the application:
   ```bash
   docker-compose up -d
   ```

4. Access the application at http://localhost:3000

### For Development

1. Clone the repository
2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file:
   ```
   NODE_ENV=development
   DB_HOST=localhost
   DB_PORT=5432
   DB_NAME=stockchecker
   DB_USER=postgres
   DB_PASSWORD=yourpassword
   API_PORT=3000
   ```

4. Run PostgreSQL (via Docker or locally)
5. Start the application:
   ```bash
   npm start
   ```
   
6. For debugging:
   ```bash
   npm run debug
   ```

## Usage

1. **Adding Products**: Enter a bol.com product URL to begin tracking
2. **Viewing Stock Status**: See current stock status and when it was last checked
3. **Checking Stock Manually**: Use the "Run Stock Check" button to check immediately
4. **Viewing Sales Data**: Navigate to the Sales Dashboard to see historical sales data
   
## How It Works

1. **Stock Checking**: 
   - The application uses Puppeteer to simulate a browser session
   - It navigates to the product page, adds the maximum quantity to cart
   - The system then extracts the available stock from the cart page

2. **Sales Calculation**:
   - Whenever stock decreases between checks, it's recorded as a sale
   - Sales are calculated and stored for different time periods (30/60/90 days)

3. **Scheduling**:
   - Each product is assigned a random check time between 8 AM and 11 PM
   - The scheduler runs checks for products at their designated times

## Database Schema

- **products**: Stores product URLs and scheduling information
- **products_history**: Records stock check history
- **products_sales**: Stores calculated sales data for different periods

## Docker Configuration

The application is containerized with Docker and includes:

- Node.js application container
- PostgreSQL database container
- Volume for persistent data storage

## CI/CD Pipeline

A GitHub Actions workflow is configured to:
1. Build the Docker image
2. Push to GitHub Container Registry
3. Tag the image appropriately

## Customization

- Change the default port by setting the `API_PORT` environment variable
- Modify stock check scheduling by editing the `scheduler.js` file
- Customize the stock checking process in `stockChecker.js`

## Troubleshooting

- **Stock Check Failures**: Check the logs for screenshot errors
- **Database Connection Issues**: Verify PostgreSQL connection settings
- **Scheduling Problems**: Ensure system time is correct and logs are showing scheduler activity

## Security Notes

- Use a strong database password in production
- Consider using Docker secrets for sensitive information
- The application should be deployed behind a reverse proxy for HTTPS

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

ISC License

## Author

FelixClements
