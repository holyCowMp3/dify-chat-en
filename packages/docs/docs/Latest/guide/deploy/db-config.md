# Custom Database Type

By default, Dify Chat uses MySQL for persistent storage of application configuration.

### 1. Using MySQL

If you have a MySQL database, it's very simple. Just build a database connection for storing Dify Chat data and configure it in environment variables:

```shell
mysql://username:password@host:port/database_name
```

### 2. Using Other Databases

If your database is another type, you need to modify the code. The following uses PostgreSQL as an example to explain how to configure it.

First, modify the database type in the Prisma configuration file:

```shell title="packages/platform/prisma/schema.prisma"
datasource db {
  provider = "postgresql"
}
```

Then configure your database connection address in .env:

```shell title="packages/platform/.env"
DATABASE_URL=postgres://username:password@ip:port/dify-chat
```

Regenerate Prisma client files and synchronize table structure:

```shell
# Regenerate client files
pnpm --filter dify-chat-platform db:generate

# Delete migrations directory
rm -rf packages/platform/prisma/migrations

# Reset migration history
pnpm --filter dify-chat-platform exec prisma migrate reset

# Regenerate migration files
pnpm --filter dify-chat-platform exec prisma migrate dev
```

Finally, start it in your preferred way (Docker or script).
