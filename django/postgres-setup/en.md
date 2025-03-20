# Set up Postgres for Django


## Info

- Default user: `postgres`
- Password:
- Port: `5432`
- Configuration file: `/etc/postgresql/<VERSION>/main/postgresql.conf`

## Connect to postgres

```
sudo -i -u postgres
psql
```

- Connect directly:

```
sudo -u postgres psql
```

## Usefull command once inside postgres

- To quit `\q`

- Show all the databases `\l`

- Connect to a db: `\c mydb;`

## Commands

```
CREATE DATABASE mydb;
CREATE USER myuser WITH PASSWORD 'mypass';
ALTER ROLE myuser SET client_encoding TO 'utf8';
ALTER ROLE myuser SET timezone TO 'UTC';
ALTER ROLE myuser SET default_transaction_isolation TO 'read committed';
```

Let us give all privileges to our user:

```
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

Enable user creating db:

```
ALTER USER myuser CREATEDB;
```
