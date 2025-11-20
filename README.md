 TabNews Config

> This beta only implements the 'tn test' command. It does not yet have the other commands.

Tool to set up a development and testing environment with the same settings used in TabNews, including:

- TabNews CLI
- Environment variables with [dotenv-expand](https://www.npmjs.com/package/dotenv-expand)
- Test Runner [Vitest](https://vitest.dev/)
- Linter [ESLint](https://eslint.org/)
- Code Formator [Prettier](https://prettier.io/)
- Docker containers
  - Database [PostgreSQL](https://hub.docker.com/_/postgres)
  - Email Server [MailCatcher](https://hub.docker.com/r/sj26/mailcatcher)

## Requirements

- [Node.js](https://nodejs.org/)
- [Docker](https://www.docker.com/) e [Docker Compose](https://docs.docker.com/compose/)

## Installation

To add to the project, run the command:

```bash
npm i -D @tabnews/config
```

To use the CLI, install globally with the command:

```bash
npm i -g @tabnews/config
```

## Usage

### Scripts NPM

Add scripts in the project 'package.json', for example:

```json
{
  "scripts": {
    "dev": "tn --seed",
    "build": "tn build --seed",
    "start": "tn start",
    "test": "tn test run",
    "test:watch": "tn test"
  }
}
```

### CLI

To start the services using the development environment variables, run the following command (remember to globally install the CLI or use 'npx'):

```bash
tn
```

All other commands can be consulted with:

```bash
tn --help
```

Some commands have subcommands, which can also be queried through the CLI, for example:

```bash
tn migration -h
```

## Environment Variables

Accepts environment variable files in the same way as [Next.js](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables). The exception is the variable 'NEXT_PUBLIC_WEBSERVER_PORT' in place of the 'PORT'. It allows you to define the port used by the server Next.js location.

According to the running command, the corresponding environment variables will be loaded from the following files, if they exist:

- Always charged:

  - .env (default variables)
  - .env.local(local variables)

- Loaded according to the running command:
  - .env.development (development variables)
  - '.env.development.local' (local development variables)
  - '.env.test' (test variables)
  - '.env.test.local' (local test variables)
  - '.env.production' (production variables)
  - '.env.production.local' (local production variables)

The CLI accepts the '--env-mode' (or '-e') parameter to specify a non-default environment for the command, e.g. to go up the development server with test environment variables but without running the tests, run:

```bash
tn --env-mode test
```

Accepts the [Docker Compose environment variables](https://docs.docker.com/compose/environment-variables/envvars/), of which we can highlight:

- ['COMPOSE_PROJECT_NAME'](https://docs.docker.com/compose/environment-variables/envvars/#compose_project_name): which allows you to isolate containers from different projects, even using the same standard TabNews 'compose.yml' file. It is also useful for isolating data from automated tests and other development environments. It also makes it possible to run different projects in parallel, as long as there are no other conflicts, such as exposed doors.

- ['COMPOSE_FILE'](https://docs.docker.com/compose/environment-variables/envvars/#compose_file): Allows you to use another 'compose.yml' file in case you need to test or develop something with a more specific configuration.

### Example of variables used in TabNews

```env
POSTGRES_USER=user
POSTGRES_PASSWORD=password
POSTGRES_DB=tabnews
POSTGRES_HOST=localhost
POSTGRES_PORT=5432

NEXT_PUBLIC_WEBSERVER_HOST=localhost
NEXT_PUBLIC_WEBSERVER_PORT=3000

EMAIL_SMTP_HOST=localhost
EMAIL_SMTP_PORT=1025
EMAIL_HTTP_HOST=localhost
EMAIL_HTTP_PORT=1080
```

If you set the POSTGRES_USER, POSTGRES_PASSWORD, and POSTGRES_DB variables to different values in each file, it is recommended that you also specify a different COMPOSE_PROJECT_NAME. Otherwise, you'll need to remove the container and volume to recreate Postgres with the new values each time you switch environments
<img width="468" height="658" alt="image" src="https://github.com/user-attachments/assets/275abcf7-c9e8-419c-8141-04dd47bf81cb" />
