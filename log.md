# Context

Create a sample project:

```
npm init -y
npm install prisma --save-dev
```

# Trying prisma db pull and push

File `user.sql`
```sql
CREATE TABLE “USER” (
	“id” INTEGER PRIMARY KEY,
     “email” TEXT UNIQUE
);
```

Initialize a db.

`cat user.sql | sqlite3 example.db`


Create a `schema.prisma` file.

```prisma
datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}
```

Do a pull to introspect the db schema.

`npx prisma db pull`

I got:

```prisma
datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model USER_ {
  id_    Int     @id @default(autoincrement()) @map("“id”")
  email_ String? @unique(map: "sqlite_autoindex_“USER”_1") @map("“email”")

  @@map("“USER”")
}
```

Now run push, attempting a no-op round trip.

`npx prisma db push`

I get the following error:

```
Environment variables loaded from .env
Prisma schema loaded from schema.prisma
Datasource "db": SQLite database "example.db" at "file:example.db"
Error: near ")": syntax error
   0: sql_migration_connector::sql_database_step_applier::apply_migration
             at migration-engine/connectors/sql-migration-connector/src/sql_database_step_applier.rs:11
   1: migration_core::api::SchemaPush
             at migration-engine/core/src/api.rs:161
```

Why can't I round-trip this way?
Why is the error message a syntax error?

