# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Payload CMS is a Next.js-native headless CMS built with TypeScript. This is a monorepo containing the core Payload packages, plugins, and test suites.

## Project Structure

- `packages/` - Core packages and plugins
  - `payload/` - Main Payload package
  - `next/` - Next.js integration
  - `ui/` - UI components
  - `db-*` - Database adapters (MongoDB, Postgres, SQLite, Vercel Postgres)
  - `richtext-*` - Rich text editors (Lexical, Slate)
  - `plugin-*` - Official plugins
  - `storage-*` - Storage adapters (S3, Azure, GCS, Vercel Blob, Uploadthing)
- `test/` - Comprehensive test suite with integration and E2E tests
- `templates/` - Starter templates
- `examples/` - Example implementations

## Development Commands

### Building

```bash
# Build core packages (excluding plugins and storage adapters)
pnpm build

# Build all packages
pnpm build:all

# Force rebuild (clears cache)
pnpm build:force

# Build specific packages
pnpm build:payload
pnpm build:next
pnpm build:ui
pnpm build:db-mongodb
pnpm build:db-postgres
pnpm build:richtext-lexical
```

### Testing

```bash
# Run all tests (integration and E2E)
pnpm test

# Run integration tests
pnpm test:int
pnpm test:int:postgres  # Test with Postgres
pnpm test:int:sqlite    # Test with SQLite

# Run E2E tests
pnpm test:e2e
pnpm test:e2e:headed    # Run with browser visible
pnpm test:e2e:debug     # Debug mode

# Run component tests
pnpm test:components

# Type checking for test suite
pnpm build:tests
```

### Development Server

```bash
# Start dev server with specific test suite
pnpm dev [test-dir]         # e.g., pnpm dev fields
pnpm dev:postgres [test-dir] # Use Postgres
pnpm dev:memorydb          # Use in-memory database

# Generate types for a test directory
pnpm dev:generate-types [test-dir]

# Generate GraphQL schema
pnpm dev:generate-graphql-schema
```

### Code Quality

```bash
# Run linting
pnpm lint
pnpm lint:fix

# Type checking
pnpm build:tests  # Type check test suite

# Run pre-commit hooks manually
pnpm lint-staged
```

### Utilities

```bash
# Clean build artifacts
pnpm clean:build

# Full reinstall
pnpm reinstall

# Start Docker services (MongoDB, Postgres)
pnpm docker:start
pnpm docker:stop
```

## Testing Architecture

Tests are organized by feature in the `test/` directory:
- `config.ts` - Payload configuration for testing
- `int.spec.ts` - Integration tests (Jest)
- `e2e.spec.ts` - End-to-end tests (Playwright)
- `payload-types.ts` - Generated types

Default test credentials:
- Email: `dev@payloadcms.com`
- Password: `test`

## Database Support

The codebase supports multiple databases:
- MongoDB (default)
- PostgreSQL
- SQLite
- Vercel Postgres

Set `PAYLOAD_DATABASE` environment variable to switch databases during development.

## Build System

This project uses:
- **Turbo** for monorepo build orchestration
- **PNPM workspaces** for dependency management
- **SWC** for TypeScript compilation
- **Next.js 15** for the admin UI

## Key Development Patterns

1. All packages follow TypeScript-first development
2. Use existing UI components from `@payloadcms/ui`
3. Follow existing code conventions in neighboring files
4. Hooks are available for every Payload action
5. All UI strings must be internationalized
6. Security: Never expose or log secrets/keys

## Contributing

1. Fork and create feature branches
2. Write tests for new functionality
3. Use conventional commits for PR titles
4. Run `pnpm lint:fix` before committing
5. Update translations for new UI strings

## Environment Setup

Required:
- Node.js 18.20.2+ or 20.9.0+
- PNPM 9.7.0+
- Enable corepack: `corepack enable`

## Common Tasks

### Creating a New Test Suite
1. Create directory under `test/`
2. Add `config.ts` with minimal Payload config
3. Write integration tests in `int.spec.ts`
4. Generate types: `pnpm dev:generate-types [dir-name]`
5. Start dev: `pnpm dev [dir-name]`

### Working with Translations
1. Add strings to `packages/translations/src/languages/en.json`
2. Run `pnpm translateNewKeys` (requires OpenAI API key)
3. Use `useTranslation` hook in UI components