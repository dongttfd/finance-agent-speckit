# Quick Start Guide: Finance Agent App Development Environment

## Prerequisites

Before setting up the development environment, ensure you have the following installed:

- Node.js v20 or higher
- pnpm package manager (v9 or higher)
- Docker and Docker Compose
- Git version control system
- A code editor (VS Code recommended)

## Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd speckit-driver
```

### 2. Navigate to the Project Directory

The main application code resides in the `finance-agent-app` directory:

```bash
cd finance-agent-app
```

### 3. Install Dependencies

Each component has its own package management. Navigate to each directory and install dependencies separately:

#### Backend Dependencies
```bash
cd backend
pnpm install
```

#### Frontend Dependencies
```bash
cd frontend
pnpm install
```

Each component maintains its own package.json, node_modules, and configuration files.

### 4. Set Up Environment Variables

Create a `.env` file in the project root with the following variables:

```env
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://user:password@localhost:5432/finance_agent_db
FRONTEND_PORT=3001
DOCKER_REGISTRY=
```

### 5. Start Development Environment with Docker

To start all services using Docker Compose:

```bash
docker-compose up
```

This will start the backend, frontend, and database services in containers.

### 6. Alternative: Run Services Separately

If you prefer to run services separately for development:

#### Start Backend Server

```bash
cd backend
pnpm start:dev
```

#### Start Frontend Server

```bash
cd frontend
pnpm dev
```

## Available Scripts

### Backend Scripts

- `pnpm start`: Start the production server
- `pnpm start:dev`: Start the development server with hot reloading
- `pnpm test`: Run unit tests
- `pnpm test:e2e`: Run end-to-end tests
- `pnpm lint`: Check code for linting errors
- `pnpm format`: Format code according to project standards

### Frontend Scripts

- `pnpm dev`: Start the development server
- `pnpm build`: Build the production bundle
- `pnpm preview`: Preview the production build locally
- `pnpm test`: Run unit tests
- `pnpm test:e2e`: Run end-to-end tests
- `pnpm lint`: Check code for linting errors
- `pnpm format`: Format code according to project standards

## Development Workflow

### Code Quality

The project enforces code quality standards using ESLint and Prettier:

```bash
pnpm lint  # Check for linting issues
pnpm format  # Automatically format code
```

All code must pass these checks before being committed.

### Testing

Maintain high test coverage (80%+) for all critical functionality:

```bash
pnpm test  # Run all unit tests
```

Write tests for all business logic and services as per the project constitution.

### Security Compliance

For financial-grade security compliance:

- All data transmission must be encrypted (HTTPS/TLS)
- Database connections must use SSL
- Implement proper authentication and authorization
- Enable audit logging for all financial operations

## Project Structure

```
finance-agent-app/
├── backend/
│   ├── src/
│   │   ├── models/         # Backend-specific data models
│   │   ├── services/       # Business logic
│   │   └── api/            # API endpoints
│   ├── tests/
│   ├── package.json        # Backend-specific dependencies and scripts
│   ├── pnpm-lock.yaml      # Backend-specific locked dependency versions
│   ├── tsconfig.json       # Backend-specific TypeScript configuration
│   ├── .eslintrc.js        # Backend-specific ESLint configuration
│   ├── .prettierrc         # Backend-specific Prettier configuration
│   └── README.md
├── frontend/
│   ├── src/
│   │   ├── components/     # UI components
│   │   ├── pages/          # Page components
│   │   └── services/       # API service clients
│   ├── tests/
│   ├── package.json        # Frontend-specific dependencies and scripts
│   ├── pnpm-lock.yaml      # Frontend-specific locked dependency versions
│   ├── tsconfig.json       # Frontend-specific TypeScript configuration
│   ├── .eslintrc.js        # Frontend-specific ESLint configuration
│   ├── .prettierrc         # Frontend-specific Prettier configuration
│   └── README.md
├── docker/
│   ├── docker-compose.yml  # Service orchestration
│   └── Dockerfile          # Container image definition
├── desktop/                # Prepared for future milestone
├── docs/                   # Documentation
└── scripts/                # Shared utility scripts if needed
```

## Troubleshooting

### Common Issues

1. **Port Already in Use**: Make sure ports 3000 (backend) and 3001 (frontend) are available.

2. **Docker Permission Errors**: Ensure your user has permission to run Docker commands.

3. **Dependency Installation Failures**: Try clearing the pnpm cache (`pnpm store prune`) and reinstalling.

4. **Database Connection Issues**: Verify that the database service is running and credentials are correct.

### Getting Help

- Check the documentation in the `docs/` directory
- Ask questions in the team communication channel
- Review the project constitution for architectural guidelines

## Next Steps

1. Review the API contracts in the `contracts/` directory
2. Explore the data models in `backend/src/models/`
3. Familiarize yourself with the frontend components in `frontend/src/components/`
4. Look at the existing tests to understand the testing patterns