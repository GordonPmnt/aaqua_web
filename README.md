# Aaqua community management

This project was generated using [Nx](https://nx.dev).

<p align="center"><img src="https://raw.githubusercontent.com/nrwl/nx/master/images/nx-logo.png" width="100"></p>

🔎 **Nx is a set of Extensible Dev Tools for Monorepos.**

## Prerequisites

- Node v16.x
- Npm v7.x

We recommend using https://github.com/nvm-sh/nvm to manage your node version.

We recommend installing nx globally to simplify operations:

`npm i nx -g`

## Generating code

Run `nx workspace-generator react-lib yourLib` to generate a library.

Run `nx workspace-generator react-component YourComponent --library yourLib` to generate a react component.

Run `nx workspace-generator sonar-properties` to update sonar properties files with correct coverage paths.

## Project commands

Most libraries accept the following commands:

`type-check` Perform a typescript validation.

`lint` Perform an eslint static analysis.

`test` Run unit tests.

`coverage` Serve test coverage report.

`storybook` Start storybook instance.

Commands can be executed by running `nx run {lib}:{command}`

You can refer to `./workspace.json` for more information about available tasks.

## Development server

Run `nx serve` for a dev server. Navigate to http://localhost:4200/. The app will automatically reload if you change any of the source files.

## Production server

In production we use a custom node serve instead of the NextJS default one.

The easiest way to run the server locally is to build and run the docker image.

`npm run build-all`
`docker-compose -f apps/community-management-web/.compose/docker-compose.prod.yml build`
`docker-compose -f apps/community-management-web/.compose/docker-compose.prod.yml up`

Navigate to http://localhost:3000/. The app will NOT automatically reload if you change any of the source files.

## Build

Run `nx build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Testing

### Unit tests

We write our unit tests with [Jest](https://jestjs.io) as executor. We also use [Testing library](https://testing-library.com/) to help with mimicking user interaction.

Run `nx test my-app` to execute the unit tests.

Run `nx affected:test` to execute the unit tests affected by a change.

### Smoke tests

For every app we also have `-smoke-test` project. This is meant to have a couple of basic tests to ensure the container of the app starts and returns a valid response. These are run before merging a PR and deploying the app.

These tests are written with [Playwright](https://playwright.dev/) and run with the help of [Docker Compose](https://docs.docker.com/compose/).

### Production e2e tests

For most apps we also have E2E tests that run against production. You can find those in the [Web E2E Prod repo](https://bitbucket.org/aaqua/aaqua-web-e2e-prod/src/master/).

## Other code checks

### Dead code analysis

We automatically check our code base for dead code. This is to prevent clean-up is not done properly when functionality get's removed. The config for this tool lives in `.ts-prunerc.json`. We have one `ignore` regex to ignore errors (for instance getServerSideProps). We also have a `skip` regex to determine what usage doesn't count. If a function is only used in it's own test it is still dead code.

If you're developing a new feature and you have for instance a component that is not used YET you can mark the export with this comment: `// ts-prune-ignore-next`. This will ignore the fact that exported function/constant is not used anywhere. This should only be a temporary solution. To ignore something for ever add it to the `ignore` regex in the config file.

You can find more info in the [ts-prune repository.](https://github.com/nadeesha/ts-prune)

### Strict typing

This codebase was not started with strict typing turned on. Eventually we want all our code to be strictly typed but at this point it's impossible to do this for the whole codebase at once. That's why we introduced `tsc-strict`. This is a library that can check certain folders for on strict types without the need to convert the whole codebase. To add strict typing for a lib:

1. Add the path to your lib in the `tsconfig.base.json` under plugins > typescript-strict-plugin > paths.
2. Add `"compilerOptions": { "strict": true }` to the tsconfig of your lib (this is only to have editor support)
3. run `npx tsc-strict` and fix all the errors

## Understand your workspace

Run `nx dep-graph` to see a diagram of the dependencies of your projects.

## Further help

Visit the [Nx Documentation](https://nx.dev) to learn more.

## Working with GraphQL

### Updating the schema

The schema is updated by running the following command:

```
APOLLO_KEY=[YOUR APOLLO STUDIO KEY HERE] npm run schema-update
```

To get your key login in on Apollo studio, click on your profile picture > personal settings. Under Personal API key click create new key. Save the key somewhere (preferably a password manager)

### Operations

To execute GraphQL operations we use https://graphql-code-generator.com/docs/plugins/typescript-react-apollo

To do so, create a `gql` file, run `graphql-types` and import the generated hook into your component.

```gql
query Foo {
  foo {
    bar
  }
}
```

```ts
import { useFooQuery } from '@aaqua-web/graphql-schema';

...
const { data } = useFooQuery();
```

## Run the application in production

The deliverable is a docker container.

The container exposes port 3000.

The image can be configure using these environment variables:

- BASE_PATH The base path used by the application to resolve its internal routing.
- GQL_ENDPOINT Then endpoint where to fetch graphql
- GQL_PROXY The frontend either hit an embedded proxy or directly the GQL_ENDPOINT. Should be set to the value that fits best the setup.
- UPLOAD_PROXY Similar to GQL_PROXY, but for uploading media (Amazon S3 bucket).
- AUTH_STRATEGY cognito/token/none. Refer to the authentication section.
  All following are aws & cognito configuration:
- AWS_REGION
- AWS_USER_POOLS_ID
- AWS_USER_POOLS_WEB_CLIENT_ID
- OAUTH_DOMAIN
- URI_SCHEME
- OAUTH_RESPONSE_TYPE

## Authentication

The application currently supports 3 authentication strategies. This strategy can be set at runtime by setting the `AUTH_STRATEGY` variable to the correct value.

### cognito

This uses AWS cognito & AWS amplify as a service to login and get a valid JWT token.

This is the default & production strategy.

### token

DEPRECATED (doesn't work anymore since we no longer have a mocked backend)

### none

DEPRECATED (doesn't work anymore since we no longer have a mocked backend)

## Documentation

We use storybook as a documentation system.

To start the general storybook instances, run `npm run storybook`.

You can still start the storybook of a specific library by running `nx storybook [lib]`.

## Further reading and documentation

https://aaqua.atlassian.net/wiki/spaces/AAQ/pages/1840284007/Core+Web+final+KSS
# aaqua_web
