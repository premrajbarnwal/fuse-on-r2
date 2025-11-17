# AGENTS.md - Developer Guide for Coding Agents

## Build/Test/Lint Commands
- **Dev**: `npm run dev` - Start local Wrangler dev server on http://localhost:8787
- **Deploy**: `npm run deploy` - Deploy to Cloudflare Workers
- **Type Generation**: `npm run cf-typegen` - Regenerate worker-configuration.d.ts after wrangler.jsonc changes
- **No test/lint commands configured** - This project has no testing framework or linting setup

## Code Style Guidelines

**TypeScript (src/):**
- ES2021 target, ES2022 modules with Bundler resolution, strict mode enabled
- Import order: External packages (@cloudflare, hono) → Types/interfaces → Local modules
- Use explicit types for `Env` bindings and Hono context (`Hono<{ Bindings: Env }>`)
- Container classes extend `Container<Env>` with properties: `defaultPort`, `sleepAfter`, `envVars`
- Use `getContainer()` helper for Durable Object stubs, forward requests via `container.fetch(c.req.raw)`

**Go (container_src/):**
- Standard library formatting, grouped imports (stdlib → external → internal)
- JSON struct tags for API responses (camelCase in JSON via backticks)
- HTTP handlers: Check env vars first, return structured JSON responses, log warnings (not errors) for non-critical issues
- Main: Use graceful shutdown with signal handling (SIGINT/SIGTERM) and 5s timeout context

**Error Handling:**
- TypeScript: Propagate errors via `await`, let Hono handle HTTP errors
- Go: Return `http.Error()` with descriptive messages, use `log.Printf()` for warnings, `log.Fatal()` only in main

**Environment Variables:** Flow from wrangler.jsonc → Worker Env → Container envVars → Go process. Never hardcode secrets in production.
