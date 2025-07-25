<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [DEVELOPING_A_RUNTIME.md](https://github.com/agattani123/vercel/blob/main/DEVELOPING_A_RUNTIME.md)

</details>

# Extending Vercel

## Introduction

Vercel is a cloud platform that enables developers to host websites and web services. One of the key features of Vercel is the ability to extend its functionality by creating custom Runtimes. A Runtime is an npm module that implements the Runtime API interface, allowing developers to add support for new programming languages or frameworks to the Vercel platform.

This wiki page provides a comprehensive guide on how to extend Vercel by developing a custom Runtime. It covers the architecture, components, and data flow involved in the process, as well as the various APIs and utilities provided by Vercel to simplify the development of Runtimes.

## Runtime API Interface

The Runtime API interface is the core of developing a custom Runtime for Vercel. It defines the structure and methods that a Runtime must implement to be compatible with the Vercel platform. The interface is defined as follows:

```typescript
interface Runtime {
  version: number;
  build: (options: BuildOptions) => Promise<BuildResult>;
  prepareCache?: (options: PrepareCacheOptions) => Promise<CacheOutputs>;
  shouldServe?: (options: ShouldServeOptions) => Promise<boolean>;
  startDevServer?: (options: StartDevServerOptions) => Promise<StartDevServerResult>;
}
```

Sources: [DEVELOPING_A_RUNTIME.md:11-21]()

### `version`

The `version` property is a required constant that specifies the version of the Runtime API to use. The latest and suggested version is `3`.

```typescript
export const version = 3;
```

Sources: [DEVELOPING_A_RUNTIME.md:26-29]()

### `build()`

The `build()` function is a required exported function that returns a Serverless Function. It is the core of the Runtime implementation and is responsible for building the code and creating the necessary Serverless Function.

```typescript
export async function build(options: BuildOptions) {
  // Build the code here...

  const lambda = createLambda(/* ... */);
  return {
    output: lambda,
    routes: [
      // If your Runtime needs to define additional routing, define it here...
    ],
  };
}
```

Sources: [DEVELOPING_A_RUNTIME.md:34-46]()

### `prepareCache()`

The `prepareCache()` function is an optional exported function that is executed after the `build()` function. Its purpose is to return an object of `File` instances that will be pre-populated in the working directory for the next build run, improving the build performance.

```typescript
export const prepareCache: PrepareCache = async ({ workPath, repoRootPath }) => {
  // Create a mapping of file names and `File` object instances to cache here...
  const rootDirectory = relative(repoRootPath, workPath);
  const cache = await glob(`${rootDirectory}/some/dir/**`, repoRootPath);
  return cache;
};
```

Sources: [DEVELOPING_A_RUNTIME.md:51-62]()

### `shouldServe()`

The `shouldServe()` function is an optional exported function that is used by `vercel dev` in the Vercel CLI. It indicates whether the Runtime should be responsible for responding to a certain request path.

```typescript
export async function shouldServe(options: ShouldServeOptions) {
  // Determine whether or not the Runtime should respond to the request path here...

  return options.requestPath === options.entrypoint;
}
```

Sources: [DEVELOPING_A_RUNTIME.md:67-76]()

### `startDevServer()`

The `startDevServer()` function is an optional exported function that is also used by `vercel dev` in the Vercel CLI. If defined, it provides an optimized development experience by spawning a child process that creates an HTTP server to execute the entrypoint code when an HTTP request is received, instead of going through the entire `build()` process.

```typescript
export async function startDevServer(options: StartDevServerOptions) {
  // Create a child process which will create an HTTP server.
  const child = spawn('my-runtime-dev-server', [options.entrypoint], {
    stdio: ['ignore', 'inherit', 'inherit', 'pipe'],
  });

  // In this example, the child process will write the port number to FD 3...
  const portPipe = child.stdio[3];
  const childPort = await new Promise(resolve => {
    portPipe.setEncoding('utf8');
    portPipe.once('data', data => {
      resolve(Number(data));
    });
  });

  return { pid: child.pid, port: childPort };
}
```

Sources: [DEVELOPING_A_RUNTIME.md:81-103]()

## Execution Context

Runtimes are executed in a Linux container that closely matches the Serverless Function runtime environment. The Runtime code is executed using Node.js version 12.x, and a new sandbox is created for each deployment for security reasons. The sandbox is cleaned up between executions to ensure no lingering temporary files are shared from build to build.

Sources: [DEVELOPING_A_RUNTIME.md:107-112]()

## Directory and Cache Lifecycle

When a new build is created, the `workPath` supplied to the `analyze` function is pre-populated with the results of the `prepareCache` step from the previous build. The `analyze` step can modify this directory, and the changes will be carried over to the `build` and `prepareCache` steps.

Sources: [DEVELOPING_A_RUNTIME.md:114-117]()

## Accessing Environment and Secrets

The environment variables and secrets specified by the user as `build.env` are passed to the Runtime process, allowing access to them via `process.env` in Node.js.

Sources: [DEVELOPING_A_RUNTIME.md:119-121]()

## Supporting Large Environment

Vercel provides the ability to support more than 4KB of environment (up to 64KB) by using a Lambda runtime wrapper added to every Lambda function created. Several existing Lambda runtimes have built-in support for the runtime wrapper, but custom runtimes may require additional work.

To add support for runtime wrappers to a custom runtime, the `AWS_LAMBDA_EXEC_WRAPPER` environment variable should be checked in the bootstrap script. If it has a value, the wrapper executable should be called with the path to the runtime and any required parameters. If the bootstrap file is not a launcher script, the bootstrap process should be replaced with the wrapper, passing the path and parameters of the executing file.

Once support for runtime wrappers is included, the `supportsWrapper` flag should be set to `true` in the call to `createLambda()` to enable large environment support for the runtime.

Sources: [DEVELOPING_A_RUNTIME.md:123-158]()

## Utilities and Types

Vercel provides several utilities and types to simplify the development of Runtimes. These include:

### `Files` and `File`

The `Files` type is an abstract representation of a virtual filesystem, implemented as a plain JavaScript object. It can contain `FileRef`, `FileFsRef`, `FileBlob`, and `Lambda` instances.

```typescript
type Files = { [filePath: string]: File };
```

Sources: [DEVELOPING_A_RUNTIME.md:162-170]()

The `File` type is an abstract type that can be one of the following:

- `FileRef`: Represents an abstract file instance stored in the Vercel platform, based on a file identifier string (its checksum).
- `FileFsRef`: Represents an abstract instance of a file present in the filesystem that the build process is executing in.
- `FileBlob`: Represents an abstract instance of a file present in memory.

Sources: [DEVELOPING_A_RUNTIME.md:172-198]()

### `Lambda`

The `Lambda` class represents a Serverless Function. An instance can be created by supplying `files`, `handler`, `runtime`, and `environment` as an object to the `createLambda` helper function.

```typescript
import { Lambda } from '@vercel/build-utils';
```

Sources: [DEVELOPING_A_RUNTIME.md:200-209]()

### Helper Functions

Vercel provides several helper functions to simplify the development of Runtimes:

- `createLambda()`: Constructor for the `Lambda` type.
- `download()`: Allows downloading the contents of a `Files` data structure, creating the filesystem represented in it.
- `glob()`: Scans the filesystem and returns a `Files` representation of the matched glob search string.
- `getWritableDirectory()`: Returns a writable temporary directory.
- `rename()`: Renames the keys (paths) of the `Files` object.

Sources: [DEVELOPING_A_RUNTIME.md:213-271]()

## Conclusion

Extending Vercel by developing a custom Runtime involves implementing the Runtime API interface and leveraging the utilities and types provided by Vercel. This wiki page has covered the architecture, components, and data flow involved in the process, as well as the various APIs and utilities available to simplify the development of Runtimes. By following the guidelines and examples provided, developers can add support for new programming languages or frameworks to the Vercel platform.