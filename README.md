# Maison Zahra

Premium fashion e-commerce for Morocco: clothing, shoes and accessories. Next.js (App Router), PostgreSQL, Drizzle ORM.

Default store name, colours, languages, currency (MAD), shipping zones, policies and homepage copy are stored in the database and can be changed from `/admin` without editing code.

## What works

- Multilingual storefront: Arabic (RTL), French and English (LTR)
- Catalogue, collections, filters, search, product pages, reviews (moderated)
- Guest and customer carts, coupons, cash-on-delivery checkout with server-side stock locks
- Customer accounts, addresses, wishlist, order history
- Private admin dashboard: analytics, products, inventory, orders, customers, promotions, content, settings
- Persistent PostgreSQL data (orders, stock, users, payments ledger)

## What is not faked

- Card payments succeed only if a real gateway is connected (not in this sandbox)
- Emails and WhatsApp campaigns are not marked as sent unless SMTP / a messaging provider is configured
- Carrier tracking numbers are recorded manually; live tracking APIs are not connected

## Setup

1. PostgreSQL with `DATABASE_URL`
2. Environment:

```
DATABASE_URL=postgresql://postgres:postgres@127.0.0.1:5432/app_db
AUTH_SECRET=<long random string>
ADMIN_EMAIL=atelier@maisonzahra.ma
ADMIN_PASSWORD=<at least 10 characters>
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Admin is created on first seed **only if** `ADMIN_PASSWORD` is set (10+ characters). It is never printed on the storefront.

3. Apply schema and start:

```
npx drizzle-kit push
npm run build
npm run start
```

The first request seeds sample catalogue data if the database is empty. Replace sample products from the admin.

Optional: `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS` for transactional email.

## Payments

COD is the supported method for Morocco in this build. To add cards later:

- Store gateway secrets in environment variables
- Add `/api/payments/webhook` with signature verification
- Never mark an order paid from a browser redirect alone

## Backups

```
pg_dump "$DATABASE_URL" > backup.sql
psql "$DATABASE_URL" < backup.sql
```

## Legal

Sample policies are editorial placeholders. Replace them with owner-approved terms before relying on them in commerce (Moroccan consumer protection, Law 09-08, tax).
