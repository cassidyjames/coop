# Getting Started

Get set up with Coop for the first time for local development or demoing.

See [Local Development](local.md) for prerequisites, configuration details, troubleshooting, and more. You may also want to familiarize yourself with Coop's [Basic Concepts](../user/concepts.md) for additional context. This guide assumes some familiarity with the command line, e.g. using a Terminal with `bash` or `zsh`.

To get Coop running:

0. **Clone the repository** using `git` and enter the `coop` folder if you haven't already.

   ```sh
   git clone https://github.com/roostorg/coop.git && cd coop
   ```

1. **Ensure you have prerequisites** installed, including `nvm`, `docker`, and the correct version of Node.js.

   ```sh
   nvm --version && nvm install && nvm use
   docker --version
   ```

   You should see output like:

   ```
   0.40.4
   Found '.nvmrc' with version <24.14.1>
   v24.14.1 is already installed.
   Now using node v24.14.1 (npm v11.11.0)
   Docker version 29.4.3, build 055a478
   ```

   If you get an error instead, see [Prerequisites](local.md#prerequisites).

2. **Start all backing services**; see [Docker Services](local.md#docker-services) for ports and more details.

   ```sh
   npm run up
   ```

   Wait for PostgreSQL, ClickHouse, ScyllaDB, and Redis to be healthy before continuing; you can check progress with `docker ps`.

3. **Install dependencies** with `npm` in the root folder and for each sub-package.

   ```sh
   npm install
   (cd db && npm install)
   (cd server && npm install)
   (cd client && npm install)
   ```

4. **Copy `.env.example` files to `.env`** in `db/`, `server/`, and `client/`. The example defaults work for local development and demoing out of the box. See [Environment Setup](local.md#environment-setup) for details of the available options.

   ```sh
   cp db/.env.example db/.env
   cp server/.env.example server/.env
   cp client/.env.example client/.env
   ```

5. **Create databases** and run migrations to ensure they're set up correctly:

   ```sh
   npm run db:create -- --env staging --db api-server-pg
   npm run db:create -- --env staging --db scylla
   npm run db:create -- --env staging --db clickhouse

   npm run db:update -- --env staging --db api-server-pg
   npm run db:update -- --env staging --db scylla
   npm run db:update -- --env staging --db clickhouse
   ```

6. **Copy static asset files** using the script in the `server/` folder:

   ```sh
   cd server && npm run copy-assets
   ```

7. **Create an organization and admin user** using the `create-org` script from the `server/` folder, providing the appropriate details.

   For example:

   ```sh
   npm run create-org -- \
     --name "Your Organization" \
     --email "email@example.com" \
     --website "https://example.com" \
     --firstName "Jane" \
     --lastName "Doe" \
     --password "correct-horse-battery-staple"
   ```

8. Finally, **start the application**! See [Running the Application](local.md#running-the-application) for additional options.

   ```sh
   npm run server:start   # Terminal 1
   npm run client:start   # Terminal 2
   ```

Log in at [localhost:3000](http://localhost:3000) using the credentials printed by `create-org`. The initial page load may take a moment.
