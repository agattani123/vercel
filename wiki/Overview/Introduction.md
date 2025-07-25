<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [README.md](https://github.com/agattani123/vercel/blob/main/README.md)

</details>

# Introduction

Vercel is a cloud platform that provides a developer experience and infrastructure for building, scaling, and securing faster and more personalized web applications. It offers a seamless deployment process, allowing developers to deploy their projects with a simple `git push` command. The platform supports various project templates and can be integrated with the Vercel CLI for enhanced functionality.

## Overview

Vercel's Frontend Cloud aims to streamline the development, preview, and deployment processes for web applications. It provides a comprehensive set of tools and services to help developers build, scale, and secure their web projects efficiently.

## Deployment Process

Vercel offers a straightforward deployment process that can be initiated in several ways:

1. **Importing a Project:** Developers can import an existing project directly into the Vercel platform.
2. **Choosing a Template:** Vercel provides a collection of pre-built templates that can be used as a starting point for new projects.
3. **Using the Vercel CLI:** The Vercel Command Line Interface (CLI) allows developers to deploy their projects directly from their local development environment with a simple `git push` command.

Once the deployment process is initiated, Vercel takes care of building, optimizing, and deploying the application to its global edge network, ensuring fast and reliable performance for end-users.

## Documentation and Resources

Vercel offers comprehensive documentation and resources to help developers get started and make the most of the platform:

- **Documentation:** The [Vercel Documentation](https://vercel.com/docs) provides detailed information on various aspects of the platform, including deployment, configuration, and advanced features.
- **Changelog:** The [Vercel Changelog](https://vercel.com/changelog) keeps developers informed about the latest updates, bug fixes, and new features.
- **Templates:** Vercel offers a collection of [Templates](https://vercel.com/templates) that serve as starting points for various types of projects, such as static sites, Next.js applications, and more.
- **CLI:** The [Vercel CLI](https://vercel.com/docs/cli) allows developers to interact with the Vercel platform from their local development environment, enabling streamlined deployment and management of projects.

Sources: [README.md](https://github.com/agattani123/vercel/blob/main/README.md)

## Contributing to Vercel

Vercel is an open-source project, and contributions from the community are welcome. The project uses the `pnpm` package manager for installing dependencies and running scripts. To contribute to the project, follow these steps:

1. **Clone the Repository:** Clone the Vercel repository from GitHub using the provided URL: `git clone https://github.com/vercel/vercel`.
2. **Install Dependencies:** Navigate to the cloned repository and install the dependencies using `pnpm install`.
3. **Build the Project:** Build the project by running `pnpm build`.
4. **Lint the Code:** Ensure the code adheres to the project's linting rules by running `pnpm lint`.
5. **Run Tests:** Execute the unit tests to verify the correctness of your changes by running `pnpm test-unit`.

### Local Development

Vercel is structured as a monorepo, with multiple npm packages contained within a single repository. Dependencies are managed using `pnpm` instead of the `npm` CLI.

To run local changes to the Vercel CLI, navigate to the `cli` package directory and execute `pnpm vercel <cli-commands...>`. This will invoke the Vercel CLI with your local changes.

For more details on local development with the Vercel CLI, refer to the [CLI Local Development](../packages/cli#local-development) documentation.

### Pull Request Process

Once you have made your changes and verified that all tests pass, you can open a pull request on the main Vercel repository. The maintainers will review your pull request, and the continuous integration platform will check the tests.

### Testing

Vercel includes two types of tests: unit tests and integration tests.

#### Unit Tests

Unit tests are executed locally using Jest and test the smallest units of code. They run quickly and provide immediate feedback on the correctness of your changes.

#### Integration Tests

Integration tests create deployments to your Vercel account using the `test` project name. After each deployment, the `probes` key is used to check if the response matches the expected value. If the value doesn't match or if the deployment fails to build, you'll receive an error message explaining the issue.

To run integration tests locally, you'll need to set up the appropriate credentials in your shell environment:

1. Create an access token with the appropriate scope for your personal account.
2. Obtain the team ID from the Vercel dashboard.
3. Source the token and team ID into your shell environment variables: `export VERCEL_TOKEN=<MY-TOKEN> VERCEL_TEAM_ID=<MY-TEAM-ID>`.

With the credentials set up, you can run individual integration tests by navigating to the desired package directory and executing `pnpm test <test_file_path>`.

If you encounter issues with the tree-shaking mechanism used by some Builders, you can create a script to analyze the imported files using `@vercel/nft`.

### Deploying a Builder with an Existing Project

To test changes to a Builder against an existing project, you can upload the Builder as a tarball instead of publishing it to npm:

1. Change directory to the desired Builder: `cd ./packages/node`.
2. Build the Builder: `pnpm build`.
3. Create a tarball file: `npm pack`.
4. Upload the tarball file and get a URL: `vercel *.tgz`.
5. Edit the existing `vercel.json` project and replace `use` with the URL.
6. Deploy with the experimental Builder using `vercel` or `vercel dev`.

Sources: [README.md](https://github.com/agattani123/vercel/blob/main/README.md)

## Code of Conduct and Licensing

Vercel adheres to a Code of Conduct to ensure a respectful and inclusive environment for all contributors. The project is licensed under the Apache 2.0 License.

- [Code of Conduct](./.github/CODE_OF_CONDUCT.md)
- [Contributing Guidelines](./.github/CONTRIBUTING.md)
- [Apache 2.0 License](./LICENSE)

Sources: [README.md](https://github.com/agattani123/vercel/blob/main/README.md)