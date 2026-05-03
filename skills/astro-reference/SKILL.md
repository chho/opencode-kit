---
name: astro-reference
description: Astro API and configuration lookup reference - CLI commands, astro.config.mjs full specification, template expressions, template directives (client:*), error codes, imports reference, and custom fonts. Load this when you need to look up specific APIs, config options, or debug errors.
---

# Astro Reference Documentation

> This skill contains reference material for looking up specific APIs, configuration options, and error diagnostics.


# Using custom fonts

> Looking to add some custom typefaces to an Astro website? Use Google Fonts with Fontsource or add a font of your choice.

This guide will show you how to add [web fonts](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text/Web_fonts) to your project and use them in your components.

Astro provides a way to use fonts from your filesystem and various font providers (e.g. Fontsource, Google) through a unified, [fully customizable](/en/reference/configuration-reference/#fonts), and type-safe API.

Web fonts can impact page performance at both load time and rendering time. This API helps you keep your site performant with automatic [web font optimizations](https://web.dev/learn/performance/optimize-web-fonts) including preload links, optimized fallbacks, and opinionated defaults. [See common usage examples](#examples).

The Fonts API focuses on performance and privacy by downloading and caching fonts so they’re served from your site. This can avoid sending user data to third-party sites, and also ensures that a consistent set of fonts is available to all your visitors.

## Configuring custom fonts

[Section titled “Configuring custom fonts”](#configuring-custom-fonts)

Registering custom fonts for your Astro project is done through [the `fonts` option](/en/reference/configuration-reference/#fonts) in your Astro config.

For each font you want to use, you must specify its [name](/en/reference/configuration-reference/#fontname), a [CSS variable](/en/reference/configuration-reference/#fontcssvariable), and an Astro font provider.

Astro provides [built-in support for the most popular font providers](/en/reference/font-provider-reference/#built-in-providers): Adobe, Bunny, Fontshare, Fontsource, Google, Google Icons and NPM, as well as for using your own local font files. Additionally, you can [further customize your font configuration](#granular-font-configuration) to optimize performance and visitor experience.

### Using a local font file

[Section titled “Using a local font file”](#using-a-local-font-file)

This example will demonstrate adding a custom font using the font file `DistantGalaxy.woff2`.

1. Add your font file inside the [`src/` directory](/en/basics/project-structure/#src), for example `src/assets/fonts/`.

2. Create a new font family in your Astro config file using the [local font provider](/en/reference/font-provider-reference/#local) and specify the variants to be included:

   astro.config.mjs

   ```js
   import { defineConfig, fontProviders } from "astro/config";


   export default defineConfig({
     fonts: [{
       provider: fontProviders.local(),
       name: "DistantGalaxy",
       cssVariable: "--font-distant-galaxy",
       options: {
         variants: [{
           src: ['./src/assets/fonts/DistantGalaxy.woff2'],
           weight: 'normal',
           style: 'normal'
         }]
       }
     }]
   });
   ```

3. Your font is now configured and ready to be [added to your page head](#applying-custom-fonts) so that it can be used in your project.

### Using Fontsource

[Section titled “Using Fontsource”](#using-fontsource)

Astro supports [several font providers](/en/reference/font-provider-reference/#built-in-providers) out of the box, including support for [Fontsource](https://fontsource.org/) that simplifies using Google Fonts and other open-source fonts.

The following example will use Fontsource to add custom font support, but the process is similar for any of Astro’s built-in font providers (e.g. [Adobe](https://fonts.adobe.com/), [Bunny](https://fonts.bunny.net/)).

1. Find the font you want to use in [Fontsource’s catalog](https://fontsource.org/). This example will use [Roboto](https://fontsource.org/fonts/roboto).

2. Create a new font family in your Astro config file using the [Fontsource provider](/en/reference/font-provider-reference/#fontsource):

   astro.config.mjs

   ```js
   import { defineConfig, fontProviders } from "astro/config";


   export default defineConfig({
     fonts: [{
       provider: fontProviders.fontsource(),
       name: "Roboto",
       cssVariable: "--font-roboto",
     }]
   });
   ```

3. Your font is now configured and ready to be [added to your page head](#applying-custom-fonts) so that it can be used in your project.

## Applying custom fonts

[Section titled “Applying custom fonts”](#applying-custom-fonts)

After [a font is configured](#configuring-custom-fonts), it must be added to your page head with an identifying CSS variable. Then, you can use this variable when defining your page styles.

1. Import and include the [`<Font />`](/en/reference/modules/astro-assets/#font-) component with the required `cssVariable` property in the head of your page, usually in a dedicated `Head.astro` component or in a [layout](/en/basics/layouts/) component directly:

   src/layouts/Layout.astro

   ```diff
   ---
   +import { Font } from "astro:assets";
   ---


   <html>
     <head>
       +<Font cssVariable="--font-distant-galaxy" />
     </head>
     <body>
       <slot />
     </body>
   </html>
   ```

2. In any page rendered with that layout, including the layout component itself, you can now define styles with your font’s `cssVariable` to apply your custom font.

   In the following example, the `<h1>` heading will have the custom font applied, while the paragraph `<p>` will not.

   src/pages/example.astro

   ```diff
   ---
   import Layout from "../layouts/Layout.astro";
   ---
   <Layout>
     <h1>In a galaxy far, far away...</h1>


     <p>Custom fonts make my headings much cooler!</p>


     <style>
     +h1 {
       +font-family: var(--font-distant-galaxy);
   +  }
     </style>
   </Layout>
   ```

## Register fonts in Tailwind

[Section titled “Register fonts in Tailwind”](#register-fonts-in-tailwind)

If you are using [Tailwind](/en/guides/styling/#tailwind) for styling, you will not apply your styles with the `font-face` CSS property.

Instead, after [configuring your custom font](#configuring-custom-fonts) and [adding it to your page head](#applying-custom-fonts), you will need to update your Tailwind configuration to register your font:

* Tailwind CSS 4.0

  src/styles/global.css

  ```diff
  @import "tailwindcss";


  @theme inline {
    +--font-sans: var(--font-roboto);
  }
  ```

* Tailwind CSS 3.0

  tailwind.config.mjs

  ```diff
  /** @type {import("tailwindcss").Config} */
  export default {
    content: ["./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}"],
    theme: {
      extend: {},
  +    fontFamily: {
  +      sans: ["var(--font-roboto)"]
  +    }
    },
    plugins: []
  };
  ```

See [Tailwind’s docs on adding custom font families](https://tailwindcss.com/docs/font-family#using-a-custom-value) for more information.

## Accessing font data programmatically

[Section titled “Accessing font data programmatically”](#accessing-font-data-programmatically)

The [`fontData`](/en/reference/modules/astro-assets/#fontdata) object allows you to retrieve lower-level font family data programmatically. For example, you can use it in an [API Route](/en/guides/endpoints/#server-endpoints-api-routes) to generate OpenGraph images using [satori](https://github.com/vercel/satori), combined with proper [formats](/en/reference/configuration-reference/#fontformats) configuration:

src/pages/og.png.ts

```tsx
import type{ APIRoute } from "astro";
import { fontData } from "astro:assets";
import satori from "satori";
import { html } from "satori-html";


export const GET: APIRoute = async (context) => {
  const data = fontData["--font-roboto"];


  const svg = await satori(
    html`<div style="color: black;">hello, world</div>`,
    {
      width: 600,
      height: 400,
      fonts: [
        {
          name: "Roboto",
          data: await fetch(
            new URL(data[0].src[0].url, context.url.origin),
          ).then((res) => res.arrayBuffer()),
          weight: 400,
          style: "normal",
        },
      ],
    },
  );


  // ...
}
```

## Granular font configuration

[Section titled “Granular font configuration”](#granular-font-configuration)

A font family is defined by a combination of properties such as weights and styles (e.g. `weights: [500, 600]` and `styles: ["normal", "bold"]`), but you may want to download only certain combinations of these.

For greater control over which font files are downloaded, you can specify the same font (ie. with the same `cssVariable`, `name`, and `provider` properties) multiple times with different combinations. Astro will merge the results and download only the required files. For example, it is possible to download normal `500` and `600` while downloading only italic `500`:

astro.config.mjs

```js
import { defineConfig, fontProviders } from "astro/config";


export default defineConfig({
  fonts: [
    {
      name: "Roboto",
      cssVariable: "--roboto",
      provider: fontProviders.google(),
      weights: [500, 600],
      styles: ["normal"]
    },
    {
      name: "Roboto",
      cssVariable: "--roboto",
      provider: fontProviders.google(),
      weights: [500],
      styles: ["italic"]
    }
  ]
});
```

## Caching

[Section titled “Caching”](#caching)

The Fonts API caching implementation was designed to be practical in development and efficient in production. During builds, font files are copied to the `_astro/fonts` output directory, so they can benefit from HTTP caching of static assets (usually a year).

To clear the cache in development, remove the `.astro/fonts` directory. To clear the build cache, remove the `node_modules/.astro/fonts` directory.

## Examples

[Section titled “Examples”](#examples)

Astro’s font feature is based on flexible configuration options. Your own project’s font configuration may look different from simplified examples, so the following are provided to show what various font configurations might look like when used in production.

astro.config.mjs

```js
import { defineConfig, fontProviders } from "astro/config";


export default defineConfig({
  fonts: [
    {
      name: "Roboto",
      cssVariable: "--font-roboto",
      provider: fontProviders.google(),
      // Default included:
      // weights: [400] ,
      // styles: ["normal", "italic"],
      // subsets: ["latin"],
      // fallbacks: ["sans-serif"],
      // formats: ["woff2"],
    },
    {
      name: "Inter",
      cssVariable: "--font-inter",
      provider: fontProviders.fontsource(),
      // Specify weights that are actually used
      weights: [400, 500, 600, 700],
      // Specify styles that are actually used
      styles: ["normal"],
      // Download only font files for characters used on the page
      subsets: ["latin", "cyrillic"],
      // Download more font formats
      formats: ["woff2", "woff"],
    },
    {
      name: "JetBrains Mono",
      cssVariable: "--font-jetbrains-mono",
      provider: fontProviders.fontsource(),
      // Download only font files for characters used on the page
      subsets: ["latin", "latin-ext"],
      // Use a fallback font family matching the intended appearance
      fallbacks: ["monospace"],
    },
    {
      name: "Poppins",
      cssVariable: "--font-poppins",
      provider: fontProviders.local(),
      options: {
        // Weight and style are not specified so Astro
        // will try to infer them for each variant
        variants: [
          {
            src: [
              "./src/assets/fonts/Poppins-regular.woff2",
              "./src/assets/fonts/Poppins-regular.woff",
            ]
          },
          {
            src: [
              "./src/assets/fonts/Poppins-bold.woff2",
              "./src/assets/fonts/Poppins-bold.woff",
            ]
          },
        ]
      }
    }
  ],
});
```


---

# Imports reference

> Learn how to import different file types into your Astro project.

Astro supports most static assets with zero configuration required. You can use the `import` statement anywhere in your project JavaScript (including your Astro frontmatter) and Astro will include a built, optimized copy of that static asset in your final build. `@import` is also supported inside of CSS & `<style>` tags.

## Supported File Types

[Section titled “Supported File Types”](#supported-file-types)

The following file types are supported out-of-the-box by Astro:

* Astro Components (`.astro`)
* Markdown (`.md`, `.markdown`, etc.)
* JavaScript (`.js`, `.mjs`)
* TypeScript (`.ts`)
* NPM Packages
* JSON (`.json`)
* CSS (`.css`)
* CSS Modules (`.module.css`)
* Images & Assets (`.svg`, `.jpg`, `.png`, etc.)

Additionally, you can extend Astro to add support for different [UI Frameworks](/en/guides/framework-components/) like React, Svelte and Vue components. You can also install the [Astro MDX integration](/en/guides/integrations-guide/mdx/) or the [Astro Markdoc integration](/en/guides/integrations-guide/markdoc/) to use `.mdx` or `.mdoc` files in your project.

### Files in `public/`

[Section titled “Files in public/”](#files-in-public)

You can place any static asset in the [`public/` directory](/en/basics/project-structure/#public) of your project, and Astro will copy it directly into your final build untouched. `public/` files are not built or bundled by Astro, which means that any type of file is supported.

You can reference a `public/` file by a URL path directly in your HTML templates.

```astro
// To link to /public/reports/annual/2024.pdf
Download the <a href="/reports/annual/2024.pdf">2024 annual statement as a PDF</a>.


// To display /public/assets/cats/ginger.jpg
<img src="/assets/cats/ginger.jpg" alt="An orange cat sleeping on a bed.">
```

## Import statements

[Section titled “Import statements”](#import-statements)

Astro uses ESM, the same [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#syntax) and [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) syntax supported in the browser.

### JavaScript

[Section titled “JavaScript”](#javascript)

```js
import { getUser } from './user.js';
```

JavaScript can be imported using normal ESM `import` & `export` syntax.

Importing JSX files

An appropriate [UI framework](/en/guides/framework-components/) ([React](/en/guides/integrations-guide/react/), [Preact](/en/guides/integrations-guide/preact/), or [Solid](/en/guides/integrations-guide/solid-js/)) is required to render JSX/TSX files. Use `.jsx`/`.tsx` extensions where appropriate, as Astro does not support JSX in `.js`/`.ts` files.

### TypeScript

[Section titled “TypeScript”](#typescript)

```js
import { getUser } from './user';
import type { UserType } from './user';
```

Astro includes built-in support for [TypeScript](https://www.typescriptlang.org/). You can import `.ts` and `.tsx` files directly in your Astro project, and even write TypeScript code directly inside your [Astro component script](/en/basics/astro-components/#the-component-script) and any [script tags](/en/guides/client-side-scripts/).

**Astro doesn’t perform any type checking itself.** Type checking should be taken care of outside of Astro, either by your IDE or through a separate script. For type checking Astro files, the [`astro check` command](/en/reference/cli-reference/#astro-check) is provided.

TypeScript and file extensions

Per [TypeScript’s module resolution rules](https://www.typescriptlang.org/docs/handbook/module-resolution.html), `.ts` and `.tsx` file extensions should not be used when importing TypeScript files. Instead, either use `.js`/`.jsx` file extensions or completely omit the file extension.

```ts
import { getUser } from './user.js'; // user.ts
import MyComponent from "./MyComponent"; // MyComponent.tsx
```

Read more about [TypeScript support in Astro](/en/guides/typescript/).

### NPM Packages

[Section titled “NPM Packages”](#npm-packages)

If you’ve installed an NPM package, you can import it in Astro.

```astro
---
import { Icon } from 'astro-icon';
---
```

If a package was published using a legacy format, Astro will try to convert the package to ESM so that `import` statements work. In some cases, you may need to adjust your [`vite` config](/en/reference/configuration-reference/#vite) for it to work.

Caution

Some packages rely on a browser environment. Astro components runs on the server, so importing these packages in the frontmatter may [lead to errors](/en/guides/troubleshooting/#document-or-window-is-not-defined).

### JSON

[Section titled “JSON”](#json)

```js
// Load the JSON object via the default export
import json from './data.json';
```

Astro supports importing JSON files directly into your application. Imported files return the full JSON object in the default import.

### CSS

[Section titled “CSS”](#css)

```js
// Load and inject 'style.css' onto the page
import './style.css';
```

Astro supports importing CSS files directly into your application. Imported styles expose no exports, but importing one will automatically add those styles to the page. This works for all CSS files by default, and can support compile-to-CSS languages like Sass & Less via plugins.

Read more about advanced CSS import use cases such as a direct URL reference for a CSS file, or importing CSS as a string in the [Styling guide](/en/guides/styling/#advanced).

### CSS Modules

[Section titled “CSS Modules”](#css-modules)

```jsx
// 1. Converts './style.module.css' classnames to unique, scoped values.
// 2. Returns an object mapping the original classnames to their final, scoped value.
import styles from './style.module.css';


// This example uses JSX, but you can use CSS Modules with any framework.
return <div className={styles.error}>Your Error Message</div>;
```

Astro supports CSS Modules using the `[name].module.css` naming convention. Like any CSS file, importing one will automatically apply that CSS to the page. However, CSS Modules export a special default `styles` object that maps your original classnames to unique identifiers.

CSS Modules help you enforce component scoping & isolation on the frontend with uniquely-generated class names for your stylesheets.

### Other Assets

[Section titled “Other Assets”](#other-assets)

```jsx
// Returns an object with `src` and other properties
import imgReference from './image.png';
import svgReference from './image.svg';


// HTML or UI Framework components use this to render the image
<img src={imgReference.src} alt="image description" />;


// The Astro `<Image />` and `<Picture />` components access `src` by default
<Image src={imgReference} alt="image description">
```

All other assets not explicitly mentioned above can be imported via ESM `import` and will return a URL reference to the final built asset (e.g. `/_astro/my-video.C7vXpQtF.mp4`) instead of an object.

This can be useful for referencing non-JS assets by URL, like creating a video element with a `src` attribute pointing to that image.

It can also be useful to place images and other assets in the `public/` folder as explained on the [project-structure page](/en/basics/project-structure/#public).

Read more about appending Vite import parameters (e.g. `?url`, `?raw`) in [Vite’s static asset handling guide](https://vite.dev/guide/assets.html).

Note

Adding **alt text** to `<img>` tags is encouraged for accessibility! Don’t forget to add an `alt="a helpful description"` attribute to your image elements. You can just leave the attribute empty if the image is purely decorative.

## Aliases

[Section titled “Aliases”](#aliases)

An **alias** is a way to create shortcuts for your imports.

Aliases can help improve the development experience in codebases with many directories or relative imports.

src/pages/about/company.astro

```astro
---
import Button from '../../components/controls/Button.astro';
import logoUrl from '../../assets/logo.png?url';
---
```

In this example, a developer would need to understand the tree relationship between `src/pages/about/company.astro`, `src/components/controls/Button.astro`, and `src/assets/logo.png`. And then, if the `company.astro` file were to be moved, these imports would also need to be updated.

You can add import aliases in `tsconfig.json`.

tsconfig.json

```diff
{
  "compilerOptions": {
    "paths": {
      +"@components/*": ["./src/components/*"],
      +"@assets/*": ["./src/assets/*"]
    }
  }
}
```

The development server will automatically restart after this configuration change. You can now import using the aliases anywhere in your project:

src/pages/about/company.astro

```astro
---
import Button from '@components/controls/Button.astro';
import logoUrl from '@assets/logo.png?url';
---
```

## `import.meta.glob()`

[Section titled “import.meta.glob()”](#importmetaglob)

[Vite’s `import.meta.glob()`](https://vite.dev/guide/features.html#glob-import) is a way to import many files at once using glob patterns to find matching file paths.

`import.meta.glob()` takes a relative [glob pattern](#glob-patterns) matching the local files you’d like to import as a parameter. It returns an array of each matching file’s exports. To load all matched modules up front, pass `{ eager: true }` as the second argument:

src/components/my-component.astro

```astro
---
// imports all files that end with `.md` in `./src/pages/post/`
const matches = import.meta.glob('../pages/post/*.md', { eager: true });
const posts = Object.values(matches);
---
<!-- Renders an <article> for the first 5 blog posts -->
<div>
{posts.slice(0, 4).map((post) => (
  <article>
    <h2>{post.frontmatter.title}</h2>
    <p>{post.frontmatter.description}</p>
    <a href={post.url}>Read more</a>
  </article>
))}
</div>
```

Astro components imported using `import.meta.glob` are of type [`AstroInstance`](#astro-files). You can render each component instance using its `default` property:

src/pages/component-library.astro

```astro
---
// imports all files that end with `.astro` in `./src/components/`
const components = Object.values(import.meta.glob('../components/*.astro', { eager: true }));
---
<!-- Display all of our components -->
{components.map((component) => (
  <div>
    <component.default size={24} />
  </div>
))}
```

### Supported Values

[Section titled “Supported Values”](#supported-values)

Vite’s `import.meta.glob()` function only supports static string literals. It does not support dynamic variables and string interpolation.

A common workaround is to instead import a larger set of files that includes all the files you need, then filter them:

src/components/featured.astro

```astro
---
const { postSlug } = Astro.props;
const pathToMyFeaturedPost = `src/pages/blog/${postSlug}.md`;


const posts = Object.values(import.meta.glob("../pages/blog/*.md", { eager: true }));
const myFeaturedPost = posts.find(post => post.file.includes(pathToMyFeaturedPost));
---


<p>
  Take a look at my favorite post, <a href={myFeaturedPost.url}>{myFeaturedPost.frontmatter.title}</a>!
</p>
```

### Import type utilities

[Section titled “Import type utilities”](#import-type-utilities)

#### Markdown files

[Section titled “Markdown files”](#markdown-files)

Markdown files loaded with `import.meta.glob()` return the following `MarkdownInstance` interface:

```ts
export interface MarkdownInstance<T extends Record<string, any>> {
  /* Any data specified in this file's YAML/TOML frontmatter */
  frontmatter: T;
  /* The absolute file path of this file */
  file: string;
  /* The rendered path of this file */
  url: string | undefined;
  /* Astro Component that renders the contents of this file */
  Content: AstroComponentFactory;
  /** (Markdown only) Raw Markdown file content, excluding layout HTML and YAML/TOML frontmatter */
  rawContent(): string;
  /** (Markdown only) Markdown file compiled to HTML, excluding layout HTML */
  compiledContent(): string;
  /* Function that returns an array of the h1...h6 elements in this file */
  getHeadings(): Promise<{ depth: number; slug: string; text: string }[]>;
  default: AstroComponentFactory;
}
```

You can optionally provide a type for the `frontmatter` variable using a TypeScript generic.

```astro
---
import type { MarkdownInstance } from 'astro';
interface Frontmatter {
    title: string;
    description?: string;
}


const posts = Object.values(import.meta.glob<MarkdownInstance<Frontmatter>>('./posts/**/*.md', { eager: true }));
---


<ul>
  {posts.map(post => <li>{post.frontmatter.title}</li>)}
</ul>
```

#### Astro files

[Section titled “Astro files”](#astro-files)

Astro files have the following interface:

```ts
export interface AstroInstance {
  /* The file path of this file */
  file: string;
  /* The URL for this file (if it is in the pages directory) */
  url: string | undefined;
  default: AstroComponentFactory;
}
```

#### Other files

[Section titled “Other files”](#other-files)

Other files may have various different interfaces, but `import.meta.glob()` accepts a TypeScript generic if you know exactly what an unrecognized file type contains.

```ts
---
interface CustomDataFile {
  default: Record<string, any>;
}
const data = import.meta.glob<CustomDataFile>('../data/**/*.js');
---
```

### Glob Patterns

[Section titled “Glob Patterns”](#glob-patterns)

A glob pattern is a file path that supports special wildcard characters. This is used to reference multiple files in your project at once.

For example, the glob pattern `./pages/**/*.{md,mdx}` starts within the pages subdirectory, looks through all of its subdirectories (`/**`), and matches any filename (`/*`) that ends in either `.md` or `.mdx` (`.{md,mdx}`).

#### Glob Patterns in Astro

[Section titled “Glob Patterns in Astro”](#glob-patterns-in-astro)

To use with `import.meta.glob()`, the glob pattern must be a string literal and cannot contain any variables.

Additionally, glob patterns must begin with one of the following:

* `./` (to start in the current directory)
* `../` (to start in the parent directory)
* `/` (to start at the root of the project)

[Read more about the glob pattern syntax](https://github.com/micromatch/picomatch#globbing-features).

### `import.meta.glob()` vs `getCollection()`

[Section titled “import.meta.glob() vs getCollection()”](#importmetaglob-vs-getcollection)

[Content collections](/en/guides/content-collections/) provide [performant, content-focused APIs](/en/reference/modules/astro-content/) for loading multiple files instead of `import.meta.glob()`. Use `getCollection()` and `getLiveCollection()` to query your collections and return content entries.

## WASM

[Section titled “WASM”](#wasm)

```js
// Loads and initializes the requested WASM file
const wasm = await WebAssembly.instantiateStreaming(fetch('/example.wasm'));
```

Astro supports loading WASM files directly into your application using the browser’s [`WebAssembly`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly) API.

## Node Builtins

[Section titled “Node Builtins”](#node-builtins)

Astro supports Node.js built-ins, with some limitations, using Node’s newer `node:` prefix. There may be differences between development and production, and some features may be incompatible with on-demand rendering. Some [adapters](/en/guides/on-demand-rendering/) may also be incompatible with these built-ins modules or require configuration to support a subset (e.g., [Cloudflare Workers](/en/guides/integrations-guide/cloudflare/) or [Deno](https://github.com/denoland/deno-astro-adapter)).

The following example imports the `util` module from Node to parse a media type (MIME):

src/components/MyComponent.astro

```astro
---
// Example: import the "util" built-in from Node.js
import util from 'node:util';


export interface Props {
  mimeType: string,
}


const mime = new util.MIMEType(Astro.props.mimeType)
---


<span>Type: {mime.type}</span>
<span>SubType: {mime.subtype}</span>
```

## Extending file type support

[Section titled “Extending file type support”](#extending-file-type-support)

With **Vite** and compatible **Rollup** plugins, you can import file types which aren’t natively supported by Astro. Learn where to find the plugins you need in the [Finding Plugins](https://vite.dev/guide/using-plugins.html#finding-plugins) section of the Vite Documentation.

Plugin configuration

Refer to your plugin’s documentation for configuration options, and how to correctly install it.

![](/houston_chef.webp) **Related recipe:** [Installing a Vite or Rollup plugin](/en/recipes/add-yaml-support/)


---

# Template expressions reference

Astro component syntax is a superset of HTML. The syntax was designed to feel familiar to anyone with experience writing HTML or JSX, and adds support for including components and JavaScript expressions.

## JSX-like Expressions

[Section titled “JSX-like Expressions”](#jsx-like-expressions)

You can define local JavaScript variables inside of the frontmatter component script between the two code fences (`---`) of an Astro component. You can then inject these variables into the component’s HTML template using JSX-like expressions!

Dynamic vs reactive

Using this approach, you can include **dynamic** values that are calculated in the frontmatter. But once included, these values are not **reactive** and will never change. Astro components are templates that only run once, during the rendering step.

See below for more examples of [differences between Astro and JSX](#differences-between-astro-and-jsx).

### Variables

[Section titled “Variables”](#variables)

Local variables can be added into the HTML using the curly braces syntax:

src/components/Variables.astro

```astro
---
const name = "Astro";
---
<div>
  <h1>Hello {name}!</h1>  <!-- Outputs <h1>Hello Astro!</h1> -->
</div>
```

### Dynamic Attributes

[Section titled “Dynamic Attributes”](#dynamic-attributes)

Local variables can be used in curly braces to pass attribute values to both HTML elements and components:

src/components/DynamicAttributes.astro

```astro
---
const name = "Astro";
---
<h1 class={name}>Attribute expressions are supported</h1>


<MyComponent templateLiteralNameAttribute={`MyNameIs${name}`} />
```

Caution

HTML attributes will be converted to strings, so it is not possible to pass functions and objects to HTML elements. For example, you can’t assign an event handler to an HTML element in an Astro component:

dont-do-this.astro

```astro
---
function handleClick () {
    console.log("button clicked!");
}
---
<!-- ❌ This doesn't work! ❌ -->
<button onClick={handleClick}>Nothing will happen when you click me!</button>
```

Instead, use a client-side script to add the event handler, like you would in vanilla JavaScript:

do-this-instead.astro

```astro
---
---
<button id="button">Click Me</button>
<script>
  function handleClick () {
    console.log("button clicked!");
  }
  document.getElementById("button").addEventListener("click", handleClick);
</script>
```

### Dynamic HTML

[Section titled “Dynamic HTML”](#dynamic-html)

Local variables can be used in JSX-like functions to produce dynamically-generated HTML elements:

src/components/DynamicHtml.astro

```astro
---
const items = ["Dog", "Cat", "Platypus"];
---
<ul>
  {items.map((item) => (
    <li>{item}</li>
  ))}
</ul>
```

Astro can conditionally display HTML using JSX logical operators and ternary expressions.

src/components/ConditionalHtml.astro

```astro
---
const visible = true;
---
{visible && <p>Show me!</p>}


{visible ? <p>Show me!</p> : <p>Else show me!</p>}
```

### Dynamic Tags

[Section titled “Dynamic Tags”](#dynamic-tags)

You can also use dynamic tags by assigning an HTML tag name to a variable or with a component import reassignment:

src/components/DynamicTags.astro

```astro
---
import MyComponent from "./MyComponent.astro";
const Element = 'div'
const Component = MyComponent;
---
<Element>Hello!</Element> <!-- renders as <div>Hello!</div> -->
<Component /> <!-- renders as <MyComponent /> -->
```

When using dynamic tags:

* **Variable names must be capitalized.** For example, use `Element`, not `element`. Otherwise, Astro will try to render your variable name as a literal HTML tag.

* **Hydration directives are not supported.** When using [`client:*` hydration directives](/en/guides/framework-components/#hydrating-interactive-components), Astro needs to know which components to bundle for production, and the dynamic tag pattern prevents this from working.

* **The [define:vars directive](/en/reference/directives-reference/#definevars) is not supported.** If you cannot wrap the children with an extra element (e.g `<div>`), then you can manually add a ``style={`--myVar:${value}`}`` to your Element.

### Fragments

[Section titled “Fragments”](#fragments)

Astro supports `<> </>` notation and also provides a built-in `<Fragment />` component. This component can be useful to avoid wrapper elements when adding [`set:*` directives](/en/reference/directives-reference/#sethtml) to inject an HTML string.

The following example renders paragraph text using the `<Fragment />` component:

src/components/SetHtml.astro

```astro
---
const htmlString = '<p>Raw HTML content</p>';
---
<Fragment set:html={htmlString} />
```

### Differences between Astro and JSX

[Section titled “Differences between Astro and JSX”](#differences-between-astro-and-jsx)

Astro component syntax is a superset of HTML. It was designed to feel familiar to anyone with HTML or JSX experience, but there are a couple of key differences between `.astro` files and JSX.

#### Attributes

[Section titled “Attributes”](#attributes)

In Astro, you use the standard `kebab-case` format for all HTML attributes instead of the `camelCase` used in JSX. This even works for `class`, which is not supported by React.

example.astro

```diff
<div className="box" dataValue="3" />
<div class="box" data-value="3" />
```

#### Multiple Elements

[Section titled “Multiple Elements”](#multiple-elements)

An Astro component template can render multiple elements with no need to wrap everything in a single `<div>` or `<>`, unlike JavaScript or JSX.

src/components/RootElements.astro

```astro
---
// Template with multiple elements
---
<p>No need to wrap elements in a single containing element.</p>
<p>Astro supports multiple root elements in a template.</p>
```

#### Comments

[Section titled “Comments”](#comments)

In Astro, you can use standard HTML comments or JavaScript-style comments.

example.astro

```astro
---
---
<!-- HTML comment syntax is valid in .astro files -->
{/* JS comment syntax is also valid */}
```

Caution

HTML-style comments will be included in browser DOM, while JS ones will be skipped. To leave TODO messages or other development-only explanations, you may wish to use JavaScript-style comments instead.

## Component utilities

[Section titled “Component utilities”](#component-utilities)

### `Astro.slots`

[Section titled “Astro.slots”](#astroslots)

`Astro.slots` contains utility functions for modifying an Astro component’s slotted children.

#### `Astro.slots.has()`

[Section titled “Astro.slots.has()”](#astroslotshas)

**Type:** `(slotName: string) => boolean`

You can check whether content for a specific slot name exists with `Astro.slots.has()`. This can be useful when you want to wrap slot contents but only want to render the wrapper elements when the slot is being used.

src/pages/index.astro

```astro
---
---
<slot />


{Astro.slots.has('more') && (
  <aside>
    <h2>More</h2>
    <slot name="more" />
  </aside>
)}
```

#### `Astro.slots.render()`

[Section titled “Astro.slots.render()”](#astroslotsrender)

**Type:** `(slotName: string, args?: any[]) => Promise<string>`

You can asynchronously render the contents of a slot to a string of HTML using `Astro.slots.render()`.

```astro
---
const html = await Astro.slots.render('default');
---
<Fragment set:html={html} />
```

Note

This is for advanced use cases! In most circumstances, it is simpler to render slot contents with [the `<slot />` element](/en/basics/astro-components/#slots).

`Astro.slots.render()` optionally accepts a second argument: an array of parameters that will be forwarded to any function children. This can be useful for custom utility components.

For example, this `<Shout />` component converts its `message` prop to uppercase and passes it to the default slot:

src/components/Shout.astro

```astro
---
const message = Astro.props.message.toUpperCase();
let html = '';
if (Astro.slots.has('default')) {
  html = await Astro.slots.render('default', [message]);
}
---
<Fragment set:html={html} />
```

A callback function passed as `<Shout />`’s child will receive the all-caps `message` parameter:

src/pages/index.astro

```astro
---
import Shout from "../components/Shout.astro";
---
<Shout message="slots!">
  {(message) => <div>{message}</div>}
</Shout>


<!-- renders as <div>SLOTS!</div> -->
```

Callback functions can be passed to named slots inside a wrapping HTML element tag with a `slot` attribute. This element is only used to transfer the callback to a named slot and will not be rendered onto the page.

```astro
<Shout message="slots!">
  <fragment slot="message">
    {(message) => <div>{message}</div>}
  </fragment>
</Shout>
```

Use a standard HTML element for the wrapping tag or any lowercase tag (e.g. `<fragment>` instead of `<Fragment />`) that will not be interpreted as a component. Do not use the HTML `<slot>` element as this will be interpreted as an Astro slot.

### `Astro.self`

[Section titled “Astro.self”](#astroself)

`Astro.self` allows Astro components to be recursively called. This behavior lets you render an Astro component from within itself by using `<Astro.self>` in the component template. This can help iterate over large data stores and nested data structures.

NestedList.astro

```astro
---
const { items } = Astro.props;
---
<ul class="nested-list">
  {items.map((item) => (
    <li>
      <!-- If there is a nested data-structure we render `<Astro.self>` -->
      <!-- and can pass props through with the recursive call -->
      {Array.isArray(item) ? (
        <Astro.self items={item} />
      ) : (
        item
      )}
    </li>
  ))}
</ul>
```

This component could then be used like this:

```astro
---
import NestedList from './NestedList.astro';
---
<NestedList items={['A', ['B', 'C'], 'D']} />
```

And would render HTML like this:

```html
<ul class="nested-list">
  <li>A</li>
  <li>
    <ul class="nested-list">
      <li>B</li>
      <li>C</li>
    </ul>
  </li>
  <li>D</li>
</ul>
```


---

# CLI Commands

You can use the Command-Line Interface (CLI) provided by Astro to develop, build, and preview your project from a terminal window.

### `astro` commands

[Section titled “astro commands”](#astro-commands)

Use the CLI by running one of the **commands** documented on this page with your preferred package manager, optionally followed by any **flags**. Flags customize the behavior of a command.

One of the commands you’ll use most often is `astro dev`. This command starts the development server and gives you a live, updating preview of your site in a browser as you work:

* npm

  ```shell
  # start the development server
  npx astro dev
  ```

* pnpm

  ```shell
  # start the development server
  pnpm astro dev
  ```

* Yarn

  ```shell
  # start the development server
  yarn astro dev
  ```

You can type `astro --help` in your terminal to display a list of all available commands:

* npm

  ```shell
  npx astro --help
  ```

* pnpm

  ```shell
  pnpm astro --help
  ```

* Yarn

  ```shell
  yarn astro --help
  ```

The following message will display in your terminal:

```bash
astro [command] [...flags]


Commands
              add  Add an integration.
            build  Build your project and write it to disk.
            check  Check your project for errors.
       create-key  Create a cryptography key
              dev  Start the development server.
             docs  Open documentation in your web browser.
             info  List info about your current Astro setup.
          preview  Preview your build locally.
             sync  Generate TypeScript types for all Astro modules.
      preferences  Configure user preferences.
        telemetry  Configure telemetry settings.


Global Flags
  --config <path>  Specify your config file.
    --root <path>  Specify your project root folder.
     --site <url>  Specify your project site.
--base <pathname>  Specify your project base.
        --verbose  Enable verbose logging.
         --silent  Disable all logging.
        --version  Show the version number and exit.
           --help  Show this help message.
```

You can add the `--help` flag after any command to get a list of all the flags for that command.

* npm

  ```shell
  # get a list of all flags for the `dev` command
  npm run dev -- --help
  ```

* pnpm

  ```shell
  # get a list of all flags for the `dev` command
  pnpm dev --help
  ```

* Yarn

  ```shell
  # get a list of all flags for the `dev` command
  yarn dev --help
  ```

The following message will display in your terminal:

```bash
astro dev [...flags]


Flags
                 --port  Specify which port to run on. Defaults to 4321.
                 --host  Listen on all addresses, including LAN and public addresses.
--host <custom-address>  Expose on a network IP address at <custom-address>
                 --open  Automatically open the app in the browser on server start
                --force  Clear the content layer cache, forcing a full rebuild.
            --help (-h)  See all available flags.
```

Note

The extra `--` before any flag is necessary for `npm` to pass your flags to the `astro` command.

### `package.json` scripts

[Section titled “package.json scripts”](#packagejson-scripts)

You can also use scripts in `package.json` for shorter versions of these commands. Using a script allows you to use the same commands that you may be familiar with from other projects, such as `npm run build`.

The following scripts for the most common `astro` commands (`astro dev`, `astro build`, and `astro preview`) are added for you automatically when you create a project using [the `create astro` wizard](/en/install-and-setup/).

When you follow the instructions to [install Astro manually](/en/install-and-setup/#manual-setup), you are instructed to add these scripts yourself. You can also add more scripts to this list manually for any commands you use frequently.

package.json

```json
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  }
}
```

You will often use these `astro` commands, or the scripts that run them, without any flags. Add flags to the command when you want to customize the command’s behavior. For example, you may wish to start the development server on a different port, or build your site with verbose logs for debugging.

* npm

  ```shell
  # run the dev server on port 8080 using the `dev` script in `package.json`
  npm run dev -- --port 8080


  # build your site with verbose logs using the `build` script in `package.json`
  npm run build -- --verbose
  ```

* pnpm

  ```shell
  # run the dev server on port 8080 using the `dev` script in `package.json`
  pnpm dev --port 8080


  # build your site with verbose logs using the `build` script in `package.json`
  pnpm build --verbose
  ```

* Yarn

  ```shell
  # run the dev server on port 8080 using the `dev` script in `package.json`
  yarn dev --port 8080


  # build your site with verbose logs using the `build` script in `package.json`
  yarn build --verbose
  ```

## `astro dev`

[Section titled “astro dev”](#astro-dev)

Runs Astro’s development server. This is a local HTTP server that doesn’t bundle assets. It uses Hot Module Replacement (HMR) to update your browser as you save changes in your editor.

The following hotkeys can be used in the terminal where the Astro development server is running:

* `s + enter` to sync the content layer data (content and types).
* `o + enter` to open your Astro site in the browser.
* `q + enter` to quit the development server.

## `astro build`

[Section titled “astro build”](#astro-build)

Builds your site for deployment. By default, this will generate static files and place them in a `dist/` directory. If any routes are [rendered on demand](/en/guides/on-demand-rendering/), this will generate the necessary server files to serve your site.

### Flags

The command accepts [common flags](#common-flags) and the following additional flags:

#### `--devOutput`

[Section titled “--devOutput”](#--devoutput)

**Added in:** `astro@5.0.0`

Outputs a development-based build similar to code transformed in `astro dev`. This can be useful to test build-only issues with additional debugging information included.

## `astro preview`

[Section titled “astro preview”](#astro-preview)

Starts a local server to serve the contents of your static directory (`dist/` by default) created by running `astro build`.

This command allows you to preview your site locally [after building](#astro-build) to catch any errors in your build output before deploying it. It is not designed to be run in production. For help with production hosting, check out our guide on [Deploying an Astro Website](/en/guides/deploy/).

The following hotkeys can be used in the terminal where the Astro preview server is running:

* `o` + `enter` to open your Astro site in the browser.
* `q` + `enter` to quit the preview server.

The `astro preview` command can be combined with the [common flags](#common-flags) documented below to further control the preview experience.

## `astro check`

[Section titled “astro check”](#astro-check)

Runs diagnostics (such as type-checking within `.astro` files) against your project and reports errors to the console. If any errors are found the process will exit with a code of **1**.

This command is intended to be used in CI workflows.

### Flags

Use these flags to customize the behavior of the command.

#### `--watch`

[Section titled “--watch”](#--watch)

The command will watch for any changes in your project, and will report any errors.

#### `--root <path-to-dir>`

[Section titled “--root \<path-to-dir>”](#--root-path-to-dir)

Specifies a different root directory to check. Uses the current working directory by default.

#### `--tsconfig <path-to-file>`

[Section titled “--tsconfig \<path-to-file>”](#--tsconfig-path-to-file)

Specifies a `tsconfig.json` file to use manually. If not provided, Astro will attempt to find a config, or infer the project’s config automatically.

#### `--minimumFailingSeverity <error|warning|hint>`

[Section titled “--minimumFailingSeverity \<error|warning|hint>”](#--minimumfailingseverity-errorwarninghint)

Specifies the minimum severity needed to exit with an error code. Defaults to `error`.

For example, running `astro check --minimumFailingSeverity warning` will cause the command to exit with an error if any warnings are detected.

#### `--minimumSeverity <error|warning|hint>`

[Section titled “--minimumSeverity \<error|warning|hint>”](#--minimumseverity-errorwarninghint)

Specifies the minimum severity to output. Defaults to `hint`.

For example, running `astro check --minimumSeverity warning` will show errors and warning, but not hints.

#### `--preserveWatchOutput`

[Section titled “--preserveWatchOutput”](#--preservewatchoutput)

Specifies not to clear the output between checks when in watch mode.

#### `--noSync`

[Section titled “--noSync”](#--nosync)

Specifies not to run `astro sync` before checking the project.

Read more about [type checking in Astro](/en/guides/typescript/#type-checking).

## `astro sync`

[Section titled “astro sync”](#astro-sync)

**Added in:** `astro@2.0.0`

Tip

Running `astro dev`, `astro build` or `astro check` will run the `sync` command as well.

Generates TypeScript types for all Astro modules. This sets up a [`.astro/types.d.ts` file](/en/guides/typescript/#setup) for type inferencing, and defines modules for features that rely on generated types:

* The `astro:content` module for the [Content Collections API](/en/guides/content-collections/).
* The `astro:db` module for [Astro DB](/en/guides/astro-db/).
* The `astro:env` module for [Astro Env](/en/guides/environment-variables/).
* The `astro:actions` module for [Astro Actions](/en/guides/actions/)

## `astro add`

[Section titled “astro add”](#astro-add)

Adds an integration to your configuration. Read more in [the integrations guide](/en/guides/integrations/#automatic-integration-setup).

## `astro docs`

[Section titled “astro docs”](#astro-docs)

Launches the Astro Docs website directly from the terminal.

## `astro info`

[Section titled “astro info”](#astro-info)

Reports useful information about your current Astro environment. Useful for providing information when opening an issue.

```shell
astro info
```

Example output:

```plaintext
Astro                    v5.14.1
Vite                     v6.3.6
Node                     v22.17.1
System                   macOS (arm64)
Package Manager          npm
Output                   static
Adapter                  none
Integrations             @astrojs/starlight (v0.35.3)
```

### Flags

Use the following flags to customize the behavior of the command.

#### `--copy`

[Section titled “--copy”](#--copy)

The command will copy the output to the clipboard without prompting.

## `astro preferences`

[Section titled “astro preferences”](#astro-preferences)

Manage user preferences with the `astro preferences` command. User preferences are specific to individual Astro users, unlike the `astro.config.mjs` file which changes behavior for everyone working on a project.

User preferences are scoped to the current project by default, stored in a local `.astro/settings.json` file.

Using the `--global` flag, user preferences can also be applied to every Astro project on the current machine. Global user preferences are stored in an operating system-specific location.

### Available preferences

* `devToolbar` — Enable or disable the development toolbar in the browser. (Default: `true`)
* `checkUpdates` — Enable or disable automatic update checks for the Astro CLI. (Default: `true`)

The `list` command prints the current settings of all configurable user preferences. It also supports a machine-readable `--json` output.

```shell
astro preferences list
```

Example terminal output:

| Preference           | Value |
| -------------------- | ----- |
| devToolbar.enabled   | true  |
|                      |       |
| checkUpdates.enabled | true  |

You can `enable`, `disable`, or `reset` preferences to their default.

For example, to disable the devToolbar in a specific Astro project:

```shell
astro preferences disable devToolbar
```

To disable the devToolbar in all Astro projects on the current machine:

```shell
astro preferences disable --global devToolbar
```

The devToolbar can later be enabled with:

```shell
astro preferences enable devToolbar
```

The `reset` command resets a preference to its default value:

```shell
astro preferences reset devToolbar
```

## `astro telemetry`

[Section titled “astro telemetry”](#astro-telemetry)

Sets telemetry configuration for the current CLI user. Telemetry is anonymous data that provides the Astro team insights into which Astro features are most often used. For more information see [Astro’s telemetry page](https://astro.build/telemetry/).

Telemetry can be disabled with this CLI command:

```shell
astro telemetry disable
```

Telemetry can later be re-enabled with:

```shell
astro telemetry enable
```

The `reset` command resets the telemetry data:

```shell
astro telemetry reset
```

Want to disable telemetry in CI environments?

Add the `astro telemetry disable` command to your CI scripts or set the `ASTRO_TELEMETRY_DISABLED` environment variable.

## `astro create-key`

[Section titled “astro create-key”](#astro-create-key)

Generates a key to encrypt props passed to server islands.

```shell
astro create-key
```

Set this key as the `ASTRO_KEY` environment variable (e.g. in a `.env` file) and include it in your CI/CD or host’s build settings when you need [a constant encryption key for your server islands](/en/guides/server-islands/#reusing-the-encryption-key) for situations like rolling deployments, multi-region hosting or a CDN that caches pages containing server islands.

## Common flags

[Section titled “Common flags”](#common-flags)

### `--root <path>`

[Section titled “--root \<path>”](#--root-path)

Specifies the path to the project root. If not specified, the current working directory is assumed to be the root.

The root is used for finding the Astro configuration file.

```shell
astro --root myRootFolder/myProjectFolder dev
```

### `--config <path>`

[Section titled “--config \<path>”](#--config-path)

Specifies the path to the config file relative to the project root. Defaults to `astro.config.mjs`. Use this if you use a different name for your configuration file or have your config file in another folder.

```shell
astro --config config/astro.config.mjs dev
```

### `--force <string>`

[Section titled “--force \<string>”](#--force-string)

**Added in:** `astro@5.0.0`

Clear the content layer cache, forcing a full rebuild.

### `--mode <string>`

[Section titled “--mode \<string>”](#--mode-string)

**Added in:** `astro@5.0.0`

Configures the [`mode`](/en/reference/programmatic-reference/#astroinlineconfigmode) inline config for your project.

### `--outDir <path>`

[Section titled “--outDir \<path>”](#--outdir-path)

**Added in:** `astro@3.3.0`

Configures the [`outDir`](/en/reference/configuration-reference/#outdir) for your project. Passing this flag will override the `outDir` value in your `astro.config.mjs` file, if one exists.

### `--site <url>`

[Section titled “--site \<url>”](#--site-url)

Configures the [`site`](/en/reference/configuration-reference/#site) for your project. Passing this flag will override the `site` value in your `astro.config.mjs` file, if one exists.

### `--base <pathname>`

[Section titled “--base \<pathname>”](#--base-pathname)

**Added in:** `astro@1.4.1`

Configures the [`base`](/en/reference/configuration-reference/#base) for your project. Passing this flag will override the `base` value in your `astro.config.mjs` file, if one exists.

### `--port <number>`

[Section titled “--port \<number>”](#--port-number)

Specifies which port to run the dev server and preview server on. Defaults to `4321`.

### `--host [optional host address]`

[Section titled “--host \[optional host address\]”](#--host-optional-host-address)

Sets which network IP addresses the dev server and preview server should listen on (i.e. non-localhost IPs). This can be useful for testing your project on local devices like a mobile phone during development.

* `--host` — listen on all addresses, including LAN and public addresses
* `--host <custom-address>` — expose on a network IP address at `<custom-address>`

Caution

Do not use the `--host` flag to expose the dev server and preview server in a production environment. The servers are designed for local use while developing your site only.

### `--allowed-hosts`

[Section titled “--allowed-hosts”](#--allowed-hosts)

**Added in:** `astro@5.4.0`

Specifies the hostnames that Astro is allowed to respond to in `dev` or `preview` modes. Can be passed a comma-separated list of hostnames or `true` to allow any hostname.

Refer to [Vite’s `allowedHosts` feature](https://vite.dev/config/server-options.html#server-allowedhosts) for more information, including security implications of allowing hostnames.

### `--verbose`

[Section titled “--verbose”](#--verbose)

Enables verbose logging, which is helpful when debugging an issue.

### `--silent`

[Section titled “--silent”](#--silent)

Enables silent logging, which will run the server without any console output.

### `--open`

[Section titled “--open”](#--open)

Automatically opens the app in the browser on server start. Can be passed a full URL string (e.g. `--open http://example.com`) or a pathname (e.g. `--open /about`) to specify the URL to open.

## Global flags

[Section titled “Global flags”](#global-flags)

Use these flags to get information about the `astro` CLI.

### `--version`

[Section titled “--version”](#--version)

Prints the Astro version number and exits.

### `--help`

[Section titled “--help”](#--help)

Prints the help message and exits.


---

# Configuration Reference

The following reference covers all supported configuration options in Astro. To learn more about configuring Astro, read our guide on [Configuring Astro](/en/guides/configuring-astro/).

astro.config.mjs

```js
import { defineConfig } from 'astro/config'


export default defineConfig({
  // your configuration options here...
})
```

## Top-Level Options

[Section titled “Top-Level Options”](#top-level-options)

### site

[Section titled “site”](#site)

**Type:** `string`

Your final, deployed URL. Astro uses this full URL to generate your sitemap and canonical URLs in your final build. It is strongly recommended that you set this configuration to get the most out of Astro.

```js
{
  site: 'https://www.my-site.dev'
}
```

### base

[Section titled “base”](#base)

**Type:** `string`

The base path to deploy to. Astro will use this path as the root for your pages and assets both in development and in production build.

In the example below, `astro dev` will start your server at `/docs`.

```js
{
  base: '/docs'
}
```

When using this option, all of your static asset imports and URLs should add the base as a prefix. You can access this value via `import.meta.env.BASE_URL`.

The value of `import.meta.env.BASE_URL` will be determined by your `trailingSlash` config, no matter what value you have set for `base`.

A trailing slash is always included if `trailingSlash: "always"` is set. If `trailingSlash: "never"` is set, `BASE_URL` will not include a trailing slash, even if `base` includes one.

Additionally, Astro will internally manipulate the configured value of `config.base` before making it available to integrations. The value of `config.base` as read by integrations will also be determined by your `trailingSlash` configuration in the same way.

In the example below, the values of `import.meta.env.BASE_URL` and `config.base` when processed will both be `/docs`:

```js
{
   base: '/docs/',
   trailingSlash: "never"
}
```

In the example below, the values of `import.meta.env.BASE_URL` and `config.base` when processed will both be `/docs/`:

```js
{
   base: '/docs',
   trailingSlash: "always"
}
```

### trailingSlash

[Section titled “trailingSlash”](#trailingslash)

**Type:** `'always' | 'never' | 'ignore'`\
**Default:** `'ignore'`

Set the route matching behavior for trailing slashes in the dev server and on-demand rendered pages. Choose from the following options:

* `'ignore'` - Match URLs regardless of whether a trailing ”/” exists. Requests for “/about” and “/about/” will both match the same route.
* `'always'` - Only match URLs that include a trailing slash (e.g: “/about/”). In production, requests for on-demand rendered URLs without a trailing slash will be redirected to the correct URL for your convenience. However, in development, they will display a warning page reminding you that you have `always` configured.
* `'never'` - Only match URLs that do not include a trailing slash (e.g: “/about”). In production, requests for on-demand rendered URLs with a trailing slash will be redirected to the correct URL for your convenience. However, in development, they will display a warning page reminding you that you have `never` configured.

When redirects occur in production for GET requests, the redirect will be a 301 (permanent) redirect. For all other request methods, it will be a 308 (permanent, and preserve the request method) redirect.

Trailing slashes on prerendered pages are handled by the hosting platform, and may not respect your chosen configuration. See your hosting platform’s documentation for more information. You cannot use Astro [redirects](#redirects) for this use case at this point.

```js
{
  // Example: Require a trailing slash during development
  trailingSlash: 'always'
}
```

**See Also:**

* build.format

### redirects

[Section titled “redirects”](#redirects)

**Type:** `Record<string, RedirectConfig>`\
**Default:** `{}`

**Added in:** `astro@2.9.0`

Specify a mapping of redirects where the key is the route to match and the value is the path to redirect to.

You can redirect both static and dynamic routes, but only to the same kind of route. For example, you cannot have a `'/article': '/blog/[...slug]'` redirect.

```js
export default defineConfig({
  redirects: {
   '/old': '/new',
   '/blog/[...slug]': '/articles/[...slug]',
   '/about': 'https://example.com/about',
   '/news': {
     status: 302,
     destination: 'https://example.com/news'
   },
   // '/product1/', '/product1' // Note, this is not supported
  }
})
```

For statically-generated sites with no adapter installed, this will produce a client redirect using a [`<meta http-equiv="refresh">` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#http-equiv) and does not support status codes.

When using SSR or with a static adapter in `output: static` mode, status codes are supported. Astro will serve redirected GET requests with a status of `301` and use a status of `308` for any other request method.

You can customize the [redirection status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) using an object in the redirect config:

```js
export default defineConfig({
  redirects: {
    '/other': {
      status: 302,
      destination: '/place',
    },
  }
})
```

### output

[Section titled “output”](#output)

**Type:** `'static' | 'server'`\
**Default:** `'static'`

Specifies the output target for builds.

* `'static'` - Prerender all your pages by default, outputting a completely static site if none of your pages opt out of prerendering.
* `'server'` - Use server-side rendering (SSR) for all pages by default, always outputting a server-rendered site.

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  output: 'static'
})
```

**See Also:**

* adapter

### adapter

[Section titled “adapter”](#adapter)

**Type:** `AstroIntegration`

Deploy to your favorite server, serverless, or edge host with build adapters. Import one of our first-party adapters ([Cloudflare](/en/guides/integrations-guide/cloudflare/), [Netlify](/en/guides/integrations-guide/netlify/), [Node.js](/en/guides/integrations-guide/node/), [Vercel](/en/guides/integrations-guide/vercel/)) or explore [community adapters](https://astro.build/integrations/2/?search=\&categories%5B%5D=adapters) to enable on-demand rendering in your Astro project.

See our [on-demand rendering guide](/en/guides/on-demand-rendering/) for more on Astro’s server rendering options.

```js
import netlify from '@astrojs/netlify';
{
  // Example: Build for Netlify serverless deployment
  adapter: netlify(),
}
```

**See Also:**

* output

### integrations

[Section titled “integrations”](#integrations)

**Type:** `AstroIntegration[]`

Extend Astro with custom integrations. Integrations are your one-stop-shop for adding framework support (like Solid.js), new features (like sitemaps), and new libraries (like Partytown).

Read our [Integrations Guide](/en/guides/integrations/) for help getting started with Astro Integrations.

```js
import react from '@astrojs/react';
import mdx from '@astrojs/mdx';
{
  // Example: Add React + MDX support to Astro
  integrations: [react(), mdx()]
}
```

### root

[Section titled “root”](#root)

**Type:** `string`\
**CLI:** `--root`\
**Default:** `"."` (current working directory)

You should only provide this option if you run the `astro` CLI commands in a directory other than the project root directory. Usually, this option is provided via the CLI instead of the Astro config file, since Astro needs to know your project root before it can locate your config file.

If you provide a relative path (ex: `--root: './my-project'`) Astro will resolve it against your current working directory.

#### Examples

[Section titled “Examples”](#examples)

```js
{
  root: './my-project-directory'
}
```

```bash
$ astro build --root ./my-project-directory
```

### srcDir

[Section titled “srcDir”](#srcdir)

**Type:** `string`\
**Default:** `"./src"`

Set the directory that Astro will read your site from.

The value can be either an absolute file system path or a path relative to the project root.

```js
{
  srcDir: './www'
}
```

### publicDir

[Section titled “publicDir”](#publicdir)

**Type:** `string`\
**Default:** `"./public"`

Set the directory for your static assets. Files in this directory are served at `/` during dev and copied to your build directory during build. These files are always served or copied as-is, without transform or bundling.

The value can be either an absolute file system path or a path relative to the project root.

```js
{
  publicDir: './my-custom-publicDir-directory'
}
```

### outDir

[Section titled “outDir”](#outdir)

**Type:** `string`\
**Default:** `"./dist"`

Set the directory that `astro build` writes your final build to.

The value can be either an absolute file system path or a path relative to the project root.

```js
{
  outDir: './my-custom-build-directory'
}
```

**See Also:**

* build.server

### cacheDir

[Section titled “cacheDir”](#cachedir)

**Type:** `string`\
**Default:** `"./node_modules/.astro"`

Set the directory for caching build artifacts. Files in this directory will be used in subsequent builds to speed up the build time.

The value can be either an absolute file system path or a path relative to the project root.

```js
{
  cacheDir: './my-custom-cache-directory'
}
```

### compressHTML

[Section titled “compressHTML”](#compresshtml)

**Type:** `boolean`\
**Default:** `true`

This is an option to minify your HTML output and reduce the size of your HTML files.

By default, Astro removes whitespace from your HTML, including line breaks, from `.astro` components in a lossless manner. Some whitespace may be kept as needed to preserve the visual rendering of your HTML. This occurs both in development mode and in the final build.

To disable HTML compression, set `compressHTML` to false.

```js
{
  compressHTML: false
}
```

### scopedStyleStrategy

[Section titled “scopedStyleStrategy”](#scopedstylestrategy)

**Type:** `'where' | 'class' | 'attribute'`\
**Default:** `'attribute'`

**Added in:** `astro@2.4`

Specify the strategy used for scoping styles within Astro components. Choose from:

* `'where'` - Use `:where` selectors, causing no specificity increase.
* `'class'` - Use class-based selectors, causing a +1 specificity increase.
* `'attribute'` - Use `data-` attributes, causing a +1 specificity increase.

Using `'class'` is helpful when you want to ensure that element selectors within an Astro component override global style defaults (e.g. from a global stylesheet). Using `'where'` gives you more control over specificity, but requires that you use higher-specificity selectors, layers, and other tools to control which selectors are applied. Using `'attribute'` is useful when you are manipulating the `class` attribute of elements and need to avoid conflicts between your own styling logic and Astro’s application of styles.

### prerenderConflictBehavior

[Section titled “prerenderConflictBehavior”](#prerenderconflictbehavior)

**Type:** `'error' | 'warn' | 'ignore'`\
**Default:** `'warn'`

**Added in:** `astro@6.0`

Determines the default behavior when two routes generate the same prerendered URL:

* `error`: fail the build and display an error, forcing you to resolve the conflict
* `warn` (default): log a warning when conflicts occur, but build using the highest-priority route
* `ignore`: silently build using the highest-priority route when conflicts occur

```js
{
  prerenderConflictBehavior: 'error'
}
```

### vite

[Section titled “vite”](#vite)

**Type:** `ViteUserConfig`

Pass additional configuration options to Vite. Useful when Astro doesn’t support some advanced configuration that you may need.

View the full `vite` configuration object documentation on [vite.dev](https://vite.dev/config/).

#### Examples

[Section titled “Examples”](#examples-1)

```js
{
  vite: {
    ssr: {
      // Example: Force a broken package to skip SSR processing, if needed
      external: ['broken-npm-package'],
    }
  }
}
```

```js
{
  vite: {
    // Example: Add custom vite plugins directly to your Astro project
    plugins: [myPlugin()],
  }
}
```

### security

[Section titled “security”](#security)

**Type:** `Record<"checkOrigin", boolean> | undefined`\
**Default:** `{checkOrigin: true}`

**Added in:** `astro@4.9.0`

Enables security measures for an Astro website.

These features only exist for pages rendered on demand (SSR) using `server` mode or pages that opt out of prerendering in `static` mode.

By default, Astro will automatically check that the “origin” header matches the URL sent by each request in on-demand rendered pages. You can disable this behavior by setting `checkOrigin` to `false`:

astro.config.mjs

```js
export default defineConfig({
  output: "server",
  security: {
    checkOrigin: false
  }
})
```

#### security.checkOrigin

[Section titled “security.checkOrigin”](#securitycheckorigin)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@4.9.0`

Performs a check that the “origin” header, automatically passed by all modern browsers, matches the URL sent by each `Request`. This is used to provide Cross-Site Request Forgery (CSRF) protection.

The “origin” check is executed only for pages rendered on demand, and only for the requests `POST`, `PATCH`, `DELETE` and `PUT` with one of the following `content-type` headers: `'application/x-www-form-urlencoded'`, `'multipart/form-data'`, `'text/plain'`.

If the “origin” header doesn’t match the `pathname` of the request, Astro will return a 403 status code and will not render the page.

#### security.allowedDomains

[Section titled “security.allowedDomains”](#securityalloweddomains)

**Type:** `Array<RemotePattern>`\
**Default:** `[]`

**Added in:** `astro@5.14.2`

Defines a list of permitted host patterns for incoming requests when using SSR. When configured, Astro will validate the `X-Forwarded-Host` header against these patterns for security. If the header doesn’t match any allowed pattern, the header is ignored and the request’s original host is used instead.

This prevents host header injection attacks where malicious actors can manipulate the `Astro.url` value by sending crafted `X-Forwarded-Host` headers.

Each pattern can specify `protocol`, `hostname`, and `port`. All three are validated if provided. The patterns support wildcards for flexible hostname matching:

* `*.example.com` - matches exactly one subdomain level (e.g., `sub.example.com` but not `deep.sub.example.com`)
* `**.example.com` - matches any subdomain depth (e.g., both `sub.example.com` and `deep.sub.example.com`)

```js
{
  security: {
    // Example: Allow any subdomain of example.com on https
    allowedDomains: [
      {
        hostname: '**.example.com',
        protocol: 'https'
      },
      {
        hostname: 'staging.myapp.com',
        protocol: 'https',
        port: '443'
      }
    ]
  }
}
```

In some specific contexts (e.g., applications behind trusted reverse proxies with dynamic domains), you may need to allow all domains. To do this, use an empty object:

```js
{
  security: {
    // Allow any domain - use this only when necessary
    allowedDomains: [{}]
  }
}
```

When not configured, `X-Forwarded-Host` headers are not trusted and will be ignored.

#### security.actionBodySizeLimit

[Section titled “security.actionBodySizeLimit”](#securityactionbodysizelimit)

**Type:** `number`\
**Default:** `1048576` (1 MB)

**Added in:** `astro@5.18.0`

Sets the maximum size in bytes allowed for action request bodies.

By default, action request bodies are limited to 1 MB (1048576 bytes) to prevent abuse. You can increase this limit if your actions need to accept larger payloads, for example when handling file uploads.

astro.config.mjs

```js
export default defineConfig({
  security: {
    actionBodySizeLimit: 10 * 1024 * 1024 // 10 MB
  }
})
```

#### security.serverIslandBodySizeLimit

[Section titled “security.serverIslandBodySizeLimit”](#securityserverislandbodysizelimit)

**Type:** `number`\
**Default:** `1048576` (1 MB)

**Added in:** `astro@6.0.0`

Sets the maximum size in bytes allowed for server island request bodies, which contain the encrypted props and slot HTML passed to the island component.

By default, server island request bodies are limited to 1 MB (1048576 bytes) to prevent abuse. You can increase this limit if your server islands need to accept larger payloads.

astro.config.mjs

```js
export default defineConfig({
  security: {
    serverIslandBodySizeLimit: 10 * 1024 * 1024 // 10 MB
  }
})
```

#### security.csp

[Section titled “security.csp”](#securitycsp)

**Type:** `boolean | object`\
**Default:** `false`

**Added in:** `astro@6.0.0`

Enables support for [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) to help minimize certain types of security threats by controlling which resources a document is allowed to load. This provides additional protection against [cross-site scripting (XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) attacks.

Enabling this feature adds additional security to Astro’s handling of processed and bundled scripts and styles by default, and allows you to further configure these, and additional, content types.

This feature comes with some limitations:

* External scripts and external styles are not supported out of the box, but you can [provide your own hashes](#securitycspscriptdirectivehashes).
* [Astro’s view transitions](/en/guides/view-transitions/) using the `<ClientRouter />` are not supported, but you can [consider migrating to the browser native View Transition API](https://events-3bg.pages.dev/jotter/astro-view-transitions/) instead if you are not using Astro’s enhancements to the native View Transitions and Navigation APIs.
* Shiki isn’t currently supported. By design, Shiki functions use inline styles that cannot work with Astro CSP implementation. Consider [using `<Prism />`](/en/guides/syntax-highlighting/#prism-) when your project requires both CSP and syntax highlighting.
* `unsafe-inline` directives are incompatible with Astro’s CSP implementation. By default, Astro will emit hashes for all its bundled scripts (e.g. client islands) and all modern browsers will automatically reject `unsafe-inline` when it occurs in a directive with a hash or nonce.

Note

Due to the nature of the Vite dev server, this feature isn’t supported while working in `dev` mode. Instead, you can test this in your Astro project using `build` and `preview`.

When enabled, Astro will add a `<meta>` element inside the `<head>` element of each page. This element will have the `http-equiv="content-security-policy"` attribute, and the `content` attribute will provide values for the `script-src` and `style-src` [directives](#securitycspdirectives) based on the script and styles used in the page.

```html
<head>
  <meta
    http-equiv="content-security-policy"
    content="
    script-src 'self' 'sha256-somehash';
    style-src 'self' 'sha256-somehash';
    "
  >
</head>
```

You can further customize the `<meta>` element by enabling this feature with a configuration object that includes additional options.

##### security.csp.algorithm

[Section titled “security.csp.algorithm”](#securitycspalgorithm)

**Type:** `"SHA-256" | "SHA-384" | "SHA-512"`\
**Default:** `'SHA-256'`

**Added in:** `astro@6.0.0`

The [hash function](https://developer.mozilla.org/en-US/docs/Glossary/Hash_function) to use when generating the hashes of the styles and scripts emitted by Astro.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
     algorithm: 'SHA-512'
    }
  }
});
```

##### security.csp.directives

[Section titled “security.csp.directives”](#securitycspdirectives)

**Type:** `Array<string>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

A list of [CSP directives](https://content-security-policy.com/#directive) (beyond `script-src` and `style-src` which are included by default) that defines valid sources for specific content types. These directives are added to all pages.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      directives: [
        "default-src 'self'",
        "img-src 'self' https://images.cdn.example.com"
      ]
    }
  }
});
```

After the build, the `<meta>` element will add your directives into the `content` value alongside Astro’s default directives:

```html
<meta
  http-equiv="content-security-policy"
  content="
    default-src 'self';
    img-src 'self' 'https://images.cdn.example.com';
    script-src 'self' 'sha256-somehash';
    style-src 'self' 'sha256-somehash';
  "
>
```

##### security.csp.styleDirective

[Section titled “security.csp.styleDirective”](#securitycspstyledirective)

**Type:** `CspStyleDirective`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

A configuration object that allows you to override the default sources for the `style-src` directive with the [`resources`](#securitycspstyledirectiveresources) property, or to provide additional [hashes](#securitycspstyledirectivehashes) to be rendered.

###### security.csp.styleDirective.hashes

[Section titled “security.csp.styleDirective.hashes”](#securitycspstyledirectivehashes)

**Type:** `Array<CspHash>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

A list of additional hashes to be rendered.

You must provide hashes that start with `sha384-`, `sha512-` or `sha256-`. Other values will cause a validation error. These hashes are added to all pages.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      styleDirective: {
        hashes: [
          "sha384-styleHash",
          "sha512-styleHash",
          "sha256-styleHash"
        ]
      }
    }
  }
});
```

After the build, the `<meta>` element will include your additional hashes in the `style-src` directives:

```html
<meta
  http-equiv="content-security-policy"
  content="
    style-src 'self' 'sha384-styleHash' 'sha512-styleHash' 'sha256-styleHash' 'sha256-generatedByAstro';
  "
>
```

###### security.csp.styleDirective.resources

[Section titled “security.csp.styleDirective.resources”](#securitycspstyledirectiveresources)

**Type:** `Array<string>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

A list of valid sources for `style-src` directives to override Astro’s default sources. This will not include `'self'` by default, and must be included in this list if you wish to keep it. These resources are added to all pages.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      styleDirective: {
        resources: [
          "'self'",
          "https://styles.cdn.example.com"
        ]
      }
    }
  }
});
```

After the build, the `<meta>` element will instead apply your sources to the `style-src` directives:

```html
<head>
  <meta
    http-equiv="content-security-policy"
    content="
     style-src 'self' https://styles.cdn.example.com 'sha256-somehash';
    "
  >
</head>
```

When resources are inserted multiple times or from multiple sources (e.g. defined in your `csp` config and added using [the CSP runtime API](/en/reference/api-reference/#csp)), Astro will merge and deduplicate all resources to create your `<meta>` element.

##### security.csp.scriptDirective

[Section titled “security.csp.scriptDirective”](#securitycspscriptdirective)

**Type:** `CspScriptDirective`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

A configuration object that allows you to override the default sources for the `script-src` directive with the [`resources`](#securitycspscriptdirectiveresources) property, or to provide additional [hashes](#securitycspscriptdirectivehashes) to be rendered.

###### security.csp.scriptDirective.hashes

[Section titled “security.csp.scriptDirective.hashes”](#securitycspscriptdirectivehashes)

**Type:** `Array<CspHash>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

A list of additional hashes to be rendered.

You must provide hashes that start with `sha384-`, `sha512-` or `sha256-`. Other values will cause a validation error. These hashes are added to all pages.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      scriptDirective: {
        hashes: [
          "sha384-scriptHash",
          "sha512-scriptHash",
          "sha256-scriptHash"
        ]
      }
    }
  }
});
```

After the build, the `<meta>` element will include your additional hashes in the `script-src` directives:

```html
<meta
  http-equiv="content-security-policy"
  content="
    script-src 'self' 'sha384-scriptHash' 'sha512-scriptHash' 'sha256-scriptHash' 'sha256-generatedByAstro';
  "
>
```

###### security.csp.scriptDirective.resources

[Section titled “security.csp.scriptDirective.resources”](#securitycspscriptdirectiveresources)

**Type:** `Array<string>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

A list of valid sources for the `script-src` directives to override Astro’s default sources. This will not include `'self'` by default, and must be included in this list if you wish to keep it. These resources are added to all pages.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      scriptDirective: {
        resources: [
          "'self'", "https://cdn.example.com"
        ]
      }
    }
  }
});
```

After the build, the `<meta>` element will instead apply your sources to the `script-src` directives:

```html
<head>
  <meta
    http-equiv="content-security-policy"
    content="
     script-src 'self' https://cdn.example.com 'sha256-somehash';
    "
  >
</head>
```

When resources are inserted multiple times or from multiple sources (e.g. defined in your `csp` config and added using [the CSP runtime API](/en/reference/api-reference/#csp)), Astro will merge and deduplicate all resources to create your `<meta>` element.

###### security.csp.scriptDirective.strictDynamic

[Section titled “security.csp.scriptDirective.strictDynamic”](#securitycspscriptdirectivestrictdynamic)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@6.0.0`

Enables [the `strict-dynamic` keyword](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP#the_strict-dynamic_keyword) to support the dynamic injection of scripts.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  security: {
    csp: {
      scriptDirective: {
        strictDynamic: true
      }
    }
  }
});
```

## Build Options

[Section titled “Build Options”](#build-options)

### build.format

[Section titled “build.format”](#buildformat)

**Type:** `('file' | 'directory' | 'preserve')`\
**Default:** `'directory'`

Control the output file format of each page. This value may be set by an adapter for you.

* `'file'`: Astro will generate an HTML file named for each page route. (e.g. `src/pages/about.astro` and `src/pages/about/index.astro` both build the file `/about.html`)
* `'directory'`: Astro will generate a directory with a nested `index.html` file for each page. (e.g. `src/pages/about.astro` and `src/pages/about/index.astro` both build the file `/about/index.html`)
* `'preserve'`: Astro will generate HTML files exactly as they appear in your source folder. (e.g. `src/pages/about.astro` builds `/about.html` and `src/pages/about/index.astro` builds the file `/about/index.html`)

```js
{
  build: {
    // Example: Generate `page.html` instead of `page/index.html` during build.
    format: 'file'
  }
}
```

#### Effect on Astro.url

[Section titled “Effect on Astro.url”](#effect-on-astrourl)

Setting `build.format` controls what `Astro.url` is set to during the build. When it is:

* `directory` - The `Astro.url.pathname` will include a trailing slash to mimic folder behavior. (e.g. `/foo/`)
* `file` - The `Astro.url.pathname` will include `.html`. (e.g. `/foo.html`)

This means that when you create relative URLs using `new URL('./relative', Astro.url)`, you will get consistent behavior between dev and build.

To prevent inconsistencies with trailing slash behaviour in dev, you can restrict the [`trailingSlash` option](#trailingslash) to `'always'` or `'never'` depending on your build format:

* `directory` - Set `trailingSlash: 'always'`
* `file` - Set `trailingSlash: 'never'`

### build.client

[Section titled “build.client”](#buildclient)

**Type:** `string`\
**Default:** `'./client'`

Controls the output directory of your client-side CSS and JavaScript when building a website with server-rendered pages. `outDir` controls where the code is built to.

This value is relative to the `outDir`.

```js
{
  output: 'server',
  build: {
    client: './client'
  }
}
```

### build.server

[Section titled “build.server”](#buildserver)

**Type:** `string`\
**Default:** `'./server'`

Controls the output directory of server JavaScript when building to SSR.

This value is relative to the `outDir`.

```js
{
  build: {
    server: './server'
  }
}
```

### build.assets

[Section titled “build.assets”](#buildassets)

**Type:** `string`\
**Default:** `'_astro'`

**Added in:** `astro@2.0.0`

Specifies the directory in the build output where Astro-generated assets (bundled JS and CSS for example) should live.

```js
{
  build: {
    assets: '_custom'
  }
}
```

**See Also:**

* outDir

### build.assetsPrefix

[Section titled “build.assetsPrefix”](#buildassetsprefix)

**Type:** `string | Record<string, string>`\
**Default:** `undefined`

**Added in:** `astro@2.2.0`

Specifies the prefix for Astro-generated asset links. This can be used if assets are served from a different domain than the current site.

This requires uploading the assets in your local `./dist/_astro` folder to a corresponding `/_astro/` folder on the remote domain. To rename the `_astro` path, specify a new directory in `build.assets`.

To fetch all assets uploaded to the same domain (e.g. `https://cdn.example.com/_astro/...`), set `assetsPrefix` to the root domain as a string (regardless of your `base` configuration):

```js
{
  build: {
    assetsPrefix: 'https://cdn.example.com'
  }
}
```

**Added in:** `astro@4.5.0`

You can also pass an object to `assetsPrefix` to specify a different domain for each file type. In this case, a `fallback` property is required and will be used by default for any other files.

```js
{
  build: {
    assetsPrefix: {
      'js': 'https://js.cdn.example.com',
      'mjs': 'https://js.cdn.example.com',
      'css': 'https://css.cdn.example.com',
      'fallback': 'https://cdn.example.com'
    }
  }
}
```

### build.serverEntry

[Section titled “build.serverEntry”](#buildserverentry)

**Type:** `string`\
**Default:** `'entry.mjs'`

Specifies the file name of the server entrypoint when building to SSR. This entrypoint is usually dependent on which host you are deploying to and will be set by your adapter for you.

Note that it is recommended that this file ends with `.mjs` so that the runtime detects that the file is a JavaScript module.

```js
{
  build: {
    serverEntry: 'main.mjs'
  }
}
```

### build.redirects

[Section titled “build.redirects”](#buildredirects)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@2.6.0`

Specifies whether redirects will be output to HTML during the build. This option only applies to `output: 'static'` mode; in SSR redirects are treated the same as all responses.

This option is mostly meant to be used by adapters that have special configuration files for redirects and do not need/want HTML based redirects.

```js
{
  build: {
    redirects: false
  }
}
```

### build.inlineStylesheets

[Section titled “build.inlineStylesheets”](#buildinlinestylesheets)

**Type:** `'always' | 'auto' | 'never'`\
**Default:** `auto`

**Added in:** `astro@2.6.0`

Control whether project styles are sent to the browser in a separate css file or inlined into `<style>` tags. Choose from the following options:

* `'always'` - project styles are inlined into `<style>` tags
* `'auto'` - only stylesheets smaller than `ViteConfig.build.assetsInlineLimit` (default: 4kb) are inlined. Otherwise, project styles are sent in external stylesheets.
* `'never'` - project styles are sent in external stylesheets

```js
{
  build: {
    inlineStylesheets: `never`,
  },
}
```

### build.concurrency

[Section titled “build.concurrency”](#buildconcurrency)

**Type:** `number`\
**Default:** `1`

**Added in:** `astro@4.16.0`

The number of pages to build in parallel.

**In most cases, you should not change the default value of `1`.**

Use this option only when other attempts to reduce the overall rendering time (e.g. batch or cache long running tasks like fetch calls or data access) are not possible or are insufficient. If the number is set too high, page rendering may slow down due to insufficient memory resources and because JS is single-threaded.

```js
{
  build: {
    concurrency: 2
  }
}
```

Breaking changes possible

This feature is stable and is not considered experimental. However, this feature is only intended to address difficult performance issues, and breaking changes may occur in a [minor release](/en/upgrade-astro/#semantic-versioning) to keep this option as performant as possible. Please check the [Astro CHANGELOG](https://github.com/withastro/astro/blob/refs/heads/next/packages/astro/CHANGELOG.md) for every minor release if you are using this feature.

## Server Options

[Section titled “Server Options”](#server-options)

Customize the Astro dev server, used by both `astro dev` and `astro preview`.

```js
{
  server: { port: 1234, host: true}
}
```

To set different configuration based on the command run (“dev”, “preview”) a function can also be passed to this configuration option.

```js
{
  // Example: Use the function syntax to customize based on command
  server: ({ command }) => ({ port: command === 'dev' ? 4321 : 4000 })
}
```

### server.host

[Section titled “server.host”](#serverhost)

**Type:** `string | boolean`\
**Default:** `false`

**Added in:** `astro@0.24.0`

Set which network IP addresses the server should listen on (i.e. non-localhost IPs).

* `false` - do not expose on a network IP address
* `true` - listen on all addresses, including LAN and public addresses
* `[custom-address]` - expose on a network IP address at `[custom-address]` (ex: `192.168.0.1`)

### server.port

[Section titled “server.port”](#serverport)

**Type:** `number`\
**Default:** `4321`

Set which port the server should listen on.

If the given port is already in use, Astro will automatically try the next available port.

```js
{
  server: { port: 8080 }
}
```

### server.allowedHosts

[Section titled “server.allowedHosts”](#serverallowedhosts)

**Type:** `Array<string> | true`\
**Default:** `[]`

**Added in:** `astro@5.4.0`

A list of hostnames that Astro is allowed to respond to. When the value is set to `true`, any hostname is allowed.

```js
{
  server: {
    allowedHosts: ['staging.example.com', 'qa.example.com']
  }
}
```

### server.open

[Section titled “server.open”](#serveropen)

**Type:** `string | boolean`\
**Default:** `false`

**Added in:** `astro@4.1.0`

Controls whether the dev server should open in your browser window on startup.

Pass a full URL string (e.g. “<http://example.com>”) or a pathname (e.g. “/about”) to specify the URL to open.

```js
{
  server: { open: "/about" }
}
```

### server.headers

[Section titled “server.headers”](#serverheaders)

**Type:** `OutgoingHttpHeaders`\
**Default:** `{}`

**Added in:** `astro@1.7.0`

Set custom HTTP response headers to be sent in `astro dev` and `astro preview`.

## Session Options

[Section titled “Session Options”](#session-options)

**Added in:** `astro@5.7.0`

Configures session storage for your Astro project. This is used to store session data in a persistent way, so that it can be accessed across different requests. Some adapters may provide a default session driver, but you can override it with your own configuration.

See [the sessions guide](/en/guides/sessions/) for more information.

astro.config.mjs

```js
  {
    session: {
      // The name of the Unstorage driver
      driver: 'redis',
      // The required options depend on the driver
      options: {
        url: process.env.REDIS_URL,
      },
      ttl: 3600, // 1 hour
    }
  }
```

### session.driver

[Section titled “session.driver”](#sessiondriver)

**Type:** `SessionDriverConfig | undefined`

**Added in:** `astro@5.7.0`

The driver to use for session storage. The [Node](/en/guides/integrations-guide/node/#sessions), [Cloudflare](/en/guides/integrations-guide/cloudflare/#sessions), and [Netlify](/en/guides/integrations-guide/netlify/#sessions) adapters automatically configure a default driver for you, but you can specify your own if you would prefer or if you are using an adapter that does not provide one.

astro.config.mjs

```diff
import { defineConfig, sessionDrivers } from 'astro/config'
import vercel from '@astrojs/vercel'


export default defineConfig({
  adapter: vercel()
  session: {
+    driver: sessionDrivers.redis({
+      url: process.env.REDIS_URL
+    }),
  }
})
```

Note

Some drivers may need extra packages to be installed. Some drivers may also require environment variables or credentials to be set. See the [Unstorage documentation](https://unstorage.unjs.io/drivers) for more information.

### session.options

[Section titled “session.options”](#sessionoptions)

**Type:** `Record<string, unknown> | undefined`\
**Default:** `{}`

**Added in:** `astro@5.7.0`

Deprecated

This is deprecated and will be removed in a future major version. Instead, pass options to the driver function.

The driver-specific options to use for session storage. The options depend on the driver you are using. See the [Unstorage documentation](https://unstorage.unjs.io/drivers) for more information on the options available for each driver.

astro.config.mjs

```diff
{
   session: {
     driver: "redis",
+     options: {
+       url: process.env.REDIS_URL
+     },
   }
}
```

### session.cookie

[Section titled “session.cookie”](#sessioncookie)

**Type:** `string | AstroCookieSetOptions | undefined`\
**Default:** `{ name: "astro-session", sameSite: "lax", httpOnly: true, secure: true }`

**Added in:** `astro@5.7.0`

The session cookie configuration. If set to a string, it will be used as the cookie name. Alternatively, you can pass an object with additional options. These will be merged with the defaults.

astro.config.mjs

```diff
{
 session: {
   +// If set to a string, it will be used as the cookie name.
+   cookie: "my-session-cookie",
 }
}
```

astro.config.mjs

```diff
{
 session: {
   // If set to an object, it will be used as the cookie options.
+   cookie: {
+     name: "my-session-cookie",
+     sameSite: "lax",
+     secure: true,
+   }
 }
}
```

### session.ttl

[Section titled “session.ttl”](#sessionttl)

**Type:** `number | undefined`\
**Default:** Infinity

**Added in:** `astro@5.7.0`

An optional default time-to-live expiration period for session values, in seconds.

By default, session values persist until they are deleted or the session is destroyed, and do not automatically expire because a particular amount of time has passed. Set `session.ttl` to add a default expiration period for your session values. Passing a `ttl` option to [`session.set()`](/en/reference/api-reference/#sessionset) will override the global default for that individual entry.

astro.config.mjs

```diff
{
 session: {
   +// Set a default expiration period of 1 hour (3600 seconds)
+   ttl: 3600,
 }
}
```

Note

Setting a value for `ttl` does not automatically delete the value from storage after the time limit has passed.

Values from storage will only be deleted when there is an attempt to access them after the `ttl` period has expired. At this time, the session value will be undefined and only then will the value be deleted.

Individual drivers may also support a `ttl` option that will automatically delete sessions after the specified time. See your chosen driver’s documentation for more information.

## Dev Toolbar Options

[Section titled “Dev Toolbar Options”](#dev-toolbar-options)

### devToolbar.enabled

[Section titled “devToolbar.enabled”](#devtoolbarenabled)

**Type:** `boolean`\
**Default:** `true`

Whether to enable the Astro Dev Toolbar. This toolbar allows you to inspect your page islands, see helpful audits on performance and accessibility, and more.

This option is scoped to the entire project, to only disable the toolbar for yourself, run `npm run astro preferences disable devToolbar`. To disable the toolbar for all your Astro projects, run `npm run astro preferences disable devToolbar --global`.

### devToolbar.placement

[Section titled “devToolbar.placement”](#devtoolbarplacement)

**Type:** `'bottom-left' | 'bottom-center' | 'bottom-right'`\
**Default:** `'bottom-center'`

**Added in:** `astro@5.17.0`

The default placement of the Astro Dev Toolbar on the screen.

The placement of the toolbar can still be changed via the toolbar settings UI. Once changed, the user’s preference is saved in `localStorage` and overrides this configuration value.

## Prefetch Options

[Section titled “Prefetch Options”](#prefetch-options)

**Type:** `boolean | object`

Enable prefetching for links on your site to provide faster page transitions. (Enabled by default on pages using the `<ClientRouter />` router. Set `prefetch: false` to opt out of this behaviour.)

This configuration automatically adds a prefetch script to every page in the project giving you access to the `data-astro-prefetch` attribute. Add this attribute to any `<a />` link on your page to enable prefetching for that page.

```html
<a href="/about" data-astro-prefetch>About</a>
```

Further customize the default prefetching behavior using the [`prefetch.defaultStrategy`](#prefetchdefaultstrategy) and [`prefetch.prefetchAll`](#prefetchprefetchall) options.

See the [Prefetch guide](/en/guides/prefetch/) for more information.

### prefetch.prefetchAll

[Section titled “prefetch.prefetchAll”](#prefetchprefetchall)

**Type:** `boolean`

Enable prefetching for all links, including those without the `data-astro-prefetch` attribute. This value defaults to `true` when using the `<ClientRouter />` router. Otherwise, the default value is `false`.

```js
prefetch: {
  prefetchAll: true
}
```

When set to `true`, you can disable prefetching individually by setting `data-astro-prefetch="false"` on any individual links.

```html
<a href="/about" data-astro-prefetch="false">About</a>
```

### prefetch.defaultStrategy

[Section titled “prefetch.defaultStrategy”](#prefetchdefaultstrategy)

**Type:** `'tap' | 'hover' | 'viewport' | 'load'`\
**Default:** `'hover'`

The default prefetch strategy to use when the `data-astro-prefetch` attribute is set on a link with no value.

* `'tap'`: Prefetch just before you click on the link.
* `'hover'`: Prefetch when you hover over or focus on the link. (default)
* `'viewport'`: Prefetch as the links enter the viewport.
* `'load'`: Prefetch all links on the page after the page is loaded.

You can override this default value and select a different strategy for any individual link by setting a value on the attribute.

```html
<a href="/about" data-astro-prefetch="viewport">About</a>
```

## Image Options

[Section titled “Image Options”](#image-options)

### image.endpoint

[Section titled “image.endpoint”](#imageendpoint)

**Type:** `Object`\
**Default:** `{route: '/_image', entrypoint: undefined}`

**Added in:** `astro@3.1.0`

Set the endpoint to use for image optimization in dev and SSR. The `entrypoint` property can be set to `undefined` to use the default image endpoint.

```js
{
  image: {
    // Example: Use a custom image endpoint at `/custom_endpoint`
    endpoint: {
       route: '/custom_endpoint',
       entrypoint: 'src/my_endpoint.ts',
    },
  },
}
```

### image.service

[Section titled “image.service”](#imageservice)

**Type:** `Object`\
**Default:** `{entrypoint: 'astro/assets/services/sharp', config?: {}}`

**Added in:** `astro@2.1.0`

Set which image service is used for Astro’s assets support.

The value should be an object with an entrypoint for the image service to use and optionally, a config object to pass to the service.

The service entrypoint can be either one of the included services, or a third-party package.

```js
{
  image: {
    // Example: Enable the Sharp-based image service with a custom config
    service: {
       entrypoint: 'astro/assets/services/sharp',
       config: {
         limitInputPixels: false,
         webp: {
           effort: 6,
           alphaQuality: 80,
         },
         jpeg: {
           mozjpeg: true,
         },
      },
     },
  },
}
```

#### image.service.config.limitInputPixels

[Section titled “image.service.config.limitInputPixels”](#imageserviceconfiglimitinputpixels)

**Type:** `number | boolean`\
**Default:** `true`

**Added in:** `astro@4.1.0`

Whether or not to limit the size of images that the Sharp image service will process.

Set `false` to bypass the default image size limit for the Sharp image service and process large images.

#### image.service.config.kernel

[Section titled “image.service.config.kernel”](#imageserviceconfigkernel)

**Type:** `string | undefined`\
**Default:** `undefined`

**Added in:** `astro@5.17.0`

The default [kernel used for resizing images](https://sharp.pixelplumbing.com/api-resize/#resize) in the Sharp image service.

By default this is `undefined`, which maps to Sharp’s default kernel of `lanczos3`.

#### image.service.config.jpeg

[Section titled “image.service.config.jpeg”](#imageserviceconfigjpeg)

**Type:** `Record<string, any> | undefined`\
**Default:** `undefined`

**Added in:** `astro@6.1.0` New

The default encoder options passed to `sharp().jpeg()` when using Astro’s built-in Sharp image service.

This can be used for options such as `mozjpeg`, `progressive`, `chromaSubsampling`, or a default `quality`. Per-image `quality` values from `<Image />`, `<Picture />`, and `getImage()` still take precedence.

#### image.service.config.webp

[Section titled “image.service.config.webp”](#imageserviceconfigwebp)

**Type:** `Record<string, any> | undefined`\
**Default:** `undefined`

**Added in:** `astro@6.1.0` New

The default encoder options passed to `sharp().webp()` when using Astro’s built-in Sharp image service.

This can be used for options such as `effort`, `alphaQuality`, `lossless`, `nearLossless`, or a default `quality`. Per-image `quality` values from `<Image />`, `<Picture />`, and `getImage()` still take precedence.

#### image.service.config.avif

[Section titled “image.service.config.avif”](#imageserviceconfigavif)

**Type:** `Record<string, any> | undefined`\
**Default:** `undefined`

**Added in:** `astro@6.1.0` New

The default encoder options passed to `sharp().avif()` when using Astro’s built-in Sharp image service.

This can be used for options such as `effort`, `chromaSubsampling`, `bitdepth`, `lossless`, or a default `quality`. Per-image `quality` values from `<Image />`, `<Picture />`, and `getImage()` still take precedence.

#### image.service.config.png

[Section titled “image.service.config.png”](#imageserviceconfigpng)

**Type:** `Record<string, any> | undefined`\
**Default:** `undefined`

**Added in:** `astro@6.1.0` New

The default encoder options passed to `sharp().png()` when using Astro’s built-in Sharp image service.

This can be used for options such as `compressionLevel`, `effort`, `palette`, or a default `quality`. Per-image `quality` values from `<Image />`, `<Picture />`, and `getImage()` still take precedence.

### image.domains

[Section titled “image.domains”](#imagedomains)

**Type:** `Array<string>`\
**Default:** `[]`

**Added in:** `astro@2.10.10`

Defines a list of permitted image source domains for remote image optimization. No other remote images will be optimized by Astro.

This option requires an array of individual domain names as strings. Wildcards are not permitted. Instead, use [`image.remotePatterns`](#imageremotepatterns) to define a list of allowed source URL patterns.

astro.config.mjs

```js
{
  image: {
    // Example: Allow remote image optimization from a single domain
    domains: ['astro.build'],
  },
}
```

### image.remotePatterns

[Section titled “image.remotePatterns”](#imageremotepatterns)

**Type:** `Array<RemotePattern>`\
**Default:** `[]`

**Added in:** `astro@2.10.10`

Defines a list of permitted image source URL patterns for remote image optimization.

`remotePatterns` can be configured with four properties:

1. protocol
2. hostname
3. port
4. pathname

```js
{
  image: {
    // Example: allow processing all images from your aws s3 bucket
    remotePatterns: [{
      protocol: 'https',
      hostname: '**.amazonaws.com',
    }],
  },
}
```

You can use wildcards to define the permitted `hostname` and `pathname` values as described below. Otherwise, only the exact values provided will be configured: `hostname`:

* Start with ’\*\*.’ to allow all subdomains (‘endsWith’).
* Start with ’\*.’ to allow only one level of subdomain.

`pathname`:

* End with ’/\*\*’ to allow all sub-routes (‘startsWith’).
* End with ’/\*’ to allow only one level of sub-route.

### image.responsiveStyles

[Section titled “image.responsiveStyles”](#imageresponsivestyles)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@5.10.0`

Whether to automatically add global styles for responsive images. You should enable this option unless you are styling the images yourself.

This option is only used when `layout` is set to `constrained`, `full-width`, or `fixed` using the configuration or the `layout` prop on the image component.

See [the images docs](/en/guides/images/#responsive-image-styles) for more information.

### image.layout

[Section titled “image.layout”](#imagelayout)

**Type:** `ImageLayout`\
**Default:** `undefined`

**Added in:** `astro@5.10.0`

The default layout type for responsive images. Can be overridden by the `layout` prop on the image component.

* `constrained` - The image will scale to fit the container, maintaining its aspect ratio, but will not exceed the specified dimensions.
* `fixed` - The image will maintain its original dimensions.
* `full-width` - The image will scale to fit the container, maintaining its aspect ratio.

See [the `layout` component property](/en/reference/modules/astro-assets/#layout) for more details.

### image.objectFit

[Section titled “image.objectFit”](#imageobjectfit)

**Type:** `ImageFit`\
**Default:** `"cover"`

**Added in:** `astro@5.10.0`

The [`object-fit` CSS property value](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit) for responsive images. Can be overridden by the `fit` prop on the image component. Requires a value for `layout` to be set.

See [the `fit` component property](/en/reference/modules/astro-assets/#fit) for more details.

### image.objectPosition

[Section titled “image.objectPosition”](#imageobjectposition)

**Type:** `string`\
**Default:** `"center"`

**Added in:** `astro@5.10.0`

The default [`object-position` CSS property value](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position) for responsive images. Can be overridden by the `position` prop on the image component. Requires a value for `layout` to be set.

See [the `position` component property](/en/reference/modules/astro-assets/#position) for more details.

### image.breakpoints

[Section titled “image.breakpoints”](#imagebreakpoints)

**Type:** `Array<number>`\
**Default:** `[640, 750, 828, 1080, 1280, 1668, 2048, 2560] | [640, 750, 828, 960, 1080, 1280, 1668, 1920, 2048, 2560, 3200, 3840, 4480, 5120, 6016]`

**Added in:** `astro@5.10.0`

The breakpoints used to generate responsive images. Requires a value for `layout` to be set. The full list is not normally used, but is filtered according to the source and output size. The defaults used depend on whether a local or remote image service is used. For remote services the more comprehensive list is used, because only the required sizes are generated. For local services, the list is shorter to reduce the number of images generated.

## Markdown Options

[Section titled “Markdown Options”](#markdown-options)

### markdown.shikiConfig

[Section titled “markdown.shikiConfig”](#markdownshikiconfig)

**Type:** `Partial<ShikiConfig>`

Shiki is our default syntax highlighter. You can configure all options via the `markdown.shikiConfig` object:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
    shikiConfig: {
      // Choose from Shiki's built-in themes (or add your own)
      // https://shiki.style/themes
      theme: 'dracula',
      // Alternatively, provide multiple themes
      // See note below for using dual light/dark themes
      themes: {
        light: 'github-light',
        dark: 'github-dark',
      },
      // Disable the default colors
      // https://shiki.style/guide/dual-themes#without-default-color
      // (Added in v4.12.0)
      defaultColor: false,
      // Add custom languages
      // Note: Shiki has countless langs built-in, including .astro!
      // https://shiki.style/languages
      langs: [],
      // Add custom aliases for languages
      // Map an alias to a Shiki language ID: https://shiki.style/languages#bundled-languages
      // https://shiki.style/guide/load-lang#custom-language-aliases
      langAlias: {
        cjs: "javascript"
      },
      // Enable word wrap to prevent horizontal scrolling
      wrap: true,
      // Add custom transformers: https://shiki.style/guide/transformers
      // Find common transformers: https://shiki.style/packages/transformers
      transformers: [],
    },
  },
});
```

See the [code syntax highlighting guide](/en/guides/syntax-highlighting/) for usage and examples.

### markdown.syntaxHighlight

[Section titled “markdown.syntaxHighlight”](#markdownsyntaxhighlight)

**Type:** `SyntaxHighlightConfig | SyntaxHighlightConfigType | false`\
**Default:** `{ type: 'shiki', excludeLangs: ['math'] }`

Which syntax highlighter to use for Markdown code blocks (\`\`\`), if any. This determines the CSS classes that Astro will apply to your Markdown code blocks.

* `shiki` - use the [Shiki](https://shiki.style) highlighter (`github-dark` theme configured by default)
* `prism` - use the [Prism](https://prismjs.com/) highlighter and [provide your own Prism stylesheet](/en/guides/syntax-highlighting/#add-a-prism-stylesheet)
* `false` - do not apply syntax highlighting.

```js
{
  markdown: {
    // Example: Switch to use prism for syntax highlighting in Markdown
    syntaxHighlight: 'prism',
  }
}
```

For more control over syntax highlighting, you can instead specify a configuration object with the properties listed below.

#### markdown.syntaxHighlight.type

[Section titled “markdown.syntaxHighlight.type”](#markdownsyntaxhighlighttype)

**Type:** `'shiki' | 'prism'`\
**Default:** `'shiki'`

**Added in:** `astro@5.5.0`

The default CSS classes to apply to Markdown code blocks. (If no other syntax highlighting configuration is needed, you can instead set `markdown.syntaxHighlight` directly to `shiki`, `prism`, or `false`.)

#### markdown.syntaxHighlight.excludeLangs

[Section titled “markdown.syntaxHighlight.excludeLangs”](#markdownsyntaxhighlightexcludelangs)

**Type:** `Array<string>`\
**Default:** `['math']`

**Added in:** `astro@5.5.0`

An array of languages to exclude from the default syntax highlighting specified in `markdown.syntaxHighlight.type`. This can be useful when using tools that create diagrams from Markdown code blocks, such as Mermaid.js and D2.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
    syntaxHighlight: {
      type: 'shiki',
      excludeLangs: ['mermaid', 'math'],
    },
  },
});
```

### markdown.remarkPlugins

[Section titled “markdown.remarkPlugins”](#markdownremarkplugins)

**Type:** `RemarkPlugins`

Pass [remark plugins](https://github.com/remarkjs/remark) to customize how your Markdown is built. You can import and apply the plugin function (recommended), or pass the plugin name as a string.

```js
import remarkToc from 'remark-toc';
{
  markdown: {
    remarkPlugins: [ [remarkToc, { heading: "contents"} ] ]
  }
}
```

### markdown.rehypePlugins

[Section titled “markdown.rehypePlugins”](#markdownrehypeplugins)

**Type:** `RehypePlugins`

Pass [rehype plugins](https://github.com/remarkjs/remark-rehype) to customize how your Markdown’s output HTML is processed. You can import and apply the plugin function (recommended), or pass the plugin name as a string.

```js
import { rehypeAccessibleEmojis } from 'rehype-accessible-emojis';
{
  markdown: {
    rehypePlugins: [rehypeAccessibleEmojis]
  }
}
```

### markdown.gfm

[Section titled “markdown.gfm”](#markdowngfm)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@2.0.0`

Astro uses [GitHub-flavored Markdown](https://github.com/remarkjs/remark-gfm) by default. To disable this, set the `gfm` flag to `false`:

```js
{
  markdown: {
    gfm: false,
  }
}
```

### markdown.smartypants

[Section titled “markdown.smartypants”](#markdownsmartypants)

**Type:** `boolean | Smartypants`\
**Default:** `true`

**Added in:** `astro@2.0.0`

Whether to use the [SmartyPants formatter](https://daringfireball.net/projects/smartypants/) to transform straight quotes into smart quotes, dashes into en/em dashes, and triple dots into ellipses.

To disable this, set the `smartypants` flag to `false`.

For more control over typography, you can instead specify a configuration object with the [properties supported by `retext-smartypants`](https://github.com/retextjs/retext-smartypants?tab=readme-ov-file#fields).

### markdown.remarkRehype

[Section titled “markdown.remarkRehype”](#markdownremarkrehype)

**Type:** `RemarkRehype`

Pass options to [remark-rehype](https://github.com/remarkjs/remark-rehype#api).

```js
{
  markdown: {
    // Example: Translate the footnotes text to another language, here are the default English values
    remarkRehype: { footnoteLabel: "Footnotes", footnoteBackLabel: "Back to reference 1"},
  },
};
```

## i18n

[Section titled “i18n”](#i18n)

**Type:** `object`

**Added in:** `astro@3.5.0`

Configures i18n routing and allows you to specify some customization options.

See our guide for more information on [internationalization in Astro](/en/guides/internationalization/)

### i18n.locales

[Section titled “i18n.locales”](#i18nlocales)

**Type:** `Locales`

**Added in:** `astro@3.5.0`

A list of all locales supported by the website. This is a required field.

Languages can be listed either as individual codes (e.g. `['en', 'es', 'pt-br']`) or mapped to a shared `path` of codes (e.g. `{ path: "english", codes: ["en", "en-US"]}`). These codes will be used to determine the URL structure of your deployed site.

No particular language code format or syntax is enforced, but your project folders containing your content files must match exactly the `locales` items in the list. In the case of multiple `codes` pointing to a custom URL path prefix, store your content files in a folder with the same name as the `path` configured.

### i18n.defaultLocale

[Section titled “i18n.defaultLocale”](#i18ndefaultlocale)

**Type:** `string`

**Added in:** `astro@3.5.0`

The default locale of your website/application, that is one of the specified `locales`. This is a required field.

No particular language format or syntax is enforced, but we suggest using lower-case and hyphens as needed (e.g. “es”, “pt-br”) for greatest compatibility.

### i18n.fallback

[Section titled “i18n.fallback”](#i18nfallback)

**Type:** `Record<string, string>`

**Added in:** `astro@3.5.0`

The fallback strategy when navigating to pages that do not exist (e.g. a translated page has not been created).

Use this object to declare a fallback `locale` route for each language you support. If no fallback is specified, then unavailable pages will return a 404.

##### Example

[Section titled “Example”](#example)

The following example configures your content fallback strategy to redirect unavailable pages in `/pt-br/` to their `es` version, and unavailable pages in `/fr/` to their `en` version. Unavailable `/es/` pages will return a 404.

```js
export default defineConfig({
  i18n: {
    defaultLocale: "en",
    locales: ["en", "fr", "pt-br", "es"],
    fallback: {
      pt: "es",
      fr: "en"
    }
  }
})
```

### i18n.routing

[Section titled “i18n.routing”](#i18nrouting)

**Type:** `object | "manual"`\
**Default:** `object`

**Added in:** `astro@3.7.0`

Controls the routing strategy to determine your site URLs. Set this based on your folder/URL path configuration for your default language.

```js
export default defineConfig({
  i18n: {
    defaultLocale: "en",
    locales: ["en", "fr"],
    routing: {
      prefixDefaultLocale: false,
      redirectToDefaultLocale: true,
      fallbackType: "redirect",
    }
  }
})
```

Since 4.6.0, this option can also be set to `manual`. When this routing strategy is enabled, Astro will **disable** its i18n middleware and no other `routing` options (e.g. `prefixDefaultLocale`) may be configured. You will be responsible for writing your own routing logic, or executing Astro’s i18n middleware manually alongside your own.

```js
export default defineConfig({
  i18n: {
    defaultLocale: "en",
    locales: ["en", "fr"],
    routing: "manual"
  }
})
```

#### i18n.routing.prefixDefaultLocale

[Section titled “i18n.routing.prefixDefaultLocale”](#i18nroutingprefixdefaultlocale)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@3.7.0`

When `false`, only non-default languages will display a language prefix. The `defaultLocale` will not show a language prefix and content files do not exist in a localized folder. URLs will be of the form `example.com/[locale]/content/` for all non-default languages, but `example.com/content/` for the default locale.

When `true`, all URLs will display a language prefix. URLs will be of the form `example.com/[locale]/content/` for every route, including the default language. Localized folders are used for every language, including the default.

```js
export default defineConfig({
  i18n: {
    defaultLocale: "en",
    locales: ["en", "fr", "pt-br", "es"],
    routing: {
      prefixDefaultLocale: true,
    }
  }
})
```

#### i18n.routing.redirectToDefaultLocale

[Section titled “i18n.routing.redirectToDefaultLocale”](#i18nroutingredirecttodefaultlocale)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@4.2.0`

Configures whether or not the home URL (`/`) generated by `src/pages/index.astro` will redirect to `/[defaultLocale]` when `prefixDefaultLocale: true` is set.

Set `redirectToDefaultLocale: true` to enable this automatic redirection at the root of your site:

astro.config.mjs

```js
export default defineConfig({
  i18n:{
    defaultLocale: "en",
    locales: ["en", "fr"],
    routing: {
      prefixDefaultLocale: true,
      redirectToDefaultLocale: true
    }
  }
})
```

#### i18n.routing.fallbackType

[Section titled “i18n.routing.fallbackType”](#i18nroutingfallbacktype)

**Type:** `"redirect" | "rewrite"`\
**Default:** `"redirect"`

**Added in:** `astro@4.15.0`

When [`i18n.fallback`](#i18nfallback) is configured to avoid showing a 404 page for missing page routes, this option controls whether to [redirect](/en/guides/routing/#redirects) to the fallback page, or to [rewrite](/en/guides/routing/#rewrites) the fallback page’s content in place.

By default, Astro’s i18n routing creates pages that redirect your visitors to a new destination based on your fallback configuration. The browser will refresh and show the destination address in the URL bar.

When `i18n.routing.fallback: "rewrite"` is configured, Astro will create pages that render the contents of the fallback page on the original, requested URL.

With the following configuration, if you have the file `src/pages/en/about.astro` but not `src/pages/fr/about.astro`, the `astro build` command will generate `dist/fr/about.html` with the same content as the `dist/en/about.html` page. Your site visitor will see the English version of the page at `https://example.com/fr/about/` and will not be redirected.

astro.config.mjs

```js
export default defineConfig({
   i18n: {
    defaultLocale: "en",
    locales: ["en", "fr"],
    routing: {
      prefixDefaultLocale: false,
      fallbackType: "rewrite",
    },
    fallback: {
      fr: "en",
    }
  },
})
```

### i18n.domains

[Section titled “i18n.domains”](#i18ndomains)

**Type:** `Record<string, string>`\
**Default:** `{}`

**Added in:** `astro@4.3.0`

Configures the URL pattern of one or more supported languages to use a custom domain (or sub-domain).

When a locale is mapped to a domain, a `/[locale]/` path prefix will not be used. However, localized folders within `src/pages/` are still required, including for your configured `defaultLocale`.

Any other locale not configured will default to a localized path-based URL according to your `prefixDefaultLocale` strategy (e.g. `https://example.com/[locale]/blog`).

astro.config.mjs

```js
export default defineConfig({
   site: "https://example.com",
   output: "server", // required, with no prerendered pages
   adapter: node({
     mode: 'standalone',
   }),
   i18n: {
    defaultLocale: "en",
    locales: ["en", "fr", "pt-br", "es"],
    prefixDefaultLocale: false,
    domains: {
      fr: "https://fr.example.com",
      es: "https://example.es"
    }
  },
})
```

Both page routes built and URLs returned by the `astro:i18n` helper functions [`getAbsoluteLocaleUrl()`](/en/reference/modules/astro-i18n/#getabsolutelocaleurl) and [`getAbsoluteLocaleUrlList()`](/en/reference/modules/astro-i18n/#getabsolutelocaleurllist) will use the options set in `i18n.domains`.

See the [Internationalization Guide](/en/guides/internationalization/#domains) for more details, including the limitations of this feature.

## env

[Section titled “env”](#env)

**Type:** `object`\
**Default:** `{}`

**Added in:** `astro@5.0.0`

Configuration options for type-safe environment variables.

See our guide for more information on [environment variables in Astro](/en/guides/environment-variables/).

### env.schema

[Section titled “env.schema”](#envschema)

**Type:** `EnvSchema`\
**Default:** `{}`

**Added in:** `astro@5.0.0`

Defines environment variables to be enforced by Zod validation and for which TypeScript support (e.g. autocompletion, type-safety) is available. Each key corresponds to the variable name and the value to the data type and validations [defined with `envField`](/en/reference/modules/astro-config/#envfield).

Four data types are supported: string, number, enumeration, and boolean. Each type requires a `context` (client or server), an `access` level (public or secret), and additional validations, such as a `default` value and an indication of whether the variable is `optional` (defaults to `false`).

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config"


export default defineConfig({
  env: {
    schema: {
      API_URL: envField.string({ context: "client", access: "public", optional: true }),
      PORT: envField.number({ context: "server", access: "public", default: 4321 }),
      API_SECRET: envField.string({ context: "server", access: "secret" }),
    }
  }
})
```

### env.validateSecrets

[Section titled “env.validateSecrets”](#envvalidatesecrets)

**Type:** `boolean`\
**Default:** `false`

**Added in:** `astro@5.0.0`

Whether or not to validate secrets on the server when starting the dev server or running a build.

By default, only public variables are validated on the server when starting the dev server or a build, and private variables are validated at runtime only. If enabled, private variables will also be checked on start. This is useful in some continuous integration (CI) pipelines to make sure all your secrets are correctly set before deploying.

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config"


export default defineConfig({
  env: {
    schema: {
      // ...
    },
    validateSecrets: true
  }
})
```

## fonts

[Section titled “fonts”](#fonts)

**Type:** `Array<FontFamily>`\
**Default:** `[]`

**Added in:** `astro@6.0.0`

Configures fonts and allows you to specify some customization options on a per-font basis.

See our guide for more information on [using custom fonts in Astro](/en/guides/fonts/).

### font.provider

[Section titled “font.provider”](#fontprovider)

**Type:** `FontProvider`

**Added in:** `astro@6.0.0`

The source of your font files. You can use a [built-in provider](/en/reference/font-provider-reference/#built-in-providers) or write your own [custom provider](/en/reference/font-provider-reference/#building-a-font-provider):

```js
import { defineConfig, fontProviders } from "astro/config";


export default defineConfig({
  fonts: [{
    provider: fontProviders.google(),
    name: "Roboto",
    cssVariable: "--font-roboto"
  }]
});
```

### font.name

[Section titled “font.name”](#fontname)

**Type:** `string`

**Added in:** `astro@6.0.0`

The font family name, as identified by your font provider:

```js
name: "Roboto"
```

### font.cssVariable

[Section titled “font.cssVariable”](#fontcssvariable)

**Type:** `string`

**Added in:** `astro@6.0.0`

A valid [ident](https://developer.mozilla.org/en-US/docs/Web/CSS/ident) of your choosing in the form of a CSS variable (i.e. starting with `--`):

```js
cssVariable: "--font-roboto"
```

### font.fallbacks

[Section titled “font.fallbacks”](#fontfallbacks)

**Type:** `Array<string>`\
**Default:** `["sans-serif"]`

**Added in:** `astro@6.0.0`

An array of fonts to use when your chosen font is unavailable, or loading. Fallback fonts will be chosen in the order listed. The first available font will be used:

```js
fallbacks: ["CustomFont", "serif"]
```

To disable fallback fonts completely, configure an empty array:

```js
fallbacks: []
```

Specify at least a [generic family name](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#generic-name) matching the intended appearance of your font. Astro will then attempt to generate [optimized fallbacks](https://developer.chrome.com/blog/font-fallbacks) using font metrics. To disable this optimization, set `optimizedFallbacks` to false.

### font.optimizedFallbacks

[Section titled “font.optimizedFallbacks”](#fontoptimizedfallbacks)

**Type:** `boolean`\
**Default:** `true`

**Added in:** `astro@6.0.0`

Whether or not to enable Astro’s default optimization when generating fallback fonts. You may disable this default optimization to have full control over how [`fallbacks`](#fontfallbacks) are generated:

```js
optimizedFallbacks: false
```

### font.weights

[Section titled “font.weights”](#fontweights)

**Type:** `Array<(number|string)>`\
**Default:** `[400]`

**Added in:** `astro@6.0.0`

An array of [font weights](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight). If no value is specified in your configuration, only weight `400` is included by default to prevent unnecessary downloads. You will need to include this property to access any other font weights:

```js
weights: [200, "400", "bold"]
```

If the associated font is a [variable font](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide), you can specify a range of weights:

```js
weights: ["100 900"]
```

### font.styles

[Section titled “font.styles”](#fontstyles)

**Type:** `Array<("normal"|"italic"|"oblique")>`\
**Default:** `["normal", "italic"]`

**Added in:** `astro@6.0.0`

An array of [font styles](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style):

```js
styles: ["normal", "oblique"]
```

### font.subsets

[Section titled “font.subsets”](#fontsubsets)

**Type:** `Array<string>`\
**Default:** `["latin"]`

**Added in:** `astro@6.0.0`

Defines a list of [font subsets](https://knaap.dev/posts/font-subsetting/) to preload.

```js
subsets: ["latin"]
```

### font.formats

[Section titled “font.formats”](#fontformats)

**Type:** `Array<("woff2"|"woff"|"otf"|"ttf"|"eot")>`\
**Default:** `["woff2"]`

**Added in:** `astro@6.0.0`

An array of [font formats](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@font-face/src#font_formats):

```js
formats: ["woff2", "woff"]
```

### font.options

[Section titled “font.options”](#fontoptions)

**Type:** `Record<string, any>`

**Added in:** `astro@6.0.0`

An object to pass provider specific options. It is typed automatically based on the font family [provider](#fontprovider):

```js
options: {
  experimental: {
    glyphs: ["a"]
  }
}
```

### font.display

[Section titled “font.display”](#fontdisplay)

**Type:** `"auto" | "block" | "swap" | "fallback" | "optional"`\
**Default:** `"swap"`

**Added in:** `astro@6.0.0`

Defines [how a font displays](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display) based on when it is downloaded and ready for use:

```js
display: "block"
```

### font.unicodeRange

[Section titled “font.unicodeRange”](#fontunicoderange)

**Type:** `Array<string>`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

Determines when a font must be downloaded and used based on a specific [range of unicode characters](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/unicode-range). If a character on the page matches the configured range, the browser will download the font and all characters will be available for use on the page. To configure a subset of characters preloaded for a single font, see the [subsets](#fontsubsets) property instead.

This can be useful for localization to avoid unnecessary font downloads when a specific part of your website uses a different alphabet and will be displayed with a separate font. For example, a website that offers both English and Japanese versions can prevent the browser from downloading the Japanese font on English versions of the page that do not contain any of the Japanese characters provided in `unicodeRange`.

```js
unicodeRange: ["U+26"]
```

### font.stretch

[Section titled “font.stretch”](#fontstretch)

**Type:** `string`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

A [font stretch](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-stretch):

```js
stretch: "condensed"
```

### font.featureSettings

[Section titled “font.featureSettings”](#fontfeaturesettings)

**Type:** `string`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

Controls the [typographic font features](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-feature-settings) (e.g. ligatures, small caps, or swashes):

```js
featureSettings: "'smcp' 2"
```

### font.variationSettings

[Section titled “font.variationSettings”](#fontvariationsettings)

**Type:** `string`\
**Default:** `undefined`

**Added in:** `astro@6.0.0`

Font [variation settings](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-variation-settings):

```js
variationSettings: "'xhgt' 0.7"
```

# Astro Container API (experimental)

---

# Template directives reference

**Template directives** are a special kind of HTML attribute available inside of any Astro component template (`.astro` files), and some can also be used in `.mdx` files.

Template directives are used to control an element or component’s behavior in some way. A template directive could enable some compiler feature that makes your life easier (like using `class:list` instead of `class`). Or, a directive could tell the Astro compiler to do something special with that component (like hydrating with `client:load`).

This page describes all of the template directives available to you in Astro, and how they work.

## Rules

[Section titled “Rules”](#rules)

For a template directive to be valid, it must:

* Include a colon `:` in its name, using the form `X:Y` (ex: `client:load`).
* Be visible to the compiler (ex: `<X {...attr}>` would not work if `attr` contained a directive).

Some template directives, but not all, can take a custom value:

* `<X client:load />` (takes no value)
* `<X class:list={['some-css-class']} />` (takes an array)

A template directive is never included directly in the final HTML output of a component.

## Common Directives

[Section titled “Common Directives”](#common-directives)

### `class:list`

[Section titled “class:list”](#classlist)

`class:list={...}` takes an array of class values and converts them into a class string. This is powered by @lukeed’s popular [clsx](https://github.com/lukeed/clsx) helper library.

`class:list` takes an array of several different possible value kinds:

* `string`: Added to the element `class`
* `Object`: All truthy keys are added to the element `class`
* `Array`: flattened
* `false`, `null`, or `undefined`: skipped

```astro
<!-- This -->
<span class:list={[ 'hello goodbye', { world: true }, [ 'friend' ] ]} />
<!-- Becomes -->
<span class="hello goodbye world friend"></span>
```

### `set:html`

[Section titled “set:html”](#sethtml)

`set:html={string}` injects an HTML string into an element, similar to setting `el.innerHTML`.

**The value is not automatically escaped by Astro!** Be sure that you trust the value, or that you have escaped it manually before passing it to the template. Forgetting to do this will open you up to [Cross Site Scripting (XSS) attacks.](https://owasp.org/www-community/attacks/xss/)

```astro
---
const rawHTMLString = "Hello <strong>World</strong>"
---
<h1>{rawHTMLString}</h1>
  <!-- Output: <h1>Hello &lt;strong&gt;World&lt;/strong&gt;</h1> -->
<h1 set:html={rawHTMLString} />
  <!-- Output: <h1>Hello <strong>World</strong></h1> -->
```

You can also use `set:html` on a `<Fragment>` to avoid adding an unnecessary wrapper element. This can be especially useful when fetching HTML from a CMS.

```astro
---
const cmsContent = await fetchHTMLFromMyCMS();
---
<Fragment set:html={cmsContent}>
```

`set:html={Promise<string>}` injects an HTML string into an element that is wrapped in a Promise.

This can be used to inject HTML stored externally, such as in a database.

```astro
---
import api from '../db/api.js';
---
<article set:html={api.getArticle(Astro.props.id)}></article>
```

`set:html={Promise<Response>}` injects a [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) into an element.

This is most helpful when using `fetch()`. For example, fetching old posts from a previous static-site generator.

```astro
<article set:html={fetch('http://example/old-posts/making-soup.html')}></article>
```

`set:html` can be used on any tag and does not have to include HTML. For example, use with [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) on a `<script>` tag to add a [JSON-LD](https://json-ld.org/) schema to your page.

```astro
<script type="application/ld+json" set:html={JSON.stringify({
  "@context": "https://schema.org/",
  "@type": "Person",
  name: "Houston",
  hasOccupation: {
    "@type": "Occupation",
    name: "Astronaut"
  }
})}/>
```

### `set:text`

[Section titled “set:text”](#settext)

`set:text={string}` injects a text string into an element, similar to setting `el.innerText`. Unlike `set:html`, the `string` value that is passed is automatically escaped by Astro.

This is equivalent to just passing a variable into a template expression directly (ex: `<div>{someText}</div>`) and therefore this directive is not commonly used.

## Client Directives

[Section titled “Client Directives”](#client-directives)

These directives control how [UI Framework components](/en/guides/framework-components/) are hydrated on the page.

By default, a UI Framework component is not hydrated in the client. If no `client:*` directive is provided, its HTML is rendered onto the page without JavaScript.

A client directive can only be used on a UI framework component that is directly imported into a `.astro` component. Hydration directives are not supported when using [dynamic tags](/en/reference/astro-syntax/#dynamic-tags) and [custom components passed via the `components` prop](/en/guides/integrations-guide/mdx/#passing-components-to-mdx-content).

### `client:load`

[Section titled “client:load”](#clientload)

* **Priority:** High
* **Useful for:** Immediately-visible UI elements that need to be interactive as soon as possible.

Load and hydrate the component JavaScript immediately on page load.

```astro
<BuyButton client:load />
```

### `client:idle`

[Section titled “client:idle”](#clientidle)

* **Priority:** Medium
* **Useful for:** Lower-priority UI elements that don’t need to be immediately interactive.

Load and hydrate the component JavaScript once the page is done with its initial load and the `requestIdleCallback` event has fired. If you are in a browser that doesn’t support [`requestIdleCallback`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback), then the document [`load`](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event) event is used.

```astro
<ShowHideButton client:idle />
```

#### `timeout`

[Section titled “timeout”](#timeout)

**Added in:** `astro@4.15.0`

The maximum time to wait, in milliseconds, before hydrating the component, even if the page is not yet done with its initial load.

This allows you to pass a value for [the `timeout` option from the `requestIdleCallback()` specification](https://www.w3.org/TR/requestidlecallback/#the-requestidlecallback-method). This means you can delay hydration for lower-priority UI elements with more control to ensure your element is interactive within a specified time frame.

```astro
<ShowHideButton client:idle={{timeout: 500}} />
```

### `client:visible`

[Section titled “client:visible”](#clientvisible)

* **Priority:** Low
* **Useful for:** Low-priority UI elements that are either far down the page (“below the fold”) or so resource-intensive to load that you would prefer not to load them at all if the user never saw the element.

Load and hydrate the component JavaScript once the component has entered the user’s viewport. This uses an `IntersectionObserver` internally to keep track of visibility.

```astro
<HeavyImageCarousel client:visible />
```

#### `client:visible={{rootMargin}}`

[Section titled “client:visible={{rootMargin}}”](#clientvisiblerootmargin)

**Added in:** `astro@4.1.0`

Optionally, a value for `rootMargin` can be passed to the underlying `IntersectionObserver`. When `rootMargin` is specified, the component JavaScript will hydrate when a specified margin (in pixels) around the component enters the viewport, rather than the component itself.

```astro
<HeavyImageCarousel client:visible={{rootMargin: "200px"}} />
```

Specifying a `rootMargin` value can reduce layout shifts (CLS), allow more time for a component to hydrate on slower internet connections, and make components interactive sooner, enhancing the stability and responsiveness of the page.

### `client:media`

[Section titled “client:media”](#clientmedia)

* **Priority:** Low
* **Useful for:** Sidebar toggles, or other elements that might only be visible on certain screen sizes.

`client:media={string}` loads and hydrates the component JavaScript once a certain CSS media query is met.

Note

If the component is already hidden and shown by a media query in your CSS, then it can be easier to just use `client:visible` and not pass that same media query into the directive.

```astro
<SidebarToggle client:media="(max-width: 50em)" />
```

### `client:only`

[Section titled “client:only”](#clientonly)

`client:only={string}` **skips** HTML server rendering, and renders only on the client. It acts similarly to `client:load` in that it loads, renders, and hydrates the component immediately on page load.

**You must pass the component’s correct framework as a value!** Because Astro doesn’t run the component during your build / on the server, Astro doesn’t know what framework your component uses unless you tell it explicitly.

```astro
<SomeReactComponent client:only="react" />
<SomePreactComponent client:only="preact" />
<SomeSvelteComponent client:only="svelte" />
<SomeVueComponent client:only="vue" />
<SomeSolidComponent client:only="solid-js" />
```

#### Display loading content

[Section titled “Display loading content”](#display-loading-content)

For components that render only on the client, it is also possible to display fallback content while they are loading. Use `slot="fallback"` on any child element to create content that will be displayed only until your client component is available:

```astro
<ClientComponent client:only="vue">
  <div slot="fallback">Loading</div>
</ClientComponent>
```

### Custom Client Directives

[Section titled “Custom Client Directives”](#custom-client-directives)

Since Astro 2.6.0, integrations can also add custom `client:*` directives to change how and when components should be hydrated.

Visit the [`addClientDirective` API](/en/reference/integrations-reference/#addclientdirective-option) page to learn more about creating a custom client directive.

## Server Directives

[Section titled “Server Directives”](#server-directives)

These directives control how server island components are rendered.

### `server:defer`

[Section titled “server:defer”](#serverdefer)

The `server:defer` directive transforms the component into a server island, causing it to be rendered on demand, outside the scope of the rest of the page rendering.

See more about using [server island components](/en/guides/server-islands/).

```astro
<Avatar server:defer />
```

## Script & Style Directives

[Section titled “Script & Style Directives”](#script--style-directives)

These directives can only be used on HTML `<script>` and `<style>` tags, to control how your client-side JavaScript and CSS are handled on the page.

### `is:global`

[Section titled “is:global”](#isglobal)

By default, Astro automatically scopes `<style>` CSS rules to the component. You can opt-out of this behavior with the `is:global` directive.

`is:global` makes the contents of a `<style>` tag apply globally on the page when the component is included. This disables Astro’s CSS scoping system. This is equivalent to wrapping all of the selectors within a `<style>` tag with `:global()`.

You can combine `<style>` and `<style is:global>` together in the same component, to create some global style rules while still scoping most of your component CSS.

See the [Styling & CSS](/en/guides/styling/#global-styles) page for more details about how global styles work.

```astro
<style is:global>
  body a { color: red; }
</style>
```

### `is:inline`

[Section titled “is:inline”](#isinline)

By default, Astro will process, optimize, and bundle any `<script>` and `<style>` tags that it sees on the page. You can opt-out of this behavior with the `is:inline` directive.

`is:inline` tells Astro to leave the `<script>` or `<style>` tag as-is in the final output HTML. The contents will not be processed, optimized, or bundled. This limits some Astro features, like importing an npm package or using a compile-to-CSS language like Sass.

The `is:inline` directive means that `<style>` and `<script>` tags:

* Will not be bundled into an external file. This means that [attributes like `defer`](https://javascript.info/script-async-defer) which control the loading of an external file will have no effect.
* Will not be deduplicated—the element will appear as many times as it is rendered.
* Will not have its `import`/`@import`/`url()` references resolved relative to the `.astro` file.
* Will be rendered in the final output HTML exactly where it is authored.
* Styles will be global and not scoped to the component.

Caution

The `is:inline` directive is implied whenever any attribute other than `src` is used on a `<script>` or `<style>` tag. The one exception is using the [`define:vars` directive](/en/reference/directives-reference/#definevars) on the `<style>` tag, which does not automatically imply `is:inline`.

```astro
<style is:inline>
  /* inline: relative & npm package imports are not supported. */
  @import '/assets/some-public-styles.css';
  span { color: green; }
</style>


<script is:inline>
  /* inline: relative & npm package imports are not supported. */
  console.log('I am inlined right here in the final output HTML.');
</script>
```

See how [client-side scripts](/en/guides/client-side-scripts/) work in Astro components.

### `define:vars`

[Section titled “define:vars”](#definevars)

`define:vars={...}` can pass server-side variables from your component frontmatter into the client `<script>` or `<style>` tags. Any JSON-serializable frontmatter variable is supported, including `props` passed to your component through `Astro.props`. Values are serialized with [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).

```astro
---
const foregroundColor = "rgb(221 243 228)";
const backgroundColor = "rgb(24 121 78)";
const message = "Astro is awesome!";
---
<style define:vars={{ textColor: foregroundColor, backgroundColor }}>
  h1 {
    background-color: var(--backgroundColor);
    color: var(--textColor);
  }
</style>


<script define:vars={{ message }}>
  alert(message);
</script>
```

Caution

Using `define:vars` on a `<script>` tag implies the [`is:inline` directive](#isinline), which means your scripts won’t be bundled and will be inlined directly into the HTML.

This is because when Astro bundles a script, it includes and runs the script once even if you include the component containing the script multiple times on one page. `define:vars` requires a script to rerun with each set of values, so Astro creates an inline script instead.

For scripts, try [passing variables to scripts manually](/en/guides/client-side-scripts/#pass-frontmatter-variables-to-scripts) instead.

## Advanced Directives

[Section titled “Advanced Directives”](#advanced-directives)

### `is:raw`

[Section titled “is:raw”](#israw)

`is:raw` instructs the Astro compiler to treat any children of that element as text. This means that all special Astro templating syntax will be ignored inside of this component.

For example, if you had a custom Katex component that converted some text to HTML, you could have users do this:

```astro
---
import Katex from '../components/Katex.astro';
---
<Katex is:raw>Some conflicting {syntax} here</Katex>
```


---

# Error reference

The following reference is a complete list of the errors you may encounter while using Astro. For additional assistance, including common pitfalls, please also see our [Troubleshooting Guide](/en/guides/troubleshooting/).

## Astro Errors

[Section titled “Astro Errors”](#astro-errors)

* [**UnknownCompilerError**](/en/reference/errors/unknown-compiler-error/)\
  Unknown compiler error.
* [**ClientAddressNotAvailable**](/en/reference/errors/client-address-not-available/)\
  `Astro.clientAddress` is not available in current adapter.
* [**PrerenderClientAddressNotAvailable**](/en/reference/errors/prerender-client-address-not-available/)\
  `Astro.clientAddress` cannot be used inside prerendered routes.
* [**StaticClientAddressNotAvailable**](/en/reference/errors/static-client-address-not-available/)\
  `Astro.clientAddress` is not available in prerendered pages.
* [**NoMatchingStaticPathFound**](/en/reference/errors/no-matching-static-path-found/)\
  No static path found for requested path.
* [**OnlyResponseCanBeReturned**](/en/reference/errors/only-response-can-be-returned/)\
  Invalid type returned by Astro page.
* [**MissingMediaQueryDirective**](/en/reference/errors/missing-media-query-directive/)\
  Missing value for `client:media` directive.
* [**NoMatchingRenderer**](/en/reference/errors/no-matching-renderer/)\
  No matching renderer found.
* [**NoClientEntrypoint**](/en/reference/errors/no-client-entrypoint/)\
  No client entrypoint specified in renderer.
* [**NoClientOnlyHint**](/en/reference/errors/no-client-only-hint/)\
  Missing hint on `client:only` directive.
* [**InvalidGetStaticPathParam**](/en/reference/errors/invalid-get-static-path-param/)\
  Invalid value returned by a `getStaticPaths` path.
* [**InvalidGetStaticPathsEntry**](/en/reference/errors/invalid-get-static-paths-entry/)\
  Invalid entry inside getStaticPath’s return value
* [**InvalidGetStaticPathsReturn**](/en/reference/errors/invalid-get-static-paths-return/)\
  Invalid value returned by getStaticPaths.
* [**GetStaticPathsExpectedParams**](/en/reference/errors/get-static-paths-expected-params/)\
  Missing params property on `getStaticPaths` route.
* [**GetStaticPathsInvalidRouteParam**](/en/reference/errors/get-static-paths-invalid-route-param/)\
  Invalid route parameter returned by `getStaticPaths()`.
* [**GetStaticPathsRequired**](/en/reference/errors/get-static-paths-required/)\
  `getStaticPaths()` function required for dynamic routes.
* [**ReservedSlotName**](/en/reference/errors/reserved-slot-name/)\
  Invalid slot name.
* [**NoAdapterInstalled**](/en/reference/errors/no-adapter-installed/)\
  Cannot use Server-side Rendering without an adapter.
* [**AdapterSupportOutputMismatch**](/en/reference/errors/adapter-support-output-mismatch/)\
  Adapter does not support server output.
* [**NoAdapterInstalledServerIslands**](/en/reference/errors/no-adapter-installed-server-islands/)\
  Cannot use Server Islands without an adapter.
* [**NoMatchingImport**](/en/reference/errors/no-matching-import/)\
  No import found for component.
* [**InvalidPrerenderExport**](/en/reference/errors/invalid-prerender-export/)\
  Invalid prerender export.
* [**InvalidComponentArgs**](/en/reference/errors/invalid-component-args/)\
  Invalid component arguments.
* [**PageNumberParamNotFound**](/en/reference/errors/page-number-param-not-found/)\
  Page number param not found.
* [**ImageMissingAlt**](/en/reference/errors/image-missing-alt/)\
  Image missing required “alt” property.
* [**InvalidImageService**](/en/reference/errors/invalid-image-service/)\
  Error while loading image service.
* [**MissingImageDimension**](/en/reference/errors/missing-image-dimension/)\
  Missing image dimensions
* [**FailedToFetchRemoteImageDimensions**](/en/reference/errors/failed-to-fetch-remote-image-dimensions/)\
  Failed to retrieve remote image dimensions
* [**RemoteImageNotAllowed**](/en/reference/errors/remote-image-not-allowed/)\
  Remote image is not allowed
* [**UnsupportedImageFormat**](/en/reference/errors/unsupported-image-format/)\
  Unsupported image format
* [**UnsupportedImageConversion**](/en/reference/errors/unsupported-image-conversion/)\
  Unsupported image conversion
* [**CannotOptimizeSvg**](/en/reference/errors/cannot-optimize-svg/)\
  Cannot optimize SVG
* [**PrerenderDynamicEndpointPathCollide**](/en/reference/errors/prerender-dynamic-endpoint-path-collide/)\
  Prerendered dynamic endpoint has path collision.
* [**PrerenderRouteConflict**](/en/reference/errors/prerender-route-conflict/)\
  Prerendered route generates the same path as another route.
* [**ExpectedImage**](/en/reference/errors/expected-image/)\
  Expected src to be an image.
* [**ExpectedImageOptions**](/en/reference/errors/expected-image-options/)\
  Expected image options.
* [**ExpectedNotESMImage**](/en/reference/errors/expected-not-esmimage/)\
  Expected image options, not an ESM-imported image.
* [**GetImageNotUsedOnServer**](/en/reference/errors/get-image-not-used-on-server/)\
  `getImage()` must be used on the server.
* [**IncompatibleDescriptorOptions**](/en/reference/errors/incompatible-descriptor-options/)\
  Cannot set both `densities` and `widths`
* [**ImageNotFound**](/en/reference/errors/image-not-found/)\
  Image not found.
* [**NoImageMetadata**](/en/reference/errors/no-image-metadata/)\
  Could not process image metadata.
* [**CouldNotTransformImage**](/en/reference/errors/could-not-transform-image/)\
  Could not transform image.
* [**ResponseSentError**](/en/reference/errors/response-sent-error/)\
  Unable to set response.
* [**MiddlewareNoDataOrNextCalled**](/en/reference/errors/middleware-no-data-or-next-called/)\
  The middleware didn’t return a `Response`.
* [**MiddlewareNotAResponse**](/en/reference/errors/middleware-not-aresponse/)\
  The middleware returned something that is not a `Response` object.
* [**EndpointDidNotReturnAResponse**](/en/reference/errors/endpoint-did-not-return-aresponse/)\
  The endpoint did not return a `Response`.
* [**LocalsNotAnObject**](/en/reference/errors/locals-not-an-object/)\
  Value assigned to `locals` is not accepted.
* [**LocalsReassigned**](/en/reference/errors/locals-reassigned/)\
  `locals` must not be reassigned.
* [**AstroResponseHeadersReassigned**](/en/reference/errors/astro-response-headers-reassigned/)\
  `Astro.response.headers` must not be reassigned.
* [**MiddlewareCantBeLoaded**](/en/reference/errors/middleware-cant-be-loaded/)\
  Can’t load the middleware.
* [**LocalImageUsedWrongly**](/en/reference/errors/local-image-used-wrongly/)\
  Local images must be imported.
* [**AstroGlobUsedOutside**](/en/reference/errors/astro-glob-used-outside/)\
  Astro.glob() used outside of an Astro file.
* [**AstroGlobNoMatch**](/en/reference/errors/astro-glob-no-match/)\
  Astro.glob() did not match any files.
* [**RedirectWithNoLocation**](/en/reference/errors/redirect-with-no-location/)\
  A redirect must be given a location with the `Location` header.
* [**UnsupportedExternalRedirect**](/en/reference/errors/unsupported-external-redirect/)\
  Unsupported or malformed URL.
* [**InvalidRedirectDestination**](/en/reference/errors/invalid-redirect-destination/)\
  Invalid redirect destination.
* [**InvalidDynamicRoute**](/en/reference/errors/invalid-dynamic-route/)\
  Invalid dynamic route.
* [**MissingSharp**](/en/reference/errors/missing-sharp/)\
  Could not find Sharp.
* [**UnknownViteError**](/en/reference/errors/unknown-vite-error/)\
  Unknown Vite Error.
* [**FailedToLoadModuleSSR**](/en/reference/errors/failed-to-load-module-ssr/)\
  Could not import file.
* [**InvalidGlob**](/en/reference/errors/invalid-glob/)\
  Invalid glob pattern.
* [**FailedToFindPageMapSSR**](/en/reference/errors/failed-to-find-page-map-ssr/)\
  Astro couldn’t find the correct page to render
* [**MissingLocale**](/en/reference/errors/missing-locale/)\
  The provided locale does not exist.
* [**MissingIndexForInternationalization**](/en/reference/errors/missing-index-for-internationalization/)\
  Index page not found.
* [**IncorrectStrategyForI18n**](/en/reference/errors/incorrect-strategy-for-i18n/)\
  You can’t use the current function with the current strategy
* [**NoPrerenderedRoutesWithDomains**](/en/reference/errors/no-prerendered-routes-with-domains/)\
  Prerendered routes aren’t supported when internationalization domains are enabled.
* [**MissingMiddlewareForInternationalization**](/en/reference/errors/missing-middleware-for-internationalization/)\
  Enabled manual internationalization routing without having a middleware.
* [**InvalidI18nMiddlewareConfiguration**](/en/reference/errors/invalid-i18n-middleware-configuration/)\
  Invalid internationalization middleware configuration
* [**CantRenderPage**](/en/reference/errors/cant-render-page/)\
  Astro can’t render the route.
* [**UnhandledRejection**](/en/reference/errors/unhandled-rejection/)\
  Unhandled rejection
* [**i18nNotEnabled**](/en/reference/errors/i18n-not-enabled/)\
  i18n Not Enabled
* [**i18nNoLocaleFoundInPath**](/en/reference/errors/i18n-no-locale-found-in-path/)\
  The path doesn’t contain any locale
* [**RouteNotFound**](/en/reference/errors/route-not-found/)\
  Route not found.
* [**EnvInvalidVariables**](/en/reference/errors/env-invalid-variables/)\
  Invalid Environment Variables
* [**EnvPrefixConflictsWithSecret**](/en/reference/errors/env-prefix-conflicts-with-secret/)\
  envPrefix conflicts with secret environment variables
* [**ServerOnlyModule**](/en/reference/errors/server-only-module/)\
  Module is only available server-side
* [**RewriteWithBodyUsed**](/en/reference/errors/rewrite-with-body-used/)\
  Cannot use Astro.rewrite after the request body has been read
* [**ForbiddenRewrite**](/en/reference/errors/forbidden-rewrite/)\
  Forbidden rewrite to a static route.
* [**UnknownFilesystemError**](/en/reference/errors/unknown-filesystem-error/)\
  An unknown error occurred while reading or writing files to disk.
* [**CannotExtractFontType**](/en/reference/errors/cannot-extract-font-type/)\
  Cannot extract the font type from the given URL.
* [**CannotDetermineWeightAndStyleFromFontFile**](/en/reference/errors/cannot-determine-weight-and-style-from-font-file/)\
  Cannot determine weight and style from font file.
* [**CannotFetchFontFile**](/en/reference/errors/cannot-fetch-font-file/)\
  Cannot fetch the given font file.
* [**FontFamilyNotFound**](/en/reference/errors/font-family-not-found/)\
  Font family not found
* [**UnavailableAstroGlobal**](/en/reference/errors/unavailable-astro-global/)\
  Unavailable Astro global in getStaticPaths()

## CSS Errors

[Section titled “CSS Errors”](#css-errors)

* [**UnknownCSSError**](/en/reference/errors/unknown-csserror/)\
  Unknown CSS Error.
* [**CSSSyntaxError**](/en/reference/errors/csssyntax-error/)\
  CSS Syntax Error.

## Markdown Errors

[Section titled “Markdown Errors”](#markdown-errors)

* [**UnknownMarkdownError**](/en/reference/errors/unknown-markdown-error/)\
  Unknown Markdown Error.
* [**MarkdownFrontmatterParseError**](/en/reference/errors/markdown-frontmatter-parse-error/)\
  Failed to parse Markdown frontmatter.
* [**InvalidFrontmatterInjectionError**](/en/reference/errors/invalid-frontmatter-injection-error/)\
  Invalid frontmatter injection.
* [**MdxIntegrationMissingError**](/en/reference/errors/mdx-integration-missing-error/)\
  MDX integration missing.
* [**UnknownConfigError**](/en/reference/errors/unknown-config-error/)\
  Unknown configuration error.
* [**ConfigNotFound**](/en/reference/errors/config-not-found/)\
  Specified configuration file not found.
* [**ConfigLegacyKey**](/en/reference/errors/config-legacy-key/)\
  Legacy configuration detected.

## CLI Errors

[Section titled “CLI Errors”](#cli-errors)

* [**UnknownCLIError**](/en/reference/errors/unknown-clierror/)\
  Unknown CLI Error.
* [**GenerateContentTypesError**](/en/reference/errors/generate-content-types-error/)\
  Failed to generate content types.

## Content Collection Errors

[Section titled “Content Collection Errors”](#content-collection-errors)

* [**UnknownContentCollectionError**](/en/reference/errors/unknown-content-collection-error/)\
  Unknown Content Collection Error.
* [**RenderUndefinedEntryError**](/en/reference/errors/render-undefined-entry-error/)\
  Attempted to render an undefined content collection entry.
* [**GetEntryDeprecationError**](/en/reference/errors/get-entry-deprecation-error/)\
  Invalid use of `getDataEntryById` or `getEntryBySlug` function.
* [**InvalidContentEntryFrontmatterError**](/en/reference/errors/invalid-content-entry-frontmatter-error/)\
  Content entry frontmatter does not match schema.
* [**InvalidContentEntryDataError**](/en/reference/errors/invalid-content-entry-data-error/)\
  Content entry data does not match schema.
* [**LegacyContentConfigError**](/en/reference/errors/legacy-content-config-error/)\
  Legacy content config file found.
* [**ContentCollectionMissingLoader**](/en/reference/errors/content-collection-missing-loader/)\
  Content collection is missing a `loader` definition.
* [**ContentCollectionInvalidType**](/en/reference/errors/content-collection-invalid-type/)\
  Content collection has an invalid `type` field.
* [**ContentLoaderReturnsInvalidId**](/en/reference/errors/content-loader-returns-invalid-id/)\
  Content loader returned an entry with an invalid `id`.
* [**ContentEntryDataError**](/en/reference/errors/content-entry-data-error/)\
  Content entry data does not match schema.
* [**LiveContentConfigError**](/en/reference/errors/live-content-config-error/)\
  Error in live content config.
* [**ContentLoaderInvalidDataError**](/en/reference/errors/content-loader-invalid-data-error/)\
  Content entry is missing an ID
* [**InvalidContentEntrySlugError**](/en/reference/errors/invalid-content-entry-slug-error/)\
  Invalid content entry slug.
* [**ContentSchemaContainsSlugError**](/en/reference/errors/content-schema-contains-slug-error/)\
  Content Schema should not contain `slug`.
* [**MixedContentDataCollectionError**](/en/reference/errors/mixed-content-data-collection-error/)\
  Content and data cannot be in same collection.
* [**ContentCollectionTypeMismatchError**](/en/reference/errors/content-collection-type-mismatch-error/)\
  Collection contains entries of a different type.
* [**DataCollectionEntryParseError**](/en/reference/errors/data-collection-entry-parse-error/)\
  Data collection entry failed to parse.
* [**DuplicateContentEntrySlugError**](/en/reference/errors/duplicate-content-entry-slug-error/)\
  Duplicate content entry slug.
* [**UnsupportedConfigTransformError**](/en/reference/errors/unsupported-config-transform-error/)\
  Unsupported transform in content config.
* [**FileParserNotFound**](/en/reference/errors/file-parser-not-found/)\
  File parser not found
* [**FileGlobNotSupported**](/en/reference/errors/file-glob-not-supported/)\
  Glob patterns are not supported in the file loader

## Action Errors

[Section titled “Action Errors”](#action-errors)

* [**ActionsWithoutServerOutputError**](/en/reference/errors/actions-without-server-output-error/)\
  Actions must be used with server output.
* [**ActionsReturnedInvalidDataError**](/en/reference/errors/actions-returned-invalid-data-error/)\
  Action handler returned invalid data.
* [**ActionNotFoundError**](/en/reference/errors/action-not-found-error/)\
  Action not found.
* [**ActionCalledFromServerError**](/en/reference/errors/action-called-from-server-error/)\
  Action unexpected called from the server.
* [**ActionsCantBeLoaded**](/en/reference/errors/actions-cant-be-loaded/)\
  Can’t load the Astro actions.

## Session Errors

[Section titled “Session Errors”](#session-errors)

* [**SessionStorageInitError**](/en/reference/errors/session-storage-init-error/)\
  Session storage could not be initialized.
* [**SessionStorageSaveError**](/en/reference/errors/session-storage-save-error/)\
  Session data could not be saved.

## Cache Errors

[Section titled “Cache Errors”](#cache-errors)

* [**CacheProviderNotFound**](/en/reference/errors/cache-provider-not-found/)\
  Cache provider not found.
* [**CacheNotEnabled**](/en/reference/errors/cache-not-enabled/)\
  Cache is not enabled.
* [**CacheQueryConfigConflict**](/en/reference/errors/cache-query-config-conflict/)\
  Conflicting cache query configuration.

# Action unexpected called from the server.

> **ActionCalledFromServerError**: Action called from a server page or endpoint without using `Astro.callAction()`. This wrapper must be used to call actions from server code.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Action called from a server page or endpoint without using `Astro.callAction()`.

**See Also:**

* [`Astro.callAction()` reference](/en/reference/api-reference/#callaction)

# Action not found.

> **ActionNotFoundError**: The server received a request for an action named `ACTION_NAME` but could not find a match. If you renamed an action, check that you’ve updated your `actions/index` file and your calling code to match.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The server received a request for an action but could not find a match with the same name.

# An invalid Action query string was passed by a form.

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **ActionQueryStringInvalidError**: The server received the query string `?_astroAction=ACTION_NAME`, but could not find an action with that name. If you changed an action’s name in development, remove this query param from your URL and refresh.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The server received the query string `?_astroAction=name`, but could not find an action with that name. Use the action function’s `.queryString` property to retrieve the form `action` URL.

**See Also:**

* [Actions RFC](https://github.com/withastro/roadmap/blob/actions/proposals/0046-actions.md)

# Can't load the Astro actions.

> **ActionsCantBeLoaded**: An unknown error was thrown while loading the Astro actions file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown in development mode when the actions file can’t be loaded.

# Action handler returned invalid data.

> **ActionsReturnedInvalidDataError**: Action handler returned invalid data. Handlers should return serializable data types like objects, arrays, strings, and numbers. Parse error: ERROR

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Action handler returned invalid data. Handlers should return serializable data types, and cannot return a Response object.

**See Also:**

* [Actions handler reference](/en/reference/modules/astro-actions/#handler-property)

# An invalid Action query string was passed by a form.

Deprecated

Deprecated since version 4.13.2.

> **ActionsUsedWithForGetError**: Action ACTION\_NAME was called from a form using a GET request, but only POST requests are supported. This often occurs if `method="POST"` is missing on the form.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Action was called from a form using a GET request, but only POST requests are supported. This often occurs if `method="POST"` is missing on the form.

**See Also:**

* [Actions RFC](https://github.com/withastro/roadmap/blob/actions/proposals/0046-actions.md)

# Actions must be used with server output.

> **ActionsWithoutServerOutputError**: A server is required to create callable backend functions. To deploy routes to a server, add an adapter to your Astro config and configure your route for on-demand rendering

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Your project must have a server output to create backend functions with Actions.

**See Also:**

* [On-demand rendering](/en/guides/on-demand-rendering/)

# Adapter does not support server output.

> **AdapterSupportOutputMismatch**: The `ADAPTER_NAME` adapter is configured to output a static website, but the project contains server-rendered pages. Please install and configure the appropriate server adapter for your final deployment.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The currently configured adapter does not support server-side rendering, which is required for the current project setup.

Depending on your adapter, there may be a different entrypoint to use for server-side rendering. For example, the `@astrojs/vercel` adapter has a `@astrojs/vercel/static` entrypoint for static rendering, and a `@astrojs/vercel/serverless` entrypoint for server-side rendering.

**See Also:**

* [Server-side Rendering](/en/guides/on-demand-rendering/)

# Astro.glob() did not match any files.

Deprecated

This error was removed in Astro v6.0.0 along with the removal of `Astro.glob()`.

> **AstroGlobNoMatch**: `Astro.glob(GLOB_STR)` did not return any matching files.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`Astro.glob()` did not return any matching files. There might be a typo in the glob pattern.

# Astro.glob() used outside of an Astro file.

Deprecated

This error was removed in Astro v6.0.0 along with the removal of `Astro.glob()`.

> **AstroGlobUsedOutside**: `Astro.glob(GLOB_STR)` can only be used in `.astro` files. `import.meta.glob(GLOB_STR)` can be used instead to achieve a similar result.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`Astro.glob()` can only be used in `.astro` files. You can use [`import.meta.glob()`](https://vite.dev/guide/features.html#glob-import) instead to achieve the same result.

# Astro.response.headers must not be reassigned.

> **AstroResponseHeadersReassigned**: Individual headers can be added to and removed from `Astro.response.headers`, but it must not be replaced with another instance of `Headers` altogether.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when a value is being set as the `headers` field on the `ResponseInit` object available as `Astro.response`.

# Cache is not enabled.

> `Astro.cache` is not available because the cache feature is not enabled.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when `Astro.cache` or `context.cache` is used but the cache feature has not been enabled in the Astro config.

# Cache provider not found.

> Could not resolve the cache provider `PROVIDER`. Make sure the package is installed.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when the configured cache provider cannot be resolved. This usually means the package is not installed or the import path is wrong.

# Conflicting cache query configuration.

> `query.include` and `query.exclude` cannot be used together.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The memory cache provider’s `query.include` and `query.exclude` options are mutually exclusive. Use `include` to allowlist specific query parameters that affect the cache key, or `exclude` to blocklist parameters. When `include` is set, all other parameters are automatically ignored.

# Cannot determine weight and style from font file.

> An error occurred while determining the weight and style from the local font file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Cannot determine weight and style from font file, update your family config and set `weight` and `style` manually instead.

# Cannot extract the font type from the given URL.

> An error occurred while trying to extract the font type from the given URL.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Cannot extract the font type from the given URL.

# Cannot fetch the given font file.

> An error occurred while fetching font file from the given URL.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Cannot fetch the given font file

# Cannot optimize SVG

> An error occurred while optimizing the SVG file with SVGO.

# Astro can't render the route.

> **CantRenderPage**: Astro cannot find any content to render for this route. There is no file or redirect associated with this route.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not find an associated file with content while trying to render the route. This is an Astro error and not a user error. If restarting the dev server does not fix the problem, please file an issue.

# Cannot use the astro:config module without enabling the experimental feature.

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **CantUseAstroConfigModuleError**: Cannot import the module “MODULE\_NAME” because the experimental feature is disabled. Enable `experimental.serializeConfig` in your `astro.config.mjs`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Cannot use the module `astro:config` without enabling the experimental feature.

# Astro.clientAddress is not available in current adapter.

> **ClientAddressNotAvailable**: `Astro.clientAddress` is not available in the `ADAPTER_NAME` adapter. File an issue with the adapter to add support.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The adapter you’re using unfortunately does not support `Astro.clientAddress`.

**See Also:**

* [Official integrations](/en/guides/integrations/#official-integrations)
* [Astro.clientAddress](/en/reference/api-reference/#clientaddress)

# Collection does not exist

Deprecated

Collections that do not exist no longer result in an error. A warning is given instead.

> A collection queried via `getCollection()` does not exist.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

When querying a collection, ensure a collection directory with the requested name exists under `src/content/`.

# Legacy configuration detected.

> **ConfigLegacyKey**: Legacy configuration detected: `LEGACY_CONFIG_KEY`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro detected a legacy configuration option in your configuration file.

**See Also:**

* [Configuration reference](/en/reference/configuration-reference/)

# Specified configuration file not found.

> **ConfigNotFound**: Unable to resolve `--config "CONFIG_FILE"`. Does the file exist?

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The specified configuration file using `--config` could not be found. Make sure that it exists or that the path is correct

**See Also:**

* [--config](/en/reference/cli-reference/#--config-path)

# Content collection has an invalid type field.

> **Example error message:**\
> Invalid collection type “data”. Remove the type from your collection definition in your content config file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Content collections should no longer have a `type` field. Remove this field from your content config file. See the [Astro 6 migration guide](/en/guides/upgrade-to/v6/#removed-legacy-content-collections) for more information.

# Content collection is missing a loader definition.

> **Example error message:**\
> Collections must have a `loader` defined. Check your collection definitions in your content config file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A content collection is missing a `loader` definition. Make sure that each collection in your content config file has a `loader`. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Collection contains entries of a different type.

> **ContentCollectionTypeMismatchError**: COLLECTION contains EXPECTED\_TYPE entries, but is configured as a ACTUAL\_TYPE collection.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Legacy content collections must contain entries of the type configured. Collections are `type: 'content'` by default. Try adding `type: 'data'` to your collection config for data collections.

**See Also:**

* [Legacy content collections](/en/guides/upgrade-to/v5/#updating-existing-collections)

# Content entry data does not match schema.

> **Example error message:**\
> **blog** → **post** data does not match collection schema.\
> “title” is required.\
> “date” must be a valid date.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A content entry does not match its collection schema. Make sure that all required fields are present, and that all fields are of the correct type. You can check against the collection schema in your `src/content.config.*` file. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Content entry is missing an ID

> **Example error message:**\
> The loader for **blog** returned invalid data.\
> Object is missing required property “id”.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The loader for a content collection returned invalid data. Inline loaders must return an array of objects with unique ID fields or a plain object with IDs as keys and entries as values.

# Content loader returned an entry with an invalid id.

> **Example error message:**\
> The content loader for the collection **blog** returned an entry with an invalid `id`:\
> {\
> “id”: 1,\
> “title”: “Hello, World!”\
> }

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A content loader returned an invalid `id`. Make sure that the `id` of the entry is a string. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Content Schema should not contain slug.

> **ContentSchemaContainsSlugError**: A content collection schema should not contain `slug` since it is reserved for slug generation. Remove this from your COLLECTION\_NAME collection schema.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A legacy content collection schema should not contain the `slug` field. This is reserved by Astro for generating entry slugs. Remove `slug` from your schema. You can still use custom slugs in your frontmatter.

**See Also:**

* [Legacy content collections](/en/guides/upgrade-to/v5/#updating-existing-collections)

# Could not transform image.

> **CouldNotTransformImage**: Could not transform image `IMAGE_PATH`. See the stack trace for more information.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not transform one of your images. Often, this is caused by a corrupted or malformed image. Re-exporting the image from your image editor may fix this issue.

Depending on the image service you are using, the stack trace may contain more information on the specific error encountered.

**See Also:**

* [Images](/en/guides/images/)

# CSP feature isn't enabled

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> The `security.csp` configuration isn’t enabled.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The CSP feature isn’t enabled

# CSS Syntax Error.

> **Example error messages:**\
> CSSSyntaxError: Missed semicolon\
> CSSSyntaxError: Unclosed string

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an error while parsing your CSS, due to a syntax error. This is often caused by a missing semicolon.

# Data collection entry failed to parse.

> `COLLECTION_ENTRY_NAME` failed to parse.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Collection entries of `type: 'data'` must return an object with valid JSON (for `.json` entries), YAML (for `.yaml` entries) or TOML (for `.toml` entries).

# Duplicate content entry slug.

> `COLLECTION_NAME` contains multiple entries with the same slug: `SLUG`. Slugs must be unique.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Content collection entries must have unique slugs. Duplicates are often caused by the `slug` frontmatter property.

# The endpoint did not return a Response.

> **EndpointDidNotReturnAResponse**: An endpoint must return either a `Response`, or a `Promise` that resolves with a `Response`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when an endpoint does not return anything or returns an object that is not a `Response` object.

An endpoint must return either a `Response`, or a `Promise` that resolves with a `Response`. For example:

```ts
import type { APIContext } from 'astro';


export async function GET({ request, url, cookies }: APIContext): Promise<Response> {
    return Response.json({
        success: true,
        result: 'Data from Astro Endpoint!'
    })
}
```

# Invalid Environment Variable

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **EnvInvalidVariable**: The following environment variable does not match the data type and/or properties defined in `experimental.env.schema`: KEY is not of type TYPE

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An environment variable does not match the data type and/or properties defined in `experimental.env.schema`.

# Invalid Environment Variables

> The following environment variables defined in `env.schema` are invalid.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Some environment variables do not match the data type and/or properties defined in `env.schema`.

# envPrefix conflicts with secret environment variables

> **EnvPrefixConflictsWithSecret**: The following environment variables are declared with `access: "secret"` in `env.schema`, but their names match a prefix in `vite.envPrefix`, which would expose them in client-side bundles:\
> \
> CONFLICTS.MAP((C) =\ >`- ${C`).join('\
> ')}\
> \
> Either remove the conflicting prefixes from `vite.envPrefix`, or rename these variables to use a prefix not in `vite.envPrefix`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The configured `vite.envPrefix` includes prefixes that match environment variables declared with `access: "secret"` in `env.schema`. This would cause Vite to expose those secret values in client-side JavaScript bundles, bypassing the `access: "secret"` protection.

To fix this, either:

* Remove the conflicting prefixes from `vite.envPrefix`, or
* Rename your secret environment variables to use a prefix that is not in `vite.envPrefix`.

# Unsupported astro:env getSecret

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **EnvUnsupportedGetSecret**: `astro:env/server` exported function `getSecret` is not supported by your adapter.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `astro:env/server` exported function `getSecret()` is not supported by your adapter.

# Expected src to be an image.

> **ExpectedImage**: Expected `src` property for `getImage` or `<Image />` to be either an ESM imported image or a string with the path of a remote image. Received `SRC` (type: `TYPEOF_OPTIONS`).\
> \
> Full serialized options received: `FULL_OPTIONS`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An image’s `src` property is not valid. The Image component requires the `src` attribute to be either an image that has been ESM imported or a string. This is also true for the first parameter of `getImage()`.

```astro
---
import { Image } from "astro:assets";
import myImage from "../assets/my_image.png";
---


<Image src={myImage} alt="..." />
<Image src="https://example.com/logo.png" width={300} height={300} alt="..." />
```

In most cases, this error happens when the value passed to `src` is undefined.

**See Also:**

* [Images](/en/guides/images/)

# Expected image options.

> **ExpectedImageOptions**: Expected getImage() parameter to be an object. Received `OPTIONS`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`getImage()`’s first parameter should be an object with the different properties to apply to your image.

```ts
import { getImage } from "astro:assets";
import myImage from "../assets/my_image.png";


const optimizedImage = await getImage({src: myImage, width: 300, height: 300});
```

In most cases, this error happens because parameters were passed directly instead of inside an object.

**See Also:**

* [Images](/en/guides/images/)

# Expected image options, not an ESM-imported image.

> **ExpectedNotESMImage**: An ESM-imported image cannot be passed directly to `getImage()`. Instead, pass an object with the image in the `src` property.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An ESM-imported image cannot be passed directly to `getImage()`. Instead, pass an object with the image in the `src` property.

```diff
import { getImage } from "astro:assets";
import myImage from "../assets/my_image.png";
 const optimizedImage = await getImage( myImage );
 const optimizedImage = await getImage({ src: myImage });
```

**See Also:**

* [Images](/en/guides/images/)

# Failed to retrieve remote image dimensions

> Failed to get the dimensions for `IMAGE_URL`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Determining the remote image’s dimensions failed. This is typically caused by an incorrect URL or attempting to infer the size of an image in the public folder which is not possible.

# Astro couldn't find the correct page to render

> **FailedToFindPageMapSSR**: Astro couldn’t find the correct page to render, probably because it wasn’t correctly mapped for SSR usage. This is an internal error. Please file an issue.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro couldn’t find the correct page to render, probably because it wasn’t correctly mapped for SSR usage. This is an internal error.

# Could not import file.

> **FailedToLoadModuleSSR**: Could not import `IMPORT_NAME`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not import the requested file. Oftentimes, this is caused by the import path being wrong (either because the file does not exist, or there is a typo in the path)

This message can also appear when a type is imported without specifying that it is a [type import](/en/guides/typescript/#type-imports).

**See Also:**

* [Type Imports](/en/guides/typescript/#type-imports)

# Glob patterns are not supported in the file loader

> **FileGlobNotSupported**: Glob patterns are not supported in the `file` loader. Use the `glob` loader instead.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `file` loader must be passed a single local file. Glob patterns are not supported. Use the built-in `glob` loader to create entries from patterns of multiple local files.

**See Also:**

* [Astro’s built-in `file()` loader](/en/reference/content-loader-reference/#file-loader)

# File parser not found

> **FileParserNotFound**: No parser was found for ‘FILE\_NAME’. Pass a parser function (e.g. `parser: csv`) to the `file` loader.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `file` loader can’t determine which parser to use. Please provide a custom parser (e.g. `csv-parse`) to create a collection from your file type.

**See Also:**

* [Passing a `parser` to the `file` loader](/en/reference/content-loader-reference/#parser)

# Font family not found

> No data was found for the `cssVariable` passed to the `<Font />` component.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Font family not found

# Forbidden rewrite to a static route.

> **ForbiddenRewrite**: You tried to rewrite the on-demand route ‘FROM’ with the static route ‘TO’, when using the ‘server’ output.\
> \
> The static route ‘TO’ is rendered by the component ‘COMPONENT’, which is marked as prerendered. This is a forbidden operation because during the build, the component ‘COMPONENT’ is compiled to an HTML file, which can’t be retrieved at runtime by Astro.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`Astro.rewrite()` can’t be used to rewrite an on-demand route with a static route when using the `"server"` output.

# Failed to generate content types.

> **GenerateContentTypesError**: `astro sync` command failed to generate content collection types: ERROR\_MESSAGE

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`astro sync` command failed to generate content collection types.

**See Also:**

* [Content collections documentation](/en/guides/content-collections/)

# Invalid use of getDataEntryById or getEntryBySlug function.

> **GetEntryDeprecationError**: The `METHOD` function is deprecated and cannot be used to query the “COLLECTION” collection. Use `getEntry` instead.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `getDataEntryById` and `getEntryBySlug` functions are deprecated and cannot be used with content collections. Use the `getEntry` function instead.

# getImage() must be used on the server.

> **GetImageNotUsedOnServer**: `getImage()` should only be used on the server. To use images on the client, render the `src` from `getImage()` during the server render, then pass it to the client for usage.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `getImage()` function is only available on the server. To use images on the client, either render the `src` from `getImage()` during the server render so it can be used in client-side scripts, or use a standard `<img>` tag.

```astro
---
import { getImage } from "astro:assets";
import myImage from "../assets/my_image.png";


const optimizedImage = await getImage({ src: myImage, width: 300 });
---


<script define:vars={{ imageSrc: optimizedImage.src }}>
  // Use imageSrc in client-side code
  document.getElementById('myImage').src = imageSrc;
</script>
```

**See Also:**

* [Images](/en/guides/images/)
* [getImage()](/en/reference/modules/astro-assets/#getimage)

# Missing params property on getStaticPaths route.

> **GetStaticPathsExpectedParams**: Missing or empty required `params` property on `getStaticPaths` route.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Every route specified by `getStaticPaths` require a `params` property specifying the path parameters needed to match the route.

For instance, the following code:

pages/blog/\[id].astro

```astro
---
export async function getStaticPaths() {
  return [
    { params: { id: '1' } }
  ];
}
---
```

Will create the following route: `site.com/blog/1`.

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [`params`](/en/reference/api-reference/#params)

# Invalid route parameter returned by getStaticPaths().

> **GetStaticPathsInvalidRouteParam**: Invalid `getStaticPaths()` route parameter for `KEY`. Expected a string or undefined, received `VALUE_TYPE` (`VALUE`)

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Since `params` are encoded into the URL, only certain types are supported as values.

/route/\[id].astro

```astro
---
export async function getStaticPaths() {
  return [
    { params: { id: '1' } } // Works
    { params: { id: 2 } } // Does not work
    { params: { id: false } } // Does not work
    { params: { id: [1, 2] } } // Does not work
  ];
}
---
```

In routes using [rest parameters](/en/guides/routing/#rest-parameters), `undefined` can be used to represent a path with no parameters passed in the URL:

/route/\[...id].astro

```astro
---
export async function getStaticPaths() {
  return [
    { params: { id: '1' } } // /route/1
    { params: { id: '2' } } // /route/2
    { params: { id: undefined } } // /route/
  ];
}
---
```

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [`params`](/en/reference/api-reference/#params)

# getStaticPaths RSS helper is not available anymore.

Deprecated

Deprecated since Astro 4.0. The RSS helper no longer exists with an error fallback.

> **GetStaticPathsRemovedRSSHelper**: The RSS helper has been removed from `getStaticPaths`. Try the new @astrojs/rss package instead.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`getStaticPaths` no longer expose an helper for generating a RSS feed. We recommend migrating to the [@astrojs/rss](/en/recipes/rss/#setting-up-astrojsrss)integration instead.

**See Also:**

* [RSS Guide](/en/recipes/rss/)

# getStaticPaths() function required for dynamic routes.

> **GetStaticPathsRequired**: `getStaticPaths()` function is required for dynamic routes. Make sure that you `export` a `getStaticPaths` function from your dynamic route.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

In [Static Mode](/en/guides/routing/#static-ssg-mode), all routes must be determined at build time. As such, dynamic routes must `export` a `getStaticPaths` function returning the different paths to generate.

**See Also:**

* [Dynamic Routes](/en/guides/routing/#dynamic-routes)
* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [Server-side Rendering](/en/guides/on-demand-rendering/)

# The path doesn't contain any locale

> **i18nNoLocaleFoundInPath**: You tried to use an i18n utility on a path that doesn’t contain any locale. You can use `pathHasLocale` first to determine if the path has a locale.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An i18n utility tried to use the locale from a URL path that does not contain one. You can prevent this error by using pathHasLocale to check URLs for a locale first before using i18n utilities.

# i18n Not Enabled

> **i18nNotEnabled**: The `astro:i18n` module cannot be used without enabling i18n in your Astro config.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `astro:i18n` module cannot be used without enabling i18n in your Astro config. To enable i18n, add a default locale and a list of supported locales to your Astro config:

```js
import { defineConfig } from 'astro'
export default defineConfig({
 i18n: {
   locales: ['en', 'fr'],
   defaultLocale: 'en',
  },
})
```

For more information on internationalization support in Astro, see our [Internationalization guide](/en/guides/internationalization/).

**See Also:**

* [Internationalization](/en/guides/internationalization/)
* [`i18n` Configuration Reference](/en/reference/configuration-reference/#i18n)

# Image missing required "alt" property.

> **ImageMissingAlt**: Image missing “alt” property. “alt” text is required to describe important images on the page.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `alt` property allows you to provide descriptive alt text to users of screen readers and other assistive technologies. In order to ensure your images are accessible, the `Image` component requires that an `alt` be specified.

If the image is merely decorative (i.e. doesn’t contribute to the understanding of the page), set `alt=""` so that screen readers know to ignore the image.

**See Also:**

* [Images](/en/guides/images/)
* [Image component](/en/reference/modules/astro-assets/#image-)
*  [Image component#alt](/en/reference/modules/astro-assets/#alt-required)

# Image not found.

> **ImageNotFound**: Could not find requested image `IMAGE_PATH`. Does it exist?

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not find an image you imported. Often, this is simply caused by a typo in the path.

Images in Markdown are relative to the current file. To refer to an image that is located in the same folder as the `.md` file, the path should start with `./`

**See Also:**

* [Images](/en/guides/images/)

# Cannot set both densities and widths

> **IncompatibleDescriptorOptions**: Only one of `densities` or `widths` can be specified. In most cases, you’ll probably want to use only `widths` if you require specific widths.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Only one of `densities` or `widths` can be specified. Those attributes are used to construct a `srcset` attribute, which cannot have both `x` and `w` descriptors.

**See Also:**

* [Images](/en/guides/images/)

# You can't use the current function with the current strategy

> **IncorrectStrategyForI18n**: The function `FUNCTION_NAME` can only be used when the `i18n.routing.strategy` is set to `"manual"`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Some internationalization functions are only available when Astro’s own i18n routing is disabled by the configuration setting `i18n.routing: "manual"`.

**See Also:**

* [`i18n` routing](/en/guides/internationalization/#routing)

# Invalid component arguments.

> **Example error messages:**\
> InvalidComponentArgs: Invalid arguments passed to `<MyAstroComponent>` component.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro components cannot be rendered manually via a function call, such as `Component()` or `{items.map(Component)}`. Prefer the component syntax `<Component />` or `{items.map(item => <Component {...item} />)}`.

# Content entry data does not match schema.

> **Example error message:**\
> **blog** → **post** data does not match collection schema.\
> “title” is required.\
> “date” must be a valid date.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A content entry does not match its collection schema. Make sure that all required fields are present, and that all fields are of the correct type. You can check against the collection schema in your `src/content.config.*` file. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Content entry frontmatter does not match schema.

Deprecated

This error only applies to legacy content collections which were removed in Astro 6.

> **Example error message:**\
> **blog** → **post.md** frontmatter does not match collection schema.\
> “title” is required.\
> “date” must be a valid date.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A Markdown or MDX entry does not match its collection schema. Make sure that all required fields are present, and that all fields are of the correct type. You can check against the collection schema in your `src/content.config.*` file. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Invalid content entry slug.

> `COLLECTION_NAME` → `ENTRY_ID` has an invalid slug. `slug` must be a string.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A collection entry has an invalid `slug`. This field is reserved for generating entry slugs, and must be a string when present.

**See Also:**

* [The reserved entry `slug` field](/en/guides/content-collections/)

# Invalid dynamic route.

> **InvalidDynamicRoute**: The INVALID\_PARAM param for route ROUTE is invalid. Received **RECEIVED**.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A dynamic route param is invalid. This is often caused by an `undefined` parameter or a missing [rest parameter](/en/guides/routing/#rest-parameters).

**See Also:**

* [Dynamic routes](/en/guides/routing/#dynamic-routes)

# Invalid frontmatter injection.

> **InvalidFrontmatterInjectionError**: A remark or rehype plugin attempted to inject invalid frontmatter. Ensure “astro.frontmatter” is set to a valid JSON object that is not `null` or `undefined`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A remark or rehype plugin attempted to inject invalid frontmatter. This occurs when “astro.frontmatter” is set to `null`, `undefined`, or an invalid JSON object.

**See Also:**

* [Modifying frontmatter programmatically](/en/guides/markdown-content/#modifying-frontmatter-programmatically)

# Invalid value returned by a getStaticPaths path.

> **InvalidGetStaticPathParam**: Invalid params given to `getStaticPaths` path. Expected an `object`, got `PARAM_TYPE`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `params` property in `getStaticPaths`’s return value (an array of objects) should also be an object.

pages/blog/\[id].astro

```astro
---
export async function getStaticPaths() {
  return [
    { params: { slug: "blog" } },
    { params: { slug: "about" } }
  ];
}
---
```

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [`params`](/en/reference/api-reference/#params)

# Invalid entry inside getStaticPath's return value

> **InvalidGetStaticPathsEntry**: Invalid entry returned by getStaticPaths. Expected an object, got `ENTRY_TYPE`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`getStaticPaths`’s return value must be an array of objects. In most cases, this error happens because an array of array was returned. Using [`.flatMap()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) or a [`.flat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) call may be useful.

pages/blog/\[id].astro

```ts
export async function getStaticPaths() {
  return [ // <-- Array
    { params: { slug: "blog" } }, // <-- Object
    { params: { slug: "about" } }
  ];
}
```

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)

# Invalid value returned by getStaticPaths.

> **InvalidGetStaticPathsReturn**: Invalid type returned by `getStaticPaths`. Expected an `array`, got `RETURN_TYPE`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`getStaticPaths`’s return value must be an array of objects.

pages/blog/\[id].astro

```ts
export async function getStaticPaths() {
  return [ // <-- Array
    { params: { slug: "blog" } },
    { params: { slug: "about" } }
  ];
}
```

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [`params`](/en/reference/api-reference/#params)

# Invalid glob pattern.

Deprecated

This error was removed in Astro v6.0.0 along with the removal of `Astro.glob()`.

> **InvalidGlob**: Invalid glob pattern: `GLOB_PATTERN`. Glob patterns must start with ’./’, ‘../’ or ’/‘.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an invalid glob pattern. This is often caused by the glob pattern not being a valid file path.

**See Also:**

* [Glob Patterns](/en/guides/imports/#glob-patterns)

# Invalid internationalization middleware configuration

> **InvalidI18nMiddlewareConfiguration**: The option `redirectToDefaultLocale` can be enabled only when `prefixDefaultLocale` is also set to `true`; otherwise, redirects might cause infinite loops. Enable the option `prefixDefaultLocale` to continue to use `redirectToDefaultLocale`, or ensure both are set to `false`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An invalid i18n middleware configuration was detected.

# Error while loading image service.

> **InvalidImageService**: There was an error loading the configured image service. Please see the stack trace for more information.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

There was an error while loading the configured image service. This can be caused by various factors, such as your image service not properly exporting a compatible object in its default export, or an incorrect path.

If you believe that your service is properly configured and this error is wrong, please [open an issue](https://astro.build/issues/).

**See Also:**

* [Image Service API](/en/reference/image-service-reference/)

# Invalid prerender export.

> **Example error messages:**\
> InvalidPrerenderExport: A `prerender` export has been detected, but its value cannot be statically analyzed.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `prerender` feature only supports a subset of valid JavaScript — be sure to use exactly `export const prerender = true` so that our compiler can detect this directive at build time. Variables, `let`, and `var` declarations are not supported.

# Invalid redirect destination.

> **InvalidRedirectDestination**: The redirect from “FROM” to “TO” is invalid. The destination “TO” does not match any existing route in your project.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A dynamic redirect destination must match an existing route pattern. This error occurs when a redirect with dynamic parameters points to a destination that doesn’t correspond to any page in your project.

**See Also:**

* [Configured redirects](/en/guides/routing/#configured-redirects)

# You attempted to rewrite a 404 inside a static page, and this isn't allowed.

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **InvalidRewrite404**: Rewriting a 404 is only allowed inside on-demand pages.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The user tried to rewrite a 404 page inside a static page.

# Legacy content config file found.

> **Example error message:**\
> Found legacy content config file in “src/content/config.ts”. Please move this file to “src/content.config.ts” and ensure each collection has a loader defined.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A legacy content config file was found. Move the file to `src/content.config.ts` and update any collection definitions if needed. See the [Astro 6 migration guide](/en/guides/upgrade-to/v6/#removed-legacy-content-collections) for more information.

# Error in live content config.

> **Example error message:**\
> The schema cannot be a function for live collections. Please use a schema object instead. Check your collection definitions in your live content config file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Error in live content config.

**See Also:**

* [Defining live content schemas](/en/reference/modules/astro-content/#schema-1)

# Local images must be imported.

> **LocalImageUsedWrongly**: `Image`’s and `getImage`’s `src` parameter must be an imported image or an URL, it cannot be a string filepath. Received `IMAGE_FILE_PATH`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

When using the default image services, `Image`’s and `getImage`’s `src` parameter must be either an imported image or an URL, it cannot be a string of a filepath.

For local images from content collections, you can use the [image() schema helper](/en/guides/images/#images-in-content-collections) to resolve the images.

```astro
---
import { Image } from "astro:assets";
import myImage from "../my_image.png";
---


<!-- GOOD: `src` is the full imported image. -->
<Image src={myImage} alt="Cool image" />


<!-- GOOD: `src` is a URL. -->
<Image src="https://example.com/my_image.png" alt="Cool image" />


<!-- BAD: `src` is an image's `src` path instead of the full image object. -->
<Image src={myImage.src} alt="Cool image" />


<!-- BAD: `src` is a string filepath. -->
<Image src="../my_image.png" alt="Cool image" />
```

**See Also:**

* [Images](/en/guides/images/)

# Value assigned to locals is not accepted.

> **LocalsNotAnObject**: `locals` can only be assigned to an object. Other values like numbers, strings, etc. are not accepted.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when `locals` is overwritten with something that is not an object

For example:

```ts
import {defineMiddleware} from "astro:middleware";
export const onRequest = defineMiddleware((context, next) => {
  context.locals = 1541;
  return next();
});
```

# Astro.locals is not serializable

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **LocalsNotSerializable**: The information stored in `Astro.locals` for the path “`HREF`” is not serializable. Make sure you store only serializable data. (E03034)

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown in development mode when a user attempts to store something that is not serializable in `locals`.

For example:

```ts
import {defineMiddleware} from "astro/middleware";
export const onRequest = defineMiddleware((context, next) => {
  context.locals = {
    foo() {
      alert("Hello world!")
    }
  };
  return next();
});
```

# locals must not be reassigned.

> **LocalsReassigned**: `locals` cannot be assigned directly.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when a value is being set as the `locals` field on the Astro global or context.

# Content collection frontmatter invalid.

Deprecated

This error is from an older version of Astro and is no longer in use. If you are unable to upgrade your project to a more recent version, then you can consult [unmaintained snapshots of older documentation](/en/upgrade-astro/#older-docs-unmaintained) for assistance.

> **Example error message:**\
> Could not parse frontmatter in **blog** → **post.md**\
> “title” is required.\
> “date” must be a valid date.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A Markdown document’s frontmatter in `src/content/` does not match its collection schema. Make sure that all required fields are present, and that all fields are of the correct type. You can check against the collection schema in your `src/content/config.*` file. See the [Content collections documentation](/en/guides/content-collections/) for more information.

# Failed to parse Markdown frontmatter.

> **Example error messages:**\
> cannot read an implicit mapping pair; a colon is missed\
> unexpected end of the stream within a double quoted scalar\
> cannot read a block mapping entry; a multiline key may not be an implicit key

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an error while parsing the frontmatter of your Markdown file. This is often caused by a mistake in the syntax, such as a missing colon or a missing end quote.

# Image not found.

Deprecated

This error is no longer Markdown specific and as such, as been replaced by `ImageNotFound`

> Could not find requested image `IMAGE_PATH` at `FULL_IMAGE_PATH`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not find an image you included in your Markdown content. Usually, this is simply caused by a typo in the path.

Images in Markdown are relative to the current file. To refer to an image that is located in the same folder as the `.md` file, the path should start with `./`

**See Also:**

* [Images](/en/guides/images/)

# MDX integration missing.

> **MdxIntegrationMissingError**: Unable to render FILE. Ensure that the `@astrojs/mdx` integration is installed.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Unable to find the official `@astrojs/mdx` integration. This error is raised when using MDX files without an MDX integration installed.

**See Also:**

* [MDX installation and usage](/en/guides/integrations-guide/mdx/)

# Can't load the middleware.

> **MiddlewareCantBeLoaded**: An unknown error was thrown while loading your middleware.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown in development mode when middleware throws an error while attempting to loading it.

For example:

```ts
import {defineMiddleware} from "astro:middleware";
throw new Error("Error thrown while loading the middleware.")
export const onRequest = defineMiddleware(() => {
  return "string"
});
```

# The middleware didn't return a Response.

> **MiddlewareNoDataOrNextCalled**: Make sure your middleware returns a `Response` object, either directly or by returning the `Response` from calling the `next` function.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when the middleware does not return any data or call the `next` function.

For example:

```ts
import {defineMiddleware} from "astro:middleware";
export const onRequest = defineMiddleware((context, _) => {
  // doesn't return anything or call `next`
  context.locals.someData = false;
});
```

# The middleware returned something that is not a Response object.

> **MiddlewareNotAResponse**: Any data returned from middleware must be a valid `Response` object.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown in development mode when middleware returns something that is not a `Response` object.

For example:

```ts
import {defineMiddleware} from "astro:middleware";
export const onRequest = defineMiddleware(() => {
  return "string"
});
```

# Missing image dimensions

> Missing width and height attributes for `IMAGE_URL`. When using remote images, both dimensions are required in order to avoid cumulative layout shift (CLS).

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

For remote images, `width` and `height` cannot automatically be inferred from the original file. To avoid cumulative layout shift (CLS), either specify these two properties, or set [`inferSize`](/en/reference/modules/astro-assets/#infersize) to `true` to fetch a remote image’s original dimensions.

If your image is inside your `src` folder, you probably meant to import it instead. See [the Imports guide for more information](/en/guides/imports/#other-assets).

**See Also:**

* [Images](/en/guides/images/)
* [Image component#width-and-height-required](/en/reference/modules/astro-assets/#width-and-height-required-for-images-in-public)

# Index page not found.

> **MissingIndexForInternationalization**: Could not find index page. A root index page is required in order to create a redirect to the index URL of the default locale. (`/DEFAULT_LOCALE`)

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not find the index URL of your website. An index page is required so that Astro can create a redirect from the main index page to the localized index page of the default locale when using [`i18n.routing.prefixDefaultLocale`](/en/reference/configuration-reference/#i18nroutingprefixdefaultlocale).

**See Also:**

* [Internationalization](/en/guides/internationalization/#routing)
* [`i18n.routing` Configuration Reference](/en/reference/configuration-reference/#i18nrouting)

# The provided locale does not exist.

> **MissingLocale**: The locale/path `LOCALE` does not exist in the configured `i18n.locales`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro can’t find the requested locale. All supported locales must be configured in [i18n.locales](/en/reference/configuration-reference/#i18nlocales) and have corresponding directories within `src/pages/`.

# Missing value for client:media directive.

> **MissingMediaQueryDirective**: Media query not provided for `client:media` directive. A media query similar to `client:media="(max-width: 600px)"` must be provided

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A [media query](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) parameter is required when using the `client:media` directive.

```astro
<Counter client:media="(max-width: 640px)" />
```

**See Also:**

* [`client:media`](/en/reference/directives-reference/#clientmedia)

# Enabled manual internationalization routing without having a middleware.

> **MissingMiddlewareForInternationalization**: Your configuration setting `i18n.routing: 'manual'` requires you to provide your own i18n `middleware` file.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro throws an error if the user enables manual routing, but it doesn’t have a middleware file.

# Could not find Sharp.

> **MissingSharp**: Could not find Sharp. Please install Sharp (`sharp`) manually into your project or migrate to another image service.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Sharp is the default image service used for `astro:assets`. When using a [strict package manager](https://pnpm.io/pnpm-vs-npm#npms-flat-tree) like pnpm, Sharp must be installed manually into your project in order to use image processing.

If you are not using `astro:assets` for image processing, and do not wish to install Sharp, you can configure the following passthrough image service that does no processing:

```js
import { defineConfig, passthroughImageService } from "astro/config";
export default defineConfig({
 image: {
   service: passthroughImageService(),
 },
});
```

**See Also:**

* [Default Image Service](/en/guides/images/#default-image-service)
* [Image Services API](/en/reference/image-service-reference/)

# Content and data cannot be in same collection.

> **MixedContentDataCollectionError**: **COLLECTION\_NAME** contains a mix of content and data entries. All entries must be of the same type.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A legacy content collection cannot contain a mix of content and data entries. You must store entries in separate collections by type.

**See Also:**

* [Legacy content collections](/en/guides/upgrade-to/v5/#updating-existing-collections)

# Cannot use Server-side Rendering without an adapter.

> **NoAdapterInstalled**: Cannot use server-rendered pages without an adapter. Please install and configure the appropriate server adapter for your final deployment.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

To use server-side rendering, an adapter needs to be installed so Astro knows how to generate the proper output for your targeted deployment platform.

**See Also:**

* [Server-side Rendering](/en/guides/on-demand-rendering/)

# Cannot use Server Islands without an adapter.

> **NoAdapterInstalledServerIslands**: Cannot use server islands without an adapter. Please install and configure the appropriate server adapter for your final deployment.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

To use server islands, the same constraints exist as for sever-side rendering, so an adapter is needed.

**See Also:**

* [On-demand Rendering](/en/guides/on-demand-rendering/)

# No client entrypoint specified in renderer.

> **NoClientEntrypoint**: `COMPONENT_NAME` component has a `client:CLIENT_DIRECTIVE` directive, but no client entrypoint was provided by `RENDERER_NAME`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro tried to hydrate a component on the client, but the renderer used does not provide a client entrypoint to use to hydrate.

**See Also:**

* [addRenderer option](/en/reference/integrations-reference/#addrenderer-option)
* [Hydrating framework components](/en/guides/framework-components/#hydrating-interactive-components)

# Missing hint on client:only directive.

> **NoClientOnlyHint**: Unable to render `COMPONENT_NAME`. When using the `client:only` hydration strategy, Astro needs a hint to use the correct renderer.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`client:only` components are not run on the server, as such Astro does not know (and cannot guess) which renderer to use and require a hint. Like such:

```astro
  <SomeReactComponent client:only="react" />
```

**See Also:**

* [`client:only`](/en/reference/directives-reference/#clientonly)

# Could not process image metadata.

> Could not process image metadata for `IMAGE_PATH`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not process the metadata of an image you imported. This is often caused by a corrupted or malformed image and re-exporting the image from your image editor may fix this issue.

**See Also:**

* [Images](/en/guides/images/)

# No import found for component.

> **NoMatchingImport**: Could not render `COMPONENT_NAME`. No matching import has been found for `COMPONENT_NAME`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

No import statement was found for one of the components. If there is an import statement, make sure you are using the same identifier in both the imports and the component usage.

# No matching renderer found.

> Unable to render `COMPONENT_NAME`. There are `RENDERER_COUNT` renderer(s) configured in your `astro.config.mjs` file, but none were able to server-side render `COMPONENT_NAME`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

None of the installed integrations were able to render the component you imported. Make sure to install the appropriate integration for the type of component you are trying to include in your page.

For JSX / TSX files, [@astrojs/react](/en/guides/integrations-guide/react/), [@astrojs/preact](/en/guides/integrations-guide/preact/) or [@astrojs/solid-js](/en/guides/integrations-guide/solid-js/) can be used. For Vue and Svelte files, the [@astrojs/vue](/en/guides/integrations-guide/vue/) and [@astrojs/svelte](/en/guides/integrations-guide/svelte/) integrations can be used respectively

**See Also:**

* [Frameworks components](/en/guides/framework-components/)
* [UI Frameworks](/en/guides/integrations/#official-integrations)

# No static path found for requested path.

> **NoMatchingStaticPathFound**: A `getStaticPaths()` route pattern was matched, but no matching static path was found for requested path `PATH_NAME`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A [dynamic route](/en/guides/routing/#dynamic-routes) was matched, but no corresponding path was found for the requested parameters. This is often caused by a typo in either the generated or the requested path.

**See Also:**

* [getStaticPaths()](/en/reference/routing-reference/#getstaticpaths)

# Prerendered routes aren't supported when internationalization domains are enabled.

> **NoPrerenderedRoutesWithDomains**: Static pages aren’t yet supported with multiple domains. To enable this feature, you must disable prerendering for the page COMPONENT

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Static pages aren’t yet supported with i18n domains. If you wish to enable this feature, you have to disable prerendering.

# Invalid type returned by Astro page.

> Route returned a `RETURNED_VALUE`. Only a Response can be returned from Astro files.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Only instances of [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) can be returned inside Astro files.

pages/login.astro

```astro
---
return new Response(null, {
 status: 404,
 statusText: 'Not found'
});


// Alternatively, for redirects, Astro.redirect also returns an instance of Response
return Astro.redirect('/login');
---
```

**See Also:**

* [Response](/en/guides/on-demand-rendering/#response)

# Page number param not found.

> **PageNumberParamNotFound**: \[paginate()] page number param `PARAM_NAME` not found in your filepath.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The page number parameter was not found in your filepath.

**See Also:**

* [Pagination](/en/guides/routing/#pagination)

# Astro.clientAddress cannot be used inside prerendered routes.

> **PrerenderClientAddressNotAvailable**: `Astro.clientAddress` cannot be used inside prerendered route NAME

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `Astro.clientAddress` property cannot be used inside prerendered routes.

**See Also:**

* [On-demand rendering](/en/guides/on-demand-rendering/)
* [Astro.clientAddress](/en/reference/api-reference/#clientaddress)

# Prerendered dynamic endpoint has path collision.

> **PrerenderDynamicEndpointPathCollide**: Could not render `PATHNAME` with an `undefined` param as the generated path will collide during prerendering. Prevent passing `undefined` as `params` for the endpoint’s `getStaticPaths()` function, or add an additional extension to the endpoint’s filename.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The endpoint is prerendered with an `undefined` param so the generated path will collide with another route.

If you cannot prevent passing `undefined`, then an additional extension can be added to the endpoint file name to generate the file with a different name. For example, renaming `pages/api/[slug].ts` to `pages/api/[slug].json.ts`.

**See Also:**

* [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths)
* [`params`](/en/reference/api-reference/#params)

# Prerendered route generates the same path as another route.

> **PrerenderRouteConflict**: Could not render `PATHNAME` from route `THIS_ROUTE` as it conflicts with higher priority route `WINNING_ROUTE`.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Two prerendered routes generate the same path, resulting in a collision. A static path can only be generated by one route.

**See Also:**

* [Route Priority Order](/en/guides/routing/#route-priority-order)
* [`prerenderConflictBehavior`](/en/reference/configuration-reference/#prerenderconflictbehavior)

# A redirect must be given a location with the Location header.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

A redirect must be given a location with the `Location` header.

**See Also:**

* [Astro.redirect](/en/reference/api-reference/#redirect)

# Remote image is not allowed

> Remote image `IMAGE_URL` is not allowed by your image configuration.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The remote image URL does not match your configured `image.domains` or `image.remotePatterns`.

# Attempted to render an undefined content collection entry.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro tried to render a content collection entry that was undefined. This can happen if you try to render an entry that does not exist.

# Invalid slot name.

> **ReservedSlotName**: Unable to create a slot named `SLOT_NAME`. `SLOT_NAME` is a reserved slot name. Please update the name of this slot.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Certain words cannot be used for slot names due to being already used internally.

**See Also:**

* [Named slots](/en/basics/astro-components/#named-slots)

# Unable to set response.

> **ResponseSentError**: The response has already been sent to the browser and cannot be altered.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Making changes to the response, such as setting headers, cookies, and the status code can only be done in [page components](/en/basics/astro-pages/).

**See Also:**

* [HTML streaming](/en/guides/on-demand-rendering/#html-streaming)

# Astro couldn't find the route to rewrite, or if was found but it emitted an error during the rendering phase.

Deprecated

This error cannot be emitted by Astro anymore

> **RewriteEncounteredAnError**: The route ROUTE that you tried to render doesn’t exist, or it emitted an error during the rendering phase. STACK ? STACK : ”.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The user tried to rewrite using a route that doesn’t exist, or it emitted a runtime error during its rendering phase.

# Cannot use Astro.rewrite after the request body has been read

> **RewriteWithBodyUsed**: Astro.rewrite() cannot be used if the request body has already been read. If you need to read the body, first clone the request.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`Astro.rewrite()` cannot be used if the request body has already been read. If you need to read the body, first clone the request. For example:

```js
const data = await Astro.request.clone().formData();


Astro.rewrite("/target")
```

**See Also:**

* [Request.clone()](https://developer.mozilla.org/en-US/docs/Web/API/Request/clone)
* [Astro.rewrite](/en/reference/api-reference/#rewrite)

# Route not found.

> **RouteNotFound**: Astro could not find a route that matches the one you requested.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro couldn’t find a route matching the one provided by the user

# Module is only available server-side

> **ServerOnlyModule**: The “NAME” module is only available server-side.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

This module is only available server-side.

# Session storage was enabled but not configured.

Deprecated

This error was removed in Astro 5.7, when the Sessions feature stopped being experimental.

> The `experimental.session` flag was set to `true`, but no storage was configured. Either configure the storage manually or use an adapter that provides session storage.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when session storage is enabled but not configured.

**See Also:**

* [Sessions](/en/guides/sessions/)

# Session flag not set

Deprecated

This error was removed in Astro 5.7, when the Sessions feature stopped being experimental.

> Session config was provided without enabling the `experimental.session` flag

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when session storage is configured but the `experimental.session` flag is not enabled.

**See Also:**

* [Sessions](/en/guides/sessions/)

# Session storage could not be initialized.

> Error when initializing session storage with driver `DRIVER`. `ERROR`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when the session storage could not be initialized.

**See Also:**

* [Sessions](/en/guides/sessions/)

# Session data could not be saved.

> Error when saving session data with driver `DRIVER`. `ERROR`

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Thrown when the session data could not be saved.

**See Also:**

* [Sessions](/en/guides/sessions/)

# Sessions cannot be used with an adapter that doesn't support server output.

Deprecated

This error was removed in Astro 5.7, when the Sessions feature stopped being experimental.

> **SessionWithoutSupportedAdapterOutputError**: Sessions require an adapter that supports server output. The adapter must set `"server"` in the `buildOutput` adapter feature.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Your adapter must support server output to use sessions.

**See Also:**

* [Sessions](/en/guides/sessions/)

# Astro.clientAddress is not available in prerendered pages.

> **StaticClientAddressNotAvailable**: `Astro.clientAddress` is only available on pages that are server-rendered.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `Astro.clientAddress` property is only available when [Server-side rendering](/en/guides/on-demand-rendering/) is enabled.

To get the user’s IP address in static mode, different APIs such as [Ipify](https://www.ipify.org/) can be used in a [Client-side script](/en/guides/client-side-scripts/) or it may be possible to get the user’s IP using a serverless function hosted on your hosting provider.

**See Also:**

* [Enabling SSR in Your Project](/en/guides/on-demand-rendering/)
* [Astro.clientAddress](/en/reference/api-reference/#clientaddress)

# Astro.redirect is not available in static mode.

Deprecated

Deprecated since version 2.6.

> **StaticRedirectNotAvailable**: Redirects are only available when using `output: 'server'` or `output: 'hybrid'`. Update your Astro config if you need SSR features.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The `Astro.redirect` function is only available when [Server-side rendering](/en/guides/on-demand-rendering/) is enabled.

To redirect on a static website, the [meta refresh attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta) can be used. Certain hosts also provide config-based redirects (ex: [Netlify redirects](https://docs.netlify.com/routing/redirects/)).

**See Also:**

* [Enabling SSR in Your Project](/en/guides/on-demand-rendering/)
* [Astro.redirect](/en/reference/api-reference/#redirect)

# Unavailable Astro global in getStaticPaths()

> The Astro global is not available in getStaticPaths().

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Unavailable Astro global in getStaticPaths

# Unhandled rejection

> **UnhandledRejection**: Astro detected an unhandled rejection. Here’s the stack trace:\
> STACK

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro could not find any code to handle a rejected `Promise`. Make sure all your promises have an `await` or `.catch()` handler.

# Unknown CLI Error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error while starting one of its CLI commands. The error message should contain more information.

If you can reliably cause this error to happen, we’d appreciate if you could [open an issue](https://astro.build/issues/)

# Unknown compiler error.

> Unknown compiler error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error while compiling your files. In most cases, this is not your fault, but an issue in our compiler.

If there isn’t one already, please [create an issue](https://astro.build/issues/compiler).

**See Also:**

* [withastro/compiler issues list](https://astro.build/issues/compiler)

# Unknown configuration error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error loading your Astro configuration file. This is often caused by a syntax error in your config and the message should offer more information.

If you can reliably cause this error to happen, we’d appreciate if you could [open an issue](https://astro.build/issues/)

**See Also:**

* [Configuration Reference](/en/reference/configuration-reference/)

# Unknown Content Collection Error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error loading your content collections. This can be caused by certain errors inside your `src/content.config.ts` file or some internal errors.

If you can reliably cause this error to happen, we’d appreciate if you could [open an issue](https://astro.build/issues/)

# Unknown CSS Error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error while parsing your CSS. Oftentimes, this is caused by a syntax error and the error message should contain more information.

**See Also:**

* [Styles and CSS](/en/guides/styling/)

# An unknown error occurred while reading or writing files to disk.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An unknown error occurred while reading or writing files to disk. It can be caused by many things, eg. missing permissions or a file not existing we attempt to read.

# Unknown Markdown Error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro encountered an unknown error while parsing your Markdown. Oftentimes, this is caused by a syntax error and the error message should contain more information.

# Unknown Vite Error.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Vite encountered an unknown error while rendering your project. We unfortunately do not know what happened (or we would tell you!)

If you can reliably cause this error to happen, we’d appreciate if you could [open an issue](https://astro.build/issues/)

**See Also:**

* [Vite troubleshooting guide](https://vite.dev/guide/troubleshooting.html)

# Unsupported transform in content config.

> **UnsupportedConfigTransformError**: `transform()` functions in your content config must return valid JSON, or data types compatible with the devalue library (including Dates, Maps, and Sets).\
> Full error: PARSE\_ERROR

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

`transform()` functions in your content config must return valid JSON, or data types compatible with the devalue library (including Dates, Maps, and Sets).

**See Also:**

* [devalue library](https://github.com/rich-harris/devalue)

# Unsupported or malformed URL.

> **UnsupportedExternalRedirect**: The destination URL in the external redirect from “FROM” to “TO” is unsupported.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

An external redirect must start with http or https, and must be a valid URL.

**See Also:**

* [Astro.redirect](/en/reference/api-reference/#redirect)

# Unsupported image conversion

> **UnsupportedImageConversion**: Converting between vector (such as SVGs) and raster (such as PNGs and JPEGs) images is not currently supported.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

Astro does not currently supporting converting between vector (such as SVGs) and raster (such as PNGs and JPEGs) images.

**See Also:**

* [Images](/en/guides/images/)

# Unsupported image format

> **UnsupportedImageFormat**: Received unsupported format `FORMAT` from `IMAGE_PATH`. Currently only SUPPORTED\_FORMATS.JOIN(’, ’) are supported by our image services.

## What went wrong?

[Section titled “What went wrong?”](#what-went-wrong)

The built-in image services do not currently support optimizing all image formats.

For unsupported formats such as GIFs, you may be able to use an `img` tag directly:

```astro
---
import rocket from '../assets/images/rocket.gif';
---


<img src={rocket.src} width={rocket.width} height={rocket.height} alt="A rocketship in space." />
```

# Configuring experimental flags

Experimental features are available only after enabling a flag in the Astro configuration file.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
    experimental: {
        // enable experimental flags
        // to try out new features
    },
});
```

Astro offers experimental flags to give users early access to new features for testing and feedback.

These flags allow you to participate in feature development by reporting issues and sharing your opinions. These features are not guaranteed to be stable and may include breaking changes even in small `patch` releases while the feature is actively developed.

We recommend [updating Astro](/en/upgrade-astro/#upgrade-to-the-latest-version) frequently, and keeping up with release notes in the [Astro changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) which will inform you of any changes needed to your project code. The experimental feature documentation will always be updated for the current released version only.

