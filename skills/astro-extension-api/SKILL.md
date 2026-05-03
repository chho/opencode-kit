---
name: astro-extension-api
description: Astro extension and plugin development APIs - Adapter API, Integration API hooks (astro:config:setup, astro:build:*), Content Loader API, Image Service API, Font Provider API, Dev Toolbar App API, Container API, Render Context, experimental flags, and all module-level API references (Actions, Assets, Content Collections, Env, i18n, Middleware, Static Paths, View Transitions, Zod, Programmatic API, Routing, Session Driver). Load this when building Astro integrations, adapters, or plugins.
---

# Astro Extension & API Development Documentation

> This skill contains low-level APIs for building Astro integrations, adapters, and plugins.


# Astro Adapter API

Astro is designed to make it easy to deploy to any cloud provider for on-demand rendering, also known as server-side rendering (SSR). This ability is provided by **adapters**, which are [integrations](/en/reference/integrations-reference/). See the [on-demand rendering guide](/en/guides/on-demand-rendering/) to learn how to use an existing adapter.

## What is an adapter?

[Section titled “What is an adapter?”](#what-is-an-adapter)

An adapter is a special kind of [integration](/en/reference/integrations-reference/) that provides an entrypoint for server rendering at request time. An adapter has access to the full Integration API and does two things:

* Implements host-specific APIs for handling requests.
* Configures the build according to host conventions.

## Building an adapter

[Section titled “Building an adapter”](#building-an-adapter)

Create an integration and call the `setAdapter()` function in the [`astro:config:done`](/en/reference/integrations-reference/#astroconfigdone) hook. This allows you to define a server entrypoint and the features supported by your adapter.

The following example creates an adapter with a server entrypoint and stable support for Astro static output:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          supportedAstroFeatures: {
            staticOutput: 'stable'
          }
        });
      },
    },
  };
}
```

The `setAdapter()` function accepts an object containing the following properties:

### `name`

[Section titled “name”](#name)

**Type:** `string`

Defines a unique name for your adapter. This will be used for logging.

### `entrypointResolution`

[Section titled “entrypointResolution”](#entrypointresolution)

**Type:** `"explicit" | "auto"`\
**Default**: `"explicit"`

**Added in:** `astro@6.0.0`

Deprecated

`entrypointResolution: "explicit"` is deprecated. `entrypointResolution: "auto"` will become the default, and only, behavior in a future major version.

See [how to migrate](/en/guides/upgrade-to/v6/#deprecated-createexports-and-start-adapter-api).

Specifies the method Astro will use to resolve the server entrypoint: `"auto"` (recommended) or `"explicit"` (default, but deprecated):

* **`"auto"` (recommended):** You are responsible for providing a valid module as an entrypoint using either [`serverEntrypoint`](/en/reference/adapter-reference/#serverentrypoint) or, if you need further customization at the Vite level using [`vite.build.rollupOptions.input`](https://rollupjs.org/configuration-options/#input).
* **`"explicit"` (deprecated)**: You must provide the exports required by the host in the server entrypoint using a `createExports()` function before passing them to `setAdapter()` as an [`exports`](#exports) list. This supports adapters built using the Astro 5 version of the Adapter API. By default, all adapters will receive this value to allow backwards compatibility. **However, no new adapters should be created with this value.** Existing adapters should override this default value with `"auto"` as soon as they are able to migrate to the new v6 API.

The following example defines the `entrypointResolution` and `serverEntrypoint` to tell Astro that a custom entrypoint is provided:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/custom-entrypoint.js',
        });
      },
    },
  };
}
```

The following example defines the `entrypointResolution` and Rollup options to tell Astro that a custom entrypoint is provided:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:setup': ({ updateConfig }) => {
        updateConfig({
          vite: {
            build: {
              rollupOptions: {
                input: '@example/my-adapter/custom-entrypoint.js'
              }
            }
          }
        })
      },
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
        });
      },
    },
  };
}
```

Learn more about how to [build a server entrypoint](#building-a-server-entrypoint).

### `serverEntrypoint`

[Section titled “serverEntrypoint”](#serverentrypoint)

**Type:** `string | URL`

Defines the entrypoint for on-demand rendering.

### `supportedAstroFeatures`

[Section titled “supportedAstroFeatures”](#supportedastrofeatures)

**Type:** `AstroAdapterFeatureMap`

**Added in:** `astro@3.0.0`

A map of Astro’s built-in features supported by the adapter. This allows Astro to determine which features an adapter supports, so appropriate error messages can be provided.

Discover the [available Astro features](#astro-features) that an adapter can configure.

### `adapterFeatures`

[Section titled “adapterFeatures”](#adapterfeatures)

**Type:** `AstroAdapterFeatures`

**Added in:** `astro@3.0.0`

An object that specifies which [adapter features that change the build output](#adapter-features) are supported by the adapter.

### `client`

[Section titled “client”](#client)

**Type:** `{ internalFetchHeaders?: Record<string, string> | () => Record<string, string>; assetQueryParams?: URLSearchParams; }`

**Added in:** `astro@5.15.0`

A configuration object for Astro’s client-side code.

#### `client.internalFetchHeaders`

[Section titled “client.internalFetchHeaders”](#clientinternalfetchheaders)

**Type:** `Record<string, string> | () => Record<string, string>`

Defines the headers to inject into Astro’s internal fetch calls (e.g. Actions, View Transitions, Server Islands, Prefetch). This can be an object of headers or a function that returns headers.

The following example retrieves a `DEPLOY_ID` from the environment variables and, if provided, returns an object with the header name as key and the deploy id as value:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ config, setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          client: {
            internalFetchHeaders: () => {
              const deployId = process.env.DEPLOY_ID;
              return deployId ? { 'Your-Header-ID': deployId } : {};
            },
          },
        });
      },
    },
  };
}
```

#### `client.assetQueryParams`

[Section titled “client.assetQueryParams”](#clientassetqueryparams)

**Type:** `URLSearchParams`

Defines the query parameters to append to all asset URLs (e.g. images, stylesheets, scripts). This is useful for adapters that need to track deployment versions or other metadata.

The following example retrieves a `DEPLOY_ID` from the environment variables and, if provided, returns an object with a custom search parameter name as key and the deploy id as value:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ config, setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          client: {
            assetQueryParams: process.env.DEPLOY_ID
              ? new URLSearchParams({ yourParam: process.env.DEPLOY_ID })
              : undefined,
          },
        });
      },
    },
  };
}
```

### `previewEntrypoint`

[Section titled “previewEntrypoint”](#previewentrypoint)

**Type:** `string | URL`

**Added in:** `astro@1.5.0`

Defines the path or ID of a module in the adapter’s package that is responsible for starting up the built server when `astro preview` is run.

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ config, setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          previewEntrypoint: '@example/my-adapter/preview.js',
        });
      },
    },
  };
}
```

### `args`

[Section titled “args”](#args)

**Type:** `any`

Deprecated

This property is deprecated and will be removed in a future major version. Learn how to [update your adapter with `entrypointResolution: "auto"`](/en/guides/upgrade-to/v6/#deprecated-createexports-and-start-adapter-api).

A JSON-serializable value that will be passed to the adapter’s server entrypoint at runtime. This is useful to pass an object containing build-time configuration (e.g. paths, secrets) to your server runtime code.

The following example defines an `args` object with a property that identifies where assets generated by Astro are located:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ config, setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'explicit',
          args: {
            assets: config.build.assets
          },
          serverEntrypoint: '@example/my-adapter/server.js'
        });
      },
    },
  };
}
```

### `exports`

[Section titled “exports”](#exports)

**Type:** `string[]`

Deprecated

This property is deprecated and will be removed in a future major version. Learn how to [update your adapter with `entrypointResolution: "auto"`](/en/guides/upgrade-to/v6/#deprecated-createexports-and-start-adapter-api).

Defines an array of named exports to use in conjunction with the `createExports()` function of your server entrypoint.

The following example assumes that `createExports()` provides an export named `handler`:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ config, setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'explicit',
          exports: ['handler'],
          serverEntrypoint: '@example/my-adapter/server.js'
        });
      },
    },
  };
}
```

## Custom prerenderer

[Section titled “Custom prerenderer”](#custom-prerenderer)

**Added in:** `astro@6.0.0`

Adapters can provide a custom prerenderer to control how pages are prerendered by using the [`setPrerenderer()`](/en/reference/integrations-reference/#setprerenderer-option) function in the `astro:build:start` hook.

The following example shows how an adapter can set a custom prerenderer:

my-adapter.mjs

```js
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:build:start': ({ setPrerenderer }) => {
        setPrerenderer((defaultPrerenderer) => ({
          name: 'my-prerenderer',
          async setup() {
            // Start a preview server
          },
          async getStaticPaths() {
            // Returns array of { pathname: string, route: RouteData }
            return defaultPrerenderer.getStaticPaths();
          },
          async render(request, { routeData }) {
            // request: Request, options: { routeData: RouteData }
            // Custom rendering logic, e.g. make HTTP requests to a preview server
            const response = await fetch(`http://localhost:4321${new URL(request.url).pathname}`);
            return response;
          },
          async teardown() {
            // Stop the preview server
          }
        }));
      },
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
        });
      },
    },
  };
}
```

The factory function receives the default prerenderer, allowing you to wrap or extend its behavior. This is useful when you only need to customize specific aspects of prerendering.

## Building a server entrypoint

[Section titled “Building a server entrypoint”](#building-a-server-entrypoint)

You will need to create a file that executes during server-side requests to enable on-demand rendering with your particular host. Astro’s adapter API attempts to work with any type of host and gives a flexible way to conform to the host APIs.

You can import and use [`createApp()`](/en/reference/modules/astro-app/#createapp) to access methods that allow you to work with standard [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) and [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) objects.

This file should conform to what the host expects. For example, some serverless hosts expect you to export an `handler()` function:

my-adapter/server.js

```js
import { createApp } from 'astro/app/entrypoint';


const app = createApp();


export async function handler(event, context) {
  // ...
}
```

Learn more about the methods and utilities available in the [Adapter Server Entrypoint API Reference](/en/reference/modules/astro-app/).

### Passing build-time configuration

[Section titled “Passing build-time configuration”](#passing-build-time-configuration)

If you need to access build-time configuration in your server entrypoint, you can pass via a virtual module. For example, your server might need to identify where assets generated by Astro are located.

First, create and register a Vite plugin to serialize the data:

my-adapter.mjs

```js
const VIRTUAL_MODULE_ID = 'virtual:@example/my-adapter:config';
const RESOLVED_VIRTUAL_MODULE_ID = '\0' + VIRTUAL_MODULE_ID;


function createConfigPlugin(config) {
  return {
    name: VIRTUAL_MODULE_ID,
    resolveId: {
      filter: {
        id: new RegExp(`^${VIRTUAL_MODULE_ID}$`),
      },
      handler() {
        return RESOLVED_VIRTUAL_MODULE_ID;
      },
    },
    load: {
      filter: {
        id: new RegExp(`^${RESOLVED_VIRTUAL_MODULE_ID}$`),
      },
      handler() {
        return `
          export const assets = ${JSON.stringify(config.build.assets)};
        `;
      },
    },
  };
}


export default function createIntegration() {
  let _config;
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:setup': ({ config, updateConfig }) => {
        _config = config;


        updateConfig({
          vite: {
            plugins: [createConfigPlugin(_config)]
          }
        })
      },
      'astro:config:done': ({ config, setAdapter }) => {
        _config = config;
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
        });
      },
    },
  };
}
```

You can create internal types if needed:

virtual.d.ts

```ts
declare module 'virtual:@example/my-adapter:config' {
  export const assets: string;
}
```

You can then import the virtual module:

my-adapter/server.js

```diff
import { createApp } from 'astro/app/entrypoint';
+import { assets } from 'virtual:@example/my-adapter:config';


const app = createApp();


export async function handler(event, context) {
  // ...
}
```

## Astro features

[Section titled “Astro features”](#astro-features)

Astro features are a way for an adapter to tell Astro whether they are able to support a feature, and also the adapter’s level of support.

When using these properties, Astro will:

* run specific validation;
* emit contextual information to the logs;

These operations are run based on the features supported or not supported, their level of support, the [desired amount of logging](#adaptersupportwithmessagesuppress), and the user’s own configuration.

The following configuration tells Astro that this adapter has experimental support for the Sharp-powered built-in image service:

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          supportedAstroFeatures: {
+            sharpImageService: 'experimental'
+          }
        });
      },
    },
  };
}
```

If the Sharp image service is used, Astro will log a warning and error to the terminal based on your adapter’s support:

```plaintext
[@example/my-adapter] The feature is experimental and subject to issues or changes.


[@example/my-adapter] The currently selected adapter `@example/my-adapter` is not compatible with the service "Sharp". Your project will NOT be able to build.
```

A message can additionally be provided to give more context to the user:

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          supportedAstroFeatures: {
+            sharpImageService: {
+              support: 'limited',
+              message: 'This adapter has limited support for Sharp. Certain features may not work as expected.'
+            }
+          }
        });
      },
    },
  };
}
```

This object contains the following configurable features:

### `staticOutput`

[Section titled “staticOutput”](#staticoutput)

**Type:** [`AdapterSupport`](#adaptersupport)

Defines whether the adapter is able to serve static pages.

### `hybridOutput`

[Section titled “hybridOutput”](#hybridoutput)

**Type:** [`AdapterSupport`](#adaptersupport)

Defines whether the adapter is able to serve sites that include a mix of static and on-demand rendered pages.

### `serverOutput`

[Section titled “serverOutput”](#serveroutput)

**Type:** [`AdapterSupport`](#adaptersupport)

Defines whether the adapter is able to serve on-demand rendered pages.

### `i18nDomains`

[Section titled “i18nDomains”](#i18ndomains)

**Type:** [`AdapterSupport`](#adaptersupport)

**Added in:** `astro@4.3.0`

Defines whether the adapter is able to support i18n domains.

### `envGetSecret`

[Section titled “envGetSecret”](#envgetsecret)

**Type:** [`AdapterSupport`](#adaptersupport)

**Added in:** `astro@4.10.0`

Defines whether the adapter is able to support `getSecret()` exported from [`astro:env/server`](/en/reference/modules/astro-env/). When enabled, this feature allows your adapter to retrieve secrets configured by users in `env.schema`.

The following example enables the feature by passing [a valid `AdapterSupportsKind` value](#adaptersupportskind) to the adapter:

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          supportedAstroFeatures: {
+            envGetSecret: 'stable'
+          }
        });
      },
    },
  };
}
```

The `astro/env/setup` module allows you to provide an implementation for `getSecret()`. In [your server entrypoint](#building-a-server-entrypoint), call `setGetEnv()` as soon as possible:

```diff
import { createApp } from 'astro/app/entrypoint';
+import { setGetEnv } from "astro/env/setup"


+setGetEnv((key) => process.env[key])


const app = createApp();


export async function handler(event, context) {
  // ...
}
```

If the adapter supports secrets, be sure to call `setGetEnv()` before `getSecret()` when environment variables are tied to the request:

```diff
import { createApp } from 'astro/app/entrypoint';
+import { setGetEnv } from 'astro/env/setup';


const app = createApp();


export default {
  async fetch(request: Request, env: Record<string, unknown>) {
    +setGetEnv((key) => env[key]);


    return await app.render(request);
  }
}
```

### `sharpImageService`

[Section titled “sharpImageService”](#sharpimageservice)

**Type:** [`AdapterSupport`](#adaptersupport)

**Added in:** `astro@5.0.0`

Defines whether the adapter supports image transformation using the built-in Sharp image service.

## Adapter features

[Section titled “Adapter features”](#adapter-features)

A set of features that changes the output of the emitted files. When an adapter opts in to these features, they will get additional information inside specific hooks and must implement the proper logic to handle the different output.

### `middlewareMode`

[Section titled “middlewareMode”](#middlewaremode)

**Type:** [`MiddlewareMode`](#middlewaremode-1)\
**Default:** `"classic"`

**Added in:** `astro@6.0.0`

Determines at which stage of the page lifecycle the middleware is executed, and how the middleware code is emitted in the build output.

The `classic` mode matches the default behavior of Astro. On prerendered pages, middleware is run at build time, and when the page is requested, the middleware is not run again. On dynamic pages, middleware is run at request time only. The middleware code is part of your server bundle.

In `edge` mode, the middleware code can be deployed independently from the server bundle, for example, as an edge function.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          adapterFeatures: {
+            middlewareMode: 'edge'
+          }
        });
      },
    },
  };
}
```

Then, consume the hook [`astro:build:ssr`](/en/reference/integrations-reference/#astrobuildssr), which will give you a `middlewareEntryPoint`, an `URL` to the physical file on the file system.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          adapterFeatures: {
            middlewareMode: 'edge'
          }
        });
      },


      +'astro:build:ssr': ({ middlewareEntryPoint }) => {
        +// remember to check if this property exits, it will be `undefined` if the adapter doesn't opt in to the feature
        +if (middlewareEntryPoint) {
          +createEdgeMiddleware(middlewareEntryPoint)
+        }
+      }
    },
  };
}


function createEdgeMiddleware(middlewareEntryPoint) {
  // emit a new physical file using your bundler
}
```

### `buildOutput`

[Section titled “buildOutput”](#buildoutput)

**Type:** `"static" | "server"`\
**Default:** `"server"`

**Added in:** `astro@5.0.0`

Allows you to force a specific output shape for the build. This can be useful for adapters that only work with a specific output type. For example, your adapter might expect a static website so it can create host-specific files. Defaults to `server` if not specified.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          adapterFeatures: {
+            buildOutput: 'static'
+          }
        });
      },
    },
  };
}
```

### `staticHeaders`

[Section titled “staticHeaders”](#staticheaders)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

Whether or not the adapter provides support for setting response headers for static pages. When this feature is enabled, Astro will return a map of the `Headers` emitted by the static pages. This map is available as `routeToHeaders` in the [`astro:build:generated` hook](/en/reference/integrations-reference/#astrobuildgenerated) and can be used to generate platform-specific output that controls HTTP headers, for example, to create a `_headers` file for platforms that support it.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          adapterFeatures: {
+            staticHeaders: true,
+          },
        });
      },
      'astro:build:generated': ({ routeToHeaders }) => {
        // use `routeToHeaders` to generate a configuration file
        // for your virtual host of choice
      },
    },
  };
}
```

The value of the headers might change based on the features enabled/used by the application. For example, if [CSP is enabled](/en/reference/configuration-reference/#securitycsp), the `<meta http-equiv="content-security-policy">` element is not added to the static page. Instead, its `content` is available in the `routeToHeaders` map.

### `preserveBuildClientDir`

[Section titled “preserveBuildClientDir”](#preservebuildclientdir)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

When `true`, static builds will preserve the `client/server` directory structure instead of outputting directly to `outDir`. This ensures static builds use `build.client` for assets, maintaining consistency with server builds.

This is useful for adapters that require a specific directory structure regardless of the build output type, such as deploying to platforms with specific file organization requirements.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
+          adapterFeatures: {
+            preserveBuildClientDir: true,
+          },
        });
      },
    },
  };
}
```

## Adapter types reference

[Section titled “Adapter types reference”](#adapter-types-reference)

### `AdapterSupport`

[Section titled “AdapterSupport”](#adaptersupport)

**Type:** `AdapterSupportsKind | AdapterSupportWithMessage`

**Added in:** `astro@5.0.0`

A union of valid formats to describe the support level for a feature.

### `AdapterSupportsKind`

[Section titled “AdapterSupportsKind”](#adaptersupportskind)

**Type:** `"deprecated" | "experimental" | "limited" | "stable" | "unsupported"`

Defines the level of support for a feature by your adapter:

* Use `"deprecated"` when your adapter deprecates support for a feature before removing it completely in a future version.
* Use `"experimental"` when your adapter adds support for a feature, but issues or breaking changes are expected.
* Use `"limited"` when your adapter only supports a subset of the full feature.
* Use `"stable"` when the feature is fully supported by your adapter.
* Use `"unsupported"` to warn users that they may encounter build issues in their project, as this feature is not supported by your adapter.

### `AdapterSupportWithMessage`

[Section titled “AdapterSupportWithMessage”](#adaptersupportwithmessage)

**Added in:** `astro@5.0.0`

An object that allows you to define a support level for a feature and a message to be logged in the user console. This object contains the following properties:

#### `AdapterSupportWithMessage.support`

[Section titled “AdapterSupportWithMessage.support”](#adaptersupportwithmessagesupport)

**Type:** `Exclude<AdapterSupportsKind, “stable”>`

Defines the level of support for a feature by your adapter.

#### `AdapterSupportWithMessage.message`

[Section titled “AdapterSupportWithMessage.message”](#adaptersupportwithmessagemessage)

**Type:** `string`

Defines a custom message to log regarding the support of a feature by your adapter.

#### `AdapterSupportWithMessage.suppress`

[Section titled “AdapterSupportWithMessage.suppress”](#adaptersupportwithmessagesuppress)

**Type:** `"default" | "all"`

**Added in:** `astro@5.9.0`

An option to prevent showing some or all logged messages about an adapter’s support for a feature.

If Astro’s default log message is redundant, or confusing to the user in combination with your [custom `message`](#adaptersupportwithmessagemessage), you can use `suppress: "default"` to suppress the default message and only log your message:

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          supportedAstroFeatures: {
            sharpImageService: {
              support: 'limited',
              message: 'The adapter has limited support for Sharp. It will be used for images during build time, but will not work at runtime.',
+              suppress: 'default' // custom message is more detailed than the default
            }
          }
        });
      },
    },
  };
}
```

You can also use `suppress: "all"` to suppress all messages about support for the feature. This is useful when these messages are unhelpful to users in a specific context, such as when they have a configuration setting that means they are not using that feature. For example, you can choose to prevent logging any messages about Sharp support from your adapter:

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@example/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@example/my-adapter',
          entrypointResolution: 'auto',
          serverEntrypoint: '@example/my-adapter/server.js',
          supportedAstroFeatures: {
            sharpImageService: {
              support: 'limited',
              message: 'This adapter has limited support for Sharp. Certain features may not work as expected.',
+              suppress: 'all'
            }
          }
        });
      },
    },
  };
}
```

### `MiddlewareMode`

[Section titled “MiddlewareMode”](#middlewaremode-1)

**Type:** `"classic" | "edge"`

**Added in:** `astro@6.0.0`

A union of valid formats to describe the mode in which middleware is run.

## Allow installation via `astro add`

[Section titled “Allow installation via astro add”](#allow-installation-via-astro-add)

[The `astro add` command](/en/reference/cli-reference/#astro-add) allows users to easily add integrations and adapters to their project. To allow your adapter to be installed with this command, **add `astro-adapter` to the `keywords` field in your `package.json`**:

```json
{
  "name": "example",
  "keywords": ["astro-adapter"],
}
```

Once you [publish your adapter to npm](https://docs.npmjs.com/cli/v8/commands/npm-publish), running `astro add example` will install your package with any peer dependencies specified in your `package.json` and instruct users to update their project config manually.

# Astro render context

When rendering a page, Astro provides a runtime API specific to the current render. This includes useful information such as the current page URL as well as APIs to perform actions like redirecting to another page.

In `.astro` components, this context is available from the `Astro` global object. Endpoint functions are also called with this same context object as their first argument, whose properties mirror the Astro global properties.

Some properties are only available for routes rendered on demand or may have limited functionality on prerendered pages.

The `Astro` global object is available to all `.astro` files. Use the `context` object in [endpoint functions](/en/guides/endpoints/) to serve static or live server endpoints and in [middleware](/en/guides/middleware/) to inject behavior when a page or endpoint is about to be rendered.

## The context object

[Section titled “The context object”](#the-context-object)

The following properties are available on the `Astro` global (e.g. `Astro.props`, `Astro.redirect()`) and are also available on the context object (e.g. `context.props`, `context.redirect()`) passed to endpoint functions and middleware.

### `props`

[Section titled “props”](#props)

`props` is an object containing any values that have been passed as [component attributes](/en/basics/astro-components/#component-props).

src/components/Heading.astro

```astro
---
const { title, date } = Astro.props;
---
<div>
  <h1>{title}</h1>
  <p>{date}</p>
</div>
```

src/pages/index.astro

```astro
---
import Heading from '../components/Heading.astro';
---
<Heading title="My First Post" date="09 Aug 2022" />
```

Learn more about how [Markdown and MDX layouts](/en/guides/markdown-content/#frontmatter-layout-property) handle props.

The `props` object also contains any `props` passed from `getStaticPaths()` when rendering static routes.

* Astro.props

  src/pages/posts/\[id].astro

  ```astro
  ---
  export function getStaticPaths() {
    return [
      { params: { id: '1' }, props: { author: 'Blu' } },
      { params: { id: '2' }, props: { author: 'Erika' } },
      { params: { id: '3' }, props: { author: 'Matthew' } }
    ];
  }


  const { id } = Astro.params;
  const { author } = Astro.props;
  ---
  ```

* context.props

  src/pages/posts/\[id].json.ts

  ```ts
  import type { APIContext } from 'astro';


  export function getStaticPaths() {
    return [
      { params: { id: '1' }, props: { author: 'Blu' } },
      { params: { id: '2' }, props: { author: 'Erika' } },
      { params: { id: '3' }, props: { author: 'Matthew' } }
    ];
  }


  export function GET({ props }: APIContext) {
    return new Response(
      JSON.stringify({ author: props.author }),
    );
  }
  ```

See also: [Data Passing with `props`](/en/reference/routing-reference/#data-passing-with-props)

### `params`

[Section titled “params”](#params)

`params` is an object containing the values of dynamic route segments matched for a request. Its keys must match the [parameters](/en/guides/routing/#dynamic-routes) in the page or endpoint file path.

In static builds, this will be the `params` returned by `getStaticPaths()` used for prerendering [dynamic routes](/en/guides/routing/#dynamic-routes):

* Astro.params

  src/pages/posts/\[id].astro

  ```astro
  ---
  export function getStaticPaths() {
    return [
      { params: { id: '1' } },
      { params: { id: '2' } },
      { params: { id: '3' } }
    ];
  }
  const { id } = Astro.params;
  ---
  <h1>{id}</h1>
  ```

* context.params

  src/pages/posts/\[id].json.ts

  ```ts
  import type { APIContext } from 'astro';


  export function getStaticPaths() {
    return [
      { params: { id: '1' } },
      { params: { id: '2' } },
      { params: { id: '3' } }
    ];
  }


  export function GET({ params }: APIContext) {
    return new Response(
      JSON.stringify({ id: params.id }),
    );
  }
  ```

When routes are rendered on demand, `params` can be any value matching the path segments in the dynamic route pattern.

src/pages/posts/\[id].astro

```astro
---
import { getPost } from '../api';


const post = await getPost(Astro.params.id);


// No posts found with this ID
if (!post) {
  return Astro.redirect("/404")
}
---
<html>
  <h1>{post.name}</h1>
</html>
```

See also: [`params`](/en/reference/routing-reference/#params)

### `url`

[Section titled “url”](#url)

**Type:** `URL`

**Added in:** `astro@1.0.0`

`url` is a [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) object constructed from the current `request.url` value. It is useful for interacting with individual properties of the request URL, like pathname and origin.

`Astro.url` is equivalent to doing `new URL(Astro.request.url)`.

`url` will be a `localhost` URL in dev mode. When building a site, prerendered routes will receive a URL based on the [`site`](/en/reference/configuration-reference/#site) and [`base`](/en/reference/configuration-reference/#base) options. If `site` is not configured, prerendered pages will receive a `localhost` URL during builds as well.

src/pages/index.astro

```astro
<h1>The current URL is: {Astro.url}</h1>
<h1>The current URL pathname is: {Astro.url.pathname}</h1>
<h1>The current URL origin is: {Astro.url.origin}</h1>
```

You can also use `url` to create new URLs by passing it as an argument to [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL).

src/pages/index.astro

```astro
---
// Example: Construct a canonical URL using your production domain
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
// Example: Construct a URL for SEO meta tags using your current domain
const socialImageURL = new URL('/images/preview.png', Astro.url);
---
<link rel="canonical" href={canonicalURL} />
<meta property="og:image" content={socialImageURL} />
```

### `site`

[Section titled “site”](#site)

**Type:** `URL | undefined`

`site` returns a `URL` made from `site` in your Astro config. It returns `undefined` if you have not set a value for [`site`](/en/reference/configuration-reference/#site) in your Astro config.

src/pages/index.astro

```astro
<link
    rel="alternate"
    type="application/rss+xml"
    title="Your Site's Title"
    href={new URL("rss.xml", Astro.site)}
/>
```

### `clientAddress`

[Section titled “clientAddress”](#clientaddress)

**Type:** `string`

**Added in:** `astro@1.0.0`

`clientAddress` specifies the [IP address](https://en.wikipedia.org/wiki/IP_address) of the request. This property is only available for routes rendered on demand and cannot be used on prerendered pages.

* Astro.clientAddress

  src/pages/ip-address.astro

  ```astro
  ---
  export const prerender = false; // Not needed in 'server' mode
  ---


  <div>Your IP address is: <span class="address">{Astro.clientAddress}</span></div>
  ```

* context.clientAddress

  src/pages/ip-address.ts

  ```ts
  export const prerender = false; // Not needed in 'server' mode
  import type { APIContext } from 'astro';


  export function GET({ clientAddress }: APIContext) {
    return new Response(`Your IP address is: ${clientAddress}`);
  }
  ```

### `isPrerendered`

[Section titled “isPrerendered”](#isprerendered)

**Type**: `boolean`

**Added in:** `astro@5.0.0`

A boolean representing whether or not the current page is prerendered.

You can use this property to run conditional logic in middleware, for example, to avoid accessing headers in prerendered pages.

### `generator`

[Section titled “generator”](#generator)

**Type:** `string`

**Added in:** `astro@1.0.0`

`generator` provides the current version of Astro your project is running. This is a convenient way to add a [`<meta name="generator">`](https://html.spec.whatwg.org/multipage/semantics.html#meta-generator) tag with your current version of Astro. It follows the format `"Astro v5.x.x"`.

* Astro.generator

  src/pages/site-info.astro

  ```astro
  <html>
    <head>
      <meta name="generator" content={Astro.generator} />
    </head>
    <body>
      <footer>
        <p>Built with <a href="https://astro.build">{Astro.generator}</a></p>
      </footer>
    </body>
  </html>
  ```

* context.generator

  src/pages/site-info.json.ts

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ generator, site }: APIContext) {
    const body = JSON.stringify({ generator, site });
    return new Response(body);
  }
  ```

### `request`

[Section titled “request”](#request)

**Type:** `Request`

`request` is a standard [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) object. It can be used to get the `url`, `headers`, `method`, and even the body of the request.

* Astro.request

  src/pages/index.astro

  ```astro
  <p>Received a {Astro.request.method} request to "{Astro.request.url}".</p>
  <p>Received request headers:</p>
  <p><code>{JSON.stringify(Object.fromEntries(Astro.request.headers))}</code></p>
  ```

* context.request

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ request }: APIContext) {
    return new Response(`Hello ${request.url}`);
  }
  ```

Note

On prerendered pages, `request.url` does not contain search parameters, like `?type=new`, as it’s not possible to determine them ahead of time during static builds. However, `request.url` does contain search parameters for pages rendered on-demand as they can be determined from a server request.

### `response`

[Section titled “response”](#response)

**Type:** `ResponseInit & { readonly headers: Headers }`

`response` is a standard `ResponseInit` object. It has the following structure.

* `status`: The numeric status code of the response, e.g., `200`.
* `statusText`: The status message associated with the status code, e.g., `'OK'`.
* `headers`: A [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers) instance that you can use to set the HTTP headers of the response.

`Astro.response` is used to set the `status`, `statusText`, and `headers` for a page’s response.

```astro
---
if (condition) {
  Astro.response.status = 404;
  Astro.response.statusText = 'Not found';
}
---
```

Or to set a header:

```astro
---
Astro.response.headers.set('Set-Cookie', 'a=b; Path=/;');
---
```

### `redirect()`

[Section titled “redirect()”](#redirect)

**Type:** `(path: string, status?: number) => Response`

**Added in:** `astro@1.5.0`

`redirect()` returns a [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) object that allows you to redirect to another page, and optionally provide an [HTTP response status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) as a second parameter.

A page (and not a child component) must `return` the result of `Astro.redirect()` for the redirect to occur.

For statically-generated routes, this will produce a client redirect using a [`<meta http-equiv="refresh">` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#http-equiv) and does not support status codes.

For on-demand rendered routes, setting a custom status code is supported when redirecting. If not specified, redirects will be served with a `302` status code.

The following example redirects a user to a login page:

* Astro.redirect()

  src/pages/account.astro

  ```astro
  ---
  import { isLoggedIn } from '../utils';


  const cookie = Astro.request.headers.get('cookie');


  // If the user is not logged in, redirect them to the login page
  if (!isLoggedIn(cookie)) {
    return Astro.redirect('/login');
  }
  ---


  <p>User information</p>
  ```

* context.redirect()

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ redirect, request }: APIContext) {
    const cookie = request.headers.get('cookie');
    if (!isLoggedIn(cookie)) {
      return redirect('/login', 302);
    } else {
      // return user information
    }
  }
  ```

### `rewrite()`

[Section titled “rewrite()”](#rewrite)

**Type:** `(rewritePayload: string | URL | Request) => Promise<Response>`

**Added in:** `astro@4.13.0`

`rewrite()` allows you to serve content from a different URL or path without redirecting the browser to a new page.

The method accepts either a string, a `URL`, or a `Request` for the location of the path.

Use a string to provide an explicit path:

* Astro.rewrite()

  src/pages/index.astro

  ```astro
  ---
  return Astro.rewrite("/login")
  ---
  ```

* context.rewrite()

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ rewrite }: APIContext) {
    return rewrite('/login');
  }
  ```

Use a `URL` type when you need to construct the URL path for the rewrite. The following example renders a page’s parent path by creating a new URL from the relative `"../"` path:

* Astro.rewrite()

  src/pages/blog/index.astro

  ```astro
  ---
  return Astro.rewrite(new URL("../", Astro.url))
  ---
  ```

* context.rewrite()

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ rewrite }: APIContext) {
    return rewrite(new URL("../", Astro.url));
  }
  ```

Use a `Request` type for complete control of the `Request` sent to the server for the new path. The following example sends a request to render the parent page while also providing headers:

* Astro.rewrite()

  src/pages/blog/index.astro

  ```astro
  ---
  return Astro.rewrite(new Request(new URL("../", Astro.url), {
    headers: {
      "x-custom-header": JSON.stringify(Astro.locals.someValue)
    }
  }))
  ---
  ```

* context.rewrite()

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ rewrite }: APIContext) {
    return rewrite(new Request(new URL("../", Astro.url), {
      headers: {
        "x-custom-header": JSON.stringify(Astro.locals.someValue)
      }
    }));
  }
  ```

### `originPathname`

[Section titled “originPathname”](#originpathname)

**Type:** `string`

**Added in:** `astro@5.0.0`

`originPathname` defines the original pathname of the request, before rewrites were applied.

* Astro.originPathname

  src/pages/404.astro

  ```astro
  <p>The origin path is {Astro.originPathname}</p>
  <p>The rewritten path is {Astro.url.pathname}</p>
  ```

* context.originPathname

  src/middleware.ts

  ```ts
  import { defineMiddleware } from 'astro:middleware';


  export const onRequest = defineMiddleware(async (context, next) => {
    // Record the original pathname before any rewrites
    recordPageVisit(context.originPathname);
    return next();
  });
  ```

### `locals`

[Section titled “locals”](#locals)

**Added in:** `astro@2.4.0`

`locals` is an object used to store and access arbitrary information during the lifecycle of a request. `Astro.locals` is an object containing any values from the `context.locals` object set by middleware. Use this to access data returned by middleware in your `.astro` files.

Middleware functions can both read and write the values of `context.locals`:

src/middleware.ts

```ts
import type { MiddlewareHandler } from 'astro';


export const onRequest: MiddlewareHandler = ({ locals }, next) => {
  if (!locals.title) {
    locals.title = "Default Title";
  }
  return next();
}
```

Astro components and API endpoints can read values from `locals` when they render:

* Astro.locals

  src/pages/Orders.astro

  ```astro
  ---
  const title = Astro.locals.title;
  ---
  <h1>{title}</h1>
  ```

* context.locals

  src/pages/hello.ts

  ```ts
  import type { APIContext } from 'astro';


  export function GET({ locals }: APIContext) {
    return new Response(locals.title); // "Default Title"
  }
  ```

### `preferredLocale`

[Section titled “preferredLocale”](#preferredlocale)

**Type:** `string | undefined`

**Added in:** `astro@3.5.0`

`preferredLocale` is a computed value to find the best match between your visitor’s browser language preferences and the locales supported by your site.

It is computed by checking the configured locales in your [`i18n.locales`](/en/reference/configuration-reference/#i18nlocales) array and the locales supported by the user’s browser via the header `Accept-Language`. This value is `undefined` if no such match exists.

This property is only available for routes rendered on demand and cannot be used on prerendered, static pages.

### `preferredLocaleList`

[Section titled “preferredLocaleList”](#preferredlocalelist)

**Type:** `string[] | undefined`

**Added in:** `astro@3.5.0`

`preferredLocaleList` represents the array of all locales that are both requested by the browser and supported by your website. This produces a list of all compatible languages between your site and your visitor.

If none of the browser’s requested languages are found in your locales array, then the value is `[]`. This occurs when you do not support any of your visitor’s preferred locales.

If the browser does not specify any preferred languages, then this value will be [`i18n.locales`](/en/reference/configuration-reference/#i18nlocales): all of your supported locales will be considered equally preferred by a visitor with no preferences.

This property is only available for routes rendered on demand and cannot be used on prerendered, static pages.

### `currentLocale`

[Section titled “currentLocale”](#currentlocale)

**Type:** `string | undefined`

**Added in:** `astro@3.5.6`

The locale computed from the current URL, using the syntax specified in your `locales` configuration. If the URL does not contain a `/[locale]/` prefix, then the value will default to [`i18n.defaultLocale`](/en/reference/configuration-reference/#i18ndefaultlocale).

### `getActionResult()`

[Section titled “getActionResult()”](#getactionresult)

**Type:** `(action: TAction) => ActionReturnType<TAction> | undefined`

**Added in:** `astro@4.15.0`

`getActionResult()` is a function that returns the result of an [Action](/en/guides/actions/) submission. This accepts an action function as an argument (e.g. `actions.logout`) and returns a `data` or `error` object when a submission is received. Otherwise, it will return `undefined`.

src/pages/index.astro

```astro
---
import { actions } from 'astro:actions';


const result = Astro.getActionResult(actions.logout);
---


<form action={actions.logout}>
  <button type="submit">Log out</button>
</form>
{result?.error && <p>Failed to log out. Please try again.</p>}
```

### `callAction()`

[Section titled “callAction()”](#callaction)

**Added in:** `astro@4.15.0`

`callAction()` is a function used to call an Action handler directly from your Astro component. This function accepts an Action function as the first argument (e.g. `actions.logout`) and any input that action receives as the second argument. It returns the result of the action as a promise.

src/pages/index.astro

```astro
---
import { actions } from 'astro:actions';


const { data, error } = await Astro.callAction(actions.logout, { userId: '123' });
---
```

### `routePattern`

[Section titled “routePattern”](#routepattern)

**Type**: `string`

**Added in:** `astro@5.0.0`

The route pattern responsible for generating the current page or route. In file-based routing, this resembles the file path in your project used to create the route. When integrations create routes for your project, `context.routePattern` is identical to the value for `injectRoute.pattern`.

The value will start with a leading slash and look similar to the path of a page component relative to your `src/pages/` folder without a file extension.

For example, the file `src/pages/en/blog/[slug].astro` will return `/en/blog/[slug]` for `routePattern`. Every page on your site generated by that file (e.g. `/en/blog/post-1/`, `/en/blog/post-2/`, etc.) shares the same value for `routePattern`. In the case of `index.*` routes, the route pattern will not include the word “index.” For example, `src/pages/index.astro` will return `/`.

You can use this property to understand which route is rendering your component. This allows you to target or analyze similarly-generated page URLs together. For example, you can use it to conditionally render certain information, or collect metrics about which routes are slower.

### `cookies`

[Section titled “cookies”](#cookies)

**Type:** `AstroCookies`

**Added in:** `astro@1.4.0`

`cookies` contains utilities for reading and manipulating cookies for [routes rendered on demand](/en/guides/on-demand-rendering/).

#### Cookie utilities

[Section titled “Cookie utilities”](#cookie-utilities)

##### `cookies.get()`

[Section titled “cookies.get()”](#cookiesget)

**Type:** `(key: string, options?: AstroCookieGetOptions) => AstroCookie | undefined`

Gets the cookie as an [`AstroCookie`](#astrocookie-type) object, which contains the `value` and utility functions for converting the cookie to non-string types.

##### `cookies.has()`

[Section titled “cookies.has()”](#cookieshas)

**Type:** `(key: string, options?: AstroCookieGetOptions) => boolean`

Whether this cookie exists. If the cookie has been set via `Astro.cookies.set()` this will return true, otherwise, it will check cookies in the `Astro.request`.

##### `cookies.set()`

[Section titled “cookies.set()”](#cookiesset)

**Type:** `(key: string, value: string | object, options?: AstroCookieSetOptions) => void`

Sets the cookie `key` to the given value. This will attempt to convert the cookie value to a string. Options provide ways to set [cookie features](https://www.npmjs.com/package/cookie#options-1), such as the `maxAge` or `httpOnly`.

##### `cookies.delete()`

[Section titled “cookies.delete()”](#cookiesdelete)

**Type:** `(key: string, options?: AstroCookieDeleteOptions) => void`

Invalidates a cookie by setting the expiration date in the past (0 in Unix time).

Once a cookie is “deleted” (expired), `Astro.cookies.has()` will return `false` and `Astro.cookies.get()` will return an [`AstroCookie`](#astrocookie-type) with a `value` of `undefined`. Options available when deleting a cookie are: `domain`, `path`, `httpOnly`, `sameSite`, and `secure`.

##### `cookies.merge()`

[Section titled “cookies.merge()”](#cookiesmerge)

**Type:** `(cookies: AstroCookies) => void`

Merges a new `AstroCookies` instance into the current instance. Any new cookies will be added to the current instance and any cookies with the same name will overwrite existing values.

##### `cookies.headers()`

[Section titled “cookies.headers()”](#cookiesheaders)

**Type:** `() => Iterator<string>`

Gets the header values for `Set-Cookie` that will be sent out with the response.

#### `AstroCookie` Type

[Section titled “AstroCookie Type”](#astrocookie-type)

The type returned from getting a cookie via `Astro.cookies.get()`. It has the following properties:

##### `AstroCookie.value`

[Section titled “AstroCookie.value”](#astrocookievalue)

**Type:** `string`

The raw string value of the cookie.

##### `AstroCookie.json()`

[Section titled “AstroCookie.json()”](#astrocookiejson)

**Type:** `() => Record<string, any>`

Parses the cookie value via `JSON.parse()`, returning an object. Throws if the cookie value is not valid JSON.

##### `AstroCookie.number()`

[Section titled “AstroCookie.number()”](#astrocookienumber)

**Type:** `() => number`

Parses the cookie value as a Number. Returns NaN if not a valid number.

##### `AstroCookie.boolean()`

[Section titled “AstroCookie.boolean()”](#astrocookieboolean)

**Type:** `() => boolean`

Converts the cookie value to a boolean.

#### `AstroCookieGetOptions`

[Section titled “AstroCookieGetOptions”](#astrocookiegetoptions)

**Added in:** `astro@4.1.0`

The `AstroCookieGetOption` interface allows you to specify options when you get a cookie.

##### `AstroCookieGetOptions.decode()`

[Section titled “AstroCookieGetOptions.decode()”](#astrocookiegetoptionsdecode)

**Type:** `(value: string) => string`

Allows customization of how a cookie is deserialized into a value.

#### `AstroCookieSetOptions`

[Section titled “AstroCookieSetOptions”](#astrocookiesetoptions)

**Added in:** `astro@4.1.0`

`AstroCookieSetOptions` is an object that can be passed to `Astro.cookies.set()` when setting a cookie to customize how the cookie is serialized.

##### `AstroCookieSetOptions.domain`

[Section titled “AstroCookieSetOptions.domain”](#astrocookiesetoptionsdomain)

**Type:** `string`

Specifies the domain. If no domain is set, most clients will interpret to apply to the current domain.

##### `AstroCookieSetOptions.expires`

[Section titled “AstroCookieSetOptions.expires”](#astrocookiesetoptionsexpires)

**Type:** `Date`

Specifies the date on which the cookie will expire.

##### `AstroCookieSetOptions.httpOnly`

[Section titled “AstroCookieSetOptions.httpOnly”](#astrocookiesetoptionshttponly)

**Type:** `boolean`

If true, the cookie will not be accessible client-side.

##### `AstroCookieSetOptions.maxAge`

[Section titled “AstroCookieSetOptions.maxAge”](#astrocookiesetoptionsmaxage)

**Type:** `number`

Specifies a number, in seconds, for which the cookie is valid.

##### `AstroCookieSetOptions.path`

[Section titled “AstroCookieSetOptions.path”](#astrocookiesetoptionspath)

**Type:** `string`

Specifies a subpath of the domain in which the cookie is applied.

##### `AstroCookieSetOptions.partitioned`

[Section titled “AstroCookieSetOptions.partitioned”](#astrocookiesetoptionspartitioned)

**Type:** `boolean`

**Added in:** `astro@5.17.0`

If true, the cookie is a [partitioned cookie](https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Privacy_sandbox/Partitioned_cookies). Partitioned cookies can only be read within the context of the top-level site on which they were set, which allows cross-site tracking to be blocked while still enabling legitimate uses of third-party cookies.

Partitioned cookies must be set with `secure: true`.

##### `AstroCookieSetOptions.sameSite`

[Section titled “AstroCookieSetOptions.sameSite”](#astrocookiesetoptionssamesite)

**Type:** `boolean | 'lax' | 'none' | 'strict'`

Specifies the value of the [SameSite](https://datatracker.ietf.org/doc/html/draft-ietf-httpbis-rfc6265bis-09#section-5.4.7) cookie header.

##### `AstroCookieSetOptions.secure`

[Section titled “AstroCookieSetOptions.secure”](#astrocookiesetoptionssecure)

**Type:** `boolean`

If true, the cookie is only set on https sites.

##### `AstroCookieSetOptions.encode()`

[Section titled “AstroCookieSetOptions.encode()”](#astrocookiesetoptionsencode)

**Type:** `(value: string) => string`

Allows customizing how the cookie is serialized.

### `session`

[Section titled “session”](#session)

**Type:** `AstroSession`

**Added in:** `astro@5.7.0`

`session` is an object that allows data to be stored between requests for [routes rendered on demand](/en/guides/on-demand-rendering/). It is associated with a cookie that contains the session ID only: the data itself is not stored in the cookie.

The session is created when first used, and the session cookie is automatically set. The `session` object is `undefined` if no session storage has been configured, or if the current route is prerendered, and will log an error if you try to use it.

See [the session guide](/en/guides/sessions/) for more information on how to use sessions in your Astro project.

#### `session.get()`

[Section titled “session.get()”](#sessionget)

**Type**: `(key: string) => Promise<any>`

Returns the value of the given key in the session. If the key does not exist, it returns `undefined`.

* Astro.session

  src/components/Cart.astro

  ```astro
  ---
  const cart = await Astro.session?.get('cart');
  ---
  <button>🛒 {cart?.length}</button>
  ```

* context.session

  src/pages/api/cart.ts

  ```ts
  import type { APIContext } from 'astro';


  export async function GET({ session }: APIContext) {
    const cart = await session.get('cart');
    return Response.json({ cart });
  }
  ```

#### `session.set()`

[Section titled “session.set()”](#sessionset)

**Type**: `(key: string, value: any, options?: { ttl: number }) => void`

Sets the value of the given key in the session. The value can be any serializable type. This method is synchronous and the value is immediately available for retrieval, but it is not saved to the backend until the end of the request. The `ttl` option sets the value’s expiration time, in seconds.

* Astro.session

  src/pages/products/\[slug].astro

  ```astro
  ---
  const { slug } = Astro.params;
  Astro.session?.set('lastViewedProduct', slug);
  ---
  ```

* context.session

  src/pages/api/add-to-cart.ts

  ```ts
  import type { APIContext } from 'astro';


  export async function POST({ session, request }: APIContext) {
    const cart = await session.get('cart');
    const newItem = await request.json();
    cart.push(newItem);
    // Save the updated cart to the session
    session.set('cart', cart);
    return Response.json({ cart });
  }
  ```

#### `session.regenerate()`

[Section titled “session.regenerate()”](#sessionregenerate)

**Type**: `() => void`

Regenerates the session ID. Call this when a user logs in or escalates their privileges, to prevent session fixation attacks.

* Astro.session

  src/pages/welcome.astro

  ```astro
  ---
  Astro.session?.regenerate();
  ---
  ```

* context.session

  src/pages/api/login.ts

  ```ts
  import type { APIContext } from 'astro';


  export async function POST({ session }: APIContext) {
    // Authenticate the user...
    doLogin();
    // Regenerate the session ID to prevent session fixation attacks
    session.regenerate();
    return Response.json({ success: true });
  }
  ```

#### `session.destroy()`

[Section titled “session.destroy()”](#sessiondestroy)

**Type**: `() => void`

Destroys the session, deleting the cookie and the object from the backend. Call this when a user logs out or their session is otherwise invalidated.

* Astro.session

  src/pages/logout.astro

  ```astro
  ---
  Astro.session?.destroy();
  return Astro.redirect('/login');
  ---
  ```

* context.session

  src/pages/api/logout.ts

  ```ts
  import type { APIContext } from 'astro';


  export async function POST({ session }: APIContext) {
    session.destroy();
    return Response.json({ success: true });
  }
  ```

#### `session.load()`

[Section titled “session.load()”](#sessionload)

**Type**: `(id: string) => Promise<void>`

Loads a session by ID. In normal use, a session is loaded automatically from the request cookie. Use this method to load a session from a different ID. This is useful if you are handling the session ID yourself, or if you want to keep track of a session without using cookies.

* Astro.session

  src/pages/cart.astro

  ```astro
  ---
  // Load the session from a header instead of cookies
  const sessionId = Astro.request.headers.get('x-session-id');
  await Astro.session?.load(sessionId);
  const cart = await Astro.session?.get('cart');
  ---
  <h1>Your cart</h1>
  <ul>
    {cart?.map((item) => (
      <li>{item.name}</li>
    ))}
  </ul>
  ```

* context.session

  src/pages/api/load-session.ts

  ```ts
  import type { APIRoute } from 'astro';


  export const GET: APIRoute = async ({ session, request }) => {
    // Load the session from a header instead of cookies
    const sessionId = request.headers.get('x-session-id');
    await session.load(sessionId);
    const cart = await session.get('cart');
    return Response.json({ cart });
  };
  ```

### `csp`

[Section titled “csp”](#csp)

**Type**: `object | undefined`

**Added in:** `astro@6.0.0`

Astro’s CSP runtime APIs enable support for [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) to help minimize certain types of security threats by controlling which resources a document is allowed to load. This provides additional protection against [cross-site scripting (XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) attacks.

You can customize the `<meta>` element per page from the `Astro` global inside `.astro` components, or the `APIContext` type in endpoints and middleware.

When resources are inserted multiple times or from multiple sources (e.g. defined in your [`csp` config](/en/reference/configuration-reference/#securitycsp) and added using the following CSP runtime APIs, Astro will merge and deduplicate all resources to create your `<meta>` element.

#### `csp.insertDirective()`

[Section titled “csp.insertDirective()”](#cspinsertdirective)

**Type:** `(directive: CspDirective) => void`

**Added in:** `astro@6.0.0`

Adds a single directive to the current page. You can call this method multiple times to add additional directives.

src/pages/index.astro

```astro
---
Astro.csp?.insertDirective("default-src 'self'");
Astro.csp?.insertDirective("img-src 'self' https://images.cdn.example.com");
---
```

After the build, the `<meta>` element for this individual page will incorporate your additional directives alongside the existing `script-src` and `style-src` directives:

```html
<meta
http-equiv="content-security-policy"
content="
  default-src 'self';
  img-src 'self' https://images.cdn.example.com;
  script-src 'self' 'sha256-somehash';
  style-src 'self' 'sha256-somehash';
  "
>
```

#### `csp.insertStyleResource()`

[Section titled “csp.insertStyleResource()”](#cspinsertstyleresource)

**Type:** `(resource: string) => void`

**Added in:** `astro@6.0.0`

Inserts a new resource to be used for the `style-src` directive.

src/pages/index.astro

```astro
---
Astro.csp?.insertStyleResource("https://styles.cdn.example.com");
---
```

After the build, the `<meta>` element for this individual page will add your source to the default `style-src` directive:

```html
<meta
http-equiv="content-security-policy"
content="
  script-src 'self' 'sha256-somehash';
  style-src https://styles.cdn.example.com 'sha256-somehash';
  "
>
```

#### `csp.insertStyleHash()`

[Section titled “csp.insertStyleHash()”](#cspinsertstylehash)

**Type:** `(hash: CspHash) => void`

**Added in:** `astro@6.0.0`

Adds a new hash to the `style-src` directive.

src/pages/index.astro

```astro
---
Astro.csp?.insertStyleHash("sha512-styleHash");
---
```

After the build, the `<meta>` element for this individual page will add your hash to the default `style-src` directive:

```html
<meta
http-equiv="content-security-policy"
content="
  script-src 'self' 'sha256-somehash';
  style-src 'self' 'sha256-somehash' 'sha512-styleHash';
  "
>
```

#### `csp.insertScriptResource()`

[Section titled “csp.insertScriptResource()”](#cspinsertscriptresource)

**Type:** `(resource: string) => void`

**Added in:** `astro@6.0.0`

Inserts a new valid source to be used for the `script-src` directive.

src/pages/index.astro

```astro
---
Astro.csp?.insertScriptResource("https://scripts.cdn.example.com");
---
```

After the build, the `<meta>` element for this individual page will add your source to the default `script-src` directive:

```html
<meta
http-equiv="content-security-policy"
content="
  script-src https://scripts.cdn.example.com 'sha256-somehash';
  style-src 'self' 'sha256-somehash';
  "
>
```

#### `csp.insertScriptHash()`

[Section titled “csp.insertScriptHash()”](#cspinsertscripthash)

**Type:** `(hash: CspHash) => void`

**Added in:** `astro@6.0.0`

Adds a new hash to the `script-src` directive.

src/pages/index.astro

```astro
---
Astro.csp?.insertScriptHash("sha512-scriptHash");
---
```

After the build, the `<meta>` element for this individual page will add your hash to the default `script-src` directive:

```html
<meta
http-equiv="content-security-policy"
content="
  script-src 'self' 'sha256-somehash' 'sha512-styleHash';
  style-src 'self' 'sha256-somehash';
  "
>
```


---

# Astro Container API (experimental)

**Added in:** `astro@4.9.0`

The Container API allows you to render Astro components in isolation.

This experimental server-side API unlocks a variety of potential future uses, but is currently scoped to allow [testing of `.astro` component output](/en/guides/testing/#vitest-and-container-api) in `vite` environments such as `vitest`.

It also allows you to [manually load rendering scripts](#adding-a-renderer-manually) for creating containers in pages rendered on demand or other “shell” environments outside of `vite` (e.g. inside a PHP or Elixir application).

This API allows you to [create a new container](#create), and render an Astro component returning [a string](#rendertostring) or a [`Response`](#rendertoresponse).

This API is experimental and subject to breaking changes, even in [minor or patch releases](/en/upgrade-astro/#semantic-versioning). Please consult [the Astro CHANGELOG](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for changes as they occur. This page will always be updated with the most current information for the latest version of Astro.

## `create()`

[Section titled “create()”](#create)

**Type:** `(options?: AstroContainerOptions) => Promise<experimental_AstroContainer>`

Creates a new instance of the container.

```js
import { experimental_AstroContainer } from "astro/container";


const container = await experimental_AstroContainer.create();
```

It accepts an object with the following options:

```ts
export type AstroContainerOptions = {
  streaming?: boolean;
  renderers?: AddServerRenderer[];
};


export type AddServerRenderer =
  | {
      renderer: NamedSSRLoadedRendererValue;
      name: never;
    }
  | {
      renderer: SSRLoadedRendererValue;
      name: string;
    };
```

### `streaming` option

[Section titled “streaming option”](#streaming-option)

**Type:** `boolean`\
**Default:** `false`

Enables rendering components using [HTML streaming](/en/guides/on-demand-rendering/#html-streaming).

### `renderers` option

[Section titled “renderers option”](#renderers-option)

**Type:** `AddServerRenderer[]`\
**Default:** `[]`

A list of loaded client renderers required by the component. Use this if your `.astro` component renders any [UI framework components](/en/guides/framework-components/) or MDX using an official Astro integration (e.g. React, Vue, etc.).

Renderers can be added through the Container API automatically for static applications, or cases where the container isn’t called at runtime (e.g. testing with `vitest`).

For [on-demand rendered applications](/en/guides/on-demand-rendering/), or cases where the container is called at runtime or inside other “shells” (e.g. PHP, Ruby, Java, etc.), renderers must be manually imported.

#### Adding a renderer through the Container API

[Section titled “Adding a renderer through the Container API”](#adding-a-renderer-through-the-container-api)

For each official Astro integration, import and use the `getContainerRenderer()` helper function to expose its client and server rendering scripts. These are available for `@astrojs/react`, `@astrojs/preact`, `@astrojs/solid-js`, `@astrojs/svelte`, `@astrojs/vue`, and `@astrojs/mdx`.

For renderer packages outside the `@astrojs` npm org, look in their documentation for `getContainerRenderer()` or a similar function provided.

When using `vite` (`vitest`, Astro integrations, etc.), the renderers are loaded with the function `loadRenderers()` from the virtual module `astro:container`.

Caution

Outside `vite` or for on-demand usage, you’ll have to [load the renderers manually](#adding-a-renderer-manually).

The following example provides the necessary object to render an Astro component that renders a React component and a Svelte component:

```js
import { getContainerRenderer as reactContainerRenderer } from "@astrojs/react";
import { getContainerRenderer as svelteContainerRenderer } from "@astrojs/svelte";
import { loadRenderers } from "astro:container";


const renderers = await loadRenderers([reactContainerRenderer(), svelteContainerRenderer()]);
const container = await experimental_AstroContainer.create({
    renderers
})
const result = await container.renderToString(ReactWrapper);
```

#### Adding a renderer manually

[Section titled “Adding a renderer manually”](#adding-a-renderer-manually)

When the container is called at runtime, or inside other “shells”, the `astro:container` virtual module’s helper functions are not available. You must import the necessary server and client renderers manually and store them inside the container using `addServerRenderer` and `addClientRenderer`.

Server renderers are required to build your project, and must be stored in the container for every framework used. Client renderers are additionally needed to any hydrate client-side components using [`client:*` directives](/en/reference/directives-reference/#client-directives).

Only one import statement is needed per framework. Importing a renderer makes both the server and client renderers available to your container. However, **server renderers must be added to your container before client renderers**. This allows your entire container to render first, and then hydrate any interactive components.

The following example manually imports the necessary server renderers to be able to display static Vue components and `.mdx` pages. It additionally adds both server and client renderers for interactive React components.

```js
import reactRenderer from "@astrojs/react/server.js";
import vueRenderer from "@astrojs/vue/server.js";
import mdxRenderer from "@astrojs/mdx/server.js";


const container = await experimental_AstroContainer.create();
container.addServerRenderer({ renderer: vueRenderer });
container.addServerRenderer({ renderer: mdxRenderer });


container.addServerRenderer({ renderer: reactRenderer });
container.addClientRenderer({ name: "@astrojs/react", entrypoint: "@astrojs/react/client.js" });
```

## `renderToString()`

[Section titled “renderToString()”](#rendertostring)

**Type:** `(component: AstroComponentFactory; options?: ContainerRenderOptions) => Promise<string>`

This function renders a specified component inside a container. It takes an Astro component as an argument and it returns a string that represents the HTML/content rendered by the Astro component.

```js
import { experimental_AstroContainer } from "astro/container";
import Card from "../src/components/Card.astro";


const container = await experimental_AstroContainer.create();
const result = await container.renderToString(Card);
```

Under the hood, this function calls [`renderToResponse()`](#rendertoresponse) and `Response.text()`.

It also accepts an object as a second argument that can contain a [number of options](#rendering-options).

## `renderToResponse()`

[Section titled “renderToResponse()”](#rendertoresponse)

**Type:** `(component: AstroComponentFactory; options?: ContainerRenderOptions) => Promise<Response>`

It renders a component, and it returns a `Response` object.

```js
import { experimental_AstroContainer } from "astro/container";
import Card from "../src/components/Card.astro";


const container = await experimental_AstroContainer.create();
const result = await container.renderToResponse(Card);
```

It also accepts an object as a second argument that can contain a [number of options](#rendering-options).

## Rendering options

[Section titled “Rendering options”](#rendering-options)

Both [`renderToResponse()`](#rendertoresponse) and [`renderToString()`](#rendertostring) accept an object as their second argument:

```ts
export type ContainerRenderOptions = {
  slots?: Record<string, any>;
  props?: Record<string, unknown>;
  request?: Request;
  params?: Record<string, string | undefined>;
  locals?: App.Locals;
  routeType?: RouteType;
  partial?: boolean;
};
```

These optional values can be passed to the rendering function in order to provide additional information necessary for an Astro component to properly render.

### `slots`

[Section titled “slots”](#slots)

**Type**: `Record<string, any>`

An option to pass content to be rendered with [`<slots>`](/en/basics/astro-components/#slots).

If your Astro component renders one default slot, pass an object with `default` as the key:

```js
import Card from "../src/components/Card.astro";


const result = await container.renderToString(Card, {
  slots: { default: "Some value" }
});
```

If your component renders named slots, use the slot names as the object keys:

```astro
---
---
<div>
  <slot name="header" />
  <slot name="footer" />
</div>
```

```js
import Card from "../src/components/Card.astro";


const result = await container.renderToString(Card, {
  slots: {
    header: "Header content",
    footer: "Footer"
  }
});
```

You can also render components in cascade:

```astro
---
---
<div>
  <slot name="header" />
  <slot name="footer" />
</div>
```

```js
import Card from "../src/components/Card.astro";
import CardHeader from "../src/components/CardHeader.astro";
import CardFooter from "../src/components/CardFooter.astro";


const result = await container.renderToString(Card, {
  slots: {
    header: await container.renderToString(CardHeader),
    footer:  await container.renderToString(CardFooter)
  }
});
```

### `props` option

[Section titled “props option”](#props-option)

**Type**: `Record<string, unknown>`

An option to pass [properties](/en/basics/astro-components/#component-props) for Astro components.

```js
import Card from "../src/components/Card.astro";


const result = await container.renderToString(Card, {
  props: { name: "Hello, world!" }
});
```

```astro
---
// For TypeScript support
interface Props {
  name: string;
};


const { name } = Astro.props;
---
<div>
  {name}
</div>
```

### `request` option

[Section titled “request option”](#request-option)

**Type**: `Request`

An option to pass a `Request` with information about the path/URL the component will render.

Use this option when your component needs to read information like `Astro.url` or `Astro.request`.

You can also inject possible headers or cookies.

```js
import Card from "../src/components/Card.astro";


const result = await container.renderToString(Card, {
  request: new Request("https://example.com/blog", {
    headers: {
      "x-some-secret-header": "test-value"
    }
  })
});
```

### `params` option

[Section titled “params option”](#params-option)

**Type**: `Record<string, string | undefined>`

An object to pass information about the path parameter to an Astro component responsible for [generating dynamic routes](/en/guides/routing/#dynamic-routes).

Use this option when your component needs a value for `Astro.params` in order to generate a single route dynamically.

```astro
---
const { locale, slug } = Astro.params;
---
<div></div>
```

```js
import LocaleSlug from "../src/components/[locale]/[slug].astro";


const result = await container.renderToString(LocaleSlug, {
  params: {
    locale: "en",
    slug: "getting-started"
  }
});
```

### `locals` options

[Section titled “locals options”](#locals-options)

**Type**: `App.Locals`

An option to pass information from [`Astro.locals`](/en/reference/api-reference/#locals) for rendering your component.

Use this option to when your component needs information stored during the lifecycle of a request in order to render, such as logged in status.

```astro
---
const { checkAuth } = Astro.locals;
const isAuthenticated = checkAuth();
---
{isAuthenticated ? <span>You're in</span> : <span>You're out</span> }
```

```js
import Card from "../src/components/Card.astro";


test("User is in", async () => {
  const result = await container.renderToString(Card, {
    locals: {
      checkAuth() { return true; }
    }
  });


  // assert result contains "You're in"
});




test("User is out", async () => {
  const result = await container.renderToString(Card, {
    locals: {
      checkAuth() { return false; }
    }
  });


  // assert result contains "You're out"
});
```

### `routeType` option

[Section titled “routeType option”](#routetype-option)

**Type**: `RouteType`

An option available when using `renderToResponse()` to specify that you are rendering an [endpoint](/en/guides/endpoints/):

```js
container.renderToString(Endpoint, { routeType: "endpoint" });
```

```js
import * as Endpoint from "../src/pages/api/endpoint.js";


const response = await container.renderToResponse(Endpoint, {
  routeType: "endpoint"
});
const json = await response.json();
```

To test your endpoint on methods such as `POST`, `PATCH`, etc., use the `request` option to call the correct function:

```js
export function GET() {}


// need to test this
export function POST() {}
```

```diff
import * as Endpoint from "../src/pages/api/endpoint.js";


const response = await container.renderToResponse(Endpoint, {
    routeType: "endpoint",
    request: new Request("https://example.com", {
      method: "POST" // Specify POST method for testing
    })
});
const json = await response.json();
```

### `partial` option

[Section titled “partial option”](#partial-option)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@4.16.6`

Whether or not the Container API renders components as if they were [page partials](/en/basics/astro-pages/#page-partials). The default `true` setting renders the component in isolation without a full page shell.

To render a component as a full Astro page, including `<!DOCTYPE html>`, you can opt-out of this behavior by setting `partial` to `false`:

```diff
import Blog from "../src/pages/Blog.astro";


const result = await container.renderToString(Card, {
    partial: false
});
console.log(result) // includes `<!DOCTYPE html>` at the beginning of the HTML
```

# Astro Content Loader API

Astro’s Content Loader API allows you to load your data from any source, local or remote, and interact with Astro’s content layer to manage your [content collections](/en/guides/content-collections/).

This API includes two ready-to-use loaders for content stored locally. It also provides tools for building your own custom objects that can load data from any source into content collections.

Learn more about [querying data loaded from build-time loaders](/en/guides/content-collections/#querying-build-time-collections) or [accessing live data from live loaders](/en/guides/content-collections/#accessing-live-data) with guided explanations and example usage in the content collections guide.

## Build-time loaders

[Section titled “Build-time loaders”](#build-time-loaders)

Build-time loaders are objects with a [`load()` method](#loaderload) that is called at build time to fetch data and update the data store. This object can also define a schema for the entries, which can be used to validate the data and generate static types.

Astro’s [`glob()`](#glob-loader) and [`file()`](#file-loader) loaders are examples of object loaders that are provided out-of-the-box for use with local content. For remote content, no prebuilt loaders are provided. You will have to build an object loader or use a [community-published loader](https://astro.build/integrations/?search=\&categories%5B%5D=loaders) to retrieve remote content and interact with the data store.

For simple data fetching, you can also [define a loader as an async function](#defining-a-loader-as-a-function) that returns an array or object containing entries.

### `glob()` loader

[Section titled “glob() loader”](#glob-loader)

**Type:** `(options: GlobOptions) => Loader`

**Added in:** `astro@5.0.0`

The `glob()` loader creates entries from directories of files from anywhere on the filesystem. The supported file types are Markdown, MDX, Markdoc, JSON, YAML, and TOML files.

This loader accepts an object with the following properties: `pattern`, `base` (optional), `generateId` (optional), and `retainBody` (optional).

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';


const pages = defineCollection({
  /* Retrieve all Markdown files in your pages directory. */
  loader: glob({ pattern: "**/*.md", base: "./src/data/pages" }),
});
const blog = defineCollection({
  /* Retrieve all Markdown and MDX files in your blog directory. */
  loader: glob({ pattern: "**/*.(md|mdx)", base: "./src/data/blog" }),
});
const notes = defineCollection({
  /* Retrieve all Markdown files in your notes directory and prevent
   * the raw body of content files from being stored in the data store. */
  loader: glob({
    pattern: '**/*.md',
    base: './src/data/notes',
    retainBody: false
  }),
});
const authors = defineCollection({
  /* Retrieve all JSON files in your authors directory while retaining
   * uppercase letters in the ID. */
  loader: glob({
    pattern: '**/*.json',
    base: "./src/data/authors",
    generateId: ({ entry }) => entry.replace(/\.json$/, ''),
  }),
});


export const collections = { pages, blog, authors };
```

#### `pattern`

[Section titled “pattern”](#pattern)

**Type:** `string | string[]`

The `pattern` property accepts a string or an array of strings using glob matching (e.g. wildcards, globstars). The patterns must be relative to the base directory of entry files to match.

You can learn more about the syntax to use in the [micromatch documentation](https://github.com/micromatch/micromatch#matching-features). You can also verify the validity of your pattern using an online tool like the [DigitalOcean Glob Tool](https://www.digitalocean.com/community/tools/glob).

#### `base`

[Section titled “base”](#base)

**Type:** `string | URL`\
**Default:** `"."`

A relative path or [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) to the directory from which to resolve the `pattern`.

#### `generateId()`

[Section titled “generateId()”](#generateid)

**Type:** `(options: GenerateIdOptions) => string`

A callback function that returns a unique string per entry in a collection. It accepts an object as parameter with the following properties:

* `entry` - the path to the entry file, relative to the base directory
* `base` - the base directory [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)
* `data` - the parsed, unvalidated data of the entry

By default it uses [`github-slugger`](https://github.com/Flet/github-slugger) to generate a slug with [kebab-cased](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case) words.

#### `retainBody`

[Section titled “retainBody”](#retainbody)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@5.17.0`

Whether or not to store the raw body of content files in the data store.

When `retainBody` is `false`, [`entry.body`](/en/reference/modules/astro-content/#collectionentrybody) will be `undefined` instead of containing the raw file contents.

Setting this property to `false` significantly reduces the deployed size of the data store and helps avoid hitting size limits for sites with very large collections.

For Markdown files, the rendered body will still be available in the [`entry.rendered.html` property](#dataentryrenderedhtml), and the [`entry.filePath` property](#dataentryfilepath) will still point to the original file.

For MDX collections, this will dramatically reduce the size of the collection, as there will no longer be any body retained in the store.

### `file()` loader

[Section titled “file() loader”](#file-loader)

**Type:** `(fileName: string, options?: FileOptions) => Loader`

**Added in:** `astro@5.0.0`

The `file()` loader creates entries from a single file that contains an array of objects with a unique `id` field, or an object with IDs as keys and entries as values.

It supports JSON, YAML, or TOML files and you can provide a custom `parser` for data files it cannot parse by default, or to parse data asynchronously.

This loader accepts a `fileName` property and an optional options object as second argument:

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { file } from 'astro/loaders';


const authors = defineCollection({
  /* Retrieve all entries from a JSON file. */
  loader: file("src/data/authors.json"),
});
const products = defineCollection({
  /* Retrieve all entries from a CSV file using a custom parser. */
  loader: file("src/data/products.csv", {
    parser: (fileContent) => { /* your parser logic */ },
  }),
});


export const collections = { authors, products };
```

#### `fileName`

[Section titled “fileName”](#filename)

**Type:** `string`

Sets the path to the file to load, relative to the root directory.

#### Options

[Section titled “Options”](#options)

**Type:** `FileOptions`

An optional object with the following properties:

##### `parser()`

[Section titled “parser()”](#parser)

**Type:** `(text: string) => Record<string, Record<string, unknown>> | Array<Record<string, unknown>> | Promise<Record<string, Record<string, unknown>> | Array<Record<string, unknown>>>`

A callback function to create a collection from a file’s contents. Use it when you need to process files other than JSON, YAML, or TOML that not supported by default (e.g. `.csv`) or when using [nested `.json` documents](/en/guides/content-collections/#nested-json-documents).

### Building a loader

[Section titled “Building a loader”](#building-a-loader)

The Content Loader API is flexible and full-featured, allowing for a variety of data fetching options. It is possible to build both simple and complex loaders. Your custom loader will depend on both the source and the shape of your data, as well as how you choose to manage the persistent data storage layer.

Most loaders will export a function that accepts configuration options and returns a [loader object](#the-loader-object) including a `name` for your loader, a `load()` method, and a `schema` defining your entries.

#### Loading collections into the data store

[Section titled “Loading collections into the data store”](#loading-collections-into-the-data-store)

The [`load()`](#loaderload) function returned in the loader object defines how your content is fetched, parsed, validated and updated. It accepts a `context` object that allows you to customize your data handling in a variety of ways and interact with the data store. A typical `load()` function will:

* Fetch your data from a source.
* Clear the existing data store.
* Parse and validate your data entries according to a provided schema.
* Update the data store with new entries.

The `load()` method also provides helpers to log messages to the console, render content to HTML, watch for changes in dev mode and reload data, provide access to metadata and even the full Astro config, and more.

See the full [`LoaderContext`](#loadercontext) list of properties for all options available to the `load()` function.

#### Providing a schema

[Section titled “Providing a schema”](#providing-a-schema)

Providing a Zod [`schema`](#loaderschema) in your loader allows you to validate your fetched content entries with [`parseData()`](#loadercontextparsedata) before adding them to the data [store](#loadercontextstore). This schema will also be used as the collection’s default schema when one does not exist in `src/content.config.ts` to provide type safety and editor tooling. You do not also need a schema defined in the content collection if the loader provides this property.

However, if the content collection also [defines a schema](/en/guides/content-collections/#defining-the-collection-schema), that schema will be used instead of your loader’s schema. This is to allow users of your loader to extend its schema, or transform data for use in their project. If you are [publishing and distributing a loader](#distributing-your-loader) for others to use, you may wish to document this behavior and encourage users not to define a collection schema themselves, or how to do so safely if they need data returned in a different format.

If you need to dynamically generate the schema based on the configuration options or by introspecting an API, you can use [`createSchema()`](#loadercreateschema) instead.

#### Loader example

[Section titled “Loader example”](#loader-example)

The following example shows a loader that fetches data from a provided feed URL (using a custom `loadFeedData` utility) and updates the data store with new entries each time the site is built:

src/feed-loader.ts

```ts
// 1. Import the `Loader` type and any other dependencies needed
import type { Loader } from 'astro/loaders';
import { z } from 'astro/zod';
import { loadFeedData } from "./feed.js";


// 2. Define any options that your loader needs
export function feedLoader(options: { url: string, apiKey: string }) {
  const feedUrl = new URL(options.url);
  // 3. Return a loader object
  return {
    name: "feed-loader",
    load: async ({ store, parseData }) => {
      const feed = await loadFeedData(feedUrl, options.apiKey);


      store.clear();


      for (const item of feed.items) {
        const id = item.guid;
        const data = await parseData({
          id,
          data: item,
        });
        store.set({
          id,
          data,
        });
      }
    },
    // 4. Define the schema of an entry.
    schema: z.object({
      // ...
    })
  } satisfies Loader;
}
```

#### Defining your collection with your loader

[Section titled “Defining your collection with your loader”](#defining-your-collection-with-your-loader)

Use your custom loader as the value of the `loader` property when you define your collection in `src/content.config.ts`. Configuration options can be passed to your loader as arguments:

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { feedLoader } from './feed-loader.ts';


const blog = defineCollection({
  loader: feedLoader({
    url: "https://api.example.com/posts",
    apiKey: "my-secret",
  }),
});


export const collections = { blog };
```

### Defining a loader as a function

[Section titled “Defining a loader as a function”](#defining-a-loader-as-a-function)

For simple data fetches that do not need custom data store handling, validation, logging, or any other helpers provided by the [build-time loader object](#the-loader-object), you can define your loader as a function.

The function can be async and must return either an array of entries that each contain a unique `id` field, or an object where each key is a unique ID and each value is the entry.

This pattern provides a convenient shorthand to accomplish the basic tasks normally performed by the `load()` function to [load collections into the data store](#loading-collections-into-the-data-store). At build-time, the loader will automatically clear the data store and reload all the entries. No further customization options or helpers for data handling are provided.

These loaders are often simple enough that you may choose to define them inline in the `src/content.config.ts` file:

src/content.config.ts

```ts
import { defineCollection } from "astro:content";


const countries = defineCollection({
  loader: async () => {
    const response = await fetch("https://restcountries.com/v3.1/all");
    const data = await response.json();
    // Must return an array of entries with an id property
    // or an object with IDs as keys and entries as values
    return data.map((country) => ({
      id: country.cca3,
      ...country,
    }));
  },
});


export const collections = { countries };
```

## Live Loaders

[Section titled “Live Loaders”](#live-loaders)

The Live Loader API is built to handle querying any data in real time. Live loaders can filter incoming data and verify content with type safety. Since live loaders fetch data fresh upon every request, there is no data store to update. These loaders are designed to return either data or an `Error` object to allow you to handle errors gracefully.

### Building a live loader

[Section titled “Building a live loader”](#building-a-live-loader)

Most live loaders will export a function that accepts configuration options and returns a [live loader object](#the-liveloader-object) including a `name` for your loader and two methods to define how to load your collection of entries and how to load a single entry: `loadCollection()` and `loadEntry()`.

#### Loading live data

[Section titled “Loading live data”](#loading-live-data)

To return data about your collection, you must provide a [`loadCollection()`](#liveloaderloadcollection) function that fetches data, and returns an array of content [`entries`](#livedatacollectionentries) or an error.

To return a single live collection entry, you must provide a [`loadEntry()`](#liveloaderloadentry) function that fetches data filtered for a given `id`, and returns a single [`entry`](#livedataentry), `undefined`, or an error.

The data fetching for both of these functions is typically done using a [`try...catch` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) to [handle errors when accessing live data](#error-handling-in-live-loaders).

See the full [Live Loader API](#live-loader-api) for more about the functions and types available for building your live loader.

#### Providing a schema for live loaders

[Section titled “Providing a schema for live loaders”](#providing-a-schema-for-live-loaders)

Live loaders do not include a schema property. Instead, you can provide type safety by [defining a Zod schema for your collection](/en/guides/content-collections/#using-zod-schemas-with-live-collections) in `src/live.config.ts`, or by passing generic types to the `LiveLoader` interface for the data they return.

#### Example live loader

[Section titled “Example live loader”](#example-live-loader)

The following example shows a live loader that defines data fetching from a CMS (using a custom `fetchFromCMS` utility) for both a collection of entries and a single entry, including type safety and error handling:

src/article-loader.ts

```ts
import type { LiveLoader } from 'astro/loaders';
import { fetchFromCMS } from './cms-client.js';


interface Article {
  id: string;
  title: string;
  htmlContent: string;
  author: string;
}


interface EntryFilter {
  id: string;
}


interface CollectionFilter {
  author?: string;
}


export function articleLoader(config: { apiKey: string }): LiveLoader<Article, EntryFilter, CollectionFilter> {
  return {
    name: 'article-loader',
    loadCollection: async ({ filter }) => {
      try {
        const articles = await fetchFromCMS({
          apiKey: config.apiKey,
          type: 'article',
          filter,
        });


        return {
          entries: articles.map((article) => ({
            id: article.id,
            data: article,
          })),
        };
      } catch (error) {
        return {
          error: new Error('Failed to load articles', { cause: error }),
        };
      }
    },
    loadEntry: async ({ filter }) => {
      try {
        // filter will be { id: "some-id" } when called with a string
        const article = await fetchFromCMS({
          apiKey: config.apiKey,
          type: 'article',
          id: filter.id,
        });


        if (!article) {
          return {
            error: new Error('Article not found'),
          };
        }


        return {
          id: article.id,
          data: article,
          rendered: {
            html: article.htmlContent,
          },
        };
      } catch (error) {
        return {
          error: new Error('Failed to load article', { cause: error }),
        };
      }
    },
  };
}
```

#### Defining your live collection with your loader

[Section titled “Defining your live collection with your loader”](#defining-your-live-collection-with-your-loader)

Use your custom live loader as the value of the `loader` property when you define your collection in `src/live.config.ts`. Configuration options can be passed to your loader as arguments:

src/live.config.ts

```ts
import { defineLiveCollection } from 'astro:content';
import { articleLoader } from './article-loader.ts';


const blog = defineLiveCollection({
  loader: articleLoader({
    apiKey: "my-secret",
  }),
});


export const collections = { blog };
```

#### Error handling in live loaders

[Section titled “Error handling in live loaders”](#error-handling-in-live-loaders)

Live loaders return an [Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) subclass for errors. You can create [custom error types](#creating-live-loader-error-types) and use them for more specific error handling if needed. If an error is thrown in the live loader, it will be caught and returned, wrapped in a `LiveCollectionError`.

Astro will generate some errors itself, depending on the response from the live loader:

* If `loadEntry` returns `undefined`, Astro will return a `LiveEntryNotFoundError` to the user.
* If a schema is defined for the collection and the data does not match the schema, Astro will return a `LiveCollectionValidationError`.
* If the loader returns an invalid cache hint, Astro will return a `LiveCollectionCacheHintError`. The `cacheHint` field is optional, so if you do not have valid data to return, you can simply omit it.

my-loader.ts

```ts
import type { LiveLoader } from 'astro/loaders';
import type { MyData } from "./types";
import { MyLoaderError } from './errors';


export function myLoader(config): LiveLoader<MyData, never, never, MyLoaderError> {
  return {
    name: 'my-loader',
    loadCollection: async () => {
      // Return your custom error type
      return {
        error: new MyLoaderError('Failed to load', 'LOAD_ERROR'),
      };
    },
    // ...
  };
}
```

##### Creating live loader error types

[Section titled “Creating live loader error types”](#creating-live-loader-error-types)

You can create custom error types for [errors returned by your loader](#error-handling-in-live-loaders) and pass them as a generic to get proper typing:

my-loader.ts

```ts
import type { LiveLoader } from "astro/loaders";
import type { MyData } from "./types"


export class MyLoaderError extends Error {
  constructor(message: string, public code?: string) {
    super(message);
    this.name = 'MyLoaderError';
  }
}


export function myLoader(config): LiveLoader<MyData, never, never, MyLoaderError> {
  return {
    name: 'my-loader',
    loadCollection: async () => {
      // Return your custom error type
      return {
        error: new MyLoaderError('Failed to load', 'LOAD_ERROR'),
      };
    },
    // ...
  };
}
```

When you use `getLiveCollection()` or `getLiveEntry()`, TypeScript will infer the custom error type, allowing you to handle it appropriately:

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getLiveEntry } from 'astro:content';
import { MyLoaderError } from "../my-loader";


const { entry, error } = await getLiveEntry('products', '123');


if (error) {
  if (error instanceof MyLoaderError) {
    console.error(`Loader error: ${error.message} (code: ${error.code})`);
  } else {
    console.error(`Unexpected error: ${error.message}`);
  }
  return Astro.rewrite('/500');
}
---
```

#### Defining custom filter types

[Section titled “Defining custom filter types”](#defining-custom-filter-types)

Live loaders can define custom filter types for both `getLiveCollection()` and `getLiveEntry()`. This enables type-safe querying that matches your API’s capabilities, making it easier for users to discover available filters and ensure they are used correctly. If you include JSDoc comments in your filter types, the user will see these in their IDE as hints when using the loader.

store-loader.ts

```ts
import type { LiveLoader } from 'astro/loaders';
import { fetchProduct, fetchCategory, type Product } from './store-client';


interface CollectionFilter {
  category?: string;
  /** Minimum price to filter products */
  minPrice?: number;
  /** Maximum price to filter products */
  maxPrice?: number;
}


interface EntryFilter {
  /** Alias for `sku` */
  id?: string;
  slug?: string;
  sku?: string;
}


export function productLoader(config: {
  apiKey: string;
  endpoint: string;
}): LiveLoader<Product, EntryFilter, CollectionFilter> {
  return {
    name: 'product-loader',
    loadCollection: async ({ filter }) => {
      // filter is typed as CollectionFilter
      const data = await fetchCategory({
        apiKey: config.apiKey,
        category: filter?.category ?? 'all',
        minPrice: filter?.minPrice,
        maxPrice: filter?.maxPrice,
      });


      return {
        entries: data.products.map((product) => ({
          id: product.sku,
          data: product,
        })),
      };
    },
    loadEntry: async ({ filter }) => {
      // filter is typed as EntryFilter | { id: string }
      const product = await fetchProduct({
        apiKey: config.apiKey,
        slug: filter.slug,
        sku: filter.sku || filter.id,
      });
      if (!product) {
        return {
          error: new Error('Product not found'),
        };
      }
      return {
        id: product.sku,
        data: product,
      };
    },
  };
}
```

#### Cache hints

[Section titled “Cache hints”](#cache-hints)

Live loaders can provide cache hints to help with response caching. You can use this data to send HTTP cache headers or otherwise inform your caching strategy.

my-loader.ts

```ts
import type { LiveLoader } from "astro/loaders";
import { loadStoreProduct, loadStoreProducts, getLastModifiedDate } from "./store";
import type { Product, ProductEntryFilter, ProductCollectionFilter } from "./types";


export function myLoader(config): LiveLoader<Product, ProductEntryFilter, ProductCollectionFilter> {
  return {
    name: 'cached-loader',
    loadCollection: async ({ filter }) => {
      const products = await loadStoreProducts(filter);
      return {
        entries: products.map((item) => ({
          id: item.id,
          data: item,
          // You can optionally provide cache hints for each entry
          cacheHint: {
            tags: [`product-${item.id}`, `category-${item.category}`],
          },
        })),
        cacheHint: {
          // All fields are optional, and are combined with each entry's cache hints
          // tags are merged from all entries
          // lastModified is the most recent lastModified of all entries and the collection
          lastModified: getLastModifiedDate(products),
          tags: ['products'],
        },
      };
    },
    loadEntry: async ({ filter }) => {
      const item = await loadStoreProduct(filter);
      return {
        id: item.id,
        data: item,
        cacheHint: {
          lastModified: new Date(item.lastModified),
          tags: [`product-${item.id}`, `category-${item.category}`],
        },
      };
    },
  };
}
```

You can then use these hints in your pages. If you have [experimental route caching](/en/reference/experimental-flags/route-caching/) enabled, pass cache hints directly to `Astro.cache.set()`:

src/pages/store/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getLiveEntry } from 'astro:content';


const { entry, error, cacheHint } = await getLiveEntry('products', Astro.params.id);


if (error) {
  return Astro.redirect('/404');
}


// Pass cache hints to route caching
if (cacheHint) {
  Astro.cache.set(cacheHint);
}
Astro.cache.set({ maxAge: 300 });
---


<h1>{entry.data.name}</h1>
<p>{entry.data.description}</p>
```

Without route caching enabled, you can use cache hints to set response headers manually for your own caching strategy:

src/pages/store/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getLiveEntry } from 'astro:content';


const { entry, error, cacheHint } = await getLiveEntry('products', Astro.params.id);


if (error) {
  return Astro.redirect('/404');
}


if (cacheHint?.tags) {
  Astro.response.headers.set('Cache-Tag', cacheHint.tags.join(','));
}


if (cacheHint?.lastModified) {
  Astro.response.headers.set('Last-Modified', cacheHint.lastModified.toUTCString());
}
---


<h1>{entry.data.name}</h1>
<p>{entry.data.description}</p>
```

Note

Cache hints do not automatically cause the response to be cached by Astro. They provide values you can pass to [route caching](/en/reference/experimental-flags/route-caching/) or use to implement your own caching strategy.

## Distributing your loader

[Section titled “Distributing your loader”](#distributing-your-loader)

Loaders can be defined in your site or as a separate npm package. If you want to share your loader with the community, you can [publish it to npm with the `withastro` and `astro-loader` keywords](/en/guides/integrations/#packagejson-data).

A loader should export a function that returns a `LiveLoader` object for live loaders or a `Loader` object for build-time loaders, allowing users to configure it with their own settings.

## Object loader API

[Section titled “Object loader API”](#object-loader-api)

**Added in:** `astro@5.0.0`

This section shows the API for defining a [build-time object loader](#building-a-loader).

### The `Loader` object

[Section titled “The Loader object”](#the-loader-object)

**Type:** `Loader`

A loader function returns an object with two required properties. In addition to providing a name for the loader, this object describes how to fetch the collection data.

Optionally, you can return a third property defining a schema to validate your collection entries. Use the [Typescript `satisfies` operator](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator) instead of a return type annotation to provide type safety inside your loader object and to preserve type inference when your loader is used in a collection.

#### `Loader.name`

[Section titled “Loader.name”](#loadername)

**Type:** `string`

**Added in:** `astro@5.0.0`

A unique name for the loader, used in logs and [for conditional loading](/en/reference/integrations-reference/#refreshcontent-option).

#### `Loader.load()`

[Section titled “Loader.load()”](#loaderload)

**Type:** `(context: LoaderContext) => Promise<void>`

**Added in:** `astro@5.0.0`

An async function that is called at build time to load data and update the store. It is passed a [`LoaderContext`](#loadercontext) object that contains helper functions and properties for writing your loader’s implementation logic, as well as the `store` database and methods for interacting with it.

#### `Loader.schema`

[Section titled “Loader.schema”](#loaderschema)

**Type:** `ZodSchema`

**Added in:** `astro@5.0.0`

An optional [Zod schema](/en/guides/content-collections/#defining-datatypes-with-zod) that defines the shape of the entries. It is used to both validate the data and also to generate TypeScript types for the collection.

When you need to dynamically generate the schema at build time based on configuration options or by introspecting an API, use [`createSchema()`](#loadercreateschema) instead.

If present, it will be overridden by any Zod `schema` defined for the collection in the `src/content.config.ts` file.

#### `Loader.createSchema()`

[Section titled “Loader.createSchema()”](#loadercreateschema)

**Type:** `() => Promise<{ schema: ZodSchema; types: string }>`

**Added in:** `astro@6.0.0`

An optional async function that returns an object containing a [Zod schema](/en/guides/content-collections/#defining-datatypes-with-zod) and types. It is used to dynamically generate the schema at build time based on the configuration options or by introspecting an API.

When you only need to provide a static schema, provide a Zod validation object using [`schema`](#loaderschema) instead.

If present, it will be overridden by any Zod `schema` defined for the collection in the `src/content.config.ts` file.

The returned `types` contents will be written to a TypeScript file, and must export an `Entry` type or interface:

src/feed-loader.ts

```diff
import type { Loader } from 'astro/loaders';
import { z } from 'astro/zod';
import { loadFeedData, getSchema, getTypes } from "./feed.js";


export function myLoader(options: { url: string, apiKey: string }) {
  const feedUrl = new URL(options.url);


  return {
    name: "feed-loader",
    load: async ({ store, parseData }) => {
      const feed = await loadFeedData(feedUrl, options.apiKey);


      store.clear();


      for (const item of feed.items) {
        const id = item.guid;
        const data = await parseData({
          id,
          data: item,
        });
        store.set({
          id,
          data,
        });
      }
    },
    +createSchema: async () => {
      +const schema = await getSchema();
      +const types = await getTypes();


      +return {
+        schema,
+        types: `export type Entry = ${types}`,
+      };
+    },
  } satisfies Loader;
}
```

### `LoaderContext`

[Section titled “LoaderContext”](#loadercontext)

This object is passed to the [`load()`](#loaderload) method of the loader, and contains the following properties:

#### `LoaderContext.collection`

[Section titled “LoaderContext.collection”](#loadercontextcollection)

**Type:** `string`

**Added in:** `astro@5.0.0`

The unique name of the collection. This is the key in the `collections` object in the `src/content.config.ts` file.

#### `LoaderContext.store`

[Section titled “LoaderContext.store”](#loadercontextstore)

**Type:** [`DataStore`](#datastore)

**Added in:** `astro@5.0.0`

A database to store the actual data. Use this to update the store with new entries. See [`DataStore`](#datastore) for more information.

#### `LoaderContext.meta`

[Section titled “LoaderContext.meta”](#loadercontextmeta)

**Type:** `MetaStore`

**Added in:** `astro@5.0.0`

A key-value store scoped to the collection, designed for things like sync tokens and last-modified times. This metadata is persisted between builds alongside the collection data but is only available inside the loader.

```ts
const lastModified = meta.get("lastModified");
// ...
meta.set("lastModified", new Date().toISOString());
```

#### `LoaderContext.logger`

[Section titled “LoaderContext.logger”](#loadercontextlogger)

**Type:** [`AstroIntegrationLogger`](/en/reference/integrations-reference/#astrointegrationlogger)

**Added in:** `astro@5.0.0`

A logger that can be used to log messages to the console. Use this instead of `console.log` for more helpful logs that include loader-specific content such as the loader name or information about the loading process in the log message. See [`AstroIntegrationLogger`](/en/reference/integrations-reference/#astrointegrationlogger) for more information.

Extract from the file() loader

```ts
return {
  name: 'file-loader',
  load: async ({ config, store, logger, watcher }) => {
    const url = new URL(fileName, config.root);
    const filePath = fileURLToPath(url);
    await syncData(filePath, store);


    watcher?.on('change', async (changedPath) => {
      if (changedPath === filePath) {
        logger.info(`Reloading data from ${fileName}`);
        await syncData(filePath, store);
      }
    });
  },
};
```

#### `LoaderContext.config`

[Section titled “LoaderContext.config”](#loadercontextconfig)

**Type:** `AstroConfig`

**Added in:** `astro@5.0.0`

The full, resolved Astro configuration object with all defaults applied. See [the configuration reference](/en/reference/configuration-reference/) for more information.

Extract from the file() loader

```ts
return {
  name: 'file-loader',
  load: async ({ config, store, logger, watcher }) => {
    const url = new URL(fileName, config.root);
    const filePath = fileURLToPath(url);
    await syncData(filePath, store);


    watcher?.on('change', async (changedPath) => {
      if (changedPath === filePath) {
        logger.info(`Reloading data from ${fileName}`);
        await syncData(filePath, store);
      }
    });
  },
};
```

#### `LoaderContext.parseData()`

[Section titled “LoaderContext.parseData()”](#loadercontextparsedata)

**Type:** `(props: ParseDataOptions<TData>) => Promise<TData>`

**Added in:** `astro@5.0.0`

Validates and parses the data according to the collection schema. Pass data to this function to validate and parse it before storing it in the data store.

loader.ts

```ts
import type { Loader } from "astro/loaders";
import { loadFeed } from "./feed.js";


export function feedLoader({ url }) {
  const feedUrl = new URL(url);
  return {
    name: "feed-loader",
    load: async ({ store, logger, parseData, meta, generateDigest }) => {
      logger.info("Loading posts");
      const feed = loadFeed(feedUrl);
      store.clear();


      for (const item of feed.items) {
        const id = item.guid;
        const data = await parseData({
          id,
          data: item,
        });
        store.set({
          id,
          data,
        });
      }
    },
  } satisfies Loader;
}
```

#### `LoaderContext.renderMarkdown()`

[Section titled “LoaderContext.renderMarkdown()”](#loadercontextrendermarkdown)

**Type:** `(content: string, options?: { fileURL?: URL }) => Promise<RenderedContent>`

**Added in:** `astro@5.9.0`

Renders a Markdown string to HTML, returning a `RenderedContent` object.

This allows you to render Markdown content directly within your loaders using the same Markdown processing as Astro’s built-in `glob()` loader and provides access to the `render()` function and `<Content />` component for [rendering body content](/en/guides/content-collections/#rendering-body-content).

Assign this object to the [rendered](#dataentryrendered) field of the [DataEntry](#dataentry) object to allow users to [render the content in a page](/en/guides/content-collections/#rendering-body-content). If the Markdown content includes frontmatter, it will be parsed and available in `metadata.frontmatter`. The frontmatter will be excluded from the HTML output.

loader.ts

```ts
import type { Loader } from 'astro/loaders';
import { loadFromCMS } from './cms.js';


export function myLoader(settings) {
  return {
    name: 'cms-loader',
    async load({ renderMarkdown, store }) {
      const entries = await loadFromCMS();


      store.clear();


      for (const entry of entries) {
        store.set({
          id: entry.id,
          data: entry,
          // Assume each entry has a 'content' field with markdown content
          rendered: await renderMarkdown(entry.content),
        });
      }
    },
  } satisfies Loader;
}
```

##### `fileURL`

[Section titled “fileURL”](#fileurl)

**Type:** `URL`

**Added in:** `astro@6.0.0`

Specifies the file path to use for resolving relative image paths in Markdown content.

The following example uses the [configured root directory](/en/reference/configuration-reference/#root) to resolve image paths:

loader.ts

```ts
for (const file of files) {
  const content = await readFile(file.path, 'utf8');
  store.set({
    id: file.id,
    data: file.data,
    rendered: await renderMarkdown(content, {
      fileURL: new URL(file.path, config.root),
    }),
  });
}
```

#### `LoaderContext.generateDigest()`

[Section titled “LoaderContext.generateDigest()”](#loadercontextgeneratedigest)

**Type:** `(data: Record<string, unknown> | string) => string`

**Added in:** `astro@5.0.0`

Generates a non-cryptographic content digest of an object or string. This can be used to track if the data has changed by setting [the `digest` field](#dataentrydigest) of an entry.

loader.ts

```ts
import type { Loader } from "astro/loaders";
import { loadFeed } from "./feed.js";


export function feedLoader({ url }) {
  const feedUrl = new URL(url);
  return {
    name: "feed-loader",
    load: async ({ store, logger, parseData, meta, generateDigest }) => {
      logger.info("Loading posts");
      const feed = loadFeed(feedUrl);
      store.clear();


      for (const item of feed.items) {
        const id = item.guid;
        const data = await parseData({
          id,
          data: item,
        });


        const digest = generateDigest(data);


        store.set({
          id,
          data,
          digest,
        });
      }
    },
  } satisfies Loader;
}
```

#### `LoaderContext.watcher`

[Section titled “LoaderContext.watcher”](#loadercontextwatcher)

**Type:** `FSWatcher`

**Added in:** `astro@5.0.0`

When running in dev mode, this is a filesystem watcher that can be used to trigger updates. See [`ViteDevServer`](https://vite.dev/guide/api-javascript.html#vitedevserver) for more information.

Extract from the file() loader

```ts
return {
  name: 'file-loader',
  load: async ({ config, store, watcher }) => {
    const url = new URL(fileName, config.root);
    const filePath = fileURLToPath(url);
    await syncData(filePath, store);


    watcher?.on('change', async (changedPath) => {
      if (changedPath === filePath) {
        logger.info(`Reloading data from ${fileName}`);
        await syncData(filePath, store);
      }
    });
  },
};
```

#### `LoaderContext.refreshContextData`

[Section titled “LoaderContext.refreshContextData”](#loadercontextrefreshcontextdata)

**Type:** `Record<string, unknown>`

**Added in:** `astro@5.0.0`

If the loader has been triggered by an integration, this may optionally contain extra data set by that integration. It is only set when the loader is triggered by an integration. See the [`astro:server:setup`](/en/reference/integrations-reference/#refreshcontent-option) hook reference for more information.

loader.ts

```ts
import type { Loader } from "astro/loaders";
import { processWebhook } from "./lib/webhooks";


export function myLoader(options: { url: string }) {
  return {
    name: "my-loader",
    load: async ({ refreshContextData, store, logger }) => {
      if(refreshContextData?.webhookBody) {
        logger.info("Webhook triggered with body");
        processWebhook(store, refreshContextData.webhookBody);
      }
      // ...
    },
  } satisfies Loader;
}
```

### `DataStore`

[Section titled “DataStore”](#datastore)

The data store is a loader’s interface to the content collection data. It is a key-value (KV) store, scoped to the collection, and therefore a loader can only access the data for its own collection.

#### `DataStore.get()`

[Section titled “DataStore.get()”](#datastoreget)

**Type:** `(key: string) => DataEntry | undefined`

**Added in:** `astro@5.0.0`

Get an entry from the store by its ID. Returns `undefined` if the entry does not exist.

```ts
const existingEntry = store.get("my-entry");
```

The returned object is a [`DataEntry`](#dataentry) object.

#### `DataStore.set()`

[Section titled “DataStore.set()”](#datastoreset)

**Type:** `(entry: DataEntry) => boolean`

**Added in:** `astro@5.0.0`

Used after data has been [validated and parsed](#loadercontextparsedata) to add an entry to the store, returning `true` if the entry was set. This returns `false` when the [`digest`](#dataentrydigest) property determines that an entry has not changed and should not be updated.

loader.ts

```ts
    for (const item of feed.items) {
      const id = item.guid;
      const data = await parseData({
        id,
        data: item,
      });
      const digest = generateDigest(data);
      store.set({
        id,
        data,
        rendered: {
          html: data.description ?? "",
        },
        digest,
      });
    }
```

#### `DataStore.entries()`

[Section titled “DataStore.entries()”](#datastoreentries)

**Type:** `() => Array<[id: string, DataEntry]>`

**Added in:** `astro@5.0.0`

Get all entries in the collection as an array of key-value pairs.

#### `DataStore.keys()`

[Section titled “DataStore.keys()”](#datastorekeys)

**Type:** `() => Array<string>`

**Added in:** `astro@5.0.0`

Get all the keys of the entries in the collection.

#### `DataStore.values()`

[Section titled “DataStore.values()”](#datastorevalues)

**Type:** `() => Array<DataEntry>`

**Added in:** `astro@5.0.0`

Get all entries in the collection as an array.

#### `DataStore.delete()`

[Section titled “DataStore.delete()”](#datastoredelete)

**Type:** `(key: string) => void`

**Added in:** `astro@5.0.0`

Delete an entry from the store by its ID.

#### `DataStore.clear()`

[Section titled “DataStore.clear()”](#datastoreclear)

**Type:** `() => void`

**Added in:** `astro@5.0.0`

Clear all entries from the collection.

#### `DataStore.has()`

[Section titled “DataStore.has()”](#datastorehas)

**Type:** `(key: string) => boolean`

**Added in:** `astro@5.0.0`

Check if an entry exists in the store by its ID.

### `DataEntry`

[Section titled “DataEntry”](#dataentry)

This is the type of the object that is stored in the data store. It has the following properties:

#### `DataEntry.id`

[Section titled “DataEntry.id”](#dataentryid)

**Type:** `string`

**Added in:** `astro@5.0.0`

An identifier for the entry, which must be unique within the collection. This is used to look up the entry in the store and is the key used with [`getEntry()`](/en/reference/modules/astro-content/#getentry) for that collection.

#### `DataEntry.data`

[Section titled “DataEntry.data”](#dataentrydata)

**Type:** `Record<string, unknown>`

**Added in:** `astro@5.0.0`

The actual data for the entry. When a user accesses the collection, this will have TypeScript types generated according to the collection schema.

It is the loader’s responsibility to use [`parseData()`](#loadercontextparsedata) to validate and parse the data before storing it in the data store: no validation is done when getting or setting the data.

#### `DataEntry.filePath`

[Section titled “DataEntry.filePath”](#dataentryfilepath)

**Type:** `string | undefined`

**Added in:** `astro@5.0.0`

A path to the file that is the source of this entry, relative to the root of the site. This only applies to file-based loaders and is used to resolve paths such as images or other assets.

If not set, then any fields in the schema that use [the `image()` helper](/en/guides/images/#images-in-content-collections) will be treated as [public paths](/en/guides/images/#where-to-store-images) and not transformed.

#### `DataEntry.body`

[Section titled “DataEntry.body”](#dataentrybody)

**Type:** `string | undefined`

**Added in:** `astro@5.0.0`

The raw body of the entry, if applicable. If the entry includes [rendered content](#dataentryrendered), then this field can be used to store the raw source. This is optional and is not used internally.

#### `DataEntry.digest`

[Section titled “DataEntry.digest”](#dataentrydigest)

**Type:** `string | undefined`

**Added in:** `astro@5.0.0`

An optional content digest for the entry. This can be used to check if the data has changed.

When [setting an entry](#datastoreset), the entry will only update if the digest does not match an existing entry with the same ID.

The format of the digest is up to the loader, but it must be a string that changes when the data changes. This can be done with the [`generateDigest`](#loadercontextgeneratedigest) function.

#### `DataEntry.rendered`

[Section titled “DataEntry.rendered”](#dataentryrendered)

**Type:** `RenderedContent | undefined`

**Added in:** `astro@5.0.0`

Stores an object with an entry’s rendered content and metadata if it has been rendered to HTML. For example, this can be used to store the rendered content of a Markdown entry, or HTML from a CMS.

If this field is provided, then [the `render()` function and `<Content />` component](/en/guides/content-collections/#rendering-body-content) are available to render the entry in a page.

If the entry has Markdown content then you can use the [`renderMarkdown()`](#loadercontextrendermarkdown) function to generate this object from the Markdown string.

##### `DataEntry.rendered.html`

[Section titled “DataEntry.rendered.html”](#dataentryrenderedhtml)

**Type:** `string`

Contains the rendered HTML string. This is used by [`render()`](/en/reference/modules/astro-content/#render) to return a component that renders this HTML.

##### `DataEntry.rendered.metadata`

[Section titled “DataEntry.rendered.metadata”](#dataentryrenderedmetadata)

**Type:** `object | undefined`

Describes the metadata present in this file. This includes the `imagePaths`, the `headings`, the `frontmatter`, and any other metadata present in the file. When the file has not been rendered as HTML, this value will be `undefined`.

###### `DataEntry.rendered.metadata.imagePaths`

[Section titled “DataEntry.rendered.metadata.imagePaths”](#dataentryrenderedmetadataimagepaths)

**Type:** `string[]`

Specifies the list of images paths present in this entry. Each path is relative to the [entry `filePath`](#dataentryfilepath).

###### `DataEntry.rendered.metadata.headings`

[Section titled “DataEntry.rendered.metadata.headings”](#dataentryrenderedmetadataheadings)

**Type:** `MarkdownHeading[]`

Specifies the list of headings present in this file. Each heading is described by a `depth` determined by the heading level (`h1 -> h6`), a `slug` generated with [`github-slugger`](https://github.com/Flet/github-slugger), and its `text` content.

###### `DataEntry.rendered.metadata.frontmatter`

[Section titled “DataEntry.rendered.metadata.frontmatter”](#dataentryrenderedmetadatafrontmatter)

**Type:** `Record<string, any>`

Describes the raw frontmatter, parsed from the file. This may include [programmatically injected data from remark plugins](/en/guides/markdown-content/#modifying-frontmatter-programmatically).

## Live loader API

[Section titled “Live loader API”](#live-loader-api)

**Added in:** `astro@6.0.0`

This section shows the API for defining a [live loader](#live-loaders).

### The `LiveLoader` object

[Section titled “The LiveLoader object”](#the-liveloader-object)

**Type:** `LiveLoader<TData, TEntryFilter, TCollectionFilter, TError>`

**Added in:** `astro@6.0.0`

A live loader function returns an object with three required live loader properties. In addition to providing a name for the loader, this object describes how to fetch both single entries and an entire collection from your live data source.

Use the `LiveLoader` generic type to provide type safety in your loader. This type accepts the following type parameters, in this order:

* **`TData`** (defaults to `Record<string, unknown>`): The data structure of each entry returned by the loader.
* **`TEntryFilter`** (defaults to `never`): The filter object type accepted by [`getLiveEntry()`](/en/reference/modules/astro-content/#getliveentry) and accessible in [`loadEntry()`](#liveloaderloadentry). Use `never` when you don’t support filtering single entries.
* **`TCollectionFilter`** (defaults to `never`): The filter object type accepted by [`getLiveCollection()`](/en/reference/modules/astro-content/#getlivecollection) and accessible in [`loadCollection()`](#liveloaderloadcollection). Use `never` when you don’t support filtering collections.
* **`TError`** (defaults to `Error`): A [custom `Error` class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#custom_error_types) that can be returned by the loader for more granular error handling.

#### `LiveLoader.name`

[Section titled “LiveLoader.name”](#liveloadername)

**Type:** `string`

**Added in:** `astro@6.0.0`

A unique name for the loader, used in logs.

#### `LiveLoader.loadCollection()`

[Section titled “LiveLoader.loadCollection()”](#liveloaderloadcollection)

**Type:** `(context: LoadCollectionContext<TCollectionFilter>) => Promise<LiveDataCollection<TData> | { error: TError; }>`

**Added in:** `astro@6.0.0`

Defines a method to load a collection of entries. This function receives a [context object](#loadcollectioncontext) containing an optional `filter` property and must return the data associated with this collection or the errors.

#### `LiveLoader.loadEntry()`

[Section titled “LiveLoader.loadEntry()”](#liveloaderloadentry)

**Type:** `(context: LoadEntryContext<TEntryFilter>) => Promise<LiveDataEntry<TData> | undefined | { error: TError; }>`

**Added in:** `astro@6.0.0`

Defines a method to load a single entry. This function receives a [context object](#loadentrycontext) containing a `filter` property and returns either the data associated with the requested entry, `undefined` when the entry cannot be found, or the errors.

### `LoadCollectionContext`

[Section titled “LoadCollectionContext”](#loadcollectioncontext)

**Type:** `{ filter?: TCollectionFilter; }`

**Added in:** `astro@6.0.0`

This object is passed to the [`loadCollection()` method](#liveloaderloadcollection) of the loader and contains the following properties:

#### `LoadCollectionContext.filter`

[Section titled “LoadCollectionContext.filter”](#loadcollectioncontextfilter)

**Type:** `Record<string, any> | never`\
**Default:** `never`

**Added in:** `astro@6.0.0`

An object describing the [filters supported by your loader](#defining-custom-filter-types).

### `LoadEntryContext`

[Section titled “LoadEntryContext”](#loadentrycontext)

**Type:** `{ filter: TEntryFilter; }`

**Added in:** `astro@6.0.0`

This object is passed to the [`loadEntry()` method](#liveloaderloadentry) of the loader and contains the following properties:

#### `LoadEntryContext.filter`

[Section titled “LoadEntryContext.filter”](#loadentrycontextfilter)

**Type:** `Record<string, any> | never`\
**Default:** `never`

**Added in:** `astro@6.0.0`

An object describing the [filters supported by your loader](#defining-custom-filter-types).

### `LiveDataEntry`

[Section titled “LiveDataEntry”](#livedataentry)

**Type:** `{ id: string; data: TData; rendered?: { html: string }; cacheHint?: CacheHint; }`

**Added in:** `astro@6.0.0`

This is the type object that is returned by the [`loadEntry()`](#liveloaderloadentry) method. It contains the following properties:

#### `LiveDataEntry.id`

[Section titled “LiveDataEntry.id”](#livedataentryid)

**Type:** `string`

**Added in:** `astro@6.0.0`

An identifier for the entry, which must be unique within the collection. This is the key used with [`getLiveEntry()`](/en/reference/modules/astro-content/#getliveentry) for that collection.

#### `LiveDataEntry.data`

[Section titled “LiveDataEntry.data”](#livedataentrydata)

**Type:** `Record<string, unknown>`

**Added in:** `astro@6.0.0`

The actual data for the entry. When a user accesses the collection, this will have TypeScript types generated according to the collection schema.

It is the loader’s responsibility to validate and parse the data before returning it.

#### `LiveDataEntry.rendered`

[Section titled “LiveDataEntry.rendered”](#livedataentryrendered)

**Type:** `{ html: string }`

**Added in:** `astro@6.0.0`

An object with an entry’s rendered content if it has been rendered to HTML. For example, this can be the rendered content of a Markdown entry, or HTML from a CMS.

If this field is provided, then [the `render()` function and `<Content />` component](/en/guides/content-collections/#rendering-body-content) are available to render the entry in a page.

If the loader does not return a `rendered` property for an entry, the `<Content />` component will render nothing.

#### `LiveDataEntry.cacheHint`

[Section titled “LiveDataEntry.cacheHint”](#livedataentrycachehint)

**Type:** [`CacheHint`](#cachehint)

**Added in:** `astro@6.0.0`

An optional object to provide a hint for how to cache this specific entry.

### `LiveDataCollection`

[Section titled “LiveDataCollection”](#livedatacollection)

**Type:** `{ entries: Array<LiveDataEntry<TData>>; cacheHint?: CacheHint; }`

**Added in:** `astro@6.0.0`

This is the type object that is returned by the [`loadCollection()`](#liveloaderloadcollection) method. It contains the following properties:

#### `LiveDataCollection.entries`

[Section titled “LiveDataCollection.entries”](#livedatacollectionentries)

**Type:** `Array<LiveDataEntry<TData>>`

**Added in:** `astro@6.0.0`

An array of [`LiveDataEntry`](#livedataentry) objects.

#### `LiveDataCollection.cacheHint`

[Section titled “LiveDataCollection.cacheHint”](#livedatacollectioncachehint)

**Type:** [`CacheHint`](#cachehint)

**Added in:** `astro@6.0.0`

An optional object providing guidance on how to cache this collection. This object will be merged with the cache hints defined for each individual entry, if provided.

### `CacheHint`

[Section titled “CacheHint”](#cachehint)

An object that loaders can return through the `cacheHint` property in [`LiveDataCollection`](#livedatacollection) or [`LiveDataEntry`](#livedataentry) to provide hints to assist in caching the response. This contains the following properties:

#### `CacheHint.tags`

[Section titled “CacheHint.tags”](#cachehinttags)

**Type:** `Array<string>`

**Added in:** `astro@6.0.0`

An array of string identifiers allowing fine-grained cache control. This allows you to group related content and selectively invalidate cached responses when specific content changes.

The following example defines cache hint tags for a collection of posts filtered by author:

```ts
return {
  /* ... */
  cacheHint: {
    tags: ["posts", `posts-${filter.author}`],
  },
};
```

#### `CacheHint.lastModified`

[Section titled “CacheHint.lastModified”](#cachehintlastmodified)

**Type:** `Date`

**Added in:** `astro@6.0.0`

The date of the last modification of the content (e.g., the last update of an entry in a collection). This can be used to set HTTP cache headers like [`Last-Modified`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Last-Modified) and [`If-Modified-Since`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/If-Modified-Since).

The following example defines a cache hint for a single product using its last update date:

```ts
return {
  /* ... */
  cacheHint: {
    lastModified: new Date(product.updatedAt)
  },
};
```

# Dev Toolbar App API

The Astro Dev Toolbar App API allows you to create [Astro Integrations](/en/reference/integrations-reference/) that add apps to the Astro Dev Toolbar. This allows you to add new features and integrations with third-party services.

![](/houston_chef.webp) **Related recipe:** [Create a dev toolbar app](/en/recipes/making-toolbar-apps/)

## Toolbar app integration setup

[Section titled “Toolbar app integration setup”](#toolbar-app-integration-setup)

Integrations can add apps to the dev toolbar in [the `astro:config:setup` hook](/en/reference/integrations-reference/#astroconfigsetup).

my-integration.js

```ts
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "my-integration",
  hooks: {
    "astro:config:setup": ({ addDevToolbarApp }) => {
      addDevToolbarApp({
        id: "my-app",
        name: "My App",
        icon: "<svg>...</svg>",
        entrypoint: "./my-app.js",
      });
    },
  },
});
```

### `addDevToolbarApp()`

[Section titled “addDevToolbarApp()”](#adddevtoolbarapp)

**Type:** `(entrypoint: DevToolbarAppEntry) => void`

**Added in:** `astro@4.0.0`

A function available to [the `astro:config:setup` hook](/en/reference/integrations-reference/#astroconfigsetup) that adds dev toolbar apps. It takes an object with the following required properties to define the toolbar app: [`id`](#id), [`name`](#name), and [`entrypoint`](#entrypoint). Optionally, you can also define an [`icon`](#icon) for your app.

### `id`

[Section titled “id”](#id)

**Type:** `string`

A unique identifier for the app. This will be used to uniquely identify the app in hooks and events.

my-integration.js

```ts
{
  id: 'my-app',
  // ...
}
```

### `name`

[Section titled “name”](#name)

**Type:** `string`

The name of the app. This will be shown to users whenever the app needs to be referenced using a human-readable name.

my-integration.js

```ts
{
  // ...
  name: 'My App',
  // ...
}
```

### `icon`

[Section titled “icon”](#icon)

**Type:** `Icon | string`\
**Default**: `"?"`

The icon used to display the app in the toolbar. This can either be an icon from [the icon list](#icons), or a string containing the SVG markup of the icon.

my-integration.js

```ts
{
  // ...
  icon: '<svg>...</svg>', // or, e.g. 'astro:logo'
  // ...
}
```

### `entrypoint`

[Section titled “entrypoint”](#entrypoint)

**Type:** `string | URL`

The path to the file that exports the dev toolbar app.

my-integration.js

```ts
{
  // ...
  entrypoint: './my-app.js',
}
```

**Added in:** `astro@5.0.0`

The function also accepts a `URL` as `entrypoint`:

my-integration.js

```ts
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "my-integration",
  hooks: {
    "astro:config:setup": ({ addDevToolbarApp }) => {
      addDevToolbarApp({
        id: "my-app",
        name: "My App",
        icon: "<svg>...</svg>",
        entrypoint: new URL("./my-app.js", import.meta.url),
      });
    },
  },
});
```

## Structure of a Dev Toolbar App

[Section titled “Structure of a Dev Toolbar App”](#structure-of-a-dev-toolbar-app)

A Dev Toolbar App is a `.js` or `.ts` file that default exports an object using the [`defineToolbarApp()` function](#definetoolbarapp) available from the `astro/toolbar` module.

src/my-app.js

```ts
import { defineToolbarApp } from "astro/toolbar";


export default defineToolbarApp({
  init(canvas) {
    const text = document.createTextNode('Hello World!');
    canvas.appendChild(text);
  },
  beforeTogglingOff() {
    const confirmation = window.confirm('Really exit?');
    return confirmation;
  }
});
```

### `defineToolbarApp()`

[Section titled “defineToolbarApp()”](#definetoolbarapp)

**Type:** `(app: DevToolbarApp) => DevToolbarApp`

**Added in:** `astro@4.7.0`

A function that defines the logic of your toolbar app when it is loaded and toggled off.

This function takes an object with an [`init()`](#init) function that will be called when the dev toolbar app is loaded. It can also take a [`beforeTogglingOff()`](#beforetogglingoff) function that will run when the toolbar app is clicked to toggle off its active status.

### `init()`

[Section titled “init()”](#init)

**Type:** `(canvas: ShadowRoot, app: ToolbarAppEventTarget, server: ToolbarServerHelpers) => void | Promise<void>`

Although not required, most apps will use this function to define the core behavior of the app. This function will be called only once when the app is loaded, which will either be when the browser is idle or when the user clicks on the app in the UI, depending on which one comes first.

The function receives three arguments to define your app logic: [`canvas`](#canvas) (to render elements to the screen), [`app`](#app) (to send and receive client-side events from the dev toolbar), and [`server`](#server) (to communicate with the server).

#### `canvas`

[Section titled “canvas”](#canvas)

**Type:** `ShadowRoot`

A standard [ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot) that the app can use to render its UI. Elements can be created and added to the ShadowRoot using the standard DOM APIs.

Every app receives its own dedicated ShadowRoot for rendering its UI. Additionally, the parent element is positioned using `position: absolute;` so the app UI will not affect the layout of an Astro page.

src/my-app.js

```ts
export default defineToolbarApp({
  init(canvas) {
    canvas.appendChild(document.createTextNode('Hello World!'))
  }
});
```

#### `app`

[Section titled “app”](#app)

**Type:** `ToolbarAppEventTarget`

**Added in:** `astro@4.7.0`

A standard [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) with a few additional [helpers for client-side events](#client-side-events) that can be used to send and receive events from the Dev toolbar.

src/my-app.js

```ts
export default defineToolbarApp({
  init(canvas, app) {
    app.onToggled(({ state }) => {
      const text = document.createTextNode(
        `The app is now ${state ? "enabled" : "disabled"}!`,
      );
      canvas.appendChild(text);
    });
  },
});
```

#### `server`

[Section titled “server”](#server)

**Type:** `ToolbarServerHelpers`

**Added in:** `astro@4.7.0`

An object that can be used to [communicate with the server](#client-server-communication).

src/my-app.js

```ts
export default defineToolbarApp({
  init(canvas, app, server) {
    server.send('my-message', { message: 'Hello!' });


    server.on('server-message', (data) => {
      console.log(data.message);
    });
  },
});
```

### `beforeTogglingOff()`

[Section titled “beforeTogglingOff()”](#beforetogglingoff)

**Type:** `(canvas: ShadowRoot) => boolean | Promise<boolean>`

**Added in:** `astro@4.7.0`

This optional function will be called when the user clicks on the app icon in the UI to toggle off the app. This function can be used, for example, to perform cleanup operations, or to ask the user for confirmation before toggling off the app.

If a falsy value is returned, the toggling off will be cancelled and the app will stay enabled.

src/my-app.js

```ts
export default defineToolbarApp({
  // ...
  beforeTogglingOff() {
    const confirmation = window.confirm('Are you sure you want to disable this app?');
    return confirmation;
  }
});
```

#### canvas

[Section titled “canvas”](#canvas-1)

**Type:** `ShadowRoot`

The ShadowRoot of the app, can be used to render any UI needed before closing. Same as the [`canvas` argument of the `init` function](#canvas).

## Client-side Events

[Section titled “Client-side Events”](#client-side-events)

In addition to the standard methods of an `EventTarget` ([`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget), [`dispatchEvent`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/dispatchEvent), [`removeEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)etc.), the [`app`](#app) object also has the following methods:

### `onToggled()`

[Section titled “onToggled()”](#ontoggled)

**Type:** `(callback: (options: {state: boolean})) => void`

**Added in:** `astro@4.7.0`

Registers a callback to be called when the user clicks on the app icon in the UI to toggle the app on or off.

src/my-app.js

```ts
app.onToggled((options) => {
  console.log(`The app is now ${options.state ? 'enabled' : 'disabled'}!`);
});
```

### `onToolbarPlacementUpdated()`

[Section titled “onToolbarPlacementUpdated()”](#ontoolbarplacementupdated)

**Type:** `(callback: (options: {placement: 'bottom-left' | 'bottom-center' | 'bottom-right'})) => void`

**Added in:** `astro@4.7.0`

This event is fired when the user changes the placement of the Dev Toolbar. This can, for example, be used to reposition the app’s UI when the toolbar is moved.

src/my-app.js

```ts
app.onToolbarPlacementUpdated((options) => {
  console.log(`The toolbar is now placed at ${options.placement}!`);
});
```

### `toggleState()`

[Section titled “toggleState()”](#togglestate)

**Type:** `(options: {state: boolean}) => void`

**Added in:** `astro@4.7.0`

Changes the state of the app. This can be used to enable or disable the app programmatically, for example, when the user clicks on a button in the app’s UI.

src/my-app.js

```ts
app.toggleState({ state: false });
```

### `toggleNotification()`

[Section titled “toggleNotification()”](#togglenotification)

**Type:** `(options: {state?: boolean, level?: 'error' | 'warning' | 'info'}) => void`

**Added in:** `astro@4.7.0`

Toggles a notification on the app icon. This can be used to inform the user that the app requires attention, or remove the current notification.

src/my-app.js

```ts
app.toggleNotification({
  state: true,
  level: 'warning',
});
```

#### `state`

[Section titled “state”](#state)

**Type:** `boolean`

Indicates whether or not the app has a notification for the user. When `true`, the app icon will be highlighted. Conversely, when `false`, the highlight will be removed. If this property is not specified, `true` will be assumed.

#### `level`

[Section titled “level”](#level)

**Type:** `'error' | 'warning' | 'info'`\
**Default:** `'error'`

Indicates the level of the notification. This will be used to determine the color and shape (dark pink circle, gold triangle, or blue square) of the highlight on the app icon. If this property is not specified, `'error'` will be assumed.

## Client-Server Communication

[Section titled “Client-Server Communication”](#client-server-communication)

Using [Vite’s methods for client-server communication](https://vite.dev/guide/api-plugin.html#client-server-communication), Dev Toolbar Apps and the server can communicate with each other. In order to facilitate sending and receiving custom messages, helper methods are provided for use both in your toolbar app (on the client) and in your integration (on the server).

### On the client

[Section titled “On the client”](#on-the-client)

In your app, use the [`server` object on the `init()` hook](#server) to send and receive messages to and from the server.

src/my-app.js

```ts
export default defineToolbarApp({
  init(canvas, app, server) {
    server.send('my-message', { message: 'Hello!' });


    server.on('server-message', (data) => {
      console.log(data.message);
    });
  },
});
```

#### `send()`

[Section titled “send()”](#send)

**Type:** `<T>(event: string, payload: T) => void`

**Added in:** `astro@4.7.0`

Sends data to the server from logic defined in your toolbar app.

src/my-app.js

```ts
init(canvas, app, server) {
  server.send('my-app:my-message', { message: 'Hello!' });
}
```

When sending messages from the client to the server, it is good practice to prefix the event name with the app ID or other namespaces to avoid conflicts with other apps or other integrations that may be listening for messages.

#### `on()`

[Section titled “on()”](#on)

**Type:** `<T>(event: string, callback: (data: T) => void) => void`

**Added in:** `astro@4.7.0`

Registers a callback to be called when the server sends a message with the specified event.

src/my-app.js

```ts
init(canvas, app, server) {
  server.on('server-message', (data) => {
    console.log(data.message);
  });
}
```

### On the server

[Section titled “On the server”](#on-the-server)

In an integration, such as [the integration that adds your toolbar app](#toolbar-app-integration-setup), use the [`astro:server:setup` hook](/en/reference/integrations-reference/#astroserversetup) to access the `toolbar` object to send and receive messages to and from your apps.

my-integration.js

```ts
export default () => ({
  name: "my-integration",
  hooks: {
    "astro:config:setup": ({ addDevToolbarApp }) => {},
    "astro:server:setup": ({ toolbar }) => {},
  },
});
```

#### `send()`

[Section titled “send()”](#send-1)

**Type:** `<T>(event: string, payload: T) => void`

**Added in:** `astro@4.7.0`

Sends data to the client.

my-integration.js

```ts
'astro:server:setup': ({ toolbar }) => {
  toolbar.send('server-message', { message: 'Hello!' });
},
```

#### `on()`

[Section titled “on()”](#on-1)

**Type:** `<T>(event: string, callback: (data: T) => void) => void`

**Added in:** `astro@4.7.0`

Registers a callback to be called when the client sends a message with the specified event.

my-integration.js

```ts
'astro:server:setup': ({ toolbar }) => {
  toolbar.on('my-app:my-message', (data) => {
    console.log(data.message);
  });
},
```

#### `onAppInitialized()`

[Section titled “onAppInitialized()”](#onappinitialized)

**Type:** `(appId: string, callback: () => void) => void`

**Added in:** `astro@4.7.0`

Registers a callback to be called when the app is initialized.

my-integration.js

```ts
'astro:server:setup': ({ toolbar }) => {
  toolbar.onAppInitialized('my-app', () => {
    console.log('The app is now initialized!');
  });
},
```

Note

The built-in `connection` event from Vite fires **before** Dev Toolbar apps are initialized and therefore cannot be used directly by apps. Use the `onAppInitialized` method to ensure that the app is fully initialized before sending messages to it.

#### `onAppToggled()`

[Section titled “onAppToggled()”](#onapptoggled)

**Type:** `(appId: string, callback: (options: {state: boolean}) => void) => void`

**Added in:** `astro@4.7.0`

Registers a callback to be called when the user clicks on the app icon in the UI to toggle the app on or off.

my-integration.js

```ts
'astro:server:setup': ({ toolbar }) => {
  toolbar.onAppToggled('my-app', ({ state }) => {
    console.log(`The app is now ${state ? 'enabled' : 'disabled'}!`);
  });
},
```

## Component Library

[Section titled “Component Library”](#component-library)

The Dev Toolbar includes a set of web components that can be used to build apps with a consistent look and feel.

### `astro-dev-toolbar-window`

[Section titled “astro-dev-toolbar-window”](#astro-dev-toolbar-window)

Shows a window.

The slot of the component will be used as the content of the window.

```html
<astro-dev-toolbar-window>
  <p>My content</p>
</astro-dev-toolbar-window>
```

When building a window using JavaScript, slotted content must go inside the light DOM of the component.

```js
const myWindow = document.createElement('astro-dev-toolbar-window');
const myContent = document.createElement('p');
myContent.textContent = 'My content';


// use appendChild directly on `window`, not `myWindow.shadowRoot`
myWindow.appendChild(myContent);
```

### `astro-dev-toolbar-button`

[Section titled “astro-dev-toolbar-button”](#astro-dev-toolbar-button)

Shows a button.

The slot of the component will be used as the content of the button.

```js
const myButton = document.createElement('astro-dev-toolbar-button');
myButton.textContent = 'Click me!';
myButton.buttonStyle = "purple";
myButton.size = "medium";


myButton.addEventListener('click', () => {
  console.log('Clicked!');
});
```

#### `size`

[Section titled “size”](#size)

**Type:** `"small" | "medium" | "large"`\
**Default:** `"small"`

The size of the button.

#### `button-style`

[Section titled “button-style”](#button-style)

**Type:** `"ghost" | "outline" | "purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"purple"`

The style of the button. When using `ghost`, the button itself is invisible and only the content of the button will be shown.

In JavaScript, set this property using the `buttonStyle` property to avoid conflict with the native `style` property.

#### `button-border-radius`

[Section titled “button-border-radius”](#button-border-radius)

**Type:** `"normal" | "rounded"`\
**Default:** `"normal"`

**Added in:** `astro@4.8.0`

The border radius of the button. When using `rounded`, the button will have rounded corners and uniform padding on all sides.

In JavaScript, set this property using the `buttonBorderRadius` property.

### `astro-dev-toolbar-badge`

[Section titled “astro-dev-toolbar-badge”](#astro-dev-toolbar-badge)

Shows a badge.

The slot of the component will be used as the content of the badge.

```html
<astro-dev-toolbar-badge>My badge</astro-dev-toolbar-badge>
```

#### `size`

[Section titled “size”](#size-1)

**Type:** `"small" | "large"`\
**Default:** `"small"`

The size of the badge.

#### `badge-style`

[Section titled “badge-style”](#badge-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"purple"`

The style (color) of the badge.

In JavaScript, set this property using the `badgeStyle` property to avoid conflict with the native `style` property.

### `astro-dev-toolbar-card`

[Section titled “astro-dev-toolbar-card”](#astro-dev-toolbar-card)

Shows a card. Specify an optional `link` attribute to make the card act like an `<a>` element.

When making a card using JavaScript, a `clickAction` property can be specified to make the card act like a `<button>` element.

The slot of the component will be used as the content of the card.

```html
<astro-dev-toolbar-card icon="astro:logo" link="https://github.com/withastro/astro/issues/new/choose">Report an issue</astro-dev-toolbar-card>
```

#### `card-style`

[Section titled “card-style”](#card-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"purple"`

The style of the card. The color is only applied to the border of the card on hover.

In JavaScript, set this property using the `cardStyle`.

### `astro-dev-toolbar-toggle`

[Section titled “astro-dev-toolbar-toggle”](#astro-dev-toolbar-toggle)

Shows a toggle element, acting as a checkbox. This element internally is a simple wrapper around a native `<input type="checkbox">` element. The checkbox element can be accessed using the `input` property.

```ts
const toggle = document.createElement('astro-dev-toolbar-toggle');


toggle.input.addEventListener('change', (evt) => {
  console.log(`The toggle is now ${evt.currentTarget.checked ? 'enabled' : 'disabled'}!`);
});
```

#### `toggle-style`

[Section titled “toggle-style”](#toggle-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"gray"`

The style of the toggle.

In JavaScript, set this property using the `toggleStyle` property.

### `astro-dev-toolbar-radio-checkbox`

[Section titled “astro-dev-toolbar-radio-checkbox”](#astro-dev-toolbar-radio-checkbox)

**Added in:** `astro@4.8.0`

Shows a radio checkbox. Similar to the `astro-dev-toolbar-toggle` component, this element is a simple wrapper around a native `<input type="radio">` element. The radio element can be accessed using the `input` property.

```ts
const radio = document.createElement('astro-dev-toolbar-radio-checkbox');


radio.input.addEventListener('change', (evt) => {
  console.log(`The radio is now ${evt.currentTarget.checked ? 'enabled' : 'disabled'}!`);
});
```

#### `radio-style`

[Section titled “radio-style”](#radio-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"purple"`

The style of the radio.

In JavaScript, set this property using the `radioStyle` property.

### `astro-dev-toolbar-highlight`

[Section titled “astro-dev-toolbar-highlight”](#astro-dev-toolbar-highlight)

Can be used to highlight an element on the page. In most cases, you’ll want to position and resize this element using the `top`, `left`, `width` and `height` CSS properties to match the element you want to highlight.

```html
<!-- Highlight the entire page -->
<astro-dev-toolbar-highlight style="top: 0; left: 0; width: 100%; height: 100%;"></astro-dev-toolbar-highlight>
```

```ts
const elementToHighlight = document.querySelector('h1');
const rect = elementToHighlight.getBoundingClientRect();


const highlight = document.createElement('astro-dev-toolbar-highlight');


highlight.style.top = `${Math.max(rect.top + window.scrollY - 10, 0)}px`;
highlight.style.left = `${Math.max(rect.left + window.scrollX - 10, 0)}px`;
highlight.style.width = `${rect.width + 15}px`;
highlight.style.height = `${rect.height + 15}px`;
highlight.icon = 'astro:logo';
```

#### `highlight-style`

[Section titled “highlight-style”](#highlight-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"purple"`

The style of the highlight.

#### `icon`

[Section titled “icon”](#icon-1)

An [icon](#icons) to show in the top right corner of the highlight.

### `astro-dev-toolbar-tooltip`

[Section titled “astro-dev-toolbar-tooltip”](#astro-dev-toolbar-tooltip)

Shows a tooltip with different sections. This component is set to `display: none;` by default and can be made visible using a `data-show="true"` attribute.

Sections are defined using the `sections` property. This property is an array of objects with the following shape:

```ts
{
  title?: string; // Title of the section
  inlineTitle?: string; // Title of the section, shown inline next to the title
  icon?: Icon; // Icon of the section
  content?: string; // Content of the section
  clickAction?: () => void | Promise<void>; // Action to perform when clicking on the section
  clickDescription?: string; // Description of the action to perform when clicking on the section
}
```

```ts
const tooltip = document.createElement('astro-dev-toolbar-tooltip');


tooltip.sections = [{
  title: 'My section',
  icon: 'astro:logo',
  content: 'My content',
  clickAction: () => {
    console.log('Clicked!')
  },
  clickDescription: 'Click me!'
}]
```

This component is often combined with the `astro-dev-toolbar-highlight` component to show a tooltip when hovering a highlighted element:

```ts
const highlight = document.createElement('astro-dev-toolbar-highlight');


// Position the highlight...


const tooltip = document.createElement('astro-dev-toolbar-tooltip');


// Add sections to the tooltip...


highlight.addEventListener('mouseover', () => {
  tooltip.dataset.show = 'true';
});


highlight.addEventListener('mouseout', () => {
  tooltip.dataset.show = 'false';
});
```

### `astro-dev-toolbar-icon`

[Section titled “astro-dev-toolbar-icon”](#astro-dev-toolbar-icon)

Shows an icon. An icon from [the icon list](#icons) can be specified using the `icon` attribute, or the SVG markup of an icon can be passed as a slot.

```html
<astro-dev-toolbar-icon icon="astro:logo" />
```

```html
<astro-dev-toolbar-icon>
  <svg>...</svg>
</astro-dev-toolbar-icon>
```

### `astro-dev-toolbar-select`

[Section titled “astro-dev-toolbar-select”](#astro-dev-toolbar-select)

**Added in:** `astro@4.6.0`

Shows a select element. Similar to the `astro-dev-toolbar-toggle` component, this element is a simple wrapper around a native `<select>` element. Use the `element` property to have access to the select element.

```ts
const mySelect = document.createElement("astro-dev-toolbar-select");
const options = [
  { label: "First option", value: "first" },
  { label: "Second option", value: "second", isDefault: true },
];
const myOptions = options.map((option) => {
  const optionEl = document.createElement("option");
  optionEl.textContent = option.label;
  optionEl.setAttribute("value", option.value);
  optionEl.selected = option.isDefault || false;
  return optionEl;
});


mySelect.selectStyle = "green";
mySelect.append(...myOptions);


mySelect.element.addEventListener("change", (evt) => {
  if (evt.currentTarget instanceof HTMLSelectElement) {
    console.log(`The select value is now ${evt.currentTarget.value}!`);
  }
});
```

#### `select-style`

[Section titled “select-style”](#select-style)

**Type:** `"purple" | "gray" | "red" | "green" | "yellow" | "blue"`\
**Default:** `"gray"`

The style of the select.

In JavaScript, set this property using the `selectStyle` property.

#### Icons

[Section titled “Icons”](#icons)

Currently, the following icons are available and can be used in any component that accepts an icon:

* `astro:logo`
* `warning`
* `arrow-down`
* `bug`
* `check-circle`
* `gear`
* `lightbulb`
* `file-search`
* `star`
* `checkmark`
* `dots-three`
* `copy`
* `compress`
* `grid`
* `puzzle`
* `approveUser`
* `checkCircle`
* `resizeImage`
* `searchFile`
* `image`
* `robot`
* `sitemap`
* `gauge`
* `person-arms-spread`
* `arrow-left`
* `houston-detective`

All of the above icons have `fill="currentColor"` set by default and will inherit their color from the parent element.


---

# Experimental Chrome DevTools workspace

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@5.13.0`

Enables experimental [Chrome DevTools workspace integration](https://developer.chrome.com/docs/devtools/workspaces) for the Astro dev server.

This feature allows you to edit files directly in Chrome DevTools and have those changes reflected in your local file system via a connected workspace folder. This is useful for applying edits such as adjusting CSS values without leaving your browser tab.

With this feature enabled, running `astro dev` will automatically configure a Chrome DevTools workspace for your project. Your project will then appear as an available [workspace source that you can connect](#connecting-your-project). Then, changes that you make in the “Sources” panel are automatically saved to your project source code.

To enable this feature, add the experimental flag `chromeDevtoolsWorkspace` to your Astro config:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
+  experimental: {
+    chromeDevtoolsWorkspace: true,
+  },
});
```

## Connecting your project

[Section titled “Connecting your project”](#connecting-your-project)

Astro will create the necessary configuration file to support Chrome DevTools workspaces. However, your project must also be [connected as a source](https://developer.chrome.com/docs/devtools/workspaces#manual-connection) to enable file saving.

1. [Start the Astro dev server](/en/develop-and-build/#start-the-astro-dev-server) with the appropriate CLI command for your package manager.

2. Navigate to your site preview (e.g. `http://localhost:4321/`) in Chrome and open DevTools.

3. Under the **Sources** > **Workspaces** tab, you will find your Astro project folder. Click **Connect** to add your directory as a workspace.

See the [Chrome DevTools workspace documentation](https://developer.chrome.com/docs/devtools/workspaces#connect) for more information.

# Experimental client prerendering

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@4.2.0`

Enables pre-rendering your prefetched pages on the client in supported browsers.

This feature uses the experimental [Speculation Rules Web API](https://developer.mozilla.org/en-US/docs/Web/API/Speculation_Rules_API) and enhances the default `prefetch` behavior globally to prerender links on the client. You may wish to review the [possible risks when prerendering on the client](https://developer.mozilla.org/en-US/docs/Web/API/Speculation_Rules_API#unsafe_prefetching) before enabling this feature.

Enable client side prerendering in your `astro.config.mjs` along with any desired `prefetch` configuration options:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  prefetch: {
    prefetchAll: true,
    defaultStrategy: 'viewport',
  },
  experimental: {
    clientPrerender: true,
  },
});
```

Continue to use the `data-astro-prefetch` attribute on any `<a />` link on your site to opt in to prefetching. Instead of appending a `<link>` tag to the head of the document or fetching the page with JavaScript, a `<script>` tag will be appended with the corresponding speculation rules.

Client side prerendering requires browser support. If the Speculation Rules API is not supported, `prefetch` will fallback to the supported strategy.

See the [Prefetch Guide](/en/guides/prefetch/) for more `prefetch` options and usage.

# Experimental Intellisense for content collections

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@4.14.0`

Enables Intellisense features (e.g. code completion, quick hints) for your content collection entries in compatible editors.

When enabled, this feature will generate and add JSON schemas to the `.astro` directory in your project. These files can be used by the Astro language server to provide Intellisense inside content files (`.md`, `.mdx`, `.mdoc`).

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  experimental: {
    contentIntellisense: true,
  },
});
```

To use this feature with the Astro VS Code extension, you must also enable the `astro.content-intellisense` option in your VS Code settings. For editors using the Astro language server directly, pass the `contentIntellisense: true` initialization parameter to enable this feature.

# Experimental queued rendering

**Type:** `object`\
**Default:** `{ enabled: false }`

**Added in:** `astro@6.0.0`

Enables an experimental, more performant rendering infrastructure that is based on a queue instead of recursion.

By default, Astro renders `.astro`, `.md`, and `.mdx` files using a recursion algorithm. It takes as input a series of components that are serialised in a tree-like structure, and for each node of the tree, Astro calls a render function.

When queued rendering is enabled, Astro traverses all nodes in the tree and emits a [depth-first](https://en.wikipedia.org/wiki/Depth-first_search) list of nodes. This list is then iterated and rendered, without the need of a recursion algorithm. This rendering is more memory efficient, and it should provide more benefits in big projects.

To enable this feature with default settings, set `queuedRendering.enabled` to `true` in your Astro config:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
  experimental: {
+    queuedRendering: {
+      enabled: true
+    }
  }
});
```

In a future major version, Astro will use this new compiler by default, but you can opt in to the future behavior early using the `experimental.queuedRendering` flag.

## Configuration

[Section titled “Configuration”](#configuration)

The queued rendering engine comes with additional, low-level features, which allow you to experiment with other possible optimizations. These optimisations aren’t directly part of the queued engine, and may be removed if they are proven inefficient during this experimental phase of testing.

### Node pooling

[Section titled “Node pooling”](#node-pooling)

**Type:** `number`\
**Default:** `1000`

**Added in:** `astro@6.0.0`

Node pooling is a caching system designed to reuse component nodes across renders. This feature is automatically enabled with a reasonable default according to our early testing. However, you can configure the size of the pool to increase or decrease the number of nodes combined in a single pool according to your project needs. To disable this feature entirely, set `poolSize` to `0`.

The node pooling is very effective when rendering static pages because it saves some memory when building websites with many pages that share the same components.

The node pooling is turned off for pages rendered on demand because these rendering requests do not share memory, and there are therefore no savings to be gained with this strategy.

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
  experimental: {
    queuedRendering: {
      enabled: true,
+      poolSize: 3000 // use a pool of 3000 nodes
    }
  }
});
```

### Content caching

[Section titled “Content caching”](#content-caching)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

Content caching is another technique to reuse values (usually strings) during the rendering of a page. Currently, this feature can only be enabled or disabled with no further configuration. It’s disabled by default, but when enabled, the experimental queued engine will choose a reasonable default cache size for most large content collections:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
  experimental: {
    queuedRendering: {
      enabled: true,
+      contentCache: true
    }
  }
});
```

# Experimental route caching

**Type:** `object`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

Enables a platform-agnostic API for caching responses from [on-demand rendered](/en/guides/on-demand-rendering/) pages and endpoints. Cache directives set in your routes are translated into the appropriate headers or runtime behavior depending on your configured cache provider.

Route caching builds on standard [HTTP caching semantics](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching), including `max-age` and [`stale-while-revalidate`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#stale-while-revalidate), with support for tag-based and path-based invalidation, config-level route rules, and pluggable cache providers that adapters can set automatically.

This feature requires [on-demand rendering](/en/guides/on-demand-rendering/). Prerendered pages are already static and do not use route caching.

To enable this feature, configure `experimental.cache` with a cache provider in your Astro config:

astro.config.mjs

```js
import { defineConfig, memoryCache } from 'astro/config';
import node from '@astrojs/node';


export default defineConfig({
  adapter: node({ mode: 'standalone' }),
  experimental: {
    cache: {
      provider: memoryCache(),
    },
  },
});
```

## Using route caching

[Section titled “Using route caching”](#using-route-caching)

Use `Astro.cache` in `.astro` pages or `context.cache` in API routes and middleware to control caching per request. Cache defaults for groups of routes can also be defined declaratively in your config using [`experimental.routeRules`](#route-rules).

### Checking if caching is enabled

[Section titled “Checking if caching is enabled”](#checking-if-caching-is-enabled)

Use `cache.enabled` to check whether a cache provider is configured and active. This returns `false` when no provider is configured, or in development mode:

src/pages/products/\[id].astro

```astro
---
if (Astro.cache.enabled) {
  const tags = await getProductTags(Astro.params.id);
  Astro.cache.set({ maxAge: 3600, tags });
}
---
```

When caching is not enabled, `cache.set()`, `cache.tags`, and `cache.options` will log a warning, and `cache.invalidate()` will throw an error. Use `cache.enabled` to check before calling these methods.

### Setting cache options

[Section titled “Setting cache options”](#setting-cache-options)

Call `cache.set()` with an options object to enable caching for the current response.

The following example caches a page for 2 minutes, serves stale content for 1 minute while revalidating, and tags the response for targeted invalidation:

src/pages/index.astro

```diff
---
export const prerender = false; // Not needed in 'server' mode


+Astro.cache.set({
+  maxAge: 120,
+  swr: 60,
+  tags: ['home'],
+});
---


<html><body>Cached page</body></html>
```

In API routes and middleware, use `context.cache`:

src/pages/api/data.ts

```diff
export function GET(context) {
+  context.cache.set({
+    maxAge: 300,
+    tags: ['api', 'data'],
+  });
  return Response.json({ ok: true });
}
```

### Opting out of caching

[Section titled “Opting out of caching”](#opting-out-of-caching)

Call `cache.set(false)` to explicitly opt a request out of caching. This is useful when a matched [route rule](#route-rules) would otherwise cache the response:

src/pages/dashboard.astro

```astro
---
if (isPersonalized) {
  Astro.cache.set(false);
}
---
```

### Reading cache state

[Section titled “Reading cache state”](#reading-cache-state)

Access the current accumulated cache options via `cache.options`:

src/pages/api/debug.ts

```ts
const { maxAge, swr, tags } = context.cache.options;
```

### Merge behavior

[Section titled “Merge behavior”](#merge-behavior)

Multiple calls to `cache.set()` within a single request are merged:

* **Scalar values** (`maxAge`, `swr`, `etag`): last-write-wins
* **`lastModified`**: most recent date wins
* **`tags`**: accumulate across all calls

Middleware, layouts, content loaders, and page code can each contribute cache directives independently.

### Dev mode behavior

[Section titled “Dev mode behavior”](#dev-mode-behavior)

In dev mode, the cache API is available so that route code does not need conditional checks, but no actual caching occurs. `cache.enabled` is `false`, and `cache.set()` and `cache.invalidate()` are no-ops. To test your caching locally, build then preview your site.

## Using with live content collections

[Section titled “Using with live content collections”](#using-with-live-content-collections)

Route caching integrates directly with [live content collections](/en/guides/content-collections/#live-content-collections). `cache.set()` accepts `CacheHint` and `LiveDataEntry` objects natively, allowing cache hints from loaders to be passed through without manually setting headers.

A [live loader](/en/reference/content-loader-reference/#live-loaders) can return a `cacheHint` on individual entries or on the collection as a whole. These hints include `tags` (for targeted invalidation) and `lastModified` (for freshness). When passed to `cache.set()`, they merge with any other cache options already set on the page.

### Passing cache hints from entries

[Section titled “Passing cache hints from entries”](#passing-cache-hints-from-entries)

Pass the `cacheHint` returned by `getLiveEntry()` or `getLiveCollection()` directly to `cache.set()`.

The following example passes the loader’s cache hint and adds a `maxAge` to control how long the response stays fresh:

src/pages/products/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { entry, error, cacheHint } = await getLiveEntry('products', Astro.params.id);


if (error) {
  return Astro.redirect('/404');
}


if (cacheHint) {
  Astro.cache.set(cacheHint);
}
Astro.cache.set({ maxAge: 300 });
---


<h1>{entry.data.name}</h1>
```

A [`LiveDataEntry`](/en/reference/content-loader-reference/#livedataentry) can also be passed directly. Astro extracts its `cacheHint` automatically:

src/pages/products/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { entry, error } = await getLiveEntry('products', Astro.params.id);


if (error) {
  return Astro.redirect('/404');
}


Astro.cache.set(entry);
Astro.cache.set({ maxAge: 300, swr: 60 });
---


<h1>{entry.data.name}</h1>
```

### Invalidating by entry

[Section titled “Invalidating by entry”](#invalidating-by-entry)

`cache.invalidate()` also accepts a `LiveDataEntry`, invalidating all cached responses tagged with that entry’s cache tags.

The following example invalidates the cached response for a specific product entry:

src/pages/api/revalidate.ts

```ts
import { getLiveEntry } from 'astro:content';


export async function POST(context) {
  const { entry } = await getLiveEntry('products', 'featured');
  if (entry) {
    await context.cache.invalidate(entry);
  }
  return Response.json({ ok: true });
}
```

### Collection-level cache hints

[Section titled “Collection-level cache hints”](#collection-level-cache-hints)

When fetching a full collection with `getLiveCollection()`, Astro merges cache hints from the collection response and all individual entries: tags are accumulated, and the most recent `lastModified` wins.

The following example passes the merged cache hint from a collection and sets a 10-minute freshness window:

src/pages/products/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';


const { entries, error, cacheHint } = await getLiveCollection('products');


if (error) {
  return new Response('Error loading products', { status: 500 });
}


if (cacheHint) {
  Astro.cache.set(cacheHint);
}
Astro.cache.set({ maxAge: 600 });
---


<ul>
  {entries.map((p) => <li>{p.data.name}</li>)}
</ul>
```

See the [Content Loader Reference](/en/reference/content-loader-reference/) for more about implementing cache hints in your live loaders.

## Invalidation

[Section titled “Invalidation”](#invalidation)

Purge cached entries by tag or path using `cache.invalidate()`.

The following example creates an API route that invalidates by tag and by path:

src/pages/api/revalidate.ts

```ts
export async function POST(context) {
  // Invalidate all entries tagged 'data'
  await context.cache.invalidate({ tags: ['data'] });


  // Invalidate a specific path
  await context.cache.invalidate({ path: '/api/data' });


  return Response.json({ purged: true });
}
```

Tag-based invalidation removes all cached entries whose tags include any of the provided tags. Path-based invalidation is exact-match only (no [glob](/en/guides/imports/#glob-patterns) or wildcard patterns).

## Route rules

[Section titled “Route rules”](#route-rules)

**Type:** `Record<string, { maxAge?: number; swr?: number; tags?: string[]; }>`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

`experimental.routeRules` sets default cache options for routes declaratively in your config, without modifying route code. This is useful for applying caching to large groups of routes at once.

The following example caches all API routes with stale-while-revalidate, product pages with a 1-hour freshness window, and blog posts for 5 minutes:

astro.config.mjs

```diff
import { defineConfig, memoryCache } from 'astro/config';


export default defineConfig({
  experimental: {
    cache: {
      provider: memoryCache(),
    },
+    routeRules: {
      +'/api/*': { swr: 600 },
      +'/products/*': { maxAge: 3600, tags: ['products'] },
      +'/blog/[...slug]': { maxAge: 300, swr: 60 },
+    },
  },
});
```

### Pattern syntax

[Section titled “Pattern syntax”](#pattern-syntax)

Route patterns support:

* **Static paths**: `/about`, `/api/health`
* **Dynamic parameters**: `/products/[id]`, `/blog/[slug]`
* **Rest parameters**: `/docs/[...path]`
* **Glob wildcards**: `/api/*`

Patterns use the same matching and priority rules as Astro’s [file-based routing](/en/guides/routing/#route-priority-order), so more specific patterns take precedence.

### Merging with per-route `cache.set()`

[Section titled “Merging with per-route cache.set()”](#merging-with-per-route-cacheset)

Per-route `cache.set()` calls merge with config-level route rules. Route code can override or extend the defaults set in config.

## Cache providers

[Section titled “Cache providers”](#cache-providers)

Cache behavior is determined by the configured **cache provider**. Providers fall into two categories:

### CDN providers

[Section titled “CDN providers”](#cdn-providers)

CDN providers translate cache directives into response headers (e.g. `CDN-Cache-Control`, `Cache-Tag`) and rely on a CDN or reverse proxy to handle caching. These internal headers are stripped before the response reaches the client.

A CDN provider implements `setHeaders()` to produce the appropriate headers.

### Runtime providers

[Section titled “Runtime providers”](#runtime-providers)

Runtime providers implement `onRequest()` to intercept requests and cache responses in-process. They add an `X-Astro-Cache` response header for observability:

* **`HIT`**: response served from cache
* **`MISS`**: response rendered fresh and stored in cache
* **`STALE`**: stale response served while revalidating in the background

### Built-in memory cache provider

[Section titled “Built-in memory cache provider”](#built-in-memory-cache-provider)

Astro includes a built-in in-memory LRU runtime cache provider suitable for single-instance deployments. Import `memoryCache` from `astro/config`:

astro.config.mjs

```js
import { defineConfig, memoryCache } from 'astro/config';


export default defineConfig({
  experimental: {
    cache: {
      provider: memoryCache({ max: 500 }),
    },
  },
});
```

#### Cache key behavior

[Section titled “Cache key behavior”](#cache-key-behavior)

The memory provider automatically normalizes cache keys for better hit rates:

* **Query parameter sorting**: Parameters are sorted alphabetically, so `/page?b=2&a=1` and `/page?a=1&b=2` resolve to the same cache entry.
* **Tracking parameter exclusion**: Common analytics and tracking parameters (e.g. `utm_source`, `fbclid`, `gclid`) are excluded from the cache key by default. This prevents cache fragmentation from marketing links without affecting your page content.
* **Vary header support**: When a response includes a [`Vary`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary) header, the memory provider uses the specified request header values to create separate cache entries for each variant. For example, a response with `Vary: Accept-Language` will cache different versions for different languages.

Note

The `Cookie` header is always ignored for Vary-based cache keying because it has extremely high cardinality (every user typically has different cookies), making it effectively uncacheable.

#### `memoryCache()` options

[Section titled “memoryCache() options”](#memorycache-options)

**Type:** `{ max?: number; query?: object }`\
**Default:** `{ max: 1000 }`

##### `max`

[Section titled “max”](#max)

**Type:** `number`\
**Default:** `1000`

Maximum number of entries to keep in cache. When the cache exceeds this limit, the least recently used entry is evicted.

##### `query`

[Section titled “query”](#query)

Controls how query parameters are handled in cache keys.

###### `query.sort`

[Section titled “query.sort”](#querysort)

**Type:** `boolean`\
**Default:** `true`

Sort query parameters alphabetically so that parameter order does not affect the cache key. Set to `false` to disable sorting, and cache URLs with different query parameter order separately. This is useful when parameter order is significant.

###### `query.exclude`

[Section titled “query.exclude”](#queryexclude)

**Type:** `string[]`\
**Default:** `['utm_*', 'fbclid', 'gclid', 'gbraid', 'wbraid', 'dclid', 'msclkid', 'twclid', 'li_fat_id', 'mc_cid', 'mc_eid', '_ga', '_gl', '_hsenc', '_hsmi', '_ke', 'oly_anon_id', 'oly_enc_id', 'rb_clickid', 's_cid', 'vero_id', 'wickedid', 'yclid', '__s', 'ref']`

Exclude query parameters whose names match these patterns from the cache key. Supports glob wildcards (e.g. `"utm_*"`). Cannot be used together with `include`.

By default, common tracking and analytics parameters are excluded. Set to `[]` to include all query parameters in the cache key:

astro.config.mjs

```js
memoryCache({
  query: { exclude: [] },
});
```

###### `query.include`

[Section titled “query.include”](#queryinclude)

**Type:** `string[]`

Only include these query parameter names in the cache key. All other parameters are ignored, including the default tracking parameter exclusions. Cannot be used together with `exclude`.

The following example only uses `page` and `sort` parameters in the cache key, ignoring all others:

astro.config.mjs

```js
memoryCache({
  query: { include: ['page', 'sort'] },
});
```

## Writing a custom cache provider

[Section titled “Writing a custom cache provider”](#writing-a-custom-cache-provider)

A cache provider has two parts:

1. **The runtime module** — A file that **default-exports** a `CacheProviderFactory` function. This module is bundled into your SSR output, so it must be runtime-agnostic: avoid Node.js built-in modules (e.g. `node:fs`, `node:path`) unless your target runtime supports them.

2. **The config helper** — A function exported for users to call in `astro.config.mjs`. It returns a [`CacheProviderConfig` object](#cacheproviderconfig) that tells Astro where to find the runtime module and what options to pass it. This is the same pattern used by `memoryCache()` from `astro/config`.

The following example shows a config helper that accepts typed options and points to a runtime module:

my-provider/config.ts

```ts
import type { CacheProviderConfig } from 'astro';


interface MyProviderOptions {
  apiKey: string;
  region?: string;
}


export function myCache(options: MyProviderOptions): CacheProviderConfig {
  return {
    entrypoint: 'my-provider/runtime', // resolved from the project root
    config: options, // passed to the factory at runtime
  };
}
```

The config helper is then called in the Astro config:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';
import { myCache } from 'my-provider/config';


export default defineConfig({
  experimental: {
    cache: {
      provider: myCache({ apiKey: '...' }),
    },
  },
});
```

The runtime module default-exports a factory that receives the serialized `config` and returns a [`CacheProvider`](#cacheprovider-interface):

my-provider/runtime.ts

```ts
import type { CacheProviderFactory } from 'astro';


const factory: CacheProviderFactory = (config) => {
  return {
    name: 'my-cache-provider',


    // CDN-style: translate cache options into response headers
    setHeaders(options) {
      const headers = new Headers();
      if (options.maxAge !== undefined) {
        let value = `max-age=${options.maxAge}`;
        if (options.swr !== undefined) {
          value += `, stale-while-revalidate=${options.swr}`;
        }
        headers.set('CDN-Cache-Control', value);
      }
      if (options.tags?.length) {
        headers.set('Cache-Tag', options.tags.join(','));
      }
      return headers;
    },


    // Runtime-style: intercept requests (optional)
    async onRequest(context, next) {
      // Check cache, call next(), store response...
      return next();
    },


    // Handle invalidation requests
    async invalidate(options) {
      // Purge by tags or path...
    },
  };
};


export default factory;
```

### `CacheProvider` interface

[Section titled “CacheProvider interface”](#cacheprovider-interface)

Describes a provider used for caching. This requires the `name` and `invalidate()` properties and accepts optional properties.

#### `name`

[Section titled “name”](#name)

**Type:** `string`

A unique name for the provider, used in logs and for identification.

#### `setHeaders()`

[Section titled “setHeaders()”](#setheaders)

**Type:** `(options: CacheOptions) => Headers`

Translates cache options into response headers. Called after the response is rendered but before it is sent to the client. These headers are stripped from the final response.

#### `onRequest()`

[Section titled “onRequest()”](#onrequest)

**Type:** `(context: { request: Request; url: URL; waitUntil?: (promise: Promise<unknown>) => void }, next: MiddlewareNext) => Promise<Response>`

Intercepts requests to implement runtime caching. The `context` includes a `waitUntil` function (when available in the runtime) for background work such as stale-while-revalidate.

#### `invalidate()`

[Section titled “invalidate()”](#invalidate)

**Type:** `(options: InvalidateOptions) => Promise<void>`

Handles purge requests by tag or path.

### `CacheProviderFactory`

[Section titled “CacheProviderFactory”](#cacheproviderfactory)

**Type:** `(config: Record<string, any> | undefined) => CacheProvider`

The factory function type. Receives the provider’s serializable config object from the Astro config.

## API reference

[Section titled “API reference”](#api-reference)

### `cache.enabled`

[Section titled “cache.enabled”](#cacheenabled)

**Type:** `boolean`

Whether caching is active. Returns `false` when no cache provider is configured or in development mode. Returns `true` when a cache provider is configured and the app is running in production.

### `cache.set()`

[Section titled “cache.set()”](#cacheset)

**Type:** `(options: CacheOptions | false) => void`

Sets cache options for the current request. Pass `false` to opt out of caching.

#### `CacheOptions`

[Section titled “CacheOptions”](#cacheoptions)

##### `maxAge`

[Section titled “maxAge”](#maxage)

**Type:** `number`

Time in seconds the response is considered fresh.

##### `swr`

[Section titled “swr”](#swr)

**Type:** `number`

Stale-while-revalidate window in seconds. Stale content is served while a fresh response is generated in the background.

##### `tags`

[Section titled “tags”](#tags)

**Type:** `string[]`

Cache tags for targeted invalidation. Tags accumulate across multiple `set()` calls.

##### `lastModified`

[Section titled “lastModified”](#lastmodified)

**Type:** `Date`

When multiple `set()` calls provide `lastModified`, the most recent date wins.

##### `etag`

[Section titled “etag”](#etag)

**Type:** `string`

Entity tag for conditional requests.

### `cache.options`

[Section titled “cache.options”](#cacheoptions-1)

**Type:** `Readonly<CacheOptions>`

Read-only snapshot of the current accumulated cache options, including all merged `maxAge`, `swr`, `etag`, `lastModified`, and `tags` values.

### `cache.tags`

[Section titled “cache.tags”](#cachetags)

**Type:** `string[]`

Read-only array of all accumulated cache tags.

### `cache.invalidate()`

[Section titled “cache.invalidate()”](#cacheinvalidate)

**Type:** `(options: InvalidateOptions) => Promise<void>`

Purges cached entries. Requires a configured cache provider.

#### `InvalidateOptions`

[Section titled “InvalidateOptions”](#invalidateoptions)

##### `path`

[Section titled “path”](#path)

**Type:** `string`

Exact path to invalidate. No glob or wildcard support.

##### `tags`

[Section titled “tags”](#tags-1)

**Type:** `string | string[]`

Tag or tags to invalidate. All entries with matching tags are purged.

### `CacheProviderConfig`

[Section titled “CacheProviderConfig”](#cacheproviderconfig)

**Type:** `{ entrypoint: string | URL; config?: Record<string, any> }`

The configuration object passed to `experimental.cache.provider`. Use a helper function (e.g. `memoryCache()`) for type-safe configuration.

## Error handling

[Section titled “Error handling”](#error-handling)

### `CacheNotEnabled`

[Section titled “CacheNotEnabled”](#cachenotenabled)

Thrown when `cache.invalidate()` is called without a configured cache provider. Other cache methods (`set()`, `tags`, `options`) no-op when caching is not configured, logging a one-time warning on the first `set()` call.

### `CacheProviderNotFound`

[Section titled “CacheProviderNotFound”](#cacheprovidernotfound)

Thrown at build time when the configured cache provider cannot be resolved. This typically means the package is not installed or the import path is incorrect.

## Further reading

[Section titled “Further reading”](#further-reading)

For full details and to give feedback on this experimental API, see the [Route Caching RFC](https://github.com/withastro/roadmap/pull/1245).

# Experimental Rust compiler

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

Enables using the new Rust-based compiler for Astro files. This compiler is faster, provides better error messages, and generally has better support for modern JavaScript, TypeScript, and CSS features.

In a future major version, Astro will use this new compiler by default, but you can opt in to the future behavior early using the `experimental.rustCompiler` flag.

To give feedback on the compiler, or to keep up with its development, see the [RFC for a new compiler for Astro](https://github.com/withastro/roadmap/discussions/1306) for more information and discussion.

## Usage

[Section titled “Usage”](#usage)

This experimental flag requires no specific usage and only affects which compiler Astro uses for your project.

To enable the Rust compiler, add the following to your `astro.config.mjs`:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
+  experimental: {
+    rustCompiler: true
+  }
});
```

and then install the `@astrojs/compiler-rs` package into your project:

* npm

  ```shell
  npm install @astrojs/compiler-rs
  ```

* pnpm

  ```shell
  pnpm add @astrojs/compiler-rs
  ```

* Yarn

  ```shell
  yarn add @astrojs/compiler-rs
  ```

### Expected differences

[Section titled “Expected differences”](#expected-differences)

Unlike Astro’s current Go compiler, this experimental Rust compiler will not correct invalid HTML structure. For example, the following notable patterns will be left as written, and no longer corrected:

* `<p><div>Bad nesting</div></p>` (instead of removing the `div` from of the `p`)
* `<p>My paragraph` (instead of adding the missing closing `</p>` tag)

This means that if your Astro files contain invalid HTML, you may see a different output from the Rust compiler than you did with the previous compiler, or may encounter errors while building.

## Limitations

[Section titled “Limitations”](#limitations)

At this time, the Rust compiler does not output the required metadata for the dev toolbar audits to work correctly.

# Experimental SVG optimization

**Type:** `boolean | object`\
**Default:** `false`

**Added in:** `astro@5.16.0`

This experimental feature enables automatic optimization of your [SVG components](/en/guides/images/#svg-components) using [SVGO](https://svgo.dev/) during build time.

When enabled, your imported SVG files used as components will be optimized for smaller file sizes and better performance while maintaining visual quality. This can significantly reduce the size of your SVG assets by removing unnecessary metadata, comments, and redundant code.

To enable this feature with default settings, set it to `true` in your Astro config:

astro.config.mjs

```diff
import { defineConfig } from "astro/config"


export default defineConfig({
  experimental: {
+    svgo: true
  }
})
```

## Usage

[Section titled “Usage”](#usage)

No change to using SVG components is required to take advantage of this feature. With experimental `svgo` enabled, all your SVG component import files will be automatically optimized:

src/pages/index.astro

```astro
---
import Logo from '../assets/logo.svg';
---


<Logo />
```

The SVG will be optimized during the build process, resulting in smaller file sizes in your production build.

Note that this optimization applies to every SVG component import in your project. It is not possible to opt out on a per-component basis.

## Configuration

[Section titled “Configuration”](#configuration)

You can pass an [SVGO configuration object](https://github.com/svg/svgo#configuration) to customize optimization behavior:

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      multipass: true,
      floatPrecision: 2,
      plugins: [
        'preset-default',
        'removeXMLNS',
        {
          name: "removeXlink",
          params: {
            includeLegacy: true
          }
        }
      ]
    }
  }
})
```

### `plugins`

[Section titled “plugins”](#plugins)

**Type:** `Array<string | PluginConfig>`\
**Default:** `[]`

An array of [SVGO plugins](https://svgo.dev/docs/plugins/) that will be used to optimize your SVG component imports.

This can include SVGO’s [`preset-default`](https://svgo.dev/docs/preset-default/) plugin collection, individual built-in plugins, or [custom plugins](https://svgo.dev/docs/plugins-api/).

To use a plugin’s default configuration, add its name to the array. If you need more control, use the `overrides` parameter to customize specific plugins within `preset-default`, or pass an object with a plugin’s `name` to override its individual parameters.

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      plugins: [
        {
          name: 'preset-default',
          params: {
            overrides: {
              convertPathData: false,
              convertTransform: {
                degPrecision: 1,
                transformPrecision: 3
              },
              inlineStyles: false
            },
          },
        },
        'removeXMLNS',
        {
          name: "removeXlink",
          params: {
            includeLegacy: true
          }
        }
      ]
    }
  }
})
```

### Other configuration options

[Section titled “Other configuration options”](#other-configuration-options)

There are a few [SVGO configuration options](https://github.com/svg/svgo/blob/66d503a48c6c95661726262a3068053c429b06a9/lib/types.ts#L335), especially `floatPrecision` and `multipass`, that can be passed directly to your config object:

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      multipass: true,
      floatPrecision: 2
    }
  }
})
```

The `multipass` option sets whether to run the optimization engine multiple times until no further optimizations are found. The `floatPrecision` option sets the number of decimal places to preserve globally, but can be overridden for a specific plugin by specifying a custom value in its `params` property.

## Common use cases

[Section titled “Common use cases”](#common-use-cases)

SVGO provides an extensive [default plugin list](https://svgo.dev/docs/preset-default/) with opinionated optimizations. While using this preset is more convenient than adding each plugin individually, you may need to customize it further. For example, it may remove items or clean up too aggressively for your situation, especially when using animations.

### Preserve specific attributes

[Section titled “Preserve specific attributes”](#preserve-specific-attributes)

You may want to preserve certain SVG attributes and elements, such as `<style>`, that SVGO inlines or removes by default:

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      plugins: [
        {
          name: 'preset-default',
          params: {
            overrides: {
              inlineStyles: false, // Preserve style elements for CSP hashing
              removeDesc: false // Keep element regardless of contents
            }
          }
        }
      ]
    }
  }
})
```

### Remove specific elements

[Section titled “Remove specific elements”](#remove-specific-elements)

You can configure plugins to remove specific unwanted elements like metadata or hidden layers. Note that many plugins are already included in `preset-default`, so you typically only need to configure their behavior:

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      plugins: [
        {
          name: 'preset-default',
          params: {
            overrides: {
              removeHiddenElems: {
                isHidden: false,
                displayNone: false
              }
            },
          },
        },
        'removeRasterImages'
      ]
    }
  }
})
```

### Optimize for inlining in modern HTML5

[Section titled “Optimize for inlining in modern HTML5”](#optimize-for-inlining-in-modern-html5)

Inline SVG does not require the `xmlns` attribute and can be safely converted to the SVG 2 specification. The `removeXMLNS` and `removeXlink` plugins are recommended for this purpose:

astro.config.mjs

```js
export default defineConfig({
  experimental: {
    svgo: {
      plugins: [
        'preset-default',
        'removeXMLNS',
        {
          name: "removeXlink",
          params: {
            includeLegacy: true
          }
        }
      ]
    }
  }
})
```

## How it works

[Section titled “How it works”](#how-it-works)

SVG optimization happens during the build process, not at runtime:

* In **development mode**, SVG files are not optimized to ensure faster rebuild times and a smoother development experience.
* In **production builds**, all imported SVG files are optimized once during the build process, resulting in smaller file sizes.
* There is **no runtime overhead** - optimized SVGs are served as pre-processed static assets.

While the optimization process may slightly increase your build times, the result is smaller file sizes and faster page loads for your users.

## Further reading

[Section titled “Further reading”](#further-reading)

* [SVGO documentation](https://svgo.dev/)


---

# Astro Font Provider API

**Added in:** `astro@6.0.0`

The [Fonts API](/en/guides/fonts/) allows you to access fonts in a unified way. Each family requires the use of an Astro Font Provider, which either downloads font files from a remote service or loads local font files from disk.

## Built-in providers

[Section titled “Built-in providers”](#built-in-providers)

Astro exports built-in font providers from `astro/config`:

```js
import { fontProviders } from 'astro/config'
```

To use a built-in font provider, set [`provider`](/en/reference/configuration-reference/#fontprovider) with the appropriate value for your chosen font provider:

* [Adobe](#adobe)
* [Bunny](#bunny)
* [Fontshare](#fontshare)
* [Fontsource](#fontsource)
* [Google](#google)
* [Google Icons](#google-icons)
* [Local](#local)
* [NPM](#npm)

### Adobe

[Section titled “Adobe”](#adobe)

Retrieves fonts from [Adobe](https://fonts.adobe.com/):

```js
provider: fontProviders.adobe({ id: "your-id" })
```

Pass the Adobe font provider an ID loaded as an [environment variable in your Astro config file](/en/guides/environment-variables/#in-the-astro-config-file).

### Bunny

[Section titled “Bunny”](#bunny)

Retrieves fonts from [Bunny](https://fonts.bunny.net/):

```js
provider: fontProviders.bunny()
```

### Fontshare

[Section titled “Fontshare”](#fontshare)

Retrieves fonts from [Fontshare](https://www.fontshare.com/):

```js
provider: fontProviders.fontshare()
```

### Fontsource

[Section titled “Fontsource”](#fontsource)

Retrieves fonts from [Fontsource](https://fontsource.org/):

```js
provider: fontProviders.fontsource()
```

### Google

[Section titled “Google”](#google)

Retrieves fonts from [Google](https://fonts.google.com/):

```js
provider: fontProviders.google()
```

The provider comes with the following family-specific options that can be added in the [`font.options` object](/en/reference/configuration-reference/#fontoptions).

#### `experimental.glyphs`

[Section titled “experimental.glyphs”](#experimentalglyphs)

**Type:** `string[]`

Allows specifying a list of glyphs to be included in the font for each font family. This can reduce the size of the font file:

```js
{
  // ...
  provider: fontProviders.google(),
  options: {
    experimental: {
      glyphs: ["a"]
    }
  }
}
```

#### `experimental.variableAxis`

[Section titled “experimental.variableAxis”](#experimentalvariableaxis)

**Type:** `Partial<Record<VariableAxis, ([string, string] | string)[]>>`

Allows setting variable axis configuration:

```js
{
  // ...
  provider: fontProviders.google(),
  options: {
    experimental: {
      variableAxis: {
        slnt: [["-15", "0"]],
        CASL: [["0", "1"]],
        CRSV: ["1"],
        MONO: [["0", "1"]],
      }
    }
  }
}
```

### Google Icons

[Section titled “Google Icons”](#google-icons)

Retrieves fonts from [Google Icons](https://fonts.google.com/icons):

```js
provider: fontProviders.googleicons()
```

The provider comes with the following family-specific options that can be added in the [`font.options` object](/en/reference/configuration-reference/#fontoptions).

#### `experimental.glyphs`

[Section titled “experimental.glyphs”](#experimentalglyphs-1)

**Type:** `string[]`

When resolving the new Material Symbols icons, allows specifying a list of glyphs to be included in the font for each font family. This can reduce the size of the font file:

```js
{
  // ...
  provider: fontProviders.googleicons(),
  options: {
    experimental: {
      glyphs: ["a"]
    }
  }
}
```

### Local

[Section titled “Local”](#local)

Retrieves fonts from disk:

```js
provider: fontProviders.local()
```

The provider requires that [`variants`](#variants) be defined in the [`font.options` object](/en/reference/configuration-reference/#fontoptions).

#### `variants`

[Section titled “variants”](#variants)

**Type:** `LocalFontFamily["variants"]`

The `options.variants` property is required. Each variant represents a [`@font-face` declaration](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/) and requires a [`src`](#src).

Additionally, [some other properties](#other-properties) may be specified within each variant.

astro.config.mjs

```js
import { defineConfig, fontProviders } from "astro/config";


export default defineConfig({
  fonts: [{
    provider: fontProviders.local(),
    name: "Custom",
    cssVariable: "--font-custom",
    options: {
      variants: [
        {
          weight: 400,
          style: "normal",
          src: ["./src/assets/fonts/custom-400.woff2"]
        },
        {
          weight: 700,
          style: "normal",
          src: ["./src/assets/fonts/custom-700.woff2"]
        }
        // ...
      ]
    }
  }]
});
```

##### `weight`

[Section titled “weight”](#weight)

**Type:** `number | string`\
**Default:** `undefined`

A [font weight](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight):

```js
weight: 200
```

If the associated font is a [variable font](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide), you can specify a range of weights:

```js
weight: "100 900"
```

When the value is not set, by default Astro will try to infer the value based on the first [`source`](#src).

##### `style`

[Section titled “style”](#style)

**Type:** `"normal" | "italic" | "oblique"`\
**Default:** `undefined`

A [font style](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style):

```js
style: "normal"
```

When the value is not set, by default Astro will try to infer the value based on the first [`source`](#src).

##### `src`

[Section titled “src”](#src)

**Type:** `(string | URL | { url: string | URL; tech?: string })[]`

Font [sources](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/src). It can be a path relative to the root, a package import or a URL. URLs are particularly useful if you inject local fonts through an integration:

* Relative path

  ```js
  src: ["./src/assets/fonts/MyFont.woff2", "./src/assets/fonts/MyFont.woff"]
  ```

* URL

  ```js
  src: [new URL("./custom.ttf", import.meta.url)]
  ```

* Package import

  ```js
  src: ["my-package/SomeFont.ttf"]
  ```

Caution

We recommend not putting your font files in [the `public/` directory](/en/reference/configuration-reference/#publicdir). Since Astro will copy these files into that folder at build time, this will result in duplicated files in your build output. Instead, store them somewhere else in your project, such as in [`src/`](/en/reference/configuration-reference/#srcdir).

You can also specify a [tech](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/src#tech) by providing objects:

```js
src: [{ url:"./src/assets/fonts/MyFont.woff2", tech: "color-COLRv1" }]
```

##### Other properties

[Section titled “Other properties”](#other-properties)

The following options from font families are also available for local font families within variants:

* [display](/en/reference/configuration-reference/#fontdisplay)
* [unicodeRange](/en/reference/configuration-reference/#fontunicoderange)
* [stretch](/en/reference/configuration-reference/#fontstretch)
* [featureSettings](/en/reference/configuration-reference/#fontfeaturesettings)
* [variationSettings](/en/reference/configuration-reference/#fontvariationsettings)

astro.config.mjs

```js
import { defineConfig, fontProviders } from "astro/config";


export default defineConfig({
  fonts: [{
    provider: fontProviders.local(),
    name: "Custom",
    cssVariable: "--font-custom",
    options: {
      variants: [
        {
          weight: 400,
          style: "normal",
          src: ["./src/assets/fonts/custom-400.woff2"],
          display: "block"
        }
      ]
    }
  }]
});
```

### NPM

[Section titled “NPM”](#npm)

Retrieves fonts from NPM packages, either from locally installed packages in `node_modules` or from a CDN:

```js
provider: fontProviders.npm()
```

The provider automatically detects fonts from your `package.json` dependencies and can resolve fonts from packages like `@fontsource/*`, `@fontsource-variable/*`, and other known font packages.

#### Provider options

[Section titled “Provider options”](#provider-options)

The NPM provider accepts the following configuration options:

##### `cdn`

[Section titled “cdn”](#cdn)

**Type:** `string`\
**Default:** `'https://cdn.jsdelivr.net/npm'`

CDN to use for fetching npm packages remotely:

```js
provider: fontProviders.npm({ cdn: 'https://esm.sh' })
```

##### `remote`

[Section titled “remote”](#remote)

**Type:** `boolean`\
**Default:** `true`

Whether to fall back to fetching from the CDN when local resolution fails. Set to `false` to only resolve from locally installed packages:

```js
provider: fontProviders.npm({ remote: false })
```

#### Family options

[Section titled “Family options”](#family-options)

The provider comes with the following family-specific options that can be added in the [`font.options` object](/en/reference/configuration-reference/#fontoptions).

##### `package`

[Section titled “package”](#package)

**Type:** `string`\
**Default:** Auto-detected or inferred from family name

The NPM package name. When not specified, the provider will try to find the font family in known font package patterns or infer based on Fontsource conventions:

```js
{
  // ...
  provider: fontProviders.npm(),
  options: {
    package: '@fontsource/roboto'
  }
}
```

##### `version`

[Section titled “version”](#version)

**Type:** `string`\
**Default:** `'latest'`

The version of the package (used for CDN resolution only):

```js
{
  // ...
  provider: fontProviders.npm(),
  options: {
    version: '5.0.0'
  }
}
```

##### `file`

[Section titled “file”](#file)

**Type:** `string`\
**Default:** `'index.css'`

The entry CSS file to parse from the package:

```js
{
  // ...
  provider: fontProviders.npm(),
  options: {
    file: 'latin.css'
  }
}
```

## Building a font provider

[Section titled “Building a font provider”](#building-a-font-provider)

If you do not wish to use one of the [built-in providers](#built-in-providers) (e.g. you want to use a [3rd-party unifont provider](#supporting-a-3rd-party-unifont-provider) or [build something for a private registry](#supporting-a-private-registry)), you can build your own.

The preferred method for implementing a custom font provider is to export a function that returns [the `FontProvider` object](#the-font-provider-object) and takes the [configuration](#config) as a parameter.

### The font provider object

[Section titled “The font provider object”](#the-font-provider-object)

A `FontProvider` is an object containing required [`name`](#name) and [`resolveFont()`](#resolvefont) properties. It also has optional [`config`](#config), [`init()`](#init) and [`listFonts()`](#listfonts) properties available.

The `FontProvider` type accepts a generic for family [options](/en/reference/configuration-reference/#fontoptions).

#### `name`

[Section titled “name”](#name)

**Type:** `string`

A unique name for the provider, used in logs and for identification.

#### `resolveFont()`

[Section titled “resolveFont()”](#resolvefont)

**Type:** `(options: ResolveFontOptions) => Awaitable<{ fonts: FontFaceData[] } | undefined>`

Used to retrieve and return font face data based on the given options.

#### `config`

[Section titled “config”](#config)

**Type:** `Record<string, any>`\
**Default:** `undefined`

A serializable object, used for identification.

#### `init()`

[Section titled “init()”](#init)

**Type:** `(context: FontProviderInitContext) => Awaitable<void>`\
**Default:** `undefined`

Optional callback, used to perform any initialization logic.

##### `context.storage`

[Section titled “context.storage”](#contextstorage)

**Type:** `Storage`

Useful for caching.

##### `context.root`

[Section titled “context.root”](#contextroot)

**Type:** `URL`

The project root, useful for resolving local files paths.

#### `listFonts()`

[Section titled “listFonts()”](#listfonts)

**Type:** `() => Awaitable<string[] | undefined>`\
**Default:** `undefined`

Optional callback, used to return the list of available font names.

### Supporting a private registry

[Section titled “Supporting a private registry”](#supporting-a-private-registry)

The following example defines a font provider for a private registry:

* Simple

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import { retrieveFonts, type Fonts } from "./utils.js",


  export function registryFontProvider(): FontProvider {
    let data: Fonts = {}


    return {
      name: "registry",
      init: async () => {
        data = await retrieveFonts(token);
      },
      listFonts: () => {
        return Object.keys(data);
      },
      resolveFont: ({ familyName, ...rest }) => {
        const fonts = data[familyName];
        if (fonts) {
          return { fonts };
        }
        return undefined;
      },
    };
  }
  ```

* Provider options

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import { retrieveFonts, type Fonts } from "./utils.js",


  interface Config {
    token: string;
  }


  export function registryFontProvider(config: Config): FontProvider {
    let data: Fonts = {}


    return {
      name: "registry",
      config,
      init: async () => {
        data = await retrieveFonts(token);
      },
      listFonts: () => {
        return Object.keys(data);
      },
      resolveFont: ({ familyName, ...rest }) => {
        const fonts = data[familyName];
        if (fonts) {
          return { fonts };
        }
        return undefined;
      },
    };
  }
  ```

* Family options

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import { retrieveFonts, type Fonts } from "./utils.js",


  interface FamilyOptions {
    minimal?: boolean;
  }


  export function registryFontProvider(): FontProvider<FamilyOptions | undefined> {
    let data: Fonts = {}


    return {
      name: "registry",
      init: async () => {
        data = await retrieveFonts(token);
      },
      listFonts: () => {
        return Object.keys(data);
      },
      // options is typed as FamilyOptions | undefined
      resolveFont: ({ familyName, options, ...rest }) => {
        const fonts = data[familyName];
        if (fonts) {
          return { fonts };
        }
        return undefined;
      },
    };
  }
  ```

You can then register this font provider in the Astro config:

* Simple

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { registryFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: registryFontProvider(),
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

* Provider options

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { registryFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: registryFontProvider({
        token: "..."
      }),
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

* Family options

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { registryFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: registryFontProvider(),
      options: {
        minimal: true
      },
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

### Supporting a 3rd-party unifont provider

[Section titled “Supporting a 3rd-party unifont provider”](#supporting-a-3rd-party-unifont-provider)

You can define an Astro font provider using a unifont provider under the hood:

* Simple

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import type { InitializedProvider } from "unifont";
  import { acmeProvider } from "@acme/unifont-provider"


  export function acmeFontProvider(): FontProvider {
    const provider = acmeProvider();
    let initializedProvider: InitializedProvider | undefined;
    return {
      name: provider._name,
      async init(context) {
        initializedProvider = await provider(context);
      },
      async resolveFont({ familyName, ...rest }) {
        return await initializedProvider?.resolveFont(familyName, rest);
      },
      async listFonts() {
        return await initializedProvider?.listFonts?.();
      },
    };
  }
  ```

* Provider options

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import type { InitializedProvider } from "unifont";
  import { acmeProvider, type AcmeOptions } from "@acme/unifont-provider"


  export function acmeFontProvider(config?: AcmeOptions): FontProvider {
    const provider = acmeProvider(config);
    let initializedProvider: InitializedProvider | undefined;
    return {
      name: provider._name,
      config,
      async init(context) {
        initializedProvider = await provider(context);
      },
      async resolveFont({ familyName, ...rest }) {
        return await initializedProvider?.resolveFont(familyName, rest);
      },
      async listFonts() {
        return await initializedProvider?.listFonts?.();
      },
    };
  }
  ```

* Family options

  font-provider.ts

  ```ts
  import type { FontProvider } from "astro";
  import type { InitializedProvider } from "unifont";
  import { acmeProvider, type AcmeFamilyOptions } from "@acme/unifont-provider"


  export function acmeFontProvider(): FontProvider<AcmeFamilyOptions | undefined> {
    const provider = acmeProvider();
    let initializedProvider: InitializedProvider<AcmeFamilyOptions> | undefined;
    return {
      name: provider._name,
      async init(context) {
        initializedProvider = await provider(context);
      },
      async resolveFont({ familyName, ...rest }) {
        return await initializedProvider?.resolveFont(familyName, rest);
      },
      async listFonts() {
        return await initializedProvider?.listFonts?.();
      },
    };
  }
  ```

You can then register this font provider in the Astro config:

* Simple

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { acmeFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: acmeFontProvider(),
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

* Provider options

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { acmeFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: acmeFontProvider({
        token: "..."
      }),
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

* Family options

  astro.config.ts

  ```ts
  import { defineConfig } from "astro/config";
  import { acmeFontProvider } from "./font-provider";


  export default defineConfig({
    fonts: [{
      provider: acmeFontProvider(),
      options: {
        minimal: true
      },
      name: "Custom",
      cssVariable: "--font-custom"
    }]
  });
  ```

# Image Service API

`astro:assets` was designed to make it easy for any image optimization service to build a service on top of Astro.

## What is an Image Service?

[Section titled “What is an Image Service?”](#what-is-an-image-service)

Astro provides two types of image services: Local and External.

* **Local services** handle image transformations directly at build for static sites, or at runtime both in development mode and for on-demand rendering. These are often wrappers around libraries like Sharp, ImageMagick, or Squoosh. In dev mode and in production routes rendered on demand, local services use an API endpoint to do the transformation.
* **External services** point to URLs and can add support for services such as Cloudinary, Vercel, or any [RIAPI](https://github.com/riapi/riapi)-compliant server.

## Building using the Image Services API

[Section titled “Building using the Image Services API”](#building-using-the-image-services-api)

Service definitions take the shape of an exported default object with various required methods (“hooks”).

External services provide a `getURL()` that points to the `src` of the output `<img>` tag.

Local services provide a `transform()` method to perform transformations on your image, and `getURL()` and `parseURL()` methods to use an endpoint for dev mode and when rendered on demand.

Both types of services can provide `getHTMLAttributes()` to determine the other attributes of the output `<img>` and `validateOptions()` to validate and augment the passed options.

### External Services

[Section titled “External Services”](#external-services)

An external service points to a remote URL to be used as the `src` attribute of the final `<img>` tag. This remote URL is responsible for downloading, transforming, and returning the image.

```ts
import type { ExternalImageService, ImageTransform, AstroConfig } from "astro";


const service: ExternalImageService = {
  validateOptions(options: ImageTransform, imageConfig: AstroConfig['image']) {
    const serviceConfig = imageConfig.service.config;


    // Enforce the user set max width.
    if (options.width && options.width > serviceConfig.maxWidth) {
      console.warn(`Image width ${options.width} exceeds max width ${serviceConfig.maxWidth}. Falling back to max width.`);
      options.width = serviceConfig.maxWidth;
    }


    return options;
  },
  getURL(options, imageConfig) {
    return `https://mysupercdn.com/${options.src}?q=${options.quality}&w=${options.width}&h=${options.height}`;
  },
  getHTMLAttributes(options, imageConfig) {
    const { src, format, quality, ...attributes } = options;
    return {
      ...attributes,
      loading: options.loading ?? 'lazy',
      decoding: options.decoding ?? 'async',
    };
  }
};




export default service;
```

### Local Services

[Section titled “Local Services”](#local-services)

To create your own local service, you can point to the [built-in endpoint](https://github.com/withastro/astro/blob/main/packages/astro/src/assets/endpoint/generic.ts) (`/_image`), or you can additionally create your own endpoint that can call the service’s methods.

```ts
import type { ImageTransform, LocalImageService, AstroConfig } from "astro";


const service: LocalImageService<AstroConfig["image"]> = {
  getURL(options: ImageTransform, imageConfig) {
    const searchParams = new URLSearchParams();
    searchParams.append('href', typeof options.src === "string" ? options.src : options.src.src);
    options.width && searchParams.append('w', options.width.toString());
    options.height && searchParams.append('h', options.height.toString());
    options.quality && searchParams.append('q', options.quality.toString());
    options.format && searchParams.append('f', options.format);
    return `/my_custom_endpoint_that_transforms_images?${searchParams}`;
    // Or use the built-in endpoint, which will call your parseURL and transform functions:
    // return `/_image?${searchParams}`;
  },
  parseURL(url: URL, imageConfig) {
    const params = url.searchParams;
    return {
      src: params.get('href')!,
      width: params.has('w') ? parseInt(params.get('w')!) : undefined,
      height: params.has('h') ? parseInt(params.get('h')!) : undefined,
      format: params.get('f'),
      quality: params.get('q'),
    };
  },
  async transform(inputBuffer: Uint8Array, options: { src: string, [key: string]: any }, imageConfig) {
    const { buffer } = await mySuperLibraryThatEncodesImages(options);
    return {
      data: buffer,
      format: options.format,
    };
  },
  getHTMLAttributes(options, imageConfig) {
    let targetWidth = options.width;
    let targetHeight = options.height;
    if (typeof options.src === "object") {
      const aspectRatio = options.src.width / options.src.height;


      if (targetHeight && !targetWidth) {
        targetWidth = Math.round(targetHeight * aspectRatio);
      } else if (targetWidth && !targetHeight) {
        targetHeight = Math.round(targetWidth / aspectRatio);
      }
    }


    const { src, width, height, format, quality, ...attributes } = options;


    return {
      ...attributes,
      width: targetWidth,
      height: targetHeight,
      loading: attributes.loading ?? 'lazy',
      decoding: attributes.decoding ?? 'async',
    };
  },
  propertiesToHash: ['src', 'width', 'height', 'format', 'quality'],
};
export default service;
```

At build time for static sites and pre-rendered routes, both `<Image />` and `getImage(options)` call the `transform()` function. They pass options either through component attributes or an `options` argument, respectively. The transformed images will be built to a `dist/_astro` folder. Their file names will contain a hash of the properties passed to `propertiesToHash`. This property is optional and will default to `['src', 'width', 'height', 'format', 'quality']`. If your custom image service has more options that change the generated images, add these to the array.

In dev mode and when using an adapter to render on demand, Astro doesn’t know ahead of time which images need to be optimized. Astro uses a GET endpoint (by default, `/_image`) to process the images at runtime. `<Image />` and `getImage()` pass their options to `getURL()`, which will return the endpoint URL. Then, the endpoint calls `parseURL()` and passes the resulting properties to `transform()`.

#### getConfiguredImageService & imageConfig

[Section titled “getConfiguredImageService & imageConfig”](#getconfiguredimageservice--imageconfig)

If you implement your own endpoint as an Astro endpoint, you can use [`getConfiguredImageService`](/en/reference/modules/astro-assets/#getconfiguredimageservice) and [`imageConfig`](/en/reference/modules/astro-assets/#imageconfig) to call your service’s `parseURL` and `transform` methods and provide the image config.

To access the image service config ([`image.service.config`](/en/reference/configuration-reference/#imageservice)), you can use `imageConfig.service.config`.

src/api/my\_custom\_endpoint\_that\_transforms\_images.ts

```ts
import type { APIRoute } from "astro";
import { getConfiguredImageService, imageConfig } from 'astro:assets';


export const GET: APIRoute = async ({ request }) => {
  const imageService = await getConfiguredImageService();


  const imageTransform = imageService.parseURL(new URL(request.url), imageConfig);
  // ... fetch the image from imageTransform.src and store it in inputBuffer
  const { data, format } = await imageService.transform(inputBuffer, imageTransform, imageConfig);
  return new Response(data, {
      status: 200,
      headers: {
        'Content-Type': mime.getType(format) || ''
      }
    }
  );
}
```

[See the built-in endpoint](https://github.com/withastro/astro/blob/main/packages/astro/src/assets/endpoint/generic.ts) for a full example.

## Hooks

[Section titled “Hooks”](#hooks)

### `getURL()`

[Section titled “getURL()”](#geturl)

**Type:** `(options: ImageTransform, imageConfig: AstroConfig[‘image’]) => string | Promise<string>`

**Added in:** `astro@2.1.0`

**Required for local and external services**

For local services, this hook returns the URL of the endpoint that generates your image (for on-demand rendering and in dev mode). It is unused during build. The local endpoint that `getURL()` points to may call both `parseURL()` and `transform()`.

For external services, this hook returns the final URL of the image.

For both types of services, `options` are the properties passed by the user as attributes of the `<Image />` component or as options to `getImage()`.

### `parseURL()`

[Section titled “parseURL()”](#parseurl)

**Type:** `(url: URL, imageConfig: AstroConfig[‘image’]) => { src: string, [key: string]: any } | undefined | Promise<{ src: string, [key: string]: any }> | Promise<undefined>`

**Added in:** `astro@2.1.0`

**Required for local services only; unavailable for external services**

This hook parses the generated URLs by `getURL()` back into an object with the different properties to be used by `transform` (for on-demand rendering and in dev mode). It is unused during build.

### `transform()`

[Section titled “transform()”](#transform)

**Type:** `(inputBuffer: Uint8Array, options: { src: string, [key: string]: any }, imageConfig: AstroConfig[‘image’]) => Promise<{ data: Uint8Array; format: ImageOutputFormat }>`

**Added in:** `astro@2.1.0`

**Required for local services only; unavailable for external services**

This hook transforms and returns the image and is called during the build to create the final asset files.

You must return a `format` to ensure that the proper MIME type is served to users for on-demand rendering and development mode.

### `getHTMLAttributes()`

[Section titled “getHTMLAttributes()”](#gethtmlattributes)

**Type:** `(options: ImageTransform, imageConfig: AstroConfig[‘image’] ) => Record<string, any> | Promise<Record<string, any>>`

**Added in:** `astro@2.1.0`

**Optional for both local and external services**

This hook returns all additional attributes used to render the image as HTML, based on the parameters passed by the user (`options`).

### `getSrcSet()`

[Section titled “getSrcSet()”](#getsrcset)

**Type:** `(options: ImageTransform, imageConfig: AstroConfig[‘image’] ) => UnresolvedSrcSetValue[] | Promise<UnresolvedSrcSetValue[]>`

**Added in:** `astro@3.3.0`

**Optional for both local and external services.**

This hook generates multiple variants of the specified image, for example, to generate a `srcset` attribute on an `<img>` or `<picture>`’s `source`.

This hook returns an array of objects with the following properties:

```ts
export type UnresolvedSrcSetValue = {
  transform: ImageTransform;
  descriptor?: string;
  attributes?: Record<string, any>;
};
```

### `validateOptions()`

[Section titled “validateOptions()”](#validateoptions)

**Type:** `(options: ImageTransform, imageConfig: AstroConfig[‘image’] ) => ImageTransform | Promise<ImageTransform>`

**Added in:** `astro@2.1.4`

**Optional for both local and external services**

This hook allows you to validate and augment the options passed by the user. This is useful for setting default options, or telling the user that a parameter is required.

[See how `validateOptions()` is used in Astro built-in services](https://github.com/withastro/astro/blob/0ab6bad7dffd413c975ab00e545f8bc150f6a92f/packages/astro/src/assets/services/service.ts#L124).

### `getRemoteSize()`

[Section titled “getRemoteSize()”](#getremotesize)

**Type:** `(url: string, imageConfig: AstroConfig[‘image’] ) => Omit<ImageMetadata, ‘src’ | ‘fsPath’> | Promise<Omit<ImageMetadata, ‘src’ | ‘fsPath’>>`

**Added in:** `astro@6.0.0`

**Optional for both local and external services**

This hook allows you to extend the behavior of [`inferRemoteSize()`](/en/reference/modules/astro-assets/#inferremotesize). This is useful for reducing network traffic by caching images, or when you can predict image information from the image URL.

## User configuration

[Section titled “User configuration”](#user-configuration)

Configure the image service to use in `astro.config.mjs`. The config takes the following form:

astro.config.mjs

```js
import { defineConfig } from "astro/config";


export default defineConfig({
  image: {
    service: {
      entrypoint: "your-entrypoint", // 'astro/assets/services/sharp' | string,
      config: {
        // ... service-specific config. Optional.
      }
    }
  },
});
```

## Typing custom image service props

[Section titled “Typing custom image service props”](#typing-custom-image-service-props)

**Added in:** `astro@5.16.6`

If your image service supports additional props in Astro’s `<Image>` component, `<Picture>` component, or the `getImage()` function, you can add types for these by extending the `Astro.CustomImageProps` interface.

For example, to add a custom `blur` prop that your image service supports:

```ts
declare namespace Astro {
  interface CustomImageProps {
    /** Apply a Gaussian blur with this radius to the image. */
    blur?: number;
  }
}
```

You can expose these types to users by making your image service an [Astro integration](/en/reference/integrations-reference/) and using the [`injectTypes()`](/en/reference/integrations-reference/#injecttypes-option) helper.

Then, users will be able to get autocomplete and type safety for your custom props:

```astro
<Image blur="yes" src={myPhoto}  />
//     ^^^^^^^^^^
//     Type 'string' is not assignable to type 'number | undefined'.
```


---

# Astro Integration API

**Astro Integrations** add new functionality and behaviors for your project with only a few lines of code.

This reference page is for anyone writing their own integration. To learn how to use an integration in your project, check out our [Using Integrations](/en/guides/integrations/) guide instead.

## Examples

[Section titled “Examples”](#examples)

The official Astro integrations can act as reference for you as you go to build your own integrations.

* **Renderers:** [`svelte`](/en/guides/integrations-guide/svelte/), [`react`](/en/guides/integrations-guide/react/), [`preact`](/en/guides/integrations-guide/preact/), [`vue`](/en/guides/integrations-guide/vue/), [`solid`](/en/guides/integrations-guide/solid-js/)
* **Libraries:** [`partytown`](/en/guides/integrations-guide/partytown/)
* **Features:** [`sitemap`](/en/guides/integrations-guide/sitemap/)

## Quick API Reference

[Section titled “Quick API Reference”](#quick-api-reference)

```ts
interface AstroIntegration {
  name: string;
  hooks: {
    'astro:config:setup'?: (options: {
      config: AstroConfig;
      command: 'dev' | 'build' | 'preview' | 'sync';
      isRestart: boolean;
      updateConfig: (newConfig: DeepPartial<AstroConfig>) => AstroConfig;
      addRenderer: (renderer: AstroRenderer) => void;
      addWatchFile: (path: URL | string) => void;
      addClientDirective: (directive: ClientDirectiveConfig) => void;
      addMiddleware: (middleware: AstroIntegrationMiddleware) => void;
      addDevToolbarApp: (entrypoint: DevToolbarAppEntry) => void;
      injectScript: (stage: InjectedScriptStage, content: string) => void;
      injectRoute: (injectedRoute: InjectedRoute) => void;
      createCodegenDir: () => URL;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:route:setup'?: (options: {
      route: RouteOptions;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:routes:resolved'?: (options: {
      routes: IntegrationResolvedRoute[];
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:config:done'?: (options: {
      config: AstroConfig;
      setAdapter: (adapter: AstroAdapter) => void;
      injectTypes: (injectedType: InjectedType) => URL;
      logger: AstroIntegrationLogger;
      buildOutput: 'static' | 'server';
    }) => void | Promise<void>;
    'astro:server:setup'?: (options: {
      server: vite.ViteDevServer;
      logger: AstroIntegrationLogger;
      toolbar: ReturnType<typeof getToolbarServerCommunicationHelpers>;
      refreshContent?: (options: RefreshContentOptions) => Promise<void>;
    }) => void | Promise<void>;
    'astro:server:start'?: (options: {
      address: AddressInfo;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:server:done'?: (options: {
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:build:start'?: (options: {
      logger: AstroIntegrationLogger;
      setPrerenderer: (prerenderer: AstroPrerenderer | ((defaultPrerenderer: AstroPrerenderer) => AstroPrerenderer)) => void;
    }) => void | Promise<void>;
    'astro:build:setup'?: (options: {
      vite: vite.InlineConfig;
      pages: Map<string, PageBuildData>;
      updateConfig: (newConfig: vite.InlineConfig) => void;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:build:ssr'?: (options: {
      manifest: SerializedSSRManifest;
      middlewareEntryPoint: URL | undefined;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:build:generated'?: (options: {
      dir: URL;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;
    'astro:build:done'?: (options: {
      pages: { pathname: string }[];
      dir: URL;
      assets: Map<string, URL[]>;
      logger: AstroIntegrationLogger;
    }) => void | Promise<void>;


    // ... any custom hooks from integrations
  };
}
```

## Hooks

[Section titled “Hooks”](#hooks)

Astro provides hooks that integrations can implement to execute during certain parts of Astro’s lifecycle. Astro hooks are defined in the `IntegrationHooks` interface, which is part of the global `Astro` namespace. Each hook has a [`logger` option](#astrointegrationlogger) that allows you to use the Astro logger to write logs.

The following hooks are built in to Astro:

### `astro:config:setup`

[Section titled “astro:config:setup”](#astroconfigsetup)

**Next hook:** [`astro:route:setup`](#astroroutesetup)

**When:** On initialization, before either the [Vite](https://vite.dev/config/) or [Astro config](/en/reference/configuration-reference/) have resolved.

**Why:** To extend the project config. This includes updating the [Astro config](/en/reference/configuration-reference/), applying [Vite plugins](https://vite.dev/guide/api-plugin.html), adding component renderers, and injecting scripts onto the page.

```ts
'astro:config:setup'?: (options: {
  config: AstroConfig;
  command: 'dev' | 'build' | 'preview' | 'sync';
  isRestart: boolean;
  updateConfig: (newConfig: DeepPartial<AstroConfig>) => AstroConfig;
  addRenderer: (renderer: AstroRenderer) => void;
  addClientDirective: (directive: ClientDirectiveConfig) => void;
  addMiddleware: (middleware: AstroIntegrationMiddleware) => void;
  addDevToolbarApp: (entrypoint: DevToolbarAppEntry) => void;
  addWatchFile: (path: URL | string) => void;
  injectScript: (stage: InjectedScriptStage, content: string) => void;
  injectRoute: (injectedRoute: InjectedRoute) => void;
  createCodegenDir: () => URL;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `config` option

[Section titled “config option”](#config-option)

**Type:** `AstroConfig`

A read-only copy of the user-supplied [Astro config](/en/reference/configuration-reference/). This is resolved *before* any other integrations have run. If you need a copy of the config after all integrations have completed their config updates, [see the `astro:config:done` hook](#astroconfigdone).

#### `command` option

[Section titled “command option”](#command-option)

**Type:** `'dev' | 'build' | 'preview' | 'sync'`

* `dev` - Project is executed with `astro dev`
* `build` - Project is executed with `astro build`
* `preview` - Project is executed with `astro preview`
* `sync` - Project is executed with `astro sync`

#### `isRestart` option

[Section titled “isRestart option”](#isrestart-option)

**Type:** `boolean`

**Added in:** `astro@1.5.0`

`false` when the dev server starts, `true` when a reload is triggered. Useful to detect when this function is called more than once.

#### `updateConfig()` option

[Section titled “updateConfig() option”](#updateconfig-option)

**Type:** `(newConfig: DeepPartial<AstroConfig>) => AstroConfig;`

A callback function to update the user-supplied [Astro config](/en/reference/configuration-reference/). Any config you provide **will be merged with the user config + other integration config updates,** so you are free to omit keys!

For example, say you need to supply a [Vite](https://vite.dev/) plugin to the user’s project:

```js
import bananaCSS from '@vitejs/official-banana-css-plugin';


export default {
  name: 'banana-css-integration',
  hooks: {
    'astro:config:setup': ({ updateConfig }) => {
      updateConfig({
        vite: {
          plugins: [bananaCSS()],
        }
      })
    }
  }
}
```

#### `addRenderer()` option

[Section titled “addRenderer() option”](#addrenderer-option)

**Type:** `(renderer: AstroRenderer) => void;`\
**Examples:** [`svelte`](https://github.com/withastro/astro/blob/main/packages/integrations/svelte/src/index.ts), [`react`](https://github.com/withastro/astro/blob/main/packages/integrations/react/src/index.ts), [`preact`](https://github.com/withastro/astro/blob/main/packages/integrations/preact/src/index.ts), [`vue`](https://github.com/withastro/astro/blob/main/packages/integrations/vue/src/index.ts), [`solid`](https://github.com/withastro/astro/blob/main/packages/integrations/solid/src/index.ts)

A callback function to add a component framework renderer (i.e. React, Vue, Svelte, etc). You can browse the examples and type definition above for more advanced options, but here are the 2 main options to be aware of:

* `clientEntrypoint` - path to a file that executes on the client whenever your component is used. This is mainly for rendering or hydrating your component with JS.
* `serverEntrypoint` - path to a file that executes during server-side requests or static builds whenever your component is used. These should render components to static markup, with hooks for hydration where applicable. [React’s `renderToString` callback](https://react.dev/reference/react-dom/server/renderToString) is a classic example.

**Added in:** `astro@5.0.0`

The functions `clientEntrypoint` and `serverEntrypoint` accept a `URL`.

#### `addWatchFile()` option

[Section titled “addWatchFile() option”](#addwatchfile-option)

**Type:** `(path: URL | string) => void`

**Added in:** `astro@1.5.0`

If your integration depends on some configuration file that Vite doesn’t watch and/or needs a full dev server restart to take effect, add it with `addWatchFile()`. Whenever that file changes, the Astro dev server will be reloaded (you can check when a reload happens with [`isRestart`](#isrestart-option)).

Example usage:

```js
// Must be an absolute path!
addWatchFile('/home/user/.../my-config.json');
addWatchFile(new URL('./ec.config.mjs', config.root));
```

#### `addClientDirective()` option

[Section titled “addClientDirective() option”](#addclientdirective-option)

**Type:** `(directive: ClientDirectiveConfig) => void;`

**Added in:** `astro@2.6.0`

Adds a [custom client directive](/en/reference/directives-reference/#custom-client-directives) to be used in `.astro` files.

Note that directive entrypoints are only bundled through esbuild and should be kept small so they don’t slow down component hydration.

Example usage:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';
import clickDirective from './astro-click-directive/register.js'


// https://astro.build/config
export default defineConfig({
  integrations: [
    clickDirective()
  ],
});
```

astro-click-directive/register.js

```js
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "client:click",
  hooks: {
    "astro:config:setup": ({ addClientDirective }) => {
      addClientDirective({
        name: "click",
        entrypoint: "./astro-click-directive/click.js",
      });
    },
  },
});
```

astro-click-directive/click.js

```js
/**
 * Hydrate on first click on the window
 * @type {import('astro').ClientDirective}
 */
export default (load, opts, el) => {
  window.addEventListener('click', async () => {
    const hydrate = await load()
    await hydrate()
  }, { once: true })
}
```

You can also add types for the directives in your library’s type definition file:

astro-click-directive/index.d.ts

```ts
import 'astro'
declare module 'astro' {
  interface AstroClientDirectives {
    'client:click'?: boolean
  }
}
```

#### `addDevToolbarApp()` option

[Section titled “addDevToolbarApp() option”](#adddevtoolbarapp-option)

**Type:** `(entrypoint: DevToolbarAppEntry) => void;`

**Added in:** `astro@3.4.0`

Adds a [custom dev toolbar app](/en/reference/dev-toolbar-app-reference/).

Example usage:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';
import devToolbarIntegration from './astro-dev-toolbar-app/integration.js'


// https://astro.build/config
export default defineConfig({
  integrations: [
    devToolbarIntegration()
  ],
});
```

astro-dev-toolbar-app/integration.js

```js
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "dev-toolbar-app",
  hooks: {
    "astro:config:setup": ({ addDevToolbarApp }) => {
      addDevToolbarApp({
        entrypoint: "./astro-dev-toolbar-app/plugin.js",
        id: "my-plugin",
        name: "My Plugin"
      });
    },
  },
});
```

astro-dev-toolbar-app/plugin.js

```js
/**
 * @type {import('astro').DevToolbarApp}
 */
export default {
  id: "my-plugin",
  name: "My Plugin",
  icon: "<svg>...</svg>",
  init() {
    console.log("I'm a dev toolbar app!")
  },
};
```

#### `addMiddleware()` option

[Section titled “addMiddleware() option”](#addmiddleware-option)

**Type:** `(middleware: AstroIntegrationMiddleware) => void;`

**Added in:** `astro@3.5.0`

Adds [middleware](/en/guides/middleware/) to run on each request. Takes the `entrypoint` module that contains the middleware, and an `order` to specify whether it should run before (`pre`) other middleware or after (`post`).

@my-package/integration.js

```js
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "my-middleware-package",
  hooks: {
    "astro:config:setup": ({ addMiddleware }) => {
      addMiddleware({
        entrypoint: '@my-package/middleware',
        order: 'pre'
      });
    },
  },
});
```

Middleware is defined in a package with an [`onRequest()` function](/en/reference/modules/astro-middleware/#onrequest), as with user-defined middleware.

@my-package/middleware.js

```js
import { defineMiddleware } from 'astro:middleware';


export const onRequest = defineMiddleware(async (context, next) => {
  if(context.url.pathname === '/some-test-path') {
    return Response.json({
      ok: true
    });
  }


  return next();
});
```

**Added in:** `astro@5.0.0`

The function also accepts a `URL` for `entrypoint`:

@my-package/integration.js

```diff
/**
 * @type {() => import('astro').AstroIntegration}
 */
export default () => ({
  name: "my-middleware-package",
  hooks: {
    "astro:config:setup": ({ addMiddleware }) => {
      addMiddleware({
+        entrypoint: new URL('./middleware.js', import.meta.url),
        order: 'pre'
      });
    },
  },
});
```

#### `injectRoute()` option

[Section titled “injectRoute() option”](#injectroute-option)

**Type:** `({ pattern: string; entrypoint: string | URL; prerender?: boolean }) => void;`

A callback function to inject routes into an Astro project. Injected routes can be [`.astro` pages](/en/basics/astro-pages/) or [`.js` and `.ts` route handlers](/en/guides/endpoints/#static-file-endpoints).

`injectRoute()` takes an object with a `pattern` and an `entrypoint`.

* `pattern` - where the route should be output in the browser, for example `/foo/bar`. A `pattern` can use Astro’s filepath syntax for denoting dynamic routes, for example `/foo/[bar]` or `/foo/[...bar]`. Note that a file extension is **not** needed in the `pattern`.
* `entrypoint` - a bare module specifier pointing towards the `.astro` page or `.js`/`.ts` route handler that handles the route denoted in the `pattern`.
* `prerender` - a boolean to set if Astro can’t detect your `prerender` export.

##### Example usage

[Section titled “Example usage”](#example-usage)

```js
injectRoute({
  // Use Astro’s pattern syntax for dynamic routes.
  pattern: '/subfolder/[dynamic]',
  // Use relative path syntax for a local route.
  entrypoint: './src/dynamic-page.astro',
  // Use only if Astro can't detect your prerender export
  prerender: false
});
```

For an integration designed to be installed in other projects, use its package name to refer to the route entrypoint. The following example shows a package published to npm as `@fancy/dashboard` injecting a dashboard route:

```js
injectRoute({
  pattern: '/fancy-dashboard',
  entrypoint: '@fancy/dashboard/dashboard.astro'
});
```

When publishing your package (`@fancy/dashboard`, in this case) to npm, you must export `dashboard.astro` in your `package.json`:

package.json

```json
{
  "name": "@fancy/dashboard",
  // ...
  "exports": { "./dashboard.astro": "./dashboard.astro" }
}
```

**Added in:** `astro@5.0.0`

The function also accepts a `URL` for `entrypoint`:

```js
injectRoute({
  pattern: '/fancy-dashboard',
  entrypoint: new URL('./dashboard.astro', import.meta.url)
});
```

#### `injectScript()` option

[Section titled “injectScript() option”](#injectscript-option)

**Type:** `(stage: InjectedScriptStage, content: string) => void;`

A callback function to inject a string of JavaScript content onto every page.

The **`stage`** denotes how this script (the `content`) should be inserted. Some stages allow inserting scripts without modification, while others allow optimization during [Vite’s bundling step](https://vite.dev/guide/build.html):

* `"head-inline"`: Injected into a script tag in the `<head>` of every page. **Not** optimized or resolved by Vite.

* `"before-hydration"`: Imported client-side, before the hydration script runs. Optimized and resolved by Vite.

* `"page"`: Similar to `head-inline`, except that the injected snippet is handled by Vite and bundled with any other `<script>` tags defined inside of Astro components on the page. The script will be loaded with a `<script type="module">` in the final page output, optimized and resolved by Vite.

* `"page-ssr"`: Imported as a separate module in the frontmatter of every Astro page component. Because this stage imports your script, the `Astro` global is not available and your script will only be run once when the `import` is first evaluated.

  The main use for the `page-ssr` stage is injecting a CSS `import` into every page to be optimized and resolved by Vite:

  ```js
  injectScript('page-ssr', 'import "global-styles.css";');
  ```

#### `createCodegenDir()`

[Section titled “createCodegenDir()”](#createcodegendir)

**Type:** `() => URL;`

**Added in:** `astro@5.0.0`

A function that creates the `<root>/.astro/integrations/<normalized_integration_name>` folder and returns its path.

It allows you to have a dedicated folder, avoiding conflicts with another integration or Astro itself. This directory is created by calling this function so it’s safe to write files to it directly:

my-integration.ts

```ts
import { writeFileSync } from 'node:fs'


const integration = {
  name: 'my-integration',
  hooks: {
    'astro:config:setup': ({ createCodegenDir }) => {
      const codegenDir = createCodegenDir()
      writeFileSync(new URL('cache.json', codegenDir), '{}', 'utf-8')
    }
  }
}
```

### `astro:route:setup`

[Section titled “astro:route:setup”](#astroroutesetup)

**Added in:** `astro@4.14.0`

**Previous hook:** [`astro:config:setup`](#astroconfigsetup)

**Next hook:** [`astro:routes:resolved`](#astroroutesresolved)

**When:** In `astro build`, before bundling starts. In `astro dev`, while building the module graph and on every change to a file based route (added/removed/updated).

**Why:** To set options for a route at build or request time, such as enabling [on-demand server rendering](/en/guides/on-demand-rendering/#enabling-on-demand-rendering).

```js
'astro:route:setup'?: (options: {
  route: RouteOptions;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `route` option

[Section titled “route option”](#route-option)

**Type:** `{ readonly component: string; prerender?: boolean; }`

An object with a `component` property to identify the route and the following additional values to allow you to configure the generated route: `prerender`.

##### `route.component`

[Section titled “route.component”](#routecomponent)

**Type:** `string`

**Added in:** `astro@4.14.0`

The `component` property indicates the entrypoint that will be rendered on the route. You can access this value before the routes are built to configure on-demand server rendering for that page.

##### `route.prerender`

[Section titled “route.prerender”](#routeprerender)

**Type:** `boolean`\
**Default:** `undefined`

**Added in:** `astro@4.14.0`

The `prerender` property is used to configure [on-demand server rendering](/en/guides/on-demand-rendering/#enabling-on-demand-rendering) for a route. If the route file contains an explicit `export const prerender` value, the value will be used as the default instead of `undefined`.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  integrations: [setPrerender()],
});


function setPrerender() {
  return {
    name: 'set-prerender',
    hooks: {
      'astro:route:setup': ({ route }) => {
        if (route.component.endsWith('/blog/[slug].astro')) {
          route.prerender = true;
        }
      },
    },
  };
}
```

If the final value after running all the hooks is `undefined`, the route will fall back to a prerender default based on the [`output` option](/en/reference/configuration-reference/#output): prerendered for `static` mode, and on-demand rendered for `server` mode.

### `astro:routes:resolved`

[Section titled “astro:routes:resolved”](#astroroutesresolved)

**Added in:** `astro@5.0.0`

**Previous hook:** [`astro:route:setup`](#astroroutesetup)

**Next hook:** [`astro:config:done`](#astroconfigdone) (only during setup)

**When:** In `astro dev`, it also runs on every change to a file based route (added/removed/updated).

**Why:** To access routes and their metadata

```js
'astro:routes:resolved'?: (options: {
  routes: IntegrationResolvedRoute[];
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `routes` option

[Section titled “routes option”](#routes-option)

**Type:** [`IntegrationResolvedRoute[]`](#integrationresolvedroute)

A list of all routes with their associated metadata.

Example use:

my-integration.mjs

```js
const integration = () => {
  return {
    name: 'my-integration',
    hooks: {
      'astro:routes:resolved': ({ routes }) => {
        const projectRoutes = routes.filter(r => r.origin === 'project').map(r => r.pattern)


        console.log(projectRoutes)
      },
    }
  }
}
```

### `astro:config:done`

[Section titled “astro:config:done”](#astroconfigdone)

**Previous hook:** [`astro:routes:resolved`](#astroroutesresolved)

**Next hook:** [`astro:server:setup`](#astroserversetup) when running in “dev” mode, or [`astro:build:start`](#astrobuildstart) during production builds

**When:** After the Astro config has resolved and other integrations have run their `astro:config:setup` hooks.

**Why:** To retrieve the final config for use in other hooks.

```js
'astro:config:done'?: (options: {
  config: AstroConfig;
  setAdapter: (adapter: AstroAdapter) => void;
  injectTypes: (injectedType: InjectedType) => URL;
  logger: AstroIntegrationLogger;
  buildOutput: 'static' | 'server';
}) => void | Promise<void>;
```

#### `config` option

[Section titled “config option”](#config-option-1)

**Type:** `AstroConfig`

A read-only copy of the user-supplied [Astro config](/en/reference/configuration-reference/). This is resolved *after* other integrations have run.

#### `setAdapter()` option

[Section titled “setAdapter() option”](#setadapter-option)

**Type:** `(adapter: AstroAdapter) => void;`

Makes the integration an adapter. Read more in the [adapter API](/en/reference/adapter-reference/).

#### `injectTypes()` option

[Section titled “injectTypes() option”](#injecttypes-option)

**Type:** `(injectedType: { filename: string; content: string }) => URL`

**Added in:** `astro@4.14.0`

Allows you to inject types into your user’s project by adding a new `*.d.ts` file.

The `filename` property will be used to generate a file at `/.astro/integrations/<normalized_integration_name>/<normalized_filename>.d.ts` and must end with `".d.ts"`.

The `content` property will create the body of the file and must be valid TypeScript.

Additionally, `injectTypes()` returns a URL to the normalized path so you can overwrite its content later on, or manipulate it in any way you want.

```js
const path = injectTypes({
  filename: "types.d.ts",
  content: "declare module 'virtual:integration' {}"
})
console.log(path) // URL
```

#### `buildOutput` option

[Section titled “buildOutput option”](#buildoutput-option)

**Type:** `'static' | 'server'`

**Added in:** `astro@5.0.0`

Allows you to adapt the logic of your integration depending on the user’s project output.

### `astro:server:setup`

[Section titled “astro:server:setup”](#astroserversetup)

**Previous hook:** [`astro:config:done`](#astroconfigdone)

**Next hook:** [`astro:server:start`](#astroserverstart)

**When:** Just after the Vite server is created in “dev” mode, but before the `listen()` event is fired. [See Vite’s createServer API](https://vite.dev/guide/api-javascript.html#createserver) for more.

**Why:** To update Vite server options and middleware, or enable support for refreshing the content layer.

```js
'astro:server:setup'?: (options: {
  server: vite.ViteDevServer;
  logger: AstroIntegrationLogger;
  toolbar: ReturnType<typeof getToolbarServerCommunicationHelpers>;
  refreshContent: (options: {
    loaders?: Array<string>;
    context?: Record<string, any>;
  }) => Promise<void>;
}) => void | Promise<void>;
```

#### `server` option

[Section titled “server option”](#server-option)

**Type:** [`ViteDevServer`](https://vite.dev/guide/api-javascript.html#vitedevserver)

A mutable instance of the Vite server used in “dev” mode. For instance, this is [used by our Partytown integration](/en/guides/integrations-guide/partytown/) to inject the Partytown server as middleware:

```js
export default {
  name: 'partytown',
  hooks: {
    'astro:server:setup': ({ server }) => {
      server.middlewares.use(
        function middleware(req, res, next) {
          // handle requests
        }
      );
    }
  }
}
```

#### `toolbar` option

[Section titled “toolbar option”](#toolbar-option)

**Type:** `ReturnType<typeof getToolbarServerCommunicationHelpers>`

**Added in:** `astro@4.7.0`

An object providing callback functions to interact with the [dev toolbar](/en/reference/dev-toolbar-app-reference/):

##### `toolbar.on()`

[Section titled “toolbar.on()”](#toolbaron)

**Type:** `<T>(event: string, callback: (data: T) => void) => void`

A function that takes an event name as first argument and a callback function as second argument. This allows you to receive a message from a dev toolbar app with data associated to that event.

##### `toolbar.onAppInitialized()`

[Section titled “toolbar.onAppInitialized()”](#toolbaronappinitialized)

**Type:** `(appId: string, callback: (data: Record<string, never>) => void) => void`

A function fired when a dev toolbar app is initialized. The first argument is the id of the app that was initialized. The second argument is a callback function to run when the app is initialized.

##### `toolbar.onAppToggled()`

[Section titled “toolbar.onAppToggled()”](#toolbaronapptoggled)

**Type:** `(appId: string, callback: (data: { state: boolean; }) => void) => void`

A function fired when a dev toolbar app is toggled on or off. The first argument is the id of the app that was toggled. The second argument is a callback function providing the state to execute when the application is toggled.

##### `toolbar.send()`

[Section titled “toolbar.send()”](#toolbarsend)

**Type:** `<T>(event: string, payload: T) => void`

A function that sends a message to the dev toolbar that an app can listen for. This takes an event name as the first argument and a payload as the second argument which can be any serializable data.

#### `refreshContent()` option

[Section titled “refreshContent() option”](#refreshcontent-option)

**Type:** `(options: { loaders?: Array<string>; context?: Record<string, any>; }) => Promise<void>`

**Added in:** `astro@5.0.0`

A function for integrations to trigger an update to the content layer during `astro dev`. This can be used, for example, to register a webhook endpoint during dev, or to open a socket to a CMS to listen for changes.

By default, `refreshContent()` will refresh all collections. You can optionally pass a `loaders` property, which is an array of loader names. If provided, only collections that use those loaders will be refreshed. For example, A CMS integration could use this property to only refresh its own collections.

You can also pass a `context` object to the loaders. This can be used to pass arbitrary data such as the webhook body, or an event from the websocket.

my-integration.ts

```ts
{
  name: 'my-integration',
  hooks: {
    'astro:server:setup': async ({ server, refreshContent }) => {
      // Register a dev server webhook endpoint
      server.middlewares.use('/_refresh', async (req, res) => {
        if(req.method !== 'POST') {
          res.statusCode = 405
          res.end('Method Not Allowed');
          return
        }
        let body = '';
        req.on('data', chunk => {
          body += chunk.toString();
        });
        req.on('end', async () => {
          try {
            const webhookBody = JSON.parse(body);
            await refreshContent({
              context: { webhookBody },
              loaders: ['my-loader']
            });
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Content refreshed successfully' }));
          } catch (error) {
            res.writeHead(500, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ error: 'Failed to refresh content: ' + error.message }));
          }
        });
      });
    }
  }
}
```

The loader can then access the `refreshContextData` property to get the webhook body. See the [`refreshContextData`](/en/reference/content-loader-reference/#loadercontextrefreshcontextdata) property for more information.

### `astro:server:start`

[Section titled “astro:server:start”](#astroserverstart)

**Previous hook:** [`astro:server:setup`](#astroserversetup)

**Next hook:** [`astro:server:done`](#astroserverdone)

**When:** Just after the server’s `listen()` event has fired.

**Why:** To intercept network requests at the specified address. If you intend to use this address for middleware, consider using `astro:server:setup` instead.

```js
'astro:server:start'?: (options: {
  address: AddressInfo;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `address` option

[Section titled “address option”](#address-option)

**Type:** `AddressInfo`

The address, family and port number supplied by the [`server.address()` method of the Node.js Net module](https://nodejs.org/api/net.html#serveraddress).

### `astro:server:done`

[Section titled “astro:server:done”](#astroserverdone)

**Previous hook:** [`astro:server:start`](#astroserverstart)

**When:** Just after the dev server is closed.

**Why:** To run any cleanup events you may trigger during the `astro:server:setup` or `astro:server:start` hooks.

```js
'astro:server:done'?: (options: {
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

### `astro:build:start`

[Section titled “astro:build:start”](#astrobuildstart)

**Previous hook:** [`astro:config:done`](#astroconfigdone)

**Next hook:** [`astro:build:setup`](#astrobuildsetup)

**When:** After the `astro:config:done` event, but before the production build begins.

**Why:** To set up any global objects or clients needed during a production build. This can also extend the build configuration options in the [adapter API](/en/reference/adapter-reference/).

```js
'astro:build:start'?: (options: {
  logger: AstroIntegrationLogger;
  setPrerenderer: (prerenderer: AstroPrerenderer | ((defaultPrerenderer: AstroPrerenderer) => AstroPrerenderer)) => void;
}) => void | Promise<void>;
```

#### `setPrerenderer()` option

[Section titled “setPrerenderer() option”](#setprerenderer-option)

**Type:** `(prerenderer: AstroPrerenderer | ((defaultPrerenderer: AstroPrerenderer) => AstroPrerenderer)) => void`

**Added in:** `astro@6.0.0`

A callback function to set a custom prerenderer for the build. This allows adapters to provide their own prerendering logic.

The function accepts either an [`AstroPrerenderer` object](#astroprerenderer) directly, or a factory function that receives the default prerenderer and returns a custom one. This is useful when you want to wrap or extend the default behavior.

```js
'astro:build:start': ({ setPrerenderer }) => {
  setPrerenderer((defaultPrerenderer) => ({
    name: 'my-prerenderer',
    async setup() {
      // Optional: called once before prerendering starts
    },
    async getStaticPaths() {
      // Returns array of { pathname: string, route: RouteData }
      return defaultPrerenderer.getStaticPaths();
    },
    async render(request, { routeData }) {
      // request: Request, options: { routeData: RouteData }
      // Returns: Response
    },
    async teardown() {
      // Optional: called after all pages are prerendered
    }
  }));
}
```

See the [adapter reference](/en/reference/adapter-reference/#custom-prerenderer) for more details on implementing a custom prerenderer.

### `astro:build:setup`

[Section titled “astro:build:setup”](#astrobuildsetup)

**Previous hook:** [`astro:build:start`](#astrobuildstart)

**Next hook:** [`astro:build:ssr`](#astrobuildssr)

**When:** After the `astro:build:start` hook, runs immediately before the build.

**Why:** At this point, the Vite config for the build has been completely constructed, this is your final chance to modify it. This can be useful for example to overwrite some defaults. If you’re not sure whether you should use this hook or `astro:build:start`, use `astro:build:start` instead.

```js
'astro:build:setup'?: (options: {
  vite: vite.InlineConfig;
  pages: Map<string, PageBuildData>;
  updateConfig: (newConfig: vite.InlineConfig) => void;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `vite` option

[Section titled “vite option”](#vite-option)

**Type:** [`InlineConfig`](https://vite.dev/guide/api-javascript.html#inlineconfig)

An object that allows you to access the Vite configuration used in the build.

This can be useful if you need to access configuration options in your integration:

```js
export default {
  name: 'my-integration',
  hooks: {
    'astro:build:setup': ({ vite }) => {
      const { publicDir, root } = vite;
    },
  }
}
```

#### `pages` option

[Section titled “pages option”](#pages-option)

**Type:** `Map<string, PageBuildData>`

A `Map` with a list of pages as key and [their build data](#the-pagebuilddata-object) as value.

This can be used to perform an action if a route matches a criteria:

```js
export default {
  name: 'my-integration',
  hooks: {
    'astro:build:setup': ({ pages }) => {
      pages.forEach((data) => {
        if (data.route.pattern.test("/blog")) {
          console.log(data.route.type);
        }
      });
    },
  }
}
```

##### The `PageBuildData` object

[Section titled “The PageBuildData object”](#the-pagebuilddata-object)

Describes how to build a page.

###### `PageBuildData.key`

[Section titled “PageBuildData.key”](#pagebuilddatakey)

**Type:** `string`

**Added in:** `astro@4.8.0`

Specifies a unique identifier for the page.

###### `PageBuildData.component`

[Section titled “PageBuildData.component”](#pagebuilddatacomponent)

**Type:** `string`

Specifies the source component URL.

###### `PageBuildData.route`

[Section titled “PageBuildData.route”](#pagebuilddataroute)

**Type:** [`RouteData`](#routedata)

Describes the information about the page route.

###### `PageBuildData.moduleSpecifier`

[Section titled “PageBuildData.moduleSpecifier”](#pagebuilddatamodulespecifier)

**Type:** `string`

Defines a string that can be resolved into a file path for the module.

###### `PageBuildData.styles`

[Section titled “PageBuildData.styles”](#pagebuilddatastyles)

**Type:** `Array<{ depth: number; order: number; sheet: { type: 'inline'; content: string } | { type: 'external'; src: string } }>`

**Added in:** `astro@2.4.0`

A list of styles to render on the page. Each style contains its `depth` in the components tree and its display `order` on the page, as well as an indication of whether this should be applied as an inline or external style.

#### `updateConfig()` option

[Section titled “updateConfig() option”](#updateconfig-option-1)

**Type:** `(newConfig: InlineConfig) => void`

A callback function to update the [Vite](https://vite.dev/) options used in the build. Any config you provide **will be merged with the user config + other integration config updates**, so you are free to omit keys!

For example, this can be used to supply a plugin to the user’s project:

```js
import awesomeCssPlugin from 'awesome-css-vite-plugin';


export default {
  name: 'my-integration',
  hooks: {
    'astro:build:setup': ({ updateConfig }) => {
      updateConfig({
        plugins: [awesomeCssPlugin()],
      })
    }
  }
}
```

### `astro:build:ssr`

[Section titled “astro:build:ssr”](#astrobuildssr)

**Previous hook:** [`astro:build:setup`](#astrobuildsetup)

**Next hook:** [`astro:build:generated`](#astrobuildgenerated)

**When:** After a production SSR build has completed.

**Why:** To access the SSR manifest and map of the emitted entry points. This is useful when creating custom SSR builds in plugins or integrations.

* `middlewareEntryPoint` is the file system path of the middleware file;

```js
'astro:build:ssr'?: (options: {
  manifest: SerializedSSRManifest;
  middlewareEntryPoint: URL | undefined;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `manifest` option

[Section titled “manifest option”](#manifest-option)

**Type:** `SerializedSSRManifest`

Allows you to create a custom build by accessing a serialized version of the [`SSRManifest`](#ssrmanifest). This contains the same information as `SSRManifest`, with some properties converted to serializable formats.

The following example checks the [`i18n.strategy`](#ssrmanifesti18nstrategy) configuration stored in the `manifest`:

```js
export default {
  name: 'my-integration',
  hooks: {
    'astro:build:ssr': ({ manifest }) => {
      const { i18n } = manifest;
      if (i18n?.strategy === "domains-prefix-always") {
        // do something
      }
    },
  },
}
```

##### `manifest.rootDir`

[Section titled “manifest.rootDir”](#manifestrootdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.rootDir`](#ssrmanifestrootdir).

##### `manifest.srcDir`

[Section titled “manifest.srcDir”](#manifestsrcdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.srcDir`](#ssrmanifestsrcdir).

##### `manifest.cacheDir`

[Section titled “manifest.cacheDir”](#manifestcachedir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.cacheDir`](#ssrmanifestcachedir).

##### `manifest.outDir`

[Section titled “manifest.outDir”](#manifestoutdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.outDir`](#ssrmanifestoutdir).

##### `manifest.publicDir`

[Section titled “manifest.publicDir”](#manifestpublicdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.publicDir`](#ssrmanifestpublicdir).

##### `manifest.buildClientDir`

[Section titled “manifest.buildClientDir”](#manifestbuildclientdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.buildClientDir`](#ssrmanifestbuildclientdir).

##### `manifest.buildServerDir`

[Section titled “manifest.buildServerDir”](#manifestbuildserverdir)

**Type:** `string`

Specifies a serialized version of the [`SSRManifest.buildServerDir`](#ssrmanifestbuildserverdir).

##### `manifest.routes`

[Section titled “manifest.routes”](#manifestroutes)

**Type:** `SerializedRouteInfo[]`

Defines a list of serialized route information. Each route contains the same properties as [`SSRManifest.routes`](#ssrmanifestroutes), with `routeData` converted to a JSON-serializable format.

##### `manifest.assets`

[Section titled “manifest.assets”](#manifestassets)

**Type:** `string[]`

Defines a list of serialized asset file paths.

##### `manifest.componentMetadata`

[Section titled “manifest.componentMetadata”](#manifestcomponentmetadata)

**Type:** `[string, SSRComponentMetadata][]`

**Added in:** `astro@2.1.7`

Defines an array of key-value pairs where the first element is the component identifier and the second is an object describing the build metadata.

##### `manifest.inlinedScripts`

[Section titled “manifest.inlinedScripts”](#manifestinlinedscripts)

**Type:** `[string, string][]`

Defines an array of key-value pairs where each entry is a tuple. The first element is the script identifier and the second is the script content.

##### `manifest.clientDirectives`

[Section titled “manifest.clientDirectives”](#manifestclientdirectives)

**Type:** `[string, string][]`

**Added in:** `astro@2.5.0`

Defines an array of key-value pairs where the first element is the directive name (e.g. `load`, `visible`) and the second is the directive’s implementation code.

##### `manifest.key`

[Section titled “manifest.key”](#manifestkey)

**Type:** `string`

**Added in:** `astro@4.13.4`

Specifies the cryptographic key, serialized as a string, used for encrypting server island props.

#### `middlewareEntryPoint` option

[Section titled “middlewareEntryPoint option”](#middlewareentrypoint-option)

**Type:** `URL | undefined`

**Added in:** `astro@2.8.0`

Exposes the [middleware](/en/guides/middleware/) file path.

```js
export default {
  name: 'my-integration',
  hooks: {
    'astro:build:ssr': ({ middlewareEntryPoint }) => {
      if (middlewareEntryPoint) {
        // do some operations if a middleware exist
      }
    },
  },
}
```

### `astro:build:generated`

[Section titled “astro:build:generated”](#astrobuildgenerated)

**Added in:** `astro@1.3.0`

**Previous hook:** [`astro:build:ssr`](#astrobuildssr)

**Next hook:** [`astro:build:done`](#astrobuilddone)

**When:** After a static production build has finished generating routes and assets.

**Why:** To access generated routes and assets **before** build artifacts are cleaned up. This is a very uncommon use case. We recommend using [`astro:build:done`](#astrobuilddone) unless you really need to access the generated files before cleanup.

```js
'astro:build:generated'?: (options: {
  dir: URL;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `dir` option

[Section titled “dir option”](#dir-option)

**Type:** [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL)

A URL path to the build output directory.

The following example uses Node’s built-in [`fileURLToPath()`](https://nodejs.org/api/url.html#urlfileurltopathurl-options) utility to compute a valid absolute path string for a file provided by the integration:

```js
import { fileURLToPath } from 'node:url';


export default {
  name: 'my-integration',
  hooks: {
    'astro:build:generated': ({ dir }) => {
      const outFile = fileURLToPath(new URL('./my-integration.json', dir));
    }
  }
}
```

### `astro:build:done`

[Section titled “astro:build:done”](#astrobuilddone)

**Previous hook:** [`astro:build:generated`](#astrobuildgenerated)

**When:** After a production build (SSG or SSR) has completed.

**Why:** To access generated routes and assets for extension (ex. copy content into the generated `/assets` directory). If you plan to transform generated assets, we recommend exploring the [Vite Plugin API](https://vite.dev/guide/api-plugin.html) and [configuring via `astro:config:setup`](#updateconfig-option) instead.

```js
'astro:build:done'?: (options: {
  pages: { pathname: string }[];
  dir: URL;
  assets: Map<string, URL[]>;
  logger: AstroIntegrationLogger;
}) => void | Promise<void>;
```

#### `dir` option

[Section titled “dir option”](#dir-option-1)

**Type:** [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL)

A URL path to the build output directory.

The following example uses Node’s built-in [`fileURLToPath()`](https://nodejs.org/api/url.html#urlfileurltopathurl-options) utility to compute a valid absolute path string for a file provided by the integration before writing to it:

```js
import { writeFile } from 'node:fs/promises';
import { fileURLToPath } from 'node:url';


export default function myIntegration() {
  return {
    hooks: {
      'astro:build:done': async ({ dir }) => {
        const metadata = await getIntegrationMetadata();
        // Use fileURLToPath to get a valid, cross-platform absolute path string
        const outFile = fileURLToPath(new URL('./my-integration.json', dir));
        await writeFile(outFile, JSON.stringify(metadata));
      }
    }
  }
}
```

#### `assets` option

[Section titled “assets option”](#assets-option)

**Type:** `Map<string, URL[]>`

**Added in:** `astro@5.0.0`

Contains URLs to output files paths, grouped by [`IntegrationResolvedRoute`](#integrationresolvedroute) `pattern` property.

#### `pages` option

[Section titled “pages option”](#pages-option-1)

**Type:** `{ pathname: string }[]`

A list of all generated pages. Each entry is an object with one property:

* `pathname` - the finalized path of the page.

### Custom hooks

[Section titled “Custom hooks”](#custom-hooks)

Custom hooks can be added to integrations by extending the `IntegrationHooks` interface through [global augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#global-augmentation).

```ts
declare global {
  namespace Astro {
    export interface IntegrationHook {
      'your:hook': (params: YourHookParameters) => Promise<void>
    }
  }
}
```

Astro reserves the `astro:` prefix for future built-in hooks. Please choose a different prefix when naming your custom hook.

## Astro vite environments

[Section titled “Astro vite environments”](#astro-vite-environments)

Astro inherits the environments Vite provides by default, `ssr` and `client`.

Additionally there are two other environments that Astro creates:

* `prerender` is an environment used during the `build` and it’s used to build static pages.
* `astro` is an environment used during the development, and it’s used as a “secondary” SSR environment when the Vite `ssr` environment [isn’t a runnable dev environment](https://vite.dev/guide/api-environment-frameworks#runnabledevenvironment).

Astro’s [Vite environments](https://vite.dev/guide/api-environment) allow you to optimize your integration’s Vite plugins for different environments. One of the main uses of Vite environments is the ability to run and configure your integration’s Vite plugins conditionally:

```js
resolveId(id) {
  if (id === '\0virtual:foo') {
    if (this.environment.name === 'client') {
      throw new Error('This is a server-only module');
    }
    return 'export const foo = "bar"';
  }
}
```

## Integration types reference

[Section titled “Integration types reference”](#integration-types-reference)

The following types can be imported from the `astro` module:

```ts
import type {
  AstroIntegrationLogger,
  AstroIntegrationMiddleware,
  AstroMiddlewareInstance,
  AstroPrerenderer,
  AstroRenderer,
  ClientDirectiveConfig,
  HookParameters,
  IntegrationResolvedRoute,
  RedirectConfig,
  RouteData,
  RoutePart,
  RouteType,
  SSRComponentMetadata,
  SSRLoadedRenderer,
  SSRLoadedRendererValue,
  SSRManifest,
  ValidRedirectStatus,
} from "astro";
```

### `AstroIntegrationLogger`

[Section titled “AstroIntegrationLogger”](#astrointegrationlogger)

An instance of the Astro logger, useful to write logs. This logger uses the same [log level](/en/reference/cli-reference/#--verbose) configured via CLI.

**Methods available** to write to terminal:

* `logger.info("Message")`;
* `logger.warn("Message")`;
* `logger.error("Message")`;
* `logger.debug("Message")`;

All the messages are prepended with a label that has the same value as the name of the integration.

integration.ts

```ts
import type { AstroIntegration } from "astro";
export function formatIntegration(): AstroIntegration {
  return {
    name: "astro-format",
    hooks: {
      "astro:build:done": ({ logger }) => {
        // do something
        logger.info("Integration ready.");
      }
    }
  }
}
```

The example above will log a message that includes the provided `info` message:

```shell
[astro-format] Integration ready.
```

To log some messages with a different label, use the `.fork` method to specify an alternative to the default `name`:

integration.ts

```ts
import type { AstroIntegration } from "astro";
export function formatIntegration(): AstroIntegration {
  return {
    name: "astro-format",
    hooks: {
      "astro:config:done": ({ logger }) => {
        // do something
        logger.info("Integration ready.");
      },
      "astro:build:done": ({ logger }) => {
        const buildLogger = logger.fork("astro-format/build");
        // do something
        buildLogger.info("Build finished.")
      }
    }
  }
}
```

The example above will produce logs with `[astro-format]` by default, and `[astro-format/build]` when specified:

```shell
[astro-format] Integration ready.
[astro-format/build] Build finished.
```

### `AstroIntegrationMiddleware`

[Section titled “AstroIntegrationMiddleware”](#astrointegrationmiddleware)

**Type:** `{ order: "pre" | "post"; entrypoint: string | URL; }`

Describes a [middleware added by an integration](#addmiddleware-option).

#### `AstroIntegrationMiddleware.order`

[Section titled “AstroIntegrationMiddleware.order”](#astrointegrationmiddlewareorder)

**Type:** `"pre" | "post"`

Specifies whether the middleware should run before (`pre`) or after (`post`) other middleware.

#### `AstroIntegrationMiddleware.entrypoint`

[Section titled “AstroIntegrationMiddleware.entrypoint”](#astrointegrationmiddlewareentrypoint)

**Type:** `string | URL`

Defines the import path of the middleware.

### `AstroMiddlewareInstance`

[Section titled “AstroMiddlewareInstance”](#astromiddlewareinstance)

**Type:** `{ onRequest?: MiddlewareHandler; }`

An object containing an [`onRequest()`](/en/reference/modules/astro-middleware/#onrequest) property defined with the project’s middleware function when it exists.

### `AstroPrerenderer`

[Section titled “AstroPrerenderer”](#astroprerenderer)

**Type:** `string`

**Added in:** `astro@6.0.0`

Describes a [custom prerender](/en/reference/adapter-reference/#custom-prerenderer) that adapters can provide to control page prerendering.

#### `AstroPrerenderer.name`

[Section titled “AstroPrerenderer.name”](#astroprerenderername)

**Type:** `string`

Specifies a unique name for the prerender.

#### `AstroPrerenderer.setup()`

[Section titled “AstroPrerenderer.setup()”](#astroprerenderersetup)

**Type:** `() => Promise<void>`

Defines an optional method that will be called once before the prerendering starts. This is useful for starting a preview server.

#### `AstroPrerenderer.getStaticPaths()`

[Section titled “AstroPrerenderer.getStaticPaths()”](#astroprerenderergetstaticpaths)

**Type:** `() => Promise<Array<{ pathname: string; route: RouteData; }>>`

Returns a list of objects describing the prerendered route path and its associated data.

#### `AstroPrerenderer.render()`

[Section titled “AstroPrerenderer.render()”](#astroprerendererrender)

**Type:** `(request: Request, options: { routeData: RouteData }) => Promise<Response>`

Defines an optional method describing how to render a page. This will be called by Astro for each path returned by [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths).

#### `AstroPrerenderer.teardown()`

[Section titled “AstroPrerenderer.teardown()”](#astroprerendererteardown)

**Type:** `() => Promise<void>`

Defines an optional method called once all pages are pre-rendered. This is useful for performing cleanup tasks such as stopping a preview server.

### `AstroRenderer`

[Section titled “AstroRenderer”](#astrorenderer)

**Type:** `{ name: string; clientEntrypoint?: string | URL; serverEntrypoint: string | URL; }`

Describes a [component framework renderer added by an integration](#addrenderer-option).

#### `AstroRenderer.name`

[Section titled “AstroRenderer.name”](#astrorenderername)

**Type:** `string`

The name of the component framework renderer.

#### `AstroRenderer.clientEntrypoint`

[Section titled “AstroRenderer.clientEntrypoint”](#astrorenderercliententrypoint)

**Type:** `string | URL`

Defines the import path of the renderer that runs on the client whenever your component is used.

#### `AstroRenderer.serverEntrypoint`

[Section titled “AstroRenderer.serverEntrypoint”](#astrorendererserverentrypoint)

**Type:** `string | URL`

Defines the import path of the renderer that runs during server-side requests or static builds whenever your component is used.

### `ClientDirectiveConfig`

[Section titled “ClientDirectiveConfig”](#clientdirectiveconfig)

**Type:** `{ name: string; entrypoint: string | URL; }`

Describes a [custom client directive added by an integration](#addclientdirective-option).

#### `ClientDirectiveConfig.name`

[Section titled “ClientDirectiveConfig.name”](#clientdirectiveconfigname)

**Type:** `string`

A custom name for the event triggered by the directive.

#### `ClientDirectiveConfig.entrypoint`

[Section titled “ClientDirectiveConfig.entrypoint”](#clientdirectiveconfigentrypoint)

**Type:** `string | URL`

Defines the import path of the code executed whenever the directive is used.

### `HookParameters`

[Section titled “HookParameters”](#hookparameters)

You can get the type of a hook’s arguments by passing the hook’s name to the `HookParameters` utility type.

In the following example, a function’s `options` argument is typed to match the parameters of the [`astro:config:setup` hook](#astroconfigsetup):

```ts
import type { HookParameters } from 'astro';


function mySetup(options: HookParameters<'astro:config:setup'>) {
  options.updateConfig({ /* ... */ });
}
```

### `IntegrationResolvedRoute`

[Section titled “IntegrationResolvedRoute”](#integrationresolvedroute)

A subset of [`RouteData`](#routedata) with remapped properties.

```ts
interface IntegrationResolvedRoute extends Pick<
    RouteData,
    'params' | 'pathname' | 'segments' | 'type' | 'redirect' | 'origin'
  > & {
  pattern: RouteData['route'];
  patternRegex: RouteData['pattern'];
  entrypoint: RouteData['component'];
  isPrerendered: RouteData['prerender'];
  redirectRoute?: IntegrationResolvedRoute;
  fallbackRoutes: IntegrationResolvedRoute[];
  generate: (data?: any) => string;
}
```

#### `IntegrationResolvedRoute.pattern`

[Section titled “IntegrationResolvedRoute.pattern”](#integrationresolvedroutepattern)

**Type:** [`RouteData['route']`](#routedataroute)

Allows you to identify the type of route based on its path. Here are some examples of paths associated with their pattern:

* `src/pages/index.astro` will be `/`
* `src/pages/blog/[...slug].astro` will be `/blog/[...slug]`
* `src/pages/site/[blog]/[...slug].astro` will be `/site/[blog]/[...slug]`

#### `IntegrationResolvedRoute.patternRegex`

[Section titled “IntegrationResolvedRoute.patternRegex”](#integrationresolvedroutepatternregex)

**Type:** [`RouteData['pattern']`](#routedatapattern)

Allows you to access a regex used for matching an input URL against a requested route.

For example, given a `[fruit]/about.astro` path, the regex will be `/^\/([^/]+?)\/about\/?$/`. Using `pattern.test("banana/about")` will return `true`.

#### `IntegrationResolvedRoute.entrypoint`

[Section titled “IntegrationResolvedRoute.entrypoint”](#integrationresolvedrouteentrypoint)

**Type:** [`RouteData['component']`](#routedatacomponent)

The URL pathname of the source component.

#### `IntegrationResolvedRoute.isPrerendered`

[Section titled “IntegrationResolvedRoute.isPrerendered”](#integrationresolvedrouteisprerendered)

**Type:** [`RouteData['prerender']`](#routedataprerender)

Determines whether the route use [on demand rendering](/en/guides/on-demand-rendering/). The value will be `true` for projects configured with:

* `output: 'static'` when the route does not export `const prerender = true`
* `output: 'server'` when the route exports `const prerender = false`

#### `IntegrationResolvedRoute.redirectRoute`

[Section titled “IntegrationResolvedRoute.redirectRoute”](#integrationresolvedrouteredirectroute)

**Type:** `IntegrationResolvedRoute | undefined`

When the value of `IntegrationResolvedRoute.type` is `redirect`, the value will be the `IntegrationResolvedRoute` to redirect to. Otherwise, the value will be undefined.

#### `IntegrationResolvedRoute.fallbackRoutes`

[Section titled “IntegrationResolvedRoute.fallbackRoutes”](#integrationresolvedroutefallbackroutes)

**Type:** `IntegrationResolvedRoute[]`

**Added in:** `astro@6.1.0` New

When the project uses [i18n with fallback routes](/en/guides/internationalization/#fallback), the value will be a list of the routes this route falls back to when the requested locale isn’t available. The fallback content may be served as a redirect or rewrite depending on `i18n.routing.fallbackType`. Otherwise, the value will be an empty array.

#### `IntegrationResolvedRoute.generate()`

[Section titled “IntegrationResolvedRoute.generate()”](#integrationresolvedroutegenerate)

**Type:** `(data?: any) => string`

**Added in:** `astro@6.0.0`

A function that provides the optional parameters of the route, interpolates them with the route pattern, and returns the path name of the route.

For example, with a route such as `/blog/[...id].astro`, the `generate()` function could return:

```js
generate({ id: 'presentation' }) // will output `/blog/presentation`
```

### `RedirectConfig`

[Section titled “RedirectConfig”](#redirectconfig)

**Type:** `string | { status: ValidRedirectStatus; destination: string; }`

Describes the destination of a redirect. This can be a string or an object containing information about the status code and its destination.

### `RouteData`

[Section titled “RouteData”](#routedata)

Describes the information about a route.

#### `RouteData.route`

[Section titled “RouteData.route”](#routedataroute)

**Type:** `string`

Defines the current route pattern. Here are some examples of paths associated with their pattern:

* `src/pages/index.astro` will be `/`
* `src/pages/blog/[...slug].astro` will be `/blog/[...slug]`
* `src/pages/site/[blog]/[...slug].astro` will be `/site/[blog]/[...slug]`

#### `RouteData.component`

[Section titled “RouteData.component”](#routedatacomponent)

**Type:** `string`

Specifies the source component URL.

#### `RouteData.params`

[Section titled “RouteData.params”](#routedataparams)

**Type:** `string[]`

Allows you to access the route `params`. For example, when a project uses the following [dynamic routes](/en/guides/routing/#dynamic-routes) `/pages/[lang]/[...slug].astro`, the value will be `['lang', '...slug']`.

#### `RouteData.pathname`

[Section titled “RouteData.pathname”](#routedatapathname)

**Type:** `string | undefined`

For regular routes, the value will be the URL pathname where this route will be served. When the project uses [dynamic routes](/en/guides/routing/#dynamic-routes) (ie. `[dynamic]` or `[...spread]`), the pathname will be undefined.

#### `RouteData.distURL`

[Section titled “RouteData.distURL”](#routedatadisturl)

**Type:** `URL[]`

**Added in:** `astro@5.0.0`

Defines the paths of the physical files emitted by this route. When a route isn’t prerendered, the value is an empty array.

#### `RouteData.pattern`

[Section titled “RouteData.pattern”](#routedatapattern)

**Type:** `RegExp`

Specifies a regex to use for matching an input URL against a requested route.

For example, given a `[fruit]/about.astro` path, the regex will be `/^\/([^/]+?)\/about\/?$/`. Using `pattern.test("banana/about")` will return `true`.

#### `RouteData.segments`

[Section titled “RouteData.segments”](#routedatasegments)

**Type:** `RoutePart[][]`

Allows you to access the route [`params`](#routedataparams) with additional metadata. Each object contains the following properties:

* `content`: the `param` name,
* `dynamic`: whether the route is dynamic or not,
* `spread`: whether the dynamic route uses the spread syntax or not.

For example, the following route `/pages/[blog]/[...slug].astro` will output the segments:

```js
[
  [ { content: 'pages', dynamic: false, spread: false } ],
  [ { content: 'blog', dynamic: true, spread: false } ],
  [ { content: '...slug', dynamic: true, spread: true } ]
]
```

#### `RouteData.type`

[Section titled “RouteData.type”](#routedatatype)

**Type:** [`RouteType`](#routetype)

Allows you to identify the [type of route](#routetype).

#### `RouteData.prerender`

[Section titled “RouteData.prerender”](#routedataprerender)

**Type:** `boolean`

Determines whether a route uses [on demand rendering](/en/guides/on-demand-rendering/) or is statically prerendered at build time.

See also [`prerendered`](/en/reference/routing-reference/#prerender) in the routing reference.

#### `RouteData.redirect`

[Section titled “RouteData.redirect”](#routedataredirect)

**Type:** `RedirectConfig | undefined`

Allows you to access the route to redirect to.

#### `RouteData.redirectRoute`

[Section titled “RouteData.redirectRoute”](#routedataredirectroute)

**Type:** `RouteData | undefined`

Specifies the `RouteData` to redirect to when [`RouteData.type`](#routedatatype) is `redirect`.

#### `RouteData.fallbackRoutes`

[Section titled “RouteData.fallbackRoutes”](#routedatafallbackroutes)

**Type:** `RouteData[]`

**Added in:** `astro@3.5.6`

Defines a list of `RouteData` to fallback to when [`i18n.fallback`](/en/reference/configuration-reference/#i18nfallback) has a list of locales.

#### `RouteData.isIndex`

[Section titled “RouteData.isIndex”](#routedataisindex)

**Type:** `boolean`

Specifies if the route is a directory index (e.g. `src/pages/index.astro`, `src/pages/blog/index.astro`).

#### `RouteData.origin`

[Section titled “RouteData.origin”](#routedataorigin)

**Type:** `'internal' | 'external' | 'project'`

**Added in:** `astro@5.0.0`

Determines if a route comes from Astro core (`internal`), an integration (`external`) or the user’s project (`project`).

### `RoutePart`

[Section titled “RoutePart”](#routepart)

**Type:** `{ content: string; dynamic: boolean; spread: boolean; }`

Describes a route segment.

#### `RoutePart.content`

[Section titled “RoutePart.content”](#routepartcontent)

**Type:** `string`

Specifies the parameter name for the route. For example:

* `about.astro` has the name `about`
* `[slug].astro` has the name `slug`
* `[...id].astro` has the name `id`

#### `RoutePart.dynamic`

[Section titled “RoutePart.dynamic”](#routepartdynamic)

**Type:** `boolean`

Whether the route is dynamic or not.

#### `RoutePart.spread`

[Section titled “RoutePart.spread”](#routepartspread)

**Type:** `boolean`

Whether the dynamic route uses the spread syntax or not.

### `RouteType`

[Section titled “RouteType”](#routetype)

**Type:** `'page' | 'endpoint' | 'redirect' | 'fallback'`

A union of supported route types:

* `page`: a route that lives in the file system, usually an Astro component
* `endpoint`: a route that lives in the file system, usually a JS file that exposes endpoints methods
* `redirect`: a route points to another route that lives in the file system
* `fallback`: a route that doesn’t exist in the file system that needs to be handled with other means, usually a middleware

### `SSRComponentMetadata`

[Section titled “SSRComponentMetadata”](#ssrcomponentmetadata)

**Type:** `{ propagation: PropagationHint; containsHead: boolean; }`

Describes the build metadata of a component rendered by the server.

#### `SSRComponentMetadata.propagation`

[Section titled “SSRComponentMetadata.propagation”](#ssrcomponentmetadatapropagation)

**Type:** `'none' | 'self' | 'in-tree'`

A description of how to render head content from this component, including whether the Astro runtime needs to wait for a component:

* `none`: The component does not propagate the head content.
* `self`: The component appends the head content.
* `in-tree`: Another component within this component’s dependency tree appends the head content.

#### `SSRComponentMetadata.containsHead`

[Section titled “SSRComponentMetadata.containsHead”](#ssrcomponentmetadatacontainshead)

**Type:** `boolean`

Determines whether the component contains the head content.

### `SSRLoadedRenderer`

[Section titled “SSRLoadedRenderer”](#ssrloadedrenderer)

**Type:** `{ name: string; clientEntrypoint?: string | URL; ssr: SSRLoadedRendererValue; }`

Describes a renderer available for the server to use. This is a subset of [`AstroRenderer`](#astrorenderer) with additional properties.

#### `SSRLoadedRenderer.ssr`

[Section titled “SSRLoadedRenderer.ssr”](#ssrloadedrendererssr)

**Type:** [`SSRLoadedRendererValue`](#ssrloadedrenderervalue)

Defines the functions and configuration used by the server for this framework.

### `SSRLoadedRendererValue`

[Section titled “SSRLoadedRendererValue”](#ssrloadedrenderervalue)

Contains the functions and configuration necessary to render components on the server from a specific UI framework.

#### `SSRLoadedRendererValue.name`

[Section titled “SSRLoadedRendererValue.name”](#ssrloadedrenderervaluename)

**Type:** `string`

Specifies the name identifier for the renderer.

#### `SSRLoadedRendererValue.check()`

[Section titled “SSRLoadedRendererValue.check()”](#ssrloadedrenderervaluecheck)

**Type:** `AsyncRendererComponentFn<boolean>`

Determines whether the renderer should handle the component.

#### `SSRLoadedRendererValue.renderToStaticMarkup()`

[Section titled “SSRLoadedRendererValue.renderToStaticMarkup()”](#ssrloadedrenderervaluerendertostaticmarkup)

**Type:** `AsyncRendererComponentFn<{ html: string; attrs?: Record<string, string>; }>`

Renders a framework component to static HTML markup on the server.

#### `SSRLoadedRendererValue.supportsAstroStaticSlot`

[Section titled “SSRLoadedRendererValue.supportsAstroStaticSlot”](#ssrloadedrenderervaluesupportsastrostaticslot)

**Type:** `boolean`

**Added in:** `astro@2.5.0`

Indicates whether the renderer supports Astro’s static slot optimization. When true, Astro prevents the removal of nested slots within islands.

#### `SSRLoadedRendererValue.renderHydrationScript()`

[Section titled “SSRLoadedRendererValue.renderHydrationScript()”](#ssrloadedrenderervaluerenderhydrationscript)

**Type:** `() => string`

**Added in:** `astro@4.1.0`

Returns a framework-specific hydration script that must be injected into the HTML before the first component that uses this renderer.

### `SSRManifest`

[Section titled “SSRManifest”](#ssrmanifest)

An object containing build configuration and project metadata that the server adapters use at runtime to serve on-demand rendered pages.

#### `SSRManifest.adapterName`

[Section titled “SSRManifest.adapterName”](#ssrmanifestadaptername)

**Type:** `string`

Defines the name of the [server adapter](/en/guides/on-demand-rendering/#server-adapters) used for on-demand rendering.

#### `SSRManifest.routes`

[Section titled “SSRManifest.routes”](#ssrmanifestroutes)

**Type:** `RouteInfo[]`

A list of information about the routes available in this project. Each entry contains the following properties.

##### `RouteInfo.routeData`

[Section titled “RouteInfo.routeData”](#routeinforoutedata)

**Type:** [`RouteData`](#routedata)

An object describing known information about a route.

##### `RouteInfo.file`

[Section titled “RouteInfo.file”](#routeinfofile)

**Type:** `string`

Specifies the file path to the built route entrypoint.

##### `RouteInfo.links`

[Section titled “RouteInfo.links”](#routeinfolinks)

**Type:** `string[]`

Defines a list of [HTML `link` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/link) required by this route.

##### `RouteInfo.scripts`

[Section titled “RouteInfo.scripts”](#routeinfoscripts)

**Type:** `Array<{ children: string; stage: string } | { type: 'inline' | 'external'; value: string }>`

Defines a list of scripts associated with this route. This includes both integration-injected scripts with `children` and `stage` properties and hoisted scripts with `type` and `value` properties.

##### `RouteInfo.styles`

[Section titled “RouteInfo.styles”](#routeinfostyles)

**Type:** `Array<{ type: "inline"; content: string; } | { type: "external"; src: string; }>`

**Added in:** `astro@2.4.0`

Defines the list of stylesheets associated with this route. This includes both inline styles and stylesheet URLs.

#### `SSRManifest.site`

[Section titled “SSRManifest.site”](#ssrmanifestsite)

**Type:** `string`

Specifies the [configured `site`](/en/reference/configuration-reference/#site).

#### `SSRManifest.base`

[Section titled “SSRManifest.base”](#ssrmanifestbase)

**Type:** `string`

Specifies the [configured `base` path](/en/reference/configuration-reference/#base) to deploy to.

#### `SSRManifest.userAssetsBase`

[Section titled “SSRManifest.userAssetsBase”](#ssrmanifestuserassetsbase)

**Type:** `string | undefined`

**Added in:** `astro@5.3.1`

Specifies the base path to use in development mode for user-generated assets, such as scripts and styles.

#### `SSRManifest.trailingSlash`

[Section titled “SSRManifest.trailingSlash”](#ssrmanifesttrailingslash)

**Type:** [`AstroConfig['trailingSlash']`](/en/reference/configuration-reference/#trailingslash)

**Added in:** `astro@3.5.4`

Specifies the [configured behavior for trailing slashes](/en/reference/configuration-reference/#trailingslash) in development mode and for on-demand rendered pages.

#### `SSRManifest.buildFormat`

[Section titled “SSRManifest.buildFormat”](#ssrmanifestbuildformat)

**Type:** [`NonNullable<AstroConfig['build']>['format']`](/en/reference/configuration-reference/#buildformat)

**Added in:** `astro@4.2.2`

Specifies the [configured output file format](/en/reference/configuration-reference/#buildformat).

#### `SSRManifest.compressHTML`

[Section titled “SSRManifest.compressHTML”](#ssrmanifestcompresshtml)

**Type:** `boolean`

**Added in:** `astro@2.7.2`

Determines whether [HTML minification is enabled in the project configuration](/en/reference/configuration-reference/#compresshtml).

#### `SSRManifest.assetsPrefix`

[Section titled “SSRManifest.assetsPrefix”](#ssrmanifestassetsprefix)

**Type:** `string | ({ fallback: string; } & Record<string, string>) | undefined`

**Added in:** `astro@2.3.1`

Specifies the [configured prefix for Astro-generated asset links](/en/reference/configuration-reference/#buildassetsprefix).

#### `SSRManifest.renderers`

[Section titled “SSRManifest.renderers”](#ssrmanifestrenderers)

**Type:** `SSRLoadedRenderer[]`

A list of renderers (e.g. React, Vue, Svelte, MDX) available for the server to use.

#### `SSRManifest.serverLike`

[Section titled “SSRManifest.serverLike”](#ssrmanifestserverlike)

**Type:** `boolean`

**Added in:** `astro@6.0.0`

Determines whether this application uses any on-demand rendered routes.

#### `SSRManifest.clientDirectives`

[Section titled “SSRManifest.clientDirectives”](#ssrmanifestclientdirectives)

**Type:** `Map<string, string>`

**Added in:** `astro@2.5.0`

Defines a mapping of client directive names (e.g. `load`, `visible`) to their implementation code. This includes both [built-in client directives](/en/reference/directives-reference/#client-directives) and [custom client directives](/en/reference/directives-reference/#custom-client-directives).

#### `SSRManifest.entryModules`

[Section titled “SSRManifest.entryModules”](#ssrmanifestentrymodules)

**Type:** `Record<string, string>`

Defines a mapping of entrypoints to their output file paths.

#### `SSRManifest.inlinedScripts`

[Section titled “SSRManifest.inlinedScripts”](#ssrmanifestinlinedscripts)

**Type:** `Map<string, string>`

**Added in:** `astro@4.5.0`

Defines a mapping of script identifiers to their content for scripts that will be inlined in the HTML output.

#### `SSRManifest.assets`

[Section titled “SSRManifest.assets”](#ssrmanifestassets)

**Type:** `Set<string>`

Defines a set of file paths for all assets that are part of the build.

#### `SSRManifest.componentMetadata`

[Section titled “SSRManifest.componentMetadata”](#ssrmanifestcomponentmetadata)

**Type:** `Map<string, SSRComponentMetadata>`

**Added in:** `astro@2.1.7`

Defines a mapping of component identifiers to their build metadata. Each entry contains information about the [`propagation`](#ssrcomponentmetadatapropagation) behavior and whether it contains head elements.

#### `SSRManifest.pageModule`

[Section titled “SSRManifest.pageModule”](#ssrmanifestpagemodule)

**Type:** `{ page: ImportComponentInstance; onRequest?: MiddlewareHandler; renderers: SSRLoadedRenderer[]; }`

**Added in:** `astro@2.7.0`

Specifies information about a page module.

##### `SSRManifest.pageModule.page()`

[Section titled “SSRManifest.pageModule.page()”](#ssrmanifestpagemodulepage)

**Type:** `() => Promise<ComponentInstance>`

A function to retrieve an instance of the page component.

##### `SSRManifest.pageModule.onRequest()`

[Section titled “SSRManifest.pageModule.onRequest()”](#ssrmanifestpagemoduleonrequest)

**Type:** [`MiddlewareHandler`](/en/reference/modules/astro-middleware/#middlewarehandler)

**Added in:** `astro@3.0.3`

An [Astro middleware function](/en/reference/modules/astro-middleware/#onrequest) when defined in the user project.

#### `SSRManifest.pageMap`

[Section titled “SSRManifest.pageMap”](#ssrmanifestpagemap)

**Type:** `Map<string, () => Promise<typeof pageModule>>`

Defines a mapping of component paths to their importable instances.

#### `SSRManifest.serverIslandMappings`

[Section titled “SSRManifest.serverIslandMappings”](#ssrmanifestserverislandmappings)

**Type:** `() => Promise<ServerIslandMappings> | ServerIslandMappings`

**Added in:** `astro@6.0.0`

An object, or a function that returns an object, describing available server islands mapping.

##### `SSRManifest.serverIslandMappings.serverIslandMap`

[Section titled “SSRManifest.serverIslandMappings.serverIslandMap”](#ssrmanifestserverislandmappingsserverislandmap)

**Type:** `Map<string, () => Promise<ComponentInstance>>`

**Added in:** `astro@4.12.0`

Defines a mapping of server island IDs to their component instances.

##### `SSRManifest.serverIslandMappings.serverIslandNameMap`

[Section titled “SSRManifest.serverIslandMappings.serverIslandNameMap”](#ssrmanifestserverislandmappingsserverislandnamemap)

**Type:** `Map<string, string>`

**Added in:** `astro@4.12.0`

Defines a mapping of server island component paths to their assigned names.

#### `SSRManifest.key`

[Section titled “SSRManifest.key”](#ssrmanifestkey)

**Type:** `Promise<CryptoKey>`

**Added in:** `astro@4.13.4`

Determines the [cryptographic key](https://developer.mozilla.org/en-US/docs/Web/API/CryptoKey) used for encrypting server island props.

#### `SSRManifest.i18n`

[Section titled “SSRManifest.i18n”](#ssrmanifesti18n)

**Type:** `SSRManifestI18n | undefined`

**Added in:** `astro@3.5.0`

Specifies the resolved [`i18n` configuration](/en/reference/configuration-reference/#i18n) when enabled in the project.

##### `SSRManifest.i18n.strategy`

[Section titled “SSRManifest.i18n.strategy”](#ssrmanifesti18nstrategy)

**Type:** `"manual" | "pathname-prefix-always" | "pathname-prefix-other-locales" | "pathname-prefix-always-no-redirect" | "domains-prefix-always" | "domains-prefix-other-locales" | "domains-prefix-always-no-redirect"`

Defines the [i18n routing strategy](/en/reference/configuration-reference/#i18nrouting) configured. This determines how locales are handled in URLs and whether redirects occur.

##### `SSRManifest.i18n.locales`

[Section titled “SSRManifest.i18n.locales”](#ssrmanifesti18nlocales)

**Type:** `Locales`

Specifies a list of [supported locales configured in the project](/en/reference/configuration-reference/#i18nlocales).

##### `SSRManifest.i18n.defaultLocale`

[Section titled “SSRManifest.i18n.defaultLocale”](#ssrmanifesti18ndefaultlocale)

**Type:** `string`

Determines the [default locale configured in the project](/en/reference/configuration-reference/#i18ndefaultlocale).

##### `SSRManifest.i18n.fallback`

[Section titled “SSRManifest.i18n.fallback”](#ssrmanifesti18nfallback)

**Type:** `Record<string, string> | undefined`

Specifies a mapping of locales to their fallback locales as [configured in `i18n.fallback`](/en/reference/configuration-reference/#i18nfallback).

##### `SSRManifest.i18n.fallbackType`

[Section titled “SSRManifest.i18n.fallbackType”](#ssrmanifesti18nfallbacktype)

**Type:** `"redirect" | "rewrite"`

Determines the [configured fallback strategy for the project](/en/reference/configuration-reference/#i18nroutingfallbacktype).

##### `SSRManifest.i18n.domainLookupTable`

[Section titled “SSRManifest.i18n.domainLookupTable”](#ssrmanifesti18ndomainlookuptable)

**Type:** `Record<string, string>`

A mapping of [configured domains](/en/reference/configuration-reference/#i18ndomains) to their associated locales.

#### `SSRManifest.middleware`

[Section titled “SSRManifest.middleware”](#ssrmanifestmiddleware)

**Type:** `() => Promise<AstroMiddlewareInstance> | AstroMiddlewareInstance`

**Added in:** `astro@4.2.5`

Defines an instance to load the middleware.

#### `SSRManifest.actions`

[Section titled “SSRManifest.actions”](#ssrmanifestactions)

**Type:** `() => Promise<{ server: Record<string, ActionClient>; }> | { server: Record<string, ActionClient>; }`

**Added in:** `astro@5.4.2`

An object, or a function that returns an object, with a `server` property that maps action names to their callable functions.

#### `SSRManifest.sessionDriver()`

[Section titled “SSRManifest.sessionDriver()”](#ssrmanifestsessiondriver)

**Type:** `() => Promise<{ default: SessionDriverFactory | null }>`

**Added in:** `astro@6.0.0`

Retrieves the [configured session driver](/en/reference/configuration-reference/#sessiondriver) when enabled.

#### `SSRManifest.checkOrigin`

[Section titled “SSRManifest.checkOrigin”](#ssrmanifestcheckorigin)

**Type:** `boolean`

**Added in:** `astro@4.6.0`

Determines whether [origin checking is enabled in the security configuration](/en/reference/configuration-reference/#securitycheckorigin).

#### `SSRManifest.allowedDomains`

[Section titled “SSRManifest.allowedDomains”](#ssrmanifestalloweddomains)

**Type:** `Partial<RemotePattern>[]`

Specifies the [configured list of permitted host patterns](/en/reference/configuration-reference/#securityalloweddomains) for incoming requests when using on-demand rendering.

#### `SSRManifest.sessionConfig`

[Section titled “SSRManifest.sessionConfig”](#ssrmanifestsessionconfig)

**Type:** `SessionConfig<TDriver> & { driverModule?: () => Promise<{ default: () => unstorage.Driver }>; }`

**Added in:** `astro@5.1.0`

An object containing the [resolved session configuration](/en/reference/configuration-reference/#session-options) and an additional property defining the driver in use.

#### `SSRManifest.cacheDir`

[Section titled “SSRManifest.cacheDir”](#ssrmanifestcachedir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Specifies the [configured directory for caching build artifacts](/en/reference/configuration-reference/#cachedir).

#### `SSRManifest.srcDir`

[Section titled “SSRManifest.srcDir”](#ssrmanifestsrcdir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Specifies the [configured directory that Astro will read the site from](/en/reference/configuration-reference/#srcdir).

#### `SSRManifest.outDir`

[Section titled “SSRManifest.outDir”](#ssrmanifestoutdir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Specifies the [configured directory in which to write the final build](/en/reference/configuration-reference/#outdir).

#### `SSRManifest.rootDir`

[Section titled “SSRManifest.rootDir”](#ssrmanifestrootdir)

**Type:** `URL`

**Added in:** `astro@6.0.0`

Specifies the resolved URL for the [directory configured as the project root](/en/reference/configuration-reference/#root).

#### `SSRManifest.publicDir`

[Section titled “SSRManifest.publicDir”](#ssrmanifestpublicdir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Specifies the [configured directory for the static assets](/en/reference/configuration-reference/#publicdir).

#### `SSRManifest.assetsDir`

[Section titled “SSRManifest.assetsDir”](#ssrmanifestassetsdir)

**Type:** `string`

**Added in:** `astro@6.0.0`

Specifies the [configured directory for generated assets](/en/reference/configuration-reference/#buildassets) in the build output.

#### `SSRManifest.buildClientDir`

[Section titled “SSRManifest.buildClientDir”](#ssrmanifestbuildclientdir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Determines the path where client-side build artifacts (e.g. JavaScript, CSS) are output within the build directory.

#### `SSRManifest.buildServerDir`

[Section titled “SSRManifest.buildServerDir”](#ssrmanifestbuildserverdir)

**Type:** `URL`

**Added in:** `astro@5.2.0`

Determines the path where server-side build artifacts are output within the build directory.

#### `SSRManifest.csp`

[Section titled “SSRManifest.csp”](#ssrmanifestcsp)

**Type:** `SSRManifestCSP | undefined`

**Added in:** `astro@5.9.0`

Describes the [Content Security Policy configuration](/en/reference/configuration-reference/#securitycsp).

##### `SSRManifest.csp.cspDestination`

[Section titled “SSRManifest.csp.cspDestination”](#ssrmanifestcspcspdestination)

**Type:** `'adapter' | 'meta' | 'header' | undefined`

Specifies whether CSP directives should be injected as a `meta` element, as a response `header`, or by the [`adapter` when it supports setting response headers](/en/reference/adapter-reference/#staticheaders).

##### `SSRManifest.csp.algorithm`

[Section titled “SSRManifest.csp.algorithm”](#ssrmanifestcspalgorithm)

**Type:** `'SHA-256' | 'SHA-384' | 'SHA-512'`

Specifies the [configured hash function](/en/reference/configuration-reference/#securitycspalgorithm).

##### `SSRManifest.csp.scriptHashes`

[Section titled “SSRManifest.csp.scriptHashes”](#ssrmanifestcspscripthashes)

**Type:** `string[]`

Specifies a list of generated hashes for project scripts and [user-supplied hashes](/en/reference/configuration-reference/#securitycspscriptdirectivehashes) for external scripts.

##### `SSRManifest.csp.scriptResources`

[Section titled “SSRManifest.csp.scriptResources”](#ssrmanifestcspscriptresources)

**Type:** `string[]`

Specifies a list of valid sources combining the [configured script resources](/en/reference/configuration-reference/#securitycspscriptdirectiveresources) and the [injected script resources](/en/reference/api-reference/#cspinsertscriptresource).

##### `SSRManifest.csp.isStrictDynamic`

[Section titled “SSRManifest.csp.isStrictDynamic”](#ssrmanifestcspisstrictdynamic)

**Type:** `boolean`

Determines whether support for [dynamic script injection is enabled in the configuration](/en/reference/configuration-reference/#securitycspscriptdirectivestrictdynamic).

##### `SSRManifest.csp.styleHashes`

[Section titled “SSRManifest.csp.styleHashes”](#ssrmanifestcspstylehashes)

**Type:** `string[]`

Specifies a list of generated hashes for project styles and [user-supplied hashes](/en/reference/configuration-reference/#securitycspstyledirectivehashes) for external styles.

##### `SSRManifest.csp.styleResources`

[Section titled “SSRManifest.csp.styleResources”](#ssrmanifestcspstyleresources)

**Type:** `string[]`

Specifies a list of valid sources combining the [configured style resources](/en/reference/configuration-reference/#securitycspstyledirectiveresources) and the [injected style resources](/en/reference/api-reference/#cspinsertstyleresource).

##### `SSRManifest.csp.directives`

[Section titled “SSRManifest.csp.directives”](#ssrmanifestcspdirectives)

**Type:** `CspDirective[]`

Specifies the [configured list of valid sources](/en/reference/configuration-reference/#securitycspdirectives) for specific content types.

#### `SSRManifest.devToolbar`

[Section titled “SSRManifest.devToolbar”](#ssrmanifestdevtoolbar)

**Type:** `{ enabled: boolean; latestAstroVersion: string | undefined; debugInfoOutput: string | undefined; }`

**Added in:** `astro@6.0.0`

Describes the resolved dev toolbar settings.

##### `SSRManifest.devToolbar.enabled`

[Section titled “SSRManifest.devToolbar.enabled”](#ssrmanifestdevtoolbarenabled)

**Type:** `boolean`

**Added in:** `astro@6.0.0`

Determines [whether the dev toolbar is enabled](/en/reference/configuration-reference/#devtoolbarenabled).

##### `SSRManifest.devToolbar.latestAstroVersion`

[Section titled “SSRManifest.devToolbar.latestAstroVersion”](#ssrmanifestdevtoolbarlatestastroversion)

**Type:** `string | undefined`

**Added in:** `astro@6.0.0`

Specifies the latest available version of Astro. This is used to notify the user in the dev toolbar of when an update is available. This will be `undefined` when one of the following conditions applies:

* the check fails or has not been completed yet
* the user has disabled the check
* the user is already using the latest version

##### `SSRManifest.devToolbar.debugInfoOutput`

[Section titled “SSRManifest.devToolbar.debugInfoOutput”](#ssrmanifestdevtoolbardebuginfooutput)

**Type:** `string | undefined`

**Added in:** `astro@6.0.0`

Defines the serialized [debug information](/en/reference/cli-reference/#astro-info) passed to the dev toolbar for display.

#### `SSRManifest.internalFetchHeaders`

[Section titled “SSRManifest.internalFetchHeaders”](#ssrmanifestinternalfetchheaders)

**Type:** `Record<string, string>`

**Added in:** `astro@5.15.0`

Specifies the headers that are automatically added to internal fetch requests made during rendering.

#### `SSRManifest.logLevel`

[Section titled “SSRManifest.logLevel”](#ssrmanifestloglevel)

**Type:** `"error" | "warn" | "debug" | "info" | "silent"`

**Added in:** `astro@6.0.0`

Specifies the [Vite logging level](https://vite.dev/config/shared-options#loglevel).

### `ValidRedirectStatus`

[Section titled “ValidRedirectStatus”](#validredirectstatus)

**Type:** `301 | 302 | 303 | 307 | 308 | 300 | 304`

A union of supported redirect status code.

## Allow installation with `astro add`

[Section titled “Allow installation with astro add”](#allow-installation-with-astro-add)

[The `astro add` command](/en/reference/cli-reference/#astro-add) allows users to easily add integrations and adapters to their project. If you want *your* integration to be installable with this tool, **add `astro-integration` to the `keywords` field in your `package.json`**:

```json
{
  "name": "example",
  "keywords": ["astro-integration"],
}
```

Once you [publish your integration to npm](https://docs.npmjs.com/cli/v8/commands/npm-publish), running `astro add example` will install your package with any peer dependencies specified in your `package.json`. This will also apply your integration to the user’s `astro.config.*` like so:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
+import example from 'example';


export default defineConfig({
+  integrations: [example()],
})
```

Caution

This assumes your integration definition is 1) a `default` export and 2) a function. Ensure this is true before adding the `astro-integration` keyword!

## Integration Ordering

[Section titled “Integration Ordering”](#integration-ordering)

All integrations are run in the order that they are configured. For instance, for the array `[react(), svelte()]` in a user’s `astro.config.*`, `react` will run before `svelte`.

Your integration should ideally run in any order. If this isn’t possible, we recommend documenting that your integration needs to come first or last in your user’s `integrations` configuration array.

## Combine integrations into presets

[Section titled “Combine integrations into presets”](#combine-integrations-into-presets)

An integration can also be written as a collection of multiple, smaller integrations. We call these collections **presets.** Instead of creating a factory function that returns a single integration object, a preset returns an *array* of integration objects. This is useful for building complex features out of multiple integrations.

```js
integrations: [
  // Example: where examplePreset() returns: [integrationOne, integrationTwo, ...etc]
  examplePreset()
]
```

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Build your own Astro Integrations](https://www.freecodecamp.org/news/how-to-use-the-astro-ui-framework/#chapter-8-build-your-own-astro-integrations-1) - by Emmanuel Ohans on FreeCodeCamp
* [Astro Integration Template](https://github.com/florian-lefebvre/astro-integration-template) - by Florian Lefebvre on GitHub

# Legacy flags

To help some users migrate between versions of Astro, we occasionally introduce `legacy` flags.

These flags allow you to opt in to some deprecated or otherwise outdated behavior of Astro in the latest version, so that you can continue to upgrade and take advantage of new Astro releases until you are able to fully update your project code.

## `collectionsBackwardsCompat`

[Section titled “collectionsBackwardsCompat”](#collectionsbackwardscompat)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

The `legacy.collectionsBackwardsCompat` flag provides temporary backwards compatibility for projects unable to migrate to the Content Layer API introduced in v5.0.

astro.config.mjs

```js
export default defineConfig({
  legacy: {
    collectionsBackwardsCompat: true,
  },
});
```

This flag preserves some legacy v4 content collections features:

* Supports `type: 'content'` and `type: 'data'` without loaders
* Preserves legacy entry API: `entry.slug` and `entry.render()`
* Uses path-based entry IDs instead of slug-based IDs

This is a temporary migration helper. Migrate collections to the Content Layer API, then disable this flag.

# Actions API Reference

**Added in:** `astro@4.15.0`

Actions help you build a type-safe backend you can call from client code and HTML forms. All utilities to define and call actions are exposed by the `astro:actions` module. For examples and usage instructions, [see the Actions guide](/en/guides/actions/).

## Imports from `astro:actions`

[Section titled “Imports from astro:actions”](#imports-from-astroactions)

```js
import {
  ACTION_QUERY_PARAMS,
  ActionError,
  actions,
  defineAction,
  getActionContext,
  getActionPath,
  isActionError,
  isInputError,
 } from 'astro:actions';
```

### `defineAction()`

[Section titled “defineAction()”](#defineaction)

**Type:** `({ accept, input, handler }) => ActionClient`

A utility to define new actions in the `src/actions/index.ts` file. This accepts a [`handler()`](#handler-property) function containing the server logic to run, and an optional [`input`](#input-validator) property to validate input parameters at runtime.

src/actions/index.ts

```ts
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  getGreeting: defineAction({
    input: z.object({
      name: z.string(),
    }),
    handler: async (input, context) => {
      return `Hello, ${input.name}!`
    }
  })
}
```

#### `handler()` property

[Section titled “handler() property”](#handler-property)

**Type:** `(input: TInputSchema, context: ActionAPIContext) => TOutput | Promise<TOutput>`

A required function containing the server logic to run when the action is called. Data returned from the `handler()` is automatically serialized and sent to the caller.

The `handler()` is called with user input as its first argument. If an [`input`](#input-validator) validator is set, the user input will be validated before being passed to the handler. The second argument is [a subset of Astro’s `context` object](#actionapicontext).

Return values are parsed using the [devalue library](https://github.com/Rich-Harris/devalue). This supports JSON values and instances of `Date()`, `Map()`, `Set()`, and `URL()`.

#### `input` validator

[Section titled “input validator”](#input-validator)

**Type:** `ZodType | undefined`

An optional property that accepts a [Zod validator](/en/reference/modules/astro-zod/#common-data-type-validators) (e.g. Zod object, Zod discriminated union) to validate handler inputs at runtime. If the action fails to validate, [a `BAD_REQUEST` error](#actionerror) is returned and the `handler` is not called.

If `input` is omitted, the `handler` will receive an input of type `unknown` for JSON requests and type `FormData` for form requests.

#### `accept` property

[Section titled “accept property”](#accept-property)

**Type:** `"form" | "json"`\
**Default:** `json`

Defines the format expected by an action:

* Use `form` when your action accepts `FormData`.
* Use `json`, the default, for all other cases.

When your action accepts form inputs, the `z.object()` validator will automatically parse `FormData` to a typed object. All Zod validators are supported to validate your inputs.

Learn about [using validators with form inputs](/en/guides/actions/#using-validators-with-form-inputs) in the Actions guide, including example usage and special input handling.

### `actions`

[Section titled “actions”](#actions)

**Type:** `Record<string, ActionClient>`

An object containing all your actions with the action name as key associated to a function to call this action.

src/pages/index.astro

```astro
---
---


<script>
import { actions } from 'astro:actions';


async () => {
  const { data, error } = await actions.myAction({ /* ... */ });
}
</script>
```

In order for Astro to recognize this property, you may need to restart the dev server or [run the `astro sync` command](/en/reference/cli-reference/#astro-sync) (`s + enter`).

### `isInputError()`

[Section titled “isInputError()”](#isinputerror)

**Type:** `(error?: unknown) => boolean`

A utility used to check whether [an `ActionError`](#actionerror) is an input validation error. When the `input` validator is a `z.object()`, input errors include a `fields` object with error messages grouped by name.

See the [form input errors guide](/en/guides/actions/#displaying-form-input-errors) for more on using `isInputError()`.

### `isActionError()`

[Section titled “isActionError()”](#isactionerror)

**Type:** `(error?: unknown) => boolean`

A utility to check whether your action raised [an `ActionError`](#actionerror) within the [handler property](#handler-property). This is useful when narrowing the type of a generic error.

src/pages/index.astro

```astro
---
---


<script>
import { isActionError, actions } from 'astro:actions';


async () => {
  const { data, error } = await actions.myAction({ /* ... */ });
  if (isActionError(error)) {
    // Handle action-specific errors
    console.log(error.code);
  }
}
</script>
```

### `ActionError`

[Section titled “ActionError”](#actionerror)

The `ActionError()` constructor is used to create errors thrown by an action `handler`. This accepts a `code` property describing the error that occurred (example: `"UNAUTHORIZED"`), and an optional `message` property with further details.

The following example creates a new `ActionError` when the user is not logged in:

src/actions/index.ts

```ts
import { defineAction, ActionError } from "astro:actions";


export const server = {
  getUserOrThrow: defineAction({
    accept: 'form',
    handler: async (_, { locals }) => {
      if (locals.user?.name !== 'florian') {
        throw new ActionError({
          code: 'UNAUTHORIZED',
          message: 'Not logged in',
        });
      }
      return locals.user;
    },
  }),
}
```

You can also use `ActionError` to narrow the error type when handling the results of an action:

src/pages/index.astro

```astro
---
---


<script>
import { ActionError, actions } from 'astro:actions';


async () => {
  const { data, error } = await actions.myAction({ /* ... */ });
  if (error instanceof ActionError) {
    // Handle action-specific errors
    console.log(error.code);
  }
}
</script>
```

#### `code`

[Section titled “code”](#code)

**Type:** `ActionErrorCode`

Defines a human-readable version of an [HTTP status code](#actionerrorcode).

#### `message`

[Section titled “message”](#message)

**Type:** `string`

An optional property to describe the error (e.g. “User must be logged in.”).

#### `stack`

[Section titled “stack”](#stack)

**Type:** `string`

An optional property to pass the stack trace.

### `getActionContext()`

[Section titled “getActionContext()”](#getactioncontext)

**Type:** `(context: APIContext) => AstroActionContext`

**Added in:** `astro@5.0.0`

A function called from your middleware handler to retrieve information about inbound action requests. This returns an `action` object with information about the request, a `deserializeActionResult()` method, and the `setActionResult()` and `serializeActionResult()` functions to programmatically set the value returned by `Astro.getActionResult()`.

`getActionContext()` lets you programmatically get and set action results using middleware, allowing you to persist action results from HTML forms, gate action requests with added security checks, and more.

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  const { action, setActionResult, serializeActionResult } = getActionContext(context);
  if (action?.calledFrom === 'form') {
    const result = await action.handler();
    setActionResult(action.name, serializeActionResult(result));
  }
  return next();
});
```

#### `action`

[Section titled “action”](#action)

**Type:** `{ calledFrom: “rpc” | “form”; name: string; handler: () => Promise<SafeResult>; } | undefined`

An object containing information about an inbound action request. It is available from [`getActionContext()`](#getactioncontext), and provides the action `name`, `handler`, and whether the action was called from a client-side RPC function (e.g. `actions.newsletter()`) or an HTML form action.

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  const { action, setActionResult, serializeActionResult } = getActionContext(context);
  if (action?.calledFrom === 'rpc' && action.name.startsWith('private')) {
    // Check for a valid session token
  }
  // ...
});
```

##### `action.calledFrom`

[Section titled “action.calledFrom”](#actioncalledfrom)

**Type:** `"rpc" | "form"`

Whether an action was called using an RPC function or an HTML form action.

##### `action.name`

[Section titled “action.name”](#actionname)

**Type:** `string`

The name of the action. Useful to track the source of an action result during a redirect.

##### `action.handler()`

[Section titled “action.handler()”](#actionhandler)

**Type:** `() => Promise<SafeResult>`

A method to programmatically call an action to get the result.

#### `setActionResult()`

[Section titled “setActionResult()”](#setactionresult)

**Type:** `(actionName: string, actionResult: SerializedActionResult) => void`

A function to programmatically set the value returned by `Astro.getActionResult()` in middleware. It is passed the action name and an action result serialized by [`serializeActionResult()`](#serializeactionresult). Calling this function from middleware will disable Astro’s own action result handling.

This is useful when calling actions from an HTML form to persist and load results from a session.

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';
export const onRequest = defineMiddleware(async (context, next) => {
  const { action, setActionResult, serializeActionResult } = getActionContext(context);
  if (action?.calledFrom === 'form') {
    const result = await action.handler();
    // ... handle the action result
    setActionResult(action.name, serializeActionResult(result));
  }
  return next();
});
```

See the [advanced sessions guide](/en/guides/actions/#advanced-persist-action-results-with-a-session) for a sample implementation using Netlify Blob.

#### `serializeActionResult()`

[Section titled “serializeActionResult()”](#serializeactionresult)

**Type:** `(res: SafeResult) => SerializedActionResult`

Serializes an action result to JSON for persistence. This is required to properly handle non-JSON return values like `Map` or `Date` as well as the `ActionError` object.

Call this function when serializing an action result to be passed to `setActionResult()`:

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  const { action, setActionResult, serializeActionResult } = getActionContext(context);
  if (action) {
    const result = await action.handler();
    setActionResult(action.name, serializeActionResult(result));
  }
  // ...
});
```

#### `deserializeActionResult()`

[Section titled “deserializeActionResult()”](#deserializeactionresult)

**Type:** `(res: SerializedActionResult) => SafeResult`

Reverses the effect of [`serializeActionResult()`](#serializeactionresult) and returns an action result to its original state. This is useful to access the `data` and `error` objects on a serialized action result.

### `getActionPath()`

[Section titled “getActionPath()”](#getactionpath)

**Type:** `(action: ActionClient) => string`

**Added in:** `astro@5.1.0`

A utility that accepts an action and returns a URL path so you can execute an action call as a `fetch()` operation directly. This allows you to provide details such as custom headers when you call your action. Then, you can [handle the custom-formatted returned data](/en/guides/actions/#handling-returned-data) as needed, just as if you had called an action directly.

This example shows how to call a defined `like` action passing the `Authorization` header and the [`keepalive`](https://developer.mozilla.org/en-US/docs/Web/API/Request/keepalive) option:

src/components/my-component.astro

```astro
<script>
import { actions, getActionPath } from 'astro:actions'


await fetch(getActionPath(actions.like), {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Authorization: 'Bearer YOUR_TOKEN'
  },
  body: JSON.stringify({ id: 'YOUR_ID' }),
  keepalive: true
})
</script>
```

This example shows how to call the same `like` action using the [`sendBeacon`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon) API:

src/components/my-component.astro

```astro
<script>
import { actions, getActionPath } from 'astro:actions'


navigator.sendBeacon(
  getActionPath(actions.like),
  new Blob([JSON.stringify({ id: 'YOUR_ID' })], {
    type: 'application/json'
  })
)
</script>
```

### `ACTION_QUERY_PARAMS`

[Section titled “ACTION\_QUERY\_PARAMS”](#action_query_params)

**Type:** `{ actionName: string, actionPayload: string }`

An object containing the query parameter names used internally by Astro when handling form action submissions.

When you submit a form using an action, the following query parameters are added to the URL to track the action call:

* `actionName` - The query parameter that contains the name of the action being called
* `actionPayload` - The query parameter that contains the serialized form data

This constant can be useful when you need to clean up URLs after a form submission. For example, you might want to remove action-related query parameters during a redirect:

src/pages/api/contact.ts

```ts
import type { APIRoute } from "astro";
import { ACTION_QUERY_PARAMS } from 'astro:actions'


export const GET: APIRoute = ({ params, request }) => {
  const link = request.url.searchParams;
  link.delete(ACTION_QUERY_PARAMS.actionName);
  link.delete(ACTION_QUERY_PARAMS.actionPayload);


  return redirect(link, 303);
};
```

## `astro:actions` types

[Section titled “astro:actions types”](#astroactions-types)

```ts
import type {
  ActionAPIContext,
  ActionClient,
  ActionErrorCode,
  ActionInputSchema,
  ActionReturnType,
  SafeResult,
 } from 'astro:actions';
```

### `ActionAPIContext`

[Section titled “ActionAPIContext”](#actionapicontext)

A subset of the [Astro context object](/en/reference/api-reference/). The following properties are not available: `callAction`, `getActionResult`, `props`, and `redirect`.

### `ActionClient`

[Section titled “ActionClient”](#actionclient)

**Types:**

* `(input?: any) => Promise<SafeResult>`
* `{ queryString?: string; orThrow: (input?: any) => Promise<Awaited<TOutput>>; }`

Represents an action to be called on the client. You can use it as a function that accepts input data and returns a Promise with a [`SafeResult` object](#saferesult) containing the action result or validation errors.

The following example shows how you can provide error handling with an `if` statement when incrementing the like count fails:

src/pages/posts/post-1.astro

```astro
---
---


<!-- your template -->


<script>
import { actions } from 'astro:actions';


const post = document.querySelector('article');
const button = document.querySelector('button');
button?.addEventListener('click', async () => {
  const { data: updatedLikes, error } = await actions.likePost({ postId: post?.id });
  if (error) {
    /* handle errors */
  }
})
</script>
```

Alternatively, you can use it as an object giving you access to the `queryString` and an alternative `orThrow()` method.

#### `ActionClient.queryString`

[Section titled “ActionClient.queryString”](#actionclientquerystring)

**Type:** `string`

A string representation of the action that can be used to construct form action URLs. This can be useful when your form component is used in multiple places but you need to redirect to a different URL on submit.

The following example uses `queryString` to construct a URL that will be passed to the form `action` attribute through a custom prop:

src/pages/postal-service.astro

```astro
---
import { actions } from 'astro:actions';
import FeedbackForm from "../components/FeedbackForm.astro";


const feedbackUrl = new URL('/feedback', Astro.url);
feedbackUrl.search = actions.myAction.queryString;
---
<FeedbackForm sendTo={feedbackUrl.pathname} />
```

#### `ActionClient.orThrow()`

[Section titled “ActionClient.orThrow()”](#actionclientorthrow)

**Type:** `(input?: any) => Promise<Awaited<TOutput>>`

A method that throws an error on failure instead of returning the errors. This is useful when you want exceptions rather than error handling.

The following example uses `orThrow()` to skip error handling when incrementing the like count fails:

src/pages/posts/post-1.astro

```astro
---
---


<!-- your template -->


<script>
import { actions } from 'astro:actions';


const post = document.querySelector('article');
const button = document.querySelector('button');
button?.addEventListener('click', async () => {
  const updatedLikes = await actions.likePost.orThrow({ postId: post?.id });
})
</script>
```

### `ActionErrorCode`

[Section titled “ActionErrorCode”](#actionerrorcode)

**Type:** `string`

A union type of standard HTTP status codes [defined by IANA](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml) using the human-readable versions as uppercase strings separated by an underscore (e.g. `BAD_REQUEST` or `PAYLOAD_TOO_LARGE`).

### `ActionInputSchema`

[Section titled “ActionInputSchema”](#actioninputschema)

**Type:** `ZodType`

**Added in:** `astro@5.16.0`

A utility type that automatically infers the TypeScript type of an action’s input based on its Zod schema. This can be useful to reference an action’s [`input` validator type](#input-validator) as an object in your own type definitions.

Returns `never` when [`input` validator](#input-validator) is omitted.

The following example uses `ActionInputSchema` on an action named `contact` to:

* Retrieve the Zod schema type for the input of the action.
* Retrieve the expected input type of the action’s validator.

src/components/Form.astro

```astro
---
import { actions, ActionInputSchema } from 'astro:actions';
import { z } from 'astro/zod';


type ContactSchema = ActionInputSchema<typeof actions.contact>;
type ContactInput = z.input<ContactSchema>;
---
```

### `ActionReturnType`

[Section titled “ActionReturnType”](#actionreturntype)

**Type:** `Awaited<ReturnType<ActionHandler>>`

A utility type that extracts the output type from [an action handler](#defineaction). This unwraps both the `Promise` (if the handler is async) and the `ReturnType` to give you the [actual output type](#saferesult). This can be useful if you need to reference an action’s output type in your own type definitions.

The following example uses `ActionReturnType` to retrieve the expected output type for an action named `contact`:

src/components/Form.astro

```astro
---
import { actions, ActionReturnType } from 'astro:actions';


type ContactResult = ActionReturnType<typeof actions.contact>;
---
```

### `SafeResult`

[Section titled “SafeResult”](#saferesult)

**Type:** `{ data: TOutput, error: undefined } | { data: undefined, error: ActionError }`

Represents the result of an action call:

* on success, `data` contains the output of the action and `error` is `undefined`.
* on failure, `error` contains an [`ActionError`](#actionerror) with validation errors or runtime errors, and `data` is `undefined`.

# Adapter Server Entrypoint API Reference

This module helps adapter authors [build a server entrypoint](/en/reference/adapter-reference/#building-a-server-entrypoint) while supporting pages rendered in development mode or that have been prebuilt through `astro build`.

`astro/app` is used internally for [Astro’s official server adapters](/en/guides/on-demand-rendering/#server-adapters), and is also publicly available for you to build a custom adapter for your specific runtime or deploy host.

Publishing your adapter

Use the appropriate keywords when you [publish your adapter to NPM](/en/guides/integrations/#categories) so that your adapter will be listed in our [Integrations Directory](https://astro.build/integrations/).

Astro uses the standard [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) and [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) objects. Hosts using a different API for requests/responses should convert to these types in their adapter. For example, Astro exposes [helpers to work with NodeJS](#imports-from-astroappnode).

## Imports from `astro/app/entrypoint`

[Section titled “Imports from astro/app/entrypoint”](#imports-from-astroappentrypoint)

The following helpers are imported from the `entrypoint` directory in the app module:

```js
import {
  createApp
} from "astro/app/entrypoint";
```

### `createApp()`

[Section titled “createApp()”](#createapp)

**Type:** `(options?: { streaming: boolean }) => App`

**Added in:** `astro@6.0.0`

Returns an [`App` instance](#the-app-instance) that includes methods to work with standard Request and Response objects when [building an adapter’s server entrypoint](/en/reference/adapter-reference/#building-a-server-entrypoint).

```js
import { createApp } from "astro/app/entrypoint";
import http from "http";


const app = createApp();


addEventListener("fetch", event => {
  event.respondWith(
    app.render(event.request)
  );
});
```

#### Options

[Section titled “Options”](#options)

The `createApp()` function accepts the following options.

##### `options.streaming`

[Section titled “options.streaming”](#optionsstreaming)

**Type:** `boolean`\
**Default:** `true`

Defines whether HTML streaming is enabled. In most cases, disabling streaming is not recommended as it improves performance and generally provides a better visitor experience.

HTML streaming breaks a document into chunks to send over the network and render on the page in order. This normally results in visitors seeing your HTML as fast as possible, but factors such as network conditions and waiting for data fetches can block page rendering.

However, when you need to disable HTML streaming (e.g. your host only supports non-streamed HTML caching at the CDN level), you can opt out of the default behavior by passing `streaming: false` to `createApp()`:

```ts
import { createApp } from 'astro/app/entrypoint'


const app = createApp({ streaming: false })
```

#### The `App` instance

[Section titled “The App instance”](#the-app-instance)

The `createApp()` function returns a class instance with the following methods.

##### `app.render()`

[Section titled “app.render()”](#apprender)

**Type:** `(request: Request, options?: RenderOptions) => Promise<Response>`

Calls the Astro page that matches the [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request), renders it, and returns a promise to a [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) object. This also works for [API routes](/en/guides/endpoints/#server-endpoints-api-routes) that do not render pages.

```js
const response = await app.render(request);
```

##### `app.match()`

[Section titled “app.match()”](#appmatch)

**Type:** `(request: Request, allowPrerenderedRoutes = false) => RouteData | undefined`

Determines whether a request is matched by the Astro app’s routing rules.

```js
if(app.match(request)) {
  const response = await app.render(request);
}
```

You can usually call `app.render(request)` without using `.match` because Astro handles 404s if you provide a `404.astro` file. Use `app.match(request)` if you want to handle 404s in a different way.

By default, prerendered routes aren’t returned, even if they are matched. You can change this behavior by using `true` as the second argument.

##### `app.getAdapterLogger()`

[Section titled “app.getAdapterLogger()”](#appgetadapterlogger)

**Type:** `() => AstroIntegrationLogger`

**Added in:** `astro@v3.0.0`

Returns an [instance of the Astro logger](/en/reference/integrations-reference/#astrointegrationlogger) available to the adapter’s runtime environment.

```js
const logger = app.getAdapterLogger();
try {
  /* Some logic that can throw */
} catch {
  logger.error("Your custom error message using Astro logger.");
}
```

##### `app.getAllowedDomains()`

[Section titled “app.getAllowedDomains()”](#appgetalloweddomains)

**Type:** `() => Partial<RemotePattern>[] | undefined`

**Added in:** `astro@5.14.2`

Returns a list of permitted host patterns for incoming requests when using on-demand rendering [as configured in `security.allowedDomains`](/en/reference/configuration-reference/#securityalloweddomains).

##### `app.removeBase()`

[Section titled “app.removeBase()”](#appremovebase)

**Type:** `(pathname: string) => string`

**Added in:** `astro@1.6.4`

Removes the base from the given path. This is useful when you need to look up assets from the filesystem.

##### `app.setCookieHeaders()`

[Section titled “app.setCookieHeaders()”](#appsetcookieheaders)

**Type:** `(response: Response) => Generator<string, string[], any>`

**Added in:** `astro@1.4.0`

Returns a generator that yields individual cookie header values from a `Response` object. This is used to properly handle multiple cookies that may have been set during request processing.

The following example appends a `Set-Cookie` header for each header obtained from a response:

```js
for (const setCookieHeader of app.setCookieHeaders(response)) {
  response.headers.append('Set-Cookie', setCookieHeader);
}
```

## Imports from `astro/app/node`

[Section titled “Imports from astro/app/node”](#imports-from-astroappnode)

The following helpers are imported from the `node` directory in the app module:

```js
import {
  createRequest,
  writeResponse
} from "astro/app/node";
```

This module is used in conjunction with [the methods provided by `createApp()`](#createapp) to convert a [NodeJS `IncomingMessage`](https://nodejs.org/api/http.html#class-httpincomingmessage) into a web-standard `Request` and stream a web-standard `Response` into a [NodeJS `ServerResponse`](https://nodejs.org/api/http.html#class-httpserverresponse).

### `createRequest()`

[Section titled “createRequest()”](#createrequest)

**Type:** `(req: NodeRequest, options?: { skipBody?: boolean; allowedDomains?: Partial<RemotePattern>[]; }) => Request`

**Added in:** `astro@6.0.0`

Converts a NodeJS `IncomingMessage` into a standard `Request` object. An optional object can be passed as a second argument to further control how the request is created. This is useful if you want to ignore the body (defaults to `false`) or pass the configured [`allowedDomains`](/en/reference/configuration-reference/#securityalloweddomains) to the request.

The following example creates a `Request` and passes it to [`app.render()`](#apprender):

```js
import { createApp } from "astro/app/entrypoint";
import { createRequest } from "astro/app/node";
import { createServer } from "node:http";


const app = createApp();


const server = createServer(async (req, res) => {
  const request = createRequest(req);
  const response = await app.render(request);
})
```

### `writeResponse()`

[Section titled “writeResponse()”](#writeresponse)

**Type:** `(source: Response, destination: ServerResponse) => Promise<ServerResponse<IncomingMessage> | undefined>`

**Added in:** `astro@6.0.0`

Streams a web-standard `Response` into a NodeJS server response. This function takes a `Response` object and the initial `ServerResponse` before returning a promise of a `ServerResponse` object.

The following example creates a `Request`, passes it to [`app.render()`](#apprender), and writes the response:

```js
import { createApp } from "astro/app/entrypoint";
import { createRequest, writeResponse } from "astro/app/node";
import { createServer } from "node:http";


const app = createApp();


const server = createServer(async (req, res) => {
  const request = createRequest(req);
  const response = await app.render(request);
  await writeResponse(response, res);
})
```

## `astro/app` types

[Section titled “astro/app types”](#astroapp-types)

The following types are imported from the app module:

```ts
import type {
  RenderOptions,
} from "astro/app";
```

### `RenderOptions`

[Section titled “RenderOptions”](#renderoptions)

**Type:** `{addCookieHeader?: boolean; clientAddress?: string; locals?: object; prerenderedErrorPageFetch?: (url: ErrorPagePath) => Promise<Response>; routeData?: RouteData;}`

Describes the options for controlling the routes rendering.

#### `RenderOptions.addCookieHeader`

[Section titled “RenderOptions.addCookieHeader”](#renderoptionsaddcookieheader)

**Type:** `boolean`\
**Default:** `false`

Whether or not to automatically add all cookies written by [`Astro.cookie.set()`](/en/reference/api-reference/#cookiesset) to the response headers.

When set to `true`, they will be added to the `Set-Cookie` header of the response as comma-separated key-value pairs. You can use the standard `response.headers.getSetCookie()` API to read them individually.

```js
const response = await app.render(request, { addCookieHeader: true });
```

#### `RenderOptions.clientAddress`

[Section titled “RenderOptions.clientAddress”](#renderoptionsclientaddress)

**Type:** `string`\
**Default:** `request[Symbol.for("astro.clientAddress")]`

The client IP address that will be made available as [`Astro.clientAddress`](/en/reference/api-reference/#clientaddress) in pages, and as `ctx.clientAddress` in API routes and middleware.

The example below reads the `x-forwarded-for` header and passes it as `clientAddress`. This value becomes available to the user as `Astro.clientAddress`.

```js
const clientAddress = request.headers.get("x-forwarded-for");
const response = await app.render(request, { clientAddress });
```

#### `RenderOptions.locals`

[Section titled “RenderOptions.locals”](#renderoptionslocals)

**Type:** `object`

The [`context.locals` object](/en/reference/api-reference/#locals) used to store and access information during the lifecycle of a request.

The example below reads a header named `x-private-header`, attempts to parse it as an object, and passes it to `locals`, which can then be passed to any [middleware function](/en/guides/middleware/).

```js
const privateHeader = request.headers.get("x-private-header");
let locals = {};
try {
  if (privateHeader) {
    locals = JSON.parse(privateHeader);
  }
} finally {
  const response = await app.render(request, { locals });
}
```

#### `RenderOptions.prerenderedErrorPageFetch()`

[Section titled “RenderOptions.prerenderedErrorPageFetch()”](#renderoptionsprerenderederrorpagefetch)

**Type:** `(url: ErrorPagePath) => Promise<Response>`\
**Default:** `fetch`

**Added in:** `astro@5.6.0`

A function that allows you to provide custom implementations for fetching prerendered error pages.

This is used to override the default `fetch()` behavior, for example, when `fetch()` is unavailable or when you cannot call the server from itself.

The following example reads `500.html` and `404.html` from disk instead of performing an HTTP call:

```ts
return app.render(request, {
  prerenderedErrorPageFetch: async (url: string): Promise<Response> => {
    if (url.includes("/500")) {
      const content = await fs.promises.readFile("500.html", "utf-8");
      return new Response(content, {
        status: 500,
        headers: { "Content-Type": "text/html" },
      });
    }


    const content = await fs.promises.readFile("404.html", "utf-8");
    return new Response(content, {
      status: 404,
      headers: { "Content-Type": "text/html" },
    });
  }
});
```

If not provided, Astro will fallback to its default behavior for fetching error pages.

#### `RenderOptions.routeData`

[Section titled “RenderOptions.routeData”](#renderoptionsroutedata)

**Type:** [`RouteData`](/en/reference/integrations-reference/#routedata)\
**Default:** `app.match(request)`

Defines the information about a route. This is useful when you already know the route to render. Doing so will bypass the internal call to [`app.match()`](#appmatch) to determine the route to render.

```js
const routeData = app.match(request);
if (routeData) {
  return app.render(request, { routeData });
} else {
  /* adapter-specific 404 response */
  return new Response(..., { status: 404 });
}
```

# Image and Assets API Reference

**Added in:** `astro@3.0.0`

Astro provides built-in components and helper functions for optimizing and displaying your images. For features and usage examples, [see our image guide](/en/guides/images/).

## Imports from `astro:assets`

[Section titled “Imports from astro:assets”](#imports-from-astroassets)

The following helpers are imported from the virtual assets module:

```js
import {
  Image,
  Picture,
  Font,
  getImage,
  inferRemoteSize,
  getConfiguredImageService,
  imageConfig,
  fontData,
} from 'astro:assets';
```

### `<Image />`

[Section titled “\<Image />”](#image-)

The `<Image />` component optimizes and transforms images.

src/components/MyComponent.astro

```astro
---
// import the Image component and the image
import { Image } from 'astro:assets';
import myImage from "../assets/my_image.png"; // Image is 1600x900
---


<!-- `alt` is mandatory on the Image component -->
<Image src={myImage} alt="A description of my image." />
```

```html
<!-- Output -->
<!-- Image is optimized, proper attributes are enforced -->
<img
  src="/_astro/my_image.hash.webp"
  width="1600"
  height="900"
  decoding="async"
  loading="lazy"
  alt="A description of my image."
/>
```

The `<Image />` component accepts the following listed properties in addition to all properties accepted by the HTML `<img>` tag.

#### `src` (required)

[Section titled “src (required)”](#src-required)

**Type:** `ImageMetadata | string | Promise<{ default: ImageMetadata }>`

The format of the `src` value of your image file depends on where your image file is located:

* **Local images in `src/`** - you must **also import the image** using a relative file path or configure and use an [import alias](/en/guides/imports/#aliases). Then use the import name as the `src` value:

  src/pages/index.astro

  ```astro
  ---
  import { Image } from 'astro:assets';
  import myImportedImage from '../assets/my-local-image.png';
  ---
  <Image src={myImportedImage} alt="descriptive text" />
  ```

* **Images in the `public/` folder** - use the image’s **file path relative to the public folder**:

  src/pages/index.astro

  ```astro
  ---
  import { Image } from 'astro:assets';
  ---
  <Image
    src="/images/my-public-image.png"
    alt="descriptive text"
    width="200"
    height="150"
  />
  ```

* **Remote images** - use the image’s **full URL** as the property value:

  src/pages/index.astro

  ```astro
  ---
  import { Image } from 'astro:assets';
  ---
  <Image
    src="https://example.com/remote-image.jpg"
    alt="descriptive text"
    width="200"
    height="150"
  />
  ```

#### `alt` (required)

[Section titled “alt (required)”](#alt-required)

**Type:** `string`

Use the required `alt` attribute to provide a string of [descriptive alt text](https://www.w3.org/WAI/tutorials/images/) for images.

If an image is merely decorative (i.e. doesn’t contribute to the understanding of the page), set `alt=""` so that screen readers and other assistive technologies know to ignore the image.

#### `width` and `height` (required for images in `public/`)

[Section titled “width and height (required for images in public/)”](#width-and-height-required-for-images-in-public)

**Type:** ``number | `${number}` | undefined``

These properties define the dimensions to use for the image.

When a `layout` type is set, these are automatically generated based on the image’s dimensions and in most cases should not be set manually.

When using images in their original aspect ratio, `width` and `height` are optional. These dimensions can be automatically inferred from image files located in `src/`. For remote images, add [the `inferSize` attribute set to `true`](#infersize) on the `<Image />` or `<Picture />` component or use [`inferRemoteSize()` function](#inferremotesize).

However, both of these properties are required for images stored in your `public/` folder as Astro is unable to analyze these files.

#### `densities`

[Section titled “densities”](#densities)

**Type:** ``(number | `${number}x`)[] | undefined``

**Added in:** `astro@3.3.0`

A list of pixel densities to generate for the image.

The `densities` attribute is not compatible with having the `layout` prop or `image.layout` config set, and will be ignored if set.

If provided, this value will be used to generate a `srcset` attribute on the `<img>` tag. Do not provide a value for `widths` when using this value.

Densities that are equal to widths larger than the original image will be ignored to avoid upscaling the image.

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---
<Image
  src={myImage}
  width={myImage.width / 2}
  densities={[1.5, 2]}
  alt="A description of my image."
/>
```

```html
<!-- Output -->
<img
  src="/_astro/my_image.hash.webp"
  srcset="
    /_astro/my_image.hash.webp 1.5x
    /_astro/my_image.hash.webp 2x
  "
  alt="A description of my image."
  width="800"
  height="450"
  loading="lazy"
  decoding="async"
/>
```

#### `widths`

[Section titled “widths”](#widths)

**Type:** `number[] | undefined`

**Added in:** `astro@3.3.0`

A list of widths to generate for the image.

If provided, this value will be used to generate a `srcset` attribute on the `<img>` tag. A [`sizes` property](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/sizes) must also be provided.

The `widths` and `sizes` attributes will be automatically generated for images using a `layout` property. Providing these values is generally not needed, but can be used to override any automatically generated values.

Do not provide a value for `densities` when using this value. Only one of these two values can be used to generate a `srcset`.

Widths that are larger than the original image will be ignored to avoid upscaling the image.

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png'; // Image is 1600x900
---
<Image
  src={myImage}
  widths={[240, 540, 720, myImage.width]}
  sizes={`(max-width: 360px) 240px, (max-width: 720px) 540px, (max-width: 1600px) 720px, ${myImage.width}px`}
  alt="A description of my image."
/>
```

```html
<!-- Output -->
<img
  src="/_astro/my_image.hash.webp"
  srcset="
    /_astro/my_image.hash.webp 240w,
    /_astro/my_image.hash.webp 540w,
    /_astro/my_image.hash.webp 720w,
    /_astro/my_image.hash.webp 1600w
  "
  sizes="
    (max-width: 360px) 240px,
    (max-width: 720px) 540px,
    (max-width: 1600px) 720px,
    1600px
  "
  alt="A description of my image."
  width="1600"
  height="900"
  loading="lazy"
  decoding="async"
/>
```

#### `sizes`

[Section titled “sizes”](#sizes)

**Type:** `string | undefined`

**Added in:** `astro@3.3.0`

Specifies the layout width of the image for each of a list of media conditions. Must be provided when specifying `widths`.

The `widths` and `sizes` attributes will be automatically generated for images using a `layout` property. Providing these values is generally not needed, but can be used to override any automatically generated values.

The generated `sizes` attribute for `constrained` and `full-width` images is based on the assumption that the image is displayed close to the full width of the screen when the viewport is smaller than the image’s width. If it is significantly different (e.g. if it’s in a multi-column layout on small screens), you may need to adjust the `sizes` attribute manually for best results.

#### `format`

[Section titled “format”](#format)

**Type:** `ImageOutputFormat | undefined`

You can optionally state the [image file type](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#common_image_file_types) output to be used.

By default, the `<Image />` component will produce a `.webp` file.

#### `quality`

[Section titled “quality”](#quality)

**Type:** `ImageQuality | undefined`

`quality` is an optional property that can either be:

* a preset (`low`, `mid`, `high`, `max`) that is automatically normalized between formats.
* a number from `0` to `100` (interpreted differently between formats).

#### `inferSize`

[Section titled “inferSize”](#infersize)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@4.4.0`

Allows you to set the original `width` and `height` of a remote image automatically.

By default, this value is set to `false` and you must manually specify both dimensions for your remote image.

Add `inferSize` to the `<Image />` component (or `inferSize: true` to `getImage()`) to infer these values from the image content when fetched. This is helpful if you don’t know the dimensions of the remote image, or if they might change:

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
---
<Image src="https://example.com/cat.png" inferSize alt="A cat sleeping in the sun." />
```

As of Astro 5.17.3, `inferSize` only fetches dimensions for [authorized remote image domains](/en/guides/images/#authorizing-remote-images). Remote images outside the allowlist are not fetched.

#### `priority`

[Section titled “priority”](#priority)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@5.10.0`

Allows you to automatically set the `loading`, `decoding`, and `fetchpriority` attributes to their optimal values for above-the-fold images.

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---
<Image src={myImage} priority alt="A description of my image" />
```

When `priority="true"` (or the shorthand syntax `priority`) is added to the `<Image />` or `<Picture />` component, it will add the following attributes to instruct the browser to load the image immediately:

```html
loading="eager"
decoding="sync"
fetchpriority="high"
```

These individual attributes can still be set manually if you need to customize them further.

#### `layout`

[Section titled “layout”](#layout)

**Type:** `'constrained' | 'full-width' | 'fixed' | 'none'`\
**Default:** `image.layout | 'none'`

**Added in:** `astro@5.10.0`

Determines how the image should resize when its container changes size. Can be used to override the default configured value for [`image.layout`](/en/reference/configuration-reference/#imagelayout).

MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---
<Image src={myImage} alt="A description of my image." layout='constrained' width={800} height={600} />
```

When a layout is set, `srcset` and `sizes` attributes are automatically generated based on the image’s dimensions and the layout type. The previous `<Image />` component will generate the following HTML output:

```html
<img
  src="/_astro/my_image.hash3.webp"
  srcset="/_astro/my_image.hash1.webp 640w,
      /_astro/my_image.hash2.webp 750w,
      /_astro/my_image.hash3.webp 800w,
      /_astro/my_image.hash4.webp 828w,
      /_astro/my_image.hash5.webp 1080w,
      /_astro/my_image.hash6.webp 1280w,
      /_astro/my_image.hash7.webp 1600w"
  alt="A description of my image"
  sizes="(min-width: 800px) 800px, 100vw"
  loading="lazy"
  decoding="async"
  fetchpriority="auto"
  width="800"
  height="600"
  style="--fit: cover; --pos: center;"
  data-astro-image="constrained"
>
```

`layout` supports the following values:

* `constrained` - The image will scale down to fit the container, maintaining its aspect ratio, but will not scale up beyond the specified `width` and `height`, or the image’s original dimensions.

  Use this if you want the image to display at the requested size where possible, but shrink to fit smaller screens. This matches the default behavior for images when using Tailwind. If you’re not sure, this is probably the layout you should choose.

* `full-width` - The image will scale to fit the width of the container, maintaining its aspect ratio.

  Use this for hero images or other images that should take up the full width of the page.

* `fixed` - The image will maintain the requested dimensions and not resize. It will generate a `srcset` to support high density displays, but not for different screen sizes.

  Use this if the image will not resize, for example icons or logos smaller than any screen width, or other images in a fixed-width container.

* `none` - The image will not be responsive. No `srcset` or `sizes` will be automatically generated, and no styles will be applied.

  This is useful if you have enabled a default layout, but want to disable it for a specific image.

For example, with `constrained` set as the default layout, you can override any individual image’s `layout` property:

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---
<Image src={myImage} alt="This will use constrained layout" width={800} height={600} />
<Image src={myImage} alt="This will use full-width layout" layout="full-width" />
<Image src={myImage} alt="This will disable responsive images" layout="none" />
```

The value for `layout` also defines the default styles applied to the `<img>` tag to determine how the image should resize according to its container:

Responsive Image Styles

```css
:where([data-astro-image]) {
  object-fit: var(--fit);
  object-position: var(--pos);
}
:where([data-astro-image='full-width']) {
  width: 100%;
}
:where([data-astro-image='constrained']) {
  max-width: 100%;
}
```

#### `fit`

[Section titled “fit”](#fit)

**Type:** `'contain' | 'cover' | 'fill' | 'none' | 'scale-down'`\
**Default:** `image.objectFit | 'cover'`

**Added in:** `astro@5.10.0`

Defines how a image should be cropped if its aspect ratio is changed.

Values match those of CSS `object-fit`. Defaults to `cover`, or the value of [`image.objectFit`](/en/reference/configuration-reference/#imageobjectfit) if set. Can be used to override the default `object-fit` styles.

#### `position`

[Section titled “position”](#position)

**Type:** `string`\
**Default:** `image.objectPosition | 'center'`

**Added in:** `astro@5.10.0`

Defines the position of the image crop for a image if the aspect ratio is changed.

Values match those of CSS `object-position`. Defaults to `center`, or the value of [`image.objectPosition`](/en/reference/configuration-reference/#imageobjectposition) if set. Can be used to override the default `object-position` styles.

#### `background`

[Section titled “background”](#background)

**Type:** `string | undefined`

**Added in:** `astro@5.17.0`

The background color to use when flattening an image to transform it into the requested output `format`.

By default, Sharp uses a black background when flattening an image. Specifying a different background color is especially useful when transforming images with transparent backgrounds to a format that does not support transparency (e.g. `.jpeg`):

src/components/MyComponent.astro

```astro
<Image
  src={myImage}
  alt="A description of my image"
  format="jpeg"
  background="#ffffff"
/>
```

Values are passed directly to the image service. Sharp accepts [any value the `color-string` package can parse](https://github.com/Qix-/color-string/blob/master/README.md#parsing).

### `<Picture />`

[Section titled “\<Picture />”](#picture-)

**Added in:** `astro@3.3.0`

The `<Picture />` component generates an optimized image with multiple formats and/or sizes.

src/pages/index.astro

```astro
---
import { Picture } from 'astro:assets';
import myImage from "../assets/my_image.png"; // Image is 1600x900
---


<!-- `alt` is mandatory on the Picture component -->
<Picture src={myImage} formats={['avif', 'webp']} alt="A description of my image." />
```

```html
<!-- Output -->
<picture>
  <source srcset="/_astro/my_image.hash.avif" type="image/avif" />
  <source srcset="/_astro/my_image.hash.webp" type="image/webp" />
  <img
    src="/_astro/my_image.hash.png"
    width="1600"
    height="900"
    decoding="async"
    loading="lazy"
    alt="A description of my image."
  />
</picture>
```

`<Picture />` accepts all the properties of [the `<Image />` component](#image-) plus the following:

#### `formats`

[Section titled “formats”](#formats)

**Type:** `ImageOutputFormat[]`

An array of image formats to use for the `<source>` tags. Entries will be added as `<source>` elements in the order they are listed, and this order determines which format is displayed. For the best performance, list the most modern format first (e.g. `webp` or `avif`). By default, this is set to `['webp']`.

#### `fallbackFormat`

[Section titled “fallbackFormat”](#fallbackformat)

**Type:** [`ImageOutputFormat`](#imageoutputformat)

Format to use as a fallback value for the `<img>` tag. Defaults to `.png` for static images (or `.jpg` if the image is a JPG), `.gif` for animated images, and `.svg` for SVG files.

#### `pictureAttributes`

[Section titled “pictureAttributes”](#pictureattributes)

**Type:** `HTMLAttributes<'picture'>`

An object of attributes to be added to the `<picture>` tag.

Use this property to apply attributes to the outer `<picture>` element itself. Attributes applied to the `<Picture />` component directly will apply to the inner `<img>` element, except for those used for image transformation.

src/components/MyComponent.astro

```astro
---
import { Picture } from "astro:assets";
import myImage from "../my_image.png"; // Image is 1600x900
---


<Picture
  src={myImage}
  alt="A description of my image."
  pictureAttributes={{ style: "background-color: red;" }}
/>
```

```html
<!-- Output -->
<picture style="background-color: red;">
  <source srcset="/_astro/my_image.hash.webp" type="image/webp" />
  <img
    src="/_astro/my_image.hash.png"
    alt="A description of my image."
    width="1600"
    height="900"
    loading="lazy"
    decoding="async"
  />
</picture>
```

### `<Font />`

[Section titled “\<Font />”](#font-)

**Added in:** `astro@6.0.0`

The `<Font />` component outputs style tags and can optionally output preload links for a given font family.

It must be imported and added to your page `<head>`. This is commonly done in a component such as `Head.astro` that is used in a common site layout for global use but may be added to individual pages as needed.

With this component, you have control over which font family is used on which page, and which fonts are preloaded.

src/components/Head.astro

```astro
---
import { Font } from "astro:assets";
---


<Font cssVariable="--font-roboto" />
```

The `<Font />` component accepts the following properties:

#### `cssVariable` (required)

[Section titled “cssVariable (required)”](#cssvariable-required)

**Type:** `CssVariable`\
**Example type:** `"--font-roboto" | "--font-comic-sans" | ...`

The [`cssVariable`](/en/reference/configuration-reference/#fontcssvariable) registered in your Astro configuration:

src/components/Head.astro

```astro
---
import { Font } from "astro:assets";
---


<Font cssVariable="--font-roboto" />
```

#### `preload`

[Section titled “preload”](#preload)

**Type:** `boolean | { weight?: string | number; style?: string; subset?: string }[]`\
**Default:** `false`

Whether to output [preload links](https://web.dev/learn/performance/optimize-web-fonts#preload) or not. With the `preload` directive, the browser will immediately begin downloading all possible font links during page load:

src/components/Head.astro

```astro
---
import { Font } from "astro:assets";
---


<Font cssVariable="--font-roboto" preload />
```

Be very intentional about which fonts you preload. Preloading too many fonts can impact performance, as this can block loading other important resources or may download fonts that are not needed for the current page.

To selectively control which font files are preloaded, you can provide an array of objects describing any combination of font `weight`, `style`, or `subset` to preload:

src/components/Head.astro

```astro
---
import { Font } from "astro:assets";
---


<Font
  cssVariable="--font-roboto"
  preload={[
    { subset: "latin", style: "normal" },
    { weight: "400" },
  ]}
/>
```

Variable weight font files will be preloaded if any weight within its range is requested. For example, a font file for font weight `100 900` will be included when `400` is specified in a `preload` object.

### `getImage()`

[Section titled “getImage()”](#getimage)

**Type:** `(options: UnresolvedImageTransform) => Promise<GetImageResult>`

Caution

`getImage()` relies on server-only APIs and will throw an error on the client.

If you need the resulting image URL client-side, you can [pass the `src` from a server-rendered `getImage()` call to the client](/en/guides/images/#generating-images-with-getimage).

The `getImage()` function is intended for generating images destined to be used somewhere else than directly in HTML, for example in an [API Route](/en/guides/endpoints/#server-endpoints-api-routes). It also allows you to create your own custom `<Image />` component.

This takes an options object with the [same properties as the Image component](#image-) (except `alt`) and returns a [`GetImageResult` object](#getimageresult).

The following example generates an AVIF `background-image` for a `<div />`:

src/components/Background.astro

```astro
---
import { getImage } from "astro:assets";
import myBackground from "../background.png"


const optimizedBackground = await getImage({src: myBackground, format: 'avif'})
---


<div style={`background-image: url(${optimizedBackground.src});`}><slot /></div>
```

### `inferRemoteSize()`

[Section titled “inferRemoteSize()”](#inferremotesize)

**Type:** `(url: string) => Promise<Omit<ImageMetadata, ‘src’ | ‘fsPath’>>`

**Added in:** `astro@4.12.0`

A function to set the original `width` and `height` of a remote image automatically. This can be used as an alternative to passing the [`inferSize` ](#infersize)property.

```ts
import { inferRemoteSize } from 'astro:assets';
const { width, height } = await inferRemoteSize("https://example.com/cat.png");
```

### `getConfiguredImageService()`

[Section titled “getConfiguredImageService()”](#getconfiguredimageservice)

**Type:** `() => Promise<ImageService>`

**Added in:** `astro@2.1.3`

Retrieves the resolved [image service](/en/reference/configuration-reference/#imageservice).

### `imageConfig`

[Section titled “imageConfig”](#imageconfig)

**Type:** [`AstroConfig["image"]`](/en/reference/configuration-reference/#image-options)

**Added in:** `astro@3.0.9`

The [configuration options for images](/en/reference/configuration-reference/#image-options) set by the user and merged with all defaults.

### `fontData`

[Section titled “fontData”](#fontdata)

**Type:** `Record<CssVariable, Array<FontData>>`

**Added in:** `astro@6.0.0`

An object where each key is a [`cssVariable`](/en/reference/configuration-reference/#fontcssvariable) and the value is an array describing the associated fonts. Each font is an object containing an array of `src` available for that font and the following optional properties: `weight` and `style`:

```ts
import { fontData } from "astro:assets"


const data = fontData["--font-roboto"]
```

## `astro:assets` types

[Section titled “astro:assets types”](#astroassets-types)

The following types are imported from the virtual assets module:

```ts
import type {
  LocalImageProps,
  RemoteImageProps,
  FontData
} from "astro/assets";
```

### `LocalImageProps`

[Section titled “LocalImageProps”](#localimageprops)

**Type:** `ImageSharedProps<T> & { src: ImageMetadata | Promise<{ default: ImageMetadata; }> }`

Describes the [properties of a local image](#image-). This ensures that [`src`](#src-required) matches the shape of an imported image.

Learn more about [imported images in `src/`](/en/guides/images/#images-in-src) with an example usage.

### `RemoteImageProps`

[Section titled “RemoteImageProps”](#remoteimageprops)

**Types:**

* `ImageSharedProps<T> & { src: string; inferSize: true; }`
* `ImageSharedProps<T> & { src: string; inferSize?: false | undefined; }`

Describes the [properties of a remote image](#image-). This ensures that when [`inferSize`](#infersize) is not provided or is set to `false`, both [`width` and `height`](#width-and-height-required-for-images-in-public) are required.

### `FontData`

[Section titled “FontData”](#fontdata-1)

**Type:** `{ src: Array<{ url: string; format?: string; tech?: string }>; weight?: string; style?: string; }`

**Added in:** `astro@6.0.0`

Describes the font data associated with a given font family.

## Imports from `astro/assets`

[Section titled “Imports from astro/assets”](#imports-from-astroassets-1)

The following helpers are imported from the regular assets module:

```ts
import {
  baseService,
  getConfiguredImageService,
  getImage,
  isLocalService,
} from "astro/assets";
```

### `baseService`

[Section titled “baseService”](#baseservice)

**Type:** `Omit<LocalImageService, ‘transform’>`

The built-in local image service which can be extended to [create a custom image service](/en/reference/image-service-reference/).

The following example reuses the `baseService` to create a new image service:

src/image-service.ts

```ts
import { baseService } from "astro/assets";


const newImageService = {
 getURL: baseService.getURL,
 parseURL: baseService.parseURL,
 getHTMLAttributes: baseService.getHTMLAttributes,
 async transform(inputBuffer, transformOptions) {...}
}
```

### `getConfiguredImageService()`

[Section titled “getConfiguredImageService()”](#getconfiguredimageservice-1)

See [`getConfiguredImageService()`](#getconfiguredimageservice) from `astro:assets`.

### `getImage()`

[Section titled “getImage()”](#getimage-1)

**Type:** `(options: UnresolvedImageTransform, imageConfig: AstroConfig[‘image’]) => Promise<GetImageResult>`

A function similar to [`getImage()`](#getimage) from `astro:assets` with two required arguments: an `options` object with [the same properties as the Image component](#image-) and a second object for the [image configuration](/en/reference/configuration-reference/#image-options).

### `isLocalService()`

[Section titled “isLocalService()”](#islocalservice)

**Type:** `(service: ImageService | undefined) => boolean`

Checks the type of an image service and returns `true` when this is a [local service](#localimageservice).

## `astro/assets` types

[Section titled “astro/assets types”](#astroassets-types-1)

The following types are imported from the regular assets module:

```ts
import type {
  LocalImageProps,
  RemoteImageProps,
} from "astro/assets";
```

### `LocalImageProps`

[Section titled “LocalImageProps”](#localimageprops-1)

See [`LocalImageProps`](#localimageprops) from `astro:assets`.

### `RemoteImageProps`

[Section titled “RemoteImageProps”](#remoteimageprops-1)

See [`RemoteImageProps`](#remoteimageprops) from `astro:assets`.

## Imports from `astro/assets/utils`

[Section titled “Imports from astro/assets/utils”](#imports-from-astroassetsutils)

The following helpers are imported from the `utils` directory in the regular assets module and can be used to [build an image service](/en/reference/image-service-reference/):

```ts
import {
  isRemoteAllowed,
  matchHostname,
  matchPathname,
  matchPattern,
  matchPort,
  matchProtocol,
  isESMImportedImage,
  isRemoteImage,
  resolveSrc,
  imageMetadata,
  emitImageMetadata,
  emitClientAsset,
  getOrigQueryParams,
  inferRemoteSize,
  propsToFilename,
  hashTransform,
} from "astro/assets/utils";
```

### `isRemoteAllowed()`

[Section titled “isRemoteAllowed()”](#isremoteallowed)

**Type:** `(src: string, { domains, remotePatterns }: { domains: string[], remotePatterns: RemotePattern[] }) => boolean`

**Added in:** `astro@4.0.0`

Determines whether a given remote resource, identified by its source URL, is allowed based on specified domains and remote patterns.

```ts
import { isRemoteAllowed } from 'astro/assets/utils';


const url = new URL('https://example.com/images/test.jpg');
const domains = ['example.com', 'anotherdomain.com'];
const remotePatterns = [
  {
    protocol: 'https',
    hostname: 'images.example.com',
    pathname: '/**', // Allow any path under this hostname
  }
];


isRemoteAllowed(url.href, { domains, remotePatterns }); // Output: `true`
```

### `matchHostname()`

[Section titled “matchHostname()”](#matchhostname)

**Type:** `(url: URL, hostname?: string, allowWildcard = false) => boolean`

**Added in:** `astro@4.0.0`

Matches a given URL’s hostname against a specified hostname, with optional support for wildcard patterns.

```ts
import { matchHostname } from 'astro/assets/utils';


const url = new URL('https://sub.example.com/path/to/resource');


matchHostname(url, 'example.com'); // Output: `false`
matchHostname(url, 'example.com', true); // Output: `true`
```

### `matchPathname()`

[Section titled “matchPathname()”](#matchpathname)

**Type:** `(url: URL, pathname?: string, allowWildcard = false) => boolean`

**Added in:** `astro@4.0.0`

Matches a given URL’s pathname against a specified pattern, with optional support for wildcards.

```ts
import { matchPathname } from 'astro/assets/utils';


const testURL = new URL('https://example.com/images/photo.jpg');


matchPathname(testURL, '/images/photo.jpg'); // Output: `true`
matchPathname(testURL, '/images/'); // Output: `false`
matchPathname(testURL, '/images/*', true); // Output: `true`
```

### `matchPattern()`

[Section titled “matchPattern()”](#matchpattern)

**Type:** `(url: URL, remotePattern: RemotePattern) => boolean`

**Added in:** `astro@4.0.0`

Evaluates whether a given URL matches the specified remote pattern based on protocol, hostname, port, and pathname.

```ts
import { matchPattern } from 'astro/assets/utils';


const url = new URL('https://images.example.com/photos/test.jpg');
const remotePattern = {
  protocol: 'https',
  hostname: 'images.example.com',
  pathname: '/photos/**', // Allow all files under /photos/
};


matchPattern(url, remotePattern); // Output: `true`
```

### `matchPort()`

[Section titled “matchPort()”](#matchport)

**Type:** `(url: URL, port?: string) => boolean`\
**Default:** `true`

**Added in:** `astro@4.0.0`

Checks if the given URL’s port matches the specified port. If no port is provided, it returns `true`.

```ts
import { matchPort } from 'astro/assets/utils';


const urlWithPort = new URL('https://example.com:8080/resource');
const urlWithoutPort = new URL('https://example.com/resource');


matchPort(urlWithPort, '8080'); // Output: `true`
matchPort(urlWithoutPort, '8080'); // Output: `false`
```

### `matchProtocol()`

[Section titled “matchProtocol()”](#matchprotocol)

**Type:** `(url: URL, protocol?: string) => boolean`\
**Default:** `true`

**Added in:** `astro@4.0.0`

Compares the protocol of the provided URL with a specified protocol. This returns `true` if the protocol matches or if no protocol is provided.

```ts
import { matchProtocol } from 'astro/assets/utils';


const secureUrl = new URL('https://example.com/resource');
const regularUrl = new URL('http://example.com/resource');


matchProtocol(secureUrl, 'https'); // Output: `true`
matchProtocol(regularUrl, 'https'); // Output: `false`
```

### `isESMImportedImage()`

[Section titled “isESMImportedImage()”](#isesmimportedimage)

**Type:** `(src: ImageMetadata | string) => boolean`

**Added in:** `astro@4.0.0`

Determines if the given source is an ECMAScript Module (ESM) imported image.

```ts
import { isESMImportedImage } from 'astro/assets/utils';


const imageMetadata = {
  src: '/images/photo.jpg',
  width: 800,
  height: 600,
  format: 'jpg',
};
const filePath = '/images/photo.jpg';


isESMImportedImage(imageMetadata); // Output: `true`
isESMImportedImage(filePath); // Output: `false`
```

### `isRemoteImage()`

[Section titled “isRemoteImage()”](#isremoteimage)

**Type:** `(src: ImageMetadata | string) => boolean`

**Added in:** `astro@4.0.0`

Determines if the provided source is a remote image URL in the form of a string.

```ts
import { isRemoteImage } from 'astro/assets/utils';


const imageUrl = 'https://example.com/images/photo.jpg';
const localImage = {
  src: '/images/photo.jpg',
  width: 800,
  height: 600,
  format: 'jpg',
};


isRemoteImage(imageUrl); // Output: `true`
isRemoteImage(localImage); // Output: `false`
```

### `resolveSrc()`

[Section titled “resolveSrc()”](#resolvesrc)

**Type:** `(src: UnresolvedImageTransform[‘src’]) => Promise<string | ImageMetadata>`

**Added in:** `astro@4.0.0`

Returns the image source. This function ensures that if `src` is a Promise (e.g., a dynamic `import()`), it is awaited and the correct `src` is extracted. If `src` is already a resolved value, it is returned as-is.

```ts
import { resolveSrc } from 'astro/assets/utils';
import localImage from "./images/photo.jpg";


const resolvedLocal = await resolveSrc(localImage);
// Example value: `{ src: '/@fs/home/username/dev/astro-project/src/images/photo.jpg', width: 800, height: 600, format: 'jpg' }`


const resolvedRemote = await resolveSrc("https://example.com/remote-img.jpg");
// Value: `"https://example.com/remote-img.jpg"`


const resolvedDynamic = await resolveSrc(import("./images/dynamic-image.jpg"))
// Example value: `{ src: '/@fs/home/username/dev/astro-project/src/images/dynamic-image.jpg', width: 800, height: 600, format: 'jpg' }`
```

### `imageMetadata()`

[Section titled “imageMetadata()”](#imagemetadata)

**Type:** `(data: Uint8Array, src?: string) => Promise<Omit<ImageMetadata, ‘src’ | ‘fsPath’>>`

**Added in:** `astro@4.0.0`

Extracts image metadata such as dimensions, format, and orientation from the provided image data.

```ts
import { imageMetadata } from 'astro/assets/utils';


const binaryImage = new Uint8Array([/* ...binary image data... */]);
const sourcePath = '/images/photo.jpg';


const metadata = await imageMetadata(binaryImage, sourcePath);
// Example value:
// {
//    width: 800,
//    height: 600,
//    format: 'jpg',
//    orientation: undefined
// }
```

### `emitImageMetadata()`

[Section titled “emitImageMetadata()”](#emitimagemetadata)

**Type:** `(id: string | undefined, fileEmitter?: Rollup.EmitFile) => Promise<(ImageMetadata & { contents?: Buffer }) | undefined>`

**Added in:** `astro@5.7.0`

Processes an image file and emits its metadata and optionally its contents. In build mode, the function uses `fileEmitter` to generate an asset reference. In development mode, it resolves to a local file URL with query parameters for metadata.

```ts
import { emitImageMetadata } from 'astro/assets/utils';


const imageId = '/images/photo.jpg';
const metadata = await emitImageMetadata(imageId);
// Example value:
// {
//    src: '/@fs/home/username/dev/astro-project/src/images/photo.jpg?origWidth=800&origHeight=600&origFormat=jpg',
//    width: 800,
//    height: 600,
//    format: 'jpg',
//    contents: Uint8Array([...])
// }
```

### `emitClientAsset()`

[Section titled “emitClientAsset()”](#emitclientasset)

**Type:** `(pluginContext: Rollup.PluginContext, options: Rollup.EmitFile) => string`

**Added in:** `astro@6.0.0`

Emits a client asset that will be moved to the client directory for assets (e.g. `dist/client/_astro/`) during SSR builds. This function is intended for integration authors who need to emit assets (such as images) from server-rendered content that should be available on the client.

Use this instead of [Rollup `pluginContext.emitFile()`](https://rollupjs.org/plugin-development/#this-emitfile) directly when working in a Vite plugin context and you need the emitted asset to be moved to the client output directory.

```ts
import { emitClientAsset } from 'astro/assets/utils';


function myVitePlugin() {
  return {
    name: 'my-plugin',
    transform(code, id) {
      const handle = emitClientAsset(this, {
        type: 'asset',
        name: 'my-image.png',
        source: imageBuffer,
      });
      // Returns the asset handle similar to `emitFile()`
    }
  }
}
```

### `getOrigQueryParams()`

[Section titled “getOrigQueryParams()”](#getorigqueryparams)

**Type:** `(params: URLSearchParams) => Pick<ImageMetadata, ‘width’ | ‘height’ | ‘format’> | undefined`

**Added in:** `astro@4.0.0`

Retrieves the `width`, `height`, and `format` of an image from a [`URLSearchParams` object](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams). If any of these parameters are missing or invalid, the function returns `undefined`.

```ts
import { getOrigQueryParams } from 'astro/assets/utils';


const url = new URL('https://example.com/image.jpg?width=800&height=600&format=jpg');
const origParams = getOrigQueryParams(url.searchParams);
// Example value:
// {
//    width: 800,
//    height: 600,
//    format: 'jpg'
// }
```

### `inferRemoteSize()`

[Section titled “inferRemoteSize()”](#inferremotesize-1)

**Type:** `(url: string) => Promise<Omit<ImageMetadata, ‘src’ | ‘fsPath’>>`

**Added in:** `astro@4.0.0`

Infers the dimensions of a remote image by streaming its data and analyzing it progressively until sufficient metadata is available.

```ts
import { inferRemoteSize } from 'astro/assets/utils';


const remoteImageUrl = 'https://example.com/image.jpg';
const imageSize = await inferRemoteSize(remoteImageUrl);
// Example value:
// {
//    width: 1920,
//    height: 1080,
//    format: 'jpg'
// }
```

### `propsToFilename()`

[Section titled “propsToFilename()”](#propstofilename)

**Type:** `(filePath: string, transform: ImageTransform, hash: string) => string`

**Added in:** `astro@4.0.0`

Generates a formatted filename for an image based on its source path, transformation properties, and a unique hash.

The formatted filename follows this structure:

`<prefixDirname>/<baseFilename>_<hash><outputExtension>`

* `prefixDirname`: If the image is an ESM imported image, this is the directory name of the original file path; otherwise, it will be an empty string.
* `baseFilename`: The base name of the file or a hashed short name if the file is a `data:` URI.
* `hash`: A unique hash string generated to distinguish the transformed file.
* `outputExtension`: The desired output file extension derived from the `transform.format` or the original file extension.

```ts
import { propsToFilename } from 'astro/assets/utils';


const filePath = '/images/photo.jpg';
const transform = { format: 'png', src: filePath };
const hash = 'abcd1234';


const filename = propsToFilename(filePath, transform, hash);
// Example value: '/images/photo_abcd1234.png'
```

### `hashTransform()`

[Section titled “hashTransform()”](#hashtransform)

**Type:** `(transform: ImageTransform, imageService: string, propertiesToHash: string[]) => string`

**Added in:** `astro@4.0.0`

Transforms the provided `transform` object into a hash string based on selected properties and the specified `imageService`.

```ts
import { hashTransform } from 'astro/assets/utils';


const transform = {
  src: '/images/photo.jpg',
  width: 800,
  height: 600,
  format: 'jpg',
};
const imageService = 'astro/assets/services/sharp';
const propertiesToHash = ['width', 'height', 'format'];


const hash = hashTransform(transform, imageService, propertiesToHash);
// Example value: 'd41d8cd98f00b204e9800998ecf8427e'
```

## `astro` types

[Section titled “astro types”](#astro-types)

```ts
import type {
  GetImageResult,
  ImageTransform,
  UnresolvedImageTransform,
  ImageMetadata,
  ImageInputFormat,
  ImageOutputFormat,
  ImageQuality,
  ImageQualityPreset,
  RemotePattern,
  ImageService,
  ExternalImageService,
  LocalImageService,
  ImageServiceConfig,
} from "astro";
```

### `GetImageResult`

[Section titled “GetImageResult”](#getimageresult)

**Type:** `object`

**Added in:** `astro@2.2.0`

Describes the result of the transformation after the call to [`getImage()`](/en/reference/modules/astro-assets/#getimage).

#### `GetImageResult.attributes`

[Section titled “GetImageResult.attributes”](#getimageresultattributes)

**Type:** `Record<string, any>`

Defines the additional HTML attributes needed to render the image (e.g. width, height, style).

#### `GetImageResult.options`

[Section titled “GetImageResult.options”](#getimageresultoptions)

**Type:** [`ImageTransform`](#imagetransform)

Describes the transformation settings after validation.

#### `GetImageResult.rawOptions`

[Section titled “GetImageResult.rawOptions”](#getimageresultrawoptions)

**Type:** [`ImageTransform`](#imagetransform)

Describes the original transformation settings.

#### `GetImageResult.src`

[Section titled “GetImageResult.src”](#getimageresultsrc)

**Type:** `string`

The path to the generated image.

#### `GetImageResult.srcSet`

[Section titled “GetImageResult.srcSet”](#getimageresultsrcset)

**Type:** `{ values: { transform: ImageTransform; descriptor?: string; attributes?: Record<string, any>; url: string; }[]; attribute: string; }`

**Added in:** `astro@3.3.0`

An object describing how to render the [`srcset` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img#srcset).

##### `GetImageResult.srcSet.values`

[Section titled “GetImageResult.srcSet.values”](#getimageresultsrcsetvalues)

**Type:** `{ transform: ImageTransform; descriptor?: string; attributes?: Record<string, any>; url: string; }[]`

An array of generated values where each entry includes a URL and a size descriptor. This can be used to manually generate the value of the `srcset` attribute.

##### `GetImageResult.srcSet.attribute`

[Section titled “GetImageResult.srcSet.attribute”](#getimageresultsrcsetattribute)

**Type:** `string`

A value ready to use in the `srcset` attribute.

### `ImageTransform`

[Section titled “ImageTransform”](#imagetransform)

**Type:** `object`

Defines the options accepted by the image transformation service. This contains a required `src` property, optional predefined properties, and any additional properties required by the image service:

#### `ImageTransform.src`

[Section titled “ImageTransform.src”](#imagetransformsrc)

**Type:** `ImageMetadata | string`

Defines the path to a local image in the `public` directory, the URL of a remote image, or the data from an imported image.

#### `ImageTransform.width`

[Section titled “ImageTransform.width”](#imagetransformwidth)

**Type:** `number | undefined`

The width of the image.

#### `ImageTransform.height`

[Section titled “ImageTransform.height”](#imagetransformheight)

**Type:** `number | undefined`

The height of the image.

#### `ImageTransform.widths`

[Section titled “ImageTransform.widths”](#imagetransformwidths)

**Type:** `number[] | undefined`

**Added in:** `astro@3.3.0`

A list of widths to generate for the image.

#### `ImageTransform.densities`

[Section titled “ImageTransform.densities”](#imagetransformdensities)

**Type:** ``(number | `${number}x`)[] | undefined``

**Added in:** `astro@3.3.0`

A list of pixel densities to generate for the image.

#### `ImageTransform.quality`

[Section titled “ImageTransform.quality”](#imagetransformquality)

**Type:** `ImageQuality | undefined`

The desired quality for the output image.

#### `ImageTransform.format`

[Section titled “ImageTransform.format”](#imagetransformformat)

**Type:** `ImageOutputFormat | undefined`

The desired format for the output image.

#### `ImageTransform.fit`

[Section titled “ImageTransform.fit”](#imagetransformfit)

**Type:** `'fill' | 'contain' | 'cover' | 'none' | 'scale-down' | string | undefined`

**Added in:** `astro@5.0.0`

Defines a list of allowed values for the `object-fit` CSS property, extensible with any string.

#### `ImageTransform.position`

[Section titled “ImageTransform.position”](#imagetransformposition)

**Type:** `string | undefined`

**Added in:** `astro@5.0.0`

Controls the value for the `object-position` CSS property.

### `UnresolvedImageTransform`

[Section titled “UnresolvedImageTransform”](#unresolvedimagetransform)

**Type:** `Omit<ImageTransform, “src”> & { src: ImageMetadata | string | Promise<{ default: ImageMetadata }>; inferSize?: boolean; }`

Represents an image with transformation options. This contains the same properties as the [`ImageTransform` type](#imagetransform) with a different `src` type and an additional `inferSize` property.

#### `UnresolvedImageTransform.src`

[Section titled “UnresolvedImageTransform.src”](#unresolvedimagetransformsrc)

**Type:** `ImageMetadata | string | Promise<{ default: ImageMetadata }>`

The path to an image imported or located in the `public` directory, or the URL of a remote image.

#### `UnresolvedImageTransform.inferSize`

[Section titled “UnresolvedImageTransform.inferSize”](#unresolvedimagetransforminfersize)

**Type:** `boolean`

Determines whether the width and height of the image should be inferred.

See also the [`inferSize` attribute](/en/reference/modules/astro-assets/#infersize) available on `<Image />`.

### `ImageMetadata`

[Section titled “ImageMetadata”](#imagemetadata-1)

**Type:** `{ src: string; width: number; height: number; format: ImageInputFormat; orientation?: number; }`

**Added in:** `astro@2.1.3`

Describes the data collected during image import. This contains the following properties:

#### `ImageMetadata.src`

[Section titled “ImageMetadata.src”](#imagemetadatasrc)

**Type:** `string`

The absolute path of the image on the filesystem.

#### `ImageMetadata.width`

[Section titled “ImageMetadata.width”](#imagemetadatawidth)

**Type:** `number`

The width of the image.

#### `ImageMetadata.height`

[Section titled “ImageMetadata.height”](#imagemetadataheight)

**Type:** `number`

The height of the image.

#### `ImageMetadata.format`

[Section titled “ImageMetadata.format”](#imagemetadataformat)

**Type:** [`ImageInputFormat`](#imageinputformat)

The format of the image.

#### `ImageMetadata.orientation`

[Section titled “ImageMetadata.orientation”](#imagemetadataorientation)

**Type:** `number`

**Added in:** `astro@2.8.3`

The image orientation when its metadata contains this information.

### `ImageInputFormat`

[Section titled “ImageInputFormat”](#imageinputformat)

**Type:** `"jpeg" | "jpg" | "png" | "tiff" | "webp" | "gif" | "svg" | "avif"`

**Added in:** `astro@2.2.0`

Describes a union of supported formats for imported images.

### `ImageOutputFormat`

[Section titled “ImageOutputFormat”](#imageoutputformat)

**Type:** `string | "jpeg" | "jpg" | "png" | "webp" | "svg" | "avif"`

**Added in:** `astro@2.2.0`

Specifies the format for output images. This can be a predefined literal or any string.

### `ImageQuality`

[Section titled “ImageQuality”](#imagequality)

**Type:** `ImageQualityPreset | number`

**Added in:** `astro@2.2.0`

Represents the perceptual quality of the output image as a union of predefined literals, a string, or a number.

### `ImageQualityPreset`

[Section titled “ImageQualityPreset”](#imagequalitypreset)

**Type:** `string | "low" | "mid" | "high" | "max"`

**Added in:** `astro@2.2.0`

Defines the available presets to control image quality, extensible with any string.

### `RemotePattern`

[Section titled “RemotePattern”](#remotepattern)

**Type:** `{ hostname?: string; pathname?: string; protocol?: string; port?: string; }`

**Added in:** `astro@5.14.2`

Describes a remote host through four optional properties: `hostname`, `pathname`, `protocol`, and `port`.

### `ImageService`

[Section titled “ImageService”](#imageservice)

**Type:** `ExternalImageService | LocalImageService`

Defines the hooks that a local or external image service must provide.

### `ExternalImageService`

[Section titled “ExternalImageService”](#externalimageservice)

**Type:** `object`

Defines the hooks that an external image transformation service must provide. This requires a [`getUrl()` hook](/en/reference/image-service-reference/#geturl) and supports [three additional hooks](/en/reference/image-service-reference/#hooks).

Learn how to build [external services](/en/reference/image-service-reference/#external-services) in the Image Service API reference with example usage.

### `LocalImageService`

[Section titled “LocalImageService”](#localimageservice)

**Type:** `object`

Defines the hooks that a local image transformation service must provide. This requires [`getUrl()`](/en/reference/image-service-reference/#geturl), [`parseUrl()`](/en/reference/image-service-reference/#parseurl), and [`transform()`](/en/reference/image-service-reference/#transform) hooks, and supports [additional hooks](/en/reference/image-service-reference/#hooks).

Learn how to build [local services](/en/reference/image-service-reference/#local-services) in the Image Service API reference with example usage.

### `ImageServiceConfig`

[Section titled “ImageServiceConfig”](#imageserviceconfig)

**Type:** `{ entrypoint: 'astro/assets/services/sharp' | string; config?: T; }`

**Added in:** `astro@2.3.3`

Describes the configuration object for an image service. This contains the following properties:

#### `ImageServiceConfig.entrypoint`

[Section titled “ImageServiceConfig.entrypoint”](#imageserviceconfigentrypoint)

**Type:** `'astro/assets/services/sharp' | string`

A package or path to the image service module. This can be Astro’s built-in Sharp service or a third-party service.

#### `ImageServiceConfig.config`

[Section titled “ImageServiceConfig.config”](#imageserviceconfigconfig)

**Type:** `Record<string, any>`

A configuration object passed to the image service. The shape depends on the specific service being used.

# Config imports API Reference

**Added in:** `astro@5.7.0`

This virtual module `astro:config` exposes a non-exhaustive, serializable, type-safe version of the Astro configuration. There are two submodules for accessing different subsets of your configuration values: [`/client`](#imports-from-astroconfigclient) and [`/server`](#imports-from-astroconfigserver).

All available config values can be accessed from `astro:config/server`. However, for code executed on the client, only those values exposed by `astro:config/client` will be available. This protects your information by only making some data available to the client.

## Imports from `astro:config/client`

[Section titled “Imports from astro:config/client”](#imports-from-astroconfigclient)

The following helpers are imported from the `client` directory of the virtual config module.

```js
import {
  i18n,
  trailingSlash,
  base,
  build,
  site,
  compressHTML,
} from "astro:config/client";
```

Use this submodule for client-side code:

src/utils.js

```diff
+import { trailingSlash } from "astro:config/client";


function addForwardSlash(path) {
  if (trailingSlash === "always") {
    return path.endsWith("/") ? path : path + "/"
  } else {
    return path
  }
}
```

See more about the configuration imports available from `astro:config/client`:

* [`i18n`](/en/reference/configuration-reference/#i18n)
* [`trailingSlash`](/en/reference/configuration-reference/#trailingslash)
* [`base`](/en/reference/configuration-reference/#base)
* [`build.format`](/en/reference/configuration-reference/#buildformat)
* [`site`](/en/reference/configuration-reference/#site)
* [`compressHTML`](/en/reference/configuration-reference/#compresshtml)

## Imports from `astro:config/server`

[Section titled “Imports from astro:config/server”](#imports-from-astroconfigserver)

The following helpers are imported from the `server` directory of the virtual config module.

```js
import {
  i18n,
  trailingSlash,
  base,
  build,
  site,
  srcDir,
  cacheDir,
  outDir,
  publicDir,
  root,
  compressHTML,
} from "astro:config/server";
```

These imports include everything available from `astro:config/client` as well as additional sensitive information about your file system configuration that is not safe to expose to the client.

Use this submodule for server side code:

astro.config.mjs

```js
import { integration } from "./integration.mjs";


export default defineConfig({
    integrations: [
      integration(),
    ]
});
```

integration.mjs

```diff
+import { outDir } from "astro:config/server";
import { writeFileSync } from "node:fs";
import { fileURLToPath } from "node:url";


export default function() {
  return {
    name: "internal-integration",
    hooks: {
      "astro:build:done": () => {
        let file = new URL("result.json", outDir);
        // generate data from some operation
        let data = JSON.stringify([]);
        writeFileSync(fileURLToPath(file), data, "utf-8");
      }
    }
  }
}
```

See more about the configuration imports available from `astro:config/server`:

* [`i18n`](/en/reference/configuration-reference/#i18n)
* [`trailingSlash`](/en/reference/configuration-reference/#trailingslash)
* [`base`](/en/reference/configuration-reference/#base)
* [`build.format`](/en/reference/configuration-reference/#buildformat)
* [`build.client`](/en/reference/configuration-reference/#buildclient)
* [`build.server`](/en/reference/configuration-reference/#buildserver)
* [`site`](/en/reference/configuration-reference/#site)
* [`srcDir`](/en/reference/configuration-reference/#srcdir)
* [`cacheDir`](/en/reference/configuration-reference/#cachedir)
* [`outDir`](/en/reference/configuration-reference/#outdir)
* [`publicDir`](/en/reference/configuration-reference/#publicdir)
* [`root`](/en/reference/configuration-reference/#root)
* [`compressHTML`](/en/reference/configuration-reference/#compresshtml)

## Imports from `astro/config`

[Section titled “Imports from astro/config”](#imports-from-astroconfig)

The following helpers are imported from the regular config module:

```js
import {
  defineConfig,
  envField,
  fontProviders,
  getViteConfig,
  mergeConfig,
  passthroughImageService,
  sessionDrivers,
  sharpImageService,
  validateConfig,
} from "astro/config";
```

### `defineConfig()`

[Section titled “defineConfig()”](#defineconfig)

**Type:** `(config: AstroUserConfig) => AstroUserConfig`

Configures your project with type safety [in a supported Astro configuration file](/en/guides/configuring-astro/#the-astro-config-file).

### `envField`

[Section titled “envField”](#envfield)

**Type:** `object`

**Added in:** `astro@5.0.0`

Describes the supported data types when [defining environment variables](/en/reference/configuration-reference/#envschema).

Each data type must define the [variable type](/en/guides/environment-variables/#variable-types) with `context` (`"client"` or `"server"`) and `access` (`"secret"` or `"public"`). In addition, you can define a `default` value, specify whether the variable is `optional` (default `false`), and some data types provide optional validation methods.

Learn more about [using type safe environment variables](/en/guides/environment-variables/#type-safe-environment-variables) in your Astro project.

#### `envField.string()`

[Section titled “envField.string()”](#envfieldstring)

**Type:** `(options: StringFieldInput) => StringField`

Defines an environment variable of string type. You can perform [string validation with Zod](https://zod.dev/api#strings) using the following properties: `max`, `min`, `length`, `url`, `includes`, `startsWith`, and `endsWith`.

The following example defines the expected shape for an environment variable storing an API URL:

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config";


export default defineConfig({
  env: {
    schema: {
      API_URL: envField.string({
        context: "client",
        access: "public",
        optional: false,
        default: "",
        min: 12,
        url: true,
        includes: "astro",
        startsWith: "https",
      }),
    }
  }
})
```

#### `envField.number()`

[Section titled “envField.number()”](#envfieldnumber)

**Type:** `(options: NumberFieldInput) => NumberField`

Defines an environment variable of number type. You can perform [number validation with Zod](https://zod.dev/api#numbers) using the following properties: `gt`, `lt`, `min`, `max`, and `int`.

The following example defines the expected shape for an environment variable storing an API port:

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config";


export default defineConfig({
  env: {
    schema: {
      API_PORT: envField.number({
        context: "server",
        access: "public",
        optional: true,
        default: 4321,
        min: 2,
        int: true,
      }),
    }
  }
})
```

#### `envField.boolean()`

[Section titled “envField.boolean()”](#envfieldboolean)

**Type:** `(options: BooleanFieldInput) => BooleanField`

Defines an environment variable of boolean type.

The following example defines the expected shape for an environment variable storing whether analytics are enabled:

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config";


export default defineConfig({
  env: {
    schema: {
      ANALYTICS_ENABLED: envField.boolean({
        context: "client",
        access: "public",
        optional: true,
        default: true,
      }),
    }
  }
})
```

#### `envField.enum()`

[Section titled “envField.enum()”](#envfieldenum)

**Type:** `(options: EnumFieldInput<T>) => EnumField`

Defines an environment variable of enum type by providing the allowed `values` as an array.

The following example defines the expected shape for an environment variable storing the configured debug mode:

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config";


export default defineConfig({
  env: {
    schema: {
      DEBUG_MODE: envField.enum({
        context: "server",
        access: "public",
        values: ['info', 'warnings', 'errors'], // required
        optional: true,
        default: 'errors',
      }),
    }
  }
})
```

### `fontProviders`

[Section titled “fontProviders”](#fontproviders)

**Type:** `object`

**Added in:** `astro@6.0.0`

Describes the [built-in provider](/en/reference/font-provider-reference/#built-in-providers) used [to retrieve the configured font](/en/reference/configuration-reference/#fontprovider).

### `getViteConfig()`

[Section titled “getViteConfig()”](#getviteconfig)

**Type:** `(userViteConfig: ViteUserConfig, inlineAstroConfig?: AstroInlineConfig) => ViteUserConfigFn`

Retrieves the Vite configuration to use by merging a custom Vite configuration object and an optional Astro configuration object. This is useful [to set up Vitest for testing](/en/guides/testing/#vitest).

### `mergeConfig()`

[Section titled “mergeConfig()”](#mergeconfig)

See [`mergeConfig()` in the Programmatic API reference](/en/reference/programmatic-reference/#mergeconfig).

### `passthroughImageService()`

[Section titled “passthroughImageService()”](#passthroughimageservice)

**Type:** `() => ImageServiceConfig`

Retrieves a no-op image service. This is useful when your adapter does not support Astro’s built-in Sharp image optimization and you want to [use the `<Image />` and `<Picture />` components](/en/guides/images/#astro-components-for-images).

The following example defines `passthroughImageService()` as image service in the Astro configuration file to avoid Sharp image processing:

astro.config.mjs

```js
import { defineConfig, passthroughImageService } from "astro/config";


export default defineConfig({
  image: {
    service: passthroughImageService()
  }
});
```

Learn more about [configuring a no-op passthrough service](/en/guides/images/#configure-no-op-passthrough-service).

### `sessionDrivers`

[Section titled “sessionDrivers”](#sessiondrivers)

**Type:** `object`

**Added in:** `astro@5.7.0`

Describes the [built-in driver](/en/reference/session-driver-reference/#built-in-drivers) used [for session storage](/en/reference/configuration-reference/#session-options).

The following example configures the Redis driver to enable sessions:

astro.config.mjs

```js
import { defineConfig, sessionDrivers } from "astro/config";


export default defineConfig({
  session: {
    driver: sessionDrivers.redis({
      url: process.env.REDIS_URL
    }),
  }
})
```

Learn more about [using sessions](/en/guides/sessions/) in your Astro project.

### `sharpImageService()`

[Section titled “sharpImageService()”](#sharpimageservice)

**Type:** `(config?: SharpImageServiceConfig) => ImageServiceConfig`

**Added in:** `astro@2.4.1`

Retrieves the Sharp service used to process Astro’s image assets. This takes an optional object describing the [configuration options for Sharp](/en/reference/configuration-reference/#imageservice).

### `validateConfig()`

[Section titled “validateConfig()”](#validateconfig)

See [`validateConfig()` in the Programmatic API reference](/en/reference/programmatic-reference/#validateconfig).

# Content Collections API Reference

**Added in:** `astro@2.0.0`

Build-time content collections offer APIs to configure, query, and render your local Markdown, MDX, Markdoc, YAML, TOML, or JSON files, as well as remote content.

**Added in:** `astro@6.0.0`

Live content collections offer APIs to configure, query, and render fresh, up-to-the-moment live data from remote sources.

For features and usage examples, [see our content collections guide](/en/guides/content-collections/).

## Imports from `astro:content`

[Section titled “Imports from astro:content”](#imports-from-astrocontent)

```js
import {
  defineCollection,
  defineLiveCollection,
  getCollection,
  getLiveCollection,
  getEntry,
  getLiveEntry,
  getEntries,
  reference,
  render
} from 'astro:content';
```

### `defineCollection()`

[Section titled “defineCollection()”](#definecollection)

**Type:** `(input: CollectionConfig) => CollectionConfig`

**Added in:** `astro@2.0.0`

A utility to configure a collection in a `src/content.config.*` file.

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';
import { glob } from 'astro/loaders';


const blog = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/data/blog' }),
  schema: z.object({
    title: z.string(),
    permalink: z.string().optional(),
  }),
});


// Expose your defined collection to Astro
// with the `collections` export
export const collections = { blog };
```

This function accepts the following properties:

#### `loader`

[Section titled “loader”](#loader)

**Type:** `() => Promise<Array<{ id: string, [key: string]: any }> | Record<string, Record<string, any>>> | Loader`

**Added in:** `astro@5.0.0`

Either an object or a function that allows you to load data from any source, local or remote, into a build-time content collection. (For live collections, see the [live `loader`](#loader-1) property.)

Learn about [build-time collection loaders](/en/guides/content-collections/#build-time-collection-loaders) with guided explanations and example usage.

#### `schema`

[Section titled “schema”](#schema)

**Type:** `ZodType | (context: SchemaContext) => ZodType`

**Added in:** `astro@2.0.0`

An optional Zod object or function that returns a Zod object to configure the type and shape of document frontmatter for a collection. Each value must use [a Zod validator](/en/reference/modules/astro-zod/#common-data-type-validators). (For live collections, see the [live `schema`](#schema-1) property.)

Learn about [defining a collection schema](/en/guides/content-collections/#defining-the-collection-schema) using Zod with guided explanations, example usage, and common datatypes.

### `defineLiveCollection()`

[Section titled “defineLiveCollection()”](#definelivecollection)

**Type:** `(config: LiveCollectionConfig) => LiveCollectionConfig`

**Added in:** `astro@6.0.0`

A utility to configure a live collection in a `src/live.config.*` file.

src/live.config.ts

```ts
import { defineLiveCollection } from 'astro:content';
import { storeLoader } from '@example/astro-loader';


const products = defineLiveCollection({
  loader: storeLoader({
    apiKey: process.env.STORE_API_KEY,
    endpoint: 'https://api.example.com/v1',
  }),
});


// Expose your defined collection to Astro
// with the `collections` export
export const collections = { products };
```

This function accepts the following properties:

#### `loader`

[Section titled “loader”](#loader-1)

**Type:** `LiveLoader`

**Added in:** `astro@6.0.0`

An object that allows you to load data at runtime from a remote source into a live content collection. (For build-time collections, see the [build-time `loader`](#loader) property.)

Learn how to [create a live loader](/en/guides/content-collections/#creating-a-live-loader) with guided explanations and example usage.

#### `schema`

[Section titled “schema”](#schema-1)

**Type:** `ZodType`

**Added in:** `astro@6.0.0`

An optional Zod object to configure the type and shape of your data for a live collection. Each value must use [a Zod validator](https://github.com/colinhacks/zod). (For build-time collections, see the [build-time `schema`](#schema) property.)

When you define a schema, it will take precedence over the [live loader’s types](/en/reference/content-loader-reference/#live-loader-api) when you query the collection.

Learn about [using Zod schemas with live collections](/en/guides/content-collections/#using-zod-schemas-with-live-collections) through guided explanations and usage examples.

### `reference()`

[Section titled “reference()”](#reference)

**Type:** `(collection: CollectionKey) => ZodEffects<ZodString, { collection: CollectionKey, id: string }>`

**Added in:** `astro@2.5.0`

A function used in the content config to define a relationship, or “reference”, from one collection to another. This accepts a collection name and transforms the reference into an object containing the collection name and the reference id.

This example defines references from a blog author to the `authors` collection and an array of related posts to the same `blog` collection:

src/content.config.ts

```ts
import { defineCollection, reference } from 'astro:content';
import { z } from 'astro/zod';
import { glob, file } from 'astro/loaders';


const blog = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/data/blog' }),
  schema: z.object({
    // Reference a single author from the `authors` collection by `id`
    author: reference('authors'),
    // Reference an array of related posts from the `blog` collection by `slug`
    relatedPosts: z.array(reference('blog')),
  })
});


const authors = defineCollection({
  loader: file("src/data/authors.json"),
  schema: z.object({ /* ... */ })
});


export const collections = { blog, authors };
```

Validation of referenced entries happens at runtime when using `getEntry()` or `getEntries()`:

src/pages/\[posts].astro

```astro
// if a referenced entry is invalid, this will return undefined.
const relatedPosts = await getEntries(blogPost.data.relatedPosts);
```

Learn how to [define and use collection references](/en/guides/content-collections/#defining-collection-references) with guided explanations and usage examples.

### `getCollection()`

[Section titled “getCollection()”](#getcollection)

**Type:** `(collection: CollectionKey, filter?: (entry: CollectionEntry) => boolean) => CollectionEntry[]`

**Added in:** `astro@2.0.0`

A function that retrieves a list of content collection entries by collection name.

It returns all items in the collection by default, and accepts an optional `filter` function to narrow by entry properties. This allows you to query for only some items in a collection based on `id` or frontmatter values via the `data` object.

src/pages/blog/index.astro

```astro
---
import { getCollection } from 'astro:content';


// Get all `src/data/blog/` entries
const allBlogPosts = await getCollection('blog');


// Only return posts with `draft: true` in the frontmatter
const draftBlogPosts = await getCollection('blog', ({ data }) => {
  return data.draft === true;
});
---
```

Learn how to [query build time collections](/en/guides/content-collections/#querying-build-time-collections) with guided explanations and example usage.

### `getLiveCollection()`

[Section titled “getLiveCollection()”](#getlivecollection)

**Type:** `(collection: string, filter?: LiveLoaderCollectionFilterType) => Promise<LiveDataCollectionResult>`

**Added in:** `astro@6.0.0`

A function that retrieves a list of live content collection entries by collection name.

It returns all items in the collection by default, and accepts an optional `filter` object whose shape is defined by the collection’s loader. This allows you to query for only some items in a collection or retrieve data in a different form, depending on your API’s capabilities.

src/pages/shop/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';


// Get all `products` entries from your API
const { entries: allProducts } = await getLiveCollection('products');


// Only return `products` that should be featured
const { entries: featuredProducts } = await getLiveCollection('products', { featured: true });
---
```

Learn how to [access live collections data](/en/guides/content-collections/#accessing-live-data) with guided explanations and example usage.

### `getEntry()`

[Section titled “getEntry()”](#getentry)

**Types:**

* `(collection: CollectionKey, id: string) => Promise<CollectionEntry | undefined>`
* `({ collection: CollectionKey, id: string }) => Promise<CollectionEntry | undefined>`

**Added in:** `astro@2.5.0`

A function that retrieves a single collection entry by collection name and the entry `id`. `getEntry()` can also be used to get referenced entries to access the `data` or `body` properties:

src/pages/index.astro

```astro
---
import { getEntry } from 'astro:content';


// Get `src/content/blog/enterprise.md`
const enterprisePost = await getEntry('blog', 'enterprise');


// Get `src/content/captains/picard.json`
const picardProfile = await getEntry('captains', 'picard');


// Get the profile referenced by `data.captain`
const enterpriseCaptainProfile = await getEntry(enterprisePost.data.captain);
---
```

Learn more about [querying build time collections](/en/guides/content-collections/#querying-build-time-collections) with guided explanations and example usage.

### `getLiveEntry()`

[Section titled “getLiveEntry()”](#getliveentry)

**Type:** `(collection: string, filter: string | LiveLoaderEntryFilterType) => Promise<LiveDataEntryResult>`

**Added in:** `astro@6.0.0`

A function that retrieves a single live collection entry by collection name and an optional filter, either as an `id` string or as a type-safe object.

src/pages/blog/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { entry: liveCollectionsPost } = await getLiveEntry('blog', Astro.params.id);
const { entry: mattDraft } = await getLiveEntry('blog', {
  status: 'draft',
  author: 'matt',
});
---
```

Learn how to [access live collections data](/en/guides/content-collections/#accessing-live-data) with guided explanations and example usage.

### `getEntries()`

[Section titled “getEntries()”](#getentries)

**Type:** `({ collection: CollectionKey, id: string }[]) => CollectionEntry[]`

**Added in:** `astro@2.5.0`

A function that retrieves multiple collection entries from the same collection. This is useful for [returning an array of referenced entries](/en/guides/content-collections/#defining-collection-references) to access their associated `data` and `body` properties.

src/pages/blog/enterprise/index.astro

```astro
---
import { getEntries, getEntry } from 'astro:content';


const enterprisePost = await getEntry('blog', 'enterprise');


// Get related posts referenced by `data.relatedPosts`
const enterpriseRelatedPosts = await getEntries(enterprisePost.data.relatedPosts);
---
```

### `render()`

[Section titled “render()”](#render)

**Type:** `(entry: CollectionEntry) => Promise<RenderResult>`

**Added in:** `astro@5.0.0`

A function to compile a given entry for rendering. This returns the following properties:

* `<Content />` - A component used to render the document’s contents in an Astro file.
* `headings` - A generated list of headings, [mirroring Astro’s `getHeadings()` utility](/en/guides/markdown-content/#available-properties) on Markdown and MDX imports.
* `remarkPluginFrontmatter `- The modified frontmatter object after any [remark or rehype plugins have been applied](/en/guides/markdown-content/#modifying-frontmatter-programmatically). Set to type `any`.

src/pages/blog/entry-1.astro

```astro
---
import { getEntry, render } from 'astro:content';
const entry = await getEntry('blog', 'entry-1');


if (!entry) {
   // Handle Error, for example:
  throw new Error('Could not find blog post 1');
}
const { Content, headings, remarkPluginFrontmatter } = await render(entry);
---
```

Learn how to [render the body content of entries](/en/guides/content-collections/#rendering-body-content) with guided explanations and example usage.

## `astro:content` types

[Section titled “astro:content types”](#astrocontent-types)

```ts
import type {
  CollectionEntry,
  CollectionKey,
  SchemaContext,
} from 'astro:content';
```

### `CollectionEntry`

[Section titled “CollectionEntry”](#collectionentry)

Query functions including [`getCollection()`](#getcollection), [`getEntry()`](#getentry), and [`getEntries()`](#getentries) each return entries with the `CollectionEntry` type. This type is available as a utility from `astro:content`:

```ts
import type { CollectionEntry } from 'astro:content';
```

A generic type to use with the name of the collection you’re querying to represent a single entry in that collection. For example, an entry in your `blog` collection would have the type `CollectionEntry<'blog'>`.

Each `CollectionEntry` is an object with the following values:

#### `CollectionEntry.id`

[Section titled “CollectionEntry.id”](#collectionentryid)

**Type:** `string`

A unique ID. Note that all IDs from Astro’s built-in `glob()` loader are slugified.

#### `CollectionEntry.collection`

[Section titled “CollectionEntry.collection”](#collectionentrycollection)

**Type:** [`CollectionKey`](#collectionkey)

The name of a collection in which entries are located. This is the name used to reference the collection in your schema and in querying functions.

#### `CollectionEntry.data`

[Section titled “CollectionEntry.data”](#collectionentrydata)

**Type:** `CollectionSchema<TCollectionName>`

An object of frontmatter properties inferred from your collection schema ([see `defineCollection()` reference](#definecollection)). Defaults to `any` if no schema is configured.

#### `CollectionEntry.body`

[Section titled “CollectionEntry.body”](#collectionentrybody)

**Type:** `string | undefined`

A string containing the raw, uncompiled body of the Markdown or MDX document.

Note that if [`retainBody`](/en/reference/content-loader-reference/#retainbody) is set to `false`, this value will be `undefined` instead of containing the raw file contents.

#### `CollectionEntry.rendered`

[Section titled “CollectionEntry.rendered”](#collectionentryrendered)

**Type:** `RenderedContent | undefined`

The rendered content of an entry as [stored by your loader](/en/reference/content-loader-reference/#dataentryrendered). For example, this can be the rendered content of a Markdown entry, or HTML from a CMS.

#### `CollectionEntry.filePath`

[Section titled “CollectionEntry.filePath”](#collectionentryfilepath)

**Type:** `string | undefined`

The path to an entry relative to your project directory. This value is only available for local entries.

### `CollectionKey`

[Section titled “CollectionKey”](#collectionkey)

**Example Type:** `'blog' | 'authors' | ...`

**Added in:** `astro@3.1.0`

A string union of all collection names defined in your `src/content.config.*` file. This type can be useful when defining a generic function wrapping the built-in `getCollection()`.

src/utils/collections.ts

```ts
import { type CollectionKey, getCollection } from 'astro:content';


export async function queryCollection(collection: CollectionKey) {
  return getCollection(collection, ({ data }) => {
    return data.draft !== true;
  });
}
```

### `SchemaContext`

[Section titled “SchemaContext”](#schemacontext)

The `context` object that `defineCollection` uses for the function shape of `schema`. This type can be useful when building reusable schemas for multiple collections.

This includes the following property:

* `image` - The `image()` schema helper that allows you [to use local images in Content Collections](/en/guides/images/#images-in-content-collections)

src/content.config.ts

```ts
import { defineCollection, type SchemaContext } from "astro:content";
import { z } from 'astro/zod';
import { glob } from 'astro/loaders';


export const imageSchema = ({ image }: SchemaContext) =>
    z.object({
        image: image(),
        description: z.string().optional(),
    });


const blog = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/data/blog' }),
  schema: ({ image }) => z.object({
    title: z.string(),
    permalink: z.string().optional(),
    image: imageSchema({ image })
  }),
});
```

## `astro` types

[Section titled “astro types”](#astro-types)

```ts
import type {
  LiveDataCollectionResult,
  LiveDataEntryResult,
} from "astro";
```

### `LiveDataCollectionResult`

[Section titled “LiveDataCollectionResult”](#livedatacollectionresult)

**Type:** `{ entries?: Array<LiveDataEntry<TData>>; error?: TError | LiveCollectionError; cacheHint?: CacheHint; }`

**Added in:** `astro@6.0.0`

An object returned by [`getLiveCollection()`](#getlivecollection) containing the data fetched by the live loader. It has the following properties:

#### `LiveDataCollectionResult.entries`

[Section titled “LiveDataCollectionResult.entries”](#livedatacollectionresultentries)

**Type:** `Array<LiveDataEntry<TData>> | undefined`

An array of [`LiveDataEntry`](/en/reference/content-loader-reference/#livedataentry) objects returned by the loader.

The following example accesses the returned entries for a live collection named `products`:

src/pages/shop/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';


const { entries: allProducts } = await getLiveCollection('products');
---
```

Learn how to [access live data](/en/guides/content-collections/#accessing-live-data) with guided explanations and example usage.

#### `LiveDataCollectionResult.error`

[Section titled “LiveDataCollectionResult.error”](#livedatacollectionresulterror)

**Type:** `TError | LiveCollectionError | undefined`

An error returned when the loader failed to load the collection. This can be a custom error defined by the loader or a built-in error.

The following example accesses the error returned when retrieving data from a live collection named `products`:

src/pages/shop/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';


const { error } = await getLiveCollection('products');
---
```

Learn more about [error handling](/en/guides/content-collections/#error-handling) with guided explanations and example usage.

#### `LiveDataCollectionResult.cacheHint`

[Section titled “LiveDataCollectionResult.cacheHint”](#livedatacollectionresultcachehint)

**Type:** `CacheHint | undefined`

An object providing guidance on how to cache this collection.

If you have [experimental route caching](/en/reference/experimental-flags/route-caching/) enabled, pass the cache hint directly to `Astro.cache.set()`:

src/pages/shop/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';
export const prerender = false; // Not needed in 'server' mode


const { cacheHint } = await getLiveCollection('products');


if (cacheHint) {
  Astro.cache.set(cacheHint);
}
Astro.cache.set({ maxAge: 600 });
---
```

You can also use cache hints to set response headers manually:

src/pages/shop/index.astro

```astro
---
import { getLiveCollection } from 'astro:content';


const { cacheHint } = await getLiveCollection('products');


if (cacheHint?.tags) {
  Astro.response.headers.set('Cache-Tag', cacheHint.tags.join(','));
}
if (cacheHint?.lastModified) {
  Astro.response.headers.set('Last-Modified', cacheHint.lastModified.toUTCString());
}
---
```

### `LiveDataEntryResult`

[Section titled “LiveDataEntryResult”](#livedataentryresult)

**Type:** `{ entry?: LiveDataEntry<TData>; error?: TError | LiveCollectionError; cacheHint?: CacheHint; }`

**Added in:** `astro@6.0.0`

An object returned by [`getLiveEntry()`](#getliveentry) containing the data fetched by the live loader. It has the following properties:

#### `LiveDataEntryResult.entry`

[Section titled “LiveDataEntryResult.entry”](#livedataentryresultentry)

**Type:** `LiveDataEntry<TData> | undefined`

The [`LiveDataEntry`](/en/reference/content-loader-reference/#livedataentry) object returned by the loader.

The following example accesses the requested entry in a live collection named `products`:

src/pages/shop/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { entry } = await getLiveEntry('products', Astro.params.id);
---
```

Learn how to [access live data](/en/guides/content-collections/#accessing-live-data) with guided explanations and example usage.

#### `LiveDataEntryResult.error`

[Section titled “LiveDataEntryResult.error”](#livedataentryresulterror)

**Type:** `TError | LiveCollectionError | undefined`

An error returned when the loader failed to load the entry. This can be a custom error defined by the loader or a built-in error.

The following example accesses the requested entry in a live collection named `products` and any error, and redirects to the 404 page if an error exists:

src/pages/shop/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { entry, error } = await getLiveEntry('products', Astro.params.id);


if (error) {
  return Astro.redirect('/404');
}
---
<h1>{entry.data.name}</h1>
```

Learn more about [error handling](/en/guides/content-collections/#error-handling) with guided explanations and example usage.

#### `LiveDataEntryResult.cacheHint`

[Section titled “LiveDataEntryResult.cacheHint”](#livedataentryresultcachehint)

**Type:** `CacheHint | undefined`

An object providing data that can be used to inform a caching strategy.

If you have [experimental route caching](/en/reference/experimental-flags/route-caching/) enabled, pass the cache hint directly to `Astro.cache.set()`:

src/pages/shop/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


export const prerender = false; // Not needed in 'server' mode


const { cacheHint } = await getLiveEntry('products', Astro.params.id);


if (cacheHint) {
  Astro.cache.set(cacheHint);
}
Astro.cache.set({ maxAge: 300 });
---
```

You can also use cache hints to set response headers manually:

src/pages/shop/\[id].astro

```astro
---
import { getLiveEntry } from 'astro:content';


const { cacheHint } = await getLiveEntry('products', Astro.params.id);


if (cacheHint?.tags) {
  Astro.response.headers.set('Cache-Tag', cacheHint.tags.join(','));
}
if (cacheHint?.lastModified) {
  Astro.response.headers.set('Last-Modified', cacheHint.lastModified.toUTCString());
}
---
```

# Environment Variables API Reference

**Added in:** `astro@5.0.0`

The `astro:env` API lets you configure a type-safe schema for environment variables you have set. This allows you to indicate whether they should be available on the server or the client, and define their data type and additional properties. For examples and usage instructions, [see the `astro:env` guide](/en/guides/environment-variables/#type-safe-environment-variables).

## Imports from `astro:env`

[Section titled “Imports from astro:env”](#imports-from-astroenv)

```js
import {
  getSecret,
 } from 'astro:env/server';
```

### `getSecret()`

[Section titled “getSecret()”](#getsecret)

**Added in:** `astro@5.0.0`

The `getSecret()` helper function allows retrieving the raw value of an environment variable by its key.

For example, you can retrieve a boolean value as a string:

```js
import {
  FEATURE_FLAG, // boolean
  getSecret
} from 'astro:env/server'


getSecret('FEATURE_FLAG') // string | undefined
```

This can also be useful to get a secret not defined in your schema, for example one that depends on dynamic data from a database or API.

If you need to retrieve environment variables programmatically, we recommend using `getSecret()` instead of `process.env` (or equivalent). Because its implementation is provided by your adapter, you won’t need to update all your calls if you switch adapters. It defaults to `process.env` in dev and build.

# Internationalization API Reference

**Added in:** `astro@3.5.0`

This module provides functions to help you create URLs using your project’s configured locales.

Creating routes for your project with the i18n router will depend on certain configuration values you have set that affect your page routes. When creating routes with these functions, be sure to take into account your individual settings for:

* [`base`](/en/reference/configuration-reference/#base)
* [`trailingSlash`](/en/reference/configuration-reference/#trailingslash)
* [`build.format`](/en/reference/configuration-reference/#buildformat)
* [`site`](/en/reference/configuration-reference/#site)

Also, note that the returned URLs created by these functions for your `defaultLocale` will reflect your `i18n.routing` configuration.

For features and usage examples, [see our i18n routing guide](/en/guides/internationalization/).

## Imports from `astro:i18n`

[Section titled “Imports from astro:i18n”](#imports-from-astroi18n)

```js
import {
  getRelativeLocaleUrl,
  getAbsoluteLocaleUrl,
  getRelativeLocaleUrlList,
  getAbsoluteLocaleUrlList,
  getPathByLocale,
  getLocaleByPath,
  redirectToDefaultLocale,
  redirectToFallback,
  notFound,
  middleware,
  requestHasLocale,
  normalizeTheLocale,
  pathHasLocale,
  toCodes,
  toPaths
 } from 'astro:i18n';
```

### `getRelativeLocaleUrl()`

[Section titled “getRelativeLocaleUrl()”](#getrelativelocaleurl)

**Type:** `(locale: string, path?: string, options?: GetLocaleOptions) => string`

Use this function to retrieve a relative path for a locale. If the locale doesn’t exist, Astro throws an error.

```astro
---
import { getRelativeLocaleUrl } from 'astro:i18n';


getRelativeLocaleUrl("fr");
// returns /fr


getRelativeLocaleUrl("fr", "");
// returns /fr/


getRelativeLocaleUrl("fr", "getting-started");
// returns /fr/getting-started


getRelativeLocaleUrl("fr_CA", "getting-started", {
  prependWith: "blog"
});
// returns /blog/fr-ca/getting-started


getRelativeLocaleUrl("fr_CA", "getting-started", {
  prependWith: "blog",
  normalizeLocale: false
});
// returns /blog/fr_CA/getting-started
---
```

### `getAbsoluteLocaleUrl()`

[Section titled “getAbsoluteLocaleUrl()”](#getabsolutelocaleurl)

**Type:** `(locale: string, path?: string, options?: GetLocaleOptions) => string`

Use this function to retrieve an absolute path for a locale when \[`site`] has a value. If \[`site`] isn’t configured, the function returns a relative URL. If the locale doesn’t exist, Astro throws an error.

src/pages/index.astro

```astro
---
import { getAbsoluteLocaleUrl } from 'astro:i18n';


// If `site` is set to be `https://example.com`


getAbsoluteLocaleUrl("fr");
// returns https://example.com/fr


getAbsoluteLocaleUrl("fr", "");
// returns https://example.com/fr/


getAbsoluteLocaleUrl("fr", "getting-started");
// returns https://example.com/fr/getting-started


getAbsoluteLocaleUrl("fr_CA", "getting-started", {
  prependWith: "blog"
});
// returns https://example.com/blog/fr-ca/getting-started


getAbsoluteLocaleUrl("fr_CA", "getting-started", {
  prependWith: "blog",
  normalizeLocale: false
});
// returns https://example.com/blog/fr_CA/getting-started
---
```

### `getRelativeLocaleUrlList()`

[Section titled “getRelativeLocaleUrlList()”](#getrelativelocaleurllist)

**Type:** `(path?: string, options?: GetLocaleOptions) => string[]`

Use this like [`getRelativeLocaleUrl`](#getrelativelocaleurl) to return a list of relative paths for all the locales.

### `getAbsoluteLocaleUrlList()`

[Section titled “getAbsoluteLocaleUrlList()”](#getabsolutelocaleurllist)

**Type:** `(path?: string, options?: GetLocaleOptions) => string[]`

Use this like [`getAbsoluteLocaleUrl`](/en/guides/internationalization/#custom-locale-paths) to return a list of absolute paths for all the locales.

### `getPathByLocale()`

[Section titled “getPathByLocale()”](#getpathbylocale)

**Type:** `(locale: string) => string`

A function that returns the `path` associated to one or more `codes` when [custom locale paths](/en/guides/internationalization/#custom-locale-paths) are configured.

astro.config.mjs

```js
export default defineConfig({
  i18n: {
    locales: ["es", "en", {
      path: "french",
      codes: ["fr", "fr-BR", "fr-CA"]
    }]
  }
})
```

src/pages/index.astro

```astro
---
import { getPathByLocale } from 'astro:i18n';


getPathByLocale("fr"); // returns "french"
getPathByLocale("fr-CA"); // returns "french"
---
```

### `getLocaleByPath()`

[Section titled “getLocaleByPath()”](#getlocalebypath)

**Type:** `(path: string) => string`

A function that returns the `code` associated to a locale `path`.

astro.config.mjs

```js
export default defineConfig({
  i18n: {
    locales: ["es", "en", {
      path: "french",
      codes: ["fr", "fr-BR", "fr-CA"]
    }]
  }
})
```

src/pages/index.astro

```astro
---
import { getLocaleByPath } from 'astro:i18n';


getLocaleByPath("french"); // returns "fr" because that's the first code configured
---
```

### `redirectToDefaultLocale()`

[Section titled “redirectToDefaultLocale()”](#redirecttodefaultlocale)

**Type:** `(context: APIContext, statusCode?: ValidRedirectStatus) => Promise<Response>`

**Added in:** `astro@4.6.0`

Note

Available only when `i18n.routing` is set to `"manual"`

A function that returns a `Response` that redirects to the `defaultLocale` configured. It accepts an optional valid redirect status code.

middleware.js

```js
import { defineMiddleware } from "astro:middleware";
import { redirectToDefaultLocale } from "astro:i18n";


export const onRequest = defineMiddleware((context, next) => {
  if (context.url.pathname.startsWith("/about")) {
    return next();
  } else {
    return redirectToDefaultLocale(context, 302);
  }
})
```

### `redirectToFallback()`

[Section titled “redirectToFallback()”](#redirecttofallback)

**Type:** `(context: APIContext, response: Response) => Promise<Response>`

**Added in:** `astro@4.6.0`

Note

Available only when `i18n.routing` is set to `"manual"`

A function that allows you to use your [`i18n.fallback` configuration](/en/reference/configuration-reference/#i18nfallback) in your own middleware.

middleware.js

```js
import { defineMiddleware } from "astro:middleware";
import { redirectToFallback } from "astro:i18n";


export const onRequest = defineMiddleware(async (context, next) => {
  const response = await next();
  if (response.status >= 300) {
    return redirectToFallback(context, response)
  }
  return response;
})
```

### `notFound()`

[Section titled “notFound()”](#notfound)

**Type:** `(context: APIContext, response?: Response) => Promise<Response> | undefined`

**Added in:** `astro@4.6.0`

Note

Available only when `i18n.routing` is set to `"manual"`

Use this function in your routing middleware to return a 404 when:

* the current path isn’t a root. e.g. `/` or `/<base>`
* the URL doesn’t contain a locale

When a `Response` is passed, the new `Response` emitted by this function will contain the same headers of the original response.

middleware.js

```js
import { defineMiddleware } from "astro:middleware";
import { notFound } from "astro:i18n";


export const onRequest = defineMiddleware((context, next) => {
  const pathNotFound = notFound(context);
  if (pathNotFound) {
    return pathNotFound;
  }
  return next();
})
```

### `middleware()`

[Section titled “middleware()”](#middleware)

**Type:** `(options: { prefixDefaultLocale: boolean, redirectToDefaultLocale: boolean }) => MiddlewareHandler`

**Added in:** `astro@4.6.0`

Note

Available only when `i18n.routing` is set to `"manual"`

A function that allows you to programmatically create the Astro i18n middleware.

This is useful when you still want to use the default i18n logic, but add only a few exceptions to your website.

middleware.js

```js
import { middleware } from "astro:i18n";
import { sequence, defineMiddleware } from "astro:middleware";


const customLogic = defineMiddleware(async (context, next) => {
  const response = await next();


  // Custom logic after resolving the response.
  // It's possible to catch the response coming from Astro i18n middleware.


  return response;
});


export const onRequest = sequence(customLogic, middleware({
  prefixDefaultLocale: true,
  redirectToDefaultLocale: false
}))
```

### `requestHasLocale()`

[Section titled “requestHasLocale()”](#requesthaslocale)

**Type:** `(context: APIContext) => boolean`

**Added in:** `astro@4.6.0`

Note

Available only when `i18n.routing` is set to `"manual"`

Checks whether the current URL contains a configured locale. Internally, this function will use `APIContext#url.pathname`.

middleware.js

```js
import { defineMiddleware } from "astro:middleware";
import { requestHasLocale } from "astro:i18n";


export const onRequest = defineMiddleware(async (context, next) => {
  if (requestHasLocale(context)) {
    return next();
  }
  return new Response("Not found", { status: 404 });
})
```

### `normalizeTheLocale()`

[Section titled “normalizeTheLocale()”](#normalizethelocale)

**Type:** `(locale: string) => string`

Replaces underscores (`_`) with hyphens (`-`) in the given locale before returning a lowercase version.

src/pages/index.astro

```astro
---
import { normalizeTheLocale } from "astro:i18n";


normalizeTheLocale("it_VT") // returns `it-vt`
// Assuming the current locale is `"pt-PT"`:
normalizeTheLocale(Astro.currentLocale) // returns `pt-pt`
---
```

### `pathHasLocale()`

[Section titled “pathHasLocale()”](#pathhaslocale)

**Type:** `(path: string) => boolean`

**Added in:** `astro@4.6.0`

Checks whether the given path contains a configured locale.

This is useful to prevent errors before using an i18n utility that relies on a locale from a URL path.

astro.config.mjs

```js
export default defineConfig({
  i18n: {
    locales: [
      { codes: ["it-VT", "it"], path: "italiano" },
      "es"
    ]
  }
})
```

src/pages/index.astro

```astro
---
import { pathHasLocale } from "astro:i18n";


pathHasLocale("italiano"); // returns `true`
pathHasLocale("es"); // returns `true`
pathHasLocale('/es/blog/'); // returns `true`
pathHasLocale("it-VT"); // returns `false`
---
```

### `toCodes()`

[Section titled “toCodes()”](#tocodes)

**Type:** `(locales: Locales) => string[]`

**Added in:** `astro@4.0.0`

Retrieves the configured locale codes for each locale defined in your configuration. When multiple codes are associated to a locale, only the first one will be added to the array.

astro.config.mjs

```js
export default defineConfig({
  i18n: {
    locales: [
      { codes: ["it-VT", "it"], path: "italiano" },
      "es"
    ]
  }
})
```

src/pages/index.astro

```astro
---
import { i18n } from "astro:config/client";
import { toCodes } from "astro:i18n";


toCodes(i18n!.locales); // ["it-VT", "es"]
---
```

### `toPaths()`

[Section titled “toPaths()”](#topaths)

**Type:** `(locales: Locales) => string[]`

**Added in:** `astro@4.0.0`

Retrieves the configured locale paths for each locale defined in your configuration.

astro.config.mjs

```js
export default defineConfig({
  i18n: {
    locales: [
      { codes: ["it-VT", "it"], path: "italiano" },
      "es"
    ]
  }
})
```

src/pages/index.astro

```astro
---
import { i18n } from "astro:config/client";
import { toPaths } from "astro:i18n";


toPaths(i18n!.locales); // ["italiano", "es"]
---
```

# Middleware API Reference

**Added in:** `astro@2.6.0`

Middleware allows you to intercept requests and responses and inject behaviors dynamically every time a page or endpoint is about to be rendered. For features and usage examples, [see our middleware guide](/en/guides/middleware/).

## Imports from `astro:middleware`

[Section titled “Imports from astro:middleware”](#imports-from-astromiddleware)

The following helpers are imported from the virtual middleware module:

```js
import {
  defineMiddleware,
  sequence,
} from 'astro:middleware';
```

### `defineMiddleware()`

[Section titled “defineMiddleware()”](#definemiddleware)

**Type:** `(fn: MiddlewareHandler) => MiddlewareHandler`

A function for defining a middleware function with type safety. When you use this utility, the [`context`](#context) and [`next()`](#next) arguments are automatically typed, and you will get a Typescript error if you try to return a [value not supported in your middleware](#middlewarehandler).

src/middleware.ts

```ts
import { defineMiddleware } from "astro:middleware";


export const onRequest = defineMiddleware((context, next) => {
  /* your middleware logic */
});
```

### `sequence()`

[Section titled “sequence()”](#sequence)

**Type:** `(…handlers: MiddlewareHandler[]) => MiddlewareHandler`

A function that accepts middleware functions as arguments, and will execute them in the order in which they are passed.

src/middleware.js

```js
import { sequence } from "astro:middleware";


async function validation(context, next) {/* ... */}
async function auth(context, next) {/* ... */}
async function greeting(context, next) {/* ... */}


export const onRequest = sequence(validation, auth, greeting);
```

## Imports from `astro/middleware`

[Section titled “Imports from astro/middleware”](#imports-from-astromiddleware-1)

The following helpers can be imported from the regular middleware module when you build an [Astro Integration](/en/reference/integrations-reference/):

```js
import {
  createContext,
  defineMiddleware,
  sequence,
  trySerializeLocals,
} from "astro/middleware";
```

### `createContext()`

[Section titled “createContext()”](#createcontext)

**Type:** `(context: CreateContext) => APIContext`

**Added in:** `astro@2.8.0`

A low-level API to create an [`APIContext`](/en/reference/api-reference/)to be passed to an Astro middleware [`onRequest()` function](#onrequest).

This function can be used by integrations/adapters to programmatically execute the Astro middleware.

### `defineMiddleware()`

[Section titled “defineMiddleware()”](#definemiddleware-1)

See [`defineMiddleware()`](#definemiddleware) from `astro:middleware`.

### `sequence()`

[Section titled “sequence()”](#sequence-1)

See [`sequence()`](#sequence) from `astro:middleware`.

### `trySerializeLocals()`

[Section titled “trySerializeLocals()”](#tryserializelocals)

**Type:** `(value: unknown) => string`

**Added in:** `astro@2.8.0`

A low-level API that takes in any value and tries to return a serialized version (a string) of it. If the value cannot be serialized, the function will throw a runtime error.

## `astro/middleware` types

[Section titled “astro/middleware types”](#astromiddleware-types)

The following types are imported from the regular middleware module:

```js
import type {
  CreateContext,
} from "astro/middleware";
```

### `CreateContext`

[Section titled “CreateContext”](#createcontext-1)

**Type:** `{ request: Request; params?: Params; userDefinedLocales?: string[]; defaultLocale: string; locals: App.Locals; clientAddress?: string }`

**Added in:** `astro@2.8.0`

An object to [create a context](#createcontext) to be passed to an Astro middleware. This contains the following properties:

#### `CreateContext.request`

[Section titled “CreateContext.request”](#createcontextrequest)

**Type:** `Request`

The incoming [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) object.

#### `CreateContext.params`

[Section titled “CreateContext.params”](#createcontextparams)

**Type:** `Params`

An object containing the optional parameters to be passed to [`Astro.params`](/en/reference/api-reference/#params).

#### `CreateContext.userDefinedLocales`

[Section titled “CreateContext.userDefinedLocales”](#createcontextuserdefinedlocales)

**Type:** `string[]`

**Added in:** `astro@3.5.0`

A list of supported locales defined in the [user’s `i18n` configuration](/en/reference/configuration-reference/#i18nlocales).

#### `CreateContext.defaultLocale`

[Section titled “CreateContext.defaultLocale”](#createcontextdefaultlocale)

**Type:** `string`

**Added in:** `astro@4.16.0`

The default locale defined in the [user’s `i18n` configuration](/en/reference/configuration-reference/#i18ndefaultlocale).

#### `CreateContext.locals`

[Section titled “CreateContext.locals”](#createcontextlocals)

**Type:** `App.Locals`

**Added in:** `astro@5.0.0`

An object for storing arbitrary information from a middleware, accessible to the user via [`Astro.locals`](/en/reference/api-reference/#locals).

Learn more about [storing data in `locals`](/en/guides/middleware/#storing-data-in-contextlocals) with example usage.

#### `CreateContext.clientAddress`

[Section titled “CreateContext.clientAddress”](#createcontextclientaddress)

**Type:** `string`

**Added in:** `astro@6.0.0`

The [IP address](https://en.wikipedia.org/wiki/IP_address) of the request.

This must be provided by the adapter or platform from a trusted source (e.g. socket address, platform-provided header).

If not provided, accessing `clientAddress` will throw an error.

## `astro` types

[Section titled “astro types”](#astro-types)

```js
import type {
  MiddlewareHandler,
  MiddlewareNext,
  RewritePayload,
} from "astro";
```

### `MiddlewareHandler`

[Section titled “MiddlewareHandler”](#middlewarehandler)

**Type:** `(context: APIContext, next: MiddlewareNext) => Promise<Response> | Response | Promise<void> | void`

Represents an Astro middleware function. Middleware handlers receive two arguments and can either return a `Response` directly or call `next()` to invoke the next middleware in the chain. Alternatively, you can use [`defineMiddleware()`](#definemiddleware) to get type safety for your middleware.

The following example imports the `MiddlewareHandler` type to get type safety in the [`onRequest()`](#onrequest) function:

src/middleware.ts

```ts
import type { MiddlewareHandler } from "astro";


export const onRequest: MiddlewareHandler = (context, next) => {
  /* the middleware logic */
};
```

A middleware handler receives the following properties:

#### `context`

[Section titled “context”](#context)

**Type:** [`APIContext`](/en/reference/api-reference/)

An [Astro context](/en/reference/api-reference/) object mirroring many of the `Astro` global properties.

#### `next()`

[Section titled “next()”](#next)

**Type:** [`MiddlewareNext`](#middlewarenext)

A function that calls all the subsequent middleware in the chain and returns a `Response`. For example, other middleware could modify the HTML body of a response and awaiting the result of `next()` would allow your middleware to respond to those changes.

Since Astro v4.13.0, `next()` accepts an optional URL path parameter in the form of a string, `URL`, or `Request` to [rewrite](/en/guides/routing/#rewrites) the current request without retriggering a new rendering phase.

The following example uses `next()` to serve content from a different path when the current path matches `/old-path`:

src/middleware.ts

```ts
import type { MiddlewareHandler } from "astro";


export const onRequest: MiddlewareHandler = (context, next) => {
  if (context.url.pathname === '/old-path') {
    return next('/new-path');
  }
  return next();
};
```

### `MiddlewareNext`

[Section titled “MiddlewareNext”](#middlewarenext)

**Type:** `(rewritePayload?: RewritePayload) => Promise<Response>`

Represents the [`next()` function](#next) passed to middleware handlers.

### `RewritePayload`

[Section titled “RewritePayload”](#rewritepayload)

**Type:** `string | URL | Request`

**Added in:** `astro@4.13.0`

Represents the destination for a [rewrite](/en/guides/routing/#rewrites) when passed to the [`next()`](#next) function.

## Middleware exports

[Section titled “Middleware exports”](#middleware-exports)

When defining your project’s middleware in `src/middleware.js`, export the following user-defined functions:

### `onRequest()`

[Section titled “onRequest()”](#onrequest)

**Type:** [`MiddlewareHandler`](#middlewarehandler)

A required exported function from `src/middleware.js` that will be called before rendering every page or API route. It receives two arguments: [`context`](#context) and [`next()`](#next). `onRequest()` must return a `Response`: either directly, or by calling `next()`.

src/middleware.js

```js
export function onRequest (context, next) {
  // intercept response data from a request
  // optionally, transform the response
  // return a Response directly, or the result of calling `next()`
  return next();
};
```

# Static Paths API Reference

**Added in:** `astro@6.0.0`

This module provides utilities to help adapters collect static paths from within their target runtime (e.g. `workerd`). This only provides a real implementation in the `prerender` Vite environment. In other environments, it returns a no-op implementation.

## Imports from `astro:static-paths`

[Section titled “Imports from astro:static-paths”](#imports-from-astrostatic-paths)

```js
import {
  StaticPaths,
} from 'astro:static-paths';
```

### `StaticPaths`

[Section titled “StaticPaths”](#staticpaths)

Allows adapters to collect all paths that need to be prerendered from within their target runtime. This is useful when [implementing a custom prerenderer](/en/reference/adapter-reference/#custom-prerenderer) that runs in a non-Node environment:

The `StaticPaths` constructor accepts a required [SSR manifest](/en/reference/integrations-reference/#ssrmanifest) and an object describing the route cache and providing a method to access the component used to render the route. The preferred method to initiate a `StaticPaths` instance is to pass it an [app instance](/en/reference/modules/astro-app/#the-app-instance).

The following example initializes a `StaticPaths` instance from an app in an adapter server entrypoint:

my-adapter/server.js

```js
import { createApp } from 'astro/app/entrypoint';
import { StaticPaths } from 'astro:static-paths';


const app = createApp();
const staticPaths = new StaticPaths(app);


export const handler = (event, context) => {
  // do something with `staticPaths`
};
```

#### `StaticPaths.getAll()`

[Section titled “StaticPaths.getAll()”](#staticpathsgetall)

**Type:** `() => Promise<Array<{ pathname: string, route: RouteData }>>`

Retrieves all paths that should be prerendered. This returns a promise that resolves to an array of objects describing the route path and its data.

The following example collects all static paths to be pre-rendered before returning them as `Response` in an adapter handler:

my-adapter/handler.js

```js
import { StaticPaths } from 'astro:static-paths';


export function createHandler(app) {
  return async (request) => {
    const { pathname } = new URL(request.url);


    // Endpoint to collect static paths during build
    if (pathname === '/__astro_static_paths') {
      const staticPaths = new StaticPaths(app);
      const paths = await staticPaths.getAll();
      // Returns array of { pathname: string, route: RouteData }
      return new Response(JSON.stringify({ paths }));
    }


    // ... handle other requests
  };
}
```

# View Transitions Router API Reference

**Added in:** `astro@3.0.0`

These modules provide functions to control and interact with the View Transitions API and client-side router.

Note

This API is compatible with the `<ClientRouter />` included in `astro:transitions`, but can’t be used with native browser MPA routing.

For features and usage examples, [see our View Transitions guide](/en/guides/view-transitions/).

## Imports from `astro:transitions`

[Section titled “Imports from astro:transitions”](#imports-from-astrotransitions)

```ts
import {
  ClientRouter,
  fade,
  slide,
} from 'astro:transitions';
```

### `<ClientRouter />`

[Section titled “\<ClientRouter />”](#clientrouter-)

**Added in:** `astro@5.0.0`

Opt in to using view transitions on individual pages by importing and adding the `<ClientRouter />` routing component to `<head>` on every desired page.

src/pages/index.astro

```diff
---
+import { ClientRouter } from 'astro:transitions';
---
<html lang="en">
  <head>
    <title>My Homepage</title>
    +<ClientRouter />
  </head>
  <body>
    <h1>Welcome to my website!</h1>
  </body>
</html>
```

See more about how to [control the router](/en/guides/view-transitions/#router-control) and [add transition directives](/en/guides/view-transitions/#transition-directives) to page elements and components.

The `<ClientRouter />` component accepts the following props:

#### `fallback`

**Type:** [`Fallback`](#fallback)\
**Default:** `animate`

Defines the fallback strategy to use for browsers that do not support the [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API).

### `fade`

[Section titled “fade”](#fade)

**Type:** `(opts: { duration?: string | number }) => TransitionDirectionalAnimations`

**Added in:** `astro@3.0.0`

Utility function to support customizing the duration of the built-in `fade` animation.

```astro
---
import { fade } from 'astro:transitions';
---


<!-- Fade transition with the default duration -->
<div transition:animate="fade" />


<!-- Fade transition with a duration of 400 milliseconds -->
<div transition:animate={fade({ duration: '0.4s' })} />
```

### `slide`

[Section titled “slide”](#slide)

**Type:** `(opts: { duration?: string | number }) => TransitionDirectionalAnimations`

**Added in:** `astro@3.0.0`

Utility function to support customizing the duration of the built-in `slide` animation.

```astro
---
import { slide } from 'astro:transitions';
---


<!-- Slide transition with the default duration -->
<div transition:animate="slide" />


<!-- Slide transition with a duration of 400 milliseconds -->
<div transition:animate={slide({ duration: '0.4s' })} />
```

## Imports from `astro:transitions/client`

[Section titled “Imports from astro:transitions/client”](#imports-from-astrotransitionsclient)

```ts
import {
  getFallback,
  navigate,
  supportsViewTransitions,
  swapFunctions,
  transitionEnabledOnThisPage,
  /* The following were deprecated in v6: */
  isTransitionBeforePreparationEvent,
  isTransitionBeforeSwapEvent,
  TRANSITION_AFTER_PREPARATION,
  TRANSITION_AFTER_SWAP,
  TRANSITION_BEFORE_PREPARATION,
  TRANSITION_BEFORE_SWAP,
  TRANSITION_PAGE_LOAD,
} from 'astro:transitions/client';
```

### `navigate()`

[Section titled “navigate()”](#navigate)

**Type:** `(href: string, options?: Options) => void`

**Added in:** `astro@3.2.0`

Executes a navigation to the given `href` using the View Transitions API.

This function signature is based on the [`navigate` function from the browser Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation/navigate). Although based on the Navigation API, this function is implemented on top of the [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) to allow for navigation without reloading the page.

`navigate()` does not perform sanitization on the `href` parameter. [Sanitize user input](/en/guides/view-transitions/#navigating-with-user-input) if you use it to determine the URL to navigate to.

#### `history` option

[Section titled “history option”](#history-option)

**Type:** `'auto' | 'push' | 'replace'`\
**Default:** `'auto'`

**Added in:** `astro@3.2.0`

Defines how this navigation should be added to the browser history.

* `'push'`: the router will use `history.pushState` to create a new entry in the browser history.
* `'replace'`: the router will use `history.replaceState` to update the URL without adding a new entry into navigation.
* `'auto'` (default): the router will attempt `history.pushState`, but if the URL cannot be transitioned to, the current URL will remain with no changes to the browser history.

This option follows the [`history` option](https://developer.mozilla.org/en-US/docs/Web/API/Navigation/navigate#history) from the browser Navigation API but simplified for the cases that can happen on an Astro project.

#### `formData` option

[Section titled “formData option”](#formdata-option)

**Type:** `FormData`

**Added in:** `astro@3.5.0`

A `FormData` object for `POST` requests.

When this option is provided, the requests to the navigation target page will be sent as a `POST` request with the form data object as the content.

Submitting an HTML form with view transitions enabled will use this method instead of the default navigation with page reload. Calling this method allows triggering the same behavior programmatically.

#### `info` option

[Section titled “info option”](#info-option)

**Type:** `any`

**Added in:** `astro@3.6.0`

Arbitrary data to be included in the `astro:before-preparation` and `astro:before-swap` events caused by this navigation.

This option mimics the [`info` option](https://developer.mozilla.org/en-US/docs/Web/API/Navigation/navigate#info) from the browser Navigation API.

#### `state` option

[Section titled “state option”](#state-option)

**Type:** `any`

**Added in:** `astro@3.6.0`

Arbitrary data to be associated with the `NavigationHistoryEntry` object created by this navigation. This data can then be retrieved using the [`history.getState` function](https://developer.mozilla.org/en-US/docs/Web/API/NavigationHistoryEntry/getState) from the History API.

This option mimics the [`state` option](https://developer.mozilla.org/en-US/docs/Web/API/Navigation/navigate#state) from the browser Navigation API.

#### `sourceElement` option

[Section titled “sourceElement option”](#sourceelement-option)

**Type:** `Element`

**Added in:** `astro@3.6.0`

The element that triggered this navigation, if any. This element will be available in the following events:

* [`astro:before-preparation`](#astrobefore-preparation-event)
* [`astro:before-swap`](#astrobefore-swap-event)

### `supportsViewTransitions`

[Section titled “supportsViewTransitions”](#supportsviewtransitions)

**Type:** `boolean`

**Added in:** `astro@3.2.0`

Whether or not view transitions are supported and enabled in the current browser.

### `transitionEnabledOnThisPage()`

[Section titled “transitionEnabledOnThisPage()”](#transitionenabledonthispage)

**Type:** `() => boolean`

**Added in:** `astro@3.2.0`

Whether or not the current page has view transitions enabled for client-side navigation. This can be used to make components that behave differently when they are used on pages with view transitions.

### `getFallback()`

[Section titled “getFallback()”](#getfallback)

**Type:** `() => Fallback`\
**Default:** `animate`

**Added in:** `astro@3.6.0`

Returns the fallback strategy to use (`animate` by default) in browsers that do not support view transitions.

See the guide on [Fallback control](/en/guides/view-transitions/#fallback-control) for how to choose and configure the fallback behavior.

### `swapFunctions`

[Section titled “swapFunctions”](#swapfunctions)

**Type:** `object`

**Added in:** `astro@4.15.0`

An object containing the utility functions used to build Astro’s default swap function. These can be useful when [building a custom swap function](/en/guides/view-transitions/#building-a-custom-swap-function).

`swapFunctions` provides the following methods:

#### `deselectScripts()`

[Section titled “deselectScripts()”](#deselectscripts)

**Type:** `(newDocument: Document) => void`

Marks scripts in the new document that should not be executed. Those scripts are already in the current document and are not flagged for re-execution using [`data-astro-rerun`](/en/guides/view-transitions/#data-astro-rerun).

#### `swapRootAttributes()`

[Section titled “swapRootAttributes()”](#swaprootattributes)

**Type:** `(newDocument: Document) => void`

Swaps the attributes between the document roots, like the `lang` attribute. This also includes Astro-injected internal attributes like `data-astro-transition`, which makes the transition direction available to Astro-generated CSS rules.

When making a custom swap function, it is important to call this function so as not to break the view transition’s animations.

#### `swapHeadElements()`

[Section titled “swapHeadElements()”](#swapheadelements)

**Type:** `(newDocument: Document) => void`

Removes every element from the current document’s `<head>` that is not persisted to the new document. Then appends all new elements from the new document’s `<head>` to the current document’s `<head>`.

#### `saveFocus()`

[Section titled “saveFocus()”](#savefocus)

**Type:** `() => () => void`

Stores the element in focus on the current page and returns a function that when called, if the focused element was persisted, returns the focus to it.

#### `swapBodyElement()`

[Section titled “swapBodyElement()”](#swapbodyelement)

**Type:** `(newBody: Element, oldBody: Element) => void`

Replaces the old body with the new body. Then, goes through every element in the old body that should be persisted and have a matching element in the new body and swaps the old element back in place.

### Deprecated imports

[Section titled “Deprecated imports”](#deprecated-imports)

The following imports are deprecated in v6 and will be removed in v7. You can still use them in your project, but you may prefer to update your code now. [See how to upgrade](/en/guides/upgrade-to/v6/#deprecated-exposed-astrotransitions-internals).

#### `isTransitionBeforePreparationEvent()`

**Type:** `(value: any) => boolean`

**Added in:** `astro@3.6.0`

Deprecated

This function is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

Determines whether the given value matches a [`TransitionBeforePreparationEvent`](#transitionbeforepreparationevent). This can be useful when you need to narrow the type of an event in an event listener.

src/pages/index.astro

```astro
---
---


<script>
  import {
    isTransitionBeforePreparationEvent,
    TRANSITION_BEFORE_PREPARATION,
  } from "astro:transitions/client";


  function listener(event: Event) {
    const setting = isTransitionBeforePreparationEvent(event) ? 1 : 2;
    /* do something with setting */
  }


  document.addEventListener(TRANSITION_BEFORE_PREPARATION, listener);
</script>
```

#### `isTransitionBeforeSwapEvent()`

**Type:** `(value: any) => boolean`

**Added in:** `astro@3.6.0`

Deprecated

This function is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

Determines whether the given value matches a [`TransitionBeforeSwapEvent`](#transitionbeforeswapevent). This can be useful when you need to narrow the type of an event in an event listener.

src/pages/index.astro

```astro
---
---


<script>
  import {
    isTransitionBeforeSwapEvent,
    TRANSITION_BEFORE_SWAP,
  } from "astro:transitions/client";


  function listener(event: Event) {
    const setting = isTransitionBeforeSwapEvent(event) ? 1 : 2;
    /* do something with setting */
  }


  document.addEventListener(TRANSITION_BEFORE_SWAP, listener);
</script>
```

#### `TRANSITION_BEFORE_PREPARATION`

**Type:** `'astro:before-preparation'`

**Added in:** `astro@3.6.0`

Deprecated

This constant is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

A constant to avoid writing the `astro:before-preparation` event name in plain text when you define an event.

src/pages/index.astro

```astro
---
---


<script>
  import { TRANSITION_BEFORE_PREPARATION } from "astro:transitions/client";


  document.addEventListener(TRANSITION_BEFORE_PREPARATION, () => {
    /* the listener logic */
  });
</script>
```

#### `TRANSITION_AFTER_PREPARATION`

**Type:** `'astro:after-preparation'`

**Added in:** `astro@3.6.0`

Deprecated

This constant is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

A constant to avoid writing the `astro:after-preparation` event name in plain text when you define an event.

src/pages/index.astro

```astro
---
---


<script>
  import { TRANSITION_AFTER_PREPARATION } from "astro:transitions/client";


  document.addEventListener(TRANSITION_AFTER_PREPARATION, () => {
    /* the listener logic */
  });
</script>
```

#### `TRANSITION_BEFORE_SWAP`

**Type:** `'astro:before-swap'`

**Added in:** `astro@3.6.0`

Deprecated

This constant is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

A constant to avoid writing the `astro:before-swap` event name in plain text when you define an event.

src/pages/index.astro

```astro
---
---


<script>
  import { TRANSITION_BEFORE_SWAP } from "astro:transitions/client";


  document.addEventListener(TRANSITION_BEFORE_SWAP, () => {
    /* the listener logic */
  });
</script>
```

#### `TRANSITION_AFTER_SWAP`

**Type:** `'astro:after-swap'`

**Added in:** `astro@3.6.0`

Deprecated

This constant is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

A constant to avoid writing the `astro:after-swap` event name in plain text when you define an event.

src/pages/index.astro

```astro
---
---


<script>
  import { TRANSITION_AFTER_SWAP } from "astro:transitions/client";


  document.addEventListener(TRANSITION_AFTER_SWAP, () => {
    /* the listener logic */
  });
</script>
```

#### `TRANSITION_PAGE_LOAD`

**Type:** `'astro:page-load'`

**Added in:** `astro@3.6.0`

Deprecated

This constant is deprecated in v6 and will be removed in v7. You can still use it in your project, but you may prefer to update your code now.

A constant to avoid writing the `astro:page-load` event name in plain text when you define an event.

src/pages/index.astro

```astro
---
---


<script>
  import { TRANSITION_PAGE_LOAD } from "astro:transitions/client";


  document.addEventListener(TRANSITION_PAGE_LOAD, () => {
    /* the listener logic */
  });
</script>
```

## `astro:transitions/client` types

[Section titled “astro:transitions/client types”](#astrotransitionsclient-types)

```ts
import type {
  Direction,
  Fallback,
  NavigationTypeString,
  Options,
  TransitionBeforePreparationEvent,
  TransitionBeforeSwapEvent,
} from 'astro:transitions/client';
```

### `Direction`

[Section titled “Direction”](#direction)

**Type:** `'forward' | 'back'`

**Added in:** `astro@3.2.0`

A union of animation directions:

* `forward`: navigating to the next page in the history or to a new page.
* `back`: navigating to the previous page in the history.

### `Fallback`

[Section titled “Fallback”](#fallback)

**Type:** `'none' | 'animate' | 'swap'`

**Added in:** `astro@3.2.0`

A union of fallback strategies to use in browsers that do not support view transitions:

* `animate`: Astro will simulate view transitions using custom attributes before updating page content.
* `swap`: Astro will not attempt to animate the page. Instead, the old page will be immediately replaced by the new one.
* `none`: Astro will not do any animated page transitions at all. Instead, you will get full page navigation in non-supporting browsers.

Learn more about [controlling the fallback strategy](/en/guides/view-transitions/#fallback-control) with the `ClientRouter`.

### `NavigationTypeString`

[Section titled “NavigationTypeString”](#navigationtypestring)

**Type:** `'push' | 'replace' | 'traverse'`

**Added in:** `astro@3.6.0`

A union of supported history navigation events.

### `TransitionBeforePreparationEvent`

[Section titled “TransitionBeforePreparationEvent”](#transitionbeforepreparationevent)

**Type:** `Event`

**Added in:** `astro@3.6.0`

Represents an [`astro:before-preparation` event](#astrobefore-preparation-event). This can be useful to type the event received by a listener:

src/pages/index.astro

```astro
---
---


<script>
  import type { TransitionBeforePreparationEvent } from "astro:transitions/client";


  function listener(event: TransitionBeforePreparationEvent) {
    /* do something */
  }


  document.addEventListener("astro:before-preparation", listener);
</script>
```

### `TransitionBeforeSwapEvent`

[Section titled “TransitionBeforeSwapEvent”](#transitionbeforeswapevent)

**Type:** `Event`

**Added in:** `astro@3.6.0`

Represents an [`astro:before-swap` event](#astrobefore-swap-event). This can be useful to type the event received by a listener:

src/pages/index.astro

```astro
---
---


<script>
  import type { TransitionBeforeSwapEvent } from "astro:transitions/client";


  function listener(event: TransitionBeforeSwapEvent) {
    /* do something */
  }


  document.addEventListener("astro:before-swap", listener);
</script>
```

## Lifecycle events

[Section titled “Lifecycle events”](#lifecycle-events)

### `astro:before-preparation` event

[Section titled “astro:before-preparation event”](#astrobefore-preparation-event)

**Type:** [`TransitionBeforePreparationEvent`](#transitionbeforepreparationevent)

**Added in:** `astro@3.6.0`

An event dispatched at the beginning of a navigation using the View Transitions router. This event happens before any request is made and any browser state is changed.

This event has the attributes:

* [`info`](#info)
* [`sourceElement`](#sourceelement)
* [`navigationType`](#navigationtype)
* [`direction`](#direction-1)
* [`from`](#from)
* [`to`](#to)
* [`formData`](#formdata)
* [`loader()`](#loader)

Read more about how to use this event on the [View Transitions guide](/en/guides/view-transitions/#astrobefore-preparation).

### `astro:after-preparation` event

[Section titled “astro:after-preparation event”](#astroafter-preparation-event)

**Type:** `Event`

**Added in:** `astro@3.6.0`

An event dispatched after the next page in a navigation using View Transitions router is loaded.

This event has no attributes.

Read more about how to use this event on the [View Transitions guide](/en/guides/view-transitions/#astroafter-preparation).

### `astro:before-swap` event

[Section titled “astro:before-swap event”](#astrobefore-swap-event)

**Type:** [`TransitionBeforeSwapEvent`](#transitionbeforeswapevent)

**Added in:** `astro@3.6.0`

An event dispatched after the next page is parsed, prepared, and linked into a document in preparation for the transition but before any content is swapped between the documents.

This event can’t be canceled. Calling `preventDefault()` is a no-op.

This event has the attributes:

* [`info`](#info)
* [`sourceElement`](#sourceelement)
* [`navigationType`](#navigationtype)
* [`direction`](#direction-1)
* [`from`](#from)
* [`to`](#to)
* [`viewTransition`](#viewtransition)
* [`swap()`](#swap)

Read more about how to use this event on the [View Transitions guide](/en/guides/view-transitions/#astrobefore-swap).

### `astro:after-swap` event

[Section titled “astro:after-swap event”](#astroafter-swap-event)

**Type:** `Event`

An event dispatched after the contents of the page have been swapped but before the view transition ends.

The history entry and scroll position have already been updated when this event is triggered.

### `astro:page-load` event

[Section titled “astro:page-load event”](#astropage-load-event)

**Type:** `Event`

An event dispatched after a page completes loading, whether from a navigation using view transitions or native to the browser.

When view transitions is enabled on the page, code that would normally execute on `DOMContentLoaded` should be changed to execute on this event.

### Lifecycle events attributes

[Section titled “Lifecycle events attributes”](#lifecycle-events-attributes)

**Added in:** `astro@3.6.0`

The following attributes are common to both the [`astro:before-preparation`](#astrobefore-preparation-event) and [`astro:before-swap`](#astrobefore-swap-event) events, except for some that are only available with one or the other.

#### `info`

[Section titled “info”](#info)

**Type:** `any`

Arbitrary data defined during navigation.

This is the literal value passed on the [`info` option](#info-option) of the [`navigate()` function](#navigate).

#### `sourceElement`

[Section titled “sourceElement”](#sourceelement)

**Type:** `Element | undefined`

The element that triggered the navigation. This can be, for example, an `<a>` element that was clicked.

When using the [`navigate()` function](#navigate), this will be the element specified in the call.

#### `newDocument`

[Section titled “newDocument”](#newdocument)

**Type:** `Document`

The document for the next page in the navigation. The contents of this document will be swapped in place of the contents of the current document.

#### `navigationType`

[Section titled “navigationType”](#navigationtype)

**Type:** [`NavigationTypeString`](#navigationtypestring)

Which kind of history navigation is happening.

* `push`: a new `NavigationHistoryEntry` is being created for the new page.
* `replace`: the current `NavigationHistoryEntry` is being replaced with an entry for the new page.
* `traverse`: no `NavigationHistoryEntry` is created. The position in the history is changing. The direction of the traversal is given on the [`direction` attribute](#direction-1).

#### `direction`

[Section titled “direction”](#direction-1)

**Type:** `string`

The direction of the transition:

* In an [`astro:before-preparation` event](#astrobefore-preparation-event), this can be used to define custom directions. The property is writable and accepts any string.
* In an [`astro:before-swap` event](#astrobefore-swap-event), this can be used to retrieve the transition direction. The property is readonly and its value can be a predefined [`Direction`](#direction) or any string that an `astro:before-preparation` event listener might have set.

#### `from`

[Section titled “from”](#from)

**Type:** `URL`

The URL of the page initiating the navigation.

#### `to`

[Section titled “to”](#to)

**Type:** `URL`

The URL of the page being navigated to. This property can be modified, the value at the end of the lifecycle will be used in the `NavigationHistoryEntry` for the next page.

#### `formData`

[Section titled “formData”](#formdata)

**Type:** `FormData | undefined`\
**Available in:** [`astro:before-preparation` event](#astrobefore-preparation-event)

When set, a `POST` request will be sent to the [`to` URL](#to) with the given `FormData` object as the content instead of the normal `GET` request.

When submitting an HTML form with view transitions enabled, this field is automatically set to the data in the form. When using the [`navigate()` function](#navigate), this value is the same as given in the options.

#### `loader()`

[Section titled “loader()”](#loader)

**Type:** `() => Promise<void>`\
**Available in:** [`astro:before-preparation` event](#astrobefore-preparation-event)

Implementation of the following phase in the navigation (loading the next page). This implementation can be overridden to add extra behavior.

#### `viewTransition`

[Section titled “viewTransition”](#viewtransition)

**Type:** [`ViewTransition`](https://developer.mozilla.org/en-US/docs/Web/API/ViewTransition)\
**Available in:** [`astro:before-swap` event](#astrobefore-swap-event)

The view transition object used in this navigation. On browsers that do not support the [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API), this is an object implementing the same API for convenience but without the DOM integration.

#### `swap()`

[Section titled “swap()”](#swap)

**Type:** `() => void`\
**Available in:** [`astro:before-swap` event](#astrobefore-swap-event)

Calls the default document swap logic. By default, this implementation will call the following functions in order:

1. [`deselectScripts()`](#deselectscripts)
2. [`swapRootAttributes()`](#swaprootattributes)
3. [`swapHeadElements()`](#swapheadelements)
4. [`saveFocus()`](#savefocus)
5. [`swapBodyElement()`](#swapbodyelement)

Read more about [building a custom swap function](/en/guides/view-transitions/#building-a-custom-swap-function) in the View Transitions guide.

# Zod API Reference

[Zod](https://github.com/colinhacks/zod) is a TypeScript-based schema declaration and validation library. This allows you to define schemas you can use to validate data and transform data, from a simple type (e.g. `string`, `number`) to complex data structures (e.g. nested objects).

The `astro/zod` module exposes a re-export of Zod that gives you access to all the features of Zod v4. By using this module, you do not need to install Zod yourself. This also ensures that your project uses the same API versions as Astro when using features such as [Content Collections](/en/guides/content-collections/) or [Actions](/en/guides/actions/).

See the [Zod website](https://zod.dev/) for complete documentation on how Zod works and what features are available.

## Imports from `astro/zod`

[Section titled “Imports from astro/zod”](#imports-from-astrozod)

```ts
import { z } from 'astro/zod';
```

### `z`

[Section titled “z”](#z)

**Type:** `object`

The `z` utility gives you access to validators for a wide range of data types, methods and types for working with your data.

Learn more about the `z` utility in [Zod documentation](https://zod.dev/basics)

#### Common data type validators

[Section titled “Common data type validators”](#common-data-type-validators)

With Zod, you can validate any type of data, such as [primitives](https://zod.dev/api#primitives), [objects](https://zod.dev/api#objects), [arrays](https://zod.dev/api#arrays) and more.

The following example shows a cheatsheet of many common Zod data types to create a `user` schema:

```ts
import { z } from 'astro/zod';


const user = z.object({
  username: z.string(),
  name: z.string().min(2),
  email: z.email(),
  role: z.enum(["admin", "editor"]),
  language: z.enum(["en", "fr", "es"]).default("en"),
  hobbies: z.array(z.string()),
  age: z.number(),
  isEmailConfirmed: z.boolean(),
  inscriptionDate: z.date(),
  website: z.url().optional(),
});
```

#### Extracting a Typescript type

[Section titled “Extracting a Typescript type”](#extracting-a-typescript-type)

Zod allows you to create a Typescript type from any schema [using Zod type inference](https://zod.dev/basics#inferring-types). This can be useful for describing an expected data structure when [defining component props](/en/guides/typescript/#component-props).

The following example create a `User` type based on the previous schema:

```ts
type User = z.infer<typeof user>;


/* The `User` type will be:
 * type User = {
 *   username: string;
 *   name: string;
 *   email: string;
 *   role: "admin" | "editor";
 *   language: "en" | "fr" | "es";
 *   hobbies: string[];
 *   age: number;
 *   isEmailConfirmed: boolean;
 *   inscriptionDate: Date;
 *   website?: string | undefined;
 * }
 */
```

#### Using Zod methods

[Section titled “Using Zod methods”](#using-zod-methods)

Zod provides various schema methods to [customize error messages](https://zod.dev/error-customization), [transform data](https://zod.dev/api#transforms), or create [custom validation logics](https://zod.dev/api#refinements).

```ts
// Customize the error message
const nonEmptyStrings = z.array(z.string()).nonempty("Can't be empty!");


// Validate a data from a schema
nonEmptyStrings.parse([]); // will throws our custom error


// Create an object from a URL for a decorative img
const decorativeImg = z.string().transform((value) => {
  return { src: value, alt: "" };
});


// Create a custom validator and error message for a string
const constrainedString = z
  .string()
  .refine((val) => val.length > 0 && val.length <= 255, {
    error: "Must be between 1 and 255 characters.",
  });
```

### Individual imports

[Section titled “Individual imports”](#individual-imports)

Alternatively, you can import all the Zod validators, methods and types available in the [`z` utility](#z) directly from the module.

The following example imports `coerce` to create a `Date` object from a date string:

```ts
import { coerce } from 'astro/zod';


const publishedOn = coerce.date();
const publicationDate = publishedOn.parse("2025-12-03");
```

# Programmatic Astro API (experimental)

If you need more control when running Astro, the `"astro"` package exports APIs to programmatically run the CLI commands. There are also two `astro:config` helpers that can programmatically [validate](#validateconfig) and [merge](#mergeconfig) configurations.

These APIs are experimental and their API signature may change. Any updates will be mentioned in the [Astro changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) and the information below will always show the current, up-to-date information.

## Commands

[Section titled “Commands”](#commands)

The following [CLI commands](/en/reference/cli-reference/) can be run programmatically.

### `dev()`

[Section titled “dev()”](#dev)

**Type:** `(inlineConfig: AstroInlineConfig) => Promise<DevServer>`

Similar to [`astro dev`](/en/reference/cli-reference/#astro-dev), it runs Astro’s development server.

```js
import { dev } from "astro";


const devServer = await dev({
  root: "./my-project",
});


// Stop the server if needed
await devServer.stop();
```

#### `DevServer`

[Section titled “DevServer”](#devserver)

```ts
export interface DevServer {
  address: AddressInfo;
  handle: (req: http.IncomingMessage, res: http.ServerResponse<http.IncomingMessage>) => void;
  watcher: vite.FSWatcher;
  stop(): Promise<void>;
}
```

##### `DevServer.address`

[Section titled “DevServer.address”](#devserveraddress)

**Type:** `AddressInfo`

The address the dev server is listening on.

This property contains the value returned by Node’s [`net.Server#address()` method](https://nodejs.org/api/net.html#serveraddress).

##### `DevServer.handle()`

[Section titled “DevServer.handle()”](#devserverhandle)

**Type:** `(req: http.IncomingMessage, res: http.ServerResponse<http.IncomingMessage>) => void`

A handle for raw Node HTTP requests. You can call `handle()` with an [`http.IncomingMessage`](https://nodejs.org/api/http.html#class-httpincomingmessage) and an [`http.ServerResponse`](https://nodejs.org/api/http.html#class-httpserverresponse) instead of sending a request through the network.

##### `DevServer.watcher`

[Section titled “DevServer.watcher”](#devserverwatcher)

**Type:** `vite.FSWatcher`

The [Chokidar file watcher](https://github.com/paulmillr/chokidar#getting-started) as exposed by [Vite’s development server](https://vite.dev/guide/api-javascript#vitedevserver).

##### `DevServer.stop()`

[Section titled “DevServer.stop()”](#devserverstop)

**Type:** `Promise<void>`

Stops the development server. This closes all idle connections and stops listening for new connections.

Returns a `Promise` that resolves once all pending requests have been fulfilled and all idle connections have been closed.

### `build()`

[Section titled “build()”](#build)

**Type:** `(inlineConfig: AstroInlineConfig, options?: BuildOptions) => Promise<void>`

Similar to [`astro build`](/en/reference/cli-reference/#astro-build), it builds your site for deployment.

```js
import { build } from "astro";


await build({
  root: "./my-project",
});
```

#### `BuildOptions`

[Section titled “BuildOptions”](#buildoptions)

```ts
export interface BuildOptions {
  devOutput?: boolean;
  teardownCompiler?: boolean;
}
```

##### `BuildOptions.devOutput`

[Section titled “BuildOptions.devOutput”](#buildoptionsdevoutput)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@5.4.0`

Output a development-based build similar to code transformed in `astro dev`. This can be useful to test build-only issues with additional debugging information included.

##### `BuildOptions.teardownCompiler`

[Section titled “BuildOptions.teardownCompiler”](#buildoptionsteardowncompiler)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@5.4.0`

Teardown the compiler WASM instance after build. This can improve performance when building once but may cause a performance hit if building multiple times in a row.

When building multiple projects in the same execution (e.g. during tests), disabling this option can greatly increase performance and reduce peak memory usage at the cost of higher sustained memory usage.

### `preview()`

[Section titled “preview()”](#preview)

**Type:** `(inlineConfig: AstroInlineConfig) => Promise<PreviewServer>`

Similar to [`astro preview`](/en/reference/cli-reference/#astro-preview), it starts a local server to serve your build output.

If no adapter is set in the configuration, the preview server will only serve the built static files. If an adapter is set in the configuration, the preview server is provided by the adapter. Adapters are not required to provide a preview server, so this feature may not be available depending on your adapter of choice.

```js
import { preview } from "astro";


const previewServer = await preview({
  root: "./my-project",
});


// Stop the server if needed
await previewServer.stop();
```

### `sync()`

[Section titled “sync()”](#sync)

**Type:** `(inlineConfig: AstroInlineConfig) => Promise<void>`

Similar to [`astro sync`](/en/reference/cli-reference/#astro-sync), it generates TypeScript types for all Astro modules.

```js
import { sync } from "astro";


await sync({
  root: "./my-project",
});
```

## Utilities

[Section titled “Utilities”](#utilities)

The following utilities can be [imported from `astro/config`](/en/reference/modules/astro-config/#imports-from-astroconfig) and used to manipulate or validate the configuration before passing it to CLI commands.

### `mergeConfig()`

[Section titled “mergeConfig()”](#mergeconfig)

**Type:** `(defaults: AstroConfig, overrides: DeepPartial<AstroConfig>) => AstroConfig`

**Added in:** `astro@5.4.0`

Takes an Astro configuration object and a partial object containing any set of valid Astro configuration options, and returns a valid Astro configuration combining the two values such that:

* Arrays are concatenated (including integrations and remark plugins).
* Objects are merged recursively.
* Vite options are merged using [Vite’s own `mergeConfig()` function](https://vite.dev/guide/api-javascript#mergeconfig) with the default `isRoot` flag.
* Options that can be provided as functions are wrapped into new functions that recursively merge the return values from both configurations with these same rules.
* All other options override the existing config.

```ts
import { mergeConfig } from "astro/config";


mergeConfig(
  {
    output: "static",
    site: "https://example.com",
    integrations: [partytown()],
    server: ({command}) => ({
      port: command === "dev" ? 4321 : 1234,
    }),
    build: {
      client: "./custom-client",
    },
  },
  {
    output: "server",
    base: "/astro",
    integrations: [mdx()],
    server: ({command}) => ({
      host: command === "dev" ? "localhost" : "site.localhost",
    }),
    build: {
      server: "./custom-server",
    },
  }
);


// Result is equivalent to:
{
  output: "server",
  site: "https://example.com",
  base: "/astro",
  integrations: [partytown(), mdx()],
  server: ({command}) => ({
    port: command === "dev" ? 4321 : 1234,
    host: command === "dev" ? "localhost" : "site.localhost",
  }),
  build: {
    client: "./custom-client",
    server: "./custom-server",
  },
}
```

### `validateConfig()`

[Section titled “validateConfig()”](#validateconfig)

**Type:** `(userConfig: any, root: string, cmd: string) => Promise<AstroConfig>`

**Added in:** `astro@5.4.0`

Validates an object as if it were exported from `astro.config.mjs` and imported by Astro. This takes the following arguments:

* The configuration to be validated.
* The [root directory of the project](/en/reference/configuration-reference/#root).
* The [Astro command that is being executed](/en/reference/cli-reference/#astro-commands) (e.g. `build`, `dev`, `sync`)

The returned promise resolves to the validated configuration, filled with all default values appropriate for the given Astro command.

```ts
import { validateConfig } from "astro/config";


const config = await validateConfig({
  integrations: [partytown()],
}, "./my-project", "build");


// defaults are applied
await rm(config.outDir, { recursive: true, force: true });
```

## `astro` types

[Section titled “astro types”](#astro-types)

```ts
import type {
  AstroInlineConfig,
  PreviewServer,
} from "astro";
```

### `AstroInlineConfig`

[Section titled “AstroInlineConfig”](#astroinlineconfig)

The `AstroInlineConfig` type is used by all of the [command APIs](#commands). It extends from the user [Astro config](/en/reference/configuration-reference/) type:

```ts
interface AstroInlineConfig extends AstroUserConfig {
  configFile?: string | false;
  mode?: string;
  logLevel?: "debug" | "info" | "warn" | "error" | "silent";
}
```

#### `AstroInlineConfig.configFile`

[Section titled “AstroInlineConfig.configFile”](#astroinlineconfigconfigfile)

**Type:** `string | false`\
**Default:** `undefined`

A custom path to the Astro config file.

If this value is undefined (default) or unset, Astro will search for an `astro.config.(js,mjs,ts,mts)` file relative to the `root` and load the config file if found.

If a relative path is set, it will resolve based on the `root` option.

Set to `false` to disable loading any config files.

The inline config passed in this object will take highest priority when merging with the loaded user config.

#### `AstroInlineConfig.mode`

[Section titled “AstroInlineConfig.mode”](#astroinlineconfigmode)

**Type:** `string`\
**Default:** `"development"` when running `astro dev`, `"production"` when running `astro build`

**Added in:** `astro@5.0.0`

The mode used when developing or building your site (e.g. `"production"`, `"testing"`).

This value is passed to Vite using [the `--mode` flag](/en/reference/cli-reference/#--mode-string) when the `astro build` or `astro dev` commands are run to determine the value of `import.meta.env.MODE`. This also determines which `.env` files are loaded, and therefore the values of `astro:env`. See the [environment variables page](/en/guides/environment-variables/) for more details.

To output a development-based build, you can run `astro build` with the [`--devOutput` flag](/en/reference/cli-reference/#--devoutput).

#### `AstroInlineConfig.logLevel`

[Section titled “AstroInlineConfig.logLevel”](#astroinlineconfigloglevel)

**Type:** `"debug" | "info" | "warn" | "error" | "silent"`\
**Default:** `"info"`

The logging level to filter messages logged by Astro.

* `"debug"`: Log everything, including noisy debugging diagnostics.
* `"info"`: Log informational messages, warnings, and errors.
* `"warn"`: Log warnings and errors.
* `"error"`: Log errors only.
* `"silent"`: No logging.

### `PreviewServer`

[Section titled “PreviewServer”](#previewserver)

```ts
export interface PreviewServer {
  host?: string;
  port: number;
  closed(): Promise<void>;
  stop(): Promise<void>;
}
```

#### `PreviewServer.host`

[Section titled “PreviewServer.host”](#previewserverhost)

**Type:** `string`

The host where the server is listening for connections.

Adapters are allowed to leave this field unset. The value of `host` is implementation-specific.

#### `PreviewServer.port`

[Section titled “PreviewServer.port”](#previewserverport)

**Type:** `number`

The port where the server is listening for connections.

#### `PreviewServer.stop()`

[Section titled “PreviewServer.stop()”](#previewserverstop)

**Type:** `Promise<void>`

Asks the preview server to close, stop accepting requests, and drop idle connections.

The returned `Promise` resolves when the close request has been sent. This does not mean that the server has closed yet. Use the [`closed()`](#previewserverclosed) method if you need to ensure the server has fully closed.

#### `PreviewServer.closed()`

[Section titled “PreviewServer.closed()”](#previewserverclosed)

**Type:** `Promise<void>`

Returns a `Promise` that will resolve once the server is closed and reject if an error happens on the server.

# Routing Reference

There is no separate routing configuration in Astro.

Every [supported page file](/en/basics/astro-pages/#supported-page-files) located within the special `src/pages/` directory creates a route. When the file name contains a [parameter](#params), a route can create multiple pages dynamically, otherwise it creates a single page.

By default, all Astro page routes and endpoints are generated and prerendered at build time. [On-demand server rendering](/en/guides/on-demand-rendering/) can be set for individual routes, or as the default.

## `prerender`

[Section titled “prerender”](#prerender)

**Type:** `boolean`\
**Default:** `true` in static mode (default); `false` with `output: 'server'` configuration

**Added in:** `astro@1.0.0`

A value exported from each individual route to determine whether or not it is prerendered.

By default, all pages and endpoints are prerendered and will be statically generated at build time. You can opt out of prerendering on one or more routes, and you can have both static and on-demand rendered routes in the same project.

### Per-page override

[Section titled “Per-page override”](#per-page-override)

You can override the default value to enable [on demand rendering](/en/guides/on-demand-rendering/) for an individual route by exporting `prerender` with the value `false` from that file:

src/pages/rendered-on-demand.astro

```astro
---
export const prerender = false
---
<!-- server-rendered content -->
<!-- the rest of my site is static -->
```

### Switch to `server` mode

[Section titled “Switch to server mode”](#switch-to-server-mode)

You can override the default value for all routes by configuring [`output: 'server'`](/en/reference/configuration-reference/#output). In this output mode, all pages and endpoints will be generated on the server upon request by default instead of being prerendered.

In `server` mode, enable prerendering for an individual route by exporting `prerender` with the value `true` from that file:

src/pages/static-about-page.astro

```astro
---
// with `output: 'server'` configured
export const prerender = true
---
<!-- My static about page -->
<!-- All other pages are rendered on demand -->
```

## `partial`

[Section titled “partial”](#partial)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@3.4.0`

A value exported from an individual route to determine whether or not it should be rendered as a full HTML page.

By default, all files located within the reserved `src/pages/` directory automatically include the `<!DOCTYPE html>` declaration and additional `<head>` content such as Astro’s scoped styles and scripts.

You can override the default value to designate the content as a [page partial](/en/basics/astro-pages/#page-partials) for an individual route by exporting a value for `partial` from that file:

src/pages/my-page-partial.astro

```astro
---
export const partial = true
---
<!-- Generated HTML available at a URL -->
<!-- Available to a rendering library -->
```

The `export const partial` must be identifiable statically. It can have the value of:

* The boolean **`true`**.
* An environment variable using import.meta.env such as `import.meta.env.USE_PARTIALS`.

## `getStaticPaths()`

[Section titled “getStaticPaths()”](#getstaticpaths)

**Type:** `(options: GetStaticPathsOptions) => Promise<GetStaticPathsResult> | GetStaticPathsResult`

**Added in:** `astro@1.0.0`

A function to generate multiple, prerendered page routes from a single `.astro` page component with one or more [parameters](#params) in its file path. Use this for routes that will be created at build time, also known as static site building.

The `getStaticPaths()` function must return an array of objects to determine which URL paths will be prerendered by Astro. Each object must include a `params` object, to specify route paths. The object may optionally contain a `props` object with [data to be passed](#data-passing-with-props) to each page template.

src/pages/blog/\[post].astro

```astro
---
// In 'server' mode, opt in to prerendering:
// export const prerender = true


export async function getStaticPaths() {
  return [
    // { params: { /* required */ }, props: { /* optional */ } },
    { params: { post: '1' } }, // [post] is the parameter
    { params: { post: '2' } }, // must match the file name
    // ...
  ];
}
---
<!-- Your HTML template here. -->
```

`getStaticPaths()` can also be used in static file endpoints for [dynamic routing](/en/guides/endpoints/#params-and-dynamic-routing).

Tip

When using TypeScript, use the [`GetStaticPaths`](/en/guides/typescript/#infer-getstaticpaths-types) type utility to ensure type-safe access of your `params` and `props`.

Caution

The `getStaticPaths()` function executes in its own isolated scope once, before any page loads. Therefore you can’t reference anything from its parent scope, other than file imports. The compiler will warn you if you break this requirement.

### `params`

[Section titled “params”](#params)

The `params` key of each object in the array returned by `getStaticPaths()` tells Astro what routes to build.

The keys in `params` must match the parameters defined in your component file path. The value for each `params` object must match the parameters used in the page name. `params` are encoded into the URL, so only strings are supported as values.

For example,`src/pages/posts/[id].astro`has an `id` parameter in its file name. The following `getStaticPaths()` function in this `.astro` component tells Astro to statically generate `posts/1`, `posts/2`, and `posts/3` at build time.

src/pages/posts/\[id].astro

```astro
---
export async function getStaticPaths() {
  return [
    { params: { id: '1' } },
    { params: { id: '2' } },
    { params: { id: '3' } }
  ];
}


const { id } = Astro.params;
---
<h1>{id}</h1>
```

### Data passing with `props`

[Section titled “Data passing with props”](#data-passing-with-props)

To pass additional data to each generated page, you can set a `props` value on each object in the array returned by `getStaticPaths()`. Unlike `params`, `props` are not encoded into the URL and so aren’t limited to only strings.

For example, if you generate pages with data fetched from a remote API, you can pass the full data object to the page component inside of `getStaticPaths()`. The page template can reference the data from each post using `Astro.props`.

src/pages/posts/\[id].astro

```astro
---
export async function getStaticPaths() {
  const response = await fetch('...');
  const data = await response.json();


  return data.map((post) => {
    return {
      params: { id: post.id },
      props: { post },
    };
  });
}


const { id } = Astro.params;
const { post } = Astro.props;
---
<h1>{id}: {post.name}</h1>
```

### `routePattern`

[Section titled “routePattern”](#routepattern)

**Type:** `string`

**Added in:** `astro@5.14.0`

A property available in [`getStaticPaths()`](#getstaticpaths) options to access the current [`routePattern`](/en/reference/api-reference/#routepattern) as a string.

This provides data from the [Astro render context](/en/reference/api-reference/) that would not otherwise be available within the scope of `getStaticPaths()` and can be useful to calculate the `params` and `props` for each page route.

`routePattern` always reflects the original dynamic segment definition in the file path (e.g. `/[...locale]/[files]/[slug]`), unlike `params`, which are explicit values for a page (e.g. `/fr/fichiers/article-1/`).

The following example shows how to localize your route segments and return an array of static paths by passing `routePattern` to a custom `getLocalizedData()` helper function. The [params](/en/reference/routing-reference/#params) object will be set with explicit values for each route segment: `locale`, `files`, and `slug`. Then, these values will be used to generate the routes and can be used in your page template via `Astro.params`.

src/pages/\[...locale]/\[files]/\[slug].astro

```astro
---
import { getLocalizedData } from "../../../utils/i18n";


export async function getStaticPaths({ routePattern }) {
  const response = await fetch('...');
  const data = await response.json();


  console.log(routePattern); // [...locale]/[files]/[slug]


  // Call your custom helper with `routePattern` to generate the static paths
  return data.flatMap((file) => getLocalizedData(file, routePattern));
}


const { locale, files, slug } = Astro.params;
---
```

### `paginate()`

[Section titled “paginate()”](#paginate)

**Added in:** `astro@1.0.0`

A function that can be returned from [`getStaticPaths()`](#getstaticpaths) to divide a collection of content items into separate pages.

`paginate()` will automatically generate the necessary array to return from `getStaticPaths()` to create one URL for every page of your paginated collection. The page number will be passed as a `param`, and the page data will be passed as a `page` prop.

The following example fetches and passes 150 items to the `paginate` function, and creates static, prerendered pages at build time that will display 10 items per page:

src/pages/pokemon/\[page].astro

```astro
---
export async function getStaticPaths({ paginate }) {
  // Load your data with fetch(), getCollection(), etc.
  const response = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=150`);
  const result = await response.json();
  const allPokemon = result.results;


  // Return a paginated collection of paths for all items
  return paginate(allPokemon, { pageSize: 10 });
}


const { page } = Astro.props;
---
```

`paginate()` has the following arguments:

* `data` - array containing the page’s data passed to the `paginate()` function

* `options` - Optional object with the following properties:

  * `pageSize` - The number of items shown per page (`10` by default)
  * `params` - Send additional parameters for creating dynamic routes
  * `props` - Send additional props to be available on each page

`paginate()` assumes a file name of `[page].astro` or `[...page].astro`. The `page` param becomes the page number in your URL:

* `/posts/[page].astro` would generate the URLs `/posts/1`, `/posts/2`, `/posts/3`, etc.
* `/posts/[...page].astro` would generate the URLs `/posts`, `/posts/2`, `/posts/3`, etc.

#### The pagination `page` prop

[Section titled “The pagination page prop”](#the-pagination-page-prop)

**Type:** `Page<TData>`

Pagination will pass a `page` prop to every rendered page that represents a single page of data in the paginated collection. This includes the data that you’ve paginated (`page.data`) as well as metadata for the page (`page.url`, `page.start`, `page.end`, `page.total`, etc). This metadata is useful for things like a “Next Page” button or a “Showing 1-10 of 100” message.

##### `page.data`

[Section titled “page.data”](#pagedata)

**Type:** `Array<TData>`

Array of data returned from the `paginate()` function for the current page.

##### `page.start`

[Section titled “page.start”](#pagestart)

**Type:** `number`

Index of the first item on the current page, starting at `0`. (e.g. if `pageSize: 25`, this would be `0` on page 1, `25` on page 2, etc.)

##### `page.end`

[Section titled “page.end”](#pageend)

**Type:** `number`

Index of the last item on the current page.

##### `page.size`

[Section titled “page.size”](#pagesize)

**Type:** `number`\
**Default:** `10`

The total number of items per page.

##### `page.total`

[Section titled “page.total”](#pagetotal)

**Type:** `number`

The total number of items across all pages.

##### `page.currentPage`

[Section titled “page.currentPage”](#pagecurrentpage)

**Type:** `number`

The current page number, starting with `1`.

##### `page.lastPage`

[Section titled “page.lastPage”](#pagelastpage)

**Type:** `number`

The total number of pages.

##### `page.url.current`

[Section titled “page.url.current”](#pageurlcurrent)

**Type:** `string`

Get the URL of the current page (useful for canonical URLs). If a value is set for [`base`](/en/reference/configuration-reference/#base), the URL starts with that value.

##### `page.url.prev`

[Section titled “page.url.prev”](#pageurlprev)

**Type:** `string | undefined`

Get the URL of the previous page (will be `undefined` if on page 1). If a value is set for [`base`](/en/reference/configuration-reference/#base), prepend the base path to the URL.

##### `page.url.next`

[Section titled “page.url.next”](#pageurlnext)

**Type:** `string | undefined`

Get the URL of the next page (will be `undefined` if no more pages). If a value is set for [`base`](/en/reference/configuration-reference/#base), prepend the base path to the URL.

##### `page.url.first`

[Section titled “page.url.first”](#pageurlfirst)

**Type:** `string | undefined`

**Added in:** `astro@4.12.0`

Get the URL of the first page (will be `undefined` if on page 1). If a value is set for [`base`](/en/reference/configuration-reference/#base), prepend the base path to the URL.

##### `page.url.last`

[Section titled “page.url.last”](#pageurllast)

**Type:** `string | undefined`

**Added in:** `astro@4.12.0`

Get the URL of the last page (will be `undefined` if no more pages). If a value is set for [`base`](/en/reference/configuration-reference/#base), prepend the base path to the URL.

# Astro Session Driver API

Astro [sessions](/en/guides/sessions/) allow to share data between requests for on-demand rendered pages. They require an Astro Session Driver to store session data.

## Built-in drivers

[Section titled “Built-in drivers”](#built-in-drivers)

Astro exports built-in session drivers from `astro/config`:

```js
import { sessionDrivers } from 'astro/config'
```

Any [unstorage driver](https://unstorage.unjs.io/drivers) can be used, for example:

astro.config.mjs

```diff
import { defineConfig, sessionDrivers } from 'astro/config'


export default defineConfig({
    session: {
+        driver: sessionDrivers.redis({
+            url: process.env.REDIS_URL
+        }),
    }
})
```

Note

Some drivers may need extra packages to be installed. Some drivers may also require environment variables or credentials to be set. See the [Unstorage documentation](https://unstorage.unjs.io/drivers) for more information.

## Building a session driver

[Section titled “Building a session driver”](#building-a-session-driver)

A session driver is made of two parts:

* The [driver config](#the-session-driver-config), which lets Astro know what implementation to use at runtime and what config to forward
* The [driver implementation](#the-session-driver-implementation), which handles the storage logic at runtime

### The session driver config

[Section titled “The session driver config”](#the-session-driver-config)

A `SessionDriverConfig` is an object containing a required runtime [`entrypoint`](#entrypoint) and an optional [`config`](#config). The preferred method for implementing it is to export a function that returns this object and takes the configuration as an optional parameter.

The following example defines a memory driver config:

driver/config.ts

```ts
import type { SessionDriverConfig } from 'astro'


export interface Config {
    max?: number;
}


export function memoryDriver(config: Config = {}): SessionDriverConfig {
    return {
        entrypoint: new URL('./runtime.js', import.meta.url),
        config,
    }
}
```

It is then registered in the Astro config:

astro.config.ts

```ts
import { defineConfig } from 'astro/config'
import { memoryDriver } from './driver/config'


export default defineConfig({
    session: {
        driver: memoryDriver({
            max: 500
        })
    }
})
```

#### `entrypoint`

[Section titled “entrypoint”](#entrypoint)

**Type:** `string | URL`

**Added in:** `astro@6.0.0`

Defines the entrypoint for the [driver implementation](#the-session-driver-implementation).

#### `config`

[Section titled “config”](#config)

**Type:** `Record<string, any> | undefined`

**Added in:** `astro@6.0.0`

Defines the serializable config passed to [driver implementation](#the-session-driver-implementation) at runtime.

### The session driver implementation

[Section titled “The session driver implementation”](#the-session-driver-implementation)

A `SessionDriver` is an object responsible for [storing](#setitem), [retrieving](#getitem) and [deleting](#removeitem) data when [using sessions at runtime](/en/reference/api-reference/#session) (e.g. `context.session.set()`). You can implement it in your session driver module by exporting a default function that takes the [driver config](#config) as parameter.

The following example implements a memory driver:

driver/runtime.ts

```ts
import type { SessionDriver } from 'astro'
import type { Config } from './config'
import { LRUCache } from 'lru-cache'


export default function(config: Config): SessionDriver {
    const cache = new LRUCache({ max: config.max })
    return {
        setItem: async (key, value) => {
            cache.set(key, value)
        },
        getItem: async (key) => {
            return cache.get(key)
        },
        removeItem: async (key) => {
            cache.delete(key)
        },
    }
}
```

#### `setItem()`

[Section titled “setItem()”](#setitem)

**Type:** `(key: string, value: any) => Promise<void>`

**Added in:** `astro@6.0.0`

Defines a function that sets session data by key.

#### `getItem()`

[Section titled “getItem()”](#getitem)

**Type:** `(key: string) => Promise<any>`

**Added in:** `astro@6.0.0`

Defines a function that retrieves session data by key.

#### `removeItem()`

[Section titled “removeItem()”](#removeitem)

**Type:** `(key: string) => Promise<void>`

**Added in:** `astro@6.0.0`

Defines a function that removes session data by key.

## Unstorage compatibility

[Section titled “Unstorage compatibility”](#unstorage-compatibility)

Unstorage driver types are compatible with Astro’s `SessionDriver` type.

That means you can use an unstorage package export as an [entrypoint](#entrypoint). For example:

driver/config.ts

```ts
import type { SessionDriverConfig } from 'astro'


export function configuredRedisDriver(): SessionDriverConfig {
    return {
        entrypoint: 'unstorage/drivers/redis',
        config: {
            tls: true
        }
    }
}
```

Alternatively, you can import and use an unstorage driver directly in the implementation. For example:

driver/runtime.ts

```ts
import type { SessionDriver } from 'astro'
import redisDriver from "unstorage/drivers/redis";


export default function(config): SessionDriver {
    return redisDriver({
        ...config,
        tls: true
    })
}
```

