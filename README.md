# Next.js Store

A full-featured **Next.js (App Router)** storefront built as a learning project. It demonstrates a small production-style e-commerce stack with product management, user authentication, favorites & reviews, a shopping cart, Stripe checkout, image uploads (Supabase), and a Postgres + Prisma backend.

> This README was written after a code-first review of the project structure and source files.

---

## Table of Contents

* [What is this project](#what-is-this-project)
* [Key features](#key-features)
* [ScreenShots](#ScreenShots)
* [Tech stack (high level)](#tech-stack-high-level)
* [Quickstart — run locally](#quickstart---run-locally)
* [Environment variables](#environment-variables)
* [.env.example](#envexample)
* [Database & seeding](#database--seeding)
* [How it’s organised (high level)](#how-its-organised-high-level)
* [How it works (architecture notes)](#how-it-works-architecture-notes)
* [Important implementation notes & gotchas](#important-implementation-notes--gotchas)
* [Scripts](#scripts)
* [Deployment notes](#deployment-notes)
* [Contributing](#contributing)
* [License](#license)

---

## What is this project

A small but opinionated Next.js storefront using the App Router. It’s intended as a learning/boilerplate project and includes server-side database operations (Prisma), server actions, user auth (Clerk), image storage (Supabase), and Stripe-powered checkout.

Use it as a reference for how an App Router + Prisma + auth + payments flow can be structured in a modern Next.js app.

---

## Key features

* Product catalog (list, single product pages)
* Admin CRUD for products (image upload + management)
* Favorites (per-user)
* Reviews (per-user, rating + comment)
* Shopping cart persisted in the database per user
* Checkout using **Stripe** (embedded checkout + server-side session creation)
* Image storage and retrieval via **Supabase** storage
* Authentication & user state via **Clerk**
* Uses Prisma to model Product / Favorite / Review / Cart / CartItem / Order
* UI built with shadcn-like components, Radix primitives and TailwindCSS

---
## screenshots

Here is a clean, professional **Screenshots section** you can paste directly into your README.
I’ve structured it in a GitHub-friendly layout with headings + spaces for your images.

---

# 📸 Screenshots

### 🏠 Home Page

Shows the main product listing grid.

![Home Page](<img width="1882" height="922" alt="image" src="https://github.com/user-attachments/assets/6dc36a77-3493-4ed6-bf47-35f44f1c97d1" />
)

---

### 📄 Product Page

Single product view with images, price, details, reviews, and Add to Cart.

![Product Page](<img width="926" height="949" alt="image" src="https://github.com/user-attachments/assets/2ca6b7d6-b9f2-4106-8672-4b3450b02e5d" />
)

---

### ❤️ Favorites Page

Displays all products marked as favorites by the user.

![Favorites Page](<img width="1882" height="922" alt="image" src="https://github.com/user-attachments/assets/4efc3a62-9369-496a-b73a-9d0c32d62922" />
)

---

### 🛒 Cart Page

User's shopping cart with items, quantity controls, and total price.

![Cart Page](<img width="1507" height="946" alt="image" src="https://github.com/user-attachments/assets/da3e31ef-dde4-4bf8-ba67-f4906a8e2f48" />
)

---

### 💳 Checkout (Stripe)

Stripe Embedded Checkout used for processing payments.

![Checkout](<img width="1507" height="946" alt="image" src="https://github.com/user-attachments/assets/84e7cd11-b6ea-4049-9f1e-f17e8463f6e4" />
)

---

### 🛠️ Admin Dashboard

Admin panel for managing products (create, edit, delete).

![Admin Dashboard](<img width="1575" height="897" alt="image" src="https://github.com/user-attachments/assets/543d616a-684e-4127-9a16-35695754d49e" />
)

---
## Tech stack (high level)

* **Next.js** (App Router)
* **React 18** (server + client components)
* **Prisma** + PostgreSQL (database)
* **Clerk** for authentication
* **Supabase** for image storage
* **Stripe** for payments
* **Tailwind CSS** + shadcn/Radix UI for UI components
* TypeScript

> See `package.json` for the exact package list used in this project.

---

## Quickstart — run locally

**Prerequisites**

* Node.js (recommended: latest LTS — Node 18+ / 20+ works with the packages used)
* A Postgres instance (local or managed)
* (Optional) Supabase project for image upload
* Clerk account (for auth) and Stripe account for payments

Steps:

1. Clone the repo

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

2. Install dependencies

```bash
npm install
```

3. Create a `.env.local` at the project root and add the environment variables (see next section)

4. Initialize the database and Prisma client

```bash
# generate client and push schema to DB
npx prisma generate
npx prisma db push

# optionally run migrations instead of db push if you prefer
# npx prisma migrate dev --name init
```

5. Seed example products (the project includes `prisma/products.json` and a `prisma/seed.js` helper)

```bash
node prisma/seed.js
# verify with prisma studio if you want
npx prisma studio
```

6. Run the dev server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

---

## Environment variables

The project expects (at minimum) these variables to be defined in your environment (example names used in the project code):

* `DATABASE_URL` — Postgres connection string (Prisma)
* `DIRECT_URL` — optional direct DB URL used by Prisma if you use pgBouncer or Supabase DirectURL patterns
* `SUPABASE_URL` — your Supabase project URL (used for image storage)
* `SUPABASE_KEY` — key used to access Supabase storage from the server (keep secret)
* `STRIPE_SECRET_KEY` — your Stripe secret key (server)
* `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` — Stripe publishable key (client)
* `ADMIN_USER_ID` — Clerk user id to mark admin operations (used in server actions)
* Clerk environment variables — configure Clerk per their docs (dashboard) so that auth works in Next.js (set values in your environment as required by Clerk)

**Important:** Do NOT commit `.env.local` to source control. Add it to `.gitignore`.

---

## .env.example

```bash
# Database
DATABASE_URL=postgresql://USER:PASSWORD@HOST:PORT/DATABASE
DIRECT_URL=postgresql://USER:PASSWORD@HOST:PORT/DATABASE

# Supabase (for image storage)
SUPABASE_URL=https://your-supabase-instance.supabase.co
SUPABASE_KEY=your-service-role-key

# Stripe (payments)
STRIPE_SECRET_KEY=sk_test_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...

# Clerk (authentication)
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your-clerk-publishable-key
CLERK_SECRET_KEY=your-clerk-secret-key

# App settings
ADMIN_USER_ID=your-clerk-user-id-for-admin
```

Use this template to create `.env.local` in your project root. Replace placeholders with your actual values.

---

## Database & seeding

* Prisma schema is in `prisma/schema.prisma` and models: `Product`, `Favorite`, `Review`, `Cart`, `CartItem`, `Order`.
* To populate example data the project includes `prisma/products.json` and `prisma/seed.js` — run `node prisma/seed.js` after `npx prisma db push`.

---

## How it’s organised (high level)

```
app/                 # Next.js App Router pages (server and client components)
components/          # Re-usable UI and feature components
prisma/              # Prisma schema, seed and example products.json
utils/               # DB helper, supabase helper, server actions, zod schemas
public/              # static assets
styles/              # tailwind / global css
```

Key utilities worth scanning:

* `utils/actions.ts` — server actions that drive the core product/cart/favorite/review flows
* `utils/db.ts` — Prisma client singleton pattern (prevents connection explosion in dev)
* `utils/supabase.ts` — image upload / delete helpers (bucket name configured in code)
* `prisma/schema.prisma` — database models

---

## How it works (architecture notes)

* The project uses **Next.js App Router** — top-level `app/layout.tsx` defines the `ClerkProvider` + `Providers` and common layout.
* Data fetching and mutations are implemented as **server actions** and server-side helpers (see `utils/actions.ts`) — most actions run on the server and use the Prisma client directly.
* Auth is provided by **Clerk**. Server-side functions call Clerk server helpers (e.g. `currentUser()` / `auth()`) for user context and authorization checks.
* Images are uploaded to Supabase Storage via `utils/supabase.ts` and public URLs are returned/stored on the product record.
* Checkout uses **Stripe**: the client requests a server-created checkout session (`/api/payment`) and then receives a `clientSecret` for the embedded checkout flow. After checkout completes the `/api/confirm` route marks the order `isPaid` and cleans up the cart.
* Cart and order totals are computed & stored in the DB via `utils/actions` helper functions.

---

## Important implementation notes & gotchas

* **Prisma in dev**: the repo uses a Prisma client singleton pattern (`utils/db.ts`) to avoid opening many DB connections during Hot Module Replacement (HMR). Keep that in place for local dev to prevent connection exhaustion.

* **Image storage bucket**: the code expects a bucket name (the example uses `main-bucket` in `utils/supabase.ts`). Either create that bucket in Supabase or change the constant there.

* **Admin checks**: admin routes/actions compare the current Clerk user id to `ADMIN_USER_ID`. Set that env var to one of your test Clerk account ids to allow admin actions.

* **Next.js remote images**: `next.config.mjs` contains `remotePatterns` for `images.pexels.com`, Supabase hosts, and Clerk images — keep these hosts in `next.config.mjs` if you add other image sources.

* **Build script runs Prisma generate**: the `build` script runs `npx prisma generate && next build` so ensure your DB/Prisma generator config is valid in CI and when deploying.

---

## Scripts

Common commands (mirror what is in `package.json`):

```bash
npm run dev      # starts dev server (next dev)
npm run build    # generates Prisma client and builds the app (npx prisma generate && next build)
npm run start    # run production server (next start)
npm run lint     # run lint checks (if configured)
```

---

## Deployment notes

* **Vercel** is a natural fit for App Router apps. When deploying:

  * Set all environment variables in your Vercel project (DATABASE_URL, DIRECT_URL, SUPABASE_URL, SUPABASE_KEY, STRIPE keys, Clerk keys, ADMIN_USER_ID, etc.)
  * Ensure your production DB is accessible from the deployment.
  * The `build` step will run `npx prisma generate` — make sure Prisma has access to the schema generator and network.

* If you host on other platforms, ensure environment variables and any storage (Supabase) are reachable from your server.

---

## Contributing

If you want to adapt this for your own use or contribute back:

* Keep sensitive values out of the repo (`.env.local`)
* Seed and test with sample products before adding production data
* Open PRs with small focused changes

---

## License

This project is intended for learning. Add a license file if you publish or share it publicly (MIT is commonly used).

---

