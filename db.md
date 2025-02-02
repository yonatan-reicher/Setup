# DB

Our database is a Postgres SQL database, that uses the [Postgres Docker image](https://hub.docker.com/_/postgres) to run in a container.
On `docker compose up`, the container initializes the database via `db/init.sql`.

## Users

Every micro-service has it's own user. These names and passwords are stored in
`config.yaml`.

## Generating `db/init.sql`

1. Run `docker compose up db`
2. Go to the docker app and open a terminal in the `db` container
3. Run `psql` to open the Postgres terminal. Now you can run SQL and postgres
   commands to modify the database.  
   Useful commands include:
   `\du` to list users
   `\l` to list databases
   `\c <database>` to connect to a database
   `\dt` to list tables
   `\d <table>` to describe a table
   `SELECT * FROM <table>;` to list all rows in a table
4. Once done, run `pg_dumpall > init.sql.2` to generate the new `init.sql` file
   in a new file called `init.sql.2`
5. Open the container's file in the docker app and download `init.sql.2` to your
   local machine.
6. Replace the `db/init.sql` file with the new `init.sql.2` file.
7. Commit the changes!
