# Problem 5 — CRUD API (Express + TypeScript + Prisma + MySQL)

A RESTful CRUD API for managing **Resources**, built with Express.js, TypeScript, Prisma ORM, and MySQL.

---

## Prerequisites

- Node.js 18+
- npm 9+
- Docker & Docker Compose (for containerised setup)

---

## Local Development (without Docker)

### 1. Install dependencies

```bash
npm install
```

### 2. Configure environment variables

```bash
cp .env.example .env
# Edit .env with your local MySQL connection string
```

### 3. Apply database migrations

```bash
npx prisma migrate dev --name init
```

### 4. Run in dev mode (with hot-reload)

```bash
npm run dev
```

The server starts on `http://localhost:3000` (or the `PORT` you set in `.env`).

---

## Docker Setup (recommended)

Builds the app and starts both MySQL and the API in containers.  
Migrations are applied automatically on container startup.

```bash
docker compose up --build
```

The API will be available at `http://localhost:3000`.  
MySQL is accessible at `localhost:3306`.

To stop:

```bash
docker compose down
```

To stop and remove volumes (wipes DB data):

```bash
docker compose down -v
```

---

## Build & Production

```bash
npm run build      # Compile TypeScript → dist/
npm start          # Run compiled output
```

---

## API Endpoints

Base URL: `http://localhost:3000`

### Health Check

| Method | Path    | Description                |
|--------|---------|----------------------------|
| GET    | /health | Returns `{ status: "ok" }` |

### Resources

| Method | Path           | Description                           |
|--------|----------------|---------------------------------------|
| POST   | /resources     | Create a new resource                 |
| GET    | /resources     | List all resources (supports filters) |
| GET    | /resources/:id | Get a single resource by UUID         |
| PUT    | /resources/:id | Update a resource by UUID             |
| DELETE | /resources/:id | Delete a resource by UUID             |

### Query Parameters for `GET /resources`

| Param    | Type   | Description                        |
|----------|--------|------------------------------------|
| name     | string | Filter by name (partial match)     |
| category | string | Filter by category (partial match) |

### Request Body (POST / PUT)

```json
{
  "name": "My Resource",
  "description": "An optional description",
  "category": "example"
}
```

`name` and `category` are required on **POST**.  
All fields are optional on **PUT** (partial updates supported).

### Example Requests

```bash
# Create
curl -X POST http://localhost:3000/resources \
  -H "Content-Type: application/json" \
  -d '{"name":"Widget","description":"A useful widget","category":"tools"}'

# List all
curl http://localhost:3000/resources

# Filter by category
curl "http://localhost:3000/resources?category=tools"

# Get by ID
curl http://localhost:3000/resources/<uuid>

# Update
curl -X PUT http://localhost:3000/resources/<uuid> \
  -H "Content-Type: application/json" \
  -d '{"description":"Updated description"}'

# Delete
curl -X DELETE http://localhost:3000/resources/<uuid>
```

---

## Prisma Migrations

```bash
# Create and apply a new migration (dev)
npx prisma migrate dev --name <migration-name>

# Apply migrations in production
npx prisma migrate deploy

# Push schema changes without migrations (prototyping)
npx prisma db push

# Open Prisma Studio (GUI for browsing data)
npm run prisma:studio
```

---

## Environment Variables

| Variable     | Default                                      | Description                  |
|--------------|----------------------------------------------|------------------------------|
| PORT         | 3000                                         | HTTP port                    |
| DATABASE_URL | mysql://user:password@localhost:3306/crud_db | Prisma MySQL connection URL  |
| NODE_ENV     | development                                  | Node environment             |
