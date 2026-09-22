# CyberShield

CyberShield is a Node.js and MongoDB web application for monitoring authentication activity and managing basic access controls. It provides a browser-based security dashboard where administrators can review users, update roles, lock accounts, and view recorded threat events.

## Features

- JWT-based user registration and login
- Role-based administrator endpoints
- Failed-login tracking and automatic IP blocking after five failed attempts
- MongoDB-backed threat event and blocked-IP records
- Threat event feed with a Chart.js trend chart
- Admin user management: list users, change roles, and delete users
- Admin access controls: view whitelisted-IP users and lock or unlock accounts
- Static login and dashboard pages served by Express

## Tech stack

- **Backend:** Node.js, Express
- **Database:** MongoDB with Mongoose
- **Authentication:** JSON Web Tokens and bcryptjs
- **Frontend:** HTML, CSS, JavaScript, Chart.js

## Project structure

```text
Cyber-Shield/
├── config/          # MongoDB connection setup
├── controllers/     # Authentication and administrator logic
├── middleware/      # JWT, admin-role, and IP whitelist middleware
├── models/          # MongoDB schemas
├── public/          # Login page, dashboard, styles, and browser scripts
├── routes/          # API route definitions
└── server.js         # Application entry point
```

## Prerequisites

- Node.js 18 or newer
- A MongoDB database (local or Atlas)

## Getting started

1. Clone the repository and enter its folder.

   ```bash
   git clone <your-repository-url>
   cd Cyber-Shield
   ```

2. Install dependencies.

   ```bash
   npm install
   ```

3. Create a `.env` file in the project root.

   ```env
   MONGO_URI=mongodb+srv://<username>:<password>@<cluster>/<database>
   JWT_SECRET=replace-with-a-long-random-secret
   PORT=5000
   ```

4. Start the server.

   ```bash
   node server.js
   ```

5. Open [http://localhost:5000/login.html](http://localhost:5000/login.html) in a browser.

## Creating the first administrator

The project does not include seed data. Register an account through the API, then log in with it. For local development, send a request such as:

```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"Admin\",\"email\":\"admin@example.com\",\"password\":\"change-me\",\"role\":\"admin\"}"
```

> Registration currently accepts the submitted `role`. Restrict this endpoint or seed the first administrator directly before deploying the application publicly.

## API overview

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/auth/register` | Register a user account. |
| `POST` | `/api/auth/login` | Authenticate and receive a JWT. |
| `GET` | `/api/users/profile` | Get the authenticated user's profile. |
| `GET` | `/api/threats` | List recorded threat events. |
| `POST` | `/api/threats/add` | Add a threat event. |
| `GET` | `/api/admin/all-users` | List users (admin only). |
| `PUT` | `/api/admin/update-role/:id` | Change a user's role (admin only). |
| `DELETE` | `/api/admin/delete-user/:id` | Delete a user (admin only). |
| `GET` | `/api/admin/access-control` | List users associated with whitelisted IPs (admin only). |
| `PUT` | `/api/admin/lock-user/:id` | Lock a user account (admin only). |
| `PUT` | `/api/admin/unlock-user/:id` | Unlock a user account (admin only). |
| `GET` | `/api/admin/threat-analytics` | Return security-log analytics (admin only). |
| `DELETE` | `/api/admin/unblock-ip/:ip` | Remove an IP from the block list. |

For protected endpoints, include the token returned at login:

```http
Authorization: Bearer <jwt-token>
```

## Security notes

This repository is a development project and should be hardened before production use:

- Never commit `.env` files, database credentials, or JWT secrets. Use a local `.env` file and repository secrets instead.
- Protect every privileged and data-changing endpoint with authentication and authorization.
- Validate and sanitize request input, especially user roles and threat submissions.
- Use HTTPS, production-safe CORS settings, logging, rate limits, and persistent failed-login counters.
- Review the IP address handling strategy when operating behind proxies.

## License

No license has been specified. Add a license file before distributing or reusing the project.
