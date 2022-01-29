## Build

From your project directory:

`docker build -t IMAGE_NAME .`

## Run

### Production

#### Application:

`docker run -d --rm -p=8000:8000 --name=CONTAINER_NAME YOUR_IMAGE`

#### Database:

`docker run -d --rm --name blog-db -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres`

### Development

#### Local:

<strong>db</strong>:

with data persistence:
`docker run -d --rm --name blog-db --mount type=volume,src=data,target=/var/lib/postgresql/data -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres` <br />

without data persistence:
`docker run -d --rm --name blog-db -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres` <br />

<strong>app</strong>:

`npm start`

#### Fully containerized 🚀:

`docker-compose rm -f` <br />
`docker-compose up --build -d` <br />
`docker exec -it YOUR_CONTAINER bash` <br />
`npm i` <br />
`npm run migration:up && npm start` <br />

To stop: `docker-compose stop`

Code in your local environment and container will pickup the changes.

Note: make npm installations in container environment since some dependencies are compiled differently on different OS.

## Migrations

### Create

`npm run migration:create MIGRATION_NAME -- --sql-file`

### Run migrations that haven't been applied yet

`npm run migration:up`

### Revert last migration

`npm run migration:down`
