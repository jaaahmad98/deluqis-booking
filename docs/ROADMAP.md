# Roadmap

Six weeks to a live booking, e-contract and invoicing system for Deluqis. Tick a task (`- [x]`) once it is done and its tests pass.

## Week 1: foundation

Goal: the project builds, tests run in CI, and an admin can log in.

- [x] Repo layout: `backend/`, `admin/`, `web/`, `docs/`, a root `README.md`, and a root `.gitignore` covering `.env`, `CLAUDE.local.md`, `node_modules/`, `target/` and IDE files
- [ ] Spring Boot skeleton (Maven, Java 21) with packages `booking`, `contract`, `invoice`, `payment`, `notification` and `common`
- [ ] `compose.yaml` for PostgreSQL 17, the first Flyway migration, and `.env.example`
- [ ] GitHub Actions: build and test on every push
- [ ] Spring Security: admin login with role `ADMIN`
- [ ] Swagger UI with springdoc

## Week 2: bookings

Goal: an admin can receive inquiries, approve them and hold a date, with no double booking.

- [ ] Entities `Client`, `Package` and `Booking`, with migrations
- [ ] Public inquiry API with Cloudflare Turnstile verification
- [ ] Partial unique index on `(event_date, slot)` for active statuses; a clash returns HTTP 409
- [ ] Admin approval moves a booking to `HELD`, with a hold that expires 72 hours later
- [ ] `@Scheduled` job that moves bookings past their hold expiry to `EXPIRED`
- [ ] Testcontainers test: two concurrent bookings for the same date, exactly one succeeds
- [ ] Admin React app: booking list and approve button

## Week 3: e-contracts

Goal: a client can open a magic link, read the contract and sign it.

- [ ] Contract template and server-side PDF generation (OpenPDF or OpenHTMLtoPDF)
- [ ] Store the PDF in R2 with its SHA-256 hash
- [ ] Client magic link: random token, stored hashed, with an expiry
- [ ] Signing page with signature_pad in the client portal
- [ ] Signing audit trail: name, email, IP, user agent, timestamp and PDF hash
- [ ] Email the signed contract to the client and the admin (Resend)

## Week 4: deposits

Goal: a client pays the deposit by FPX and the booking becomes `CONFIRMED`.

- [ ] Deposit invoice with a gap-free yearly number, for example `DLQ-2026-0001`
- [ ] Billplz integration: create a bill, redirect the client, receive the callback
- [ ] Verify `X-Signature` on every callback
- [ ] Idempotent callback handler
- [ ] Booking moves to `CONFIRMED`, and a receipt is emailed
- [ ] Audit log for every status change and payment
- [ ] Tests for valid, forged and repeated callbacks

## Week 5: balance and reconciliation

Goal: balances are invoiced and collected automatically, and payments reconcile daily.

- [ ] `@Scheduled` reminder and balance invoice, for example 14 days before the event
- [ ] Balance payment through Billplz moves the booking to `PAID`
- [ ] Daily reconciliation job that matches Billplz payments to invoices and flags mismatches
- [ ] Admin dashboard: bookings this month, deposits received, balances outstanding
- [ ] Admin pastes the Pixieset link, the booking moves to `DELIVERED`, and the client gets an email

## Week 6: go-live

Goal: the system is live at `api.deluqis.com`, and one real booking has gone through every status.

- [ ] Deploy to AWS Lightsail with Docker Compose, HTTPS through Cloudflare
- [ ] Daily `pg_dump` backup to R2, with one restore tested
- [ ] Actuator health check connected to an uptime monitor
- [ ] Connect the Astro site's booking form to the API
- [ ] End-to-end test with one real booking and a small payment
- [ ] README with an architecture diagram and a short demo video

## After the MVP (local only)

- [ ] Publish `booking.confirmed` events to Kafka, with a small notification consumer
- [ ] Kubernetes manifests in `k8s/`, deployed to kind or k3d
- [ ] ADR in `docs/adr/`: why production is a modular monolith, not microservices

## Optional

- [ ] Port the booking module to .NET
- [ ] RAG-based FAQ chatbot for clients
