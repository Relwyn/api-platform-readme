
# API PLATFORM

API Platform is a next-generation web framework designed to easily create API-first projects without compromising extensibility and flexibility

## Getting Started

Start by downloading the API Platform distribution .tar.gz file, or generate a GitHub repository from the template we provide.
Once you have extracted its contents, the resulting directory contains the API Platform project structure. You will add your own code and configuration inside it.

### Prerequisites

  * API Platform distrubution ( from [github](https://github.com/api-platform/api-platform/releases/tag/v2.5.0) or [.tar.gz](https://github.com/api-platform/api-platform/releases/tag/v2.5.0)
  * Docker
  * Postgres
  * NGNINX
  * Mercure

### Installing

* The docker starts the folling service

  * PHP 7
  * db (Postgres port : 5432)
  * client (port : 80)
  * admin (port : 81)
  * api (HTTP server NGNINX port : 8080)
  * mercure (port : 1337)

* Launching docker
  ```
  $ docker-compose pull # Download the latest versions of the pre-built images
  $ docker-compose up -d # Running in detached mode
  ```

* Reading logs
  ```
  $ docker-compose logs -f # follow the logs
  ```

Project files are automatically shared between your local host machine and the container

* Create a Symfony project

  #### Create a new Symfony Flex project
  $ composer create-project symfony/skeleton bookshop-api
  #### Enter the project folder
  $ cd bookshop-api
  #### Install the API Platform's server component in this skeleton
  $ composer req api

  #### Make the database right for running

  $ bin/console doctrine:database:create
	$ bin/console doctrine:schema:create

	#### Built-in PHP server
	$ php -S 127.0.0.1:8000 -t public

* It's Ready !

	Open https://localhost in your favorite web browser:

### Testing

API Platform provides a set of helpful testing utilities to write unit tests,
functional tests, and to create test fixtures.

* Creating Data Fixture

Before creating your functional tests, you will need a dataset to pre-populate your API and be able to test it.

```
$ docker-compose exec php composer require --dev alice
```

Place your data fixtures files in a directory named fixtures/.

* Writing Functional Tests

Now that you have some data fixtures for your API, you are ready to write functional tests with PHPUnit.

```
$ docker-compose exec php composer require --dev test-pack http-client justinrainbow/json-schema
```

Your API is ready to be functionally tested. Create your test classes under the tests/ directory.

* Here is an example of functional tests

https://api-platform.com/docs/distribution/testing/


* Run the test

```
$ docker-compose exec php vendor/bin/simple-phpunit
```

* Writing Unit Tests

In addition to integration tests written using the helpers provided by ApiTestCase, all the classes of your project should be covered by unit tests. To do so, learn how to write unit tests with PHPUnit and its Symfony/API Platform integration.

### Docker Deployement
#### Local side
* If you are using the API Platform Distribution, we provide a [ready-to-deploy Docker Compose](https://github.com/api-platform/docker-compose-prod) setup for production.

```
$ wget -O - https://github.com/api-platform/docker-compose-prod/archive/master.tar.gz | tar -xzf - && mv docker-compose-prod-master docker-compose-prod
$ git add docker-compose-prod
```

* If you are building on your development machine, you could set the environment variables in the .env file at the top level of the distribution project (not to be confused with api/.env which is used by the Symfony application).
For example:
```
ADMIN_IMAGE=registry.example.com/api-platform/admin
CLIENT_IMAGE=registry.example.com/api-platform/client
NGINX_IMAGE=registry.example.com/api-platform/nginx
PHP_IMAGE=registry.example.com/api-platform/php
REACT_APP_API_ENTRYPOINT=https://api.example.com
VARNISH_IMAGE=registry.example.com/api-platform/varnis`
```

* Build the Docker images:
```
$ docker-compose -f docker-compose-prod/docker-compose.build.yml pull --ignore-pull-failures
$ docker-compose -f docker-compose-prod/docker-compose.build.yml build --pull
```
* Push the built images to the container registry:
```
$ docker-compose -f docker-compose-prod/docker-compose.build.yml push
```
#### Server side
* You could set the environment variables in the .env file at the top level of the distribution project (not to be confused with api/.env which is used by the Symfony application). For example:
```
ADMIN_HOST=admin.example.com
ADMIN_IMAGE=registry.example.com/api-platform/admin
API_HOST=api.example.com
APP_SECRET=3c857494cfcc42c700dfb7a6
CLIENT_HOST=example.com,www.example.com
CLIENT_IMAGE=registry.example.com/api-platform/client
CORS_ALLOW_ORIGIN=^https://(?:\w+\.)?example\.com$
DATABASE_URL=postgres://api-platform:4e3bc2766fe81df300d56481@db/api
MERCURE_ALLOW_ANONYMOUS=0
MERCURE_CORS_ALLOWED_ORIGINS=https://example.com,https://admin.example.com
MERCURE_HOST=mercure.example.com
MERCURE_JWT_KEY=4121344212538417de3e2118
MERCURE_JWT_SECRET=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZXJjdXJlIjp7InN1YnNjcmliZSI6WyJmb28iLCJiYXIiXSwicHVibGlzaCI6WyJmb28iXX19.B0MuTRMPLrut4Nt3wxVvLtfWB_y189VEpWMlSmIQABQ
MERCURE_SUBSCRIBE_URL=https://mercure.example.com/hub
NGINX_IMAGE=registry.example.com/api-platform/nginx
PHP_IMAGE=registry.example.com/api-platform/php
POSTGRES_PASSWORD=4e3bc2766fe81df300d56481
REACT_APP_API_ENTRYPOINT=https://api.example.com
TRUSTED_HOSTS=^(?:localhost|api|api\.example\.com)$
VARNISH_IMAGE=registry.example.com/api-platform/varnish
```

* Set up a redirect from e.g. www.example.com to example.com:
```
$ mkdir -p docker-compose-prod/docker/nginx-proxy/vhost.d
$ echo 'return 301 https://example.com$request_uri;' > docker-compose-prod/docker/nginx-proxy/vhost.d/www.example.com
```


* Pull the Docker images:
```
$ docker-compose -f docker-compose-prod/docker-compose.yml pull
```
* Bring up the services:
```
$ docker-compose -f docker-compose-prod/docker-compose.yml up -`
```

## To go further
If you want add an Open SSL config or deploy in an other way, you still can read our deployement [documentation](https://api-platform.com/docs/deployment/)

## Contributing

The official project [documentation](https://api-platform.com) is available on the API Platform website.

## Authors

* **Billie Thompson** - *Initial work* - [api-plateform](https://github.com/api-platform/api-platform/)

See also the list of [contributors](https://github.com/api-platform/api-platform/graphs/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/api-platform/api-platform/blob/master/LICENSE) file for details

