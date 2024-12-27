# Demo Loco Axum Tokio Rust

Demonstration of:

* Loco web application framework

* Axum modular web framework

* Tokio asynchronous runtime

* Rust programming language


## Create a new app


### Install prerequisites

Install prerequisites of loco and sea-orm-cli (if you want a database).

```sh
$ cargo install loco
$ cargo install sea-orm-cli
```


### Create a new app

For this demo, we will create a new app with these settings:

* Build a Software-as-a-Service (SaaS) app

* Use server-side rendering. Other options include client-side rendering.

* Database provider is PostgreSQL Other options include SQLite.

* Background worker type is async i.e. in-process tokio async tasks. Other options include blocking.

Create a new app via prompts;

```sh
$ loco new
✔ ❯ App name? · demo
✔ ❯ What would you like to build? · Saas App with server side rendering
✔ ❯ Select a DB Provider · Postgres
✔ ❯ Select your background worker type · Async (in-process tokio async tasks)

🚂 Loco app generated successfully in:
/Users/jph/git/joelparkerhenderson/demo/demo_loco

- database: You've selected `postgres` as your DB provider (you should have a postgres instance to connect to)
```

Create a new app via parameters:

```sh
$ loco new --name demo --db postgres --bg async --assets serverside
```

Build:

```sh
$ cd demo
$ cargo build
```


### Create a database

We use PostgreSQL to create a database, create a database administor role, and grant all privileges to the role.

SQL:

```sql
CREATE ROLE demo_owner WITH LOGIN ENCRYPTED PASSWORD 'secret';
CREATE DATABASE demo_development OWNER demo_owner;
GRANT ALL PRIVILEGES ON DATABASE demo_development TO demo_owner;
```

Set an environment variable DATABASE_URL with the database connection URL:

```sh
$ export DATABASE_URL="postgres://demo_owner:secret@localhost:5432/demo_development"
```


### Start

Start the loco app:

```sh
$ cargo loco start
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.69s
     Running `target/debug/demo-cli start`
INFO app: loco_rs::config: loading environment from selected_path="config/development.yaml" environment=development
WARN app: loco_rs::boot: pretty backtraces are enabled (this is great for development but has a runtime cost for production. disable with `logger.pretty_backtrace` in your config yaml) environment=development
INFO app: loco_rs::db: auto migrating environment=development
INFO app: sea_orm_migration::migrator: Applying all pending migrations environment=development
INFO app: sea_orm_migration::migrator: Applying migration 'm20220101_000001_users' environment=development
INFO app: sea_orm_migration::migrator: Migration 'm20220101_000001_users' has been applied environment=development
INFO app: loco_rs::boot: initializers loaded initializers="view-engine" environment=development
INFO app: loco_rs::controller::app_routes: [GET] /_ping environment=development
INFO app: loco_rs::controller::app_routes: [GET] /_health environment=development
INFO app: loco_rs::controller::app_routes: [POST] /api/auth/register environment=development
INFO app: loco_rs::controller::app_routes: [POST] /api/auth/verify environment=development
INFO app: loco_rs::controller::app_routes: [POST] /api/auth/login environment=development
INFO app: loco_rs::controller::app_routes: [POST] /api/auth/forgot environment=development
INFO app: loco_rs::controller::app_routes: [POST] /api/auth/reset environment=development
INFO app: loco_rs::controller::app_routes: [GET] /api/auth/current environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="limit_payload" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="catch_panic" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="etag" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="static" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="logger" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="request_id" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="fallback" environment=development
INFO app: loco_rs::controller::app_routes: +middleware name="powered_by" environment=development
INFO app: demo::initializers::view_engine: locales loaded environment=development

                      ▄     ▀
                                 ▀  ▄
                  ▄       ▀     ▄  ▄ ▄▀
                                    ▄ ▀▄▄
                        ▄     ▀    ▀  ▀▄▀█▄
                                          ▀█▄
▄▄▄▄▄▄▄  ▄▄▄▄▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄▄▄ ▀▀█
 ██████  █████   ███ █████   ███ █████   ███ ▀█
 ██████  █████   ███ █████   ▀▀▀ █████   ███ ▄█▄
 ██████  █████   ███ █████       █████   ███ ████▄
 ██████  █████   ███ █████   ▄▄▄ █████   ███ █████
 ██████  █████   ███  ████   ███ █████   ███ ████▀
   ▀▀▀██▄ ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀ ██▀
       ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                https://loco.rs

environment: development
   database: automigrate
     logger: debug
compilation: debug
      modes: server

listening on http://localhost:5150
```

### Fix

If you get this optimization message about "pretty backtraces", then that's fine, and you can optimize it later:

```txt
INFO app: loco_rs::config: loading environment from selected_path="config/development.yaml" environment=development
WARN app: loco_rs::boot: pretty backtraces are enabled (this is great for development but has a runtime cost for production. disable with `logger.pretty_backtrace` in your config yaml) environment=development
```

If you get this error message about "PgDatabaseError", then it means that your environment variable DATABASE_URL is not connecting to your database correctly, so you'll need to fix either the variable or the your database server or your database connection:

```txt
Error: DB(Conn(SqlxError(Database(PgDatabaseError { severity: Fatal, code: "28000", message: "role \"loco\" does not exist", detail: None, hint: None, position: None, where: None, schema: None, table: None, column: None, data_type: None, constraint: None, file: Some("miscinit.c"), line: Some(752), routine: Some("InitializeSessionUserId") }))))
```


# Welcome to Loco :train:

[Loco](https://loco.rs) is a web and API framework running on Rust.

This is the **SaaS starter** which includes a `User` model and authentication based on JWT.

It also include configuration sections that help you pick either a frontend or a server-side template set up for your fullstack server.


## Quick Start

```sh
cargo loco start
```

```sh
$ cargo loco start
Finished dev [unoptimized + debuginfo] target(s) in 21.63s
    Running `target/debug/myapp start`

    :
    :
    :

controller/app_routes.rs:203: [Middleware] Adding log trace id

                      ▄     ▀
                                 ▀  ▄
                  ▄       ▀     ▄  ▄ ▄▀
                                    ▄ ▀▄▄
                        ▄     ▀    ▀  ▀▄▀█▄
                                          ▀█▄
▄▄▄▄▄▄▄  ▄▄▄▄▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄▄▄ ▀▀█
 ██████  █████   ███ █████   ███ █████   ███ ▀█
 ██████  █████   ███ █████   ▀▀▀ █████   ███ ▄█▄
 ██████  █████   ███ █████       █████   ███ ████▄
 ██████  █████   ███ █████   ▄▄▄ █████   ███ █████
 ██████  █████   ███  ████   ███ █████   ███ ████▀
   ▀▀▀██▄ ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀ ██▀
       ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                https://loco.rs

environment: development
   database: automigrate
     logger: debug
compilation: debug
      modes: server

listening on http://localhost:5150
```

## Full Stack Serving

You can check your [configuration](config/development.yaml) to pick either frontend setup or server-side rendered template, and activate the relevant configuration sections.


## Getting help

Check out [a quick tour](https://loco.rs/docs/getting-started/tour/) or [the complete guide](https://loco.rs/docs/getting-started/guide/).
