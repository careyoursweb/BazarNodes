# BazarNodes - Hosting Services Platform

## Overview

BazarNodes is a web-based hosting services platform that sells Minecraft server hosting and VPS (Virtual Private Server) hosting. The application features a modern React frontend with a dark neon-themed UI, an Express.js backend API, PostgreSQL database, and Replit's OpenID Connect authentication system. Users can browse products, place orders, and create support tickets through a client dashboard.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript, using Vite as the build tool
- **Routing**: Wouter for client-side routing (lightweight alternative to React Router)
- **State Management**: TanStack Query (React Query) for server state and caching
- **Styling**: Tailwind CSS with shadcn/ui component library (New York style), featuring a custom dark theme with neon purple/cyan accents
- **Animations**: Framer Motion for page transitions and UI animations
- **UI Components**: Radix UI primitives wrapped with shadcn/ui styling conventions

### Backend Architecture
- **Runtime**: Node.js with Express.js
- **Language**: TypeScript with ESM modules
- **API Pattern**: RESTful API with typed route definitions in `shared/routes.ts`
- **Validation**: Zod schemas for request/response validation, integrated with Drizzle via `drizzle-zod`

### Data Storage
- **Database**: PostgreSQL with Drizzle ORM
- **Schema Location**: `shared/schema.ts` contains all table definitions
- **Key Tables**: 
  - `users` and `sessions` (authentication - mandatory for Replit Auth)
  - `products` (hosting plans)
  - `orders` (user purchases)
  - `tickets` and `ticket_messages` (support system)
- **Migrations**: Drizzle Kit with `db:push` command for schema synchronization

### Authentication System
- **Provider**: Replit OpenID Connect (OIDC) authentication
- **Session Storage**: PostgreSQL-backed sessions using `connect-pg-simple`
- **Implementation**: Passport.js with OpenID Client strategy
- **Protected Routes**: Middleware pattern with `isAuthenticated` function
- **Key Files**: `server/replit_integrations/auth/` directory contains all auth logic

### Build System
- **Development**: Vite dev server with HMR, proxied through Express
- **Production**: 
  - Client: Vite builds to `dist/public`
  - Server: esbuild bundles to `dist/index.cjs` with selective dependency bundling for faster cold starts

### Project Structure
```
client/           # React frontend application
  src/
    components/   # Reusable UI components
    pages/        # Route page components
    hooks/        # Custom React hooks for API calls
    lib/          # Utilities and query client config
server/           # Express backend
  replit_integrations/auth/  # Replit Auth implementation
shared/           # Shared types, schemas, and route definitions
  models/         # Database model definitions
  schema.ts       # Drizzle table schemas
  routes.ts       # API route contracts with Zod validation
```

## External Dependencies

### Database
- **PostgreSQL**: Primary database, connection via `DATABASE_URL` environment variable
- **Drizzle ORM**: Type-safe database queries and schema management

### Authentication
- **Replit OIDC**: OpenID Connect provider for user authentication
- **Required Environment Variables**: 
  - `ISSUER_URL` (defaults to `https://replit.com/oidc`)
  - `REPL_ID` (automatically provided by Replit)
  - `SESSION_SECRET` (for session encryption)
  - `DATABASE_URL` (PostgreSQL connection string)

### UI Libraries
- **shadcn/ui**: Pre-built accessible components using Radix UI primitives
- **Tailwind CSS**: Utility-first CSS framework
- **Framer Motion**: Animation library
- **Lucide React**: Icon library

### Key NPM Packages
- `@tanstack/react-query`: Server state management
- `drizzle-orm` / `drizzle-zod`: Database ORM and validation
- `express-session` / `connect-pg-simple`: Session management
- `openid-client` / `passport`: Authentication
- `zod`: Schema validation