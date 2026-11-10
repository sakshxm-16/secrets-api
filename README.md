# Secrets API

![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white)
![Live Demo](https://img.shields.io/badge/Live-Demo-00C7B7?logo=render&logoColor=white)

A playful Express.js API for creating, reading, filtering, and managing anonymous secret entries. The app is live at:

https://secrets-api-bu6d.onrender.com/

## Overview

Secrets API is a demo backend built to explore common API patterns, including:

- Express route design and middleware
- Basic authentication
- Bearer-token authentication with Passport.js
- API key validation
- Rate limiting
- CRUD-style resource operations
- Pagination and score-based filtering
- A lightweight front-end UI for interaction

This project is intentionally simple and stores data in a local storage layer instead of a production database, making it ideal for learning and demonstration.

## Features

- Register users with a username and password
- Generate API keys for filtered read access
- Request bearer tokens for user-specific operations
- Fetch a random secret
- Retrieve all secrets with pagination
- Filter secrets by embarrassment score
- Create, update, patch, and delete secrets
- Explore a lightweight HTML interface served by the app

## Tech Stack

- Node.js
- Express
- EJS
- Passport.js
- express-basic-auth
- express-rate-limit
- node-localstorage

## Project Structure

```text
secrets-api/
├── app.js
├── package.json
├── public/
│   └── style/
├── db/
│   ├── index.js
│   └── users.js
├── scratch/
├── views/
│   └── index.html
├── README.md
└── node_modules/
```

## Quick Start

1. Install dependencies:

```bash
npm install
```

2. Start the server:

```bash
npm start
```

3. Open the app in your browser at:

```text
http://localhost:4000
```

## Authentication

The API uses different auth mechanisms depending on the route:

- Basic Auth: used for `/all`
- API key query parameter: used for `/filter`
- Bearer Token: used for authenticated secret CRUD APIs

## API Routes

### Public routes

#### GET `/`
Serves the homepage.

#### GET `/random`
Returns a random secret from the default seed dataset.

#### POST `/register`
Registers a new username and password.

Example request body:

```json
{
  "username": "alice",
  "password": "secret123"
}
```

#### GET `/generate-api-key`
Generates and returns a new API key.

#### POST `/get-auth-token`
Creates a bearer token for a registered user.

Example request body:

```json
{
  "username": "alice",
  "password": "secret123"
}
```

Example response:

```json
{
  "token": "<uuid>"
}
```

### Protected routes

All protected endpoints below require a bearer token in the `Authorization` header:

```http
Authorization: Bearer <token>
```

#### GET `/user-secrets`
Returns the authenticated user's secrets.

#### GET `/secrets/:id`
Returns the secret with the matching ID.

#### POST `/secrets`
Creates a new secret for the authenticated user.

Example request body:

```json
{
  "secret": "I secretly collect free hotel toiletries.",
  "score": 4
}
```

#### PUT `/secrets/:id`
Replaces a secret by ID.

#### PATCH `/secrets/:id`
Updates a secret by ID.

#### DELETE `/secrets/:id`
Deletes a secret by ID.

### Filter and auth examples

#### GET `/all`
Requires HTTP Basic Auth.

Example:

```bash
curl -u username:password http://localhost:4000/all
```

#### GET `/filter?score=5&apiKey=<your-api-key>`
Returns secrets with an embarrassment score greater than or equal to the supplied value.

Example:

```bash
curl "http://localhost:4000/filter?score=5&apiKey=YOUR_API_KEY"
```

#### Example authenticated request

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  http://localhost:4000/user-secrets
```

## Notes

- This is a demo backend, not a production-ready persistence layer.
- Data can reset when the app restarts or when local storage is cleared.
- It is intended for learning Express patterns, authentication flow, and API design.

## License

This project is distributed under the ISC license as defined in `package.json`.
