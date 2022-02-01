## Build

From your project directory:

`docker build -t IMAGE_NAME .`

## Run

### Production

#### Database:

`docker run -d --restart unless-stopped --name blog-db -p 5432:5432 -e POSTGRES_DB=YOUR_DB -e POSTGRES_PASSWORD=YOUR_PASSWORD postgres`

#### Application:

`docker run -d --restart unless-stopped -p=8000:8000 -e PORT=8000 -e TOKEN_SECRET=YOUR_SECRET -e DB_USER=YOUR_USER -e DB_HOST=YOUR_DB_HOST -e DB=YOUR_DB -e DB_PASSWORD=YOUR_DB_PASSWORD -e DB_PORT=5432 --name=CONTAINER_NAME IMAGE_NAME`

#### Cache:

`docker run -d --restart unless-stopped -p 6379:6379 --name CONTAINER_NAME redis redis-server --requirepass YOUR_PASSWORD --save 60 1 --loglevel warning`

### Development

#### Local:

<strong>db</strong>:

with data persistence:
`docker run -d --rm --name blog-db --mount type=volume,src=data,target=/var/lib/postgresql/data -p 5432:5432 -e POSTGRES_DB=YOUR_DB -e POSTGRES_PASSWORD=mysecretpassword postgres` <br />

without data persistence:
`docker run -d --rm --name blog-db -p 5432:5432 -e POSTGRES_DB=blog -e POSTGRES_DB=YOUR_DB -e POSTGRES_PASSWORD=mysecretpassword postgres` <br />

<strong>redis</strong>

`docker run --rm -d -p 6379:6379 --name CONTAINER_NAME redis redis-server --requirepass YOUR_PASSWORD`

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

Note: make npm installations in the container environment since some dependencies are compiled differently on different OS.

## Migrations

### Create

`npm run migration:create MIGRATION_NAME -- --sql-file`

### Run migrations that haven't been applied yet

`npm run migration:up`

### Revert last migration

`npm run migration:down`

## Testing

Run: `npm test`
