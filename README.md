# rocket-jwt
[![Build Status](https://travis-ci.com/marirs/rocket-jwt.svg?branch=master)](https://travis-ci.com/marirs/rocket-jwt)
![GitHub](https://img.shields.io/github/license/marirs/rocket-jwt)

![Windows](https://github.com/mnaza/rocket-jwt/actions/workflows/windows.yml/badge.svg)
![Ubuntu](https://github.com/mnaza/rocket-jwt/actions/workflows/linux.yml/badge.svg)
![MacOs](https://github.com/mnaza/rocket-jwt/actions/workflows/MacOs.yml/badge.svg)
![Armv7](https://github.com/mnaza/rocket-jwt/actions/workflows/LinuxArm7.yml/badge.svg)

A barebones [Rocket](https://rocket.rs) API with [JWT](https://jwt.io) authentication and 
database integration.

The database integration can be switched between [sqlite](https://www.sqlite.org/index.html) and [postgres](https://www.postgresql.org/) with a simple feature flag.

## Requirement
- Rust version 1.52 or newer.
- Diesel CLI with `postgres` or `sqlite` features.
- A running [PostgreSQL](https://www.postgresql.org/) or [Sqlite](https://www.sqlite.org/index.html) backend.
  
## Setup environment

A `.env.{db}` at the root directory exposes environment both used by `diesel`and the project itself.  
Rename it to `.env` then set all the environment variables before running the following commands :

``` bash
source .env
```

## Build locally

Run the following command to fulfill the requirements :

``` bash
# Install Rust and cargo alongside rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install ORM and query builder
cargo install diesel_cli --no-default-features --features "postgres sqlite"

# Run migrations
diesel setup 
diesel migration run
```

Then build the project with `default` database `(sqlite)`:
```bash
cargo build --release
```

or build with `postgresql` database:

``` bash
$ cargo build --release --no-default-features --features postgres
```

## Create the first user
```bash
# Build the create-user binary
cargo build --release --features create-user-binary

# Execute the binary to create the first admin user
../target/release/create-user
```

## Available `feature` flags
You can use the feature flag to switch between database use:

- sqlite (default)
- postgres

To build the create-user binary
- create-user-binary

## Switching the database at the backend

Any time you want to perform a database switch, you have to:
1. Install and run the desired database on your machine if is not already the case
2. Run migrations
3. Build the project with the respective feature flag.

## Usage

To print the project usage, an option `-h` is available.

## API routes

- Get Swagger docs
``` http
GET /docs
```

- Create a new user
```http
POST /users
Authorization: Bearer <token>
{
"username": "string",
"password": "string",
"email": "string",
"is_admin": boolean
}
```

- Authenticate
```http
POST /auth
{
"username": "string",
"password": "string"
}
```

- Get a list of users
```http
GET /users
Authorization: Bearer <token>
```

- Delete an existing user
```http
DELETE /users/<username>
Authorization: Bearer <token>
```

---
License: MIT