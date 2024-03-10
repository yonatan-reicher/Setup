# TeKClinic Setup Bundle

Docker compose configuration which setups a development network of TekClinic.

## How to run

1. Run the following command to fetch git submodules:
```
git submodule update --init --recursive
```

2. You have to add the following entries to the end of your `hosts` file:
```
# Tekclinic
127.0.0.1 tekclinic.org
127.0.0.1 auth.tekclinic.org
127.0.0.1 api.tekclinic.org
# End of section
```

It's needed to redirect your requests to the containers.
Path to `hosts` file on Windows is `c:\Windows\System32\Drivers\etc\hosts`.


3. Install docker and run the following commands:
```
docker-compose build --no-cache
docker compose up
```

## Available services

### Web App
Web App can be accessed on `http://tekclinic.org`.

### Keycloak
KeyCloak can be accessed on `http://auth.tekclinic.org/admin`. Admin user credentials are `admin` and `admin`.
User with a username `test_user` and a password `test` is predefined for test purposes.
Public client for Web App that runs on `http://tekclinic.org` is also predefined.

### API Gateway
API Gateway can be accessed on `http://api.tekclinic.org`.

### Database
Database can ba accessed on `localhost:5432`
When the container is created, it's set up with `db/init.sql`. If you want to save you db use the following commands:
```
docker compose exec db sh -c "pg_dumpall --exclude-database=root | sed 's/CREATE ROLE root;//' > init.sql"
docker compose cp db:init.sql db/init.sql
```
