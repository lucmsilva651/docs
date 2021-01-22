# Development Environment



{% hint style="info" %}
These instructions are for setting up a local DEVELOPMENT environment. If you are looking for basic installation instruction see the [Installation](../getting-started/installation.md) page instead.
{% endhint %}

## Setting up a Local Development Environment

### Requirements

* [PHP](https://php.net) &gt;= 7.4
  * [Composer](https://getcomposer.org)
* [NPM](https://www.npmjs.com)
* [Docker](https://www.docker.com)
  * [Docker Compose](https://docs.docker.com/compose/)

### Instructions

1. [Fork the Directory Lister repository to your own account](https://github.com/DirectoryLister/DirectoryLister/fork)
2. [Clone your fork to a local repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository)

   ```text
   git clone {{ YOUR_REPOSITORY_URL }}
   ```

3. Switch to the Directory Lister directory

   ```text
   cd /path/to/DirectoryLister
   ```

4. Install and build PHP and JavaScript dependencies

   ```text
   composer install
   npm install && npm run dev
   ```

5. Create the Docker `development` network

   ```text
   docker network create development
   ```

6. Create the Docker development proxy

   ```text
   docker run -d -p 80:80 --network development --restart unless-stopped --volume /var/run/docker.sock:/tmp/docker.sock:ro --name dev-proxy jwilder/nginx-proxy
   ```

7. Add the following entry to `/etc/hosts`:

   ```text
   127.0.0.1  directory-lister.local
   ```

8. Run the local Docker container

   ```text
   docker-compose up -d
   ```

You should now be able to access your local Directory Lister installation at [http://directory-lister.local](http://directory-lister.local)

## Code Analysis and Testing

### Coding Standards

Apply coding standards to all application and tests files

```text
app/vendor/bin/php-cs-fixer fix
npx eslint --fix app/resources/js/**/*.{js.vue}
```

or to see the changes before applying

```text
app/vendor/bin/php-cs-fixer fix --diff --dry-run
npx eslint app/resources/js/**/*.{js.vue}
```

### Static Analysis

```text
app/vendor/bin/psalm
```

### Unit/Feature Tests

```text
app/vendor/bin/phpunit
```

### Code Coverage

```text
app/vendor/bin/phpunit --coverage-text
```

or generate a more detailed HTML coverage report with

```text
app/vendor/bin/phpunit --coverage-html .coverage
```

## Building Release Artifacts

Release artifacts will build in the `artifacts` directory.

### Build All

```text
make artifacts
```

#### Build `tar.gz`

```text
make tar
```

#### Build `zip`

```text
make zip
```
