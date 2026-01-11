# Monorepo Docker CI/CD

A full-stack monorepo application with Docker support and CI/CD pipelines. This project uses Turborepo for managing multiple applications including a Next.js frontend, Express backend, and WebSocket server, all connected to a PostgreSQL database via Prisma.

## Tech Stack

- **Monorepo Management**: [Turborepo](https://turborepo.com/)
- **Runtime**: [Bun](https://bun.sh/)
- **Frontend**: [Next.js](https://nextjs.org/) with React 19
- **Backend**: [Express.js](https://expressjs.com/) running on Bun
- **WebSocket**: Bun native WebSocket server
- **Database**: [PostgreSQL](https://www.postgresql.org/) with [Prisma](https://www.prisma.io/) ORM
- **Containerization**: [Docker](https://www.docker.com/) & Docker Compose
- **Language**: [TypeScript](https://www.typescriptlang.org/)
- **Code Quality**: [ESLint](https://eslint.org/) & [Prettier](https://prettier.io)

## Project Structure

```
.
├── apps/
│   ├── backend/          # Express.js API server (port 8080)
│   ├── web/              # Next.js frontend application (port 3000)
│   └── websocket/        # Bun WebSocket server (port 8081)
├── packages/
│   ├── db/               # Prisma database client and schema
│   ├── ui/               # Shared React component library (@repo/ui)
│   ├── eslint-config/    # Shared ESLint configurations (@repo/eslint-config)
│   └── typescript-config/ # Shared TypeScript configurations (@repo/typescript-config)
├── docker/
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── Dockerfile.websocket
└── docker-compose.yml
```

## Prerequisites

- [Node.js](https://nodejs.org/) >= 18
- [Bun](https://bun.sh/) >= 1.2.19
- [Docker](https://www.docker.com/) & Docker Compose (for containerized deployment)
- [PostgreSQL](https://www.postgresql.org/) (if running locally without Docker)

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/sayantann7/monorepo-docker-cicd-repo.git
   cd monorepo-docker-cicd-repo
   ```

2. Install dependencies:
   ```bash
   bun install
   ```

3. Generate Prisma client:
   ```bash
   bun run generate:db
   ```

## Environment Variables

Create a `.env` file in the root directory (or in `packages/db`) with the following variables:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/postgres
```

## Running the Project

### Development Mode

Run all applications in development mode:

```bash
bun run dev
```

Run a specific application:

```bash
# Frontend only
bun run dev --filter=web

# Backend only
bun run dev --filter=backend

# WebSocket server only
bun run dev --filter=websocket
```

### Production Mode

Start individual services:

```bash
# Start backend server
bun run start:backend

# Start WebSocket server
bun run start:ws

# Start frontend
bun run start:frontend
```

### Using Docker Compose

Run the entire stack with Docker Compose:

```bash
docker-compose up --build
```

This will start:
- **PostgreSQL** database on port `5432`
- **Backend** API server on port `8080`
- **Frontend** Next.js app on port `3000`
- **WebSocket** server on port `8081`

To run in detached mode:

```bash
docker-compose up -d --build
```

To stop all services:

```bash
docker-compose down
```

## Build

Build all applications and packages:

```bash
bun run build
```

Build a specific application:

```bash
bun run build --filter=web
bun run build --filter=backend
```

## Code Quality

### Linting

Run ESLint across all packages:

```bash
bun run lint
```

### Type Checking

Run TypeScript type checking:

```bash
bun run check-types
```

### Formatting

Format code with Prettier:

```bash
bun run format
```

## API Endpoints

### Backend (port 8080)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | `/users` | Get all users |
| POST   | `/user`  | Create a new user (requires `username` and `password` in body) |

### WebSocket (port 8081)

Connect to `ws://localhost:8081` for WebSocket communication. Messages sent to the server will be echoed back.

## Database Schema

The application uses a simple User model:

```prisma
model User {
  id       String @id @default(uuid())
  username String
  password String
}
```

## CI/CD

This project includes GitHub Actions workflows for continuous deployment:

- **Backend Deployment** (`.github/workflows/cd_backend.yml`): Builds and deploys the backend Docker image on push to `main`
- **WebSocket Deployment** (`.github/workflows/cd_ws.yml`): Builds and deploys the WebSocket server Docker image on push to `main`

### Required Secrets

Configure the following secrets in your GitHub repository:

- `DOCKERHUB_USERNAME`: Docker Hub username
- `DOCKERHUB_TOKEN`: Docker Hub access token
- `SSH_PRIVATE_KEY`: SSH private key for deployment server access

## Useful Links

- [Turborepo Documentation](https://turborepo.com/docs)
- [Bun Documentation](https://bun.sh/docs)
- [Next.js Documentation](https://nextjs.org/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Docker Documentation](https://docs.docker.com/)
