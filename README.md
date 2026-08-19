# api



## Getting Started

This is a Node.js Express backend service bootstrapped for the Stelm MVP platform.

### Development

```bash
npm install
npm run dev
```

The server will run on [http://localhost:3000](http://localhost:3000).

### Production

```bash
npm install
npm start
```

### Docker

```bash
docker build -t tredex/prod-smoke-api-backend-matrix-1787179245-production-api .
docker run -p 3000:3000 tredex/prod-smoke-api-backend-matrix-1787179245-production-api
```

## API Endpoints

- `GET /health` - Health check endpoint
- `GET /api` - API information
- `GET /api/data` - Example data endpoint

## Project Structure

```
src/
  └── index.js        # Main application entry point
.env.example          # Environment variables template
Dockerfile            # Docker configuration
package.json          # Node.js dependencies
```

## Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

For production on Stelm, use the **Environment** tab for secrets. When connected to a Database
component, `DATABASE_URL` is injected automatically.

## Database and migrations

Stelm provisions an empty RDS database. Add schema in this repo — for example `scripts/migrate.js`:

```javascript
const { Client } = require('pg');

async function main() {
  const client = new Client({ connectionString: process.env.DATABASE_URL });
  await client.connect();
  await client.query(`
    CREATE TABLE IF NOT EXISTS users (
      id SERIAL PRIMARY KEY,
      email TEXT NOT NULL UNIQUE
    );
  `);
  await client.end();
}

main().catch((err) => {
  console.error(err);
  process.exit(1);
});
```

Optional CI step you maintain (not added by Stelm by default):

```yaml
- name: Run migrations
  env:
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
  run: node scripts/migrate.js
```

Guide: https://app.stelm.dev/app/docs/database-migrations

## Learn More

- [Express Documentation](https://expressjs.com/)
- [Node.js Documentation](https://nodejs.org/)
