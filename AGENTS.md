# Project guidance

This directory is the documentation and coordination root. Application code lives in independent child repositories; do not treat this root as a monorepo or add shared packages here.

## Durable architecture

- `api/` and `app/` are separate Git repositories with their own dependencies, lockfiles, and release histories. Keep their boundaries independent.
- The web client in `app/` uses TypeScript, React 19, Vite 7, and the shadcn/ui configuration in `app/components.json` (preset `b7ClNFsdU`).
- The service in `api/` uses TypeScript, Node.js, Express 5, Prisma 7, SQLite for local development, Zod validation, and JWT sessions.
- The API owns persistence, identity, authorization, request validation, and listing lifecycle. Web clients consume its versioned JSON API; client-side checks do not replace server authorization.
- API routes use the `/api/v1` prefix. Keep errors structured and do not expose credentials, password hashes, tokens, or internal moderation notes.
- Mobile is in the product scope, but no mobile repository is present in this checkout. If it is added, keep it as an independent repository and have it use the same API and product rules.
- Keep credentials and local databases out of source control. Never store plaintext passwords.

## Documentation boundaries

- `SPEC.md` is the changing product source of truth: user journeys, feature behavior, scope, and acceptance criteria.
- This `AGENTS.md` holds the slower-changing repository boundaries and technology choices.
- `CLAUDE.md` is a short entry point for Claude-compatible tooling and points to this file and `SPEC.md`.
- Each child repository's `README.md` is for people using or developing that project. Keep agent-only instructions in that child's `AGENTS.md`.
- Read a child README for local setup. Read that child's `AGENTS.md` for implementation guidance.
