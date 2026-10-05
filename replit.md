# Book Bazaar

A responsive, sign-in-first school bookstore for browsing books by class and subject, building a cart, and placing delivery orders.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required environment: provisioned PostgreSQL connection and Replit-managed Clerk configuration

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/book-bazaar/` — React + Vite storefront and static artwork
- `artifacts/api-server/src/routes/bookstore.ts` — catalog, cart, checkout, and order API
- `lib/api-spec/openapi.yaml` — API source of truth; generated React Query hooks and Zod schemas live in shared libraries
- `lib/db/src/schema/bookstore.ts` — PostgreSQL schema for books, promotions, carts, and orders
- `artifacts/api-server/src/lib/bookstoreSeed.ts` — idempotent starter catalog and promotion data

## Architecture decisions

- PostgreSQL was selected from the database options available in this workspace instead of adding a MongoDB service.
- Clerk authenticates shoppers; cart and order data are scoped to the Clerk user ID on the server.
- Checkout currently places persistent cash-on-delivery orders; online payment processing is not configured.
- The catalog and offers are seeded into PostgreSQL and served through the API, not hardcoded in the storefront.

## Product

- Public welcome, sign-in, and sign-up pages; shopping requires an authenticated account.
- Browse offers and books, filter by class and subject, search titles/authors, and add individual books or subject bundles to the cart.
- Edit cart quantities, check out with delivery details, and view order history and order details.

## User preferences

- The user selected PostgreSQL when asked to choose between the workspace database and MongoDB.

## Gotchas

- After editing `lib/api-spec/openapi.yaml`, run API code generation before using the updated generated types.
- The Clerk proxy middleware is only active in production; local development uses Clerk's normal Frontend API flow.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
