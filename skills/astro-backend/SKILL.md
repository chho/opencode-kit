---
name: astro-backend
description: Astro backend and database integrations - Astro DB (built-in SQLite), authentication (Better Auth, Clerk, Auth.js, Firebase, Lucia, Supabase), backend services (Supabase, Firebase, Neon, Prisma, Turso, Xata, Sentry monitoring), e-commerce, and image/video hosting (Cloudinary, Mux). Load this when adding a database, auth, or backend service to Astro.
---

# Astro Backend & Services Documentation

> This skill covers database, authentication, and backend service integrations for Astro.


# Astro DB

> Learn how to use Astro DB, a fully-managed SQL database designed exclusively for Astro.

Astro DB is a fully-managed SQL database designed for the Astro ecosystem. Develop locally in Astro and deploy to any libSQL-compatible database.

Astro DB is a complete solution to configuring, developing, and querying your data. A local database is created in `.astro/content.db` whenever you run `astro dev` to manage your data without the need for Docker or a network connection.

## Installation

[Section titled “Installation”](#installation)

Install the [`@astrojs/db` integration](/en/guides/integrations-guide/db/) using the built-in `astro add` command:

* npm

  ```sh
  npx astro add db
  ```

* pnpm

  ```sh
  pnpm astro add db
  ```

* Yarn

  ```sh
  yarn astro add db
  ```

## Define your database

[Section titled “Define your database”](#define-your-database)

Installing `@astrojs/db` with the `astro add` command will automatically create a `db/config.ts` file in your project where you will define your database tables:

db/config.ts

```ts
import { defineDb } from 'astro:db';


export default defineDb({
  tables: { },
})
```

### Tables

[Section titled “Tables”](#tables)

Data in Astro DB is stored using SQL tables. Tables structure your data into rows and columns, where columns enforce the type of each row value.

Define your tables in your `db/config.ts` file by providing the structure of the data in your existing libSQL database, or the data you will collect in a new database. This will allow Astro to generate a TypeScript interface to query that table from your project. The result is full TypeScript support when you access your data with property autocompletion and type-checking.

To configure a database table, import and use the `defineTable()` and `column` utilities from `astro:db`. Then, define a name (case-sensitive) for your table and the type of data in each column.

This example configures a `Comment` table with required text columns for `author` and `body`. Then, makes it available to your project through the `defineDb()` export.

db/config.ts

```ts
import { defineDb, defineTable, column } from 'astro:db';


const Comment = defineTable({
  columns: {
    author: column.text(),
    body: column.text(),
  }
})


export default defineDb({
  tables: { Comment },
})
```

See the [table configuration reference](/en/guides/integrations-guide/db/#table-configuration-reference) for a complete reference of table options.

### Columns

[Section titled “Columns”](#columns)

Astro DB supports the following column types:

db/config.ts

```ts
import { defineTable, column } from 'astro:db';


const Comment = defineTable({
  columns: {
    // A string of text.
    author: column.text(),
    // A whole integer value.
    likes: column.number(),
    // A true or false value.
    flagged: column.boolean(),
    // Date/time values queried as JavaScript Date objects.
    published: column.date(),
    // An untyped JSON object.
    metadata: column.json(),
  }
});
```

See the [table columns reference](/en/guides/integrations-guide/db/#table-configuration-reference) for more details.

### Table References

[Section titled “Table References”](#table-references)

Relationships between tables are a common pattern in database design. For example, a `Blog` table may be closely related to other tables of `Comment`, `Author`, and `Category`.

You can define these relations between tables and save them into your database schema using **reference columns**. To establish a relationship, you will need:

* An **identifier column** on the referenced table. This is usually an `id` column with the `primaryKey` property.
* A column on the base table to **store the referenced `id`**. This uses the `references` property to establish a relationship.

This example shows a `Comment` table’s `authorId` column referencing an `Author` table’s `id` column.

db/config.ts

```ts
const Author = defineTable({
  columns: {
    id: column.number({ primaryKey: true }),
    name: column.text(),
  }
});


const Comment = defineTable({
  columns: {
    authorId: column.number({ references: () => Author.columns.id }),
    body: column.text(),
  }
});
```

## Seed your database for development

[Section titled “Seed your database for development”](#seed-your-database-for-development)

In development, Astro will use your DB config to generate local types according to your schemas. These will be generated fresh from your seed file each time the dev server is started, and will allow you to query and work with the shape of your data with type safety and autocompletion.

You will not have access to production data during development unless you [connect to a remote database](#connecting-to-remote-databases) during development. This protects your data while allowing you to test and develop with a working database with type-safety.

To seed development data for testing and debugging into your Astro project, create a `db/seed.ts` file. Import both the `db` object and your tables defined in `astro:db`. `insert` some initial data into each table. This development data should match the form of both your database schema and production data.

The following example defines two rows of development data for a `Comment` table, and an `Author` table:

db/seed.ts

```ts
import { db, Comment, Author } from 'astro:db';


export default async function() {
  await db.insert(Author).values([
    { id: 1, name: "Kasim" },
    { id: 2, name: "Mina" },
  ]);


  await db.insert(Comment).values([
    { authorId: 1, body: 'Hope you like Astro DB!' },
    { authorId: 2, body: 'Enjoy!'},
  ])
}
```

Your development server will automatically restart your database whenever this file changes, regenerating your types and seeding this development data from `seed.ts` fresh each time.

## Connect a libSQL database for production

[Section titled “Connect a libSQL database for production”](#connect-a-libsql-database-for-production)

Astro DB can connect to any local libSQL database or to any server that exposes the libSQL remote protocol, whether managed or self-hosted.

To connect Astro DB to a libSQL database, set the following environment variables obtained from your database provider:

* `ASTRO_DB_REMOTE_URL`: the connection URL to the location of your local or remote libSQL DB. This may include [URL configuration options](#remote-url-configuration-options) such as sync and encryption as parameters.
* `ASTRO_DB_APP_TOKEN`: the auth token to your libSQL server. This is required for remote databases, and not needed for [local DBs like files or in-memory](#url-scheme-and-host) databases

Depending on your service, you may have access to a CLI or web UI to retrieve these values. The following section will demonstrate connecting to Turso and setting these values as an example, but you are free to use any provider.

### Getting started with Turso

[Section titled “Getting started with Turso”](#getting-started-with-turso)

Turso is the company behind [libSQL](https://github.com/tursodatabase/libsql), the open-source fork of SQLite that powers Astro DB. They provide a fully managed libSQL database platform and are fully compatible with Astro.

The steps below will guide you through the process of installing the Turso CLI, logging in (or signing up), creating a new database, getting the required environmental variables, and pushing the schema to the remote database.

1. Install the [Turso CLI](https://docs.turso.tech/cli/installation).

2. [Log in or sign up](https://docs.turso.tech/cli/authentication) to Turso.

3. Create a new database. In this example the database name is `andromeda`.

   ```sh
   turso db create andromeda
   ```

4. Run the `show` command to see information about the newly created database:

   ```sh
   turso db show andromeda
   ```

   Copy the `URL` value and set it as the value for `ASTRO_DB_REMOTE_URL`.

   .env

   ```dotenv
   ASTRO_DB_REMOTE_URL=libsql://andromeda-houston.turso.io
   ```

5. Create a new token to authenticate requests to the database:

   ```sh
   turso db tokens create andromeda
   ```

   Copy the output of the command and set it as the value for `ASTRO_DB_APP_TOKEN`.

   .env

   ```diff
   ASTRO_DB_REMOTE_URL=libsql://andromeda-houston.turso.io
   +ASTRO_DB_APP_TOKEN=eyJhbGciOiJF...3ahJpTkKDw
   ```

6. Push your DB schema and metadata to the new Turso database.

   ```sh
   astro db push --remote
   ```

7. Congratulations, now you have a database connected! Give yourself a break. 👾

   ```sh
   turso relax
   ```

To explore more features of Turso, check out the [Turso docs](https://docs.turso.tech).

### Connecting to remote databases

[Section titled “Connecting to remote databases”](#connecting-to-remote-databases)

Astro DB allows you to connect to both local and remote databases. By default, Astro uses a local database file for `dev` and `build` commands, recreating tables and inserting development seed data each time.

To connect to a hosted remote database, use the `--remote` flag. This flag enables both readable and writable access to your remote database, allowing you to [accept and persist user data](#insert) in production environments.

Configure your build command to use the `--remote` flag:

package.json

```json
{
  "scripts": {
    "build": "astro build --remote"
  }
}
```

You can also use the flag directly in the command line:

```bash
# Build with a remote connection
astro build --remote


# Develop with a remote connection
astro dev --remote
```

Caution

Be careful when using `--remote` in development. This connects to your live production database, and all changes (inserts, updates, deletions) will be persisted.

The `--remote` flag uses the connection to the remote DB both locally during the build and on the server. Ensure you set the necessary environment variables in both your local development environment and your deployment platform. Additionally, you may need to [configure web mode](/en/guides/integrations-guide/db/#mode) for non-Node.js runtimes such as Cloudflare Workers or Deno.

When deploying your Astro DB project, make sure your deployment platform’s build command is set to `npm run build` (or the equivalent for your package manager) to utilize the `--remote` flag configured in your `package.json`.

### Remote URL configuration options

[Section titled “Remote URL configuration options”](#remote-url-configuration-options)

The `ASTRO_DB_REMOTE_URL` environment variable configures the location of your database as well as other options like sync and encryption.

#### URL scheme and host

[Section titled “URL scheme and host”](#url-scheme-and-host)

libSQL supports both HTTP and WebSockets as the transport protocol for a remote server. It also supports using a local file or an in-memory DB. Those can be configured using the following URL schemes in the connection URL:

* `memory:` will use an in-memory DB. The host must be empty in this case.
* `file:` will use a local file. The host is the path to the file (`file:path/to/file.db`).
* `libsql:` will use a remote server through the protocol preferred by the library (this might be different across versions). The host is the address of the server (`libsql://your.server.io`).
* `http:` will use a remote server through HTTP. `https:` can be used to enable a secure connection. The host is the same as for `libsql:`.
* `ws:` will use a remote server through WebSockets. `wss:` can be used to enable a secure connection. The host is the same as for `libsql:`.

Details of the libSQL connection (e.g. encryption key, replication, sync interval) can be configured as query parameters in the remote connection URL.

For example, to have an encrypted local file work as an embedded replica to a libSQL server, you can set the following environment variables:

.env

```dotenv
ASTRO_DB_REMOTE_URL=file://local-copy.db?encryptionKey=your-encryption-key&syncInterval=60&syncUrl=libsql%3A%2F%2Fyour.server.io
ASTRO_DB_APP_TOKEN=token-to-your-remote-url
```

Caution

Using a database file is an advanced feature, and care should be taken when deploying to prevent overriding your database and losing your production data.

Additionally, this method will not work in serverless deployments, as the file system is not persisted in those environments.

#### `encryptionKey`

[Section titled “encryptionKey”](#encryptionkey)

libSQL has native support for encrypted databases. Passing this search parameter will enable encryption using the given key:

.env

```dotenv
ASTRO_DB_REMOTE_URL=file:path/to/file.db?encryptionKey=your-encryption-key
```

#### `syncUrl`

[Section titled “syncUrl”](#syncurl)

Embedded replicas are a feature of libSQL clients that creates a full synchronized copy of your database on a local file or in memory for ultra-fast reads. Writes are sent to a remote database defined on the `syncUrl` and synchronized with the local copy.

Use this property to pass a separate connection URL to turn the database into an embedded replica of another database. This should only be used with the schemes `file:` and `memory:`. The parameter must be URL encoded.

For example, to have an in-memory embedded replica of a database on `libsql://your.server.io`, you can set the connection URL as such:

.env

```dotenv
ASTRO_DB_REMOTE_URL=memory:?syncUrl=libsql%3A%2F%2Fyour.server.io
```

#### `syncInterval`

[Section titled “syncInterval”](#syncinterval)

Interval between embedded replica synchronizations in seconds. By default it only synchronizes on startup and after writes.

This property is only used when `syncUrl` is also set. For example, to set an in-memory embedded replica to synchronize every minute set the following environment variable:

.env

```dotenv
ASTRO_DB_REMOTE_URL=memory:?syncUrl=libsql%3A%2F%2Fyour.server.io&syncInterval=60
```

## Query your database

[Section titled “Query your database”](#query-your-database)

You can query your database from any [Astro page](/en/basics/astro-pages/#astro-pages), [endpoint](/en/guides/endpoints/), or [action](/en/guides/actions/) in your project using the provided `db` ORM and query builder.

### Drizzle ORM

[Section titled “Drizzle ORM”](#drizzle-orm)

```ts
import { db } from 'astro:db';
```

Astro DB includes a built-in [Drizzle ORM](https://orm.drizzle.team/) client. There is no setup or manual configuration required to use the client. The Astro DB `db` client is automatically configured to communicate with your database (local or remote) when you run Astro. It uses your exact database schema definition for type-safe SQL queries with TypeScript errors when you reference a column or table that doesn’t exist.

### Select

[Section titled “Select”](#select)

The following example selects all rows of a `Comment` table. This returns the complete array of seeded development data from `db/seed.ts` which is then available for use in your page template:

src/pages/index.astro

```astro
---
import { db, Comment } from 'astro:db';


const comments = await db.select().from(Comment);
---


<h2>Comments</h2>


{
  comments.map(({ author, body }) => (
    <article>
      <p>Author: {author}</p>
      <p>{body}</p>
    </article>
  ))
}
```

See the [Drizzle `select()` API reference](https://orm.drizzle.team/docs/select) for a complete overview.

### Insert

[Section titled “Insert”](#insert)

To accept user input, such as handling form requests and inserting data into your remote hosted database, configure your Astro project for [on-demand rendering](/en/guides/on-demand-rendering/) and [add an adapter](/en/guides/on-demand-rendering/#add-an-adapter) for your deployment environment.

This example inserts a row into a `Comment` table based on a parsed form POST request:

src/pages/index.astro

```astro
---
import { db, Comment } from 'astro:db';


if (Astro.request.method === 'POST') {
  // Parse form data
  const formData = await Astro.request.formData();
  const author = formData.get('author');
  const body = formData.get('body');
  if (typeof author === 'string' && typeof body === 'string') {
    // Insert form data into the Comment table
    await db.insert(Comment).values({ author, body });
  }
}


// Render the new list of comments on each request
const comments = await db.select().from(Comment);
---


<form method="POST" style="display: grid">
  <label for="author">Author</label>
  <input id="author" name="author" />


  <label for="body">Body</label>
  <textarea id="body" name="body"></textarea>


  <button type="submit">Submit</button>
</form>


<!-- Render `comments` -->
```

You can also use [Astro actions](/en/guides/actions/) to insert data into an Astro DB table. The following example inserts a row into a `Comment` table using an action:

src/actions/index.ts

```ts
import { db, Comment } from 'astro:db';
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  addComment: defineAction({
    // Actions include type safety with Zod, removing the need
    // to check if typeof {value} === 'string' in your pages
    input: z.object({
      author: z.string(),
      body: z.string(),
    }),
    handler: async (input) => {
      const updatedComments = await db
        .insert(Comment)
        .values(input)
        .returning(); // Return the updated comments
      return updatedComments;
    },
  }),
};
```

See the [Drizzle `insert()` API reference](https://orm.drizzle.team/docs/insert) for a complete overview.

### Delete

[Section titled “Delete”](#delete)

You can also query your database from an API endpoint. This example deletes a row from a `Comment` table by the `id` parameter:

src/pages/api/comments/\[id].ts

```ts
import type { APIRoute } from "astro";
import { db, Comment, eq } from 'astro:db';


export const DELETE: APIRoute = async (ctx) => {
  await db.delete(Comment).where(eq(Comment.id, ctx.params.id ));
  return new Response(null, { status: 204 });
}
```

See the [Drizzle `delete()` API reference](https://orm.drizzle.team/docs/delete) for a complete overview.

### Filtering

[Section titled “Filtering”](#filtering)

To query for table results by a specific property, use [Drizzle options for partial selects](https://orm.drizzle.team/docs/select#partial-select). For example, add [a `.where()` call](https://orm.drizzle.team/docs/select#filtering) to your `select()` query and pass the comparison you want to make.

The following example queries for all rows in a `Comment` table that contain the phrase “Astro DB.” Use [the `like()` operator](https://orm.drizzle.team/docs/operators#like) to check if a phrase is present within the `body`:

src/pages/index.astro

```astro
---
import { db, Comment, like } from 'astro:db';


const comments = await db.select().from(Comment).where(
    like(Comment.body, '%Astro DB%')
);
---
```

### Drizzle utilities

[Section titled “Drizzle utilities”](#drizzle-utilities)

All Drizzle utilities for building queries are exposed from the `astro:db` module. This includes:

* [Filter operators](https://orm.drizzle.team/docs/operators) like `eq()` and `gt()`
* [Aggregation helpers](https://orm.drizzle.team/docs/select#aggregations-helpers) like `count()`
* [The `sql` helper](https://orm.drizzle.team/docs/sql) for writing raw SQL queries

```ts
import { eq, gt, count, sql } from 'astro:db';
```

### Relationships

[Section titled “Relationships”](#relationships)

You can query related data from multiple tables using a SQL join. To create a join query, extend your `db.select()` statement with a join operator. Each function accepts a table to join with and a condition to match rows between the two tables.

This example uses an `innerJoin()` function to join `Comment` authors with their related `Author` information based on the `authorId` column. This returns an array of objects with each `Author` and `Comment` row as top-level properties:

src/pages/index.astro

```astro
---
import { db, eq, Comment, Author } from 'astro:db';


const comments = await db.select()
  .from(Comment)
  .innerJoin(Author, eq(Comment.authorId, Author.id));
---


<h2>Comments</h2>


{
  comments.map(({ Author, Comment }) => (
    <article>
      <p>Author: {Author.name}</p>
      <p>{Comment.body}</p>
    </article>
  ))
}
```

See the [Drizzle join reference](https://orm.drizzle.team/docs/joins#join-types) for all available join operators and config options.

### Batch Transactions

[Section titled “Batch Transactions”](#batch-transactions)

All remote database queries are made as a network request. You may need to “batch” queries together into a single transaction when making a large number of queries, or to have automatic rollbacks if any query fails.

This example seeds multiple rows in a single request using the `db.batch()` method:

db/seed.ts

```ts
import { db, Author, Comment } from 'astro:db';


export default async function () {
  const queries = [];
  // Seed 100 sample comments into your remote database
  // with a single network request.
  for (let i = 0; i < 100; i++) {
    queries.push(db.insert(Comment).values({ body: `Test comment ${i}` }));
  }
  await db.batch(queries);
}
```

See the [Drizzle `db.batch()`](https://orm.drizzle.team/docs/batch-api) docs for more details.

## Pushing changes to your database

[Section titled “Pushing changes to your database”](#pushing-changes-to-your-database)

You can push changes made during development to your database.

### Pushing table schemas

[Section titled “Pushing table schemas”](#pushing-table-schemas)

Your table schema may change over time as your project grows. You can safely test configuration changes locally and push to your remote database when you deploy.

You can push your local schema changes to your remote database via the CLI using the `astro db push --remote` command:

* npm

  ```sh
  npm run astro db push --remote
  ```

* pnpm

  ```sh
  pnpm astro db push --remote
  ```

* Yarn

  ```sh
  yarn astro db push --remote
  ```

This command will verify that your local changes can be made without data loss and, if necessary, suggest how to safely make changes to your schema in order to resolve conflicts.

#### Pushing breaking schema changes

[Section titled “Pushing breaking schema changes”](#pushing-breaking-schema-changes)

Danger

**This will destroy your database**. Only perform this command if you do not need your production data.

If you must change your table schema in a way that is incompatible with your existing data hosted on your remote database, you will need to reset your production database.

To push a table schema update that includes a breaking change, add the `--force-reset` flag to reset all production data:

* npm

  ```sh
  npm run astro db push --remote --force-reset
  ```

* pnpm

  ```sh
  pnpm astro db push --remote --force-reset
  ```

* Yarn

  ```sh
  yarn astro db push --remote --force-reset
  ```

### Renaming tables

[Section titled “Renaming tables”](#renaming-tables)

It is possible to rename a table after pushing your schema to your remote database.

If you **do not have any important production data**, then you can [reset your database](#pushing-breaking-schema-changes) using the `--force-reset` flag. This flag will drop all of the tables in the database and create new ones so that it matches your current schema exactly.

To rename a table while preserving your production data, you must perform a series of non-breaking changes to push your local schema to your remote database safely.

The following example renames a table from `Comment` to `Feedback`:

1. In your database config file, add the `deprecated: true` property to the table you want to rename:

   db/config.ts

   ```diff
   const Comment = defineTable({
     deprecated: true,
     columns: {
       author: column.text(),
       body: column.text(),
     }
   });
   ```

2. Add a new table schema (matching the existing table’s properties exactly) with the new name:

   db/config.ts

   ```diff
   const Comment = defineTable({
     deprecated: true,
     columns: {
       author: column.text(),
       body: column.text(),
     }
   });
   +const Feedback = defineTable({
     columns: {
       author: column.text(),
       body: column.text(),
     }
   +});
   ```

3. [Push to your remote database](#pushing-table-schemas) with `astro db push --remote`. This will add the new table and mark the old as deprecated.

4. Update any of your local project code to use the new table instead of the old table. You might need to migrate data to the new table as well.

5. Once you are confident that the old table is no longer used in your project, you can remove the schema from your `config.ts`:

   db/config.ts

   ```diff
   -const Comment = defineTable({
     deprecated: true,
     columns: {
       author: column.text(),
       body: column.text(),
     }
   -});


   const Feedback = defineTable({
     columns: {
       author: column.text(),
       body: column.text(),
     }
   });
   ```

6. Push to your remote database again with `astro db push --remote`. The old table will be dropped, leaving only the new, renamed table.

### Pushing table data

[Section titled “Pushing table data”](#pushing-table-data)

You may need to push data to your remote database for seeding or data migrations. You can author a `.ts` file with the `astro:db` module to write type-safe queries. Then, execute the file against your remote database using the command `astro db execute <file-path> --remote`:

The following Comments can be seeded using the command `astro db execute db/seed.ts --remote`:

db/seed.ts

```ts
import { Comment } from 'astro:db';


export default async function () {
  await db.insert(Comment).values([
    { authorId: 1, body: 'Hope you like Astro DB!' },
    { authorId: 2, body: 'Enjoy!' },
  ])
}
```

See the [CLI reference](/en/guides/integrations-guide/db/#astro-db-cli-reference) for a complete list of commands.

## Building Astro DB integrations

[Section titled “Building Astro DB integrations”](#building-astro-db-integrations)

[Astro integrations](/en/reference/integrations-reference/) can extend user projects with additional Astro DB tables and seed data.

Use the `extendDb()` method in the `astro:db:setup` hook to register additional Astro DB config and seed files. The `defineDbIntegration()` helper provides TypeScript support and auto-complete for the `astro:db:setup` hook.

my-integration/index.ts

```js
import { defineDbIntegration } from '@astrojs/db/utils';


export default function MyIntegration() {
  return defineDbIntegration({
    name: 'my-astro-db-powered-integration',
    hooks: {
      'astro:db:setup': ({ extendDb }) => {
        extendDb({
          configEntrypoint: '@astronaut/my-package/config',
          seedEntrypoint: '@astronaut/my-package/seed',
        });
      },
      // Other integration hooks...
    },
  });
}
```

Integration [config](#define-your-database) and [seed](#seed-your-database-for-development) files follow the same format as their user-defined equivalents.

### Type safe operations in integrations

[Section titled “Type safe operations in integrations”](#type-safe-operations-in-integrations)

While working on integrations, you may not be able to benefit from Astro’s generated table types exported from `astro:db`. For full type safety, use the `asDrizzleTable()` utility to create a table reference object you can use for database operations.

For example, given an integration setting up the following `Pets` database table:

my-integration/config.ts

```js
import { defineDb, defineTable, column } from 'astro:db';


export const Pets = defineTable({
  columns: {
    name: column.text(),
    species: column.text(),
  },
});


export default defineDb({ tables: { Pets } });
```

The seed file can import `Pets` and use `asDrizzleTable()` to insert rows into your table with type checking:

my-integration/seed.ts

```js
import { asDrizzleTable } from '@astrojs/db/utils';
import { db } from 'astro:db';
import { Pets } from './config';


export default async function() {
  const typeSafePets = asDrizzleTable('Pets', Pets);


  await db.insert(typeSafePets).values([
    { name: 'Palomita', species: 'cat' },
    { name: 'Pan', species: 'dog' },
  ]);
}
```

The value returned by `asDrizzleTable('Pets', Pets)` is equivalent to `import { Pets } from 'astro:db'`, but is available even when Astro’s type generation can’t run. You can use it in any integration code that needs to query or insert into the database.

## Migrate from Astro Studio to Turso

[Section titled “Migrate from Astro Studio to Turso”](#migrate-from-astro-studio-to-turso)

1. In the [Studio dashboard](https://studio.astro.build/), navigate to the project you wish to migrate. In the settings tab, use the “Export Database” button to download a dump of your database.

2. Follow the official instructions to [install the Turso CLI](https://docs.turso.tech/cli/installation) and [sign up or log in](https://docs.turso.tech/cli/authentication) to your Turso account.

3. Create a new database on Turso using the `turso db create` command.

   ```sh
   turso db create [database-name]
   ```

4. Fetch the database URL using the Turso CLI, and use it as the environment variable `ASTRO_DB_REMOTE_URL`.

   ```sh
   turso db show [database-name]
   ```

   ```dotenv
   ASTRO_DB_REMOTE_URL=[your-database-url]
   ```

5. Create a token to access your database, and use it as the environment variable `ASTRO_DB_APP_TOKEN`.

   ```sh
   turso db tokens create [database-name]
   ```

   ```dotenv
   ASTRO_DB_APP_TOKEN=[your-app-token]
   ```

6. Push your DB schema and metadata to the new Turso database.

   ```sh
   astro db push --remote
   ```

7. Import the database dump from step 1 into your new Turso DB.

   ```sh
   turso db shell [database-name] < ./path/to/dump.sql
   ```

8. Once you have confirmed your project connects to the new database, you can safely delete the project from Astro Studio.

# Authentication

> An intro to authentication in Astro

Authentication and authorization are two security processes that manage access to your website or app. Authentication verifies a visitor’s identity, while authorization grants access to protected areas and resources.

Authentication allows you to customize areas of your site for logged-in individuals and provides the greatest protection for personal or private information. Authentication libraries (e.g. [Better Auth](https://better-auth.com/), [Clerk](https://clerk.com)) provide utilities for multiple authentication methods such as email sign-in and OAuth providers.

Tip

There is no official authentication solution for Astro, but you can find [community “auth” integrations](https://astro.build/integrations/?search=auth) in the integrations directory.

See how to [add authentication with Supabase](/en/guides/backend/supabase/#adding-authentication-with-supabase) or [add authentication with Firebase](/en/guides/backend/firebase/#adding-authentication-with-firebase) in our dedicated guides for these backend services.

## Better Auth

[Section titled “Better Auth”](#better-auth)

Better Auth is a framework-agnostic authentication (and authorization) framework for TypeScript. It provides a comprehensive set of features out of the box and includes a plugin ecosystem that simplifies adding advanced functionalities.

It supports Astro out of the box, and you can use it to add authentication to your astro project.

### Installation

[Section titled “Installation”](#installation)

* npm

  ```shell
  npm install better-auth
  ```

* pnpm

  ```shell
  pnpm add better-auth
  ```

* Yarn

  ```shell
  yarn add better-auth
  ```

For detailed setup instructions, check out the [Better Auth Installation Guide](https://www.better-auth.com/docs/installation).

### Configuration

[Section titled “Configuration”](#configuration)

Configure your database table to store user data and your preferred authentication methods as described in the [Better Auth Installation Guide](https://www.better-auth.com/docs/installation#configure-database). Then, you’ll need to mount the Better Auth handler in your Astro project.

src/pages/api/auth/\[...all].ts

```ts
import { auth } from "../../../lib/auth"; // import your Better Auth instance
import type { APIRoute } from "astro";


export const prerender = false; // Not needed in 'server' mode


export const ALL: APIRoute = async (ctx) => {
  return auth.handler(ctx.request);
};
```

Follow the [Better Auth Astro Guide](https://www.better-auth.com/docs/integrations/astro) to learn more.

### Usage

[Section titled “Usage”](#usage)

Better Auth offers a `createAuthClient` helper for various frameworks, including Vanilla JS, React, Vue, Svelte, and Solid.

For example, to create a client for React, import the helper from `'better-auth/react'`:

* React

  src/lib/auth-client.ts

  ```ts
  import { createAuthClient } from 'better-auth/react';


  export const authClient = createAuthClient();


  export const { signIn, signOut } = authClient;
  ```

* Solid

  src/lib/auth-client.ts

  ```ts
  import { createAuthClient } from 'better-auth/solid';


  export const authClient = createAuthClient();


  export const { signIn, signOut } = authClient;
  ```

* Svelte

  src/lib/auth-client.ts

  ```ts
  import { createAuthClient } from 'better-auth/svelte';


  export const authClient = createAuthClient();


  export const { signIn, signOut } = authClient;
  ```

* Vue

  src/lib/auth-client.ts

  ```ts
  import { createAuthClient } from 'better-auth/vue';


  export const authClient = createAuthClient();


  export const { signIn, signOut } = authClient;
  ```

Once your client is set up, you can use it to authenticate users in your Astro components or any framework-specific files. The following example adds the ability to log in or log out with your configured `signIn()` and `signOut()` functions.

src/pages/index.astro

```astro
---
import Layout from 'src/layouts/Base.astro';
---
<Layout>
  <button id="login">Login</button>
  <button id="logout">Logout</button>


  <script>
    const { signIn, signOut } = await import("./lib/auth-client")
    document.querySelector("#login").onclick = () => signIn.social({
      provider: "github",
      callbackURL: "/dashboard",
    })
    document.querySelector("#logout").onclick = () => signOut()
  </script>
</Layout>
```

You can then use the `auth` object to get the user’s session data in your server-side code. The following example personalizes page content by displaying an authenticated user’s name:

src/pages/index.astro

```astro
---
import { auth } from "../../../lib/auth"; // import your Better Auth instance


export const prerender = false; // Not needed in 'server' mode


const session = await auth.api.getSession({
  headers: Astro.request.headers,
});
---


<p>{session.user?.name}</p>
```

You can also use the `auth` object to protect your routes using middleware. The following example checks whether a user trying to access a logged-in dashboard route is authenticated, and redirects them to the home page if not.

src/middleware.ts

```ts
import { auth } from "../../../auth"; // import your Better Auth instance
import { defineMiddleware } from "astro:middleware";


export const onRequest = defineMiddleware(async (context, next) => {
  const isAuthed = await auth.api
    .getSession({
      headers: context.request.headers,
    })
  if (context.url.pathname === "/dashboard" && !isAuthed) {
    return context.redirect("/");
  }
  return next();
});
```

### Next Steps

[Section titled “Next Steps”](#next-steps)

* [Better Auth Astro Guide](https://www.better-auth.com/docs/integrations/astro)
* [Better Auth Astro Example](https://github.com/better-auth/examples/tree/main/astro-example)
* [Better Auth Documentation](https://www.better-auth.com/docs)
* [Better Auth GitHub Repository](https://github.com/better-auth/better-auth)

## Clerk

[Section titled “Clerk”](#clerk)

Clerk is a complete suite of embeddable UIs, flexible APIs, and admin dashboards to authenticate and manage your users. An [official Clerk SDK for Astro](https://clerk.com/docs/references/astro/overview) is available.

### Installation

[Section titled “Installation”](#installation-1)

Install `@clerk/astro` using the package manager of your choice.

* npm

  ```shell
  npm install @clerk/astro
  ```

* pnpm

  ```shell
  pnpm add @clerk/astro
  ```

* Yarn

  ```shell
  yarn add @clerk/astro
  ```

### Configuration

[Section titled “Configuration”](#configuration-1)

Follow [Clerk’s own Astro Quickstart guide](https://clerk.com/docs/quickstarts/astro) to set up Clerk integration and middleware in your Astro project.

### Usage

[Section titled “Usage”](#usage-1)

Clerk provides components that allow you to control the visibility of pages based on your user’s authentication state. Show logged out users a sign in button instead of the content available to users who are logged in:

src/pages/index.astro

```astro
---
import Layout from 'src/layouts/Base.astro';
import { SignedIn, SignedOut, UserButton, SignInButton } from '@clerk/astro/components';


export const prerender = false; // Not needed in 'server' mode
---


<Layout>
    <SignedIn>
        <UserButton />
    </SignedIn>
    <SignedOut>
        <SignInButton />
    </SignedOut>
</Layout>
```

Clerk also allows you to protect routes on the server using middleware. Specify which routes are protected, and prompt unauthenticated users to sign in:

src/middleware.ts

```ts
import { clerkMiddleware, createRouteMatcher } from '@clerk/astro/server';


const isProtectedRoute = createRouteMatcher([
  '/dashboard(.*)',
  '/forum(.*)',
]);


export const onRequest = clerkMiddleware((auth, context) => {
  if (!auth().userId && isProtectedRoute(context.request)) {
    return auth().redirectToSignIn();
  }
});
```

### Next Steps

[Section titled “Next Steps”](#next-steps-1)

* Read the [official `@clerk/astro` documentation](https://clerk.com/docs/references/astro/overview)
* Start from a template with the [Clerk + Astro Quickstart project](https://github.com/clerk/clerk-astro-quickstart)

## Lucia

[Section titled “Lucia”](#lucia)

[Lucia](https://lucia-auth.com/) is a resource for implementing session-based authentication in a number of frameworks, including Astro.

### Guides

[Section titled “Guides”](#guides)

1. Create a [basic sessions API](https://lucia-auth.com/sessions/basic-api/) with your chosen database.
2. Add [session cookies](https://lucia-auth.com/sessions/cookies/astro) using endpoints and middleware.
3. Implement [GitHub OAuth](https://lucia-auth.com/tutorials/github-oauth/astro) using the APIs you implemented.

### Examples

[Section titled “Examples”](#examples)

* [GitHub OAuth example in Astro](https://github.com/lucia-auth/example-astro-github-oauth)
* [Google OAuth example in Astro](https://github.com/lucia-auth/example-astro-google-oauth)
* [Email and password example with 2FA in Astro](https://github.com/lucia-auth/example-astro-email-password-2fa)
* [Email and password example with 2FA and WebAuthn in Astro](https://github.com/lucia-auth/example-astro-email-password-webauthn)

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Using Microsoft Entra Id EasyAuth with Astro and Azure Static Web App](https://agramont.net/blog/entra-id-easyauth-with-astro/)

# Use a backend service with Astro

> How to use a backend service to add authentication, storage and data

**Ready to add features like authentication, monitoring, storage, or data to your Astro project?** Follow one of our guides to integrate a backend service.

Tip

Find [community-maintained integrations](https://astro.build/integrations/) for adding popular features to your project in our integrations directory.

## Backend service guides

[Section titled “Backend service guides”](#backend-service-guides)

Note that many of these pages are **stubs**: they’re collections of resources waiting for your contribution!

* ![](/logos/appwriteio.svg)

  ### [Appwrite](/en/guides/backend/appwrite/)

* ![](/logos/firebase.svg)

  ### [Firebase](/en/guides/backend/firebase/)

* ![](/logos/neon.svg)

  ### [Neon](/en/guides/backend/neon/)

* ![](/logos/prisma-postgres.svg)

  ### [Prisma Postgres](/en/guides/backend/prisma-postgres/)

* ![](/logos/sentry.svg)

  ### [Sentry](/en/guides/backend/sentry/)

* ![](/logos/supabase.svg)

  ### [Supabase](/en/guides/backend/supabase/)

* ![](/logos/turso.svg)

  ### [Turso](/en/guides/backend/turso/)

* ![](/logos/xata.svg)

  ### [Xata](/en/guides/backend/xata/)

## What is a backend service?

[Section titled “What is a backend service?”](#what-is-a-backend-service)

A backend service is a cloud-based system that helps you build and manage your backend infrastructure. It provides a set of tools and services for managing databases, user authentication, and other server-side functionality. This enables you to focus on building your applications without having to worry about managing the underlying infrastructure.

## Why would I use a backend service?

[Section titled “Why would I use a backend service?”](#why-would-i-use-a-backend-service)

You might want to consider a backend service if your project has complex server-side needs, for example:

* user sign-ups and authentication
* persistent data storage
* user-uploaded asset storage
* API generation
* realtime communication
* application monitoring

# Appwrite & Astro

> Add a backend to your project with Appwrite

[Appwrite](https://appwrite.io/) is a self-hosted backend-as-a-service platform that provides authentication and account management, user preferences, database and storage persistence, cloud functions, localization, image manipulation, and other server-side utilities.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Appwrite Demos for Astro](https://github.com/appwrite/demos-for-astro)

# Firebase & Astro

> Add a backend to your project with Firebase

[Firebase](https://firebase.google.com/) is an app development platform that provides a NoSQL database, authentication, realtime subscriptions, functions, and storage.

See our separate guide for [deploying to Firebase hosting](/en/guides/deploy/firebase/).

## Initializing Firebase in Astro

[Section titled “Initializing Firebase in Astro”](#initializing-firebase-in-astro)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A [Firebase project with a web app configured](https://firebase.google.com/docs/web/setup).

* An Astro project with [`output: 'server'` for on-demand rendering](/en/guides/on-demand-rendering/) enabled.

* Firebase credentials: You will need two sets of credentials to connect Astro to Firebase:

  * Web app credentials: These credentials will be used by the client side of your app. You can find them in the Firebase console under *Project settings > General*. Scroll down to the **Your apps** section and click on the **Web app** icon.
  * Project credentials: These credentials will be used by the server side of your app. You can generate them in the Firebase console under *Project settings > Service accounts > Firebase Admin SDK > Generate new private key*.

### Adding Firebase credentials

[Section titled “Adding Firebase credentials”](#adding-firebase-credentials)

To add your Firebase credentials to Astro, create an `.env` file in the root of your project with the following variables:

.env

```ini
FIREBASE_PRIVATE_KEY_ID=YOUR_PRIVATE_KEY_ID
FIREBASE_PRIVATE_KEY=YOUR_PRIVATE_KEY
FIREBASE_PROJECT_ID=YOUR_PROJECT_ID
FIREBASE_CLIENT_EMAIL=YOUR_CLIENT_EMAIL
FIREBASE_CLIENT_ID=YOUR_CLIENT_ID
FIREBASE_AUTH_URI=YOUR_AUTH_URI
FIREBASE_TOKEN_URI=YOUR_TOKEN_URI
FIREBASE_AUTH_CERT_URL=YOUR_AUTH_CERT_URL
FIREBASE_CLIENT_CERT_URL=YOUR_CLIENT_CERT_URL
```

Now, these environment variables are available for use in your project.

If you would like to have IntelliSense for your Firebase environment variables, edit or create the file `env.d.ts` in your `src/` directory and configure your types:

src/env.d.ts

```ts
interface ImportMetaEnv {
  readonly FIREBASE_PRIVATE_KEY_ID: string;
  readonly FIREBASE_PRIVATE_KEY: string;
  readonly FIREBASE_PROJECT_ID: string;
  readonly FIREBASE_CLIENT_EMAIL: string;
  readonly FIREBASE_CLIENT_ID: string;
  readonly FIREBASE_AUTH_URI: string;
  readonly FIREBASE_TOKEN_URI: string;
  readonly FIREBASE_AUTH_CERT_URL: string
  readonly FIREBASE_CLIENT_CERT_URL: string;
}


interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

Tip

Read more about [environment variables](/en/guides/environment-variables/) and `.env` files in Astro.

Your project should now include these new files:

* src/

  * **env.d.ts**

* **.env**

* astro.config.mjs

* package.json

### Installing dependencies

[Section titled “Installing dependencies”](#installing-dependencies)

To connect Astro with Firebase, install the following packages using the single command below for your preferred package manager:

* `firebase` - the Firebase SDK for the client side
* `firebase-admin` - the Firebase Admin SDK for the server side

- npm

  ```shell
  npm install firebase firebase-admin
  ```

- pnpm

  ```shell
  pnpm add firebase firebase-admin
  ```

- Yarn

  ```shell
  yarn add firebase firebase-admin
  ```

Next, create a folder named `firebase` in the `src/` directory and add two new files to this folder: `client.ts` and `server.ts`.

In `client.ts`, add the following code to initialize Firebase in the client using your web app credentials and the `firebase` package:

src/firebase/client.ts

```ts
import { initializeApp } from "firebase/app";


const firebaseConfig = {
  apiKey: "my-public-api-key",
  authDomain: "my-auth-domain",
  projectId: "my-project-id",
  storageBucket: "my-storage-bucket",
  messagingSenderId: "my-sender-id",
  appId: "my-app-id",
};


export const app = initializeApp(firebaseConfig);
```

Note

Remember to replace the `firebaseConfig` object with your own web app credentials.

In `server.ts`, add the following code to initialize Firebase in the server using your project credentials and the `firebase-admin` package:

src/firebase/server.ts

```ts
import type { ServiceAccount } from "firebase-admin";
import { initializeApp, cert, getApps } from "firebase-admin/app";


const activeApps = getApps();
const serviceAccount = {
  type: "service_account",
  project_id: import.meta.env.FIREBASE_PROJECT_ID,
  private_key_id: import.meta.env.FIREBASE_PRIVATE_KEY_ID,
  private_key: import.meta.env.FIREBASE_PRIVATE_KEY,
  client_email: import.meta.env.FIREBASE_CLIENT_EMAIL,
  client_id: import.meta.env.FIREBASE_CLIENT_ID,
  auth_uri: import.meta.env.FIREBASE_AUTH_URI,
  token_uri: import.meta.env.FIREBASE_TOKEN_URI,
  auth_provider_x509_cert_url: import.meta.env.FIREBASE_AUTH_CERT_URL,
  client_x509_cert_url: import.meta.env.FIREBASE_CLIENT_CERT_URL,
};


const initApp = () => {
  if (import.meta.env.PROD) {
    console.info('PROD env detected. Using default service account.')
    // Use default config in firebase functions. Should be already injected in the server by Firebase.
    return initializeApp()
  }
  console.info('Loading service account from env.')
  return initializeApp({
    credential: cert(serviceAccount as ServiceAccount)
  })
}


export const app = activeApps.length === 0 ? initApp() : activeApps[0];
```

Note

Remember to replace the `serviceAccount` object with your own project credentials.

Finally, your project should now include these new files:

* src

  * env.d.ts

  * firebase

    * **client.ts**
    * **server.ts**

* .env

* astro.config.mjs

* package.json

## Adding authentication with Firebase

[Section titled “Adding authentication with Firebase”](#adding-authentication-with-firebase)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites-1)

* An Astro project [initialized with Firebase](#initializing-firebase-in-astro).
* A Firebase project with email/password authentication enabled in the Firebase console under *Authentication > Sign-in* method.

### Creating auth server endpoints

[Section titled “Creating auth server endpoints”](#creating-auth-server-endpoints)

Firebase authentication in Astro requires the following three [Astro server endpoints](/en/guides/endpoints/):

* `GET /api/auth/signin` - to sign in a user
* `GET /api/auth/signout` - to sign out a user
* `POST /api/auth/register` - to register a user

Create three endpoints related to authentication in a new directory `src/pages/api/auth/`: `signin.ts`, `signout.ts` and `register.ts`.

`signin.ts` contains the code to sign in a user using Firebase:

src/pages/api/auth/signin.ts

```ts
import type { APIRoute } from "astro";
import { app } from "../../../firebase/server";
import { getAuth } from "firebase-admin/auth";


export const GET: APIRoute = async ({ request, cookies, redirect }) => {
  const auth = getAuth(app);


  /* Get token from request headers */
  const idToken = request.headers.get("Authorization")?.split("Bearer ")[1];
  if (!idToken) {
    return new Response(
      "No token found",
      { status: 401 }
    );
  }


  /* Verify id token */
  try {
    await auth.verifyIdToken(idToken);
  } catch (error) {
    return new Response(
      "Invalid token",
      { status: 401 }
    );
  }


  /* Create and set session cookie */
  const fiveDays = 60 * 60 * 24 * 5 * 1000;
  const sessionCookie = await auth.createSessionCookie(idToken, {
    expiresIn: fiveDays,
  });


  cookies.set("__session", sessionCookie, {
    path: "/",
  });


  return redirect("/dashboard");
};
```

Caution

Firebase only allows the use of [one cookie, and it must be named `__session`](https://firebase.google.com/docs/hosting/manage-cache#using_cookies). Any other cookies the client sends will not be visible to your application.

Note

This is a basic implementation of the signin endpoint. You can add more logic to this endpoint to suit your needs.

`signout.ts` contains the code to log out a user by deleting the session cookie:

src/pages/api/auth/signout.ts

```ts
import type { APIRoute } from "astro";


export const GET: APIRoute = async ({ redirect, cookies }) => {
  cookies.delete("__session", {
    path: "/",
  });
  return redirect("/signin");
};
```

Note

This is a basic implementation of the signout endpoint. You can add more logic to this endpoint to suit your needs.

`register.ts` contains the code to register a user using Firebase:

src/pages/api/auth/register.ts

```ts
import type { APIRoute } from "astro";
import { getAuth } from "firebase-admin/auth";
import { app } from "../../../firebase/server";


export const POST: APIRoute = async ({ request, redirect }) => {
  const auth = getAuth(app);


  /* Get form data */
  const formData = await request.formData();
  const email = formData.get("email")?.toString();
  const password = formData.get("password")?.toString();
  const name = formData.get("name")?.toString();


  if (!email || !password || !name) {
    return new Response(
      "Missing form data",
      { status: 400 }
    );
  }


  /* Create user */
  try {
    await auth.createUser({
      email,
      password,
      displayName: name,
    });
  } catch (error: any) {
    return new Response(
      "Something went wrong",
      { status: 400 }
    );
  }
  return redirect("/signin");
};
```

Note

This is a basic implementation of the register endpoint. You can add more logic to this endpoint to suit your needs.

After creating server endpoints for authentication, your project directory should now include these new files:

* src

  * env.d.ts

  * firebase

    * client.ts
    * server.ts

  * pages

    * api

      * auth

        * **signin.ts**
        * **signout.ts**
        * **register.ts**

* .env

* astro.config.mjs

* package.json

### Creating pages

[Section titled “Creating pages”](#creating-pages)

Create the pages that will use the Firebase endpoints:

* `src/pages/register` - will contain a form to register a user
* `src/pages/signin` - will contain a form to sign in a user
* `src/pages/dashboard` - will contain a dashboard that can only be accessed by authenticated users

The example `src/pages/register.astro` below includes a form that will send a `POST` request to the `/api/auth/register` endpoint. This endpoint will create a new user using the data from the form and then will redirect the user to the `/signin` page.

src/pages/register.astro

```astro
---
import Layout from "../layouts/Layout.astro";
---


<Layout title="Register">
  <h1>Register</h1>
  <p>Already have an account? <a href="/signin">Sign in</a></p>
  <form action="/api/auth/register" method="post">
    <label for="name">Name</label>
    <input type="text" name="name" id="name" />
    <label for="email" for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Login</button>
  </form>
</Layout>
```

`src/pages/signin.astro` uses the Firebase server app to verify the user’s session cookie. If the user is authenticated, the page will redirect the user to the `/dashboard` page.

The example page below contains a form that will send a `POST` request to the `/api/auth/signin` endpoint with the ID token generated by the Firebase client app.

The endpoint will verify the ID token and create a new session cookie for the user. Then, the endpoint will redirect the user to the `/dashboard` page.

src/pages/signin.astro

```astro
---
import { app } from "../firebase/server";
import { getAuth } from "firebase-admin/auth";
import Layout from "../layouts/Layout.astro";


/* Check if the user is authenticated */
const auth = getAuth(app);
if (Astro.cookies.has("__session")) {
  const sessionCookie = Astro.cookies.get("__session")!.value;
  const decodedCookie = await auth.verifySessionCookie(sessionCookie);
  if (decodedCookie) {
    return Astro.redirect("/dashboard");
  }
}
---


<Layout title="Sign in">
  <h1>Sign in</h1>
  <p>New here? <a href="/register">Create an account</a></p>
  <form action="/api/auth/signin" method="post">
    <label for="email" for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Login</button>
  </form>
</Layout>
<script>
  import {
    getAuth,
    inMemoryPersistence,
    signInWithEmailAndPassword,
  } from "firebase/auth";
  import { app } from "../firebase/client";


  const auth = getAuth(app);
  // This will prevent the browser from storing session data
  auth.setPersistence(inMemoryPersistence);


  const form = document.querySelector("form") as HTMLFormElement;
  form.addEventListener("submit", async (e) => {
    e.preventDefault();
    const formData = new FormData(form);
    const email = formData.get("email")?.toString();
    const password = formData.get("password")?.toString();


    if (!email || !password) {
      return;
    }
    const userCredential = await signInWithEmailAndPassword(
      auth,
      email,
      password
    );
    const idToken = await userCredential.user.getIdToken();
    const response = await fetch("/api/auth/signin", {
      method: "GET",
      headers: {
        Authorization: `Bearer ${idToken}`,
      },
    });


    if (response.redirected) {
      window.location.assign(response.url);
    }
  });
</script>
```

`src/pages/dashboard.astro` will verify the user’s session cookie using the Firebase server app. If the user is not authenticated, the page will redirect the user to the `/signin` page.

The example page below display the user’s name and a button to sign out. Clicking the button will send a `GET` request to the `/api/auth/signout` endpoint.

The endpoint will delete the user’s session cookie and redirect the user to the `/signin` page.

src/pages/dashboard.astro

```astro
---
import { app } from "../firebase/server";
import { getAuth } from "firebase-admin/auth";
import Layout from "../layouts/Layout.astro";


const auth = getAuth(app);


/* Check current session */
if (!Astro.cookies.has("__session")) {
  return Astro.redirect("/signin");
}
const sessionCookie = Astro.cookies.get("__session")!.value;
const decodedCookie = await auth.verifySessionCookie(sessionCookie);
const user = await auth.getUser(decodedCookie.uid);


if (!user) {
  return Astro.redirect("/signin");
}
---


<Layout title="dashboard">
  <h1>Welcome {user.displayName}</h1>
  <p>We are happy to see you here</p>
  <form action="/api/auth/signout">
    <button type="submit">Sign out</button>
  </form>
</Layout>
```

### Adding OAuth providers

[Section titled “Adding OAuth providers”](#adding-oauth-providers)

To add OAuth providers to your app, you need to enable them in the Firebase console.

In the Firebase console, go to the **Authentication** section and click on the **Sign-in method** tab. Then, click on the **Add a new provider** button and enable the providers you want to use.

The example below uses the **Google** provider.

Edit the `signin.astro` page to add:

* a button to sign in with Google underneath the existing form
* an event listener on the button to handle the sign in process in the existing `<script>`.

src/pages/signin.astro

```diff
---
import { app } from "../firebase/server";
import { getAuth } from "firebase-admin/auth";
import Layout from "../layouts/Layout.astro";


/* Check if the user is authenticated */
const auth = getAuth(app);
if (Astro.cookies.has("__session")) {
  const sessionCookie = Astro.cookies.get("__session")!.value;
  const decodedCookie = await auth.verifySessionCookie(sessionCookie);
  if (decodedCookie) {
    return Astro.redirect("/dashboard");
  }
}
---


<Layout title="Sign in">
  <h1>Sign in</h1>
  <p>New here? <a href="/register">Create an account</a></p>
  <form action="/api/auth/signin" method="post">
    <label for="email" for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Login</button>
  </form>
  <button id="google">Sign in with Google</button>
</Layout>
<script>
  import {
    getAuth,
    inMemoryPersistence,
    signInWithEmailAndPassword,
+    GoogleAuthProvider,
+    signInWithPopup,
  } from "firebase/auth";
  import { app } from "../firebase/client";


  const auth = getAuth(app);
  auth.setPersistence(inMemoryPersistence);


  const form = document.querySelector("form") as HTMLFormElement;
  form.addEventListener("submit", async (e) => {
    e.preventDefault();
    const formData = new FormData(form);
    const email = formData.get("email")?.toString();
    const password = formData.get("password")?.toString();


    if (!email || !password) {
      return;
    }
    const userCredential = await signInWithEmailAndPassword(
      auth,
      email,
      password
    );
    const idToken = await userCredential.user.getIdToken();
    const response = await fetch("/api/auth/signin", {
      headers: {
        Authorization: `Bearer ${idToken}`,
      },
    });


    if (response.redirected) {
      window.location.assign(response.url);
    }
  });


  +const googleSignin = document.querySelector("#google") as HTMLButtonElement;
  +googleSignin.addEventListener("click", async () => {
    +const provider = new GoogleAuthProvider();
    +const userCredential = await signInWithPopup(auth, provider);
    +const idToken = await userCredential.user.getIdToken();
    +const res = await fetch("/api/auth/signin", {
      headers: {
        Authorization: `Bearer ${idToken}`,
      },
    });


    +if (res.redirected) {
      +window.location.assign(res.url);
+    }
+  });
</script>
```

When clicked, the Google sign in button will open a popup window to sign in with Google. Once the user signs in, it will send a `POST` request to the `/api/auth/signin` endpoint with the ID token generated by OAuth provider.

The endpoint will verify the ID token and create a new session cookie for the user. Then, the endpoint will redirect the user to the `/dashboard` page.

## Connecting to Firestore database

[Section titled “Connecting to Firestore database”](#connecting-to-firestore-database)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites-2)

* An Astro project initialized with Firebase as described in the [Initializing Firebase in Astro](#initializing-firebase-in-astro) section.

* A Firebase project with a Firestore database. You can follow the [Firebase documentation to create a new project and set up a Firestore database](https://firebase.google.com/docs/firestore/quickstart).

In this recipe, the Firestore collection will be called **friends** and will contain documents with the following fields:

* `id`: autogenerated by Firestore
* `name`: a string field
* `age`: a number field
* `isBestFriend`: a boolean field

### Creating the server endpoints

[Section titled “Creating the server endpoints”](#creating-the-server-endpoints)

Create two new files in a new directory `src/pages/api/friends/`: `index.ts` and `[id].ts`. These will create two server endpoints to interact with the Firestore database in the following ways:

* `POST /api/friends`: to create a new document in the friends collection.
* `POST /api/friends/:id`: to update a document in the friends collection.
* `DELETE /api/friends/:id`: to delete a document in the friends collection.

`index.ts` will contain the code to create a new document in the friends collection:

src/pages/api/friends/index.ts

```ts
import type { APIRoute } from "astro";
import { app } from "../../../firebase/server";
import { getFirestore } from "firebase-admin/firestore";


export const POST: APIRoute = async ({ request, redirect }) => {
  const formData = await request.formData();
  const name = formData.get("name")?.toString();
  const age = formData.get("age")?.toString();
  const isBestFriend = formData.get("isBestFriend") === "on";


  if (!name || !age) {
    return new Response("Missing required fields", {
      status: 400,
    });
  }
  try {
    const db = getFirestore(app);
    const friendsRef = db.collection("friends");
    await friendsRef.add({
      name,
      age: parseInt(age),
      isBestFriend,
    });
  } catch (error) {
    return new Response("Something went wrong", {
      status: 500,
    });
  }
  return redirect("/dashboard");
};
```

Note

This is a basic implementation of the `friends` endpoint. You can add more logic to this endpoint to suit your needs.

`[id].ts` will contain the code to update and delete a document in the friends collection:

src/pages/api/friends/\[id].ts

```ts
import type { APIRoute } from "astro";
import { app } from "../../../firebase/server";
import { getFirestore } from "firebase-admin/firestore";


const db = getFirestore(app);
const friendsRef = db.collection("friends");


export const POST: APIRoute = async ({ params, redirect, request }) => {
  const formData = await request.formData();
  const name = formData.get("name")?.toString();
  const age = formData.get("age")?.toString();
  const isBestFriend = formData.get("isBestFriend") === "on";


  if (!name || !age) {
    return new Response("Missing required fields", {
      status: 400,
    });
  }


  if (!params.id) {
    return new Response("Cannot find friend", {
      status: 404,
    });
  }


  try {
    await friendsRef.doc(params.id).update({
      name,
      age: parseInt(age),
      isBestFriend,
    });
  } catch (error) {
    return new Response("Something went wrong", {
      status: 500,
    });
  }
  return redirect("/dashboard");
};


export const DELETE: APIRoute = async ({ params, redirect }) => {
  if (!params.id) {
    return new Response("Cannot find friend", {
      status: 404,
    });
  }


  try {
    await friendsRef.doc(params.id).delete();
  } catch (error) {
    return new Response("Something went wrong", {
      status: 500,
    });
  }
  return redirect("/dashboard");
};
```

Note

This is a basic implementation of the `friends/:id` endpoint. You can add more logic to this endpoint to suit your needs.

After creating server endpoints for Firestore, your project directory should now include these new files:

* src

  * env.d.ts

  * firebase

    * client.ts
    * server.ts

  * pages

    * api

      * friends

        * **index.ts**
        * **\[id].ts**

* .env

* astro.config.mjs

* package.json

### Creating pages

[Section titled “Creating pages”](#creating-pages-1)

Create the pages that will use the Firestore endpoints:

* `src/pages/add.astro` - will contain a form to add a new friend.
* `src/pages/edit/[id].astro` - will contain a form to edit a friend and a button to delete a friend.
* `src/pages/friend/[id].astro` - will contain the details of a friend.
* `src/pages/dashboard.astro` - will display a list of friends.

#### Add a new record

[Section titled “Add a new record”](#add-a-new-record)

The example `src/pages/add.astro` below includes a form that will send a `POST` request to the `/api/friends` endpoint. This endpoint will create a new friend using the data from the form and then will redirect the user to the `/dashboard` page.

src/pages/add.astro

```astro
---
import Layout from "../layouts/Layout.astro";
---


<Layout title="Add a new friend">
  <h1>Add a new friend</h1>
  <form method="post" action="/api/friends">
    <label for="name">Name</label>
    <input type="text" id="name" name="name" />
    <label for="age">Age</label>
    <input type="number" id="age" name="age" />
    <label for="isBestFriend">Is best friend?</label>
    <input type="checkbox" id="isBestFriend" name="isBestFriend" />
    <button type="submit">Add friend</button>
  </form>
</Layout>
```

#### Edit or Delete a record

[Section titled “Edit or Delete a record”](#edit-or-delete-a-record)

`src/pages/edit/[id].astro` will contain a form to edit a friend data and a button to delete a friend. On submit, this page will send a `POST` request to the `/api/friends/:id` endpoint to update a friend data.

If the user clicks the delete button, this page will send a `DELETE` request to the `/api/friends/:id` endpoint to delete a friend.

src/pages/edit/\[id].astro

```astro
---
import Layout from "../../layouts/Layout.astro";
import { app } from "../../firebase/server";
import { getFirestore } from "firebase-admin/firestore";


interface Friend {
  name: string;
  age: number;
  isBestFriend: boolean;
}


const { id } = Astro.params;


if (!id) {
  return Astro.redirect("/404");
}


const db = getFirestore(app);
const friendsRef = db.collection("friends");
const friendSnapshot = await friendsRef.doc(id).get();


if (!friendSnapshot.exists) {
  return Astro.redirect("/404");
}


const friend = friendSnapshot.data() as Friend;
---


<Layout title="Edit {friend.name}">
  <h1>Edit {friend.name}</h1>
  <p>Here you can edit or delete your friend's data.</p>
  <form method="post" action={`/api/friends/${id}`}>
    <label for="name">Name</label>
    <input type="text" id="name" name="name" value={friend.name} />
    <label for="age">Age</label>
    <input type="number" id="age" name="age" value={friend.age} />
    <label for="isBestFriend">Is best friend?</label>
    <input
      type="checkbox"
      id="isBestFriend"
      name="isBestFriend"
      checked={friend.isBestFriend}
    />
    <button type="submit">Edit friend</button>
  </form>
  <button type="button" id="delete-document">Delete</button>
</Layout>
<script>
  const deleteButton = document.getElementById(
    "delete-document"
  ) as HTMLButtonElement;
  const url = document.querySelector("form")?.getAttribute("action") as string;
  deleteButton.addEventListener("click", async () => {
    const response = await fetch(url, {
      method: "DELETE",
    });
    if (response.redirected) {
      window.location.assign(response.url);
    }
  });
</script>
```

#### Display an individual record

[Section titled “Display an individual record”](#display-an-individual-record)

`src/pages/friend/[id].astro` will display the details of a friend.

src/pages/friend/\[id].astro

```astro
---
import Layout from "../../layouts/Layout.astro";
import { app } from "../../firebase/server";
import { getFirestore } from "firebase-admin/firestore";


interface Friend {
  name: string;
  age: number;
  isBestFriend: boolean;
}


const { id } = Astro.params;


if (!id) {
  return Astro.redirect("/404");
}


const db = getFirestore(app);
const friendsRef = db.collection("friends");
const friendSnapshot = await friendsRef.doc(id).get();


if (!friendSnapshot.exists) {
  return Astro.redirect("/404");
}


const friend = friendSnapshot.data() as Friend;
---


<Layout title={friend.name}>
  <h1>{friend.name}</h1>
  <p>Age: {friend.age}</p>
  <p>Is best friend: {friend.isBestFriend ? "Yes" : "No"}</p>
</Layout>
```

#### Display a list of records with an edit button

[Section titled “Display a list of records with an edit button”](#display-a-list-of-records-with-an-edit-button)

Finally, `src/pages/dashboard.astro` will display a list of friends. Each friend will have a link to their details page and an edit button that will redirect the user to the edit page.

src/pages/dashboard.astro

```astro
---
import { app } from "../firebase/server";
import { getFirestore } from "firebase-admin/firestore";
import Layout from "../layouts/Layout.astro";


interface Friend {
  id: string;
  name: string;
  age: number;
  isBestFriend: boolean;
}


const db = getFirestore(app);
const friendsRef = db.collection("friends");
const friendsSnapshot = await friendsRef.get();
const friends = friendsSnapshot.docs.map((doc) => ({
  id: doc.id,
  ...doc.data(),
})) as Friend[];
---


<Layout title="My friends">
  <h1>Friends</h1>
  <ul>
    {
      friends.map((friend) => (
        <li>
          <a href={`/friend/${friend.id}`}>{friend.name}</a>
          <span>({friend.age})</span>
          <strong>{friend.isBestFriend ? "Bestie" : "Friend"}</strong>
          <a href={`/edit/${friend.id}`}>Edit</a>
        </li>
      ))
    }
  </ul>
</Layout>
```

After creating all the pages, you should have the following file structure:

* src

  * env.d.ts

  * firebase

    * client.ts
    * server.ts

  * pages

    * dashboard.astro

    * add.astro

    * edit

      * \[id].astro

    * friend

      * \[id].astro

    * api

      * friends

        * index.ts
        * \[id].ts

* .env

* astro.config.mjs

* package.json

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Astro and Firebase SSR app example](https://github.com/kevinzunigacuellar/astro-firebase)
* [Using Firebase Realtime Database in Astro with Vue: A Step-by-Step Guide](https://www.launchfa.st/blog/vue-astro-firebase-realtime-database)

# Neon Postgres & Astro

> Add a serverless Postgres database to your Astro project with Neon

[Neon](https://neon.tech) is a fully managed serverless Postgres database. It separates storage and compute to offer autoscaling, branching, and bottomless storage.

## Adding Neon to your Astro project

[Section titled “Adding Neon to your Astro project”](#adding-neon-to-your-astro-project)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A [Neon](https://console.neon.tech/signup) account with a created project
* Neon database connection string
* An Astro project with [on-demand rendering (SSR)](/en/guides/on-demand-rendering/) enabled

### Environment configuration

[Section titled “Environment configuration”](#environment-configuration)

To use Neon with Astro, you will need to set a Neon environment variable. Create or edit the `.env` file in your project root, and add the following code, replacing your own project details:

.env

```ini
NEON_DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require"
```

For better TypeScript support, define environment variables in a `src/env.d.ts` file:

src/env.d.ts

```typescript
interface ImportMetaEnv {
  readonly NEON_DATABASE_URL: string;
}


interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

Learn more about [environment variables](/en/guides/environment-variables/) and `.env` files in Astro.

### Installing dependencies

[Section titled “Installing dependencies”](#installing-dependencies)

Install the `@neondatabase/serverless` package to connect to Neon:

```bash
npm install @neondatabase/serverless
```

### Creating a Neon client

[Section titled “Creating a Neon client”](#creating-a-neon-client)

Create a new file `src/lib/neon.ts` with the following code to initialize your Neon client:

src/lib/neon.ts

```typescript
import { neon } from '@neondatabase/serverless';


export const sql = neon(import.meta.env.NEON_DATABASE_URL);
```

## Querying your Neon database

[Section titled “Querying your Neon database”](#querying-your-neon-database)

You can now use the Neon client to query your database from any `.astro` component. The following example fetches the current time from the Postgres database:

src/pages/index.astro

```astro
---
import { sql } from '../lib/neon';


const response =  await  sql`SELECT NOW() as current_time`;
const currentTime = response[0].current_time;
---


<h1>Current Time</h1>
<p>The time is: {currentTime}</p>
```

## Database branching with Neon

[Section titled “Database branching with Neon”](#database-branching-with-neon)

Neon’s branching feature lets you create copies of your database for development or testing. Use this in your Astro project by creating different environment variables for each branch:

.env.development

```ini
NEON_DATABASE_URL=your_development_branch_url
```

.env.production

```ini
NEON_DATABASE_URL=your_production_branch_url
```

## Resources

[Section titled “Resources”](#resources)

* [Neon documentation](https://neon.tech/docs/introduction)
* [Neon serverless driver GitHub](https://github.com/neondatabase/serverless)
* [Connect an Astro site or application to Neon Postgres](https://neon.tech/docs/guides/astro)

# Prisma Postgres & Astro

> Add a serverless Postgres database to your Astro project with Prisma Postgres

[Prisma Postgres](https://www.prisma.io/) is a fully managed, serverless Postgres database built for modern web apps.

## Connect with Prisma ORM (Recommended)

[Section titled “Connect with Prisma ORM (Recommended)”](#connect-with-prisma-orm-recommended)

[Prisma ORM](https://www.prisma.io/orm) is the recommended way to connect to your Prisma Postgres database. It provides type-safe queries, migrations, and global performance.

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* An Astro project with an adapter installed to enable [on-demand rendering (SSR)](/en/guides/on-demand-rendering/).

### Install dependencies and initialize Prisma

[Section titled “Install dependencies and initialize Prisma”](#install-dependencies-and-initialize-prisma)

Run the following commands to install the necessary Prisma dependencies:

```bash
npm install prisma tsx --save-dev
npm install @prisma/adapter-pg @prisma/client
```

Once installed, initialize Prisma in your project with the following command:

```bash
npx prisma init --db --output ./generated
```

You’ll need to answer a few questions while setting up your Prisma Postgres database. Select the region closest to your location and a memorable name for your database, like “My Astro Project.”

This will create:

* A `prisma/` directory with a `schema.prisma` file
* A `.env` file with a `DATABASE_URL` already set

### Define a Model

[Section titled “Define a Model”](#define-a-model)

Even if you don’t need any specific data models yet, Prisma requires at least one model in the schema in order to generate a client and apply migrations.

The following example defines a `Post` model as a placeholder. Add the model to your schema to get started. You can safely delete or replace it later with models that reflect your actual data.

prisma/schema.prisma

```diff
generator client {
  provider = "prisma-client"
  output   = "./generated"
}


datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}


+model Post {
  +id        Int     @id @default(autoincrement())
  +title     String
  +content   String?
  +published Boolean @default(false)
+}
```

Learn more about configuring your Prisma ORM setup in the [Prisma schema reference](https://www.prisma.io/docs/concepts/components/prisma-schema).

### Generate client

[Section titled “Generate client”](#generate-client)

Run the following command to generate the Prisma Client from your schema:

```bash
npx prisma generate
```

### Generate migration files

[Section titled “Generate migration files”](#generate-migration-files)

Run the following command to create the database tables and generate the Prisma Client from your schema. This will also create a `prisma/migrations/` directory with migration history files.

```bash
npx prisma migrate dev --name init
```

### Create a Prisma Client

[Section titled “Create a Prisma Client”](#create-a-prisma-client)

Inside of `/src/lib`, create a `prisma.ts` file. This file will initialize and export your Prisma Client instance so you can query your database throughout your Astro project.

src/lib/prisma.ts

```typescript
import { PrismaPg } from '@prisma/adapter-pg';
import { PrismaClient } from '../../prisma/generated/client';


const connectionString = import.meta.env.DATABASE_URL;
const adapter = new PrismaPg({ connectionString });
const prisma = new PrismaClient({ adapter });


export default prisma;
```

### Querying and displaying data

[Section titled “Querying and displaying data”](#querying-and-displaying-data)

The following example shows fetching only your published posts with the Prisma Client sorted by `id`, and then displaying titles and post content in your Astro template:

src/pages/posts.astro

```astro
---
import prisma from '../lib/prisma';


const posts = await prisma.post.findMany({
  where: { published: true },
  orderBy: { id: 'desc' }
});
---


<html>
  <head>
    <title>Published Posts</title>
  </head>
  <body>
    <h1>Published Posts</h1>
    <ul>
      {posts.map((post) => (
        <li>
          <h2>{post.title}</h2>
          {post.content && <p>{post.content}</p>}
        </li>
      ))}
    </ul>
  </body>
</html>
```

It is best practice to handle queries in an API route. For more information on how to use Prisma ORM in your Astro project, see the [Astro + Prisma ORM Guide](https://www.prisma.io/docs/guides/astro).

## Connect with Other ORMs and Libraries

[Section titled “Connect with Other ORMs and Libraries”](#connect-with-other-orms-and-libraries)

You can connect to Prisma Postgres via direct TCP using any other ORM, database library, or tool of your choice. Create a direct connection string in your Prisma Console to get started.

### Prerequisites

[Section titled “Prerequisites”](#prerequisites-1)

* An Astro project with an adapter installed to enable [on-demand rendering (SSR)](/en/guides/on-demand-rendering/).
* A [Prisma Postgres](https://pris.ly/ppg) database with a TCP enabled connection string

### Install dependencies

[Section titled “Install dependencies”](#install-dependencies)

This example uses [`pg`, a PostgreSQL client for Node.js](https://github.com/brianc/node-postgres) to make a direct TCP connection.

Run the following command to install the `pg` package:

```bash
npm install pg
```

### Query your database client

[Section titled “Query your database client”](#query-your-database-client)

Provide your connection string to the `pg` client to communicate with your SQL server and fetch data from your database.

The following example of creating a table and inserting data can be used to validate your query URL and TCP connection:

src/pages/index.astro

```astro
---
import { Client } from 'pg';
const client = new Client({
  connectionString: import.meta.env.DATABASE_URL,
  ssl: { rejectUnauthorized: false }
});
await client.connect();


await client.query(`
  CREATE TABLE IF NOT EXISTS posts (
    id SERIAL PRIMARY KEY,
    title TEXT UNIQUE,
    content TEXT
  );


  INSERT INTO posts (title, content)
  VALUES ('Hello', 'World')
  ON CONFLICT (title) DO NOTHING;
`);


const { rows } = await client.query('SELECT * FROM posts');
await client.end();
---


<h1>Posts</h1>
<p>{rows[0].title}: {rows[0].content}</p>
```

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Astro + Prisma ORM Guide](https://www.prisma.io/docs/guides/astro)

# Monitor your Astro Site with Sentry

> How to monitor your Astro site with Sentry

[Sentry](https://sentry.io) offers a comprehensive application monitoring and error tracking service designed to help developers identify, diagnose, and resolve issues in real-time.

Read more on our blog about [Astro’s partnership with Sentry](https://astro.build/blog/sentry-official-monitoring-partner/) and Sentry’s Spotlight dev toolbar app that brings a rich debug overlay into your Astro development environment. Spotlight shows errors, traces, and important context right in your browser during local development.

Sentry’s Astro SDK enables automatic reporting of errors and tracing data in your Astro application.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

A full list of prerequisites can be found in [the Sentry guide for Astro](https://docs.sentry.io/platforms/javascript/guides/astro/#prerequisites).

## Install

[Section titled “Install”](#install)

Sentry captures data by using an SDK within your application’s runtime.

Install the SDK by running the following command for the package manager of your choice in the Astro CLI:

* npm

  ```shell
  npx astro add @sentry/astro
  ```

* pnpm

  ```shell
  pnpm astro add @sentry/astro
  ```

* Yarn

  ```shell
  yarn astro add @sentry/astro
  ```

The astro CLI installs the SDK package and adds the Sentry integration to your `astro.config.mjs` file.

## Configure

[Section titled “Configure”](#configure)

To configure the Sentry integration, you need to provide the following credentials in your `astro.config.mjs` file.

1. **Client key (DSN)** - You can find the DSN in your Sentry project settings under *Client keys (DSN)*.
2. **Project name** - You can find the project name in your Sentry project settings under *General settings*.
3. **Auth token** - You can create an auth token in your Sentry organization settings under *Auth tokens*.

Note

If you are creating a new Sentry project, select Astro as your platform to get all the necessary information to configure the SDK.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
+import sentry from '@sentry/astro';


export default defineConfig({
  integrations: [
    +sentry({
+      dsn: 'https://examplePublicKey@o0.ingest.sentry.io/0',
+      sourceMapsUploadOptions: {
+        project: 'example-project',
+        authToken: process.env.SENTRY_AUTH_TOKEN,
+      },
+    }),
  ],
});
```

Once you’ve configured your `sourceMapsUploadOptions` and added your `dsn`, the SDK will automatically capture and send errors and performance events to Sentry.

## Test your setup

[Section titled “Test your setup”](#test-your-setup)

Add the following `<button>` element to one of your `.astro` pages. This will allow you to manually trigger an error so you can test the error reporting process.

src/pages/index.astro

```astro
<button onclick="throw new Error('This is a test error')">Throw test error</button>
```

To view and resolve the recorded error, log into [sentry.io](https://sentry.io/) and open your project.

# Supabase & Astro

> Add a backend to your project with Supabase

[Supabase](https://supabase.com/) is an open source Firebase alternative. It provides a Postgres database, authentication, edge functions, realtime subscriptions, and storage.

## Initializing Supabase in Astro

[Section titled “Initializing Supabase in Astro”](#initializing-supabase-in-astro)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A Supabase project. If you don’t have one, you can sign up for free at [supabase.com](https://supabase.com/) and create a new project.

* An Astro project with [`output: 'server'` for on-demand rendering](/en/guides/on-demand-rendering/) enabled.

* Supabase credentials for your project. You can find these in the **Settings > API** tab of your Supabase project.

  * `SUPABASE_URL`: The URL of your Supabase project.
  * `SUPABASE_ANON_KEY`: The anonymous key for your Supabase project.

### Adding Supabase credentials

[Section titled “Adding Supabase credentials”](#adding-supabase-credentials)

To add your Supabase credentials to your Astro project, add the following to your `.env` file:

.env

```ini
SUPABASE_URL=YOUR_SUPABASE_URL
SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

Now, these environment variables are available in your project.

If you would like to have IntelliSense for your environment variables, edit or create the `env.d.ts` in your `src/` directory and add the following:

src/env.d.ts

```ts
interface ImportMetaEnv {
  readonly SUPABASE_URL: string
  readonly SUPABASE_ANON_KEY: string
}


interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

Tip

Read more about [environment variables](/en/guides/environment-variables/) and `.env` files in Astro.

Your project should now include these files:

* src/

  * **env.d.ts**

* **.env**

* astro.config.mjs

* package.json

### Installing dependencies

[Section titled “Installing dependencies”](#installing-dependencies)

To connect to Supabase, you will need to install `@supabase/supabase-js` in your project.

* npm

  ```shell
  npm install @supabase/supabase-js
  ```

* pnpm

  ```shell
  pnpm add @supabase/supabase-js
  ```

* Yarn

  ```shell
  yarn add @supabase/supabase-js
  ```

Next, create a folder named `lib` in your `src/` directory. This is where you will add your Supabase client.

In `supabase.ts`, add the following to initialize your Supabase client:

src/lib/supabase.ts

```ts
import { createClient } from "@supabase/supabase-js";


export const supabase = createClient(
  import.meta.env.SUPABASE_URL,
  import.meta.env.SUPABASE_ANON_KEY,
);
```

Now, your project should include these files:

* src/

  * lib/

    * **supabase.ts**

  * env.d.ts

* .env

* astro.config.mjs

* package.json

## Adding authentication with Supabase

[Section titled “Adding authentication with Supabase”](#adding-authentication-with-supabase)

Supabase provides authentication out of the box. It supports email/password authentication and OAuth authentication with many providers including GitHub, Google, and several others.

### Prerequisites

[Section titled “Prerequisites”](#prerequisites-1)

* An Astro project [initialized with Supabase](#initializing-supabase-in-astro).
* A Supabase project with email/password authentication enabled. You can enable this in the **Authentication > Providers** tab of your Supabase project.

### Creating auth server endpoints

[Section titled “Creating auth server endpoints”](#creating-auth-server-endpoints)

To add authentication to your project, you will need to create a few server endpoints. These endpoints will be used to register, sign in, and sign out users.

* `POST /api/auth/register`: to register a new user.
* `POST /api/auth/signin`: to sign in a user.
* `GET /api/auth/signout`: to sign out a user.

Create these endpoints in the `src/pages/api/auth` directory of your project. If you are using `static` rendering mode, you must specify `export const prerender = false` at the top of each file to render these endpoints on demand. Your project should now include these new files:

* src/

  * lib/

    * supabase.ts

  * pages/

    * api/

      * auth/

        * **signin.ts**
        * **signout.ts**
        * **register.ts**

  * env.d.ts

* .env

* astro.config.mjs

* package.json

`register.ts` creates a new user in Supabase. It accepts a `POST` request with the an email and password. It then uses the Supabase SDK to create a new user.

src/pages/api/auth/register.ts

```ts
// With `output: 'static'` configured:
// export const prerender = false;
import type { APIRoute } from "astro";
import { supabase } from "../../../lib/supabase";


export const POST: APIRoute = async ({ request, redirect }) => {
  const formData = await request.formData();
  const email = formData.get("email")?.toString();
  const password = formData.get("password")?.toString();


  if (!email || !password) {
    return new Response("Email and password are required", { status: 400 });
  }


  const { error } = await supabase.auth.signUp({
    email,
    password,
  });


  if (error) {
    return new Response(error.message, { status: 500 });
  }


  return redirect("/signin");
};
```

`signin.ts` signs in a user. It accepts a `POST` request with the an email and password. It then uses the Supabase SDK to sign in the user.

src/pages/api/auth/signin.ts

```ts
// With `output: 'static'` configured:
// export const prerender = false;
import type { APIRoute } from "astro";
import { supabase } from "../../../lib/supabase";


export const POST: APIRoute = async ({ request, cookies, redirect }) => {
  const formData = await request.formData();
  const email = formData.get("email")?.toString();
  const password = formData.get("password")?.toString();


  if (!email || !password) {
    return new Response("Email and password are required", { status: 400 });
  }


  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });


  if (error) {
    return new Response(error.message, { status: 500 });
  }


  const { access_token, refresh_token } = data.session;
  cookies.set("sb-access-token", access_token, {
    path: "/",
  });
  cookies.set("sb-refresh-token", refresh_token, {
    path: "/",
  });
  return redirect("/dashboard");
};
```

`signout.ts` signs out a user. It accepts a `GET` request and removes the user’s access and refresh tokens.

src/pages/api/auth/signout.ts

```ts
// With `output: 'static'` configured:
// export const prerender = false;
import type { APIRoute } from "astro";


export const GET: APIRoute = async ({ cookies, redirect }) => {
  cookies.delete("sb-access-token", { path: "/" });
  cookies.delete("sb-refresh-token", { path: "/" });
  return redirect("/signin");
};
```

### Creating auth pages

[Section titled “Creating auth pages”](#creating-auth-pages)

Now that you have created your server endpoints, create the pages that will use them.

* `src/pages/register`: contains a form to register a new user.
* `src/pages/signin`: contains a form to sign in a user.
* `src/pages/dashboard`: contains a page that is only accessible to authenticated users.

Create these pages in the `src/pages` directory. Your project should now include these new files:

* src/

  * lib/

    * supabase.ts

  * pages/

    * api/

      * auth/

        * signin.ts
        * signout.ts
        * register.ts

    * **register.astro**

    * **signin.astro**

    * **dashboard.astro**

  * env.d.ts

* .env

* astro.config.mjs

* package.json

`register.astro` contains a form to register a new user. It accepts an email and password and sends a `POST` request to `/api/auth/register`.

src/pages/register.astro

```astro
---
import Layout from "../layouts/Layout.astro";
---


<Layout title="Register">
  <h1>Register</h1>
  <p>Already have an account? <a href="/signin">Sign in</a></p>
  <form action="/api/auth/register" method="post">
    <label for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Register</button>
  </form>
</Layout>
```

`signin.astro` contains a form to sign in a user. It accepts an email and password and sends a `POST` request to `/api/auth/signin`. It also checks for the presence of the access and refresh tokens. If they are present, it redirects to the dashboard.

src/pages/signin.astro

```astro
---
import Layout from "../layouts/Layout.astro";


const { cookies, redirect } = Astro;


const accessToken = cookies.get("sb-access-token");
const refreshToken = cookies.get("sb-refresh-token");


if (accessToken && refreshToken) {
  return redirect("/dashboard");
}
---


<Layout title="Sign in">
  <h1>Sign in</h1>
  <p>New here? <a href="/register">Create an account</a></p>
  <form action="/api/auth/signin" method="post">
    <label for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Login</button>
  </form>
</Layout>
```

`dashboard.astro` contains a page that is only accessible to authenticated users. It checks for the presence of the access and refresh tokens. If they are not present or are invalid, it redirects to the sign in page.

src/pages/dashboard.astro

```astro
---
import Layout from "../layouts/Layout.astro";
import { supabase } from "../lib/supabase";


const accessToken = Astro.cookies.get("sb-access-token");
const refreshToken = Astro.cookies.get("sb-refresh-token");


if (!accessToken || !refreshToken) {
  return Astro.redirect("/signin");
}


let session;
try {
  session = await supabase.auth.setSession({
    refresh_token: refreshToken.value,
    access_token: accessToken.value,
  });
  if (session.error) {
    Astro.cookies.delete("sb-access-token", {
      path: "/",
    });
    Astro.cookies.delete("sb-refresh-token", {
      path: "/",
    });
    return Astro.redirect("/signin");
  }
} catch (error) {
  Astro.cookies.delete("sb-access-token", {
    path: "/",
  });
  Astro.cookies.delete("sb-refresh-token", {
    path: "/",
  });
  return Astro.redirect("/signin");
}


const email = session.data.user?.email;
---
<Layout title="dashboard">
  <h1>Welcome {email}</h1>
  <p>We are happy to see you here</p>
  <form action="/api/auth/signout">
    <button type="submit">Sign out</button>
  </form>
</Layout>
```

### Adding OAuth authentication

[Section titled “Adding OAuth authentication”](#adding-oauth-authentication)

To add OAuth authentication to your project, you will need to edit your Supabase client to enable authentication flow with `"pkce"`. You can read more about authentication flows in the [Supabase documentation](https://supabase.com/docs/guides/auth/server-side-rendering#understanding-the-authentication-flow).

src/lib/supabase.ts

```diff
import { createClient } from "@supabase/supabase-js";


export const supabase = createClient(
  import.meta.env.SUPABASE_URL,
  import.meta.env.SUPABASE_ANON_KEY,
  {
    auth: {
      flowType: "pkce",
    },
  },
);
```

Next, in the Supabase dashboard, enable the OAuth provider you would like to use. You can find the list of supported providers in the **Authentication > Providers** tab of your Supabase project.

The following example uses GitHub as the OAuth provider. To connect your project to GitHub, follow the steps in the [Supabase documentation](https://supabase.com/docs/guides/auth/social-login/auth-github).

Then, create a new server endpoint to handle the OAuth callback at `src/pages/api/auth/callback.ts`. This endpoint will be used to exchange the OAuth code for an access and refresh token.

src/pages/api/auth/callback.ts

```ts
import type { APIRoute } from "astro";
import { supabase } from "../../../lib/supabase";


export const GET: APIRoute = async ({ url, cookies, redirect }) => {
  const authCode = url.searchParams.get("code");


  if (!authCode) {
    return new Response("No code provided", { status: 400 });
  }


  const { data, error } = await supabase.auth.exchangeCodeForSession(authCode);


  if (error) {
    return new Response(error.message, { status: 500 });
  }


  const { access_token, refresh_token } = data.session;


  cookies.set("sb-access-token", access_token, {
    path: "/",
  });
  cookies.set("sb-refresh-token", refresh_token, {
    path: "/",
  });


  return redirect("/dashboard");
};
```

Next, edit the sign in page to include a new button to sign in with the OAuth provider. This button should send a `POST` request to `/api/auth/signin` with the `provider` set to the name of the OAuth provider.

src/pages/signin.astro

```diff
---
import Layout from "../layouts/Layout.astro";


const { cookies, redirect } = Astro;


const accessToken = cookies.get("sb-access-token");
const refreshToken = cookies.get("sb-refresh-token");


if (accessToken && refreshToken) {
  return redirect("/dashboard");
}
---


<Layout title="Sign in">
  <h1>Sign in</h1>
  <p>New here? <a href="/register">Create an account</a></p>
  <form action="/api/auth/signin" method="post">
    <label for="email">Email</label>
    <input type="email" name="email" id="email" />
    <label for="password">Password</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Login</button>
    <button value="github" name="provider" type="submit">Sign in with GitHub</button>
  </form>
</Layout>
```

Finally, edit the sign in server endpoint to handle the OAuth provider. If the `provider` is present, it will redirect to the OAuth provider. Otherwise, it will sign in the user with the email and password.

src/pages/api/auth/signin.ts

```diff
import type { APIRoute } from "astro";
import { supabase } from "../../../lib/supabase";
import type { Provider } from "@supabase/supabase-js";


export const POST: APIRoute = async ({ request, cookies, redirect }) => {
  const formData = await request.formData();
  const email = formData.get("email")?.toString();
  const password = formData.get("password")?.toString();
  const provider = formData.get("provider")?.toString();


  const validProviders = ["google", "github", "discord"];


  if (provider && validProviders.includes(provider)) {
    const { data, error } = await supabase.auth.signInWithOAuth({
      provider: provider as Provider,
      options: {
        redirectTo: "http://localhost:4321/api/auth/callback"
      },
    });


    if (error) {
      return new Response(error.message, { status: 500 });
    }


    return redirect(data.url);
  }


  if (!email || !password) {
    return new Response("Email and password are required", { status: 400 });
  }


  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });


  if (error) {
    return new Response(error.message, { status: 500 });
  }


  const { access_token, refresh_token } = data.session;
  cookies.set("sb-access-token", access_token, {
    path: "/",
  });
  cookies.set("sb-refresh-token", refresh_token, {
    path: "/",
  });
  return redirect("/dashboard");
};
```

After creating the OAuth callback endpoint and editing the sign in page and server endpoint, your project should have the following file structure:

* src/

  * lib/

    * supabase.ts

  * pages/

    * api/

      * auth/

        * signin.ts
        * signout.ts
        * register.ts
        * callback.ts

    * register.astro

    * signin.astro

    * dashboard.astro

  * env.d.ts

* .env

* astro.config.mjs

* package.json

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Getting into the holiday spirit with Astro, React, and Supabase](https://www.aleksandra.codes/astro-supabase)
* [Astro and Supabase auth demo](https://github.com/kevinzunigacuellar/astro-supabase)

# Turso & Astro

> Build locally with a SQLite file and deploy globally using Turso.

[Turso](https://turso.tech) is a distributed database built on libSQL, a fork of SQLite. It is optimized for low query latency, making it suitable for global applications.

## Initializing Turso in Astro

[Section titled “Initializing Turso in Astro”](#initializing-turso-in-astro)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* The [Turso CLI](https://docs.turso.tech/cli/introduction) installed and signed in
* A [Turso](https://turso.tech) Database with schema
* Your Database URL
* An Access Token

### Configure environment variables

[Section titled “Configure environment variables”](#configure-environment-variables)

Obtain your database URL using the following command:

```bash
turso db show <database-name> --url
```

Create an auth token for the database:

```bash
turso db tokens create <database-name>
```

Add the output from both commands above into your `.env` file at the root of your project. If this file does not exist, create one.

.env

```ini
TURSO_DATABASE_URL=libsql://...
TURSO_AUTH_TOKEN=
```

Caution

Do not use the `PUBLIC_` prefix when creating these private [environment variables](/en/guides/environment-variables/). This will expose these values on the client.

### Install LibSQL Client

[Section titled “Install LibSQL Client”](#install-libsql-client)

Install the `@libsql/client` to connect Turso to Astro:

* npm

  ```shell
  npm install @libsql/client
  ```

* pnpm

  ```shell
  pnpm add @libsql/client
  ```

* Yarn

  ```shell
  yarn add @libsql/client
  ```

### Initialize a new client

[Section titled “Initialize a new client”](#initialize-a-new-client)

Create a file `turso.ts` in the `src` folder and invoke `createClient`, passing it `TURSO_DATABASE_URL` and `TURSO_AUTH_TOKEN`:

src/turso.ts

```ts
import { createClient } from "@libsql/client/web";


export const turso = createClient({
  url: import.meta.env.TURSO_DATABASE_URL,
  authToken: import.meta.env.TURSO_AUTH_TOKEN,
});
```

## Querying your database

[Section titled “Querying your database”](#querying-your-database)

To access information from your database, import `turso` and [execute a SQL query](https://docs.turso.tech/sdk/ts/reference#simple-query) inside any `.astro` component.

The following example fetches all `posts` from your table, then displays a list of titles in a `<BlogIndex />` component:

src/components/BlogIndex.astro

```astro
---
import { turso } from '../turso'


const { rows } = await turso.execute('SELECT * FROM posts')
---


<ul>
  {rows.map((post) => (
    <li>{post.title}</li>
  ))}
</ul>
```

### SQL Placeholders

[Section titled “SQL Placeholders”](#sql-placeholders)

The `execute()` method can take [an object to pass variables to the SQL statement](https://docs.turso.tech/sdk/ts/reference#placeholders), such as `slug`, or pagination.

The following example fetches a single entry from the `posts` table `WHERE` the `slug` is the retrieved value from `Astro.params`, then displays the title of the post.

src/pages/index.astro

```astro
---
import { turso } from '../turso'


const { slug } = Astro.params


const { rows } = await turso.execute({
  sql: 'SELECT * FROM posts WHERE slug = ?',
  args: [slug!]
})
---


<h1>{rows[0].title}</h1>
```

## Turso Resources

[Section titled “Turso Resources”](#turso-resources)

* [Turso Docs](https://docs.turso.tech)
* [Turso on GitHub](https://github.com/tursodatabase)
* [Using Turso to serve a Server-side Rendered Astro blog’s content](https://blog.turso.tech/using-turso-to-serve-a-server-side-rendered-astro-blogs-content-58caa6188bd5)

# Xata & Astro

> Add a serverless database with full-text search to your project with Xata

[Xata](https://xata.io) is a **Serverless Data Platform** that combines the features of a relational database, a search engine, and an analytics engine by exposing a single consistent REST API.

## Adding a database with Xata

[Section titled “Adding a database with Xata”](#adding-a-database-with-xata)

### Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A [Xata](https://app.xata.io/signin) account with a created database. (You can use the sample database from the Web UI.)
* An Access Token (`XATA_API_KEY`).
* Your Database URL.

After you update and initialize the [Xata CLI](https://xata.io/docs/getting-started/installation), you will have your API token in your `.env` file and database URL defined.

By the end of the setup, you should have:

.env

```ini
XATA_API_KEY=hash_key


# Xata branch that will be used
# if there's not a xata branch with
# the same name as your git branch
XATA_BRANCH=main
```

And the `databaseURL` defined:

.xatarc

```ini
{
  "databaseUrl": "https://your-database-url"
}
```

### Environment configuration

[Section titled “Environment configuration”](#environment-configuration)

To have IntelliSense and type safety for your environment variables, edit or create the file `env.d.ts` in your `src/` directory:

src/env.d.ts

```ts
interface ImportMetaEnv {
  readonly XATA_API_KEY: string;
  readonly XATA_BRANCH?: string;
}


interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

Tip

Read more about [environment variables](/en/guides/environment-variables/) and `.env` files in Astro.

Using the code generation from the Xata CLI and choosing the TypeScript option, generated an instance of the SDK for you, with types tailored to your database schema. Additionally, `@xata.io/client` was added to your `package.json`.

Your Xata environment variables and database url were automatically pulled by the SDK instance, so there’s no more setup work needed.

Now, your project should have the following structure:

* src/

  * **xata.ts**
  * **env.d.ts**

* **.env**

* astro.config.mjs

* package.json

* **.xatarc**

## Create your queries

[Section titled “Create your queries”](#create-your-queries)

To query your posts, import and use `XataClient` class in a `.astro` file. The example below queries the first 50 posts from Xata’s Sample Blog Database.

src/pages/blog/index.astro

```astro
---
import { XataClient } from '../../xata';


const xata = new XataClient({
  apiKey: import.meta.env.XATA_API_KEY,
  branch: import.meta.env.XATA_BRANCH
});


const { records } = await xata.db.Posts.getPaginated({
  pagination: {
    size: 50
  }
})
---


<ul>
  {records.map((post) => (
    <li>{post.title}</li>
  ))}
</ul>
```

It’s important to note the SDK needs to be regenerated every time your schema changes. So, avoid making changes to the generated files the Xata CLI creates because once schema updates, your changes will be overwritten.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Xata Astro Starter](https://github.com/xataio/examples/tree/main/apps/getting-started-astro)
* [Xata Docs: Quick Start Guide](https://xata.io/docs/getting-started/quickstart-astro)


---

# E-commerce

> An introduction to adding e-commerce options to your Astro site

With Astro, you can build several e-commerce options, from checkout links to hosted payment pages to building an entire storefront using a payment service API.

## Payment processing overlays

[Section titled “Payment processing overlays”](#payment-processing-overlays)

Some payment processing services (e.g. [Lemon Squeezy](#lemon-squeezy), [Paddle](#paddle)) add a payment form to allow your customer to purchase from your site. These can be hosted overlays or embedded in a page on your site. These may offer some basic customization or site branding, and may be added to your Astro project as scripts, buttons, or external links.

### Lemon Squeezy

[Section titled “Lemon Squeezy”](#lemon-squeezy)

[Lemon Squeezy](https://www.lemonsqueezy.com/) is an all-in-one platform for payments and subscriptions with multi-currency support, global tax compliance, PayPal integration and more. It allows you to create and manage digital products and services through your account dashboard and provides product URLs for the checkout process.

The basic [Lemon.js JavaScript library](https://docs.lemonsqueezy.com/help/lemonjs/what-is-lemonjs) allows you to sell your Lemon Squeezy products with a checkout link.

#### Basic Usage

[Section titled “Basic Usage”](#basic-usage)

The following is an example of adding a Lemon Squeezy “Buy now” element to an Astro page. Clicking this link will open a checkout and allow the visitor to complete a single purchase.

1. Add the following `<script>` tag to your page `head` or `body`:

   src/pages/my-product-page.astro

   ```html
   <script src="https://app.lemonsqueezy.com/js/lemon.js" defer></script>
   ```

2. Create an anchor tag on the page linking to your product URL. Include the class `lemonsqueezy-button` to open a checkout overlay when clicked.

   src/pages/my-product-page.astro

   ```html
   <a class="lemonsqueezy-button" href="https://demo.lemonsqueezy.com/checkout/...">
     Buy Now
   </a>
   ```

#### Lemon.js

[Section titled “Lemon.js”](#lemonjs)

Lemon.js also provides additional behavior such as [programmatically opening overlays](https://docs.lemonsqueezy.com/help/lemonjs/opening-overlays) and [handling overlay events](https://docs.lemonsqueezy.com/help/lemonjs/handling-events).

Read the [Lemon Squeezy developer getting started guide](https://docs.lemonsqueezy.com/guides/developer-guide) for more information.

### Paddle

[Section titled “Paddle”](#paddle)

[Paddle](https://www.paddle.com/) is a billing solution for digital products and services. It handles payments, taxes, and subscription management through an overlay or inline checkout.

[Paddle.js](https://developer.paddle.com/paddlejs/overview) is a lightweight JavaScript library that lets you build rich, integrated subscription billing experiences using Paddle.

#### Basic Usage

[Section titled “Basic Usage”](#basic-usage-1)

The following is an example of adding a Paddle “Buy Now” element to an Astro page. Clicking this link will open a checkout and allow the visitor to complete a single purchase.

After your default payment link domain (your own website) is approved by Paddle, you can turn any element on your page into a trigger for a checkout overlay using HTML data attributes.

1. Add the following two `<script>` tags to your page `head` or `body`:

   src/pages/my-product-page.astro

   ```html
   <script src="https://cdn.paddle.com/paddle/v2/paddle.js"></script>
   <script type="text/javascript">
     Paddle.Setup({
       token: '7d279f61a3499fed520f7cd8c08' // replace with a client-side token
     });
   </script>
   ```

2. Turn any element on your page into a Paddle Checkout button by adding the `paddle_button` class:

   src/pages/my-product-page.astro

   ```html
   <a href="#" class="paddle_button">Buy Now</a>
   ```

3. Add a `data-items` attribute to specify your product’s Paddle `priceId` and `quantity`. You can also optionally pass additional [supported HTML data attributes](https://developer.paddle.com/paddlejs/html-data-attributes) to prefill data, handle checkout success, or style your button and checkout overlay:

   src/pages/my-product-page.astro

   ```html
   <a
     href="#"
     class="paddle_button"
     data-display-mode="overlay"
     data-theme="light"
     data-locale="en"
     data-success-url="https://example.com/thankyou"
     data-items='[
       {
         "priceId": "pri_01gs59hve0hrz6nyybj56z04eq",
         "quantity": 1
       }
     ]'
   >
     Buy now
   </a>
   ```

#### Paddle.js

[Section titled “Paddle.js”](#paddlejs)

Instead of passing HTML data attributes, you can send data to the checkout overlay using JavaScript for passing multiple attributes and even greater customization. You can also create upgrade workflows using an inline checkout.

Read more about [using Paddle.js to build an inline checkout](https://developer.paddle.com/build/checkout/build-branded-inline-checkout).

## Full-featured e-commerce solutions

[Section titled “Full-featured e-commerce solutions”](#full-featured-e-commerce-solutions)

For more customization over your site’s shopping cart and checkout process, you can connect a more fully-featured financial service provider (e.g. [Snipcart](#snipcart)) to your Astro project. These e-commerce platforms may also integrate with other third-party services for user account management, personalization, inventory and analytics.

### Snipcart

[Section titled “Snipcart”](#snipcart)

[Snipcart](https://snipcart.com/) is a powerful, developer-first HTML/JavaScript shopping cart platform.

Snipcart also allows you to integrate with third-party services such as shipping providers, enable webhooks for an advanced e-commerce integration between your shopping cart and other systems, choose from several payment gateways (e.g. Stripe, Paypal, and Square), customize email templates, and even provides live testing environments.

Tip

Want a pre-built Snipcart solution instead? Check out [`astro-snipcart`](https://astro-snipcart.vercel.app/), a fully functional Astro community template including an optional design system, ready for you to integrate with your existing Snipcart account.

#### Basic Usage

[Section titled “Basic Usage”](#basic-usage-2)

The following is an example of configuring a Snipcart checkout and adding button elements for “Add to cart” and “Check out now” to an Astro page. This will allow your visitors to add products to a cart without being immediately sent to a checkout page.

For complete instructions, including setting up your Snipcart store, please see [the Snipcart installation documentation](https://docs.snipcart.com/v3/setup/installation).

1. Add the script [as shown in the Snipcart installation instructions](https://docs.snipcart.com/v3/setup/installation) on your page after the `<body>` element.

   src/pages/my-product-page.astro

   ```html
   <body></body>
   <script>
     window.SnipcartSettings = {
       publicApiKey: "YOUR_API_KEY",
       loadStrategy: "on-user-interaction",
     };


     (function()...); // available from the Snipcart documentation
   </script>
   ```

2. Customize `window.SnipcartSettings` with any of the [available Snipcart settings](https://docs.snipcart.com/v3/setup/installation#settings) to control the behavior and appearance of your cart.

   src/pages/my-product-page.astro

   ```html
   <script>
     window.SnipcartSettings = {
       publicApiKey: "YOUR_API_KEY",
       loadStrategy: "manual",
       version: "3.7.1",
       addProductBehavior: "none",
       modalStyle: "side",
     };


     (function()...); // available from the Snipcart documentation
   </script>
   ```

3. Add `class="snipcart-add-item"` to any HTML element, such as a `<button>`, to add an item to the cart when clicked on. Also include any other data elements for [common Snipcart product attributes](https://docs.snipcart.com/v3/setup/products) such as price and description, and any optional fields.

   src/pages/my-product-page.astro

   ```html
   <button
     class="snipcart-add-item"
     data-item-id="astro-print"
     data-item-price="39.99"
     data-item-description="A framed print of the Astro logo."
     data-item-image="/assets/images/astro-print.jpg"
     data-item-name="Astro Print"
     data-item-custom1-name="Frame color"
     data-item-custom1-options="Brown|Silver[+10.00]|Gold[+20.00]"
     data-item-custom2-name="Delivery instructions"
     data-item-custom2-type="textarea"
   >
     Add to cart
   </button>
   ```

4. Add a Snipcart checkout button with the `snipcart-checkout` class to open the cart and allow guests to complete their purchase with a checkout modal.

   src/pages/my-product-page.astro

   ```html
   <button class="snipcart-checkout">Click here to checkout</button>
   ```

#### Snipcart JavaScript SDK

[Section titled “Snipcart JavaScript SDK”](#snipcart-javascript-sdk)

The [Snipcart JavaScript SDK](https://docs.snipcart.com/v3/sdk/basics) lets you configure, customize and manage your Snipcart cart programmatically.

This allows you to perform actions such as:

* Retrieve relevant information about the current Snipcart session and apply certain operations to the cart.
* Listen to incoming events and trigger callbacks dynamically.
* Listen to state changes and receive a full snapshot of the state of the cart.

See the [Snipcart documentation](https://docs.snipcart.com/v3/) for more information about all the options to integrate Snipcart with your Astro Project.

#### `astro-snipcart`

[Section titled “astro-snipcart”](#astro-snipcart)

There are two `astro-snipcart` community packages that can simplify using Snipcart.

* [`@lloydjatkinson/astro-snipcart` Astro template](https://astro-snipcart.vercel.app/): This Astro template includes an optional design system for a complete e-commerce solution out of the box. Learn more on its own extensive documentation site, including [the motivation behind building `astro-snipcart`](https://astro-snipcart.vercel.app/motivation) as providing a convenient, Astro-native way for you to interact with the Snipcart API.

* [`@Adammatthiesen/astro-snipcart` integration](https://github.com/Adammatthiesen/astro-snipcart): This integration was heavily inspired by the `astro-snipcart` theme and provides Astro components (or Vue components) that you can add to your existing Astro project for creating products, controlling the cart, and more. See the [full tutorial](https://matthiesen.xyz/blog/getting-started-with-my-astro-snipcart-addon) for more information.

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Hands-On Experience: eCommerce Store with Astro?](https://crystallize.com/blog/building-ecommerce-with-astro)
* [Collecting Payments with Stripe using Astro](https://zellwk.com/blog/stripe-astro-recipe/)


---

# Hi there!


This Markdown file creates a page at `your-domain.com/page-1/`


It probably isn't styled much, but Markdown does support:
- **bold** and _italics._
- lists
- [links](https://astro.build)
- <p>HTML elements</p>
- and more!
```

### Frontmatter `layout` property

[Section titled “Frontmatter layout property”](#frontmatter-layout-property)

To help with the limited functionality of individual Markdown pages, Astro provides a special frontmatter `layout` property which is a relative path to an Astro [Markdown layout component](/en/basics/layouts/#markdown-layouts). `layout` is not a special property when using [content collections](/en/guides/content-collections/) to query and render your Markdown content, and is not guaranteed to be supported outside of its intended use case.

If your Markdown file is located within `src/pages/`, create a layout component and add it in this layout property to provide a page shell around your Markdown content.

src/pages/posts/post-1.md

```markdown
---
layout: ../../layouts/BlogPostLayout.astro
title: Astro in brief
author: Himanshu
description: Find out what makes Astro awesome!
---
This is a post written in Markdown.
```

This layout component is a regular Astro component with [specific properties automatically available](/en/basics/layouts/#markdown-layout-props) through `Astro.props` for your Astro template. For example, you can access your Markdown file’s frontmatter properties through `Astro.props.frontmatter`:

src/layouts/BlogPostLayout.astro

```astro
---
const {frontmatter} = Astro.props;
---
<html>
  <head>
    <!-- ... -->
    <meta charset="utf-8"> // no longer added by default
  </head>
  <!-- ... -->
  <h1>{frontmatter.title}</h1>
  <h2>Post author: {frontmatter.author}</h2>
  <p>{frontmatter.description}</p>
  <slot /> <!-- Markdown content is injected here -->
  <!-- ... -->
</html>
```

When using the frontmatter `layout` property, you must include the `<meta charset="utf-8">` tag in your layout as Astro will no longer add it automatically. You can now also [style your Markdown](/en/guides/styling/#markdown-styling) in your layout component.

Learn more about [Markdown Layouts](/en/basics/layouts/#markdown-layouts).

## Fetching Remote Markdown

[Section titled “Fetching Remote Markdown”](#fetching-remote-markdown)

Astro’s internal Markdown processor is not available for processing remote Markdown.

To fetch remote Markdown for use in [content collections](/en/guides/content-collections/), you can [build a custom loader](/en/guides/content-collections/#custom-build-time-loaders) with access to a [`renderMarkdown()` function](/en/reference/content-loader-reference/#loadercontextrendermarkdown).

To fetch remote Markdown directly and render it to HTML, you will need to install and configure your own Markdown parser from NPM. This will not inherit from any of Astro’s built-in Markdown settings that you have configured.

Be sure that you understand these limitations before implementing this in your project, and consider fetching your remote Markdown using a content collections loader instead.

src/pages/remote-example.astro

```astro
---
// Example: Fetch Markdown from a remote API
// and render it to HTML, at runtime.
// Using "marked" (https://github.com/markedjs/marked)
import { marked } from 'marked';
const response = await fetch('https://raw.githubusercontent.com/wiki/adam-p/markdown-here/Markdown-Cheatsheet.md');
const markdown = await response.text();
const content = marked.parse(markdown);
---
<article set:html={content} />
```

# Image and video hosting with Astro

> How to use a hosted media service to add images and videos to Astro

Follow one of our guides to integrate images and videos from a hosted media service.

## Hosted Media Guides

[Section titled “Hosted Media Guides”](#hosted-media-guides)

* ![](/logos/cloudinary.svg)

  ### [Cloudinary](/en/guides/media/cloudinary/)

* ![](/logos/mux.svg)

  ### [Mux](/en/guides/media/mux/)

## Why use hosted media?

[Section titled “Why use hosted media?”](#why-use-hosted-media)

Hosted media helps individuals, teams, and organizations store, manage, optimize, and deliver their image and video assets with dedicated APIs from a central location.

This centralization can be useful, particularly when using a single source of truth for your assets between multiple web or mobile properties. This is important if you’re part of an organization that requires multiple teams to use the same assets, or are integrating into other content systems like a PIM (Product Information Manager) to connect your assets to products.

Image hosting services can transform and optimize your images, automatically delivering optimized versions for your visitors. These [remote images](/en/guides/images/#remote-images) can be used in Astro’s built-in `<Image />` and `<Picture />` components, and are available to all file types in your project, including Markdown, MDX, and UI Framework components.

Video hosting services like [Mux](/en/guides/media/mux/) can provide performant on-demand and live-streaming video delivery along with customizable video players, giving significant reliability and scaling benefits over handling local content. They will handle video transcoding, compression, and transformation to provide a smooth user experience. A platform like Mux may also include data analysis to help you understand your user engagement.

## Which hosted media systems work well with Astro?

[Section titled “Which hosted media systems work well with Astro?”](#which-hosted-media-systems-work-well-with-astro)

Much like when using a CMS, you’ll want to use hosted services that allow you to fetch and interact with your assets via an API or SDK. Some services may additionally include Astro-native components for displaying your images or videos.

## Can I use Astro without a hosted media system?

[Section titled “Can I use Astro without a hosted media system?”](#can-i-use-astro-without-a-hosted-media-system)

Yes! Astro provides built-in ways to [store images](/en/guides/images/#where-to-store-images), including support for referencing remote images.

However, there is no native video support in Astro, and we recommend choosing a service like [Mux](/en/guides/media/mux/) to handle the demands of optimizing and streaming video content.

# Cloudinary & Astro

> Add images and videos to your Astro project using Cloudinary

[Cloudinary](https://cloudinary.com) is an image and video platform and headless Digital Asset Manager (DAM) that lets you host assets and deliver them from their content delivery network (CDN).

When delivering from Cloudinary, you additionally get access to their Transformation API, giving you the ability to edit your assets with tools like background removal, dynamic cropping and resizing, and generative AI.

## Using Cloudinary in Astro

[Section titled “Using Cloudinary in Astro”](#using-cloudinary-in-astro)

Cloudinary supports a wide variety of SDKs that can be used depending on your Astro environment.

The [Cloudinary Astro SDK](https://astro.cloudinary.dev/) provides native Astro components, including image, video, and upload components, as well as a content loader that can be used with Astro content collections.

Alternatively, both the Cloudinary [Node.js SDK](https://cloudinary.com/documentation/node_integration) and [JavaScript SDK](https://cloudinary.com/documentation/javascript_integration) can be used to generate URLs for your images. The Node.js SDK can additionally make requests to the Cloudinary API including uploading assets, requesting resources, and running content analysis.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* An existing Astro project
* A Cloudinary account

## Installing Astro Cloudinary

[Section titled “Installing Astro Cloudinary”](#installing-astro-cloudinary)

Install the Cloudinary Astro SDK by running the appropriate command for your package manager:

* npm

  ```shell
  npm install astro-cloudinary
  ```

* pnpm

  ```shell
  pnpm add astro-cloudinary
  ```

* Yarn

  ```shell
  yarn add astro-cloudinary
  ```

## Configuring your account

[Section titled “Configuring your account”](#configuring-your-account)

Create a new `.env` file in the root of your project and add your Cloudinary credentials:

.env

```shell
PUBLIC_CLOUDINARY_CLOUD_NAME="<Your Cloud Name>"


// Only needed if using CldUploadWidget or cldAssetsLoader
PUBLIC_CLOUDINARY_API_KEY="<Your API Key>"
CLOUDINARY_API_SECRET="<Your API Secret>"
```

## Using Cloudinary images

[Section titled “Using Cloudinary images”](#using-cloudinary-images)

Add images in `.astro` components by passing image data (e.g. `src`, `width`, `alt`) to the `<CldImage>` component. This will automatically optimize your image and give you access to the Transformations API.

Component.astro

```jsx
---
import { CldImage } from 'astro-cloudinary';
---
<CldImage
  src="<Public ID>"
  width="<Width>"
  height="<Height>"
  alt="<Description>"
/>
```

See [Cloudinary’s `<CldImage>` documentation](https://astro.cloudinary.dev/cldimage/basic-usage) for more information.

## Using Cloudinary videos

[Section titled “Using Cloudinary videos”](#using-cloudinary-videos)

To add video to your `.astro` components, add the `<CldVideoPlayer>` and pass the appropriate properties. This component will automatically optimize and embed your video using the [Cloudinary Video Player](https://cloudinary.com/documentation/cloudinary_video_player).

Component.astro

```jsx
---
import { CldVideoPlayer } from 'astro-cloudinary';
---
<CldVideoPlayer
  src="<Public ID>"
  width="<Width>"
  height="<Height>"
/>
```

See [Cloudinary’s `<CldVideoPlayer>` documentation](https://astro.cloudinary.dev/cldvideoplayer/basic-usage) for more information.

## Enabling Cloudinary uploads

[Section titled “Enabling Cloudinary uploads”](#enabling-cloudinary-uploads)

To enable file uploading in your website or app’s UI, add the `<CldUploadWidget>` which will embed the [Cloudinary Upload Widget](https://cloudinary.com/documentation/upload_widget).

The following example creates a widget to allow unsigned uploads by passing an unsigned [Upload Preset](https://cloudinary.com/documentation/upload_presets):

Component.astro

```jsx
---
import { CldUploadWidget } from 'astro-cloudinary';
---
<CldUploadWidget uploadPreset="<Upload Preset>">
  <button>Upload</button>
</CldUploadWidget>
```

For signed uploads, you can find [a guide and example](https://astro.cloudinary.dev/clduploadwidget/signed-uploads) on the Astro Cloudinary docs.

See [Cloudinary’s `<CldUploadWidget>` documentation](https://astro.cloudinary.dev/clduploadwidget/basic-usage) for more information.

## Cloudinary content loader

[Section titled “Cloudinary content loader”](#cloudinary-content-loader)

The Cloudinary Astro SDK provides the `cldAssetsLoader` content loader to load Cloudinary assets for content collections.

To load a collection of images or videos, set `loader: cldAssetsLoader ({})` with a `folder`, if required:

config.ts

```jsx
import { defineCollection } from 'astro:content';
import { cldAssetsLoader } from 'astro-cloudinary/loaders';


export const collections = {
  assets: defineCollection({
    loader: cldAssetsLoader({
      folder: '<Folder>' // Optional, without loads root directory
    })
  }),
}
```

You can then use the [`getCollection()` or `getEntry()` query functions](/en/guides/content-collections/#querying-build-time-collections) to select one or many images or videos from your collection.

See [Cloudinary’s `cldAssetsLoader` documentation](https://astro.cloudinary.dev/cldassetsloader/basic-usage) for more information.

## Generating Cloudinary image URLs

[Section titled “Generating Cloudinary image URLs”](#generating-cloudinary-image-urls)

The Astro Cloudinary SDK provides a `getCldOgImageUrl()` helper for generating and using URLs for your images. Use this when you need a URL instead of a component to display your image.

One common use for a URL is for an Open Graph image in `<meta>` tags for social media cards. This helper, like the components, provides you access to Cloudinary transformations to create dynamic, unique social cards for any of your pages.

The following example shows the necessary `<meta>` tags for a social media card, using `getCldOgImageUrl()` to generate an Open Graph image:

Layout.astro

```jsx
---
import { getCldOgImageUrl } from 'astro-cloudinary/helpers';
const ogImageUrl = getCldOgImageUrl({ src: '<Public ID>' });
---
<meta property="og:image" content={ogImageUrl} />
<meta property="og:image:secure_url" content={ogImageUrl} />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="twitter:title" content="<Twitter Title>" />
<meta property="twitter:card" content="summary_large_image" />
<meta property="twitter:image" content={ogImageUrl} />
```

Find [Cloudinary Social Media Card templates](https://astro.cloudinary.dev/templates/social-media-cards) on the Cloudinary docs.

See [Cloudinary’s `getCldOgImageUrl()` documentation](https://astro.cloudinary.dev/getcldogimageurl/basic-usage) for more information.

## Using Cloudinary in Node.js

[Section titled “Using Cloudinary in Node.js”](#using-cloudinary-in-nodejs)

For more complex asset management, uploading, or analysis, you can use the Cloudinary Node.js SDK when working in an Astro Node.js environment.

Install the Cloudinary Node.js SDK by running the appropriate command for your package manager:

* npm

  ```shell
  npm install cloudinary
  ```

* pnpm

  ```shell
  pnpm add cloudinary
  ```

* Yarn

  ```shell
  yarn add cloudinary
  ```

Add the following environment variables in your `.env` file:

.env

```shell
PUBLIC_CLOUDINARY_CLOUD_NAME="<Your Cloud Name>"
PUBLIC_CLOUDINARY_API_KEY="<Your API Key>"
CLOUDINARY_API_SECRET="<Your API Secret>"
```

Configure your account with a new Cloudinary instance by adding the following code between the fences of your Astro component:

Component.astro

```js
---
import { v2 as cloudinary } from "cloudinary";


cloudinary.config({
  cloud_name: import.meta.env.PUBLIC_CLOUDINARY_CLOUD_NAME,
  api_key: import.meta.env.PUBLIC_CLOUDINARY_API_KEY,
  api_secret: import.meta.env.CLOUDINARY_API_SECRET,
});
---
```

This will give you access to all of the Cloudinary APIs to allow you to interact with your images, videos, and other supported files.

Component.astro

```js
await cloudinary.uploader.upload('./path/to/file');
```

Learn how to [upload files using the Cloudinary Node.js SDK with Astro Forms](https://www.youtube.com/watch?v=DQUYMyT2MTM).

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Cloudinary Astro SDK](https://astro.cloudinary.dev/)
* [Cloudinary Node.js SDK](https://cloudinary.com/documentation/node_integration)
* [Using Cloudinary with Astro (YouTube)](https://www.youtube.com/playlist?list=PL8dVGjLA2oMqnpf2tShn1exf5GkSWuu5-)
* [Code Examples Using Cloudinary Astro SDK (GitHub)](https://github.com/cloudinary-community/cloudinary-examples/tree/main/examples/astro-cloudinary)

# Mux & Astro

> Add high-performance video to your Astro project using Mux

[Mux](https://www.mux.com?utm_campaign=21819274-Astro\&utm_source=astro-docs) is a hosted media service that provides video streaming infrastructure and performance analytics for businesses of all scales.

When you use Mux to store and host your video content, you’ll have access to Astro-native video components for [Mux Player](#mux-player), a drop-in component for adding Mux videos in your Astro project, and [Mux Uploader](#mux-uploader) for uploading videos to Mux from your website. These components integrate seamlessly with [Mux Data](https://www.mux.com/docs/guides/data?utm_campaign=21819274-Astro\&utm_source=astro-docs) to track your video engagement and performance.

You can also interact with your content through the [Mux Node SDK](#mux-node-sdk).

Tip

Learn more about features such as embedding, storing, streaming, and customizing video at [Mux’s dedicated page for video in Astro](https://www.mux.com/video-for/astro?utm_campaign=21819274-Astro\&utm_source=astro-docs)!

## Using Mux in Astro

[Section titled “Using Mux in Astro”](#using-mux-in-astro)

Mux’s APIs and web components work in Astro to compress and optimize your videos and streams for the web, adapt the quality of your video to network conditions, and integrate additional features like captions, thumbnails, and analytics. The [Mux Node SDK](https://www.mux.com/docs/integrations/mux-node-sdk?utm_campaign=21819274-Astro\&utm_source=astro-docs) supports both Mux Data and the Mux Video API.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* An existing Astro project. Some features may additionally require an adapter installed for [on-demand server rendering](/en/guides/on-demand-rendering/).
* A Mux account. If you don’t have an account, you can [sign up with Mux](https://dashboard.mux.com/login?utm_campaign=21819274-Astro\&utm_source=astro-docs) using the code `ASTRO` to receive a $50 credit.

## Mux Player

[Section titled “Mux Player”](#mux-player)

In Astro, you can use the full-featured [Mux Player](https://www.mux.com/docs/guides/mux-player-web?utm_campaign=21819274-Astro\&utm_source=astro-docs) as a native Astro component for optimized, responsive video playback and live streams.

Mux Player provides a responsive UI based on video player dimensions and stream type, automatic thumbnail previews and poster images, and modern video player capabilities (e.g. fullscreen, picture-in-picture, Chromecast, AirPlay).

src/components/MyMuxVideoPlayer.astro

```astro
---
import { MuxPlayer } from "@mux/mux-player-astro";
---
<MuxPlayer
  playbackId="DS00Spx1CV902MCtPj5WknGlR102V5HFkDe"
  metadata={{ video_title: 'My Astro Video' }}
/>
```

Mux Player has built-in support for Mux Data analytics, and will automatically show visitor engagement and video quality metrics in your dashboard once your video has views on your deployed site.

### Installation

[Section titled “Installation”](#installation)

Install the Astro version of Mux Player using your preferred package manager:

* npm

  ```shell
  npm install @mux/mux-player-astro
  ```

* pnpm

  ```shell
  pnpm add @mux/mux-player-astro
  ```

* Yarn

  ```shell
  yarn add @mux/mux-player-astro
  ```

Mux Player can also be used in your Astro project as:

* a web component (`<mux-player>` from `@mux/mux-player` )
* a React component (`<MuxPlayer />` from `@mux/mux-player-react`)
* an HTML web embed (`<iframe>`)

### Play a video from Mux

[Section titled “Play a video from Mux”](#play-a-video-from-mux)

Import and use the native `<MuxPlayer />` Astro component directly in your `.astro` files like any other Astro component.

You will need the `playbackId` for your asset, which can be found in your Mux dashboard or [retrieved from its `ASSET_ID`](#retrieve-asset-data).

All other [options to control the Mux web player](https://www.mux.com/docs/guides/player-api-reference/?utm_campaign=21819274-Astro\&utm_source=astro-docs) (e.g. hide or display controls, style elements, disable cookies) are optional:

src/components/StarlightVideo.astro

```astro
<MuxPlayer
  playbackId="FOTbeIxKeMPzyhrob722wytaTGI02Y3zbV00NeFQbTbK00"
  metadata={{
    video_title: 'Starlight by Astro',
  }}
  style={{
    display: 'block',
    aspectRatio: '16/9',
    backgroundColor: '#000',
    margin: '1rem 0 2rem',
  }}
  primaryColor="#f2ec3a"
  secondaryColor="#0caa09"
  accentColor="#6e1e99"
  defaultShowRemainingTime={true}
/>
```

If your `playbackId` belongs to a live stream instead of a prerecorded video on demand, then the Mux Player will allow you to further customize the player with options such as whether or not to enable [DVR mode](https://www.mux.com/docs/guides/stream-recordings-of-live-streams#dvr-mode-vs-non-dvr-mode?utm_campaign=21819274-Astro\&utm_source=astro-docs).

```astro
<MuxPlayer
  playbackId="FOTbeIxKeMPzyhrob722wytaTGI02Y3zbV00NeFQbTbK00"
  metadata={{
    video_title: 'Starlight stream with Astro',
  }}
  streamType="live:dvr"
/>
```

Every live stream is recorded and saved on Mux as a video asset for future on-demand playback.

## Mux video Element

[Section titled “Mux video Element”](#mux-video-element)

The [Mux video element](https://www.mux.com/docs/guides/play-your-videos#mux-video-element?utm_campaign=21819274-Astro\&utm_source=astro-docs) is a drop-in replacement for the HTML5 `<video>` element that provides browser support for HLS playback, and has Mux Data automatically configured to show visitor and performance metrics. Use this when you do not need or want all the features of [Mux Player](#mux-player).

To use the `<mux-video>` web component, first install `mux-video` using your preferred package manager:

* npm

  ```shell
  npm install @mux/mux-video
  ```

* pnpm

  ```shell
  pnpm add @mux/mux-video
  ```

* Yarn

  ```shell
  yarn add @mux/mux-video
  ```

Then, you can import and render the web component in a `<script>` tag in your `.astro` file.

You will need the `playback-id` for your video asset, which can be found in your Mux dashboard or [retrieved from its `ASSET_ID`](#retrieve-asset-data).

All attributes for the HTML 5 `<video>` element (e.g. `poster`, `controls`, `muted`) are available, as well as additional Mux video player controls (e.g. to provide metadata, control the resolution, disable cookies):

src/components/StarlightVideo.astro

```astro
<script>import '@mux/mux-video'</script>


<mux-video
  playback-id="FOTbeIxKeMPzyhrob722wytaTGI02Y3zbV00NeFQbTbK00"
  metadata-video-title="Starlight by Astro"
  controls
  disable-tracking
></mux-video>
```

## Mux Node SDK

[Section titled “Mux Node SDK”](#mux-node-sdk)

The [Mux Node SDK](https://www.mux.com/docs/integrations/mux-node-sdk?utm_campaign=21819274-Astro\&utm_source=astro-docs) provides authenticated access to the Mux REST API from server-side TypeScript or JavaScript. This allows you to interact with your Mux assets and data in the component script of your `.astro` files.

While the Mux Player and Mux Video components do not require authentication and can play any publicly accessible video given its `playbackId`, connecting to your hosted Mux data via the Node SDK requires [a Mux API access token](#mux-environment-api-access).

### Installation

[Section titled “Installation”](#installation-1)

Install the Mux Node SDK using your preferred package manager:

* npm

  ```shell
  npm install @mux/mux-node
  ```

* pnpm

  ```shell
  pnpm add @mux/mux-node
  ```

* Yarn

  ```shell
  yarn add @mux/mux-node
  ```

### Mux Environment API access

[Section titled “Mux Environment API access”](#mux-environment-api-access)

API tokens are tied to a specific Mux Environment, which is essentially a container for your videos and related data. When you sign up for Mux, an Environment is created for you automatically. If you’ve created additional Environments, make sure you select the correct one before generating your tokens. From there, you can [get your ID and SECRET tokens](https://www.mux.com/docs/core/stream-video-files#1-get-an-api-access-token) and provide them to the Node SDK. These tokens can be passed into your Astro components as environment variables stored in a `.env` file.

This will allow you to create an instance of the Mux Node SDK for retrieving information about your videos, creating new assets, accessing metrics and real-time performance, and more:

src/components/StarlightVideo.astro

```astro
---
import Mux from "@mux/mux-node";


const mux = new Mux ({
  tokenId: import.meta.env.MUX_TOKEN_ID,
  tokenSecret: import.meta.env.MUX_TOKEN_SECRET,
})
---
```

Read more about using [environment variables](/en/guides/environment-variables/) in your Astro project, including creating a [type-safe schema](/en/guides/environment-variables/#type-safe-environment-variables) for your Mux credentials.

### Retrieve asset data

[Section titled “Retrieve asset data”](#retrieve-asset-data)

To fetch information about your video to use in your Astro project, provide the video’s `ASSET_ID` (available in the Mux dashboard) to the `retrieve()` helper function. This will allow you to pass values to both your Mux components and your HTML template, such as the video’s title or duration:

```astro
---
import Mux from "@mux/mux-node";
import { MuxPlayer } from "@mux/mux-player-astro";


const mux = new Mux({
  tokenId: import.meta.env.MUX_TOKEN_ID,
  tokenSecret: import.meta.env.MUX_TOKEN_SECRET,
})


const ASSET_ID = "E01irAaN8c6dk1010153uC2mzst7RVbAdJJWtHECAHFvDo";
const asset = await mux.video.assets.retrieve(ASSET_ID);


const playbackId = asset.playback_ids?.find((id)=> id.policy=== "public")?.id;
const videoTitle = asset?.meta?.title;
const createdAt = Number(asset?.created_at);
const duration = Number(asset?.duration)


const date = new Date(createdAt * 1000).toDateString()
const time = new Date(Math.round(duration) * 1000).toISOString().substring(14, 19)
---
<h1>My Video Page</h1>
<p>Title: {videoTitle}</p>
<p>Upload Date: {date}</p>
<p>Length: {time}</p>


<MuxPlayer
  playbackId={playbackId}
  metadata={{video_title: videoTitle}}
/>
```

See all asset properties in the [Mux Asset API documentation](https://www.mux.com/docs/api-reference/video/assets?utm_campaign=21819274-Astro\&utm_source=astro-docs).

## Mux Uploader

[Section titled “Mux Uploader”](#mux-uploader)

[Mux Uploader](https://www.mux.com/docs/guides/mux-uploader?utm_campaign=21819274-Astro\&utm_source=astro-docs) is a fully-functional, customizable video upload UI for your Astro website. The native Astro `<MuxUpload />` component allows you to build video upload functionality into your web app.

Mux Uploader supports both manual file selection and drag and drop for file uploads, optional pausing and resuming of uploads, and more.

### Installation

[Section titled “Installation”](#installation-2)

Install the Astro version of Mux Uploader using your preferred package manager:

* npm

  ```shell
  npm install @mux/mux-uploader-astro
  ```

* pnpm

  ```shell
  pnpm add @mux/mux-uploader-astro
  ```

* Yarn

  ```shell
  yarn add @mux/mux-uploader-astro
  ```

### Upload a video to Mux

[Section titled “Upload a video to Mux”](#upload-a-video-to-mux)

Before uploading a video, make sure you have your [Mux API access tokens](#mux-environment-api-access) configured. With those in place, you can use the `create()` function from the Mux Node SDK to start a new video upload:

```astro
---
import Layout from '../../layouts/Layout.astro';
import Mux from "@mux/mux-node";
import { MuxUploader } from "@mux/mux-uploader-astro";


const mux = new Mux({
  tokenId: import.meta.env.MUX_TOKEN_ID,
  tokenSecret: import.meta.env.MUX_TOKEN_SECRET
});


const upload = await mux.video.uploads.create({
  new_asset_settings: {
    playback_policy: ['public'],
    video_quality: 'basic'
  },
  cors_origin: '*',
});
---
<Layout title="Upload a video to Mux">
  <MuxUploader endpoint={upload.url} />
</Layout>
```

### Customize the uploader

[Section titled “Customize the uploader”](#customize-the-uploader)

You can customize the functionality and appearance of the `<MuxUploader />` with additional component attributes. In addition to styling your element, this allows you to control options such as the ability to pause a download or set a maximum file size.

```astro
---
import { MuxUploader } from '@mux/mux-uploader-astro';
---


<MuxUploader
  endpoint="https://my-authenticated-url/storage?your-url-params"
  pausable
  maxFileSize={1000000000}
  chunkSize={8192}
  style={{
    '--progress-bar-fill-color': '#7e22ce',
    '--button-background-color': '#f0f0f0',
  }}
/>
```

See the [Mux Uploader customization guide](https://www.mux.com/docs/guides/uploader-web-customize-look-and-feel?utm_campaign=21819274-Astro\&utm_source=astro-docs) for more options.

### Event handling for uploads

[Section titled “Event handling for uploads”](#event-handling-for-uploads)

Mux Uploader provides a feature-rich, dynamic UI that changes based on the current state of your media upload. The uploader’s behavior responds to both user-driven events (e.g. selecting a file, retrying after an error) and state-driven events (e.g. upload in-progress, upload successfully completed).

You can listen for these events and handle them in your Astro component with [client-side scripts](/en/guides/client-side-scripts/). A `MuxUploaderElement` type is also available.

```astro
---
import { MuxUploader } from '@mux/mux-uploader-astro';
---


<MuxUploader
  id="my-uploader"
  endpoint="https://my-authenticated-url/storage?your-url-params"
  pausable
/>


<script>
  import type { MuxUploaderElement } from '@mux/mux-uploader-astro';


  const uploader = document.getElementById('my-uploader') as MuxUploaderElement;


  uploader.addEventListener('uploadstart', (event) => {
    console.log('Upload started!', event.detail);
  });


  uploader.addEventListener('success', (event) => {
    console.log('Upload successful!', event.detail);
  });


  uploader.addEventListener('uploaderror', (event) => {
    console.error('Upload error!', event.detail);
  });


  uploader.addEventListener('progress', (event) => {
    console.log('Upload progress: ', event.detail);
  });
</script>
```

## Official Resources

[Section titled “Official Resources”](#official-resources)

For the full API and webhook reference, usage guides, and information about additional topics, such as integrating with a CMS, building custom video workflows, and more, please see:

* [The official Mux documentation for Astro](https://www.mux.com/docs/integrations/astro?utm_campaign=21819274-Astro\&utm_source=astro-docs)
* [`@mux/mux-player-astro` API reference](https://github.com/muxinc/elements/blob/main/packages/mux-player-astro/README.md)
* [`@mux/mux-uploader-astro` API reference](https://github.com/muxinc/elements/blob/main/packages/mux-uploader-astro/REFERENCE.md)
* [Building a video uploader with Mux and Astro (YouTube)](https://www.youtube.com/watch?v=aaL1k5FsWfE)
* [Astro uploader and player code example (GitHub)](https://github.com/muxinc/examples/tree/main/astro-uploader-and-player)

