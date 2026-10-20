# Deluqis Booking Platform

Booking, e-contract and invoicing system for Deluqis, a wedding photography and videography business in Malaysia. Clients book a date, sign a contract online, pay a deposit and a balance by FPX, and receive their gallery link.

> **Status:** Week 1 of 6, foundation. See [docs/ROADMAP.md](docs/ROADMAP.md).

## Stack

- **Backend:** Java 21, Spring Boot 4, PostgreSQL 17, Flyway
- **Admin and client portal:** React, Vite, TypeScript
- **Public website:** Astro
- **Payments:** Billplz (FPX). **Email:** Resend. **File storage:** Cloudflare R2
- **Runtime:** Docker Compose on AWS Lightsail, CI on GitHub Actions

## Repository layout

| Folder     | What it holds                                                                                                |
|------------|--------------------------------------------------------------------------------------------------------------|
| `backend/` | Spring Boot API, a modular monolith: `booking`, `contract`, `invoice`, `payment`, `notification`, `common`   |
| `admin/`   | React admin dashboard and client portal                                                                      |
| `web/`     | Astro public website                                                                                         |
| `docs/`    | Roadmap and architecture decision records                                                                    |

## Running locally

Requires JDK 21.

```bash
cd backend
./mvnw verify            # build and run all tests
./mvnw spring-boot:run   # start the API on http://localhost:8080
```

On Windows PowerShell, use `.\mvnw.cmd` instead of `./mvnw`.

Health check: http://localhost:8080/actuator/health
