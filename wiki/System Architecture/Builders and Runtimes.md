<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [examples/README.md](https://github.com/agattani123/vercel/blob/main/examples/README.md)
</details>

# Builders and Runtimes

## Introduction

The "Builders and Runtimes" feature in the Vercel project refers to the mechanisms and infrastructure responsible for building and executing user applications deployed on the Vercel platform. Based on the provided `examples/README.md` file, this wiki page aims to provide an overview of the process of deploying applications using the Vercel CLI and the underlying systems involved.

Sources: [examples/README.md](https://github.com/agattani123/vercel/blob/main/examples/README.md)

## Deployment Process

### Vercel CLI

The primary interface for deploying applications to the Vercel platform is the Vercel Command Line Interface (CLI). The CLI provides a set of commands for initializing, building, and deploying projects.

```sh
vercel init                    # Pick an example in the CLI
vercel init <example>          # Create a new project from a specific <example>
vercel init <example> <name>   # Create a new project from a specific <example> with a different folder <name>
```

The `vercel init` command allows users to create a new project based on one of the provided examples or a specific example template. The project can be created with a custom folder name if desired.

Sources: [examples/README.md:4-7](https://github.com/agattani123/vercel/blob/main/examples/README.md#L4-L7)

### Building and Serving

Once a project is initialized, the actual deployment process is triggered by the `vercel` command:

```sh
vercel                         # Deploy your project with the CLI
```

This command initiates the build process for the user's project within the Vercel infrastructure. Upon successful completion of the build, the application is served and made available through a generated URL.

Sources: [examples/README.md:10-12](https://github.com/agattani123/vercel/blob/main/examples/README.md#L10-L12)

## Contribution and Support

The Vercel project encourages community contributions and provides support channels for users and contributors.

### Contributing Examples

Vercel maintains a set of [contributing guidelines](https://github.com/vercel/vercel/blob/main/.github/CONTRIBUTING.md) to assist contributors in creating and submitting new examples. These guidelines cover requirements, best practices, and resources for seeking help.

Sources: [examples/README.md:16-20](https://github.com/agattani123/vercel/blob/main/examples/README.md#L16-L20)

### Reporting Issues

Users and contributors can report issues or provide feedback by raising an issue through the repository's "Issues" tab. Clear and concise problem descriptions are encouraged to facilitate timely resolution.

Sources: [examples/README.md:23-26](https://github.com/agattani123/vercel/blob/main/examples/README.md#L23-L26)

### Community Support

For additional questions or support, the Vercel community platform ([https://community.vercel.com/](https://community.vercel.com/)) is available, where both community members and Vercel staff can assist with inquiries related to the platform.

Sources: [examples/README.md:29-31](https://github.com/agattani123/vercel/blob/main/examples/README.md#L29-L31)

Please note that the provided `examples/README.md` file does not contain detailed information about the internal architecture, components, or implementation details of the "Builders and Runtimes" feature within the Vercel project. The content of this wiki page is limited to the information available in the given file.