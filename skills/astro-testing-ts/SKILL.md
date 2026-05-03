---
name: astro-testing-ts
description: Astro testing, TypeScript, troubleshooting, and migration guides - Vitest and Playwright testing setup, TypeScript configuration and strict modes, troubleshooting common issues, migrating from Next.js/Nuxt/SvelteKit/Gatsby/Hugo/Jekyll/WordPress/etc, and upgrading between Astro major versions (v0 through v6). Load this for testing, type issues, debugging, or migrating to a newer Astro version.
---

# Astro Testing, TypeScript & Migration Documentation

> This skill covers testing strategies, TypeScript configuration, troubleshooting, and migration/upgrade guides.


# Migrate an existing project to Astro

> Some tips and tricks for converting your site to Astro.

**Ready to convert your site to Astro?** See one of our guides for migration tips.

## Migration Guides

[Section titled “Migration Guides”](#migration-guides)

* ![](/logos/create-react-app.svg)

  ### [Create React App](/en/guides/migrate-to-astro/from-create-react-app/)

* ![](/logos/docusaurus.svg)

  ### [Docusaurus](/en/guides/migrate-to-astro/from-docusaurus/)

* ![](/logos/eleventy.svg)

  ### [Eleventy](/en/guides/migrate-to-astro/from-eleventy/)

* ![](/logos/gatsby.svg)

  ### [Gatsby](/en/guides/migrate-to-astro/from-gatsby/)

* ![](/logos/gitbook.svg)

  ### [GitBook](/en/guides/migrate-to-astro/from-gitbook/)

* ![](/logos/gridsome.svg)

  ### [Gridsome](/en/guides/migrate-to-astro/from-gridsome/)

* ![](/logos/hugo.svg)

  ### [Hugo](/en/guides/migrate-to-astro/from-hugo/)

* ![](/logos/jekyll.png)

  ### [Jekyll](/en/guides/migrate-to-astro/from-jekyll/)

* ![](/logos/nextjs.svg)

  ### [Next.js](/en/guides/migrate-to-astro/from-nextjs/)

* ![](/logos/nuxtjs.svg)

  ### [NuxtJS](/en/guides/migrate-to-astro/from-nuxtjs/)

* ![](/logos/pelican.svg)

  ### [Pelican](/en/guides/migrate-to-astro/from-pelican/)

* ![](/logos/sveltekit.svg)

  ### [SvelteKit](/en/guides/migrate-to-astro/from-sveltekit/)

* ![](/logos/vuepress.png)

  ### [VuePress](/en/guides/migrate-to-astro/from-vuepress/)

* ![](/logos/wordpress.svg)

  ### [WordPress](/en/guides/migrate-to-astro/from-wordpress/)

Note that many of these pages are **stubs**: they’re collections of resources waiting for your contribution!

## Why migrate your site to Astro?

[Section titled “Why migrate your site to Astro?”](#why-migrate-your-site-to-astro)

Astro provides many benefits: performance, simplicity, and many of the features you want built right into the framework. When you do need to extend your site, Astro provides several [official and 3rd-party community integrations](https://astro.build/integrations).

Migrating may be less work than you think!

Depending on your existing project, you may be able to use your existing:

* [UI framework components](/en/guides/framework-components/) directly in Astro.

* [CSS stylesheets or libraries](/en/guides/styling/) including Tailwind.

* [Markdown/MDX files](/en/guides/markdown-content/), configured using your existing [remark and rehype plugins](/en/guides/markdown-content/#markdown-plugins).

* [Content from a CMS](/en/guides/cms/) through an integration or API.

## Which projects can I convert to Astro?

[Section titled “Which projects can I convert to Astro?”](#which-projects-can-i-convert-to-astro)

[Many existing sites can be built with Astro](/en/concepts/why-astro/). Astro is ideally suited for your existing content-based sites like blogs, landing pages, marketing sites and portfolios. Astro integrates with several popular headless CMSes, and allows you to connect eCommerce shop carts.

Astro allows you have a fully statically-generated website, a dynamic app with routes rendered on demand, or a combination of both with [complete control over your project rendering](/en/guides/on-demand-rendering/), making it a great replacement for SSGs or for sites that need to fetch some page data on the fly.

## How will my project design change?

[Section titled “How will my project design change?”](#how-will-my-project-design-change)

Depending on your existing project, you may need to think differently about:

* Designing in [Astro Islands](/en/concepts/islands/#what-is-an-island) to avoid sending unnecessary JavaScript to the browser.

* Providing client-side interactivity with [client-side `<script>` tags](/en/guides/client-side-scripts/) or [UI framework components](/en/guides/framework-components/).

* Managing [shared state](/en/recipes/sharing-state-islands/) with Nano Stores or local storage instead of app-wide hooks or wrappers.

# Migrating from Create React App (CRA)

> Tips for migrating an existing Create React App project to Astro

Astro’s [React integration](/en/guides/integrations-guide/react/) provides support for [using React components inside Astro components](/en/guides/framework-components/), including entire React apps like Create React App (CRA)!

src/pages/index.astro

```astro
---
// Import your root App component
import App from '../cra-project/App.jsx';
---
<!-- Use a client directive to load your app -->
<App client:load />
```

See how to [Build a Single Page Application (SPA) with Astro](https://logsnag.com/blog/react-spa-with-astro) External using React Router.

Many apps will “just work” as full React apps when you add them directly to your Astro project with the React integration installed. This is a great way to get your project up and running immediately and keep your app functional while you migrate to Astro.

Over time, you can convert your structure piece-by-piece to a combination of `.astro` and `.jsx` components. You will probably discover you need fewer React components than you think!

Here are some key concepts and migration strategies to help you get started. Use the rest of our docs and our [Discord community](https://astro.build/chat) to keep going!

## Key Similarities between CRA and Astro

[Section titled “Key Similarities between CRA and Astro”](#key-similarities-between-cra-and-astro)

* The [syntax of `.astro` files is similar to JSX](/en/reference/astro-syntax/#differences-between-astro-and-jsx). Writing Astro should feel familiar.

* Astro uses file-based routing, and [allows specially named pages to create dynamic routes](/en/guides/routing/#dynamic-routes).

* Astro is [component-based](/en/basics/astro-components/), and your markup structure will be similar before and after your migration.

* Astro has [official integrations for React, Preact, and Solid](/en/guides/integrations-guide/react/) so you can use your existing JSX components. Note that in Astro, these files **must** have a `.jsx` or `.tsx` extension.

* Astro has support for [installing NPM packages](/en/guides/imports/#npm-packages), including React libraries. Many of your existing dependencies will work in Astro.

## Key Differences between CRA and Astro

[Section titled “Key Differences between CRA and Astro”](#key-differences-between-cra-and-astro)

When you rebuild your CRA site in Astro, you will notice some important differences:

* CRA is a single-page application that uses `index.js` as your project’s root. Astro is a multi-page site, and `index.astro` is your home page.

* [`.astro` components](/en/basics/astro-components/) are not written as exported functions that return page templating. Instead, you’ll split your code into a “code fence” for your JavaScript and a body exclusively for the HTML you generate.

* [content-driven](/en/concepts/why-astro/#content-driven): Astro was designed to showcase your content and to allow you to opt-in to interactivity only as needed. An existing CRA app might be built for high client-side interactivity and may require advanced Astro techniques to include items that are more challenging to replicate using `.astro` components, such as dashboards.

## Add your CRA to Astro

[Section titled “Add your CRA to Astro”](#add-your-cra-to-astro)

Your existing app can be rendered directly inside a new Astro project, often with no changes to your app’s code.

### Create a new Astro project

[Section titled “Create a new Astro project”](#create-a-new-astro-project)

Use the `create astro` command for your package manager to launch Astro’s CLI wizard and select a new “empty” Astro project.

* npm

  ```shell
  npm create astro@latest
  ```

* pnpm

  ```shell
  pnpm create astro@latest
  ```

* Yarn

  ```shell
  yarn create astro@latest
  ```

### Add integrations and dependencies

[Section titled “Add integrations and dependencies”](#add-integrations-and-dependencies)

Add the React integration using the `astro add` command for your package manager. If your app uses other packages supported by the `astro add` command, like Tailwind and MDX, you can add them all with one command:

* npm

  ```shell
  npx astro add react
  npx astro add react tailwind mdx
  ```

* pnpm

  ```shell
  pnpm astro add react
  pnpm astro add react tailwind mdx
  ```

* Yarn

  ```shell
  yarn astro add react
  yarn astro add react tailwind mdx
  ```

If your CRA requires any dependencies (e.g. NPM packages), then install them individually using the command line or by adding them to your new Astro project’s `package.json` manually and then running an install command. Note that many, but not all, React dependencies will work in Astro.

### Add your existing app files

[Section titled “Add your existing app files”](#add-your-existing-app-files)

Copy your existing Create React App (CRA) project source files and folders (e.g. `components`, `hooks`, `styles`, etc.) into a new folder inside `src/`, keeping its file structure so your app will continue to work. Note that all `.js` file extensions must be renamed to `.jsx` or `.tsx`.

Do not include any configuration files. You will use Astro’s own `astro.config.mjs`, `package.json`, and `tsconfig.json`.

Move the contents of your app’s `public/` folder (e.g. static assets) into Astro’s `public/` folder.

* public/

  * logo.png
  * favicon.ico
  * …

* src/

  * cra-project/

    * App.jsx
    * …

  * pages/

    * index.astro

* astro.config.mjs

* package.json

* tsconfig.json

### Render your app

[Section titled “Render your app”](#render-your-app)

Import your app’s root component in the frontmatter section of `index.astro`, then render the `<App />` component in your page template:

src/pages/index.astro

```astro
---
import App from '../cra-project/App.jsx';
---
<App client:load />
```

Client directives

Your app needs a [client directive](/en/reference/directives-reference/#client-directives) for interactivity. Astro will render your React app as static HTML until you opt-in to client-side JavaScript.

Use `client:load` to ensure your app loads immediately from the server, or `client:only="react"` to skip rendering on the server and run your app entirely client-side.

## Convert your CRA to Astro

[Section titled “Convert your CRA to Astro”](#convert-your-cra-to-astro)

After [adding your existing app to Astro](#add-your-cra-to-astro), you will probably want to convert your app itself to Astro!

You will replicate a similar component-based design [using Astro HTML templating components for your basic structure](/en/basics/astro-components/) while importing and including individual React components (which may themselves be entire apps!) for islands of interactivity.

Every migration will look different and can be done incrementally without disrupting your working app. Convert individual pieces at your own pace so that more and more of your app is powered by Astro components over time.

As you convert your React app, you will decide which React components you will [rewrite as Astro components](#converting-jsx-files-to-astro-files). Your only restriction is that Astro components can import React components, but React components must only import other React components:

src/pages/static-components.astro

```diff
---
+import MyReactComponent from '../components/MyReactComponent.jsx';
---
<html>
  <body>
    <h1>Use React components directly in Astro!</h1>
    +<MyReactComponent />
  </body>
</html>
```

Instead of importing Astro components into React components, you can nest React components inside a single Astro component:

src/pages/nested-components.astro

```astro
---
import MyReactSidebar from '../components/MyReactSidebar.jsx';
import MyReactButton from '../components/MyReactButton.jsx';
---
<MyReactSidebar>
  <p>Here is a sidebar with some text and a button.</p>
  <div slot="actions">
    <MyReactButton client:idle />
  </div>
</MyReactSidebar>
```

You may find it helpful to learn about [Astro islands](/en/concepts/islands/) and [Astro components](/en/basics/astro-components/) before restructuring your CRA as an Astro project.

### Compare: JSX vs Astro

[Section titled “Compare: JSX vs Astro”](#compare-jsx-vs-astro)

Compare the following CRA component and a corresponding Astro component:

* JSX

  StarCount.jsx

  ```jsx
  import React, { useState, useEffect } from 'react';
  import Header from './Header';
  import Footer from './Footer';


  const Component = () => {
  const [stars, setStars] = useState(0);
  const [message, setMessage] = useState('');


  useEffect(() => {
      const fetchData = async () => {
          const res = await fetch('https://api.github.com/repos/withastro/astro');
          const json = await res.json();


          setStars(json.stargazers_count || 0);
          setMessage(json.message);
      };


      fetchData();
  }, []);


  return (
      <>
          <Header />
          <p style={{
              backgroundColor: `#f4f4f4`,
              padding: `1em 1.5em`,
              textAlign: `center`,
              marginBottom: `1em`
          }}>Astro has {stars} 🧑‍🚀</p>
          <Footer />
      </>
  )
  };


  export default Component;
  ```

* Astro

  StarCount.astro

  ```astro
  ---
  import Header from './Header.astro';
  import Footer from './Footer.astro';
  import './layout.css';
  const res = await fetch('https://api.github.com/repos/withastro/astro')
  const json = await res.json();
  const message = json.message;
  const stars = json.stargazers_count || 0;
  ---
  <Header />
  <p class="banner">Astro has {stars} 🧑‍🚀</p>
  <Footer />
  <style>
    .banner {
      background-color: #f4f4f4;
      padding: 1em 1.5em;
      text-align: center;
      margin-bottom: 1em;
    }
  </style>
  ```

### Converting JSX files to `.astro` files

[Section titled “Converting JSX files to .astro files”](#converting-jsx-files-to-astro-files)

Here are some tips for converting a CRA `.js` component into a `.astro` component:

1. Use the returned JSX of the existing CRA component function as the basis for your HTML template.

2. Change any [CRA or JSX syntax to Astro](#reference-convert-cra-syntax-to-astro) or to HTML web standards. This includes `{children}` and `className`, for example.

3. Move any necessary JavaScript, including import statements, into a [“code fence” (`---`)](/en/basics/astro-components/#the-component-script). Note: JavaScript to [conditionally render content](/en/reference/astro-syntax/#dynamic-html) is often written inside the HTML template directly in Astro.

4. Use [`Astro.props`](/en/reference/api-reference/#props) to access any additional props that were previously passed to your CRA function.

5. Decide whether any imported components also need to be converted to Astro. You can keep them as React components for now, or forever. But, you may eventually want to convert them to `.astro` components, especially if they do not need to be interactive!

6. Replace `useEffect()` with import statements or [`import.meta.glob()`](/en/guides/imports/#importmetaglob) to query your local files. Use `fetch()` to fetch external data.

### Migrating Tests

[Section titled “Migrating Tests”](#migrating-tests)

As Astro outputs raw HTML, it is possible to write end-to-end tests using the output of the build step. Any end-to-end tests written previously might work out-of-the-box if you have been able to match the markup of your CRA site. Testing libraries such as Jest and React Testing Library can be imported and used in Astro to test your React components.

See Astro’s [testing guide](/en/guides/testing/) for more.

## Reference: Convert CRA Syntax to Astro

[Section titled “Reference: Convert CRA Syntax to Astro”](#reference-convert-cra-syntax-to-astro)

### CRA Imports to Astro

[Section titled “CRA Imports to Astro”](#cra-imports-to-astro)

Update any [file imports](/en/guides/imports/) to reference relative file paths exactly. This can be done using [import aliases](/en/guides/typescript/#import-aliases), or by writing out a relative path in full.

Note that `.astro` and several other file types must be imported with their full file extension.

src/pages/authors/Fred.astro

```astro
---
import Card from '../../components/Card.astro';
---
<Card />
```

### CRA Children Props to Astro

[Section titled “CRA Children Props to Astro”](#cra-children-props-to-astro)

Convert any instances of `{children}` to an Astro `<slot />`. Astro does not need to receive `{children}` as a function prop and will automatically render child content in a `<slot />`.

src/components/MyComponent.astro

```diff
---
---
-export default function MyComponent(props) {
    -return (
      <div>
        -{props.children}
      </div>
-    );
-}


<div>
  <slot />
</div>
```

React components that pass multiple sets of children can be migrated to an Astro component using [named slots](/en/basics/astro-components/#named-slots).

See more about [specific `<slot />` usage in Astro](/en/basics/astro-components/#slots).

### CRA Data Fetching to Astro

[Section titled “CRA Data Fetching to Astro”](#cra-data-fetching-to-astro)

Fetching data in a Create React App component is similar to Astro, with some slight differences.

You will need to remove any instances of a side effect hook (`useEffect`) for either `import.meta.glob()` or `getCollection()`/`getEntry()` to access data from other files in your project source.

To [fetch remote data](/en/guides/data-fetching/), use `fetch()`.

These data requests are made in the frontmatter of the Astro component and use top-level await.

src/pages/index.astro

```astro
---
import { getCollection } from 'astro:content';


// Get all `src/content/blog/` entries
const allBlogPosts = await getCollection('blog');


// Get all `src/pages/posts/` entries
const allPosts = Object.values(import.meta.glob('../pages/post/*.md', { eager: true }));


// Fetch remote data
const response = await fetch('https://randomuser.me/api/');
const data = await response.json();
const randomUser = data.results[0];
---
```

See more about local files imports with [`import.meta.glob()`](/en/guides/imports/#importmetaglob), [querying with content collections](/en/guides/content-collections/#querying-build-time-collections) or [fetching remote data](/en/guides/data-fetching/).

### CRA Styling to Astro

[Section titled “CRA Styling to Astro”](#cra-styling-to-astro)

You may need to replace any [CSS-in-JS libraries](https://github.com/withastro/astro/issues/4432) (e.g. styled-components) with other available CSS options in Astro.

If necessary, convert any inline style objects (`style={{ fontWeight: "bold" }}`) to inline HTML style attributes (`style="font-weight:bold;"`). Or, use an [Astro `<style>` tag](/en/guides/styling/#styling-in-astro) for scoped CSS styles.

src/components/Card.astro

```diff
<div style={{backgroundColor: `#f4f4f4`, padding: `1em`}}>{message}</div>
<div style="background-color: #f4f4f4; padding: 1em;">{message}</div>
```

Tailwind is supported after installing the [Tailwind Vite plugin](/en/guides/styling/#tailwind). No changes to your existing Tailwind code are required!

See more about [Styling in Astro](/en/guides/styling/).

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

Your CRA might “just work” in Astro! But, you may likely need to make minor adjustments to duplicate your existing app’s functionality and/or styles.

If you cannot find your answers within these docs, please visit the [Astro Discord](https://astro.build/chat) and ask questions in our support forum!

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Code Fix: The SIBA Website's Move from Create-React-App to Astro ](https://brittanisavery.com/post/move-siba-to-astro)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Create React App to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-create-react-app.mdx)!

# Migrating from Docusaurus

> Tips for migrating an existing Docusaurus project to Astro

[Docusaurus](https://Docusaurus.io) is a popular documentation website builder built on React.

## Key Similarities between Docusaurus and Astro

[Section titled “Key Similarities between Docusaurus and Astro”](#key-similarities-between-docusaurus-and-astro)

Docusaurus and Astro share some similarities that will help you migrate your project:

* Both Astro and Docusaurus are modern, JavaScript-based (Jamstack) site builders intended for [content-driven websites](/en/concepts/why-astro/#content-driven), like documentation sites.

* Both Astro and Docusaurus support [MDX pages](/en/guides/markdown-content/). You should be able to use your existing `.mdx` files with Astro.

* Both Astro and Docusaurus use [file-based routing](/en/guides/routing/) to generate page routes automatically for any MDX file located in `src/pages`. Using Astro’s file structure for your existing content and when adding new pages should feel familiar.

* Astro has an [official integration for using React components](/en/guides/integrations-guide/react/). Note that in Astro, React files **must** have a `.jsx` or `.tsx` extension.

* Astro supports [installing NPM packages](/en/guides/imports/#npm-packages), including several for React. You may be able to keep some or all of your existing React components and dependencies.

* [Astro’s JSX-like syntax](/en/basics/astro-components/#the-component-template) should feel familiar if you are used to writing React.

## Key Differences between Docusaurus and Astro

[Section titled “Key Differences between Docusaurus and Astro”](#key-differences-between-docusaurus-and-astro)

When you rebuild your Docusaurus site in Astro, you will notice some important differences:

* Docusaurus is a React-based single-page application (SPA). Astro sites are multi-page apps built using [`.astro` components](/en/basics/astro-components/), but can also support [React, Preact, Vue.js, Svelte, SolidJS, AlpineJS](/en/guides/framework-components/) and raw HTML templating.

* Docusaurus was designed to build documentation websites and has some built-in, documentation-specific website features that you would have to build yourself in Astro. Instead, Astro offers some of these features through [Starlight: an official docs theme](https://starlight.astro.build). This website was the inspiration for Starlight, and now runs on it! You can also find more [community docs themes](https://astro.build/themes?search=\&categories%5B%5D=docs) with built-in features in our Themes Showcase.

* Docusaurus sites use MDX pages for content. Astro’s docs theme uses Markdown (`.md`) files by default and does not require you to use MDX. You can optionally [install Astro’s MDX integration](/en/guides/integrations-guide/mdx/) (included in our Starlight theme by default) and use `.mdx` files in addition to standard Markdown files.

## Switch from Docusaurus to Astro

[Section titled “Switch from Docusaurus to Astro”](#switch-from-docusaurus-to-astro)

To convert a Docusaurus documentation site to Astro, start with our official [Starlight docs theme starter template](https://starlight.astro.build), or explore more community docs themes in our [theme showcase](https://astro.build/themes?search=\&categories%5B%5D=docs).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template starlight
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template starlight
  ```

* Yarn

  ```shell
  yarn create astro --template starlight
  ```

Astro’s MDX integration is included by default, so you can [bring your existing content files to Starlight](https://starlight.astro.build/getting-started/#add-content) right away.

You can find Astro’s docs starter, and other official templates, on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Speeding up documentation by 10 times (Russian) ](https://habr.com/ru/articles/880220/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Docusaurus site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-docusaurus.mdx)!

# Migrating from Eleventy

> Tips for migrating an existing Eleventy project to Astro

[Eleventy](https://11ty.dev) is an open-source static site generator that works with multiple template languages.

## Key Similarities between Eleventy (11ty) and Astro

[Section titled “Key Similarities between Eleventy (11ty) and Astro”](#key-similarities-between-eleventy-11ty-and-astro)

Eleventy (11ty) and Astro share some similarities that will help you migrate your project:

* Both Astro and Eleventy are modern, JavaScript-based (Jamstack) site builders.

* Astro and Eleventy both allow you to use a [headless CMS, APIs or Markdown files for data](/en/guides/data-fetching/). You can continue to use your preferred content authoring system, and will be able to keep your existing content.

## Key Differences between Eleventy (11ty) and Astro

[Section titled “Key Differences between Eleventy (11ty) and Astro”](#key-differences-between-eleventy-11ty-and-astro)

When you rebuild your Eleventy (11ty) site in Astro, you will notice some important differences:

* Eleventy supports a variety of templating languages. Astro supports [including components from several popular JS Frameworks (e.g. React, Svelte, Vue, Solid)](/en/guides/framework-components/), but uses [Astro layouts, pages and components](/en/basics/astro-components/) for most page templating.

* Astro uses a [`src/` directory](/en/basics/project-structure/#src) for all files, including site metadata, that are available for querying and processing during site build. Within this is a [special `src/pages/` folder for file-based routing](/en/basics/astro-pages/).

* Astro uses a [`public/` folder for static assets](/en/basics/project-structure/#public) that do not need to be processed nor transformed during the build.

* In Eleventy, bundling CSS, JavaScript, and other assets needs to be configured manually. [Astro handles this for you out-of-the-box](/en/concepts/why-astro/#easy-to-use).

## Switch from Eleventy to Astro

[Section titled “Switch from Eleventy to Astro”](#switch-from-eleventy-to-astro)

To convert an Eleventy blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

Bring your existing Markdown (or MDX, with our optional integration) files as content to [create Markdown or MDX pages](/en/guides/markdown-content/).

Your Eleventy project allowed you to use a variety of templating languages to build your site. In an Astro project, your page templating will mostly be achieved with Astro components, which can be used as UI elements, layouts and even full pages. You may want to explore [Astro’s component syntax](/en/basics/astro-components/) to see how to template in Astro using components.

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[This Site Is Now Built with Astro ](https://aqandrew.com/blog/now-built-with-astro/)Why I switched from Eleventy.

[Website Rewrite: 2025 ](https://www.welchcanavan.com/posts/site-rewrite-2025/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting an Eleventy site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-eleventy.mdx)!

# Migrating from Gatsby

> Tips for migrating an existing Gatsby project to Astro

Here are some key concepts and migration strategies to help you get started. Use the rest of our docs and our [Discord community](https://astro.build/chat) to keep going!

## Key Similarities between Gatsby and Astro

[Section titled “Key Similarities between Gatsby and Astro”](#key-similarities-between-gatsby-and-astro)

Gatsby and Astro share some similarities that will help you migrate your project:

* The [syntax of `.astro` files is similar to JSX](/en/reference/astro-syntax/#jsx-like-expressions). Writing Astro should feel familiar.

* Astro has built-in support for [Markdown](/en/guides/markdown-content/) and an integration for using MDX files. Also, you can configure and continue to use many of your existing Markdown plugins.

* Astro also has an [official integration for using React components](/en/guides/integrations-guide/react/). Note that in Astro, React files **must** have a `.jsx` or `.tsx` extension.

* Astro has support for [installing NPM packages](/en/guides/imports/#npm-packages), including React libraries. Many of your existing dependencies will work in Astro.

* Like Gatsby, Astro projects can be SSG or [SSR with page-level prerendering](/en/guides/on-demand-rendering/).

## Key Differences between Gatsby and Astro

[Section titled “Key Differences between Gatsby and Astro”](#key-differences-between-gatsby-and-astro)

When you rebuild your Gatsby site in Astro, you will notice some important differences:

* Gatsby projects are React single-page apps and use `index.js` as your project’s root. Astro projects are multi-page sites, and `index.astro` is your home page.

* [Astro components](/en/basics/astro-components/) are not written as exported functions that return page templating. Instead, you’ll split your code into a “code fence” for your JavaScript and a body exclusively for the HTML you generate.

* [Local file data](/en/guides/imports/): Gatsby uses GraphQL to retrieve data from your project files. Astro uses ESM imports and top-level await functions (e.g. [`import.meta.glob()`](/en/guides/imports/#importmetaglob), [`getCollection()`](/en/guides/content-collections/#querying-build-time-collections)) to import data from your project files. You can manually add GraphQL to your Astro project but it is not included by default.

## Convert your Gatsby Project

[Section titled “Convert your Gatsby Project”](#convert-your-gatsby-project)

Each project migration will look different, but there are some common actions you will perform when converting from Gatsby to Astro.

### Create a new Astro project

[Section titled “Create a new Astro project”](#create-a-new-astro-project)

Use the `create astro` command for your package manager to launch Astro’s CLI wizard or choose a community theme from the [Astro Theme Showcase](https://astro.build/themes).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters (e.g. `docs`, `blog`, `portfolio`). Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  # launch the Astro CLI Wizard
  npm create astro@latest


  # create a new project with an official example
  npm create astro@latest -- --template <example-name>
  ```

* pnpm

  ```shell
  # launch the Astro CLI Wizard
  pnpm create astro@latest


  # create a new project with an official example
  pnpm create astro@latest --template <example-name>
  ```

* Yarn

  ```shell
  # launch the Astro CLI Wizard
  yarn create astro@latest


  # create a new project with an official example
  yarn create astro@latest --template <example-name>
  ```

Then, copy your existing Gatsby project files over to your new Astro project into a separate folder outside of `src`.

Tip

Visit <https://astro.new> for the full list of official starter templates, and links for opening a new project in StackBlitz or CodeSandbox.

### Install integrations (optional)

[Section titled “Install integrations (optional)”](#install-integrations-optional)

You may find it useful to install some of [Astro’s optional integrations](/en/guides/integrations/) to use while converting your Gatsby project to Astro:

* **@astrojs/react**: to reuse some existing React UI components in your new Astro site or keep writing with React components.

* **@astrojs/mdx**: to bring existing MDX files from your Gatsby project, or to use MDX in your new Astro site.

### Put your code in `src`

[Section titled “Put your code in src”](#put-your-code-in-src)

Following [Astro’s project structure](/en/basics/project-structure/):

1. **Delete** Gatsby’s `public/` folder.

   Gatsby uses the `public/` directory for its build output, so you can safely discard this folder. You will no longer need a built version of your Gatsby site. (Astro uses `dist/` by default for the build output.)

2. **Rename** Gatsby’s `static/` folder to `public/`, and use it as Astro’s `public/` folder.

   Astro uses a folder called `public/` for static assets. You can alternatively copy the contents of `static/` into your existing Astro `public/` folder.

3. **Copy or Move** Gatsby’s other files and folders (e.g. `components`, `pages`, etc.) as needed into your Astro `src/` folder as you rebuild your site, following [Astro’s project structure](/en/basics/project-structure/).

   Astro’s `src/pages/` folder is a special folder used for file-based routing to create your site’s pages and posts from `.astro`, `.md` and `.mdx` files. You will not have to configure any routing behavior for your Astro, Markdown, and MDX files.

   All other folders are optional, and you can organize the contents of your `src/` folder any way you like. Other common folders in Astro projects include `src/layouts/`, `src/components`, `src/styles`, and `src/scripts`.

### Tips: Convert JSX files to `.astro` files

[Section titled “Tips: Convert JSX files to .astro files”](#tips-convert-jsx-files-to-astro-files)

Here are some tips for converting a Gatsby `.js` component into a `.astro` component:

1. Use only the `return()` of the existing Gatsby component function as your HTML template.

2. Change any [Gatsby or JSX syntax to Astro syntax](#reference-convert-to-astro-syntax) or to HTML web standards. This includes `<Link to="">`, `{children}`, and `className`, for example.

3. Move any necessary JavaScript, including import statements, into a [“code fence” (`---`)](/en/basics/astro-components/#the-component-script). Note: JavaScript to [conditionally render content](/en/reference/astro-syntax/#dynamic-html) is often written inside the HTML template directly in Astro.

4. Use [`Astro.props`](/en/reference/api-reference/#props) to access any additional props that were previously passed to your Gatsby function.

5. Decide whether any imported components also need to be converted to Astro. With the official React integration installed, you can [use existing React components in your Astro files](/en/guides/framework-components/). But, you may want to convert them to `.astro` components, especially if they do not need to be interactive!

6. Remove any GraphQL queries. Instead, use import and [`import.meta.glob()`](/en/guides/imports/#importmetaglob) statements to query your local files.

See [an example from Gatsby’s Blog starter template converted step-by-step](#guided-example-gatsby-layout-to-astro)

#### Compare: `.jsx` vs `.astro`

[Section titled “Compare: .jsx vs .astro”](#compare-jsx-vs-astro)

Compare the following Gatsby component and a corresponding Astro component:

* JSX

  component.jsx

  ```jsx
  import * as React from "react"
  import { useStaticQuery, graphql } from "gatsby"
  import Header from "./header"
  import Footer from "./footer"
  import "./layout.css"


  const Component = ({ message, children }) => {
    const data = useStaticQuery(graphql`
      query SiteTitleQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `)
    return (
      <>
        <Header siteTitle={data.site.siteMetadata.title} />
        <div style={{ margin: `0`, maxWidth: `960`}}>{message}</div>
        <main>{children}</main>
        <Footer siteTitle={data.site.siteMetadata} />
      </>
    )
  }


  export default Component
  ```

* Astro

  component.astro

  ```astro
  ---
  import Header from "./Header.astro"
  import Footer from "./Footer.astro"
  import "../styles/stylesheet.css"
  import { site } from "../data/siteMetaData.js"
  const { message } = Astro.props
  ---
  <Header siteTitle={site.title} />
    <div style="margin: 0; max-width: 960;">{message}</div>
    <main>
      <slot />
    </main>
  <Footer siteTitle={site.title} />
  ```

### Migrating Layout Files

[Section titled “Migrating Layout Files”](#migrating-layout-files)

You may find it helpful to start by converting your Gatsby layouts and templates into [Astro layout components](/en/basics/layouts/).

Each Astro page explicitly requires `<html>`, `<head>`, and `<body>` tags to be present, so it is common to reuse a layout file across pages. Astro uses a [`<slot />`](/en/basics/astro-components/#slots) instead of React’s `{children}` prop for page content, with no import statement required. Your Gatsby `layout.js` and templates will not include these.

Note the standard HTML templating, and direct access to `<head>`:

src/layouts/Layout.astro

```astro
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <title>Astro</title>
  </head>
  <body>
    <!-- Wrap the slot element with your existing layout templating -->
    <slot />
  </body>
</html>
```

You may also wish to reuse code from Gatsby’s `src/components/seo.js` to include additional site metadata. Notice that Astro uses neither `<Helmet>` nor `<Header>` but instead creates `<head>` directly. You may import and use components, even within `<head>`, to separate and organize your page content.

### Migrating Pages and Posts

[Section titled “Migrating Pages and Posts”](#migrating-pages-and-posts)

In Gatsby, your [pages and posts](/en/basics/astro-pages/) may exist in `src/pages/` or outside of `src` in another folder, like `content`. In Astro, all your page content must live within `src/` unless you are using [content collections](/en/guides/content-collections/).

#### React Pages

[Section titled “React Pages”](#react-pages)

Your existing Gatsby JSX (`.js`) pages will need to be [converted from JSX files to `.astro` pages](#tips-convert-jsx-files-to-astro-files). You cannot use an existing JSX page file in Astro.

These [`.astro` pages](/en/basics/astro-pages/) must be located within `src/pages/` and will have page routes generated automatically based on their file path.

#### Markdown and MDX pages

[Section titled “Markdown and MDX pages”](#markdown-and-mdx-pages)

Astro has built-in support for Markdown and an optional integration for MDX files. Your existing [Markdown and MDX files](/en/guides/markdown-content/) can be reused but may require some adjustments to their frontmatter, such as adding [Astro’s special `layout` frontmatter property](/en/basics/layouts/#markdown-layouts). They can also be placed within `src/pages/` to take advantage of automatic file-based routing.

Alternatively, you can use [content collections](/en/guides/content-collections/) in Astro to store and manage your content. You will retrieve the content yourself and [generate those pages dynamically](/en/guides/content-collections/#generating-routes-from-content).

### Migrating Tests

[Section titled “Migrating Tests”](#migrating-tests)

As Astro outputs raw HTML, it is possible to write end-to-end tests using the output of the build step. Any end-to-end tests written previously might work out-of-the-box if you have been able to match the markup of the older Gatsby site. Testing libraries such as Jest and React Testing Library can be imported and used in Astro to test your React components.

See Astro’s [testing guide](/en/guides/testing/) for more.

### Repurpose config files

[Section titled “Repurpose config files”](#repurpose-config-files)

Gatsby has several top-level configuration files that also include site and page metadata and are used for routing. You will not use any of these `gatsby-*.js` files in your Astro project, but there may be some content that you can reuse as you build your Astro project:

* `gatsby-config.js`: Move your `siteMetadata: {}` into `src/data/siteMetadata.js` (or `siteMetadata.json`) to import data about your site (title, description, social accounts, etc.) into page layouts.

* `gatsby-browser.js`: Consider adding anything used here directly into your [main layout](#migrating-layout-files)’s `<head>` tag.

* `gatsby-node.js`: You will not need to create your own nodes in Astro, but viewing the schema in this file may help you with defining types in your Astro project.

* `gatsby-ssr.js`: If you choose to use SSR in Astro, you will [add and configure the SSR adapter](/en/guides/on-demand-rendering/) of your choice directly in `astro.config.mjs`.

## Reference: Convert to Astro Syntax

[Section titled “Reference: Convert to Astro Syntax”](#reference-convert-to-astro-syntax)

The following are some examples of Gatsby-specific syntax that you will need to convert to Astro. See more [differences between Astro and JSX](/en/reference/astro-syntax/#differences-between-astro-and-jsx) in the guide to writing Astro components.

### Gatsby Links to Astro

[Section titled “Gatsby Links to Astro”](#gatsby-links-to-astro)

Convert any Gatsby `<Link to="">`, `<NavLink>` etc. components to HTML `<a href="">` tags.

```diff
-<Link to="/blog">Blog</Link>
<a href="/blog">Blog</a>
```

Astro does not use any special component for links, although you are welcome to build your own `<Link>` component. You can then import and use this `<Link>` just as you would any other component.

src/components/Link.astro

```astro
---
const { to } = Astro.props
---
<a href={to}><slot /></a>
```

### Gatsby Imports to Astro

[Section titled “Gatsby Imports to Astro”](#gatsby-imports-to-astro)

If necessary, update any [file imports](/en/guides/imports/) to reference relative file paths exactly. This can be done using [import aliases](/en/guides/typescript/#import-aliases), or by writing out a relative path in full.

Note that `.astro` and several other file types must be imported with their full file extension.

src/pages/authors/Fred.astro

```astro
---
import Card from `../../components/Card.astro`;
---
<Card />
```

### Gatsby Children Props to Astro

[Section titled “Gatsby Children Props to Astro”](#gatsby-children-props-to-astro)

Convert any instances of `{children}` to an Astro `<slot />`. Astro does not need to receive `{children}` as a function prop and will automatically render child content in a `<slot />`.

src/components/MyComponent

```diff
---
---
-export default function MyComponent(props) {
    -return (
      <div>
        -{props.children}
      </div>
-    );
-}


<div>
  <slot />
</div>
```

React components that pass multiple sets of children can be migrated to an Astro component using [named slots](/en/basics/astro-components/#named-slots).

See more about [specific `<slot />` usage in Astro](/en/basics/astro-components/#slots).

### Gatsby Styling to Astro

[Section titled “Gatsby Styling to Astro”](#gatsby-styling-to-astro)

You may need to replace any [CSS-in-JS libraries](https://github.com/withastro/astro/issues/4432) (e.g. styled-components) with other available CSS options in Astro.

If necessary, convert any inline style objects (`style={{ fontWeight: "bold" }}`) to inline HTML style attributes (`style="font-weight:bold;"`). Or, use an [Astro `<style>` tag](/en/guides/styling/#styling-in-astro) for scoped CSS styles.

src/components/Card.astro

```diff
<div style={{backgroundColor: `#f4f4f4`, padding: `1em`}}>{message}</div>
<div style="background-color: #f4f4f4; padding: 1em;">{message}</div>
```

Tailwind is supported after installing the [Tailwind Vite plugin](/en/guides/styling/#tailwind). No changes to your existing Tailwind code are required!

Global styling is achieved in Gatsby using CSS imports in `gatsby-browser.js`. In Astro, you will import `.css` files directly into a main layout component to achieve global styles.

See more about [Styling in Astro](/en/guides/styling/).

### Gatsby Image Plugin to Astro

[Section titled “Gatsby Image Plugin to Astro”](#gatsby-image-plugin-to-astro)

Convert Gatsby’s `<StaticImage />` and `<GatsbyImage />` components to [Astro’s own image integration components](/en/guides/images/), or to a [standard HTML `<img>` / JSX `<img />`](/en/guides/images/#images-in-ui-framework-components) tag as appropriate in your React components.

src/pages/index.astro

```astro
---
import { Image } from 'astro:assets';
import rocket from '../assets/rocket.png';
---
<Image src={rocket} alt="A rocketship in space." />
<img src={rocket.src} alt="A rocketship in space.">
```

Astro’s `<Image />` component works in `.astro` and `.mdx` files only. See a [full list of its component attributes](/en/reference/modules/astro-assets/#image-) and note that several will differ from Gatsby’s attributes.

To continue using [images in Markdown (`.md`) files](/en/guides/images/#images-in-markdown-files) using standard Markdown syntax (`![]()`), you may need to update the link. Using the HTML `<img>` tag directly is not supported in `.md` files for local images, and must be converted to Markdown syntax.

src/pages/post-1.md

```md
# My Markdown Page


<!-- Local image stored at src/assets/stars.png -->
![A starry night sky.](../assets/stars.png)
```

In React (`.jsx`) components, use standard JSX image syntax (`<img />`). Astro will not optimize these images, but you can install and use NPM packages for more flexibility.

You can learn more about [using images in Astro](/en/guides/images/) in the Images Guide.

### Gatsby GraphQL to Astro

[Section titled “Gatsby GraphQL to Astro”](#gatsby-graphql-to-astro)

Remove all references to GraphQL queries, and instead use [`import.meta.glob()`](/en/guides/imports/#importmetaglob) to access data from your local files.

Or, if using content collections, query your Markdown and MDX files using [`getEntry()` and `getCollection()`](/en/guides/content-collections/#generating-routes-from-content).

These data requests are made in the frontmatter of the Astro component using the data.

src/pages/index.astro

```diff
---
-import { graphql } from "gatsby"
import { getCollection } from 'astro:content';


// Get all `src/content/blog/` entries
const allBlogPosts = await getCollection('blog');


// Get all `src/pages/posts/` entries
const allPosts = Object.values(import.meta.glob('../pages/post/*.md', { eager: true }));
---


-export const pageQuery = graphql`
  -{
    -allMarkdownRemark(sort: { frontmatter: { date: DESC } }) {
-      nodes {
-        excerpt
-        fields {
-          slug
-        }
-        frontmatter {
          -date(formatString: "MMMM DD, YYYY")
-          title
-          description
-        }
-      }
-    }
  -}
-`
```

## Guided example: Gatsby layout to Astro

[Section titled “Guided example: Gatsby layout to Astro”](#guided-example-gatsby-layout-to-astro)

This example converts the main project layout (`layout.js`) from Gatsby’s blog starter to `src/layouts/Layout.astro`.

This page layout shows one header when visiting the home page, and a different header with a link back to Home for all other pages.

1. Identify the `return()` JSX.

   layout.js

   ```jsx
   import * as React from "react"
   import { Link } from "gatsby"
   const Layout = ({ location, title, children }) => {
     const rootPath = `${__PATH_PREFIX__}/`
     const isRootPath = location.pathname === rootPath
     let header
     if (isRootPath) {
       header = (
         <h1 className="main-heading">
           <Link to="/">{title}</Link>
         </h1>
       )
     } else {
       header = (
         <Link className="header-link-home" to="/">
           Home
         </Link>
       )
     }
     return (
       <div className="global-wrapper" data-is-root-path={isRootPath}>
         <header className="global-header">{header}</header>
         <main>{children}</main>
         <footer>
           © {new Date().getFullYear()}, Built with
           {` `}
           <a href="https://www.gatsbyjs.com">Gatsby</a>
         </footer>
       </div>
     )
   }
   export default Layout
   ```

2. Create `Layout.astro` and add this `return` value, [converted to Astro syntax](#reference-convert-to-astro-syntax).

   Note that:

   * `{new Date().getFullYear()}` just works 🎉
   * `{children}` becomes `<slot />` 🦥
   * `className` becomes `class` 📛
   * `Gatsby` becomes `Astro` 🚀

   src/layouts/Layout.astro

   ```astro
   ---
   ---
   <div class="global-wrapper" data-is-root-path={isRootPath}>
     <header class="global-header">{header}</header>
     <main><slot /></main>
     <footer>
       © {new Date().getFullYear()}, Built with
       {` `}
       <a href="https://www.astro.build">Astro</a>
     </footer>
   </div>
   ```

3. Add a page shell so that your layout provides each page with the necessary parts of an HTML document:

   src/layouts/Layout.astro

   ```diff
   ---
   ---
   <html>
     <head>
       <meta charset="utf-8" />
       <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
       <meta name="viewport" content="width=device-width" />
       <title>Astro</title>
     </head>
     <body>
       <div class="global-wrapper" data-is-root-path={isRootPath}>
         <header class="global-header">{header}</header>
         <main>
           <slot />
         </main>
         <footer>
           &#169; {new Date().getFullYear()}, Built with
           {` `}
           <a href="https://www.astro.build">Astro</a>
         </footer>
       </div>
     </body>
   </html>
   ```

4. Add any needed imports, props, and JavaScript

   To conditionally render a header based on the page route and title in Astro:

   * Provide the props via `Astro.props`. (Remember: your Astro templating accesses props from its frontmatter, not passed into a function.)
   * Use a ternary operator to show one heading if this is the home page, and a different heading otherwise.
   * Remove variables for `{header}` and `{isRootPath}` as they are no longer needed.
   * Replace Gatsby’s `<Link/>` tags with `<a>` anchor tags.
   * Use `class` instead of `className`.
   * Import a local stylesheet from your project for the class names to take effect.

   src/layouts/Layout.astro

   ```diff
   ---
   +import '../styles/style.css';
   +const { title, pathname } = Astro.props
   ---
   <html>
     <head>
       <meta charset="utf-8" />
       <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
       <meta name="viewport" content="width=device-width" />
       <title>Astro</title>
     </head>
     <body>
       <div class="global-wrapper">
         <header class="global-header">
           +{ pathname === "/"
           +?
             <h1 class="main-heading">
             <a href="/">{title}</a>
             </h1>
           +:
             <h1 class="main-heading">
             <a class="header-link-home" href="/">Home</a>
             </h1>
           +}
         </header>
         <main>
           <slot />
         </main>
         <footer>
           &#169; {new Date().getFullYear()}, Built with
           {` `}
           <a href="https://www.astro.build">Astro</a>
         </footer>
       </div>
     </body>
   </html>
   ```

5. Update `index.astro` to use this new layout and pass it the necessary `title` and `pathname` props:

   src/pages/index.astro

   ```astro
   ---
   import Layout from '../layouts/Layout.astro';
   const pagePathname = Astro.url.pathname
   ---
   <Layout title="Home Page" pathname={pagePathname}>
       <p>Astro</p>
   </Layout>
   ```

   Tip

   You can [get the current page’s path using `Astro.url`](/en/reference/api-reference/#url).

6. To test the conditional header, create a second page, `about.astro` using the same pattern:

   src/pages/about.astro

   ```astro
   ---
   import Layout from '../layouts/Layout.astro';
   const pagePathname = Astro.url.pathname
   ---
   <Layout title="About" pathname={pagePathname}>
       <p>About</p>
   </Layout>
   ```

   You should see a link to “Home” only when visiting the About page.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Migrating from Gatsby to Astro ](https://loige.co/migrating-from-gatsby-to-astro/)How and why I migrated this blog from Gatsby to Astro and what I learned in the process.

[Migrating to Astro was EZ ](https://joelhooks.com/migrating-to-astro-was-ez)This is about the process of migrating from Gatsby to Astro, and why I chose Astro.

[My Switch from Gatsby to Astro ](https://www.joshfinnie.com/blog/my-switch-from-gatsby-to-astro/)The switch to Astro is definitely worth a blog post! It’s revolutionizing the static web development scene for the better.

[Why I moved to Astro from Gatsby ](https://dev.to/askrodney/why-i-moved-to-astro-from-gatsby-3fck)Taking a quick look at what made me want to switch and why Astro was a good fit.

[Another Migration: From Gatsby to Astro ](https://logarithmicspirals.com/blog/migrating-from-gatsby-to-astro/)Learn about how I transitioned my personal website from Gatsby to Astro as I share insights and experiences from the migration process.

[From Gatsby gridlock to Astro bliss: my personal site redesign ](https://jwn.gr/posts/migrating-from-gatsby-to-astro/)Gatsby has shown its age and I found myself seeking a modern alternative. Enter Astro — a framework that has breathed some new life into this site.

[Why and how I moved my blog away from Gatsby and React to Astro Js and Preact ](https://www.helmerdavila.com/blog/en/why-and-how-i-moved-my-blog-away-from-gatsby-and-react-to-astro-js-and-preact)All is about simplicity and power at the same time.

[How I rewrote my HUGE Gatsby site in Astro and learned to love it in the process ](https://dunedinsound.com/blog/how_i_rewrote_my_huge_gatsby_site_in_astro_and_learned_to_love_it_in_the_process/)Everything is faster. Happier. More productive.

[How I switched from Gatsby to Astro (While Keeping Drupal in the Mix) ](https://albert.skibinski.nl/en/blog/how-i-switched-gatsby-astro-while-keeping-drupal-mix/)I came across the relatively new Astro, which ticked all the boxes.

[Migrating my website from Gatsby to Astro ](https://dev.to/flashblaze/migrating-my-website-from-gatsby-to-astro-2ej5)Astro has entered the chat.

[Gatsby to Astro ](https://alvin.codes/writing/gatsby-to-astro)Why and how I migrated this website from Gatsby to Astro.

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Gatsby site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-gatsby.mdx)!

# Migrating from GitBook

> Tips for migrating an existing GitBook project to Astro

[GitBook](https://gitbook.com) is a web-based platform for creating and publishing documentation and books in a collaborative manner, with version control integration and customizable features.

## Key Similarities between GitBook and Astro

[Section titled “Key Similarities between GitBook and Astro”](#key-similarities-between-gitbook-and-astro)

GitBook and Astro share some similarities that will help you migrate your project:

* Both Astro and GitBook support [Markdown](/en/guides/markdown-content/). You can migrate all your existing documentation utilizing GitBook’s Git Sync feature.

* Both Astro and GitBook use some form of [file-based routing](/en/guides/routing/). Using Astro’s file structure for your existing content and when adding new pages should feel familiar.

## Key Differences between GitBook and Astro

[Section titled “Key Differences between GitBook and Astro”](#key-differences-between-gitbook-and-astro)

When you migrate your GitBook docs to Astro, you will notice some important differences:

* A GitBook site is edited using an online dashboard. In Astro, you will use a [code editor](/en/editor-setup/) and development environment to maintain your site. You can develop locally on your machine, or choose a cloud editor/development environment like StackBlitz or CodeSandbox.

* GitBook stores your content in a database. In Astro, you will have individual files (typically Markdown or MDX) in your [project directory](/en/basics/project-structure/) for each page’s content. Or, you can choose to use a [CMS for your content](/en/guides/cms/) and use Astro to fetch and present the data.

* GitBook uses a custom syntax on top of Markdown for content. Astro supports Markdoc via the optional [Markdoc integration](/en/guides/integrations-guide/markdoc/), which features a similar syntax to the one you would use in GitBook.

## Switch from GitBook to Astro

[Section titled “Switch from GitBook to Astro”](#switch-from-gitbook-to-astro)

To convert a GitBook documentation site to Astro, start with our official [Starlight docs theme starter template](https://starlight.astro.build), or explore more community docs themes in our [theme showcase](https://astro.build/themes?search=\&categories%5B%5D=docs).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template starlight
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template starlight
  ```

* Yarn

  ```shell
  yarn create astro --template starlight
  ```

Once you have a new Astro project, you can sync your existing GitBook content to your new Astro project. GitBook has a [Git Sync feature](https://docs.gitbook.com/product-tour/git-sync) that will automatically sync your GitBook content to a GitHub/GitLab repository.

To sync directly to the docs template’s content collection, specify `src/content/docs/en` or `src/content/docs` as the project directory.

Caution

When enabling Git Sync be sure to specify “**GitBook to GitHub**” as the priority. This will ensure that your GitBook content is synced to your GitHub repository. Otherwise, you will overwrite your existing GitBook content.

After syncing the content, you will now have a copy of your GitBook content in your Astro repository. Disable git sync to prevent future syncing with GitBook.

Note that although you now have your content migrated to your Astro project, it will not be immediately usable. To use this content in your Astro site, you will need to spend some time manually changing GitBook’s syntax into a format compatible with Astro. In particular:

* Astro’s [Markdoc integration](/en/guides/integrations-guide/markdoc/) requires that the file extension be `.mdoc`. This is to avoid conflicts with other Markdown extensions like `.mdx` and `.md`.
* GitBook syntax differs from Markdoc where the `/` prefix denoting a closing tag is replaced with `end` for GitBook files. You will need to update this notation throughout your files.
* Some features of GitBook rely on custom components. These components will not exist in Astro and must be created and added to your project through [Markdoc’s config `tags` attribute](/en/guides/integrations-guide/markdoc/#use-astro-components-as-markdoc-tags) or removed from your files.

## Community Resources

[Section titled “Community Resources”](#community-resources)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a GitBook site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-gitbook.mdx)!

# Migrating from Gridsome

> Tips for migrating an existing Gridsome project to Astro

[Gridsome](https://gridsome.org) is an open-source static site generator built on Vue and GraphQL.

## Key Similarities between Gridsome and Astro

[Section titled “Key Similarities between Gridsome and Astro”](#key-similarities-between-gridsome-and-astro)

Gridsome and Astro share some similarities that will help you migrate your project:

* Both Gridsome and Astro are modern JavaScript static-site generators with similar [project file structures](/en/basics/project-structure/#directories-and-files).

* Both Gridsome and Astro use a `src/` folder for your project files and a [special `src/pages/` folder for file-based routing](/en/basics/astro-pages/). Creating and managing pages for your site should feel familiar.

* Astro has [an official integration for using Vue components](/en/guides/integrations-guide/vue/) and supports [installing NPM packages](/en/guides/imports/#npm-packages), including several for Vue. You will be able to write Vue UI components, and may be able to keep some or all of your existing Gridsome Vue components and dependencies.

* Astro and Gridsome both allow you to use a [headless CMS, APIs or Markdown files for data](/en/guides/data-fetching/). You can continue to use your preferred content authoring system, and will be able to keep your existing content.

## Key Differences between Gridsome and Astro

[Section titled “Key Differences between Gridsome and Astro”](#key-differences-between-gridsome-and-astro)

When you rebuild your Gridsome site in Astro, you will notice some important differences:

* Gridsome is a Vue-based single-page application (SPA). Astro sites are multi-page apps built using [`.astro` components](/en/basics/astro-components/), but can also support [React, Preact, Vue.js, Svelte, SolidJS, AlpineJS](/en/guides/framework-components/) and raw HTML templating.

* As an SPA, Gridsome uses `vue-router` for SPA routing, and `vue-meta` for managing `<head>`. In Astro, you will create separate HTML pages and control your page `<head>` directly, or in a [layout component](/en/basics/layouts/).

* [Local file data](/en/guides/imports/): Gridsome uses GraphQL to retrieve data from your project files. Astro uses ESM imports and [`import.meta.glob()`](/en/guides/imports/#importmetaglob) to import data from local project files. Remote resources can be loaded using the standard `fetch()` API. GraphQL may be optionally added to your project, but is not included by default.

## Switch from Gridsome to Astro

[Section titled “Switch from Gridsome to Astro”](#switch-from-gridsome-to-astro)

To convert a Gridsome blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

Bring your existing Markdown (or MDX, with our optional integration) files as content to [create Markdown or MDX pages](/en/guides/markdown-content/).

Since Gridsome’s project structure is similar to Astro’s, you may be able to copy several existing files from your project into the same location in your new Astro project. However, the two project structures are not identical. You may want to examine [Astro’s project structure](/en/basics/project-structure/) to see what the differences are.

Since Astro queries and imports your local files differently than Gridsome, you may want to read about how to load files using [`import.meta.glob()`](/en/guides/imports/#importmetaglob) to understand how to work with your local files.

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Migration from Gridsome to Astro ](https://fyodor.io/migration-from-gridsome-to-astro/)

[Hello Astro! ](https://thamas.hu/astro-hello)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Gridsome site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-gridsome.mdx)!

# Migrating from Hugo

> Tips for migrating an existing Hugo project to Astro

[Hugo](https://gohugo.io) is an open-source static site generator built on Go.

## Key Similarities between Hugo and Astro

[Section titled “Key Similarities between Hugo and Astro”](#key-similarities-between-hugo-and-astro)

Hugo and Astro share some similarities that will help you migrate your project:

* Hugo and Astro are both modern static-site generators, ideally suited to [content-driven websites](/en/concepts/why-astro/#content-driven) like blogs.

* Hugo and Astro both allow you to [author your content in Markdown](/en/guides/markdown-content/). However, Hugo includes several special frontmatter properties and allows you to write frontmatter in YAML, TOML or JSON. Even though many of your existing Hugo frontmatter properties will not be “special” in Astro, you can continue to use your existing Markdown files and YAML (or TOML) frontmatter values.

* Hugo and Astro both allow you to enhance your site with a variety of [integrations and external packages](https://astro.build/integrations/).

## Key Differences between Hugo and Astro

[Section titled “Key Differences between Hugo and Astro”](#key-differences-between-hugo-and-astro)

When you rebuild your Hugo site in Astro, you will notice some important differences:

* Hugo uses Go Templating for page templating. [Astro syntax](/en/basics/astro-components/) is a JSX-like superset of HTML.

* Astro does not use shortcodes for dynamic content in standard Markdown files, but [Astro’s MDX integration](/en/guides/integrations-guide/mdx/) does allow you to use JSX and import components in MDX files.

* While Hugo can use “partials” for reusable layout elements, [Astro is entirely component-based](/en/basics/astro-components/). Any `.astro` file can be a component, a layout or an entire page, and can import and render any other Astro components. Astro components can also include [other UI framework components (e.g. React, Svelte, Vue, Solid)](/en/guides/framework-components/) as well as content or metadata from [other files in your project](/en/guides/imports/), such as Markdown or MDX.

## Switch from Hugo to Astro

[Section titled “Switch from Hugo to Astro”](#switch-from-hugo-to-astro)

To convert a Hugo blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

Bring your existing Markdown (or MDX, with our optional integration) files as content to [create Markdown or MDX pages](/en/guides/markdown-content/). Astro allows YAML or TOML frontmatter in these files, so if you are using JSON frontmatter, you will need to convert it.

To continue to use dynamic content such as variables, expressions or UI components within your Markdown content, add Astro’s optional MDX integration and convert your existing Markdown files to [MDX pages](/en/guides/markdown-content/). MDX supports YAML and TOML frontmatter, so you can keep your existing frontmatter properties. But, you must replace any shortcode syntax with [MDX’s own syntax](https://mdxjs.com/docs/what-is-mdx/#mdx-syntax), which allows JSX expressions and/or component imports.

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Elio Struyf's migration story from Hugo to Astro ](https://www.eliostruyf.com/migration-story-hugo-astro/)

[Hugo Vs Astro - Which Static Site Generator To Choose In 2023 ](https://onebite.dev/hugo-vs-astro-which-static-site-generator-to-choose-in-2023/)

[Lessons from an AI-assisted migration to Astro ](https://bennet.org/blog/lessons-from-ai-assisted-migration-to-astro/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Hugo site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-hugo.mdx)!

# Migrating from Jekyll

> Tips for migrating an existing Jekyll project to Astro

[Jekyll](https://jekyllrb.com) is a static site generator built on Ruby.

## Key Similarities between Jekyll and Astro

[Section titled “Key Similarities between Jekyll and Astro”](#key-similarities-between-jekyll-and-astro)

Jekyll and Astro share some similarities that will help you migrate your project:

* Both Jekyll and Astro are static-site generators, commonly used to create blogs.

* Both Jekyll and Astro allow you to write your content in Markdown and HTML. Jekyll and Astro both provide some special frontmatter YAML properties for page layout and unpublished draft posts. You can continue to use your existing Markdown files in Astro.

* Both Jekyll and Astro use [file-based routing](/en/guides/routing/) for creating pages from your blog posts. Astro provides a [special `src/pages/` directory for all pages and posts](/en/basics/project-structure/#srcpages). Jekyll uses a similar special folder called `_posts/` for your Markdown blog posts, however your site pages can exist elsewhere. Creating new blog posts should feel familiar.

## Key Differences between Jekyll and Astro

[Section titled “Key Differences between Jekyll and Astro”](#key-differences-between-jekyll-and-astro)

When you rebuild your Jekyll site in Astro, you will notice some important differences:

* As Jekyll is primarily a blogging platform, several blog features are built-in that you may have to build yourself in Astro. Or, choose a [blog starter template theme](https://astro.build/themes?search=\&categories%5B%5D=blog) that includes these features. For example, Jekyll has built-in support for tags and categories which you will find in several Astro blog themes, but is not included in a minimal Astro project.

* Jekyll uses Liquid templates for reusable layout elements and templating. Astro uses JSX-like [`.astro` files for templating and components](/en/basics/astro-components/). Any `.astro` file can be a component, a layout or an entire page, and can import and render any other Astro components. You can also build using [other UI framework components (e.g. React, Svelte, Vue, Solid)](/en/guides/framework-components/) as well as content or metadata from [other files in your project](/en/guides/imports/), such as Markdown or MDX.

## Switch from Jekyll to Astro

[Section titled “Switch from Jekyll to Astro”](#switch-from-jekyll-to-astro)

To convert a Jekyll blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

Bring your existing Markdown files as content to [create Markdown pages](/en/guides/markdown-content/), using an [Astro Markdown layout](/en/basics/layouts/#markdown-layouts) instead of a Liquid template.

Much of your existing HTML page content can be converted into [Astro pages](/en/basics/astro-pages/), and you will additionally be able to [use variables, JSX-like expressions and component imports directly in your HTML templating](/en/reference/astro-syntax/#jsx-like-expressions).

Astro does not have a `permalink` property that accepts placeholders. You may need to read more about [Astro’s page routing](/en/guides/routing/) if you want to keep your existing URL structure. Or, consider [setting redirects at a host like Netlify](https://docs.netlify.com/routing/redirects/).

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[From Jekyll to Astro ](https://jackcarey.co.uk/posts/astro-rewrite/)

[Goodbye Jekyll, Hello Astro ](https://kiranrao.in/blog/bye-jekyll-hello-astro/)

[Back to the Future: Our Tech Blog's Transition from Jekyll to Astro ](https://alasco.tech/2023/09/06/migrating-to-astro)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Jekyll site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-jekyll.mdx)!

# Migrating from Next.js

> Tips for migrating an existing Next.js project to Astro

Here are some key concepts and migration strategies to help you get started. Use the rest of our docs and our [Discord community](https://astro.build/chat) to keep going!

## Key Similarities between Next.js and Astro

[Section titled “Key Similarities between Next.js and Astro”](#key-similarities-between-nextjs-and-astro)

Next.js and Astro share some similarities that will help you migrate your project:

* The [syntax of `.astro` files is similar to JSX](/en/reference/astro-syntax/#differences-between-astro-and-jsx). Writing Astro should feel familiar.
* Astro projects can also be SSG or [SSR with page-level prerendering](/en/guides/on-demand-rendering/).
* Astro uses file-based routing, and [allows specially named pages to create dynamic routes](/en/guides/routing/#dynamic-routes).
* Astro is [component-based](/en/basics/astro-components/), and your markup structure will be similar before and after your migration.
* Astro has [official integrations for React, Preact, and Solid](/en/guides/integrations-guide/react/) so you can use your existing JSX components. Note that in Astro, these files **must** have a `.jsx` or `.tsx` extension.
* Astro has support for [installing NPM packages](/en/guides/imports/#npm-packages), including React libraries. Many of your existing dependencies will work in Astro.

## Key Differences between Next.js and Astro

[Section titled “Key Differences between Next.js and Astro”](#key-differences-between-nextjs-and-astro)

When you rebuild your Next.js site in Astro, you will notice some important differences:

* Next.js is a React single-page app, and uses `index.js` as your project’s root. Astro is a multi-page site, and `index.astro` is your home page.

* [`.astro` components](/en/basics/astro-components/) are not written as exported functions that return page templating. Instead, you’ll split your code into a “code fence” for your JavaScript and a body exclusively for the HTML you generate.

* [content-driven](/en/concepts/why-astro/#content-driven): Astro was designed to showcase your content and to allow you to opt-in to interactivity only as needed. An existing Next.js app might be built for high client-side interactivity and may require advanced Astro techniques to include items that are more challenging to replicate using `.astro` components, such as dashboards.

## Convert your Next.js Project

[Section titled “Convert your Next.js Project”](#convert-your-nextjs-project)

Each project migration will look different, but there are some common actions you will perform when converting from Next.js to Astro.

### Create a new Astro project

[Section titled “Create a new Astro project”](#create-a-new-astro-project)

Use the `create astro` command for your package manager to launch Astro’s CLI wizard or choose a community theme from the [Astro Theme Showcase](https://astro.build/themes).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters (e.g. `docs`, `blog`, `portfolio`). Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  # launch the Astro CLI Wizard
  npm create astro@latest


  # create a new project with an official example
  npm create astro@latest -- --template <example-name>
  ```

* pnpm

  ```shell
  # launch the Astro CLI Wizard
  pnpm create astro@latest


  # create a new project with an official example
  pnpm create astro@latest --template <example-name>
  ```

* Yarn

  ```shell
  # launch the Astro CLI Wizard
  yarn create astro@latest


  # create a new project with an official example
  yarn create astro@latest --template <example-name>
  ```

Then, copy your existing Next project files over to your new Astro project in a separate folder outside of `src`.

Tip

Visit <https://astro.new> for the full list of official starter templates, and links for opening a new project in StackBlitz or CodeSandbox.

### Install integrations (optional)

[Section titled “Install integrations (optional)”](#install-integrations-optional)

You may find it useful to install some of [Astro’s optional integrations](/en/guides/integrations/) to use while converting your Next project to Astro:

* **@astrojs/react**: to reuse some existing React UI components in your new Astro site, or keep writing with React components.

* **@astrojs/mdx**: to bring existing MDX files from your Next project, or to use MDX in your new Astro site.

### Put your source code in `src`

[Section titled “Put your source code in src”](#put-your-source-code-in-src)

Following [Astro’s project structure](/en/basics/project-structure/):

1. **Keep** Next’s `public/` folder untouched.

   Astro uses the `public/` directory for static assets, just like Next. There is no change needed to this folder, nor its contents.

2. **Copy or Move** Next’s other files and folders (e.g. `pages`, `styles` etc.) into Astro’s `src/` folder as you rebuild your site, following [Astro’s project structure](/en/basics/project-structure/).

   Like Next, Astro’s `src/pages/` folder is a special folder used for file-based routing. All other folders are optional, and you can organize the contents of your `src/` folder any way you like. Other common folders in Astro projects include `src/layouts/`, `src/components`, `src/styles`, `src/scripts`.

### The Astro config file

[Section titled “The Astro config file”](#the-astro-config-file)

Astro has a configuration file at the root of your project called [`astro.config.mjs`](/en/guides/configuring-astro/). This is used only for configuring your Astro project and any installed integrations, including [SSR adapters](/en/guides/deploy/).

### Tips: Convert JSX files to `.astro` files

[Section titled “Tips: Convert JSX files to .astro files”](#tips-convert-jsx-files-to-astro-files)

Here are some tips for converting a Next `.js` component into a `.astro` component:

1. Use the returned JSX of the existing Next.js component function as the basis for your HTML template.

2. Change any [Next or JSX syntax to Astro](#reference-convert-nextjs-syntax-to-astro) or to HTML web standards. This includes `<Link>`, `<Script>`, `{children}`, and `className`, for example.

3. Move any necessary JavaScript, including import statements, into a [“code fence” (`---`)](/en/basics/astro-components/#the-component-script). Note: JavaScript to [conditionally render content](/en/reference/astro-syntax/#dynamic-html) is often written inside the HTML template directly in Astro.

4. Use [`Astro.props`](/en/reference/api-reference/#props) to access any additional props that were previously passed to your Next function.

5. Decide whether any imported components also need to be converted to Astro. With the official integration installed, you can [use existing React components in your Astro file](/en/guides/framework-components/). But, you may want to convert them to `.astro` components, especially if they do not need to be interactive!

6. Replace `getStaticProps()` with import statements or [`import.meta.glob()`](/en/guides/imports/#importmetaglob) to query your local files. Use `fetch()` to fetch external data.

See [an example of a Next `.js` file converted step-by-step](#guided-example-next-data-fetching-to-astro).

#### Compare: JSX vs Astro

[Section titled “Compare: JSX vs Astro”](#compare-jsx-vs-astro)

Compare the following Next component and a corresponding Astro component:

* JSX

  StarCount.jsx

  ```jsx
  import Header from "./header";
  import Footer from "./footer";
  import "./layout.css";


  export async function getStaticProps() {
      const res = await fetch("https://api.github.com/repos/withastro/astro");
      const json = await res.json();
      return {
          props: { message: json.message, stars: json.stargazers_count || 0 },
      }
  }


  const Component = ({ stars, message }) => {


      return (
          <>
              <Header />
              <p style={{
                  backgroundColor: `#f4f4f4`,
                  padding: `1em 1.5em`,
                  textAlign: `center`,
                  marginBottom: `1em`
              }}>Astro has {stars} 🧑‍🚀</p>
              <Footer />
          </>
      )
  }


  export default Component;
  ```

* Astro

  StarCount.astro

  ```astro
  ---
  import Header from "./header";
  import Footer from "./footer";
  import "./layout.css";


  const res = await fetch("https://api.github.com/repos/withastro/astro");
  const json = await res.json();
  const message = json.message;
  const stars = json.stargazers_count || 0;
  ---
  <Header />
  <p class="banner">Astro has {stars} 🧑‍🚀</p>
  <Footer />


  <style>
    .banner {
      background-color: #f4f4f4;
      padding: 1em 1.5em;
      text-align: center;
      margin-bottom: 1em;
    }
  </style>
  ```

### Migrating Layout Files

[Section titled “Migrating Layout Files”](#migrating-layout-files)

You may find it helpful to start by converting your Next.js layouts and templates into [Astro layout components](/en/basics/layouts/).

Next has two different methods for creating layout files, each of which handles layouts differently than Astro:

* The `pages` directory

* [The `/app` directory](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#layouts)

Each Astro page explicitly requires `<html>`, `<head>`, and `<body>` tags to be present, so it is common to reuse a layout file across pages. Astro uses a [`<slot />`](/en/basics/astro-components/#slots) for page content, with no import statement required. Note the standard HTML templating, and direct access to `<head>`:

src/layouts/Layout.astro

```astro
---
---
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro</title>
  </head>
  <body>
    <!-- Wrap the slot element with your existing layout templating -->
    <slot />
  </body>
</html>
```

#### Migrating from Next.js’ `pages` directory

[Section titled “Migrating from Next.js’ pages directory”](#migrating-from-nextjs-pages-directory)

Your Next project may have a `pages/_document.jsx` file that imports React components to customize your app’s `<head>`:

pages/\_document.jsx

```jsx
import Document, { Html, Head, Main, NextScript } from "next/document";


export default class MyDocument extends Document {
  render() {
    return (
      <Html lang="en">
        <Head>
          <link rel="icon" href="/favicon.ico" />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}
```

1. Make a new Astro layout file using only the returned JSX.

2. Replace any React components with `<html>`, `<head>`, `<slot>`, and other HTML standard tags.

   src/layouts/Document.astro

   ```astro
   <html lang="en">
     <head>
         <link rel="icon" href="/favicon.ico" />
     </head>
     <body>
       <slot/>
     </body>
   </html>
   ```

#### Migrating from Next.js’ `/app` directory

[Section titled “Migrating from Next.js’ /app directory”](#migrating-from-nextjs-app-directory)

Next.js’ `app/` directory layout files are created with two files: a `layout.jsx` file to customize the `<html>` and `<body>` contents, and a `head.jsx` file to customize the `<head>` element contents.

app/layout.jsx

```jsx
export default function Layout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

app/head.jsx

```jsx
export default function Head() {
  return (
    <>
      <title>My Page</title>
    </>
  );
}
```

1. Make a new Astro layout file using only the returned JSX.

2. Replace both these files with a single Astro layout file that contains a page shell (`<html>`, `<head>`, and `<body>` tags) and a `<slot/>` instead of React’s `{children}` prop:

   src/layouts/Layout.astro

   ```astro
   <html lang="en">
     <head>
         <title>My Page</title>
     </head>
     <body>
       <slot/>
     </body>
   </html>
   ```

### Migrating Pages and Posts

[Section titled “Migrating Pages and Posts”](#migrating-pages-and-posts)

In Next.js, your posts either live in `/pages` or `/app/routeName/page.jsx`.

In Astro, all your page content must live within `src/` unless you are using [content collections](/en/guides/content-collections/).

#### React pages

[Section titled “React pages”](#react-pages)

Your existing Next JSX (`.js`) pages will need to be [converted from JSX files to `.astro` pages](#tips-convert-jsx-files-to-astro-files). You cannot use an existing JSX page file in Astro.

These [`.astro` pages](/en/basics/astro-pages/) must be located within `src/pages/` and will have page routes generated automatically based on their file path.

#### Markdown and MDX pages

[Section titled “Markdown and MDX pages”](#markdown-and-mdx-pages)

Astro has built-in support for Markdown and an optional integration for MDX files. You can reuse any existing [Markdown and MDX files](/en/guides/markdown-content/), but they may require some adjustments to their frontmatter, such as adding [Astro’s special `layout` frontmatter property](/en/basics/layouts/#markdown-layouts). You will no longer need to manually create pages for each Markdown-generated route. These files can be placed within `src/pages/` to take advantage of automatic file-based routing.

Alternatively, you can use [content collections](/en/guides/content-collections/) in Astro to store and manage your content. You will retrieve the content yourself and [generate those pages dynamically](/en/guides/content-collections/#generating-routes-from-content).

### Migrating Tests

[Section titled “Migrating Tests”](#migrating-tests)

As Astro outputs raw HTML, it is possible to write end-to-end tests using the output of the build step. Any end-to-end tests written previously might work out-of-the-box if you have been able to match the markup of your Next site. Testing libraries such as Jest and React Testing Library can be imported and used in Astro to test your React components.

See Astro’s [testing guide](/en/guides/testing/) for more.

## Reference: Convert Next.js Syntax to Astro

[Section titled “Reference: Convert Next.js Syntax to Astro”](#reference-convert-nextjs-syntax-to-astro)

### Next Links to Astro

[Section titled “Next Links to Astro”](#next-links-to-astro)

Convert any Next `<Link to="">`, `<NavLink>` etc. components to HTML `<a href="">` tags.

```diff
-<Link to="/blog">Blog</Link>
<a href="/blog">Blog</a>
```

Astro does not use any special component for links, although you are welcome to build your own `<Link>` component. You can then import and use this `<Link>` just as you would any other component.

src/components/Link.astro

```astro
---
const { to } = Astro.props;
---
<a href={to}><slot /></a>
```

### Next Imports to Astro

[Section titled “Next Imports to Astro”](#next-imports-to-astro)

Update any [file imports](/en/guides/imports/) to reference relative file paths exactly. This can be done using [import aliases](/en/guides/typescript/#import-aliases), or by writing out a relative path in full.

Note that `.astro` and several other file types must be imported with their full file extension.

src/pages/authors/Fred.astro

```astro
---
import Card from "../../components/Card.astro";
---
<Card />
```

### Next Children Props to Astro

[Section titled “Next Children Props to Astro”](#next-children-props-to-astro)

Convert any instances of `{children}` to an Astro `<slot />`. Astro does not need to receive `{children}` as a function prop and will automatically render child content in a `<slot />`.

src/components/MyComponent.astro

```diff
---
---
-export default function MyComponent(props) {
    -return (
      <div>
        -{props.children}
      </div>
-    );
-}


<div>
  <slot />
</div>
```

React components that pass multiple sets of children can be migrated to an Astro component using [named slots](/en/basics/astro-components/#named-slots).

See more about [specific `<slot />` usage in Astro](/en/basics/astro-components/#slots).

### Next Data Fetching to Astro

[Section titled “Next Data Fetching to Astro”](#next-data-fetching-to-astro)

Convert any instances of `getStaticProps()` to either `import.meta.glob()` or `getCollection()`/`getEntry()` in order to access data from other files in your project source. To [fetch remote data](/en/guides/data-fetching/), use `fetch()`.

These data requests are made in the frontmatter of the Astro component and use top-level await.

src/pages/index.astro

```astro
---
import { getCollection } from 'astro:content';


// Get all `src/content/blog/` entries
const allBlogPosts = await getCollection('blog');


// Get all `src/pages/posts/` entries
const allPosts = Object.values(import.meta.glob('../pages/posts/*.md', { eager: true }));


const response = await fetch('https://randomuser.me/api/');
const data = await response.json();
const randomUser = data.results[0];
---
```

See more about local files imports with [`import.meta.glob()`](/en/guides/imports/#importmetaglob), [querying with content collections](/en/guides/content-collections/#querying-build-time-collections) or [fetching remote data](/en/guides/data-fetching/).

### Next Styling to Astro

[Section titled “Next Styling to Astro”](#next-styling-to-astro)

You may need to replace any [CSS-in-JS libraries](https://github.com/withastro/astro/issues/4432) (e.g. styled-components) with other available CSS options in Astro.

If necessary, convert any inline style objects (`style={{ fontWeight: "bold" }}`) to inline HTML style attributes (`style="font-weight:bold;"`). Or, use an [Astro `<style>` tag](/en/guides/styling/#styling-in-astro) for scoped CSS styles.

src/components/Card.astro

```diff
<div style={{backgroundColor: `#f4f4f4`, padding: `1em`}}>{message}</div>
<div style="background-color: #f4f4f4; padding: 1em;">{message}</div>
```

Tailwind is supported after installing the [Tailwind Vite plugin](/en/guides/styling/#tailwind). No changes to your existing Tailwind code are required!

See more about [Styling in Astro](/en/guides/styling/).

### Next Image Plugin to Astro

[Section titled “Next Image Plugin to Astro”](#next-image-plugin-to-astro)

Convert any Next `<Image />` components to [Astro’s own image component](/en/guides/images/) in `.astro` or `.mdx` files, or to a [standard HTML `<img>` / JSX `<img />`](/en/guides/images/#images-in-ui-framework-components) tag as appropriate in your React components.

Astro’s `<Image />` component works in `.astro` and `.mdx` files only. See a [full list of its component attributes](/en/reference/modules/astro-assets/#image-) and note that several will differ from Next’s attributes.

src/pages/index.astro

```astro
---
import { Image } from 'astro:assets';
import rocket from '../assets/rocket.png';
---
<Image src={rocket} alt="A rocketship in space." />
<img src={rocket.src} alt="A rocketship in space.">
```

In React (`.jsx`) components, use standard JSX image syntax (`<img />`). Astro will not optimize these images, but you can install and use NPM packages for more flexibility.

You can learn more about [using images in Astro](/en/guides/images/) in the Images Guide.

## Guided example: Next data fetching to Astro

[Section titled “Guided example: Next data fetching to Astro”](#guided-example-next-data-fetching-to-astro)

Here is an example of Next.js Pokédex data fetch converted to Astro.

`pages/index.js` fetches and displays a list of the first 151 Pokémon using [the REST PokéAPI](https://pokeapi.co/).

Here’s how to recreate that in `src/pages/index.astro`, replacing `getStaticProps()` with `fetch()`.

1. Identify the return() JSX.

   pages/index.js

   ```jsx
   import Link from 'next/link'
   import styles from '../styles/poke-list.module.css';


   export default function Home({ pokemons }) {
       return (
           <>
               <ul className={`plain-list ${styles.pokeList}`}>
                   {pokemons.map((pokemon) => (
                       <li className={styles.pokemonListItem} key={pokemon.name}>
                           <Link className={styles.pokemonContainer} as={`/pokemon/${pokemon.name}`} href="/pokemon/[name]">
                               <p className={styles.pokemonId}>No. {pokemon.id}</p>
                               <img className={styles.pokemonImage} src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`} alt={`${pokemon.name} picture`}></img>
                               <h2 className={styles.pokemonName}>{pokemon.name}</h2>
                           </Link>
                       </li>
                   ))}
               </ul>
           </>
       )
   }


   export const getStaticProps = async () => {
       const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
       const resJson = await res.json();
       const pokemons = resJson.results.map(pokemon => {
           const name = pokemon.name;
           // https://pokeapi.co/api/v2/pokemon/1/
           const url = pokemon.url;
           const id = url.split("/")[url.split("/").length - 2];
           return {
               name,
               url,
               id
           }
       });
       return {
           props: {
               pokemons,
           },
       }
   }
   ```

2. Create `src/pages/index.astro`

   Use the return value of the Next function. Convert any Next or React syntax to Astro, including changing the case of any [HTML global attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes).

   Note that:

   * `.map` just works!

   * `className` becomes `class`.

   * `<Link>` becomes `<a>`.

   * The `<> </>` fragment is not required in Astro templating.

   * `key` is a React attribute, and is not an attribute of `li` in Astro.

   src/pages/index.astro

   ```astro
   ---
   ---
   <ul class="plain-list pokeList">
       {pokemons.map((pokemon) => (
           <li class="pokemonListItem">
               <a class="pokemonContainer" href={`/pokemon/${pokemon.name}`}>
                   <p class="pokemonId">No. {pokemon.id}</p>
                   <img class="pokemonImage" src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`} alt={`${pokemon.name} picture`}/>
                   <h2 class="pokemonName">{pokemon.name}</h2>
               </a>
           </li>
       ))}
   </ul>
   ```

3. Add any needed imports, props, and JavaScript

   Note that:

   * the `getStaticProps` function is no longer needed. Data from the API is fetched directly in the code fence.
   * A `<Layout>` component is imported and wraps the page templating.

   src/pages/index.astro

   ```diff
   ---
   +import Layout from '../layouts/layout.astro';


   +const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151");
   +const resJson = await res.json();
   +const pokemons = resJson.results.map(pokemon => {
       const name = pokemon.name;
       +// https://pokeapi.co/api/v2/pokemon/1/
       const url = pokemon.url;
       const id = url.split("/")[url.split("/").length - 2];
       return {
           +name,
           +url,
           +id
       }
   +});
   ---


   +<Layout>
     <ul class="plain-list pokeList">
         {pokemons.map((pokemon) => (
             <li class="pokemonListItem" key={pokemon.name}>
                 <a class="pokemonContainer" href={`/pokemon/${pokemon.name}`}>
                     <p class="pokemonId">No. {pokemon.id}</p>
                     <img class="pokemonImage" src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`} alt={`${pokemon.name} picture`}/>
                     <h2 class="pokemonName">{pokemon.name}</h2>
                 </a>
             </li>
         ))}
     </ul>
   +</Layout>
   ```

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Why we switched to Astro (and why it might interest you) ](https://www.datocms.com/blog/why-we-switched-to-astro)

[Migrating from Next.js to Astro ](https://johnzanussi.com/posts/nextjs-to-astro-migration)

[From NextJS to Astro ](https://vanntile.com/blog/next-to-astro)

[Converting Next.js to Astro ](https://ericclemmons.com/blog/converting-nextjs-to-astro)

[Migrating to Astro (from Next.js) ](https://www.raygesualdo.com/posts/migrating-to-astro-the-beginning/)

[Astro.js as an alternative to Next.js ](https://www.railyard.works/blog/astro-as-alternative-to-next)

[Why I Switched My Website from Next.js to Astro ](https://praveenjuge.com/blog/why-i-switched-my-website-from-nextjs-to-astro/)

[NextJS to Astro: more control = faster sites ](https://www.youtube.com/watch?v=PSzCtdM20Fc)

[How Astro made my site 100x faster ](https://www.youtube.com/watch?v=cOxA3kMYtkM)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Next.js site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-nextjs.mdx)!

# Migrating from NuxtJS

> Tips for migrating an existing NuxtJS project to Astro

Here are some key concepts and migration strategies to help you get started. Use the rest of our docs and our [Discord community](https://astro.build/chat) to keep going!

> This guide is referring to [Nuxt 2](https://nuxtjs.org/), not the newer Nuxt 3. While some of the concepts are similar, Nuxt 3 is a newer version of the framework and may require different strategies for parts of your migration.

## Key Similarities between Nuxt and Astro

[Section titled “Key Similarities between Nuxt and Astro”](#key-similarities-between-nuxt-and-astro)

Nuxt and Astro share some similarities that will help you migrate your project:

* Astro projects can also be SSG or [SSR with page level prerendering](/en/guides/on-demand-rendering/).
* Astro uses file-based routing, and [allows specially named pages to create dynamic routes](/en/guides/routing/#dynamic-routes).
* Astro is [component-based](/en/basics/astro-components/), and your markup structure will be similar before and after your migration.
* Astro has [an official integration for using Vue components](/en/guides/integrations-guide/vue/).
* Astro has support for [installing NPM packages](/en/guides/imports/#npm-packages), including Vue libraries. You may be able to keep some or all of your existing Vue components and dependencies.

## Key Differences between Nuxt and Astro

[Section titled “Key Differences between Nuxt and Astro”](#key-differences-between-nuxt-and-astro)

When you rebuild your Nuxt site in Astro, you will notice some important differences:

* Nuxt is a Vue-based SPA (single-page application). Astro sites are multi-page apps built using `.astro` components, but can also support React, Preact, Vue.js, Svelte, SolidJS, AlpineJS, and raw HTML templating.

* [Page Routing](/en/basics/astro-pages/#file-based-routing): Nuxt uses `vue-router` for SPA routing, and `vue-meta` for managing `<head>`. In Astro, you will create separate HTML page routes and control your page `<head>` directly, or in a layout component.

* [content-driven](/en/concepts/why-astro/#content-driven): Astro was designed to showcase your content and to allow you to opt-in to interactivity only as needed. An existing Nuxt app may be built for high client-side interactivity. Astro has built-in capabilities for working with your content, such as page generation, but may require advanced Astro techniques to include items that are more challenging to replicate using `.astro` components, such as dashboards.

## Convert your NuxtJS Project

[Section titled “Convert your NuxtJS Project”](#convert-your-nuxtjs-project)

Each project migration will look different, but there are some common actions you will perform when converting from Nuxt to Astro.

### Create a new Astro project

[Section titled “Create a new Astro project”](#create-a-new-astro-project)

Use the `create astro` command for your package manager to launch Astro’s CLI wizard or choose a community theme from the [Astro Theme Showcase](https://astro.build/themes).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters (e.g. `docs`, `blog`, `portfolio`). Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  # launch the Astro CLI Wizard
  npm create astro@latest


  # create a new project with an official example
  npm create astro@latest -- --template <example-name>
  ```

* pnpm

  ```shell
  # launch the Astro CLI Wizard
  pnpm create astro@latest


  # create a new project with an official example
  pnpm create astro@latest --template <example-name>
  ```

* Yarn

  ```shell
  # launch the Astro CLI Wizard
  yarn create astro@latest


  # create a new project with an official example
  yarn create astro@latest --template <example-name>
  ```

Then, copy your existing Nuxt project files over to your new Astro project in a separate folder outside of `src`.

Tip

Visit <https://astro.new> for the full list of official starter templates, and links for opening a new project in StackBlitz or CodeSandbox.

### Install integrations (optional)

[Section titled “Install integrations (optional)”](#install-integrations-optional)

You may find it useful to install some of [Astro’s optional integrations](/en/guides/integrations/) to use while converting your Nuxt project to Astro:

* **@astrojs/vue**: to reuse some existing Vue UI components in your new Astro site, or keep writing with Vue components.

* **@astrojs/mdx**: to bring existing MDX files from your Nuxt project, or to use MDX in your new Astro site.

### Put your source code in `src`

[Section titled “Put your source code in src”](#put-your-source-code-in-src)

1. **Move** the contents of Nuxt’s `static/` folder into `public/`.

   Astro uses the `public/` directory for static assets, similar to Nuxt’s `static/` folder.

2. **Copy or Move** Nuxt’s other files and folders (e.g. `pages`, `layouts` etc.) into Astro’s `src/` folder.

   Like Nuxt, Astro’s `src/pages/` folder is a special folder used for file-based routing. All other folders are optional, and you can organize the contents of your `src/` folder any way you like. Other common folders in Astro projects include `src/layouts/`, `src/components`, `src/styles`, `src/scripts`.

### Convert Vue SFC pages to `.astro` files

[Section titled “Convert Vue SFC pages to .astro files”](#convert-vue-sfc-pages-to-astro-files)

Here are some tips for converting a Nuxt `.vue` component into a `.astro` component:

1. Use the `<template>` of the existing NuxtJS component function as the basis for your HTML template.

2. Change any [Nuxt or Vue syntax to Astro](#reference-convert-nuxtjs-syntax-to-astro) or to HTML web standards. This includes `<NuxtLink>`, `:class`, `{{variable}}`, and `v-if`, for example.

3. Move `<script>` JavaScript, into a “code fence” (`---`). Convert your component’s data-fetching properties to server-side JavaScript - see [Nuxt data fetching to Astro](#nuxt-data-fetching-to-astro).

4. Use `Astro.props` to access any additional props that were previously passed to your Vue component.

5. Decide whether any imported components also need to be converted to Astro. With the official integration installed, you can [use existing Vue components in your Astro file](/en/guides/integrations-guide/vue/). But, you may want to convert them to Astro, especially if they do not need to be interactive!

See [an example from a Nuxt app converted step-by-step](#guided-example-see-the-steps).

#### Compare: Vue vs Astro

[Section titled “Compare: Vue vs Astro”](#compare-vue-vs-astro)

Compare the following Nuxt component and a corresponding Astro component:

* Vue

  Page.vue

  ```vue
  <template>
    <div>
      <p v-if="message === 'Not found'">
        The repository you're looking up doesn't exist
      </p>
      <div v-else>
        <Header/>
        <p class="banner">Astro has {{stars}} 🧑‍🚀</p>
        <Footer />
      </div>
    </div>
  </template>


  <script>
  import Vue from 'vue'


  export default Vue.extend({
    name: 'IndexPage',
    async asyncData() {
      const res = await fetch('https://api.github.com/repos/withastro/astro')
      const json = await res.json();
      return {
        message: json.message,
        stars: json.stargazers_count || 0,
      };
    }
  });
  </script>


  <style scoped>
  .banner {
    background-color: #f4f4f4;
    padding: 1em 1.5em;
    text-align: center;
    margin-bottom: 1em;
  }
  </style>
  ```

* Astro

  Page.astro

  ```astro
  ---
  import Header from "./header";
  import Footer from './footer';
  import "./layout.css";


  const res = await fetch('https://api.github.com/repos/withastro/astro')
  const json = await res.json()
  const message = json.message;
  const stars = json.stargazers_count || 0;
  ---


  {message === "Not Found" ?
        <p>The repository you're looking up doesn't exist</p> :
        <>
            <Header />
            <p class="banner">Astro has {stars} 🧑‍🚀</p>
            <Footer />
        </>
  }


  <style>
    .banner {
      background-color: #f4f4f4;
      padding: 1em 1.5em;
      text-align: center;
      margin-bottom: 1em;
    }
  </style>
  ```

### Migrating Layout Files

[Section titled “Migrating Layout Files”](#migrating-layout-files)

You may find it helpful to start by converting your Nuxt layouts and templates into [Astro layout components](/en/basics/layouts/).

Each Astro page explicitly requires `<html>`, `<head>`, and `<body>` tags to be present. Your Nuxt `layout.vue` and templates will not include these.

Note the standard HTML templating, and direct access to `<head>`:

src/layouts/Layout.astro

```astro
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <title>Astro</title>
  </head>
  <body>
    <!-- Wrap the slot element with your existing layout templating -->
    <slot />
  </body>
</html>
```

You may also wish to reuse code from [your Nuxt’s page’s `head` property](https://nuxtjs.org/docs/configuration-glossary/configuration-head/#the-head-property) to include additional site metadata. Notice that Astro uses neither `vue-meta` nor a component’s `head` property but instead creates `<head>` directly. You may import and use components, even within `<head>`, to separate and organize your page content.

### Migrating Pages and Posts

[Section titled “Migrating Pages and Posts”](#migrating-pages-and-posts)

In NuxtJS, your [pages](/en/basics/astro-pages/) live in `/pages`. In Astro, all your page content must live within `src/` unless you are using [content collections](/en/guides/content-collections/).

#### Vue Pages

[Section titled “Vue Pages”](#vue-pages)

Your existing Nuxt Vue (`.vue`) pages will need to be [converted from Vue files to `.astro` pages](#convert-vue-sfc-pages-to-astro-files). You cannot use an existing Vue page file in Astro.

These [`.astro` pages](/en/basics/astro-pages/) must be located within `src/pages/` and will have page routes generated automatically based on their file path.

##### Dynamic File Path Naming

[Section titled “Dynamic File Path Naming”](#dynamic-file-path-naming)

In Nuxt, your dynamic pages use an underscore to represent a dynamic page property that’s then passed to the page generation:

* pages/

  * pokemon/

    * \_name.vue

  * index.vue

* nuxt.config.js

To convert to Astro, change this underscored dynamic path property (e.g. `_name.vue`) to be wrapped in a pair of square brackets (e.g. `[name].astro`):

* src/

  * pages/

    * pokemon/

      * \[name].astro

    * index.astro

* astro.config.mjs

#### Markdown and MDX pages

[Section titled “Markdown and MDX pages”](#markdown-and-mdx-pages)

Astro has built-in support for Markdown and an optional integration for MDX files. You can reuse any existing Markdown and MDX pages, but they may require some adjustments to their frontmatter, such as adding [Astro’s special `layout` frontmatter property](/en/basics/layouts/#markdown-layouts).

You will no longer need to manually create pages for each Markdown-generated route or use an external package like `@nuxt/content`. These files can be placed within `src/pages/` to take advantage of automatic file-based routing.

When part of a [content collection](/en/guides/content-collections/), you will [generate pages dynamically](/en/guides/content-collections/#generating-routes-from-content) from your content entries.

### Migrating Tests

[Section titled “Migrating Tests”](#migrating-tests)

As Astro outputs raw HTML, it is possible to write end-to-end tests using the output of the build step. Any end-to-end tests written previously might work out-of-the-box, if you have been able to match the markup of your Nuxt site. Testing libraries such as Jest and Vue Testing Library can be imported and used in Astro to test your Vue components.

See Astro’s [testing guide](/en/guides/testing/) for more.

## Reference: Convert NuxtJS Syntax to Astro

[Section titled “Reference: Convert NuxtJS Syntax to Astro”](#reference-convert-nuxtjs-syntax-to-astro)

### Nuxt Local Variables to Astro

[Section titled “Nuxt Local Variables to Astro”](#nuxt-local-variables-to-astro)

To use local variables in an Astro component’s HTML, change the set of two curly braces to one set of curly braces:

src/components/Component.astro

```diff
---
const message = "Hello!"
---
<p>{{message}}</p>
<p>{message}</p>
```

### Nuxt Property Passing to Astro

[Section titled “Nuxt Property Passing to Astro”](#nuxt-property-passing-to-astro)

To bind an attribute or component property in an Astro component, change this syntax to the following:

src/components/Component.astro

```diff
---
---
<p v-bind:aria-label="message">...</p>
-<!-- Or -->
<p :aria-label="message">...</p>
-<!-- Also support component props -->
-<Header title="Page"/>


<p aria-label={message}>...</p>
+<!-- Also support component props -->
+<Header title={"Page"}/>
```

### Nuxt Links to Astro

[Section titled “Nuxt Links to Astro”](#nuxt-links-to-astro)

Convert any Nuxt `<NuxtLink to="">` components to HTML `<a href="">` tags.

```diff
-<NuxtLink to="/blog">Blog</Link>
<a href="/blog">Blog</a>
```

Astro does not use any special component for links, although you are welcome to build custom link components. You can then import and use this `<Link>` just as you would any other component.

src/components/Link.astro

```astro
---
const { to } = Astro.props
---
<a href={to}><slot /></a>
```

### Nuxt Imports to Astro

[Section titled “Nuxt Imports to Astro”](#nuxt-imports-to-astro)

If necessary, update any [file imports](/en/guides/imports/) to reference relative file paths exactly. This can be done using [import aliases](/en/guides/typescript/#import-aliases), or by writing out a relative path in full.

Note that `.astro` and several other file types must be imported with their full file extension.

src/pages/authors/Fred.astro

```astro
---
import Card from `../../components/Card.astro`;
---
<Card />
```

### Nuxt Dynamic Page Generation to Astro

[Section titled “Nuxt Dynamic Page Generation to Astro”](#nuxt-dynamic-page-generation-to-astro)

In Nuxt, to generate a dynamic page you either must:

* Use SSR.
* [Use the `generate` function in `nuxt.config.js`](https://nuxtjs.org/docs/configuration-glossary/configuration-generate/) to define all possible static routes.

In Astro, you similarly have two choices:

* [Use SSR](/en/guides/on-demand-rendering/).
* Export a `getStaticPaths()` function in the frontmatter of an Astro page to tell the framework which [static routes to generate dynamically](/en/guides/routing/#dynamic-routes).

#### Convert a `generate` function in Nuxt to a `getStaticPaths` function in Astro.

[Section titled “Convert a generate function in Nuxt to a getStaticPaths function in Astro.”](#convert-a-generate-function-in-nuxt-to-a-getstaticpaths-function-in-astro)

To generate multiple pages, replace the function to create routes in your `nuxt.config.js` with `getStaticPaths()` directly inside a dynamic routing page itself:

nuxt.config.js

```javascript
{
  // ...
    generate: {
        async routes() {
          // Axios is required here unless you're using Node 18
          const res = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=151")
          const pokemons = res.data.results;
          return pokemons.map(pokemon => {
            return '/pokemon/' + pokemon.name
          })
        }
      }
}
```

src/pages/pokemon/\[name].astro

```astro
---
export const getStaticPaths = async () => {
  const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
  const resJson = await res.json();
  const pokemons = resJson.results;
  return pokemons.map(({ name }) => ({
      params: { name },
    }))
}
// ...
---
<!-- Your template here -->
```

### Nuxt Data Fetching to Astro

[Section titled “Nuxt Data Fetching to Astro”](#nuxt-data-fetching-to-astro)

Nuxt has two methods of fetching server-side data:

* [`asyncData` options API](https://nuxtjs.org/docs/features/data-fetching/#async-data)
* [`fetch` hook](https://nuxtjs.org/docs/features/data-fetching/#the-fetch-hook)

In Astro, fetch data inside of your page’s code fence.

Migrate the following:

pages/index.vue

```vue
{
  // ...
  async asyncData() {
    const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
    const resJson = await res.json();
    const pokemons = resJson.results;
    return {
      pokemons,
    }
  },
}
```

To a code fence without a wrapper function:

src/pages/index.astro

```astro
---
const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
const resJson = await res.json();
const pokemons = resJson.results;
---


<!-- Your template here -->
```

### Nuxt Styling to Astro

[Section titled “Nuxt Styling to Astro”](#nuxt-styling-to-astro)

Nuxt utilizes Vue’s component styling to generate a page’s style.

pages/index.vue

```vue
<template>
  <!-- Your template here -->
</template>


<script>
  // Your server logic here
</script>


<style scoped>
    .class {
        color: red;
    }
</style>
```

Similarly, in Astro you can drop in a `<style>` element in your page’s template to provide scoped styles to the component.

src/pages/index.vue

```astro
---
// Your server logic here
---


<style>
    .class {
        color: red;
    }
</style>
```

#### Global Styling

[Section titled “Global Styling”](#global-styling)

`<style>` tags are `scoped` by default in Astro. To make a `<style>` tag global, mark it with the `is:global` attribute:

src/pages/index.vue

```astro
<style is:global>
  p {
    color: red;
  }
</style>
```

#### Pre-processor support

[Section titled “Pre-processor support”](#pre-processor-support)

[Astro supports the most popular CSS preprocessors](/en/guides/styling/#css-preprocessors) by installing them as a dev dependency. For example, to use SCSS:

```shell
npm install -D sass
```

After doing so, you’re then able to use `.scss` or `.sass` styled without modification from your Vue components.

src/layouts/Layout.astro

```astro
<p>Hello, world</p>
<style lang="scss">
p {
   color: black;


   &:hover {
       color: red;
   }
}
</style>
```

See more about [Styling in Astro](/en/guides/styling/).

### Nuxt Image Plugin to Astro

[Section titled “Nuxt Image Plugin to Astro”](#nuxt-image-plugin-to-astro)

Convert any [Nuxt `<nuxt-img/>` or `<nuxt-picture/>` components](https://image.nuxt.com/usage/nuxt-img) to [Astro’s own image component](/en/guides/images/) in `.astro` or `.mdx` files, or to a [standard HTML `<img>`](/en/guides/images/#images-in-ui-framework-components) or `<picture>` tag as appropriate in your Vue components.

Astro’s `<Image />` component works in `.astro` and `.mdx` files only. See a [full list of its component attributes](/en/reference/modules/astro-assets/#image-) and note that several will differ from Nuxt’s attributes.

src/pages/index.astro

```astro
---
import { Image } from 'astro:assets';
import rocket from '../assets/rocket.png';
---
<Image src={rocket} alt="A rocketship in space." />
<img src={rocket.src} alt="A rocketship in space.">
```

In Vue (`.vue`) components within your Astro app, use standard JSX image syntax (`<img />`). Astro will not optimize these images, but you can install and use NPM packages for more flexibility.

You can learn more about [using images in Astro](/en/guides/images/) in the Images Guide.

## Guided example: See the steps!

[Section titled “Guided example: See the steps!”](#guided-example-see-the-steps)

Here is an example of Nuxt Pokédex data fetch converted to Astro.

`pages/index.vue` fetches and displays a list of the first 151 Pokémon using [the REST PokéAPI](https://pokeapi.co/).

Here’s how to recreate that in `src/pages/index.astro`, replacing `asyncData()` with `fetch()`.

1. Identify the `<template>` and `<style>` in the Vue SFC.

   pages/index.vue

   ```jsx
   <template>
     <ul class="plain-list pokeList">
               <li v-for="pokemon of pokemons" class="pokemonListItem" :key="pokemon.name">
                   <NuxtLink class="pokemonContainer" :to="`/pokemon/${pokemon.name}`">
                       <p class="pokemonId">No. {{pokemon.id}}</p>
                       <img
                         class="pokemonImage"
                         :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`"
                         :alt="`${pokemon.name} picture`"/>
                       <h2 class="pokemonName">{{pokemon.name}}</h2>
                   </NuxtLink>
               </li>
       </ul>
   </template>


   <script>
   import Vue from 'vue'
   export default Vue.extend({
     name: 'IndexPage',
     layout: 'default',
     async asyncData() {
       const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
       const resJson = await res.json();
       const pokemons = resJson.results.map(pokemon => {
           const name = pokemon.name;
           // https://pokeapi.co/api/v2/pokemon/1/
           const url = pokemon.url;
           const id = url.split("/")[url.split("/").length - 2];
           return {
               name,
               url,
               id
           }
       });
       return {
         pokemons,
       }
     },
     head() {
       return {
         title: "Pokedex: Generation 1"
       }
     }
   });
   </script>


   <style scoped>
   .pokeList {
     display: grid;
     grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );
     gap: 1rem;
   }


   /* ... */
   </style>
   ```

2. Create `src/pages/index.astro`

   Use the `<template>` and `<style>` tags of the Nuxt SFC. Convert any Nuxt or Vue syntax to Astro.

   Note that:

   * `<template>` is removed

   * `<style>` has its `scoped` attribute removed

   * `v-for` becomes `.map`.

   * `:attr="val"` becomes `attr={val}`

   * `<NuxtLink>` becomes `<a>`.

   * The `<> </>` fragment is not required in Astro templating.

   src/pages/index.astro

   ```astro
   ---
   ---
   <ul class="plain-list pokeList">
       {pokemons.map((pokemon) => (
           <li class="pokemonListItem" key={pokemon.name}>
               <a class="pokemonContainer" href={`/pokemon/${pokemon.name}`}>
                   <p class="pokemonId">No. {pokemon.id}</p>
                   <img class="pokemonImage" src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`} alt={`${pokemon.name} picture`}/>
                   <h2 class="pokemonName">{pokemon.name}</h2>
               </a>
           </li>
       ))}
   </ul>


   <style>
   .pokeList {
     display: grid;
     grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );
     gap: 1rem;
   }


   /* ... */
   </style>
   ```

3. Add any needed imports, props and JavaScript

   Note that:

   * The `asyncData` function is no longer needed. Data from the API is fetched directly in the code fence.
   * A `<Layout>` component is imported, and wraps the page templating.
     * Our `head()` Nuxt method is passed to the `<Layout>` component, which is passed to the `<title>` element as a property.

   src/pages/index.astro

   ```diff
   ---
   +import Layout from '../layouts/layout.astro';


   +const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151");
   +const resJson = await res.json();
   +const pokemons = resJson.results.map(pokemon => {
       const name = pokemon.name;
       +// https://pokeapi.co/api/v2/pokemon/1/
       const url = pokemon.url;
       const id = url.split("/")[url.split("/").length - 2];
       return {
           +name,
           +url,
           +id
       }
   +});
   ---


   +<Layout title="Pokedex: Generation 1">
     <ul class="plain-list pokeList">
         {pokemons.map((pokemon) => (
             <li class="pokemonListItem" key={pokemon.name}>
                 <a class="pokemonContainer" href={`/pokemon/${pokemon.name}`}>
                     <p class="pokemonId">No. {pokemon.id}</p>
                     <img class="pokemonImage" src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`} alt={`${pokemon.name} picture`}/>
                     <h2 class="pokemonName">{pokemon.name}</h2>
                 </a>
             </li>
         ))}
     </ul>
   +</Layout>


   <style>
   .pokeList {
     display: grid;
     grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );
     gap: 1rem;
   }


   /* ... */
   </style>
   ```

## Community Resources

[Section titled “Community Resources”](#community-resources)

[From Nuxt to Astro - rebuilding with Astro ](https://dev.to/lindsaykwardell/from-nuxt-to-astro-rebuilding-with-astro-5ann)

[Nuxt 2 to Astro 3 Replatforming – from Setup to Production ](https://stevenwoodson.com/blog/replatforming-from-nuxtjs-2-to-astro/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Nuxt site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-nuxtjs.mdx)!

# Migrating from Pelican

> Tips for migrating an existing Pelican project to Astro

[Pelican](https://getpelican.com) is an open-source static site generator built on Python.

## Key Similarities between Pelican and Astro

[Section titled “Key Similarities between Pelican and Astro”](#key-similarities-between-pelican-and-astro)

Pelican and Astro share some similarities that will help you migrate your project:

* Pelican and Astro are both static-site generators, ideally suited to [content-driven websites](/en/concepts/why-astro/#content-driven) like blogs.

* Pelican and Astro both have built-in support for [writing in Markdown](/en/guides/markdown-content/), including frontmatter YAML properties for page metadata. However, Astro has very few reserved frontmatter properties compared to Pelican. Even though many of your existing Pelican frontmatter properties will not be “special” in Astro, you can continue to use your existing Markdown files and frontmatter values.

## Key Differences between Pelican and Astro

[Section titled “Key Differences between Pelican and Astro”](#key-differences-between-pelican-and-astro)

When you rebuild your Pelican site in Astro, you will notice some important differences:

* Pelican supports writing content in Markdown and reStructured Text (`.rst`). Astro supports [creating pages from Markdown and MDX](/en/guides/markdown-content/) files, but does not support reStructured Text.

* Pelican uses HTML files and Jinja syntax for templating. [Astro syntax](/en/basics/astro-components/) is a JSX-like superset of HTML. All valid HTML is valid `.astro` syntax.

* Pelican was designed to build content-rich websites like blogs and has some built-in, blog features that you would have to build yourself in Astro. Instead, Astro offers some of these features included in an [official blog theme](https://github.com/withastro/astro/tree/latest/examples/blog).

## Switch from Pelican to Astro

[Section titled “Switch from Pelican to Astro”](#switch-from-pelican-to-astro)

To convert a Pelican documentation site to Astro, start with our official [Starlight docs theme starter template](https://starlight.astro.build), or explore more community themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template starlight
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template starlight
  ```

* Yarn

  ```shell
  yarn create astro --template starlight
  ```

Bring your existing Markdown content files to [create Markdown pages](/en/guides/markdown-content/). You can still take advantage of [file-based routing](/en/guides/routing/) by copying these documents from Pelican’s `content/` folder into `src/pages/` in Astro. You may wish to read about [Astro’s project structure](/en/basics/project-structure/) to learn where files should be located.

Pelican may have handled much of your site layout and metadata for you. You may wish to read about [building Astro Layouts as Markdown page wrappers](/en/basics/layouts/#markdown-layouts) to see how to manage templating yourself in Astro, including your page `<head>`.

Like Pelican, Astro has many plugins that extend its functionality. Explore the [official list of integrations](/en/guides/integrations/) for adding features such as MDX support, and find hundreds more of community-maintained integrations in the [Astro Integrations Directory](https://astro.build/integrations/). You can even use the [Astro Integration API](/en/reference/integrations-reference/) to build your own custom integration to extend your project’s features.

To convert other types of sites, such as a portfolio or a blog, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a Pelican site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-pelican.mdx)!

# Migrating from SvelteKit

> Tips for migrating an existing SvelteKit project to Astro

[SvelteKit](https://kit.svelte.dev) is a framework for building web applications on top of Svelte.

## Key Similarities between SvelteKit and Astro

[Section titled “Key Similarities between SvelteKit and Astro”](#key-similarities-between-sveltekit-and-astro)

SvelteKit and Astro share some similarities that will help you migrate your project:

* Both SvelteKit and Astro are modern JavaScript static-site generators and server-side rendering frameworks.

* Both SvelteKit and Astro use a [`src/` folder for your project files](/en/basics/project-structure/#src) and a [special folder for file-based routing](/en/basics/astro-pages/). Creating and managing pages for your site should feel familiar.

* Astro has [an official integration for using Svelte components](/en/guides/integrations-guide/svelte/) and supports [installing NPM packages](/en/guides/imports/#npm-packages), including several for Svelte. You will be able to write Svelte UI components, and may be able to keep some or all of your existing components and dependencies.

* Astro and SvelteKit both allow you to use a [headless CMS, APIs or Markdown files for data](/en/guides/data-fetching/). You can continue to use your preferred content authoring system, and will be able to keep your existing content.

## Key Differences between SvelteKit and Astro

[Section titled “Key Differences between SvelteKit and Astro”](#key-differences-between-sveltekit-and-astro)

When you rebuild your SvelteKit site in Astro, you will notice some important differences:

* Astro sites are multi-page apps, whereas SvelteKit defaults to SPAs (single-page applications) with server-side rendering, but can also create MPAs, traditional SPAs, or you can mix and match these techniques within an app.

* [Components](/en/basics/astro-components/): SvelteKit uses [Svelte](https://svelte.dev). Astro pages are built using [`.astro` components](/en/basics/astro-components/), but can also support [React, Preact, Vue.js, Svelte, SolidJS, AlpineJS](/en/guides/framework-components/) and raw HTML templating.

* [content-driven](/en/concepts/why-astro/#content-driven): Astro was designed to showcase your content and to allow you to opt-in to interactivity only as needed. An existing SvelteKit app might be built for high client-side interactivity. Astro has built-in capabilities for working with your content, such as page generation, but may require advanced Astro techniques to include items that are more challenging to replicate using `.astro` components, such as dashboards.

* [Markdown-ready](/en/guides/markdown-content/): Astro includes built-in Markdown support, and includes a [special frontmatter YAML `layout` property](/en/basics/layouts/#markdown-layouts) used per-file for page templating. If you are converting a SvelteKit Markdown-based blog, you will not have to install a separate Markdown integration and you will not set a layout via a configuration file. You can bring your existing Markdown files, but you may need to reorganize as Astro’s file-based routing does not require a folder for each page route.

## Switch from SvelteKit to Astro

[Section titled “Switch from SvelteKit to Astro”](#switch-from-sveltekit-to-astro)

To convert a SvelteKit blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes/).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

Bring your existing Markdown (or MDX, with our optional integration) files as content to [create Markdown or MDX pages](/en/guides/markdown-content/).

While file-based routing and layout components are similar in Astro, you may wish to read about [Astro’s project structure](/en/basics/project-structure/) to learn where files should be located.

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Rewriting my blog from SvelteKit to Astro ](https://kharann.com/blog/rewriting-my-blog/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a SvelteKit site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-sveltekit.mdx)!

# Migrating from VuePress

> Tips for migrating an existing VuePress project to Astro

[VuePress](https://vuePress.vuejs.org) is an open-source static site generator built on Vue.

## Key Similarities between VuePress and Astro

[Section titled “Key Similarities between VuePress and Astro”](#key-similarities-between-vuepress-and-astro)

VuePress and Astro share some similarities that will help you migrate your project:

* Both VuePress and Astro are modern JavaScript static-site generators with similar project file structures. Both use a [special `src/pages/` folder for file-based routing](/en/basics/astro-pages/). Creating and managing pages for your site should feel familiar.

* Astro and VuePress are both designed for [content-driven websites](/en/concepts/why-astro/#content-driven), with excellent built-in support for Markdown files. Writing in Markdown will feel familiar, and you will be able to keep your existing content.

* Astro has [an official integration for using Vue components](/en/guides/integrations-guide/vue/) and supports [installing NPM packages](/en/guides/imports/#npm-packages), including several for Vue. You will be able to write Vue UI components, and may be able to keep some or all of your existing Vue components and dependencies.

## Key Differences between VuePress and Astro

[Section titled “Key Differences between VuePress and Astro”](#key-differences-between-vuepress-and-astro)

When you rebuild your VuePress site in Astro, you will notice some important differences.

* VuePress is a Vue-based single-page application (SPA). Astro sites are multi-page apps built using [`.astro` components](/en/basics/astro-components/), but can also support [React, Preact, Vue.js, Svelte, SolidJS, AlpineJS](/en/guides/framework-components/) and raw HTML templating.

* [Layout templates](/en/basics/layouts/): VuePress sites are created using Markdown (`.md`) files for page content and HTML (`.html`) templates for layout. Astro is component-based, and uses Astro components, which include HTML templating for pages, layouts and individual UI elements. Astro can also create [pages from `.md` and `.mdx` files](/en/guides/markdown-content/), using an Astro layout component for wrapping Markdown content in a page template.

* VuePress was designed to build content-heavy, Markdown-centric sites and has some built-in, documentation-specific website features that you would have to build yourself in Astro. Instead, Astro offers some documentation-specific features through an [official docs theme](https://starlight.astro.build). This website was the inspiration for that template! You can also find more [community docs themes](https://astro.build/themes?search=\&categories%5B%5D=docs) with built-in features in our Themes Showcase.

## Switch from VuePress to Astro

[Section titled “Switch from VuePress to Astro”](#switch-from-vuepress-to-astro)

To convert a VuePress documentation site to Astro, start with our official [Starlight docs theme starter template](https://starlight.astro.build), or explore more community docs themes in our [theme showcase](https://astro.build/themes?search=\&categories%5B%5D=docs).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template starlight
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template starlight
  ```

* Yarn

  ```shell
  yarn create astro --template starlight
  ```

Bring your existing Markdown content files to [create Markdown pages](/en/guides/markdown-content/). You can still take advantage of [file-based routing](/en/guides/routing/) by moving these documents from `docs` in VuePress to `src/pages/` in Astro. Create folders with names that correspond to your existing VuePress project, and you should be able to keep your existing URLs.

VuePress, or any theme you installed, probably handled much of your site layout and metadata for you. You may wish to read about [building Astro Layouts as Markdown page wrappers](/en/basics/layouts/#markdown-layouts) to see how to manage templating yourself in Astro, including your page `<head>`.

You can find Astro’s docs starter, and other templates, on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a VuePress site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-vuepress.mdx)!

# Migrating from WordPress

> Tips for migrating an existing WordPress project to Astro

[WordPress](https://wordpress.org) is an open-source, personal publishing system built on PHP and MySQL.

Tip

You can [use WordPress as a headless CMS for your Astro project](/en/guides/cms/wordpress/). Follow our guide to use your existing WordPress content in a new Astro project.

## Key Similarities between WordPress and Astro

[Section titled “Key Similarities between WordPress and Astro”](#key-similarities-between-wordpress-and-astro)

WordPress and Astro share some similarities that will help you migrate your project:

* Both WordPress and Astro are ideal for [content-driven websites](/en/concepts/why-astro/#content-driven) like blogs and support writing your content in Markdown (requires a plugin in WordPress). Although the process for adding new content is different, [writing in Markdown files](/en/guides/markdown-content/) for your Astro blog should feel familiar if you have used Markdown syntax in your WordPress editor.

* Both WordPress and Astro encourage you to [think about the design of your site in “blocks”](/en/concepts/islands/) (components). In Astro you will probably [write more of your own code to create these blocks](/en/basics/astro-components/) rather than rely on pre-built plugins. But thinking about the individual pieces of your site and how they are presented on the page should feel familiar.

## Key Differences between WordPress and Astro

[Section titled “Key Differences between WordPress and Astro”](#key-differences-between-wordpress-and-astro)

When you rebuild your WordPress site in Astro, you will notice some important differences:

* A WordPress site is edited using an online dashboard. In Astro, you will use a [code editor](/en/editor-setup/) and development environment to maintain your site. You can develop locally on your machine, or choose a cloud editor/development environment like StackBlitz or CodeSandbox.

* WordPress has an extensive plugin and theme market. In Astro, you will find some themes and [integrations](https://astro.build/integrations/) available, but you may now have to build many of your existing features yourself instead of looking for third-party solutions. Or, you can choose to start with an [Astro theme](https://astro.build/themes) with built-in features!

* WordPress stores your content in a database. In Astro, you will have individual files (typically Markdown or MDX) in your [project directory](/en/basics/project-structure/) for each page’s content. Or, you can choose to use a [CMS for your content](/en/guides/cms/), even your existing WordPress site, and use Astro to fetch and present the data.

## Switch from WordPress to Astro

[Section titled “Switch from WordPress to Astro”](#switch-from-wordpress-to-astro)

To convert a WordPress blog to Astro, start with our blog theme starter template, or explore more community blog themes in our [theme showcase](https://astro.build/themes).

You can pass a `--template` argument to the `create astro` command to start a new Astro project with one of our official starters. Or, you can [start a new project from any existing Astro repository on GitHub](/en/install-and-setup/#install-from-the-cli-wizard).

* npm

  ```shell
  npm create astro@latest -- --template blog
  ```

* pnpm

  ```shell
  pnpm create astro@latest --template blog
  ```

* Yarn

  ```shell
  yarn create astro --template blog
  ```

You can continue to [use your existing WordPress blog as your CMS for Astro](/en/guides/cms/wordpress/), which means you will keep using your WordPress dashboard for writing your posts. Your content will be managed at WordPress, but all other aspects of your Astro site will be built in your code editing environment, and you will [deploy your Astro site](/en/guides/deploy/) separately from your WordPress site. (Be sure to update your domain at your host to keep the same website URL!)

You may wish to take [Astro’s Build a Blog Tutorial](/en/tutorial/0-introduction/) if you are new to working in a code editor and using GitHub to store and deploy your site. It will walk you through all the accounts and setup you need! You will also learn how to [build Astro components yourself](/en/tutorial/3-components/), and it will show you how to [add blog posts directly in Astro](/en/tutorial/2-pages/2/) if you choose not to use WordPress to write your content.

If you want to move all of your existing post content to Astro, you may find this [tool for exporting Markdown from WordPress helpful](https://github.com/lonekorean/wordpress-export-to-markdown). You may need to make some adjustments to the result if you have to [convert a large or complicated WordPress site to Markdown](https://swizec.com/blog/how-to-export-a-large-wordpress-site-to-markdown/).

To convert other types of sites, such as a portfolio or documentation site, see more official starter templates on [astro.new](https://astro.new). You’ll find a link to each project’s GitHub repository, as well as one-click links to open a working project in StackBlitz and CodeSandbox online development environments.

## Community Resources

[Section titled “Community Resources”](#community-resources)

[Goodbye Wordpress, hello Astro! ](https://trib.tv/posts/2025/wordpress-to-astro/)

[How I Migrated from Wordpress to Astro ](https://itsthatlady.dev/blog/migrate-from-wordpress-to-astro/)

[How and Why I Moved My Blog from WordPress to Astro and Markdown ](https://levelup.gitconnected.com/how-and-why-i-moved-my-blog-from-wordpress-to-astro-and-markdown-3549672d5a86)

[How I Migrated From Wordpress to Astro: Boosted Pagespeed Scores to 100% and Cut 100% Hosting cost ](https://devaradise.com/wordpress-to-static-website-astro/)

[WordPress to Astro site conversion ](https://share.transistor.fm/s/d86496cd)

[How to Convert a Wordpress blog to an Astro Static Site ](https://blog.okturtles.org/2024/10/convert-wordpress-to-static-site/)

[Why I switched from WordPress to Astro ](https://dev.to/fratzinger/why-i-switched-from-wordpress-to-astro-5ge)

[Why I ditched WordPress for Astro ](https://vbartalis.xyz/en/blog/why-i-ditched-wordpress-for-astro-js/)

[DeWP: utility to use your WordPress data in Astro projects ](https://delucis.github.io/dewp/)

[Astro vs. WordPress: Rendering Patterns of the Modern Web ](https://andrewkepson.com/blog/headless-wordpress/astro-vs-wordpress-rendering-patterns/)

Have a resource to share?

If you found (or made!) a helpful video or blog post about converting a WordPress site to Astro, [add it to this list](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/migrate-to-astro/from-wordpress.mdx)!


---

# Testing

> An intro to testing in Astro

Testing helps you write and maintain working Astro code. Astro supports many popular tools for unit tests, component tests, and end-to-end tests including Jest, Mocha, Jasmine, [Cypress](https://cypress.io) and [Playwright](https://playwright.dev). You can even install framework-specific testing libraries such as React Testing Library to test your UI framework components.

Testing frameworks allow you to state **assertions** or **expectations** about how your code should behave in specific situations, then compare these to the actual behavior of your current code.

## Unit and integration tests

[Section titled “Unit and integration tests”](#unit-and-integration-tests)

### Vitest

[Section titled “Vitest”](#vitest)

A Vite-native unit test framework with ESM, TypeScript and JSX support powered by esbuild.

Use Astro’s `getViteConfig()` helper in your [`vitest.config.ts` configuration file](https://vitest.dev/config/) to set up Vitest with your Astro project’s settings:

vitest.config.ts

```js
/// <reference types="vitest/config" />
import { getViteConfig } from 'astro/config';


export default getViteConfig({
  test: {
    // Vitest configuration options
  },
});
```

By default, `getViteConfig()` will try to load an Astro config file in your project and apply it to the test environment. As of Astro 4.8, if you need to customize the Astro configuration applied in your tests, pass a second argument to `getViteConfig()`:

```js
export default getViteConfig(
  { test: { /* Vitest configuration options */ } },
  {
    site: 'https://example.com/',
    trailingSlash: 'always',
  },
);
```

See the [Astro + Vitest starter template](https://github.com/withastro/astro/tree/latest/examples/with-vitest) on GitHub.

#### Vitest and Container API

[Section titled “Vitest and Container API”](#vitest-and-container-api)

**Added in:** `astro@4.9.0`

You can natively test Astro components using the [container API](/en/reference/container-reference/). First, setup [`vitest` as explained above](#vitest), then create a `.test.js` file to test your component:

example.test.js

```js
import { experimental_AstroContainer as AstroContainer } from 'astro/container';
import { expect, test } from 'vitest';
import Card from '../src/components/Card.astro';


test('Card with slots', async () => {
  const container = await AstroContainer.create();
  const result = await container.renderToString(Card, {
    slots: {
      default: 'Card content',
    },
  });


  expect(result).toContain('This is a card');
  expect(result).toContain('Card content');
});
```

## End-to-end tests

[Section titled “End-to-end tests”](#end-to-end-tests)

### Playwright

[Section titled “Playwright”](#playwright)

Playwright is an end-to-end testing framework for modern web apps. Use the Playwright API in JavaScript or TypeScript to test your Astro code on all modern rendering engines including Chromium, WebKit, and Firefox.

#### Installation

[Section titled “Installation”](#installation)

You can get started and run your tests using the [VS Code Extension](https://playwright.dev/docs/getting-started-vscode).

Alternatively, you can install Playwright within your Astro project using the package manager of your choice. Follow the CLI steps to choose JavaScript/TypeScript, name your test folder, and add an optional GitHub Actions workflow.

* npm

  ```shell
  npm init playwright@latest
  ```

* pnpm

  ```shell
  pnpm create playwright
  ```

* Yarn

  ```shell
  yarn create playwright
  ```

#### Create your first Playwright test

[Section titled “Create your first Playwright test”](#create-your-first-playwright-test)

1. Choose a page to test. This example will test the example page `index.astro` below.

   src/pages/index.astro

   ```html
   ---
   ---
   <html lang="en">
     <head>
       <title>Astro is awesome!</title>
       <meta name="description" content="Pull content from anywhere and serve it fast with Astro's next-gen islands architecture." />
     </head>
     <body></body>
   </html>
   ```

2. Create a new folder and add the following test file in `src/test`. Copy and paste the following test into the file to verify that the page meta information is correct. Update the value of the page `<title>` to match the page you are testing.

   src/test/index.spec.ts

   ```jsx
   import { test, expect } from '@playwright/test';


   test('meta is correct', async ({ page }) => {
     await page.goto("http://localhost:4321/");


     await expect(page).toHaveTitle('Astro is awesome!');
   });
   ```

   Set a `baseUrl`

   You can set [`"baseURL": "http://localhost:4321"`](https://playwright.dev/docs/api/class-testoptions#test-options-base-url) in the `playwright.config.ts` configuration file to use `page.goto("/")` instead of `page.goto("http://localhost:4321/")` for a more convenient URL.

#### Running your Playwright tests

[Section titled “Running your Playwright tests”](#running-your-playwright-tests)

You can run a single test or several tests at once, testing one or multiple browsers. By default, your test results will be shown in the terminal. Optionally, you can open the HTML Test Reporter to show a full report and filter test results.

1. To run our test from the previous example using the command line, use the `test` command. Optionally, include the file name to run just the single test:

   ```sh
   npx playwright test index.spec.ts
   ```

2. To see the full HTML Test Report, open it using the following command:

   ```sh
   npx playwright show-report
   ```

Tip

Run your tests against your production code to more closely resemble your live, deployed site.

##### Advanced: Launching a development web server during the tests

[Section titled “Advanced: Launching a development web server during the tests”](#advanced-launching-a-development-web-server-during-the-tests)

You can also have Playwright start your server when you run your testing script by using the [`webServer`](https://playwright.dev/docs/test-advanced#launching-a-development-web-server-during-the-tests) option in the Playwright configuration file.

Here is an example of the configuration and commands required when using npm:

1. Add a test script to your `package.json` file in the project root, such as `"test:e2e": "playwright test"`.

2. In `playwright.config.ts`, add the `webServer` object and update the command value to `npm run preview`.

   playwright.config.ts

   ```diff
   import { defineConfig } from '@playwright/test';


   export default defineConfig({
   +  webServer: {
   +    command: 'npm run preview',
   +    url: 'http://localhost:4321/',
   +    timeout: 120 * 1000,
   +    reuseExistingServer: !process.env.CI,
   +  },
     use: {
       baseURL: 'http://localhost:4321/',
     },
   });
   ```

3. Run `npm run build`, then run `npm run test:e2e` to run the Playwright tests.

More information about Playwright can be found in the links below:

* [Getting started with Playwright](https://playwright.dev/docs/intro)
* [Use a development server](https://playwright.dev/docs/test-webserver#configuring-a-web-server)

### Cypress

[Section titled “Cypress”](#cypress)

Cypress is a front-end testing tool built for the modern web. Cypress enables you to write end-to-end tests for your Astro site.

#### Installation

[Section titled “Installation”](#installation-1)

You can install Cypress using the package manager of your choice. This will install Cypress locally as a dev dependency for your project.

* npm

  ```shell
  npm install cypress --save-dev
  ```

* pnpm

  ```shell
  pnpm add --save-dev cypress
  ```

* Yarn

  ```shell
  yarn add cypress --dev
  ```

#### Configuration

[Section titled “Configuration”](#configuration)

In the root of your project, create a `cypress.config.js` file with the following content:

cypress.config.js

```js
import { defineConfig } from 'cypress'


export default defineConfig({
  e2e: {
    supportFile: false
  }
})
```

#### Create your first Cypress test

[Section titled “Create your first Cypress test”](#create-your-first-cypress-test)

1. Choose a page to test. This example will test the example page `index.astro` below.

   src/pages/index.astro

   ```html
   ---
   ---
   <html lang="en">
     <head>
       <title>Astro is awesome!</title>
       <meta name="description" content="Pull content from anywhere and serve it fast with Astro's next-gen islands architecture." />
     </head>
     <body>
     <h1>Hello world from Astro</h1>
     </body>
   </html>
   ```

2. Create an `index.cy.js` file in the `cypress/e2e` folder. Use the following test in the file to verify that the page title and header are correct.

   cypress/e2e/index.cy.js

   ```js
   it('titles are correct', () => {
     const page = cy.visit('http://localhost:4321');


     page.get('title').should('have.text', 'Astro is awesome!')
     page.get('h1').should('have.text', 'Hello world from Astro');
   });
   ```

   Set a `baseUrl`

   You can set [`"baseUrl": "http://localhost:4321"`](https://docs.cypress.io/guides/end-to-end-testing/testing-your-app#Step-3-Configure-Cypress) in the `cypress.config.js` configuration file to use `cy.visit("/")` instead of `cy.visit("http://localhost:4321/")` for a more convenient URL.

#### Running your Cypress tests

[Section titled “Running your Cypress tests”](#running-your-cypress-tests)

Cypress can be run from the command line or from the Cypress App. The App provides a visual interface for running and debugging your tests.

First, start the dev server so Cypress can access your live site.

To run our test from the previous example using the command line, execute the following command:

```shell
npx cypress run
```

Alternatively, to run the test using the Cypress App, execute the following command:

```shell
npx cypress open
```

Once the Cypress App is launched, choose **E2E Testing**, then select the browser to be used to run tests.

Once the test run is finished, you should see green check marks in the output confirming that your test passed:

Output from npx cypress run

```shell
Running:  index.cy.js                                                                     (1 of 1)


✓ titles are correct (107ms)


1 passing (1s)
```

Fail the test

To check that your test really does work, you can change the following line in the `index.astro` file:

src/pages/index.astro

```diff
 <body>
   <h1>Hello world from Astro</h1>
   <h1>Hello from Astro</h1>
 </body>
```

Then run the test again. You should see a red “x” in the output confirming that your test failed.

#### Next steps

[Section titled “Next steps”](#next-steps)

More information about Cypress can be found in the links below:

* [Introduction to Cypress](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress)
* [Testing Your App](https://docs.cypress.io/guides/end-to-end-testing/testing-your-app)

### NightwatchJS

[Section titled “NightwatchJS”](#nightwatchjs)

Nightwatch.js is a test automation framework with a powerful set of tools to write, run, and debug your tests across the web with built-in support for all major browsers and their mobile equivalents, as well as native mobile applications.

#### Installation

[Section titled “Installation”](#installation-2)

You can install NightwatchJS within your Astro project using the package manager of your choice. Follow the CLI steps to choose JavaScript/TypeScript, name your test folder, and select whether or not to include component testing and testing on mobile browsers.

* npm

  ```shell
  npm init nightwatch@latest
  ```

* pnpm

  ```shell
  pnpm create nightwatch
  ```

* Yarn

  ```shell
  yarn create nightwatch
  ```

#### Create your first Nightwatch test

[Section titled “Create your first Nightwatch test”](#create-your-first-nightwatch-test)

1. Choose a page to test. This example will test the example page `index.astro` below.

   src/pages/index.astro

   ```html
   ---
   ---
   <html lang="en">
     <head>
       <title>Astro is awesome!</title>
       <meta name="description" content="Pull content from anywhere and serve it fast with Astro's next-gen islands architecture." />
     </head>
     <body></body>
   </html>
   ```

2. Create a new folder `src/test/` and add the following test file:

   src/test/index.js

   ```js
   describe('Astro testing with Nightwatch', function () {
       before(browser => browser.navigateTo('http://localhost:4321/'));


       it("check that the title is correct", function (browser) {
           browser.assert.titleEquals('Astro is awesome!')
       });


       after(browser => browser.end());
   });
   ```

   Set a `baseUrl`

   You can set [`"baseURL": "http://localhost:4321"`](https://nightwatchjs.org/guide/reference/settings.html#setting-the-baseurl-property) in the `nightwatch.conf.js` configuration file to use `browser.navigateTo("/")` instead of `browser.navigateTo("http://localhost:4321/")` for a more convenient URL.

#### Running your NightwatchJS tests

[Section titled “Running your NightwatchJS tests”](#running-your-nightwatchjs-tests)

You can run a single test or several tests at once, testing one or multiple browsers. By default, your test results will be shown in the terminal. Optionally, you can open the HTML Test Reporter to show a full report and filter test results.

You can run the tests with the [NightwatchJS VSCode Extension](https://marketplace.visualstudio.com/items?itemName=browserstackcom.nightwatch) or using the CLI steps below:

1. To run all tests, enter the following command in the terminal. Optionally, include the file name to run just the single test:

   ```sh
   npx nightwatch test/index.js
   ```

   Additionally, you can run the tests against a specific browser using the `--environment` or `-e` CLI argument. If you don’t have the relevant browser installed, Nightwatch will attempt to set it up for you using [Selenium Manager](https://www.selenium.dev/blog/2022/introducing-selenium-manager/):

   ```sh
   npx nightwatch test/index.ts -e firefox
   ```

2. To see the full HTML Test Report, open it using the following command:

   ```sh
   npx nightwatch test/index.ts --open
   ```

Tip

Run your tests against your production code to more closely resemble your live, deployed site.

More information about NightwatchJS can be found in the links below:

* [Intro to Nightwatch](https://nightwatchjs.org/guide/overview/what-is-nightwatch.html)
* [Testing with Nightwatch](https://nightwatchjs.org/guide/writing-tests/introduction.html)

# Troubleshooting

> Need help? Stuck on something? We've got you covered.

Astro provides several different tools to help you troubleshoot and debug your code.

## Tips and tricks

[Section titled “Tips and tricks”](#tips-and-tricks)

### Debugging with `console.log()`

[Section titled “Debugging with console.log()”](#debugging-with-consolelog)

`console.log()` is a simple-but-popular method of debugging your Astro code. Where you write your `console.log()` statement will determine where your debugging output is printed:

```astro
---
console.log('Hi! I’m the server. This is logged in the terminal where Astro is running.');
---


<script>
console.log('Hi! I’m the client. This is logged in browser dev console.');
</script>
```

A `console.log()` statement in Astro frontmatter will always output to the **terminal** running the Astro CLI. This is because Astro runs on the server, and never in the browser.

Code that is written or imported inside of an Astro `<script>` tag is run in the browser. Any `console.log()` statements or other debug output will be printed to the **console in your browser**.

### Debugging framework components

[Section titled “Debugging framework components”](#debugging-framework-components)

[Framework components](/en/guides/framework-components/) (like React and Svelte) are unique: They render server-side by default, meaning that `console.log()` debug output will be visible in the terminal. However, they can also be hydrated for the browser, which may cause your debug logs to also appear in the browser.

This can be useful for debugging differences between the server output and the hydrated components in the browser.

### Astro `<Debug />` component

[Section titled “Astro \<Debug /> component”](#astro-debug--component)

To help you debug your Astro components, Astro provides a built-in `<Debug />` component which renders any value directly into your component HTML template.

This component provides a way to inspect values on the client-side, without any JavaScript. It can be useful for quick debugging in the browser without having to flip back-and-forth between your terminal and your browser.

```astro
---
import { Debug } from 'astro:components';
const sum = (a, b) => a + b;
---


<!-- Example: Outputs {answer: 6} to the browser -->
<Debug answer={sum(2, 4)} />
```

The Debug component supports a variety of syntax options for even more flexible and concise debugging:

```astro
---
import { Debug } from 'astro:components';
const sum = (a, b) => a + b;
const answer = sum(2, 4);
---
<!-- Example: All three examples are equivalent. -->
<Debug answer={sum(2, 4)} />
<Debug {{answer: sum(2, 4)}} />
<Debug {answer} />
```

## Common Error Messages

[Section titled “Common Error Messages”](#common-error-messages)

Here are some common error messages you might see in the terminal, what they might mean, and what to do about them. See our [full error reference guide](/en/reference/error-reference/) for a complete list of Astro errors you may encounter.

### Cannot use import statement outside a module

[Section titled “Cannot use import statement outside a module”](#cannot-use-import-statement-outside-a-module)

In Astro components, `<script>` tags are loaded as [JS modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) by default. If you have included the [`is:inline` directive](/en/reference/directives-reference/#isinline) or any other attribute in your tag, this default behavior is removed.

**Solution**: If you have added any attributes to your `<script>` tag, you must also add the `type="module"` attribute to be able to use import statements.

**Status**: Expected Astro behavior, as intended.

**Not sure that this is your problem?**\
Check to see if anyone else has reported [this issue](https://github.com/withastro/astro/issues?q=is%3Aissue+is%3Aopen+Cannot+use+import+statement)!

### `document` (or `window`) is not defined

[Section titled “document (or window) is not defined”](#document-or-window-is-not-defined)

This error occurs when trying to access `document` or `window` on the server.

Astro components run on the server, so you can’t access these browser-specific objects within the frontmatter.

Framework components run on the server by default, so this error can occur when accessing `document` or `window` during rendering.

**Solution**: Determine the code that calls `document` or `window`. If you aren’t using `document` or `window` directly and still getting this error, check to see if any packages you’re importing are meant to run on the client.

* If the code is in an Astro component, move it to a `<script>` tag outside of the frontmatter. This tells Astro to run this code on the client, where `document` and `window` are available.

* If the code is in a framework component, try to access these objects after rendering using lifecycle methods (e.g. [`useEffect()`](https://react.dev/reference/react/useEffect) in React, [`onMounted()`](https://vuejs.org/api/composition-api-lifecycle.html#onmounted) in Vue, and [`onMount()`](https://svelte.dev/docs#run-time-svelte-onmount) in Svelte). Tell the framework component to hydrate client-side by using a [client:](/en/reference/directives-reference/#client-directives) directive, like `client:load`, to run these lifecycle methods. You can also prevent the component from rendering on the server at all by adding the [`client:only`](/en/reference/directives-reference/#clientonly) directive.

**Status**: Expected Astro behavior, as intended.

### Expected a default export

[Section titled “Expected a default export”](#expected-a-default-export)

This error can be thrown when trying to import or render an invalid component, or one that is not working properly. (This particular message occurs because of the way importing a UI component works in Astro.)

**Solution**: Try looking for errors in any component you are importing and rendering, and make sure it’s working correctly. Consider opening an Astro starter template from [astro.new](https://astro.new) and troubleshooting just your component in a minimal Astro project.

**Status**: Expected Astro behavior, as intended.

### Refused to execute inline script

[Section titled “Refused to execute inline script”](#refused-to-execute-inline-script)

You may see the following error logged in the browser console:

> Refused to execute inline script because it violates the following Content Security Policy directive: …

This means that your site’s [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) (CSP) disallows running inline `<script>` tags, which Astro outputs by default.

**Solution:** Update your CSP to include `script-src: 'unsafe-inline'` to allow inline scripts to run. Alternatively, you can use a third-party integration such as [`astro-shield`](https://github.com/KindSpells/astro-shield) to generate the CSP headers for you.

## Common gotchas

[Section titled “Common gotchas”](#common-gotchas)

### My component is not rendering

[Section titled “My component is not rendering”](#my-component-is-not-rendering)

First, check to see that you have **imported the component** in your [`.astro` component script](/en/basics/astro-components/#the-component-script) or [`.mdx` file](/en/guides/integrations-guide/mdx/#using-components-in-mdx).

Then check your import statement:

* Is your import linking to the wrong place? (Check your import path.)

* Does your import have the same name as the imported component? (Check your component name and that it [follows the `.astro` syntax](/en/reference/astro-syntax/#differences-between-astro-and-jsx).)

* Have you included the extension in the import? (Check that your imported file contains an extension. e.g. `.astro`, `.md`, `.vue`, `.svelte`. Note: File extensions are **not** required for `.js(x)` and `.ts(x)` files only.)

### My component is not interactive

[Section titled “My component is not interactive”](#my-component-is-not-interactive)

If your component is rendering (see above) but is not responding to user interaction, then you may be missing a [`client:*` directive](/en/reference/directives-reference/#client-directives) to hydrate your component.

By default, a [UI Framework component is not hydrated in the client](/en/guides/framework-components/#hydrating-interactive-components). If no `client:*` directive is provided, its HTML is rendered onto the page without JavaScript.

Tip

[Astro components](/en/basics/astro-components/) are HTML-only templating components with no client-side runtime. But, you can use a `<script>` tag in your Astro component template to send JavaScript to the browser that executes in the global scope.

### Cannot find package ‘X’

[Section titled “Cannot find package ‘X’”](#cannot-find-package-x)

If you see a `"Cannot find package 'react'"` (or similar) warning when you start up Astro, that means that you need to install that package into your project. Not all package managers will install peer dependencies for you automatically. If you are on Node v16+ and using npm, you should not need to worry about this section.

React, for example, is a peer dependency of the `@astrojs/react` integration. That means that you should install the official `react` and `react-dom` packages alongside your integration. The integration will then pull from these packages automatically.

```shell
# Example: Install integrations and frameworks together
npm install @astrojs/react react react-dom
```

See [Astro’s integration guide](/en/guides/integrations/) for instructions on adding framework renderers, CSS tools and other packages to Astro.

### Using Astro with Yarn 2+ (Berry)

[Section titled “Using Astro with Yarn 2+ (Berry)”](#using-astro-with-yarn-2-berry)

Yarn 2+, a.k.a. Berry, uses a technique called [Plug’n’Play (PnP)](https://yarnpkg.com/features/pnp) to store and manage Node modules, which can [cause problems](https://github.com/withastro/astro/issues/3450) while initializing a new Astro project using `create astro` or while working with Astro. A workaround is to set the [`nodeLinker` property](https://yarnpkg.com/configuration/yarnrc#nodeLinker) in `.yarnrc.yml` to `node-modules`:

.yarnrc.yml

```yaml
nodeLinker: "node-modules"
```

### Adding dependencies to Astro in a monorepo

[Section titled “Adding dependencies to Astro in a monorepo”](#adding-dependencies-to-astro-in-a-monorepo)

When working with Astro in a monorepo setup, project dependencies should be added in each project’s own `package.json` file.

However, you may also want to use Astro in the root of the monorepo (e.g. [Nx projects recommend installing dependencies at the root](https://github.com/nrwl/nx/issues/3023#issuecomment-630558318)). In this case, manually add Astro-related dependencies (e.g. `@astrojs/vue`, `astro-component-lib`) to the `vite.ssr.noExternal` part of Astro’s config to ensure that these dependencies are properly installed and bundled:

astro.config.mjs

```js
import { defineConfig } from 'astro/config'
export default defineConfig({
  vite: {
    ssr: {
      noExternal: [
        '@astrojs/vue',
        'astro-component-lib',
      ]
    }
  }
})
```

### Using `<head>` in a component

[Section titled “Using \<head> in a component”](#using-head-in-a-component)

In Astro, using a `<head>` tag works like any other HTML tag: it does not get moved to the top of the page or merged with the existing `<head>`. Because of this, you usually only want to include one `<head>` tag throughout a page. We recommend writing that single `<head>` and its contents in a [layout component](/en/basics/layouts/).

### An unexpected `<style>` is included

[Section titled “An unexpected \<style> is included”](#an-unexpected-style-is-included)

You may notice an imported component’s `<style>` tag included in your HTML source even if that component doesn’t appear in the final output. For example, this will occur with [conditionally rendered](/en/reference/astro-syntax/#dynamic-html) components that are not displayed.

Astro’s build process works on the module graph: once a component is included in the template, its `<style>` tag is processed, optimized, and bundled, whether it appears in the final output or not.

### Escaping special characters in Markdown

[Section titled “Escaping special characters in Markdown”](#escaping-special-characters-in-markdown)

Certain characters have a special meaning in Markdown. You may need to use a different syntax if you want to display them. To do this, you can use [HTML entities](https://developer.mozilla.org/en-US/docs/Glossary/Entity) for these characters instead.

For example, to prevent `<` being interpreted as the beginning of an HTML element, write `&lt;`.

## Creating minimal reproductions

[Section titled “Creating minimal reproductions”](#creating-minimal-reproductions)

When troubleshooting your code, it can be helpful to create a **minimal reproduction** of the issue that you can share. This is a smaller, simplified Astro project that demonstrates your issue. Having a working reproduction in a new project helps to confirm that this is a repeatable problem, and is not caused by something else in your personal environment or existing project.

Sharing a minimal reproduction is helpful when asking for help in our support threads and is often required when filing a bug report to Astro.

### Create a StackBlitz via [astro.new](https://astro.new/repro)

[Section titled “Create a StackBlitz via astro.new”](#create-a-stackblitz-via-astronew)

You can use [astro.new](https://astro.new/repro) to create a new Astro project with a single click. For minimal reproductions, we strongly recommend starting from the minimal (empty) example running in [StackBlitz](https://stackblitz.com), with as little extra code as possible.

StackBlitz will run this Astro project in the browser, outside of your local environment. It will also provide you with a shareable link so that any Astro maintainer or support squad member can view your minimal reproduction outside of their own local environment. This means that everyone is viewing the exact same project, with the same configuration and dependencies. This makes it easy for someone else to help troubleshoot your code. If the issue is reproducible, it allows you to verify that the issue lies within the Astro code itself and you can feel confident submitting a bug report.

Note that not every issue is reproducible in StackBlitz. For example, your issue might be dependent on a specific environment or package manager, or it may involve HTML Streaming, which isn’t supported in StackBlitz. In this case, create a new minimal (empty) Astro project using the CLI, reproduce the issue, and upload it to a GitHub repository. Instead of sharing a StackBlitz URL, provide a link to the GitHub repository of your minimal reproduction.

### Minimal code

[Section titled “Minimal code”](#minimal-code)

Once your empty project is set up, go through the steps to reproduce the issue. This can include adding packages, changing configuration, and writing code.

You should only add the minimum amount of code necessary to reproduce the issue. Do not reproduce other elements of your existing project, and remove all code that is not directly related to the issue.

### Create an issue

[Section titled “Create an issue”](#create-an-issue)

If your issue can be reproduced, then it is time to create an issue and file a bug report!

Go to the appropriate Astro repository on GitHub and open a new issue. Most repositories have an issue template that will ask questions or require information in order to submit. It’s important that you follow these templates because if you don’t provide the information we need, then we have to ask you for it… and no one is working on your issue!

Include the link to your minimal reproduction on StackBlitz (or GitHub repository, if necessary). Start with a description of the expected versus actual behavior to provide context for the issue. Then, include clear, step-by-step instructions on how to replicate the issue in an Astro project.

## Need more?

[Section titled “Need more?”](#need-more)

Come and chat with us on [Discord](https://astro.build/chat) and explain your issue in the `#support` forum channel. We’re always happy to help!

Visit the current [open Issues in Astro](https://github.com/withastro/astro/issues/) to see if you are encountering a known problem or file a bug report.

You can also visit [Roadmap Discussions](https://github.com/withastro/roadmap/discussions) to see whether you’ve found a known limitation of Astro, and check to see whether there are current proposals related to your use case.

# TypeScript

> Learn how to use Astro's built-in TypeScript support.

Astro ships with built-in support for [TypeScript](https://www.typescriptlang.org/). You can import `.ts` and `.tsx` files in your Astro project, write TypeScript code directly inside your [Astro component](/en/basics/astro-components/#the-component-script), and even use an [`astro.config.ts`](/en/guides/configuring-astro/#the-astro-config-file) file for your Astro configuration if you like.

Using TypeScript, you can prevent errors at runtime by defining the shapes of objects and components in your code. For example, if you use TypeScript to [type your component’s props](#component-props), you’ll get an error in your editor if you set a prop that your component doesn’t accept.

You don’t need to write TypeScript code in your Astro projects to benefit from it. Astro always treats your component code as TypeScript, and the [Astro VS Code Extension](/en/editor-setup/) will infer as much as it can to provide autocompletion, hints, and errors in your editor.

The Astro dev server won’t perform any type checking, but you can use a [separate script](#type-checking) to check for type errors from the command line.

## Setup

[Section titled “Setup”](#setup)

Astro starter projects include a `tsconfig.json` file in your project. Even if you don’t write TypeScript code, this file is important so that tools like Astro and VS Code know how to understand your project. Some features (like npm package imports) aren’t fully supported in the editor without a `tsconfig.json` file. If you install Astro manually, be sure to create this file yourself.

### TSConfig templates

[Section titled “TSConfig templates”](#tsconfig-templates)

Three extensible `tsconfig.json` templates are included in Astro: `base`, `strict`, and `strictest`. The `base` template enables support for modern JavaScript features and is also used as a basis for the other templates. We recommend using `strict` or `strictest` if you plan to write TypeScript in your project. You can view and compare the three template configurations at [astro/tsconfigs/](https://github.com/withastro/astro/blob/main/packages/astro/tsconfigs/).

To inherit from one of the templates, use [the `extends` setting](https://www.typescriptlang.org/tsconfig#extends):

tsconfig.json

```json
{
  "extends": "astro/tsconfigs/base"
}
```

Additionally, we recommend setting `include` and `exclude` as follows to benefit from Astro types and avoid checking built files:

tsconfig.json

```diff
{
  "extends": "astro/tsconfigs/base",
  +"include": [".astro/types.d.ts", "**/*"],
  +"exclude": ["dist"]
}
```

### TypeScript editor plugin

[Section titled “TypeScript editor plugin”](#typescript-editor-plugin)

The [Astro TypeScript plugin](https://www.npmjs.com/package/@astrojs/ts-plugin) can be installed separately when you are not using the [official Astro VS Code extension](https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode). This plugin is automatically installed and configured by the VS Code extension, and you do not need to install both.

This plugin runs only in the editor. When running `tsc` in the terminal, `.astro` files are ignored entirely. Instead, you can use [the `astro check` CLI command](/en/reference/cli-reference/#astro-check) to check both `.astro` and `.ts` files.

This plugin also supports importing `.astro` files from `.ts` files (which can be useful for re-exporting).

* npm

  ```shell
  npm install @astrojs/ts-plugin
  ```

* pnpm

  ```shell
  pnpm add @astrojs/ts-plugin
  ```

* Yarn

  ```shell
  yarn add @astrojs/ts-plugin
  ```

Then, add the following to your `tsconfig.json`:

tsconfig.json

```json
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "@astrojs/ts-plugin"
      },
    ],
  }
}
```

To check that the plugin is working, create a `.ts` file and import an Astro component into it. You should have no warning messages from your editor.

### UI Frameworks

[Section titled “UI Frameworks”](#ui-frameworks)

If your project uses a [UI framework](/en/guides/framework-components/), additional settings depending on the framework might be needed. Please see your framework’s TypeScript documentation for more information. ([Vue](https://vuejs.org/guide/typescript/overview.html#using-vue-with-typescript), [React](https://react-typescript-cheatsheet.netlify.app/docs/basic/setup), [Preact](https://preactjs.com/guide/v10/typescript), [Solid](https://www.solidjs.com/guides/typescript), [Svelte](https://svelte.dev/docs/svelte/typescript))

## Type Imports

[Section titled “Type Imports”](#type-imports)

Use explicit type imports and exports whenever possible.

```diff
-import { SomeType } from "./script";
+import type { SomeType } from "./script";
```

This way, you avoid edge cases where Astro’s bundler may try to incorrectly bundle your imported types as if they were JavaScript.

You can configure TypeScript to enforce type imports in your `tsconfig.json` file. Set [`verbatimModuleSyntax`](https://www.typescriptlang.org/tsconfig#verbatimModuleSyntax) to `true`. TypeScript will check your imports and tell you when `import type` should be used. This setting is enabled by default in all our presets.

tsconfig.json

```diff
{
  "compilerOptions": {
    +"verbatimModuleSyntax": true
  }
}
```

## Import Aliases

[Section titled “Import Aliases”](#import-aliases)

Astro supports import aliases that you define in your `tsconfig.json` `paths` configuration. [Read our imports guide](/en/guides/imports/#aliases) to learn more.

src/pages/about/nate.astro

```astro
---
import HelloWorld from "@components/HelloWorld.astro";
import Layout from "@layouts/Layout.astro";
---
```

tsconfig.json

```json
{
  "compilerOptions": {
    "paths": {
      "@components/*": ["./src/components/*"],
      "@layouts/*": ["./src/layouts/*"]
    }
  }
}
```

## Extending global types

[Section titled “Extending global types”](#extending-global-types)

You can create `src/env.d.ts` as a convention for adding custom types declarations, or to benefit from Astro types if you don’t have a `tsconfig.json`:

src/env.d.ts

```ts
// Custom types declarations
declare var myString: string;


// Astro types, not necessary if you already have a `tsconfig.json`
/// <reference path="../.astro/types.d.ts" />
```

### `window` and `globalThis`

[Section titled “window and globalThis”](#window-and-globalthis)

You may want to add a property to the global object. You can do this by adding top-level declarations using the `declare` keyword to your `env.d.ts` file:

src/env.d.ts

```ts
declare var myString: string;
declare function myFunction(): boolean;
```

This will provide typing to `globalThis.myString` and `globalThis.myFunction`, as well as `window.myString` and `window.myFunction`.

Note that `window` is only available in client-side code. `globalThis` is available both server-side and client-side, but its server-side value won’t be shared with the client.

If you only want to type a property on the `window` object, provide a `Window` interface instead:

src/env.d.ts

```ts
interface Window {
  myFunction(): boolean;
}
```

### Add non-standard attributes

[Section titled “Add non-standard attributes”](#add-non-standard-attributes)

You may want to define a type for custom attributes or CSS properties. You can extend the default JSX definitions to add non-standard attributes by redeclaring the `astroHTML.JSX` namespace in a `.d.ts` file.

src/env.d.ts

```ts
declare namespace astroHTML.JSX {
  interface HTMLAttributes {
    "data-count"?: number;
    "data-label"?: string;
  }


  // Add a CSS custom property to the style object
  interface CSSProperties {
    "--theme-color"?: "black" | "white";
  }
}
```

Note

`astroHTML` is injected globally inside `.astro` components. To use it in TypeScript files, use a [triple-slash directive](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html):

```ts
/// <reference types="astro/astro-jsx" />


type MyAttributes = astroHTML.JSX.ImgHTMLAttributes;
```

### Using imports

[Section titled “Using imports”](#using-imports)

You may want to extend global types by reusing types declared elsewhere in your project or from an external library. To do this, use [dynamic imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import):

src/env.d.ts

```ts
type Product = {
  id: string;
  name: string;
  price: number;
};


declare namespace App {
  interface Locals {
    orders: Map<string, Product[]>
    session: import("./lib/server/session").Session | null;
    user: import("my-external-library").User;
  }
}
```

A `.d.ts` file is an [ambient module](https://www.typescriptlang.org/docs/handbook/modules/reference.html#ambient-modules) declaration. While its syntax is similar to ES modules, these files do not allow top-level imports/exports. If Typescript encounters one, the file will be considered a [module augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation) and this will break your global types.

## Component Props

[Section titled “Component Props”](#component-props)

Astro supports typing your component props via TypeScript. To enable, add a TypeScript `Props` interface to your component frontmatter. An `export` statement may be used, but is not necessary. The [Astro VS Code Extension](/en/editor-setup/) will automatically look for the `Props` interface and give you proper TS support when you use that component inside another template.

src/components/HelloProps.astro

```diff
---
+interface Props {
+  name: string;
+  greeting?: string;
+}


const { greeting = "Hello", name } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

### Common prop type patterns

[Section titled “Common prop type patterns”](#common-prop-type-patterns)

* If your component takes no props or slotted content, you can use `type Props = Record<string, never>`.
* If your component must be passed children to its default slot, you can enforce this by using `type Props = { children: any; };`.

## Type Utilities

[Section titled “Type Utilities”](#type-utilities)

**Added in:** `astro@1.6.0`

Astro comes with some built-in utility types for common prop type patterns. These are available under the `astro/types` entrypoint.

### Built-in HTML attributes

[Section titled “Built-in HTML attributes”](#built-in-html-attributes)

Astro provides the `HTMLAttributes` type to check that your markup is using valid HTML attributes. You can use these types to help build component props.

For example, if you were building a `<Link>` component, you could do the following to mirror the default HTML attributes for `<a>` tags in your component’s prop types.

src/components/Link.astro

```astro
---
import type { HTMLAttributes } from "astro/types";


// use a `type`
type Props = HTMLAttributes<"a">;


// or extend with an `interface`
interface Props extends HTMLAttributes<"a"> {
  myProp?: boolean;
}


const { href, ...attrs } = Astro.props;
---
<a href={href} {...attrs}>
  <slot />
</a>
```

### `ComponentProps` type

[Section titled “ComponentProps type”](#componentprops-type)

**Added in:** `astro@4.3.0`

This type export allows you to reference the `Props` accepted by another component, even if that component doesn’t export that `Props` type directly.

The following example shows using the `ComponentProps` utility from `astro/types` to reference a `<Button />` component’s `Props` types:

src/pages/index.astro

```astro
---
import type { ComponentProps } from "astro/types";
import Button from "./Button.astro";


type ButtonProps = ComponentProps<typeof Button>;
---
```

### Polymorphic type

[Section titled “Polymorphic type”](#polymorphic-type)

**Added in:** `astro@2.5.0`

Astro includes a helper to make it easier to build components that can render as different HTML elements with full type safety. This is useful for components like `<Link>` that can render as either `<a>` or `<button>` depending on the props passed to it.

The example below implements a fully-typed, polymorphic component that can render as any HTML element. The [`HTMLTag`](#built-in-html-attributes) type is used to ensure that the `as` prop is a valid HTML element.

```astro
---
import type { HTMLTag, Polymorphic } from "astro/types";


type Props<Tag extends HTMLTag> = Polymorphic<{ as: Tag }>;


const { as: Tag, ...props } = Astro.props;
---
<Tag {...props} />
```

### Infer `getStaticPaths()` types

[Section titled “Infer getStaticPaths() types”](#infer-getstaticpaths-types)

**Added in:** `astro@2.1.0`

Astro includes helpers for working with the types returned by your [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths) function for dynamic routes.

You can get the type of [`Astro.params`](/en/reference/api-reference/#params) with `InferGetStaticParamsType` and the type of [`Astro.props`](/en/reference/api-reference/#props) with `InferGetStaticPropsType` or you can use `GetStaticPaths` to infer both at once:

src/pages/posts/\[...id].astro

```astro
---
import type {
  InferGetStaticParamsType,
  InferGetStaticPropsType,
  GetStaticPaths,
} from "astro";


export const getStaticPaths = (async () => {
  const posts = await getCollection("blog");
  return posts.map((post) => {
    return {
      params: { id: post.id },
      props: { draft: post.data.draft, title: post.data.title },
    };
  });
}) satisfies GetStaticPaths;


type Params = InferGetStaticParamsType<typeof getStaticPaths>;
type Props = InferGetStaticPropsType<typeof getStaticPaths>;


const { id } = Astro.params as Params;
//                   ^? { id: string; }


const { title } = Astro.props;
//                      ^? { draft: boolean; title: string; }
---
```

## Type checking

[Section titled “Type checking”](#type-checking)

To see type errors in your editor, please make sure that you have the [Astro VS Code extension](/en/editor-setup/) installed. Please note that the `astro start` and `astro build` commands will transpile the code with esbuild, but will not run any type checking. To prevent your code from building if it contains TypeScript errors, change your “build” script in `package.json` to the following:

package.json

```diff
{
  "scripts": {
    -"build": "astro build",
    +"build": "astro check && astro build",
  },
}
```

Note

`astro check` checks all the files included in your TypeScript project. To check types within Svelte and Vue files, you can use the [`svelte-check`](https://www.npmjs.com/package/svelte-check) and the [`vue-tsc`](https://www.npmjs.com/package/vue-tsc) packages respectively.

Read more about [`.ts` file imports](/en/guides/imports/#typescript) in Astro.

Read more about [TypeScript Configuration](https://www.typescriptlang.org/tsconfig/).

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

### Errors typing multiple JSX frameworks at the same time

[Section titled “Errors typing multiple JSX frameworks at the same time”](#errors-typing-multiple-jsx-frameworks-at-the-same-time)

An issue may arise when using multiple JSX frameworks in the same project, as each framework requires different, sometimes conflicting, settings inside `tsconfig.json`.

**Solution**: Set the [`jsxImportSource` setting](https://www.typescriptlang.org/tsconfig#jsxImportSource) to `react` (default), `preact` or `solid-js` depending on your most-used framework. Then, use a [pragma comment](https://www.typescriptlang.org/docs/handbook/jsx.html#configuring-jsx) inside any conflicting file from a different framework.

For the default setting of `jsxImportSource: react`, you would use:

```jsx
// For Preact
/** @jsxImportSource preact */


// For Solid
/** @jsxImportSource solid-js */
```

# Legacy v0.x Upgrade Guide

> Archived guide documenting changes between pre-v1 versions of Astro

This guide will help you upgrade through breaking changes in pre-v1 versions of Astro.

You can update your project’s version of Astro to the latest version using your package manager. If you’re using Astro integrations, you’ll also want to update those to the latest version.

* npm

  ```shell
  # updates the astro dependency:
  npm upgrade astro
  # or, to update all dependencies:
  npm upgrade
  ```

* pnpm

  ```shell
  # updates the astro dependency:
  pnpm upgrade astro
  # or, to update all dependencies:
  pnpm upgrade
  ```

* Yarn

  ```shell
  # updates the astro dependency:
  yarn upgrade astro
  # or, to update all dependencies:
  yarn upgrade
  ```

Read the guide below for major highlights and instructions on how to handle breaking changes.

## Astro 1.0

[Section titled “Astro 1.0”](#astro-10)

Astro v1.0 introduces some changes that you should be aware of when migrating from v0.x and v1.0-beta releases. See below for more details.

### Updated: Vite 3

[Section titled “Updated: Vite 3”](#updated-vite-3)

Astro v1.0 has upgraded from Vite 2 to [Vite 3](https://vite.dev/). We’ve handled most of the upgrade for you inside of Astro; however, some subtle Vite behaviors may still change between versions. Refer to the official [Vite Migration Guide](https://vite.dev/guide/migration.html#general-changes) if you run into trouble.

### Deprecated: `Astro.canonicalURL`

[Section titled “Deprecated: Astro.canonicalURL”](#deprecated-astrocanonicalurl)

You can now use the new [`Astro.url`](/en/reference/api-reference/#url) helper to construct your own canonical URL from the current page/request URL.

```js
// Before:
const canonicalURL = Astro.canonicalURL;
// After:
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
```

### Changed: Scoped CSS specificity

[Section titled “Changed: Scoped CSS specificity”](#changed-scoped-css-specificity)

[Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) will now be preserved in scoped CSS styles. This change will cause most scoped styles to *happen* to take precedence over global styles. But, this behavior is no longer explicitly guaranteed.

Technically, this is accomplished using [the `:where()` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/:where) instead of using classes directly in Astro’s CSS output.

Let’s use the following style block in an Astro component as an example:

```astro
<style>
  div { color: red; } /* 0-0-1 specificity */
</style>
```

Previously, Astro would transform this into the following CSS, which has a specificity of `0-1-1` — a higher specificity than the source CSS:

```css
div.astro-XXXXXX { color: red; } /* 0-1-1 specificity */
```

Now, Astro wraps the class selector with `:where()`, maintaining the authored specificity:

```css
div:where(.astro-XXXXXX) { color: red; } /* 0-0-1 specificity */
```

The previous specificity increase made it hard to combine scoped styles in Astro with other CSS files or styling libraries (e.g. Tailwind, CSS Modules, Styled Components, Stitches). This change will allow Astro’s scoped styles to work consistently alongside them while still preserving the exclusive boundaries that prevent styles from applying outside the component.

Caution

When upgrading, please visually inspect your site output to make sure everything is styled as expected. If not, find your scoped style and increase the selector specificity manually to match the old behavior.

### Deprecated: Components and JSX in Markdown

[Section titled “Deprecated: Components and JSX in Markdown”](#deprecated-components-and-jsx-in-markdown)

Astro no longer supports components or JSX expressions in Markdown pages by default. For long-term support you should migrate to the [`@astrojs/mdx`](/en/guides/integrations-guide/mdx/) integration.

To make migrating easier, a new `legacy.astroFlavoredMarkdown` flag (removed in v2.0) can be used to re-enable previous Markdown features.

### Converting existing `.md` files to `.mdx`

[Section titled “Converting existing .md files to .mdx”](#converting-existing-md-files-to-mdx)

If you’re not familiar with MDX, here are some steps you can follow to quickly convert an existing “Astro Flavored Markdown” file to MDX. As you learn more about MDX, feel free to explore other ways of writing your pages!

1. Install the [`@astrojs/mdx`](/en/guides/integrations-guide/mdx/) integration.

2. Change your existing `.md` file extensions to `.mdx`

3. Remove any `setup:` properties from your frontmatter, and write any import statements below the frontmatter instead.

   src/pages/posts/my-post.mdx

   ```diff
   ---
   layout: '../../layouts/BaseLayout.astro'
   -setup: |
     import ReactCounter from '../../components/ReactCounter.jsx'
   title: 'Migrating to MDX'
   date: 2022-07-26
   tags: ["markdown", "mdx", "astro"]
   ---
   +import ReactCounter from '../../components/ReactCounter.jsx'


   # {frontmatter.title}


   Here is my counter component, working in MDX:


   <ReactCounter client:load />
   ```

4. Update any `Astro.glob()` statements that currently return `.md` files so that they will now return your `.mdx` files.

   Caution

   The object returned when importing `.mdx` files (including using Astro.glob) differs from the object returned when importing `.md` files. However, `frontmatter`, `file`, and `url` work identically.

5. Update any use of the `<Content />` component to use the default export when importing MDX:

   src/pages/index.astro

   ```astro
   ---
   // Multiple imports with Astro.glob
   const mdxPosts = await Astro.glob('./posts/*.mdx');
   ---


   {mdxPosts.map(Post => <Post.default />)}
   ```

   src/pages/index.astro

   ```astro
   ---
   // Import a single page
   import { default as About } from './about.mdx';
   ---


   <About />
   ```

Tip

While you are transitioning to MDX, you may wish to enable the `legacy.astroFlavoredMarkdown` flag (removed in v2.0) and include both **`.md` and `.mdx`** files, so that your site continues to function normally even before all your files have been converted. Here is one way you can do that:

```astro
---
const mdPosts = await Astro.glob('../pages/posts/*.md');
const mdxPosts = await Astro.glob('../pages/posts/*.mdx');
const allPosts = [...mdxPosts, ...mdPosts];
---
```

### `<Markdown />` Component Removed

[Section titled “\<Markdown /> Component Removed”](#markdown--component-removed)

Astro’s built-in `<Markdown />` component has been moved to a separate package. To continue using this component, you will now need to install `@astrojs/markdown-component` and update your imports accordingly. For more details, see [the `@astrojs/markdown` README](https://github.com/withastro/astro/tree/main/packages/markdown/component).

Tip

Astro now has support for [MDX](https://mdxjs.com/) through our [MDX integration](https://github.com/withastro/astro/tree/main/packages/integrations/mdx). MDX gives you the ability to include both Markdown and imported components in the same file. MDX can be good alternative for the `<Markdown />` component due to its large community and stable APIs.

## Migrate to v1.0.0-beta

[Section titled “Migrate to v1.0.0-beta”](#migrate-to-v100-beta)

On April 4, 2022 we released the Astro 1.0 Beta! 🎉

If you are coming from v0.25 or earlier, make sure you have read and followed the [v0.26 Migration Guide](#migrate-to-v026) below, which contained several major breaking changes.

The `v1.0.0-beta.0` release of Astro contained no breaking changes. Below are small changes that were introduced during the beta period.

### Changed: RSS Feeds

[Section titled “Changed: RSS Feeds”](#changed-rss-feeds)

RSS feeds should now be generated using the `@astrojs/rss` package, as described in our [RSS guide](/en/recipes/rss/).

## Migrate to v0.26

[Section titled “Migrate to v0.26”](#migrate-to-v026)

### New Configuration API

[Section titled “New Configuration API”](#new-configuration-api)

Our Configuration API has been redesigned to solve a few glaring points of confusion that had built up over the last year. Most of the configuration options have just been moved or renamed, which will hopefully be a quick update for most users. A few options have been refactored more heavily, and may require a few additional changes:

* `.buildOptions.site` has been replaced with `.site` (your deployed domain) and a new `.base` (your deployed subpath) option.
* `.markdownOptions` has been replaced with `.markdown`, a mostly similar config object with some small changes to simplify Markdown configuration.
* `.sitemap` has been moved into the [@astrojs/sitemap](https://www.npmjs.com/package/@astrojs/sitemap) integration.

If you run Astro with legacy configuration, you will see a warning with instructions on how to update. See our updated [Configuration Reference](/en/reference/configuration-reference/) for more information on upgrading.

Read [RFC0019](https://github.com/withastro/rfcs/blob/main/proposals/0019-config-finalization.md) for more background on these changes.

### New Markdown API

[Section titled “New Markdown API”](#new-markdown-api)

Astro v0.26 releases a brand new Markdown API for your content. This included three major user-facing changes:

* You can now `import`/`import()` markdown content directly using an ESM import.
* A new `Astro.glob()` API, for easier glob imports (especially for Markdown).
* **BREAKING CHANGE:** `Astro.fetchContent()` has been removed and replaced by `Astro.glob()`
* **BREAKING CHANGE:** Markdown objects have an updated interface.

```diff
// v0.25
-let allPosts = Astro.fetchContent('./posts/*.md');
// v0.26+
+let allPosts = await Astro.glob('./posts/*.md');
```

When migrating, be careful about the new Markdown object interface. Frontmatter, for example, has been moved to the `.frontmatter` property, so references like `post.title` should change to `post.frontmatter.title`.

This should solve many issues for Markdown users, including some nice performance boosts for larger sites.

Read [RFC0017](https://github.com/withastro/rfcs/blob/main/proposals/0017-markdown-content-redesign.md) for more background on these changes.

### New Default Script Behavior

[Section titled “New Default Script Behavior”](#new-default-script-behavior)

`<script>` tags in Astro components are now built, bundled and optimized by default. This completes a long-term move to make our Astro component syntax more consistent, matching the default-optimized behavior our `<style>` tags have today.

This includes a few changes to be aware of:

* **BREAKING:** `<script hoist>` is the new default `<script>` behavior. The `hoist` attribute has been removed. To use the new default behaviour, make sure there are no other attributes on the `<script>` tag. For example, remove `type="module"` if you were using it before.
* New `<script is:inline>` directive, to revert a `<script>` tag to previous default behavior (unbuilt, unbundled, untouched by Astro).
* New `<style is:inline>` directive, to leave a style tag inline in the page template (similar to previous `<script>` behavior).
* New `<style is:global>` directive to replace `<style global>` in a future release.

```diff
// v0.25
<script hoist type="module">
// v0.26+
<script>
```

See how to use [client-side scripts](/en/guides/client-side-scripts/) in Astro for full details.

Read [RFC0016](https://github.com/withastro/rfcs/blob/main/proposals/0016-style-script-defaults.md) for more background on these changes.

### Updated `Astro.request` API

[Section titled “Updated Astro.request API”](#updated-astrorequest-api)

`Astro.request` has been changed from our custom object to a standard `Request` object. This is part of a project to use more web standard APIs, especially where SSR is concerned.

This includes a few changes to be aware of:

* Change `Astro.request` to become a [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) object.
* Move `Astro.request.params` to `Astro.params`.
* Move `Astro.request.canonicalURL` to `Astro.canonicalURL`.

Read [RFC0018](https://github.com/withastro/rfcs/blob/main/proposals/0018-astro-request.md) for more background on these changes.

### Other Changes

[Section titled “Other Changes”](#other-changes)

* Improve `Astro.slots` API to support passing arguments to function-based slots. This allows for more ergonomic utility components that accept a callback function as a child.
* Update CLI output formatting, especially around error reporting.
* Update `@astrojs/compiler`, fixing some bugs related to RegExp usage in frontmatter

## Migrate to v0.25

[Section titled “Migrate to v0.25”](#migrate-to-v025)

### Astro Integrations

[Section titled “Astro Integrations”](#astro-integrations)

The `renderers` config has been replaced by a new, official integration system! This unlocks some really exciting new features for Astro. You can read our [Using Integrations](/en/guides/integrations/) guide for more details on how to use this new system.

Integrations replace our original `renderers` concept, and come with a few breaking changes and new defaults for existing users. These changes are covered below.

#### Removed: Built-in Framework Support

[Section titled “Removed: Built-in Framework Support”](#removed-built-in-framework-support)

Previously, React, Preact, Svelte, and Vue were all included with Astro by default. Starting in v0.25.0, Astro no longer comes with any built-in renderers. If you did not have a `renderers` configuration entry already defined for your project, you will now need to install those frameworks yourself.

Read our [step-by-step walkthrough](/en/guides/integrations/) to learn how to add a new Astro integration for the framework(s) that you currently use.

#### Deprecated: Renderers

[Section titled “Deprecated: Renderers”](#deprecated-renderers)

Note

Read this section if you have custom “renderers” already defined in your configuration file.

The new integration system replaces the previous `renderers` system, including the published `@astrojs/renderer-*` packages on npm. Going forward, `@astrojs/renderer-react` becomes `@astrojs/react`, `@astrojs/renderer-vue` becomes `@astrojs/vue`, and so on.

**To migrate:** update Astro to `v0.25.0` and then run `astro dev` or `astro build` with your old configuration file containing the outdated `"renderers"` config. You will immediately see a notice telling you the exact changes you need to make to your `astro.config.mjs` file, based on your current config. You can also update your packages yourself, using the table below.

For a deeper walkthrough, read our [step-by-step guide](/en/guides/integrations/) to learn how to replace existing renderers with a new Astro framework integration.

```diff
# Install your new integrations and frameworks:
# (Read the full walkthrough: https://docs.astro.build/en/guides/integrations-guide)
+npm install @astrojs/lit lit
+npm install @astrojs/react react react-dom
```

```diff
// Then, update your `astro.config.mjs` file:
// (Read the full walkthrough: https://docs.astro.build/en/guides/integrations-guide)
+import lit from '@astrojs/lit';
+import react from '@astrojs/react';


export default {
-  renderers: ['@astrojs/renderer-lit', '@astrojs/renderer-react'],
+  integrations: [lit(), react()],
}
```

| Deprecated Renderers on npm | v0.25+ Integrations on npm |
| --------------------------- | -------------------------- |
| @astrojs/renderer-react     | @astrojs/react             |
| @astrojs/renderer-preact    | @astrojs/preact            |
| @astrojs/renderer-solid     | @astrojs/solid-js          |
| @astrojs/renderer-vue       | @astrojs/vue               |
| @astrojs/renderer-svelte    | @astrojs/svelte            |

#### Handling Peer Dependencies

[Section titled “Handling Peer Dependencies”](#handling-peer-dependencies)

Note

Read this section if: You are on Node v14 **or** if you use any package manager other than npm.

Unlike the old renderers, integrations no longer mark the frameworks themselves (“react”, “svelte”, “vue”, etc.) as direct dependencies of the integration. Instead, you should now install your framework packages *in addition to* your integrations.

```shell
# Example: Install integrations and frameworks together
npm install @astrojs/react react react-dom
```

If you see a `"Cannot find package 'react'"` (or similar) warning when you start up Astro, that means that you need to install that package into your project. See our [note on peer dependencies](/en/guides/troubleshooting/#cannot-find-package-x) in the troubleshooting guide for more information.

If you are using `npm` & Node v16+, then this may be automatically handled for you by `npm`, since the latest version of `npm` (v7+) installs peer dependencies like this for you automatically. In that case, installing a framework like “react” into your project is an optional but still recommended step.

### Updated: Syntax Highlighting

[Section titled “Updated: Syntax Highlighting”](#updated-syntax-highlighting)

We love to find sensible defaults that “just work” out-of-the-box. As part of this, we decided to make [Shiki](https://github.com/shikijs/shiki) our new default syntax highlighter. This comes pre-configured with the `github-dark` theme, providing zero-config highlighting in your code blocks without extraneous CSS classes, stylesheets, or client-side JS.

Check our new syntax highlighting docs for full details. **If you prefer to keep Prism as your syntax highlighter,** set the `syntaxHighlight` option to `'prism'` in your project’s markdown configuration.

#### The `<Prism />` component has a new home

[Section titled “The \<Prism /> component has a new home”](#the-prism--component-has-a-new-home)

As part of our mission to keep Astro core as lean as possible, we’ve moved the built-in `Prism` component out of `astro/components` and into the `@astrojs/prism` package. You can now import this component from `@astrojs/prism` like so:

```astro
---
import { Prism } from '@astrojs/prism';
---
```

Since the `@astrojs/prism` package is still bundled with `astro` core, you won’t need to install anything new, nor add Prism as an integration! However, note that we *do* plan to extract `@astrojs/prism` (and Prism syntax highlighting in general) to a separate, installable package in the future. See the `<Prism />` component API reference for more details.

### CSS Parser Upgrade

[Section titled “CSS Parser Upgrade”](#css-parser-upgrade)

Our internal CSS parser has been updated, and comes with better support for advanced CSS syntax, like container queries. This should be a mostly invisible change for most users, but hopefully advanced users will enjoy the new CSS feature support.

## Migrate to v0.24

[Section titled “Migrate to v0.24”](#migrate-to-v024)

Note

The new build strategy is on by default on 0.24. If you run into a problem you can continue using the old build strategy by passing the `--legacy-build` flag. Please [open an issue](https://github.com/withastro/astro/issues/new/choose) so that we can resolve problems with the new build strategy.

0.24 introduced a new *static build* strategy that changes the behavior of a few features. In previous versions of Astro this was available behavior with an opt-in flag: `--experimental-static-build`.

To migrate for the transition, be aware of the following changes that will be required to move to this new build engine. You can make these changes to your codebase at any time so that you are ready ahead of schedule.

### Deprecated: `Astro.resolve()`

[Section titled “Deprecated: Astro.resolve()”](#deprecated-astroresolve)

`Astro.resolve()` allows you to get resolved URLs to assets that you might want to reference in the browser. This was most commonly used inside of `<link>` and `<img>` tags to load CSS files and images as needed. Unfortunately, this will no longer work due to Astro now building assets at *build time* rather than at *render time*. You’ll want to upgrade your asset references to one of the following future-proof options available going forward:

#### How to Resolve CSS Files

[Section titled “How to Resolve CSS Files”](#how-to-resolve-css-files)

**1. ESM Import (Recommended)**

**Example:** `import './style.css';` **When to use this:** If your CSS file lives inside of the `src/` directory, and you want automatic CSS build and optimization features.

Use an ESM import to add some CSS onto the page. Astro detects these CSS imports and then builds, optimizes, and adds the CSS to the page automatically. This is the easiest way to migrate from `Astro.resolve()` while keeping the automatic building/bundling that Astro provides.

```astro
---
// Example: Astro will include and optimize this CSS for you automatically
import './style.css';
---
<html><!-- Your page here --></html>
```

Importing CSS files should work anywhere that ESM imports are supported, including:

* JavaScript files
* TypeScript files
* Astro component frontmatter
* non-Astro components like React, Svelte, and others

When a CSS file is imported using this method, any `@import` statements are also resolved and inlined into the imported CSS file. All `url()` references are also resolved relative to the source file, and any `url()` referenced assets will be included in the final build.

**2. Absolute URL Path**

**Example:** `<link href="/style.css">` **When to use this:** If your CSS file lives inside of `public/`, and you prefer to create your HTML `link` element yourself.

You can reference any file inside of the `public/` directory by absolute URL path in your component template. This is a good option if you want to control the `<link>` tag on the page yourself. However, this approach also skips the CSS processing, bundling and optimizations that are provided by Astro when you use the `import` method described above.

We recommend using the `import` approach over the absolute URL approach since it provides the best possible CSS performance and features by default.

#### How to Resolve JavaScript Files

[Section titled “How to Resolve JavaScript Files”](#how-to-resolve-javascript-files)

**1. Absolute URL Path**

**Example:** `<script src="/some-external-script.js" />` **When to use this:** If your JavaScript file lives inside of `public/`.

You can reference any file inside of the `public/` directory by absolute URL path in your Astro component templates. This is a good default option for external scripts because it lets you control the `<script>` tag on the page yourself.

Note that this approach skips the JavaScript processing, bundling and optimizations that are provided by Astro when you use the `import` method described below. However, this may be preferred for any external scripts that have already been published and minified separately from Astro. If your script was downloaded from an external source, then this method is probably preferred.

**2. ESM Import via `<script hoist>`**

**Example:** `<script hoist>import './some-external-script.js';</script>` **When to use this:** If your external script lives inside of `src/` *and* it supports the ESM module type.

Use an ESM import inside of a `<script hoist>` element in your Astro template, and Astro will include the JavaScript file in your final build. Astro detects these JavaScript client-side imports and then builds, optimizes, and adds the JavaScript to the page automatically. This is the easiest way to migrate from `Astro.resolve()` while keeping the automatic building/bundling that Astro provides.

```astro
<script hoist>
  import './some-external-script.js';
</script>
```

Note that Astro will bundle this external script with the rest of your client-side JavaScript, and load it in the `type="module"` script context. Some older JavaScript files may not be written for the `module` context, in which case they may need to be updated to use this method.

#### How to Resolve Images & Other Assets

[Section titled “How to Resolve Images & Other Assets”](#how-to-resolve-images--other-assets)

**1. Absolute URL Path (Recommended)**

**Example:** `<img src="/penguin.png">` **When to use this:** If your asset lives inside of `public/`.

If you place your images inside of `public/` you can safely reference them by absolute URL path directly in your component templates. This is the simplest way to reference an asset that you can use today, and it is recommended for most users who are getting started with Astro.

**2. ESM Import**

**Example:** `import imgUrl from './penguin.png'` **When to use this:** If your asset lives inside of the `src/` directory, and you want automatic optimization features like filename hashing.

This works inside of any JavaScript or Astro component, and returns a resolved URL to the final image. Once you have the resolved URL, you can use it anywhere inside of the component template.

```astro
---
// Example: Astro will include this image file in your final build
import imgUrl from './penguin.png';
---
<img src={imgUrl} />
```

Similar to how Astro handles CSS, the ESM import allows Astro to perform some simple build optimizations for you automatically. For example, any asset inside of `src/` that is imported using an ESM import (ex: `import imgUrl from './penguin.png'`) will have its filename hashed automatically. This can let you cache the file more aggressively on the server, improving user performance. In the future, Astro may add more optimizations like this.

**Tip:** If you dislike static ESM imports, Astro also supports dynamic ESM imports. We only recommend this option if you prefer this syntax: `<img src={(await import('./penguin.png')).default} />`.

### Deprecated: `<script>` Default Processing

[Section titled “Deprecated: \<script> Default Processing”](#deprecated-script-default-processing)

Previously, all `<script>` elements were read from the final HTML output and processed + bundled automatically. This behavior is no longer the default. Starting in 0.24, you must opt-in to `<script>` element processing via the `hoist` attribute. The `type="module"` is also required for hoisted modules.

```astro
<script>
  // Will be rendered into the HTML exactly as written!
  // ESM imports will not be resolved relative to the file.
</script>
<script type="module" hoist>
  // Processed! Bundled! ESM imports work, even to npm packages.
</script>
```

## Migrate to v0.23

[Section titled “Migrate to v0.23”](#migrate-to-v023)

### Missing Sass Error

[Section titled “Missing Sass Error”](#missing-sass-error)

```plaintext
Preprocessor dependency "sass" not found. Did you install it?
```

In our quest to reduce npm install size, we’ve moved [Sass](https://sass-lang.com/) out to an optional dependency. If you use Sass in your project, you’ll want to make sure that you run `npm install sass --save-dev` to save it as a dependency.

### Deprecated: Unescaped HTML

[Section titled “Deprecated: Unescaped HTML”](#deprecated-unescaped-html)

In Astro v0.23+, unescaped HTML content in expressions is now deprecated. In future releases, content within expressions will have strings escaped to protect against unintended HTML injection.

```diff
<h1>{title}</h1> <!-- <h1>Hello <strong>World</strong></h1> -->
<h1>{title}</h1> <!-- <h1>Hello &lt;strong&gt;World&lt;/strong&gt;</h1> -->
```

To continue injecting unescaped HTML, you can now use `set:html`.

```diff
<h1>{title}</h1>
<h1 set:html={title} />
```

To avoid a wrapper element, `set:html` can work alongside `<Fragment>`.

```diff
<h1>{title}!</h1>
<h1><Fragment set:html={title}>!</h1>
```

You can also protect against unintended HTML injection with `set:text`.

```astro
<h1 set:text={title} /> <!-- <h1>Hello &lt;strong&gt;World&lt;/strong&gt;</h1> -->
```

## Migrate to v0.21

[Section titled “Migrate to v0.21”](#migrate-to-v021)

### Vite

[Section titled “Vite”](#vite)

Starting in v0.21, Astro is built with [Vite](https://vite.dev). As a result, configurations written in `snowpack.config.mjs` should be moved into `astro.config.mjs`.

```js
// @ts-check


/** @type {import('astro').AstroUserConfig} */
export default {
  renderers: [],
  vite: {
    plugins: [],
  },
};
```

To learn more about configuring Vite, please visit their [configuration guide](https://vite.dev/config/).

#### Vite Plugins

[Section titled “Vite Plugins”](#vite-plugins)

In Astro v0.21+, Vite plugins may be configured within `astro.config.mjs`.

```diff
import { imagetools } from 'vite-imagetools';


export default {
+  vite: {
+    plugins: [imagetools()],
+  },
};
```

To learn more about Vite plugins, please visit their [plugin guide](https://vite.dev/guide/using-plugins.html).

#### Vite Changes to Renderers

[Section titled “Vite Changes to Renderers”](#vite-changes-to-renderers)

In Astro v0.21+, plugins should now use `viteConfig()`.

renderer-svelte/index.js

```diff
+import { svelte } from '@sveltejs/vite-plugin-svelte';


export default {
  name: '@astrojs/renderer-svelte',
  client: './client.js',
  server: './server.js',
-  snowpackPlugin: '@snowpack/plugin-svelte',
-  snowpackPluginOptions: { compilerOptions: { hydratable: true } },
  +viteConfig() {
    +return {
+      optimizeDeps: {
+        include: ['@astrojs/renderer-svelte/client.js', 'svelte', 'svelte/internal'],
+        exclude: ['@astrojs/renderer-svelte/server.js'],
+      },
+      plugins: [
        +svelte({
+          emitCss: true,
+          compilerOptions: { hydratable: true },
+        }),
+      ],
+    };
+  },
}
```

To learn more about Vite plugins, please visit their [plugin guide](https://vite.dev/guide/using-plugins.html).

Note

In prior releases, these were configured with `snowpackPlugin` or `snowpackPluginOptions`.

### Aliasing

[Section titled “Aliasing”](#aliasing)

In Astro v0.21+, import aliases can be added in `tsconfig.json`.

```diff
{
  "compilerOptions": {
    "baseUrl": ".",
    +"paths": {
      +"@/components/*": ["src/components/*"]
+    }
  }
}
```

### File Extensions in Imports

[Section titled “File Extensions in Imports”](#file-extensions-in-imports)

In Astro v0.21+, files need to be referenced by their actual extension, exactly as it is on disk. In this example, `Div.tsx` would need to be referenced as `Div.tsx`, not `Div.jsx`.

```diff
-import Div from './Div.jsx' // Astro v0.20
+import Div from './Div.tsx' // Astro v0.21
```

This same change applies to a compile-to-css file like `Div.scss`:

```diff
<link rel="stylesheet" href={Astro.resolve('./Div.css')}>
<link rel="stylesheet" href={Astro.resolve('./Div.scss')}>
```

### Removed: Components in Frontmatter

[Section titled “Removed: Components in Frontmatter”](#removed-components-in-frontmatter)

Previously, you could create mini Astro Components inside of the Astro Frontmatter, using JSX syntax instead of Astro’s component syntax. This was always a bit of a hack, but in the new compiler it became impossible to support. We hope to re-introduce this feature in a future release of Astro using a different, non-JSX API.

To migrate to v0.21+, please convert all JSX Astro components (that is, any Astro components created inside of another component’s frontmatter) to standalone components.

### Styling Changes

[Section titled “Styling Changes”](#styling-changes)

#### Autoprefixer

[Section titled “Autoprefixer”](#autoprefixer)

Autoprefixer is no longer run by default. To enable:

1. Install the latest version (`npm install autoprefixer`)

2. Create a `postcss.config.cjs` file at the root of your project with:

   ```js
   module.exports = {
     plugins: {
       autoprefixer: {},
     },
   };
   ```

#### Tailwind CSS

[Section titled “Tailwind CSS”](#tailwind-css)

Ensure you have PostCSS installed. This was optional in previous releases, but is required now:

1. Install the latest version of postcss (`npm install -D postcss`)

2. Create a `postcss.config.cjs` file at the root of your project with:

   ```js
   module.exports = {
     plugins: {
       tailwindcss: {},
     },
   };
   ```

   For more information, read the [Tailwind CSS documentation](https://tailwindcss.com/docs/installation#add-tailwind-as-a-post-css-plugin)

### Known Issues

[Section titled “Known Issues”](#known-issues)

#### Imports on top

[Section titled “Imports on top”](#imports-on-top)

In Astro v0.21+, a bug has been introduced that requires imports inside components to be at the top of your frontmatter.

```astro
---
import Component from '../components/Component.astro'
const whereShouldIPutMyImports = "on top!"
---
```

# Upgrade to Astro v2

> How to upgrade your project to the latest version of Astro.

This guide will help you migrate from Astro v1 to Astro v2.

Need to upgrade an older project to v1? See our [older migration guide](/en/guides/upgrade-to/v1/).

## Upgrade Astro

[Section titled “Upgrade Astro”](#upgrade-astro)

Update your project’s version of Astro to the latest version using your package manager. If you’re using Astro integrations, please also update those to the latest version.

* npm

  ```shell
  # Upgrade to Astro v2.x
  npm install astro@latest


  # Example: upgrade React and Tailwind integrations
  npm install @astrojs/react@latest @astrojs/tailwind@latest
  ```

* pnpm

  ```shell
  # Upgrade to Astro v2.x
  pnpm add astro@latest


  # Example: upgrade React and Tailwind integrations
  pnpm add @astrojs/react@latest @astrojs/tailwind@latest
  ```

* Yarn

  ```shell
  # Upgrade to Astro v2.x
  yarn add astro@latest


  # Example: upgrade React and Tailwind integrations
  yarn add @astrojs/react@latest @astrojs/tailwind@latest
  ```

## Astro v2.0 Breaking Changes

[Section titled “Astro v2.0 Breaking Changes”](#astro-v20-breaking-changes)

Astro v2.0 includes some breaking changes, as well as the removal of some previously deprecated features. If your project doesn’t work as expected after upgrading to v2.0, check this guide for an overview of all breaking changes and instructions on how to update your codebase.

See [the changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for full release notes.

### Removed: Support for Node 14

[Section titled “Removed: Support for Node 14”](#removed-support-for-node-14)

Node 14 is scheduled to reach its End of Life in April 2023.

Astro v2.0 drops Node 14 support entirely, so that all Astro users can take advantage of Node’s more modern features.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do)

Check that both your development environment and your deployment environment are using **Node `16.12.0` or later**.

1. Check your local version of Node using:

   ```sh
   node -v
   ```

   If your local development environment needs upgrading, [install Node](https://nodejs.org/en/download/).

2. Check your [deployment environment’s](/en/guides/deploy/) own documentation to verify that they support Node 16.

   You can specify Node `16.12.0` for your Astro project either in a dashboard configuration setting, or a `.nvmrc` file.

### Reserved: `src/content/`

[Section titled “Reserved: src/content/”](#reserved-srccontent)

Astro v2.0 now includes the Collections API for organizing your Markdown and MDX files into [content collections](/en/guides/content-collections/). This API reserves `src/content/` as a special folder.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-1)

Rename an existing `src/content/` folder to avoid conflicts. This folder, if it exists, can now only be used for [content collections](/en/guides/content-collections/).

### Changed: `Astro.site` trailing slash

[Section titled “Changed: Astro.site trailing slash”](#changed-astrosite-trailing-slash)

In v1.x, Astro ensured the URL you set as `site` in `astro.config.mjs` always had a trailing slash when accessed using `Astro.site`.

Astro v2.0 no longer modifies the value of `site`. `Astro.site` will use the exact value defined, and a trailing slash must be specified if desired.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-2)

In `astro.config.mjs`, add a trailing slash to the URL set in `site`.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
-  site: 'https://example.com',
+  site: 'https://example.com/',
});
```

### Changed: `_astro/` folder for build assets

[Section titled “Changed: \_astro/ folder for build assets”](#changed-_astro-folder-for-build-assets)

In v1.x, assets were built to various locations, including `assets/`, `chunks/`, and to the root of the build output.

Astro v2.0 moves and unifies the location of all build output assets to a new `_astro/` folder.

* dist/

  * \_astro

    * client.9218e799.js
    * index.df3f880e0.css

You can control this location with the [new `build.assets` configuration option](/en/reference/configuration-reference/#buildassets).

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-3)

Update your deployment platform configuration if it relies on the location of these assets.

### Changed: Markdown plugin configuration

[Section titled “Changed: Markdown plugin configuration”](#changed-markdown-plugin-configuration)

#### Removed: `extendDefaultPlugins`

[Section titled “Removed: extendDefaultPlugins”](#removed-extenddefaultplugins)

In v1.x, Astro used `markdown.extendDefaultPlugins` to re-enable Astro’s default plugins when adding your own Markdown plugins.

Astro v2.0 removes this configuration option entirely because its behavior is now the default.

Applying remark and rehype plugins in your Markdown configuration **no longer disables Astro’s default plugins**. GitHub-Flavored Markdown and Smartypants are now applied whether or not custom `remarkPlugins` or `rehypePlugins` are configured.

##### What should I do?

[Section titled “What should I do?”](#what-should-i-do-4)

Remove `extendDefaultPlugins` in your configuration. This is now Astro’s default behavior in v2.0, and you can delete this line without any replacement.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
-    extendDefaultPlugins,
  }
});
```

#### Added: `gfm` and `smartypants`

[Section titled “Added: gfm and smartypants”](#added-gfm-and-smartypants)

In v1.x, you could choose to disable both of Astro’s default Markdown plugins (GitHub-Flavored Markdown and SmartyPants) by setting `markdown.extendDefaultPlugins: false`.

Astro v2.0 replaces `markdown.extendDefaultPlugins: false` with separate Boolean options to individually control each of Astro’s built-in default Markdown plugins. These are enabled by default and can be set to `false` independently.

##### What should I do?

[Section titled “What should I do?”](#what-should-i-do-5)

Remove `extendDefaultPlugins: false` and add the flags to disable each plugin individually instead.

* `markdown.gfm: false` disables GitHub-Flavored Markdown
* `markdown.smartypants: false` disables SmartyPants

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
-    extendDefaultPlugins: false,
+    smartypants: false,
+    gfm: false,
  }
});
```

### Changed: MDX plugin configuration

[Section titled “Changed: MDX plugin configuration”](#changed-mdx-plugin-configuration)

#### Replaced: `extendPlugins` changed to `extendMarkdownConfig`

[Section titled “Replaced: extendPlugins changed to extendMarkdownConfig”](#replaced-extendplugins-changed-to-extendmarkdownconfig)

In v1.x, the MDX integration’s `extendPlugins` option managed how your MDX files should inherit your Markdown configuration: all your Markdown configuration (`markdown`), or Astro’s default plugins only (`default`).

Astro v2.0 replaces the behavior controlled by `mdx.extendPlugins` with three new, independently-configurable options that are `true` by default:

* **[`mdx.extendMarkdownConfig`](/en/guides/integrations-guide/mdx/#extendmarkdownconfig)** to inherit all or none of your Markdown configuration
* **`mdx.gfm`** to enable or disable GitHub-Flavored Markdown in MDX
* **`mdx.smartypants`** to enable or disable SmartyPants in MDX

##### What should I do?

[Section titled “What should I do?”](#what-should-i-do-6)

Delete `extendPlugins: 'markdown'` in your configuration. This is now the default behavior.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';


export default defineConfig({
  integrations: [
    mdx({
-      extendPlugins: 'markdown',
    }),
  ],
});
```

Replace `extendPlugins: 'defaults'` with `extendMarkdownConfig: false` and add the separate options for GitHub-Flavored Markdown and SmartyPants to enable these default plugins individually in MDX.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';


export default defineConfig({
  integrations: [
    mdx({
-      extendPlugins: 'defaults',
+      extendMarkdownConfig: false,
+      smartypants: true,
+      gfm: true,
    }),
  ],
});
```

#### Added: More MDX config options to match Markdown

[Section titled “Added: More MDX config options to match Markdown”](#added-more-mdx-config-options-to-match-markdown)

Astro v2.0 allows you to now individually set [every available Markdown configuration option](/en/reference/configuration-reference/#markdown-options) (except `drafts`) separately in your MDX integration configuration.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';


export default defineConfig({
  markdown: {
    remarkPlugins: [remarkPlugin1],
    gfm: true,
  },
  integrations: [
    mdx({
      remarkPlugins: [remarkPlugin2],
      gfm: false,
    })
  ]
});
```

##### What should I do?

[Section titled “What should I do?”](#what-should-i-do-7)

Revisit your Markdown and MDX configuration and compare your existing config with the new options available.

### Changed: Plugin access to frontmatter

[Section titled “Changed: Plugin access to frontmatter”](#changed-plugin-access-to-frontmatter)

In v1.x, remark and rehype plugins did not have access to user frontmatter. Astro merged plugin frontmatter with your file’s frontmatter, without passing the file frontmatter to your plugins.

Astro v2.0 gives remark and rehype plugins access to user frontmatter via frontmatter injection. This allows plugin authors to modify a user’s existing frontmatter, or compute new properties based on other properties.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-8)

Check any remark and rehype plugins you have written to see whether their behavior has changed. Note that `data.astro.frontmatter` is now the *complete* Markdown or MDX document’s frontmatter, rather than an empty object.

### Changed: RSS Configuration

[Section titled “Changed: RSS Configuration”](#changed-rss-configuration)

In v1.x, Astro’s RSS package allowed you to use `items: import.meta.glob(...)` to generate a list of RSS feed items. This usage is now deprecated and will eventually be removed.

Astro v2.0 introduces a `pagesGlobToRssItems()` wrapper to the `items` property.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-9)

Import, then wrap your existing function containing `import.meta.glob()` with the `pagesGlobToRssItems()` helper.

src/pages/rss.xml.js

```diff
import rss, {
+  pagesGlobToRssItems
} from '@astrojs/rss';


export async function get(context) {
  return rss({
+    items: await pagesGlobToRssItems(
      import.meta.glob('./blog/*.{md,mdx}'),
+    ),
  });
}
```

### Changed: Svelte IDE support

[Section titled “Changed: Svelte IDE support”](#changed-svelte-ide-support)

Astro v2.0 requires a `svelte.config.js` file in your project if you are using [the `@astrojs/svelte` integration](/en/guides/integrations-guide/svelte/). This is needed to provide IDE autocompletion.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-10)

Add a `svelte.config.js` file to the root of your project:

svelte.config.js

```js
import { vitePreprocess } from '@astrojs/svelte';


export default {
  preprocess: vitePreprocess(),
};
```

For new users, this file will be added automatically when running `astro add svelte`.

### Removed: `legacy.astroFlavoredMarkdown`

[Section titled “Removed: legacy.astroFlavoredMarkdown”](#removed-legacyastroflavoredmarkdown)

In v1.0, Astro moved the old Astro-Flavored Markdown (also known as Components in Markdown) to a legacy feature.

Astro v2.0 removes the `legacy.astroFlavoredMarkdown` option completely. Importing and using components in `.md` files will no longer work.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-11)

Remove this legacy flag. It is no longer available in Astro.

astro.config.mjs

```diff
export default defineConfig({
-  legacy: {
-    astroFlavoredMarkdown: true,
-  },
})
```

If you were using this feature in v1.x, we recommend [using the MDX integration](/en/guides/integrations-guide/mdx/) which allows you to combine components and JSX expressions with Markdown syntax.

### Removed: `Astro.resolve()`

[Section titled “Removed: Astro.resolve()”](#removed-astroresolve)

In v0.24, Astro deprecated `Astro.resolve()` for getting resolved URLs to assets that you might want to reference in the browser.

Astro v2.0 removes this option entirely. `Astro.resolve()` in your code will cause an error.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-12)

Resolve asset paths using `import` instead. For example:

src/pages/index.astro

```astro
---
import 'style.css';
import imageUrl from './image.png';
---


<img src={imageUrl} />
```

### Removed: `Astro.fetchContent()`

[Section titled “Removed: Astro.fetchContent()”](#removed-astrofetchcontent)

In v0.26, Astro deprecated `Astro.fetchContent()` for fetching data from your local Markdown files.

Astro v2.0 removes this option entirely. `Astro.fetchContent()` in your code will cause an error.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-13)

Use `Astro.glob()` to fetch Markdown files, or convert to the [Content Collections](/en/guides/content-collections/) feature.

src/pages/index.astro

```astro
---
const allPosts = await Astro.glob('./posts/*.md');
---
```

### Removed: `Astro.canonicalURL`

[Section titled “Removed: Astro.canonicalURL”](#removed-astrocanonicalurl)

In v1.0, Astro deprecated `Astro.canonicalURL` for constructing a canonical URL.

Astro v2.0 removes this option entirely. `Astro.canonicalURL` in your code will cause an error.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-14)

Use `Astro.url` to construct a canonical URL.

src/pages/index.astro

```astro
---
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
---
```

### Updated: Vite 4

[Section titled “Updated: Vite 4”](#updated-vite-4)

Astro v2.0 upgrades from Vite 3 to [Vite 4](https://vite.dev/), released in December 2022.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-15)

There should be no changes to your code necessary! We’ve handled most of the upgrade for you inside of Astro; however, some subtle Vite behaviors may still change between versions.

Refer to the official [Vite Migration Guide](https://vite.dev/guide/migration.html) if you run into trouble.

## Astro v2.0 Experimental Flags Removed

[Section titled “Astro v2.0 Experimental Flags Removed”](#astro-v20-experimental-flags-removed)

Remove the following experimental flags from `astro.config.mjs`:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
-  experimental: {
-    contentCollections: true,
-    prerender: true,
-    errorOverlay: true,
-  },
})
```

These features are now available by default:

* [Content collections](/en/guides/content-collections/) as a way to manage your Markdown and MDX files with type-safety.
* [Prerendering individual pages to static HTML](/en/guides/on-demand-rendering/) when using SSR to improve speed and cacheability.
* A redesigned error message overlay.

## Known Issues

[Section titled “Known Issues”](#known-issues)

There are currently no known issues.

# Upgrade to Astro v3

> How to upgrade your project to the latest version of Astro (v3.0).

This guide will help you migrate from Astro v2 to Astro v3.

Need to upgrade an older project to v2? See our [older migration guide](/en/guides/upgrade-to/v2/).

## Upgrade Astro

[Section titled “Upgrade Astro”](#upgrade-astro)

Update your project’s version of Astro to the latest version using your package manager. If you’re using Astro integrations, please also update those to the latest version.

* npm

  ```shell
  # Upgrade to Astro v3.x
  npm install astro@latest


  # Example: upgrade React and Tailwind integrations
  npm install @astrojs/react@latest @astrojs/tailwind@latest
  ```

* pnpm

  ```shell
  # Upgrade to Astro v3.x
  pnpm add astro@latest


  # Example: upgrade React and Tailwind integrations
  pnpm add @astrojs/react@latest @astrojs/tailwind@latest
  ```

* Yarn

  ```shell
  # Upgrade to Astro v3.x
  yarn add astro@latest


  # Example: upgrade React and Tailwind integrations
  yarn add @astrojs/react@latest @astrojs/tailwind@latest
  ```

Need to continue?

After upgrading Astro to the latest version, you may not need to make any changes to your project at all!

But, if you notice errors or unexpected behavior, please check below for what has changed that might need updating in your project.

## Astro v3.0 Experimental Flags Removed

[Section titled “Astro v3.0 Experimental Flags Removed”](#astro-v30-experimental-flags-removed)

Remove the following experimental flags from `astro.config.mjs`:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
-  experimental: {
-    assets: true,
-    viewTransitions: true,
-  },
})
```

These features are now available by default:

* View Transitions for animated page transitions and persistent islands. See [view transitions API breaking changes and upgrading advice](#upgrade-view-transitions-to-v3) if you were using this experimental flag.
* A new image services API `astro:assets` for using images in Astro, including a new `<Image />` component and `getImage()` function. Please read the detailed [image upgrade advice](#upgrade-images-to-v3) **whether or not you were using this experimental flag** to see how this might affect your project.

Read more about these two exciting features and more in [the 3.0 Blog post](https://astro.build/blog/astro-3/)!

## Astro v3.0 Breaking Changes

[Section titled “Astro v3.0 Breaking Changes”](#astro-v30-breaking-changes)

Astro v3.0 includes some breaking changes, as well as the removal of some previously deprecated features. If your project doesn’t work as expected after upgrading to v3.0, check this guide for an overview of all breaking changes and instructions on how to update your codebase.

See [the changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for full release notes.

### Removed: Support for Node 16

[Section titled “Removed: Support for Node 16”](#removed-support-for-node-16)

Node 16 is scheduled to reach its End of Life in September 2023.

Astro v3.0 drops Node 16 support entirely so that all Astro users can take advantage of Node’s more modern features.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do)

Check that both your development environment and your deployment environment are using **Node `18.14.1` or higher**.

1. Check your local version of Node using:

   ```sh
   node -v
   ```

2. Check your [deployment environment’s](/en/guides/deploy/) own documentation to verify that they support Node 18.

   You can specify Node `18.14.1` for your Astro project either in a dashboard configuration setting or a `.nvmrc` file.

   .nvmrc

   ```bash
   18.14.1
   ```

### Removed: Support for TypeScript 4

[Section titled “Removed: Support for TypeScript 4”](#removed-support-for-typescript-4)

In Astro v2.x, the `tsconfig.json` presets include support for both TypeScript 4.x and 5.x.

Astro v3.0 updates the `tsconfig.json` presets to only support TypeScript 5.x. Astro now assumes that you use TypeScript 5.0 (March 2023), or that your editor includes it (e.g. VS Code 1.77).

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-1)

If you have installed TypeScript locally, update to at least v5.0.

```bash
npm install typescript@latest --save-dev
```

### Removed: `@astrojs/image`

[Section titled “Removed: @astrojs/image”](#removed-astrojsimage)

In Astro v2.x, Astro offered an official image integration that included Astro `<Image />` and `<Picture />` components.

Astro v3.0 removes this integration from the codebase entirely. Astro’s new solution for images is a built-in image services API: `astro:assets`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-2)

Remove the `@astrojs/image` integration from your project. You will need to not only uninstall the integration but also update or remove any import statements and existing `<Image />` and `<Picture />` components. You might also need to configure a preferred default image processing service.

You will find [complete, step-by-step instructions for removing the old image integration](#remove-astrojsimage) in our Images guide.

Migrating to `astro:assets` will also bring some new image options and features that you may now wish to use. Please see the full [v3.0 Image Upgrade Advice](#upgrade-images-to-v3) for full details!

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
-import image from '@astrojs/image';


export default defineConfig({
  integrations: [
    -image(),
  ]
})
```

### Removed: `<Markdown />` component

[Section titled “Removed: \<Markdown /> component”](#removed-markdown--component)

In Astro v1.x, Astro deprecated the `<Markdown />` component and moved it to an external package.

Astro v3.0 completely removes the package `@astrojs/markdown-component`. Astro’s `<Markdown />` component will no longer work in your project.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-3)

Remove all instances of the `@astrojs/markdown-component`.

src/components/MyAstroComponent.astro

```diff
---
-import Markdown from '@astrojs/markdown-component';
---
```

To continue using a similar `<Markdown />` component in your code, consider using [community integrations](https://astro.build/integrations/) such as [`astro-remote`](https://github.com/natemoo-re/astro-remote). Be sure to update your `<Markdown />` component imports and attributes as necessary, according to the integration’s own documentation.

Otherwise, delete all references to importing Astro’s `<Markdown />` component and the component itself in your `.astro` files. You will need to rewrite your content as HTML directly or [import Markdown](/en/guides/markdown-content/#importing-markdown) from a `.md` file.

### Removed: deprecated 1.x APIs

[Section titled “Removed: deprecated 1.x APIs”](#removed-deprecated-1x-apis)

In Astro v1.x, Astro deprecated our original configuration settings as well as `<style global>` and `<script hoist>` support. However, these were still supported for backwards compatibility.

Astro v3.0 removes these deprecated APIs entirely. The officially supported [configuration settings](/en/reference/configuration-reference/) and modern `<style is:global>` and `<script>` syntax should be used instead.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-4)

If you are continuing to use v1.x APIs, use the new APIs for each feature instead:

* Deprecated config options: See [the 0.26 migration guide](/en/guides/upgrade-to/v1/#new-configuration-api)
* Deprecated script/style attribute types: See [the 0.26 migration guide](/en/guides/upgrade-to/v1/#new-default-script-behavior)

### Removed: Partial shims for Web APIs in server code

[Section titled “Removed: Partial shims for Web APIs in server code”](#removed-partial-shims-for-web-apis-in-server-code)

In Astro v2.x, Astro provided partial shims for Web APIs such as `document` or `localStorage` in server-rendered code. These shims were often incomplete and unreliable.

Astro v3.0 removes these partial shims entirely. Web APIs are no longer available in server-rendered code.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-5)

If you are using Web APIs in server-rendered components, you will need to either make the usage of those APIs conditional or use [the `client:only` client directive](/en/reference/directives-reference/#clientonly).

### Removed: `image` from `astro:content` in content collections schema

[Section titled “Removed: image from astro:content in content collections schema”](#removed-image-from-astrocontent-in-content-collections-schema)

In Astro v2.x, the content collections API deprecated an `image` export from `astro:content` for use in your content collections schemas.

Astro v3.0 removes this export entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-6)

If you are using the deprecated `image()` from `astro:content`, remove it as this no longer exists. Validate images through [the `image` helper from `schema`](#update-content-collections-schemas) instead:

src/content/config.ts

```diff
-import { defineCollection, z, image } from "astro:content";
+import { defineCollection, z } from "astro:content";


defineCollection({
  schema: ({ image }) =>
    z.object({
      image: image(),
   }),
});
```

### Removed: pre-0.14 Shiki theme names

[Section titled “Removed: pre-0.14 Shiki theme names”](#removed-pre-014-shiki-theme-names)

In Astro v2.x, some Shiki theme names had been renamed, but the original names were kept for backwards compatibility.

Astro v3.0 removes the original names in favor of the renamed theme names.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-7)

If your project uses any of the themes below, rename them to their updated name:

* `material-darker` -> `material-theme-darker`
* `material-default` -> `material-theme`
* `material-lighter` -> `material-theme-lighter`
* `material-ocean` -> `material-theme-ocean`
* `material-palenight` -> `material-theme-palenight`

### Removed: `class:list` features

[Section titled “Removed: class:list features”](#removed-classlist-features)

In Astro v2.x, the [`class:list` directive](/en/reference/directives-reference/#classlist) used a custom implementation inspired by [`clsx`](https://github.com/lukeed/clsx) with a few extra features like deduplication and `Set` support.

Astro v3.0 now uses `clsx` directly for `class:list`, which does not support deduplication or `Set` values.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-8)

Replace any `Set` elements passed to the `class:list` directive with a plain `Array`.

src/components/MyAstroComponent.astro

```diff
<Component class:list={[
  'a',
  'b',
  -new Set(['c', 'd'])
+  ['c', 'd']
]} />
```

### Removed: passing `class:list` as a prop

[Section titled “Removed: passing class:list as a prop”](#removed-passing-classlist-as-a-prop)

In Astro v2.x, [`class:list` values](/en/reference/directives-reference/#classlist) were sent to components via [`Astro.props['class:list']`](/en/reference/api-reference/#props).

Astro v3.0 normalizes `class:list` values into a string before being sent to components via `Astro.props['class']`

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-9)

Remove any code that expects to receive the `class:list` prop.

src/components/MyAstroComponent.astro

```diff
---
-import { clsx } from 'clsx';
-const { class: className, 'class:list': classList } = Astro.props;
+const { class: className } = Astro.props;
---
<div
  -class:list={[className, classList]}
  +class:list={[className]}
/>
```

### Removed: kebab-case transform for camelCase CSS variables

[Section titled “Removed: kebab-case transform for camelCase CSS variables”](#removed-kebab-case-transform-for-camelcase-css-variables)

In Astro v2.x, camelCase [CSS variables](/en/guides/styling/#css-variables) passed to the `style` attribute were rendered as both camelCase (as written) and kebab-case (kept for backwards compatibility).

Astro v3.0 removes the kebab-case transform for these camelCase CSS variable names, and only the original camelCase CSS variable is rendered.

src/components/MyAstroComponent.astro

```astro
---
const myValue = "red"
---
<!-- input -->
<div style={{ "--myValue": myValue }}></div>


<!-- output (Astro 2.x) -->
<div style="--my-value:var(--myValue);--myValue:red"></div>
<!-- output (Astro 3.0) -->
<div style="--myValue:red"></div>
```

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-10)

If you were relying on Astro to transform kebab-case in your styles, update your existing styles to camelCase to prevent missing styles. For example:

src/components/MyAstroComponent.astro

```diff
<style>
  div {
   -color: var(--my-value);
   +color: var(--myValue);
  }
</style>
```

### Removed: automatic flattening of `getStaticPaths()`’s return value

[Section titled “Removed: automatic flattening of getStaticPaths()’s return value”](#removed-automatic-flattening-of-getstaticpathss-return-value)

In Astro v2.x, the return value of [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths) was automatically flattened to allow you to return an array of arrays without errors.

Astro v3.0 removes automatic flattening of `getStaticPaths()`’s result.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-11)

If you’re returning an array of arrays instead of an array of *objects* (as is expected), `.flatMap` and `.flat` should now be used to ensure that you are returning a flat array.

An [error message indicating that `getStaticPath()`’s return value must be an array of objects](/en/reference/errors/invalid-get-static-paths-entry/#what-went-wrong) will be provided if you need to update your code.

### Moved: `astro check` now requires an external package

[Section titled “Moved: astro check now requires an external package”](#moved-astro-check-now-requires-an-external-package)

In Astro v2.x, [`astro check`](/en/reference/cli-reference/#astro-check) was included in Astro by default, and its dependencies were bundled in Astro. This meant a larger package whether or not you ever used `astro check`. This also prevented you from having control over the version of TypeScript and the Astro Language Server to use.

Astro v3.0 moves the `astro check` command out of Astro core and now requires an external package `@astrojs/check`. Additionally, you must install `typescript` in your project to use the `astro check` command.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-12)

Run the `astro check` command after upgrading to Astro v3.0 and follow the prompts to install the required dependencies, or manually install `@astrojs/check` and `typescript` into your project.

### Deprecated: `build.excludeMiddleware` and `build.split`

[Section titled “Deprecated: build.excludeMiddleware and build.split”](#deprecated-buildexcludemiddleware-and-buildsplit)

In Astro v2.x, `build.excludeMiddleware` and `build.split` were used to change how specific files were emitted when using an adapter in SSR mode.

Astro v3.0 replaces these build config options with new [SSR adapter configuration options](/en/guides/integrations/#official-integrations) to perform the same tasks: `edgeMiddleware` and `functionPerRoute`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-13)

Update the Astro config file to now use the new options **in the adapter configuration** directly.

astro.config.mjs

```diff
import { defineConfig } from "astro/config";
import vercel from "@astrojs/vercel/serverless";


export default defineConfig({
-    build: {
-      excludeMiddleware: true
-    },
    adapter: vercel({
+      edgeMiddleware: true
    }),
});
```

astro.config.mjs

```diff
import { defineConfig } from "astro/config";
import netlify from "@astrojs/netlify/functions";


export default defineConfig({
-     build: {
-        split: true
-     },
     adapter: netlify({
+        functionPerRoute: true
     }),
});
```

### Deprecated: `markdown.drafts`

[Section titled “Deprecated: markdown.drafts”](#deprecated-markdowndrafts)

In Astro v2.x, the `markdown.drafts` configuration allowed you to have draft pages that were available in when running the dev server, but not built in production.

Astro v3.0 deprecates this feature in favor of the content collections method of handling draft pages by filtering manually instead, which gives more control over the feature.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-14)

To continue to mark some pages in your project as drafts, [migrate to content collections](/en/guides/content-collections/) and manually filter out pages with the `draft: true` frontmatter property instead.

### Deprecated: returning simple object in endpoints

[Section titled “Deprecated: returning simple object in endpoints”](#deprecated-returning-simple-object-in-endpoints)

In Astro v2.x, endpoints could return a simple object, which would be converted to a JSON response.

Astro v3.0 deprecates this behavior in favor of returning a `Response` object directly.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-15)

Update your endpoints to return a `Response` object directly.

endpoint.json.ts

```diff
export async function GET() {
  -return { body: { "title": "Bob's blog" }};
  +return new Response(JSON.stringify({ "title": "Bob's blog" }));
}
```

If you really need to keep the previous format, you can use the `ResponseWithEncoding` object but will be deprecated in the future.

endpoint.json.ts

```diff
export async function GET() {
  -return { body: { "title": "Bob's blog" } };
  +return new ResponseWithEncoding({ body: { "title": "Bob's blog" }});
}
```

### Changed default: `verbatimModuleSyntax` in tsconfig.json presets

[Section titled “Changed default: verbatimModuleSyntax in tsconfig.json presets”](#changed-default-verbatimmodulesyntax-in-tsconfigjson-presets)

In Astro v2.x, the [`verbatimModuleSyntax`](https://www.typescriptlang.org/tsconfig#verbatimModuleSyntax) setting was off by default, with its TypeScript 4.x equivalent `importsNotUsedAsValues` being enabled in the `strict` preset.

In Astro v3.0, `verbatimModuleSyntax` is enabled in every preset.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-16)

This option requires that types are imported using the `import type` syntax.

src/components/MyAstroComponent.astro

```astro
---
import { type CollectionEntry, getEntry } from "astro:content";
---
```

While we recommend keeping it on and properly making your type imports with `type` (as shown above), you can disable it by setting `verbatimModuleSyntax: false` in your `tsconfig.json` file if it causes any issues.

tsconfig.json

```json
{
  "compilerOptions": {
    "verbatimModuleSyntax": false
  }
}
```

### Changed default: port `3000`

[Section titled “Changed default: port 3000”](#changed-default-port-3000)

In Astro v2.x, Astro ran on port `3000` by default.

Astro v3.0 changes the [default port](/en/reference/cli-reference/#--port-number) to `4321`. 🚀

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-17)

Update any existing references to `localhost:3000`, for example in tests or in your `README`, to reflect the new port `localhost:4321`.

### Changed default: import.meta.env.BASE\_URL `trailingSlash`

[Section titled “Changed default: import.meta.env.BASE\_URL trailingSlash”](#changed-default-importmetaenvbase_url-trailingslash)

In Astro v2.x, `import.meta.env.BASE_URL` appended your [`base`](/en/reference/configuration-reference/#base) setting with a [trailingSlash](/en/reference/configuration-reference/#trailingslash) by default. `trailingSlash: "ignore"` also appended a trailing slash.

Astro v3.0 no longer appends `import.meta.env.BASE_URL` with a trailing slash by default, nor when `trailingSlash: "ignore"` is set. (The existing behavior of `base` in combination with `trailingSlash: "always"` or `trailingSlash: "never"` is unchanged.)

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-18)

If your `base` already has a trailing slash, no change is needed.

If your `base` does not have a trailing slash, add one if you wish to preserve the previous default (or `trailingSlash: "ignore"`) behavior:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
-  base: 'my-base',
+  base: 'my-base/',
});
```

### Changed default: `compressHTML`

[Section titled “Changed default: compressHTML”](#changed-default-compresshtml)

In Astro v2.x, Astro only compressed your emitted HTML when [`compressHTML`](/en/reference/configuration-reference/#compresshtml) was explicitly set to `true`. The default value was `false`.

Astro v3.0 now compresses emitted HTML by default.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-19)

You can now remove `compressHTML: true` from your configuration as this is the new default behavior.

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
-  compressHTML: true
})
```

You must now set `compressHTML: false` to opt out of HTML compression.

### Changed default: `scopedStyleStrategy`

[Section titled “Changed default: scopedStyleStrategy”](#changed-default-scopedstylestrategy)

In Astro v2.x, the default value of [`scopedStyleStrategy`](/en/reference/configuration-reference/#scopedstylestrategy) was `"where"`.

Astro v3.0 introduces a new, default value: `"attribute"`. By default, styles are now applied using `data-*` attributes.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-20)

To retain your project’s current [style scoping](/en/guides/styling/#scoped-styles), update the configuration file to the previous default value:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
+  scopedStyleStrategy: "where"
})
```

### Changed default: `inlineStyleSheets`

[Section titled “Changed default: inlineStyleSheets”](#changed-default-inlinestylesheets)

In Astro v2.x, all project stylesheets were sent as link tags by default. You could opt in to inlining them into `<style>` tags every time with `"always"`, or to inlining only stylesheets below a certain size with `"auto"` by setting the [`build.inlineStylesheets`](/en/reference/configuration-reference/#buildinlinestylesheets) configuration. The default setting was `"never"`.

Astro v3.0 changes the default value of `inlineStylesheets` to `"auto"`. Stylesheets smaller than `ViteConfig.build.assetsInlineLimit` (default: 4kb) are inlined by default. Otherwise, project styles are sent in external stylesheets.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-21)

If you want to keep your project’s current behavior, set `build.inlineStylesheets` to the previous default, `"never"`:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
+   build: {
+    inlineStylesheets: "never"
+  }
})
```

### Changed default: image service

[Section titled “Changed default: image service”](#changed-default-image-service)

In Astro v2.x, Squoosh was the [default image processing service](/en/guides/images/#default-image-service).

Astro v3.0 now includes Sharp as the default image processing service and instead provides a configuration option to use Squoosh.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-22)

Note

When using a [strict package manager](https://pnpm.io/pnpm-vs-npm#npms-flat-tree) like `pnpm`, you may need to manually install Sharp into your project even though it is an Astro dependency:

```bash
pnpm add sharp
```

If you would prefer to continue to use Squoosh to transform your images, update your config with the following:

astro.config.mjs

```diff
import { defineConfig, squooshImageService } from "astro/config";


export default defineConfig({
+  image: {
+    service: squooshImageService(),
+  }
})
```

### Changed: HTTP request methods case

[Section titled “Changed: HTTP request methods case”](#changed-http-request-methods-case)

In Astro v2.x, [HTTP request methods](/en/guides/endpoints/#http-methods) were written using lowercase function names: `get`, `post`, `put`, `all`, and `del`.

Astro v3.0 uses uppercase function names, including `DELETE` instead of `del`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-23)

Rename all functions to their uppercase equivalent:

* `get` to `GET`
* `post` to `POST`
* `put` to `PUT`
* `all` to `ALL`
* `del` to `DELETE`

endpoint.ts

```diff
-export function get() {
+export function GET() {
    return new Response(JSON.stringify({ "title": "Bob's blog" }));
}
```

### Changed: Multiple JSX framework configuration

[Section titled “Changed: Multiple JSX framework configuration”](#changed-multiple-jsx-framework-configuration)

In Astro v2.x, you could use [multiple JSX framework integrations](/en/guides/integrations/#official-integrations) (React, Solid, Preact) in the same project without needing to identify which files belonged to which framework.

Astro v3.0 now requires you to specify which framework to use for your files with new `include` and `exclude` integration config options when you have multiple JSX framework integrations installed. This allows Astro to better support single-framework usage, as well as advanced features like React Fast Refresh.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-24)

If you are using multiple JSX frameworks in the same project, set `include` (and optionally `exclude`) to an array of files and/or folders. Wildcards may be used to include multiple file paths.

We recommend placing common framework components in the same folder (e.g. `/components/react/` and `/components/solid/`) to make specifying your includes easier, but this is not required:

```diff
import { defineConfig } from 'astro/config';
import preact from '@astrojs/preact';
import react from '@astrojs/react';
import svelte from '@astrojs/svelte';
import vue from '@astrojs/vue';
import solid from '@astrojs/solid-js';


export default defineConfig({
  // Enable many frameworks to support all different kinds of components.
  // No `include` is needed if you are only using a single framework!
  integrations: [
    preact({
+      include: ['**/preact/*']
    }),
    react({
+      include: ['**/react/*']
    }),
    solid({
+      include: ['**/solid/*'],
    }),
  ]
});
```

### Changed: `Astro.cookies.get(key)` can return `undefined`

[Section titled “Changed: Astro.cookies.get(key) can return undefined”](#changed-astrocookiesgetkey-can-return-undefined)

In Astro v2.x, `Astro.cookies.get(key)` would always return an `AstroCookie` object, even if the cookie did not exist. To check for its existence, you needed to use `Astro.cookies.has(key)`.

Astro v3.0 returns `undefined` for `Astro.cookies.get(key)` if the cookie does not exist.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-25)

This change will not break any code that checks for the existence of the `Astro.cookie` object before using `Astro.cookies.get(key)`, but is now no longer required.

You can safely remove any code that uses `has()` to check if the value of `Astro.cookies` is `undefined`:

```diff
-if (Astro.cookies.has(id)) {
  -const id = Astro.cookies.get(id)!;
-}


+const id = Astro.cookies.get(id);
+if (id) {
+}
```

### Changed: running the Astro CLI programmatically

[Section titled “Changed: running the Astro CLI programmatically”](#changed-running-the-astro-cli-programmatically)

In Astro v2.x, the `"astro"` package entrypoint exported and ran the Astro CLI directly. It is not recommended to run Astro this way in practice.

Astro v3.0 removes the CLI from the entrypoint, and exports a new set of experimental JavaScript APIs, including `dev()`, `build()`, `preview()`, and `sync()`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-26)

To [run the Astro CLI programmatically](/en/reference/programmatic-reference/), use the new experimental JavaScript APIs:

```js
import { dev, build } from "astro";


// Start the Astro dev server
const devServer = await dev();
await devServer.stop();


// Build your Astro project
await build();
```

### Changed: internal Astro API entry point export paths

[Section titled “Changed: internal Astro API entry point export paths”](#changed-internal-astro-api-entry-point-export-paths)

In Astro v2.x, you could import internal Astro APIs from `astro/internal/*` and `astro/runtime/server/*`.

Astro v3.0 removes the two entry points in favor of the existing `astro/runtime/*` entrypoint. Additionally, a new `astro/compiler-runtime` export has been added for compiler-specific runtime code.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-27)

These are entry points for Astro’s internal API and should not affect your project. But if you do use these entrypoints, update as shown below:

```diff
-import 'astro/internal/index.js';
+import 'astro/runtime/server/index.js';


-import 'astro/server/index.js';
+import 'astro/runtime/server/index.js';
```

```diff
import { transform } from '@astrojs/compiler';


const result = await transform(source, {
  internalURL: 'astro/runtime/server/index.js',
  internalURL: 'astro/compiler-runtime',
  // ...
});
```

## Feature Upgrades

[Section titled “Feature Upgrades”](#feature-upgrades)

### Upgrade images to v3

[Section titled “Upgrade images to v3”](#upgrade-images-to-v3)

`astro:assets` is no longer behind an experimental flag in Astro v3.0.

`<Image />` is now a built-in component and the previous `@astrojs/image` integration has been removed.

These and other accompanying changes to using images in Astro may cause some breaking changes when you upgrade your Astro project from an earlier version.

Please follow the instructions below as appropriate to upgrade an Astro v2.x project to v3.0.

#### Upgrade from `experimental.assets`

[Section titled “Upgrade from experimental.assets”](#upgrade-from-experimentalassets)

If you had previously enabled the experimental flag for `astro:assets`, you will need to update your project for Astro v3.0 which now includes assets features by default.

##### Remove `experimental.assets` flag

[Section titled “Remove experimental.assets flag”](#remove-experimentalassets-flag)

Remove the experimental flag:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
-  experimental: {
-    assets: true
-  }
});
```

If necessary, also update your `src/env.d.ts` file to replace the `astro/client-image` reference with `astro/client`:

src/env.d.ts

```diff
-/// <reference types="astro/client-image" />
+/// <reference types="astro/client" />
```

##### Remove the `~/assets` import alias

[Section titled “Remove the \~/assets import alias”](#remove-the-assets-import-alias)

This import alias is no longer included by default with `astro:assets`. If you were using this alias with experimental assets, you must convert them to relative file paths, or [create your own import aliases](/en/guides/imports/#aliases).

src/pages/posts/post-1.astro

```diff
---
-import rocket from '~/assets/rocket.png';
+import rocket from '../../assets/rocket.png';
---
```

##### Add simple asset support for Cloudflare, Deno, Vercel Edge and Netlify Edge

[Section titled “Add simple asset support for Cloudflare, Deno, Vercel Edge and Netlify Edge”](#add-simple-asset-support-for-cloudflare-deno-vercel-edge-and-netlify-edge)

Astro v3.0 allows `astro:assets` to work without errors in Cloudflare, Deno, Vercel Edge and Netlify Edge, which do not support Astro’s built-in Squoosh and Sharp image optimization. Note that Astro does not perform any image transformation and processing in these environments. However, you can still enjoy the other benefits of using `astro:assets`, including no Cumulative Layout Shift (CLS), the enforced `alt` attribute, and a consistent authoring experience.

If you previously avoided using `astro:assets` because of these constraints, you can now use them without issues. You can configure the no-op image service to explicitly opt-in to this behavior:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
+  image: {
+    service: {
+      entrypoint: 'astro/assets/services/noop'
+    }
+  }
});
```

#### Decide where to store your images

[Section titled “Decide where to store your images”](#decide-where-to-store-your-images)

See the Images guide to help you decide [where to store your images](/en/guides/images/#where-to-store-images). You may wish to take advantage of new options for storing your images with the added flexibility `astro:assets` brings. For example, relative images from your project `src/` can now be referenced in Markdown, MDX, and Markdoc using standard Markdown `![alt](src)` syntax.

#### Update existing `<img>` tags

[Section titled “Update existing \<img> tags”](#update-existing-img-tags)

Previously, importing an image would return a simple `string` with the path of the image. Now, imported image assets match the following signature:

```ts
interface ImageMetadata {
  src: string;
  width: number;
  height: number;
  format: string;
}
```

You must update the `src` attribute of any existing `<img>` tags (including any [images in UI framework components](/en/guides/images/#images-in-ui-framework-components)) and you may also update other attributes that are now available to you from the imported image.

src/components/MyComponent.astro

```diff
---
import rocket from '../images/rocket.svg';
---
<img src={rocket} width="250" height="250" alt="A rocketship in space." />


<img src={rocket.src} width={rocket.width} height={rocket.height} alt="A rocketship in space." />
```

#### Update your Markdown, MDX, and Markdoc files

[Section titled “Update your Markdown, MDX, and Markdoc files”](#update-your-markdown-mdx-and-markdoc-files)

Relative images from your project `src/` can now be referenced in Markdown, MDX, and Markdoc using standard Markdown `![alt](src)` syntax.

This allows you to move your images from the `public/` directory to your project `src/` where they will now be processed and optimized. Your existing images in `public/` and remote images are still valid but are not optimized by Astro’s build process.

src/pages/posts/post-1.md

```md
# My Markdown Page


<!-- Local images now possible! -->
![A starry night sky.](../../images/stars.png)


<!-- Keep your images next to your content! -->
![A starry night sky.](./stars.png)
```

If you require more control over your image attributes, we recommend using the `.mdx` file format, which allows you to include Astro’s `<Image />` component or a JSX `<img />` tag in addition to the Markdown syntax. Use the [MDX integration](/en/guides/integrations-guide/mdx/) to add support for MDX to Astro.

#### Remove `@astrojs/image`

[Section titled “Remove @astrojs/image”](#remove-astrojsimage)

If you were using the image integration in Astro v2.x, complete the following steps:

1. Remove the `@astrojs/image` integration.

   You must [remove the integration](/en/guides/integrations/#removing-an-integration) by uninstalling and then removing it from your `astro.config.mjs` file.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   -import image from '@astrojs/image';


   export default defineConfig({
     integrations: [
       -image(),
     ]
   })
   ```

2. Update types (if required).

   If you had special types configured for `@astrojs/image` in `src/env.d.ts`, you may need to change them back to the default Astro types if your upgrade to v3 did not complete this step for you.

   src/env.d.ts

   ```diff
    -/// <reference types="@astrojs/image/client" />
    +/// <reference types="astro/client" />
   ```

   Similarly, update `tsconfig.json` if necessary:

   tsconfig.json

   ```diff
   {
     "compilerOptions": {
       -"types": ["@astrojs/image/client"]
       +"types": ["astro/client"]
     }
   }
   ```

3. Migrate any existing `<Image />` components.

   Change all `import` statements from `@astrojs/image/components` to `astro:assets` in order to use the new built-in `<Image />` component.

   Remove any component attributes that are not [currently supported image asset properties](/en/reference/modules/astro-assets/#image-).

   For example, `aspectRatio` is no longer supported, as it is now automatically inferred from the `width` and `height` attributes.

   src/components/MyComponent.astro

   ```diff
   ---
   -import { Image } from '@astrojs/image/components';
   +import { Image } from 'astro:assets';
   import localImage from '../assets/logo.png';
   const localAlt = 'The Astro Logo';
   ---


   <Image
     src={localImage}
     width={300}
     -aspectRatio="16:9"
     alt={localAlt}
   />
   ```

4. Choose a default image service.

   [Sharp](https://github.com/lovell/sharp) is now the default image service used for `astro:assets`. If you would like to use Sharp, no configuration is required.

   If you would prefer to use [Squoosh](https://github.com/GoogleChromeLabs/squoosh) to transform your images, update your config with the following `image.service` option:

   astro.config.mjs

   ```diff
   import { defineConfig, squooshImageService } from 'astro/config';


   export default defineConfig({
   +  image: {
   +    service: squooshImageService(),
   +  },
   });
   ```

#### Update Content Collections schemas

[Section titled “Update Content Collections schemas”](#update-content-collections-schemas)

You can now declare an associated image for a content collections entry, such as a blog post’s cover image, in your frontmatter using its path relative to the current folder.

The new `image` helper for content collections lets you validate the image metadata using Zod. Learn more about [how to use images in content collections](/en/guides/images/#images-in-content-collections)

#### Navigating Image Imports in Astro v3.0

[Section titled “Navigating Image Imports in Astro v3.0”](#navigating-image-imports-in-astro-v30)

In Astro v3.0, if you have to preserve the old import behavior for images and require a string representation of the image’s URL, append `?url` to the end of your image path when importing it. For example:

src/pages/blog/MyImages.astro

```astro
---
import Sprite from '../assets/logo.svg?url';
---


<svg>
  <use xlink:href={Sprite + '#cart'} />
</svg>
```

This approach ensures you obtain the URL string. Keep in mind that during development, Astro uses a `src/` path, but upon building, it generates hashed paths like `/_astro/cat.a6737dd3.png`.

If you prefer to work directly with the image object itself, you can access the `.src` property. This approach is best for tasks like managing image dimensions for Core Web Vitals metrics and preventing CLS.

If you are transitioning into the new import behavior, combining `?url` and `.src` methods might be the right method for seamless image handling.

### Upgrade view transitions to v3

[Section titled “Upgrade view transitions to v3”](#upgrade-view-transitions-to-v3)

View transitions are no longer behind an experimental flag in Astro v3.0.

If you had **not** enabled this experimental flag in Astro 2.x, this will not cause any breaking changes to your project. The new View Transitions API has no effect on your existing code.

If you were previously using experimental view transitions, there may be some breaking changes when you upgrade your Astro project from an earlier version.

Please follow the instructions below as appropriate to upgrade **an Astro v2.x project configured with `experimental.viewTransitions: true`** to v3.0.

#### Upgrade from `experimental.viewTransitions`

[Section titled “Upgrade from experimental.viewTransitions”](#upgrade-from-experimentalviewtransitions)

If you had previously enabled the experimental flag for view transitions, you will need to update your project for Astro v3.0 which now allows view transitions by default.

##### Remove `experimental.viewTransitions` flag

[Section titled “Remove experimental.viewTransitions flag”](#remove-experimentalviewtransitions-flag)

Remove the experimental flag:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
-  experimental: {
-   viewTransitions: true
-  }
});
```

##### Update import source

[Section titled “Update import source”](#update-import-source)

The `<ViewTransitions />` component has been moved from `astro:components` to `astro:transitions`. Update the import source across all occurrences in your project.

src/layouts/BaseLayout.astro

```astro
---
import { ViewTransitions } from "astro:components astro:transitions"
---
<html lang="en">
  <head>
    <title>My Homepage</title>
    <ViewTransitions />
  </head>
  <body>
    <h1>Welcome to my website!</h1>
  </body>
</html>
```

#### Update `transition:animate` directives

[Section titled “Update transition:animate directives”](#update-transitionanimate-directives)

**Changed:** The `transition:animate` value `morph` has been renamed to `initial`. Also, this is no longer the default animation. If no `transition:animate` directive is specified, your animations will now default to `fade`.

1. Rename any `morph` animations to `initial`.

   src/components/MyComponent.astro

   ```astro
   <div transition:name="name" transition:animate="morph initial" />
   ```

2. To keep any animations that were previously using `morph` by default, explicitly add `transition:animate="initial"`

   src/components/MyComponent.astro

   ```astro
   <div transition:name="name" transition:animate="initial" />
   ```

3. You can safely remove any animations explicitly set to `fade`. This is now the default behavior:

   src/components/MyComponent.astro

   ```astro
   <div transition:name="name" transition:animate="fade" />
   ```

**Added:** Astro also supports a new `transition:animate` value, `none`. This value can be used on a page’s `<html>` element to disable animated full-page transitions on an entire page. This will only override **default animation behavior** on page elements without an animation directive. You can still set animations on individual elements, and these specific animations will occur.

4. You may now disable all default transitions on an individual page, animating only elements that explicitly use a `transition:animate` directive:

   ```astro
   <html transition:animate="none">
     <head></head>
     <body>
       <h1>Hello world!</h1>
     </body>
   </html>
   ```

##### Update event names

[Section titled “Update event names”](#update-event-names)

The event `astro:load` has been renamed to `astro:page-load`. Rename all occurrences in your project.

src/components/MyComponent.astro

```astro
<script>
document.addEventListener('astro:load astro:page-load', runSetupLogic);
</script>
```

The event `astro:beforeload` has been renamed to `astro:after-swap`. Rename all occurrences in your project.

src/components/MyComponent.astro

```astro
<script>
document.addEventListener('astro:beforeload astro:after-swap', setDarkMode);
</script>
```

## Community Resources

[Section titled “Community Resources”](#community-resources)

Know a good resource for Astro v3.0? [Edit this page](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/upgrade-to/v3.mdx) and add a link below!

## Known Issues

[Section titled “Known Issues”](#known-issues)

There are currently no known issues.

# Upgrade to Astro v4

> How to upgrade your project to the latest version of Astro (v4.0).

This guide will help you migrate from Astro v3 to Astro v4.

Need to upgrade an older project to v3? See our [older migration guide](/en/guides/upgrade-to/v3/).

Need to see the v3 docs? Visit this [older version of the docs site (unmaintained v3.6 snapshot)](https://web.archive.org/web/20231203051122/https://docs.astro.build/en/getting-started/).

## Upgrade Astro

[Section titled “Upgrade Astro”](#upgrade-astro)

Update your project’s version of Astro and all official integrations to the latest versions using your package manager.

* npm

  ```shell
  # Upgrade Astro and official integrations together
  npx @astrojs/upgrade
  ```

* pnpm

  ```shell
  # Upgrade Astro and official integrations together
  pnpm dlx @astrojs/upgrade
  ```

* Yarn

  ```shell
  # Upgrade Astro and official integrations together
  yarn dlx @astrojs/upgrade
  ```

You can also [upgrade your Astro integrations manually](/en/guides/integrations/#manual-upgrading) if needed, and you may also need to upgrade other dependencies in your project.

Need to continue?

After upgrading Astro to the latest version, you may not need to make any changes to your project at all!

But, if you notice errors or unexpected behavior, please check below for what has changed that might need updating in your project.

Astro v4.0 includes [potentially breaking changes](#breaking-changes), as well as the [removal of some previously deprecated features](#previously-deprecated-features-now-removed).

If your project doesn’t work as expected after upgrading to v4.0, check this guide for an overview of all breaking changes and instructions on how to update your codebase.

See [the changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for full release notes.

## Astro v4.0 Experimental Flags Removed

[Section titled “Astro v4.0 Experimental Flags Removed”](#astro-v40-experimental-flags-removed)

Remove the `devOverlay` experimental flag and move any `i18n` config to the top level in `astro.config.mjs`:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  experimental: {
-    devOverlay: true,
-    i18n: {
-      locales: ["en", "fr", "pt-br", "es"],
-      defaultLocale: "en",
-    }
  },
+  i18n: {
+    locales: ["en", "fr", "pt-br", "es"],
+    defaultLocale: "en",
+  },
})
```

These configurations, `i18n` and the renamed `devToolbar`, are now available in Astro v4.0.

Read more about these two exciting features and more in [the v4.0 Blog post](https://astro.build/blog/astro-4/)!

## Upgrades

[Section titled “Upgrades”](#upgrades)

Any major upgrades to Astro’s dependencies may cause breaking changes in your project.

### Upgraded: Vite 5.0

[Section titled “Upgraded: Vite 5.0”](#upgraded-vite-50)

In Astro v3.0, Vite 4 was used as the development server and production bundler.

Astro v4.0 upgrades from Vite 4 to Vite 5.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do)

If you are using Vite-specific plugins, configuration, or APIs, check the [Vite migration guide](https://vite.dev/guide/migration) for their breaking changes and upgrade your project as needed. There are no breaking changes to Astro itself.

### Upgraded: unified, remark, and rehype dependencies

[Section titled “Upgraded: unified, remark, and rehype dependencies”](#upgraded-unified-remark-and-rehype-dependencies)

In Astro v3.x, unified v10 and its related compatible remark/rehype packages were used to process Markdown and MDX.

Astro v4.0 upgrades [unified to v11](https://github.com/unifiedjs/unified/releases/tag/11.0.0) and the other remark/rehype packages to the latest version.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-1)

If you used custom remark/rehype packages, update all of them to the latest version using your package manager to ensure they support unified v11. The packages you are using can be found in `astro.config.mjs`.

There should not be any significant breaking changes if you use actively updated packages, but some packages may not yet be compatible with unified v11. Visually inspect your Markdown/MDX pages before deploying to ensure your site is functioning as intended.

## Breaking Changes

[Section titled “Breaking Changes”](#breaking-changes)

The following changes are considered breaking changes in Astro. Breaking changes may or may not provide temporary backwards compatibility, and all documentation is updated to refer to only the current, supported code.

If you need to refer to the documentation for a v3.x project, you can browse this [(unmaintained) snapshot of the docs from before v4.0 was released](https://web.archive.org/web/20231203051122/https://docs.astro.build/en/getting-started/).

### Renamed: `entrypoint` (Integrations API)

[Section titled “Renamed: entrypoint (Integrations API)”](#renamed-entrypoint-integrations-api)

In Astro v3.x, the property of the `injectRoute` integrations API that specified the route entry point was named `entryPoint`.

Astro v4.0 renames this property to `entrypoint` to be consistent with other Astro APIs. The `entryPoint` property is deprecated but will continue to work and logs a warning prompting you to update your code.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-2)

If you have integrations that use the `injectRoute` API, rename the `entryPoint` property to `entrypoint`. If you’re a library author who wants to support both Astro 3 and 4, you can specify both `entryPoint` and `entrypoint`, in which case, a warning will not be logged.

```diff
injectRoute({
  pattern: '/fancy-dashboard',
-  entryPoint: '@fancy/dashboard/dashboard.astro'
+  entrypoint: '@fancy/dashboard/dashboard.astro'
});
```

### Changed: `app.render` signature in Integrations API

[Section titled “Changed: app.render signature in Integrations API”](#changed-apprender-signature-in-integrations-api)

In Astro v3.0, the `app.render()` method accepted `routeData` and `locals` as separate, optional arguments.

Astro v4.0 changes the `app.render()` signature. These two properties are now available in a single object. Both the object and these two properties are still optional.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-3)

If you are maintaining an adapter, the current signature will continue to work until the next major version. To migrate to the new signature, pass `routeData` and `locals` as properties of an object instead of as multiple independent arguments.

```diff
-app.render(request, routeData, locals)
+app.render(request, { routeData, locals })
```

### Changed: adapters must now specify supported features

[Section titled “Changed: adapters must now specify supported features”](#changed-adapters-must-now-specify-supported-features)

In Astro v3.x, adapters were not required to specify the features they support.

Astro v4.0 requires adapters to pass the `supportedAstroFeatures{}` property to specify a list of features they support. This property is no longer optional.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-4)

Adapter authors need to pass the `supportedAstroFeatures{}` option to specify a list of features they support.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@matthewp/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@matthewp/my-adapter',
          serverEntrypoint: '@matthewp/my-adapter/server.js',
+          supportedAstroFeatures: {
+              staticOutput: 'stable'
+          }
        });
      },
    },
  };
}
```

### Removed: Shiki language `path` property

[Section titled “Removed: Shiki language path property”](#removed-shiki-language-path-property)

In Astro v3.x, a Shiki language passed to `markdown.shikiConfig.langs` was automatically converted to a Shikiji-compatible language. Shikiji is the internal tooling used by Astro for syntax highlighting.

Astro v4.0 removes support for the `path` property of a Shiki language, which was confusing to configure. It is replaced by an import which can be passed to `langs` directly.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-5)

The language JSON file should be imported and passed to the option instead.

astro.config.js

```diff
 +import customLang from './custom.tmLanguage.json'


export default defineConfig({
  markdown: {
    shikiConfig: {
      langs: [
-       { path: '../../custom.tmLanguage.json' },
+       customLang,
      ],
    },
  },
})
```

## Deprecated

[Section titled “Deprecated”](#deprecated)

The following deprecated features are no longer supported and are no longer documented. Please update your project accordingly.

Some deprecated features may temporarily continue to function until they are completely removed. Others may silently have no effect, or throw an error prompting you to update your code.

### Deprecated: `handleForms` for View Transitions `submit` events

[Section titled “Deprecated: handleForms for View Transitions submit events”](#deprecated-handleforms-for-view-transitions-submit-events)

In Astro v3.x, projects using the `<ViewTransitions />` component were required to opt-in to handling `submit` events for `form` elements. This was done by passing a `handleForms` prop.

Astro v4.0 handles `submit` events for `form` elements by default when `<ViewTransitions />` are used. The `handleForms` prop has been deprecated and no longer has any effect.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-6)

Remove the `handleForms` property from your `ViewTransitions` component. It is no longer necessary.

src/pages/index.astro

```astro
---
import { ViewTransitions } from "astro:transitions";
---
<html>
  <head>
    <ViewTransitions handleForms />
  </head>
  <body>
    <!-- stuff here -->
  </body>
</html>
```

To opt out of `submit` event handling, add the `data-astro-reload` attribute to relevant `form` elements.

src/components/Form.astro

```astro
<form action="/contact" data-astro-reload>
  <!-- -->
</form>
```

## Previously deprecated features now removed

[Section titled “Previously deprecated features now removed”](#previously-deprecated-features-now-removed)

The following deprecated features have now been entirely removed from the code base and can no longer be used. Some of these features may have continued to work in your project even after deprecation. Others may have silently had no effect.

Projects now containing these removed features will be unable to build, and there will no longer be any supporting documentation prompting you to remove these features.

### Removed: returning simple objects from endpoints

[Section titled “Removed: returning simple objects from endpoints”](#removed-returning-simple-objects-from-endpoints)

In Astro v3.x, returning simple objects from endpoints was deprecated, but was still supported to maintain compatibility with Astro v2. A `ResponseWithEncoding` utility was also provided to ease the migration.

Astro v4.0 removes support for simple objects and requires endpoints to always return a `Response`. The `ResponseWithEncoding` utility is also removed in favor of a proper `Response` type.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-7)

Update your endpoints to return a `Response` object directly.

```diff
export async function GET() {
  -return { body: { "title": "Bob's blog" }};
  +return new Response(JSON.stringify({ "title": "Bob's blog" }));
}
```

To remove usage of `ResponseWithEncoding`, refactor your code to use an `ArrayBuffer` instead:

```diff
export async function GET() {
  const file = await fs.readFile('./bob.png');
  -return new ResponseWithEncoding(file.toString('binary'), undefined, 'binary');
  +return new Response(file.buffer);
}
```

### Removed: `build.split` and `build.excludeMiddleware`

[Section titled “Removed: build.split and build.excludeMiddleware”](#removed-buildsplit-and-buildexcludemiddleware)

In Astro v3.0, `build.split` and `build.excludeMiddleware` build config options were deprecated and replaced with [adapter configuration options](/en/reference/adapter-reference/#adapter-features) to perform the same tasks.

Astro v4.0 removes these properties entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-8)

If you are using the deprecated `build.split` or `build.excludeMiddleware`, you must now remove them as these no longer exist.

Please see the v3 migration guide to [update these deprecated middleware properties](/en/guides/upgrade-to/v3/#deprecated-buildexcludemiddleware-and-buildsplit) with adapter configurations.

### Removed: `Astro.request.params`

[Section titled “Removed: Astro.request.params”](#removed-astrorequestparams)

In Astro v3.0, the `Astro.request.params` API was deprecated, but preserved for backwards compatibility.

Astro v4.0 removes this option entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-9)

Update all occurrences to [`Astro.params`](/en/reference/api-reference/#params), which is the supported replacement.

```diff
-const { id } = Astro.request.params;
+const { id } = Astro.params;
```

### Removed: `markdown.drafts`

[Section titled “Removed: markdown.drafts”](#removed-markdowndrafts)

In Astro v3.0, using `markdown.drafts` to control the building of draft posts was deprecated.

Astro v4.0 removes this option entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-10)

If you are using the deprecated `markdown.drafts`, you must now remove it as it no longer exists.

To continue to mark some pages in your project as drafts, [migrate to content collections](/en/guides/content-collections/) and manually filter out pages with the `draft: true` frontmatter property instead.

### Removed: `getHeaders()`

[Section titled “Removed: getHeaders()”](#removed-getheaders)

In Astro v3.0, the `getHeaders()` Markdown export was deprecated and replaced with `getHeadings()`.

Astro v4.0 removes this option entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-11)

If you are using the deprecated `getHeaders()`, you must now remove it as it no longer exists. Replace any instances with `getHeadings()`, which is the supported replacement.

```diff
const posts = await Astro.glob('../content/blog/*.mdx');
-const firstPostHeadings = posts.at(0).getHeaders();
+const firstPostHeadings = posts.at(0).getHeadings();
```

### Removed: using `rss` in `getStaticPaths()`

[Section titled “Removed: using rss in getStaticPaths()”](#removed-using-rss-in-getstaticpaths)

In Astro v3.0, using the deprecated `rss` helper in `getStaticPaths()` would throw an error.

Astro v4.0 removes this helper entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-12)

If you are using the unsupported method for generating RSS feeds, you must now use the [`@astrojs/rss` integration](/en/recipes/rss/) for a complete RSS setup.

### Removed: lowercase HTTP method names

[Section titled “Removed: lowercase HTTP method names”](#removed-lowercase-http-method-names)

In Astro v3.0, using lowercase HTTP request method names (`get`, `post`, `put`, `all`, `del`) was deprecated.

Astro v4.0 removes support for lowercase names entirely. All HTTP request methods must now be written using uppercase.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-13)

If you are using the deprecated lowercase names, you must now replace them with their uppercase equivalents.

Please see the v3 migration guide [for guidance using uppercase HTTP request methods](/en/guides/upgrade-to/v3/#changed-http-request-methods-case).

### Removed: 301 redirects when missing a `base` prefix

[Section titled “Removed: 301 redirects when missing a base prefix”](#removed-301-redirects-when-missing-a-base-prefix)

In Astro v3.x, the Astro preview server returned a 301 redirect when accessing public directory assets without a base path.

Astro v4.0 returns a 404 status without a base path prefix for public directory assets when the preview server is running, matching the behavior of the dev server.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-14)

When using the Astro preview server, all of your static asset imports and URLs from the public directory must have [the base value](/en/reference/configuration-reference/#base) prefixed to the path.

The following example shows the `src` attribute required to display an image from the public folder when `base: '/docs'` is configured:

src/pages/index.astro

```astro
// To access public/images/my-image.png:


<img src="/docs/images/my-image.png" alt="">
```

### Removed: `astro/client-image` auto-conversion

[Section titled “Removed: astro/client-image auto-conversion”](#removed-astroclient-image-auto-conversion)

In Astro v3.x, the `astro/client-image` type (used for the deprecated image integration) was removed but was auto-converted to the default Astro type `astro/client` if found in your `env.d.ts` file.

Astro v4.0 ignores `astro/client-image` and will no longer update `env.d.ts` for you automatically.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-15)

If you had types configured for `@astrojs/image` in `src/env.d.ts` and upgrading to v3.0 did not automatically convert the type for you, replace the `astro/client-image` type manually with `astro/client`.

src/env.d.ts

```diff
  -/// <reference types="astro/client-image" />
  +/// <reference types="astro/client" />
```

## Community Resources

[Section titled “Community Resources”](#community-resources)

Know a good resource for Astro v4.0? [Edit this page](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/upgrade-to/v4.mdx) and add a link below!

## Known Issues

[Section titled “Known Issues”](#known-issues)

Please check [Astro’s issues on GitHub](https://github.com/withastro/astro/issues/) for any reported issues, or to file an issue yourself.

# Upgrade to Astro v5

> How to upgrade your project to Astro v5.0.

This guide will help you migrate from Astro v4 to Astro v5.

Need to upgrade an older project to v4 first? See our [older migration guide](/en/guides/upgrade-to/v4/).

Need to see the v4 docs? Visit this [older version of the docs site (unmaintained v4.16 snapshot)](https://v4.docs.astro.build/).

## Upgrade Astro

[Section titled “Upgrade Astro”](#upgrade-astro)

Update your project’s version of Astro to the latest version using your package manager:

* npm

  ```shell
  # Upgrade Astro and official integrations together
  npx @astrojs/upgrade
  ```

* pnpm

  ```shell
  # Upgrade Astro and official integrations together
  pnpm dlx @astrojs/upgrade
  ```

* Yarn

  ```shell
  # Upgrade Astro and official integrations together
  yarn dlx @astrojs/upgrade
  ```

You can also [upgrade your Astro integrations manually](/en/guides/integrations/#manual-upgrading) if needed, and you may also need to upgrade other dependencies in your project.

Need to continue?

After upgrading Astro, you may not need to make any changes to your project at all!

But, if you notice errors or unexpected behavior, please check below for what has changed that might need updating in your project.

Astro v5.0 includes [potentially breaking changes](#breaking-changes), as well as the removal and deprecation of some features.

If your project doesn’t work as expected after upgrading to v5.0, check this guide for an overview of all breaking changes and instructions on how to update your codebase.

See [the Astro changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for full release notes.

## Dependency Upgrades

[Section titled “Dependency Upgrades”](#dependency-upgrades)

Any major upgrades to Astro’s dependencies may cause breaking changes in your project.

### Vite 6.0

[Section titled “Vite 6.0”](#vite-60)

Astro v5.0 upgrades to Vite v6.0 as the development server and production bundler.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do)

If you are using Vite-specific plugins, configuration, or APIs, check the [Vite migration guide](https://vite.dev/guide/migration.html) for their breaking changes and upgrade your project as needed.

### `@astrojs/mdx`

[Section titled “@astrojs/mdx”](#astrojsmdx)

[Implementation PR: Cleanup unused JSX code (#11741)](https://github.com/withastro/astro/pull/11741)

In Astro v4.x, Astro performed internal JSX handling for the `@astrojs/mdx` integration.

Astro v5.0 moves this responsibility to handle and render JSX and MDX to the `@astrojs/mdx` package directly. This means that Astro 5.0 is no longer compatible with older versions of the MDX integration.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-1)

If your project includes `.mdx` files, you must upgrade `@astrojs/mdx` to the latest version (v4.0.0) so that your JSX can be handled properly by the integration.

If you are using an MDX server renderer with the experimental [Astro Container API](/en/reference/container-reference/) you must update the import to reflect the new location:

```diff
-import mdxRenderer from "astro/jsx/server.js";
+import mdxRenderer from "@astrojs/mdx/server.js";
```

Learn more about [using MDX in your project](/en/guides/integrations-guide/mdx/).

## Legacy

[Section titled “Legacy”](#legacy)

The following features are now considered legacy features. They should function normally but are no longer recommended and are in maintenance mode. They will see no future improvements and documentation will not be updated. These features will eventually be deprecated, and then removed entirely.

### Legacy: v2.0 Content Collections API

[Section titled “Legacy: v2.0 Content Collections API”](#legacy-v20-content-collections-api)

In Astro 4.x, content collections were defined, queried, and rendered using [the Content Collections API first introduced in Astro v2.0](https://astro.build/blog/introducing-content-collections/). All collection entries were local files within the reserved `src/content/` folder. Additionally, Astro’s [file name convention to exclude building individual pages](/en/guides/routing/#excluding-pages) was built in to the Content Collections API.

Astro 5.0 introduces a new version of content collections using the Content Layer API which brings several performance improvements and added capabilities. While old (legacy) and new (Content Layer API) collections can continue exist together in this release, there are potentially breaking changes to existing legacy collections.

This release also removes the option to prefix collection entry file names with an underscore (`_`) to prevent building a route.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-2)

We recommend [converting any existing collections to the new Content Layer API](#updating-existing-collections) as soon as you are able and making any new collections using the Content Layer API.

If you are unable to convert your collections, then please consult the [legacy collections breaking changes](#breaking-changes-to-legacy-content-and-data-collections) to see whether your existing collections are affected and require updating.

If you are unable to make any changes to your collections at this time, you can [enable the `legacy.collections` flag](#enabling-the-legacycollections-flag) which will allow you to keep your collections in their current state until the legacy flag is no longer supported.

Learn more about the updated [content collections](/en/guides/content-collections/).

##### Updating existing collections

[Section titled “Updating existing collections”](#updating-existing-collections)

See the instructions below for updating an existing content collection (`type: 'content'` or `type: 'data'`) to use the Content Layer API.

Step-by-step instructions to update a collection

1. **Move the content config file**. This file no longer lives within the `src/content/` folder. This file should now exist at `src/content.config.ts`.

2. **Edit the collection definition**. Your updated collection requires a `loader` which indicates both a folder for the location of your collection (`base`) and a `pattern` defining the collection entry filenames and extensions to match. (You may need to update the example below accordingly. You can use [globster.xyz](https://globster.xyz/) to check your glob pattern.) The option to select a collection `type` is no longer available.

   src/content.config.ts

   ```diff
   import { defineCollection, z } from 'astro:content';
   +import { glob } from 'astro/loaders';


   const blog = defineCollection({
     // For content layer you no longer define a `type`
     type: 'content',
     loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: "./src/data/blog" }),
     schema: z.object({
       title: z.string(),
       description: z.string(),
       pubDate: z.coerce.date(),
       updatedDate: z.coerce.date().optional(),
     }),
   });
   ```

3. **Change references from `slug` to `id`**. Content layer collections do not have a reserved `slug` field. Instead, all updated collections will have an `id`:

   src/pages/\[slug].astro

   ```diff
   ---
   export async function getStaticPaths() {
     const posts = await getCollection('blog');
     return posts.map((post) => ({
   -    params: { slug: post.slug },
   +    params: { slug: post.id },
       props: post,
     }));
   }
   ---
   ```

   You can also update the dynamic routing file names to match the value of the changed `getStaticPaths()` parameter.

4. **Switch to the new `render()` function**. Entries no longer have a `render()` method, as they are now serializable plain objects. Instead, import the `render()` function from `astro:content`.

   src/pages/index.astro

   ```diff
   ---
   import { getEntry, render } from 'astro:content';


   const post = await getEntry('blog', params.slug);


   -const { Content, headings } = await post.render();
   +const { Content, headings } = await render(post);
   ---
   <Content />
   ```

##### Breaking changes to legacy `content` and `data` collections

[Section titled “Breaking changes to legacy content and data collections”](#breaking-changes-to-legacy-content-and-data-collections)

[Implementation PR: Implement legacy collections using glob (#11976)](https://github.com/withastro/astro/pull/11976)

By default, collections that use the old `type` property (`content` or `data`) and do not define a `loader` are now implemented under the hood using the Content Layer API’s built-in `glob()` loader, with extra backward-compatibility handling.

Additionally, temporary backwards compatibility exists for keeping the content config file in its original location of `src/content/config.ts`.

This backwards compatibility implementation is able to emulate most of the features of legacy collections and will allow many legacy collections to continue to work even without updating your code. However, **there are some differences and limitations that may cause breaking changes to existing collections**:

* In previous versions of Astro, collections would be generated for all folders in `src/content/`, even if they were not defined in `src/content/config.ts`. This behavior is now deprecated, and collections should always be defined in `src/content.config.ts`. For existing collections, these can just be empty declarations (e.g. `const blog = defineCollection({})`) and Astro will implicitly define your legacy collection for you in a way that is compatible with the new loading behavior.
* The special `layout` field is not supported in Markdown collection entries. This property is intended only for standalone page files located in `src/pages/` and not likely to be in your collection entries. However, if you were using this property, you must now create dynamic routes that include your page styling.
* Sort order of generated collections is non-deterministic and platform-dependent. This means that if you are calling `getCollection()`, the order in which entries are returned may be different than before. If you need a specific order, you must sort the collection entries yourself.
* `image().refine()` is not supported. If you need to validate the properties of an image you will need to do this at runtime in your page or component.
* The `key` argument of `getEntry(collection, key)` is typed as `string`, rather than having types for every entry.
* Previously when calling `getEntry(collection, key)` with a static string as the key, the return type was not nullable. The type now includes `undefined` so you must check if the entry is defined before using the result or you will have type errors.

##### Enabling the `legacy.collections` flag

[Section titled “Enabling the legacy.collections flag”](#enabling-the-legacycollections-flag)

[Implementation PR: Implement legacy collections using glob (#11976)](https://github.com/withastro/astro/pull/11976)

If you are not yet ready to update your existing collections, you can enable the [`legacy.collections`](/en/reference/legacy-flags/) flag and your existing collections will continue to function as before.

## Deprecated

[Section titled “Deprecated”](#deprecated)

The following deprecated features are no longer supported and are no longer documented. Please update your project accordingly.

Some deprecated features may temporarily continue to function until they are completely removed. Others may silently have no effect, or throw an error prompting you to update your code.

### Deprecated: `Astro.glob()`

[Section titled “Deprecated: Astro.glob()”](#deprecated-astroglob)

[Implementation PR: Deprecate glob (#11826)](https://github.com/withastro/astro/pull/11826)

In Astro v4.x, you could use `Astro.glob()` in your `.astro` components to query multiple files in your project. This had some limitations (where it could be used, performance, etc.), and using querying functions from the Content Collections API or Vite’s own `import.meta.glob()` often provided more function and flexibility.

Astro 5.0 deprecates `Astro.glob()` in favor of using `getCollection()` to query your collections, and `import.meta.glob()` to query other source files in your project.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-3)

Replace all use of `Astro.glob()` with `import.meta.glob()`. Note that `import.meta.glob()` no longer returns a `Promise`, so you may have to update your code accordingly. You should not require any updates to your [glob patterns](/en/guides/imports/#glob-patterns).

src/pages/blog.astro

```diff
---
-const posts = await Astro.glob('./posts/*.md');
+const posts = Object.values(import.meta.glob('./posts/*.md', { eager: true }));
---


{posts.map((post) => <li><a href={post.url}>{post.frontmatter.title}</a></li>)}
```

Where appropriate, consider using [content collections](/en/guides/content-collections/) to organize your content, which has its own newer, more performant querying functions.

You may also wish to consider using glob packages from NPM, such as [`fast-glob`](https://www.npmjs.com/package/fast-glob).

Learn more about [importing files with `import.meta.glob`](/en/guides/imports/#importmetaglob).

### Deprecated: `functionPerRoute` (Adapter API)

[Section titled “Deprecated: functionPerRoute (Adapter API)”](#deprecated-functionperroute-adapter-api)

[Implementation PR: Remove functionPerRoute option (#11714)](https://github.com/withastro/astro/pull/11714)

In Astro v4.x, you could opt into creating a separate file for each route defined in the project, mirroring your `src/pages/` directory in the build folder. By default, Astro emitted a single `entry.mjs` file, which was responsible for emitting the rendered page on each request.

Astro v5.0 removes the option to opt out of the default behavior. This behavior is now standard, and non-configurable.

Remove the `functionPerRoute` property from your `adapterFeatures` configuration. It is no longer available.

my-adapter.mjs

```diff
export default function createIntegration() {
  return {
    name: '@matthewp/my-adapter',
    hooks: {
      'astro:config:done': ({ setAdapter }) => {
        setAdapter({
          name: '@matthewp/my-adapter',
          serverEntrypoint: '@matthewp/my-adapter/server.js',
          adapterFeatures: {
-              functionPerRoute: true
          }
        });
      },
    },
  };
}
```

Learn more about [the Adapter API](/en/reference/adapter-reference/) for building adapter integrations.

### Deprecated: `routes` on `astro:build:done` hook (Integration API)

[Section titled “Deprecated: routes on astro:build:done hook (Integration API)”](#deprecated-routes-on-astrobuilddone-hook-integration-api)

[Implementation PR: feat(next): astro:routes:resolved (#12329)](https://github.com/withastro/astro/pull/12329)

In Astro v4.x, integrations accessed routes from the `astro:build:done` hook.

Astro v5.0 deprecates the `routes` array passed to this hook. Instead, it exposes a new `astro:routes:resolved` hook that runs before `astro:config:done`, and whenever a route changes in development. It has all the same properties of the deprecated `routes` list, except `distURL` which is only available during build.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-4)

Remove any instance of `routes` passed to `astro:build:done` and replace it with the new `astro:routes:resolved` hook. Access `distURL` on the newly exposed `assets` map:

my-integration.mjs

```diff
const integration = () => {
    let routes
    return {
        name: 'my-integration',
        hooks: {
            +'astro:routes:resolved': (params) => {
                +routes = params.routes
            },
            'astro:build:done': ({
                -routes
                +assets
            }) => {
                for (const route of routes) {
                    const distURL = assets.get(route.pattern)
                    if (distURL) {
                        +Object.assign(route, { distURL })
                    }
                }
                console.log(routes)
            }
        }
    }
}
```

Learn more about [the Integration API `astro:routes:resolved` hook](/en/reference/integrations-reference/#astroroutesresolved) for building integrations.

## Removed

[Section titled “Removed”](#removed)

The following features have now been entirely removed from the code base and can no longer be used. Some of these features may have continued to work in your project even after deprecation. Others may have silently had no effect.

Projects now containing these removed features will be unable to build, and there will no longer be any supporting documentation prompting you to remove these features.

### Removed: The Lit integration

[Section titled “Removed: The Lit integration”](#removed-the-lit-integration)

[Implementation PR: Remove \`@astrojs/lit\` (#11680)](https://github.com/withastro/astro/pull/11680)

In Astro v4.x, [Lit](https://lit.dev/) was a core-maintained framework library through the `@astrojs/lit` package.

Astro v5.0 removes the integration and it will not receive further updates for compatibility with 5.x and above.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-5)

You can continue to use Lit for client components by adding a client-side script tag. For example:

```astro
<script>
  import "../components/MyTabs";
</script>


<my-tabs title="These are my tabs">...</my-tabs>
```

If you’re interested in maintaining a Lit integration yourself, you may wish to use the [last published version of `@astrojs/lit`](https://github.com/withastro/astro/tree/astro%404.13.0/packages/integrations/lit) as a starting point and upgrade the relevant packages.

Learn more about [Astro’s official integrations](/en/guides/integrations/).

### Removed: `hybrid` rendering mode

[Section titled “Removed: hybrid rendering mode”](#removed-hybrid-rendering-mode)

[Implementation PR: Merge output:hybrid and output:static (#11824)](https://github.com/withastro/astro/pull/11824)

In Astro v4.x, Astro provided three rendering `output` rendering modes: `'static'`, `'hybrid'`, and `'server'`

Astro v5.0 merges the `output: 'hybrid'` and `output: 'static'` configurations into one single configuration (now called `'static'`) that works the same way as the previous hybrid option.

It is no longer necessary to specify `output: 'hybrid'` in your Astro config to use server-rendered pages. The new `output: 'static'` has this capability included.

Astro will now automatically allow you to opt out of prerendering in your static site with no change to your output configuration required. Any page route or endpoint can include `export const prerender = false` to be server-rendered on demand, while the rest of your site is statically generated.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-6)

If your project used hybrid rendering, you must now remove the `output: 'hybrid'` option from your Astro config as it no longer exists. However, no other changes to your project are required, and you should have no breaking changes. The previous `'hybrid'` behavior is now the default, under a new name `'static'`.

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
-  output: 'hybrid',
});
```

If you were using the `output: 'static'` (default) option, you can continue to use it as before. By default, all of your pages will continue to be prerendered and you will have a completely static site. You should have no breaking changes to your project.

An adapter is still required to deploy an Astro project with any server-rendered pages, no matter which `output` mode your project uses. Failure to include an adapter will result in a warning in development and an error at build time.

Learn more about [on-demand rendering in Astro](/en/guides/on-demand-rendering/).

### Removed: support for dynamic `prerender` values in routes

[Section titled “Removed: support for dynamic prerender values in routes”](#removed-support-for-dynamic-prerender-values-in-routes)

[Implementation PR: Merge output:hybrid and output:static (#11824)](https://github.com/withastro/astro/pull/11824)

In Astro 4.x, environment variables could be used to dynamically set the value of `prerender` exports in routes, for example `export const prerender = import.meta.env.SOME_VAR`.

Astro v5.0 removes support for dynamic values in `prerender` exports. Only the static values `true` and `false` are supported.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-7)

1. Remove any dynamic `prerender` exports in your routes:

   src/pages/blog/\[slug].astro

   ```diff
   ---
   -export const prerender = import.meta.env.SOME_VAR;
   ---
   ```

2. Use an Astro integration in your `astro.config.mjs` file to set `prerender` values that need to be dynamic in the `"astro:route:setup"` hook:

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import { loadEnv } from 'vite';


   export default defineConfig({
     integrations: [
       {
         name: 'set-prerender',
         hooks: {
           'astro:route:setup': ({ route }) => {
             // Load environment variables from .env files (if needed)
             const { PRERENDER } = loadEnv(process.env.NODE_ENV, process.cwd(), '');
             // Find routes matching the expected filename.
             if (route.component.endsWith('/blog/[slug].astro')) {
               // Set the prerender value on routes as needed.
               route.prerender = PRERENDER;
             }
           },
         },
       }
     ],
   });
   ```

### Removed: Squoosh image service

[Section titled “Removed: Squoosh image service”](#removed-squoosh-image-service)

[Implementation PR: remove the squoosh image service (#11770)](https://github.com/withastro/astro/pull/11770)

In Astro 4.x, you could configure `image.service: squooshImageService()` to use Squoosh to transform your images instead of Sharp. However, the underlying library `libsquoosh` is no longer maintained and has memory and performance issues.

Astro 5.0 removes the Squoosh image optimization service entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-8)

To switch to the built-in Sharp image service, remove the `squooshImageService` import from your Astro config. By default, you will use Sharp for `astro:assets`.

astro.config.mjs

```diff
-import { squooshImageService } from "astro/config";
import { defineConfig } from "astro/config";


export default defineConfig({
- image: {
-   service: squooshImageService()
- }
});
```

If you are using a strict package manager like `pnpm`, you may need to install the `sharp` package manually to use the Sharp image service, even though it is built into Astro by default.

If your adapter does not support Astro’s built-in Sharp image optimization, you can [configure a no-op image service](/en/guides/images/#configure-no-op-passthrough-service) to allow you to use the `<Image />` and `<Picture />` components.

Alternatively, you may wish to consider [a community-maintained Squoosh image service](https://github.com/Princesseuh/astro-image-service-squoosh) if you are unable to use the Sharp image service.

##### For adapters

[Section titled “For adapters”](#for-adapters)

If your adapter previously precised its compatibility status with Squoosh, you should now remove this information from your adapter configuration.

my-adapter.mjs

```diff
supportedAstroFeatures: {
-  assets: {
-    isSquooshCompatible: true
-  }
}
```

Read more about [configuring your default image service](/en/guides/images/#default-image-service).

### Removed: some public-facing types

[Section titled “Removed: some public-facing types”](#removed-some-public-facing-types)

[Implementation PR: Refactor/types (#11715)](https://github.com/withastro/astro/pull/11715)

In Astro v4.x, `@types/astro.ts` exposed all types publicly to users, whether or not they were still actively used or only intended for internal use.

Astro v5.0 refactors this file to remove outdated and internal types. This refactor brings improvements to your editor (e.g. faster completions, lower memory usage, and more relevant completion options). However, this refactor may cause errors in some projects that have been relying on types that are no longer available to the public.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-9)

Remove any types that now cause errors in your project as you no longer have access to them. These are mostly APIs that have previously been deprecated and removed, but may also include types that are now internal.

See the [public types exposed for use](https://github.com/withastro/astro/tree/main/packages/astro/src/types/public).

### Experimental Flags

[Section titled “Experimental Flags”](#experimental-flags)

The following experimental flags have been removed in Astro v5.0 and these features are available for use:

* `env`
* `serverIslands`

Additionally, the following experimental flags have been removed and **are now the default or recommended behavior in Astro v5.0**.

* `directRenderScript` (See below for breaking changes to [default `<script>` behavior](#script-tags-are-rendered-directly-as-declared).)
* `globalRoutePriority` (See below for breaking changes to [default route priority order](#route-priority-order-for-injected-routes-and-redirects).)
* `contentLayer` (See guidance for [upgrading existing content collections](#legacy-v20-content-collections-api) to the new, preferred Content Layer API.)

The following experimental flags have been removed and **their corresponding features are not part of Astro v5.0**.

* `contentCollectionsCache`

Remove these experimental flags if you were previously using them, and move your `env` configuration to the root of your Astro config:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  experimental: {
-    directRenderScript: true,
-    globalRoutePriority: true,
-    contentLayer: true,
-    serverIslands: true,
-    contentCollectionsCache: true,
-    env: {
-      schema: {...}
-    }
  },
+  env: {
+      schema: {...}
+  }
})
```

These features are all available by default in Astro v5.0.

Read about these exciting features and more in [the v5.0 Blog post](https://astro.build/blog/astro-5/).

## Changed Defaults

[Section titled “Changed Defaults”](#changed-defaults)

Some default behavior has changed in Astro v5.0 and your project code may need updating to account for these changes.

In most cases, the only action needed is to review your existing project’s deployment and ensure that it continues to function as you expect, making updates to your code as necessary. In some cases, there may be a configuration setting to allow you to continue to use the previous default behavior.

### CSRF protection is now set by default

[Section titled “CSRF protection is now set by default”](#csrf-protection-is-now-set-by-default)

[Implementation PR: change default value of checkOrigin (#11788)](https://github.com/withastro/astro/pull/11788)

In Astro v4.x, The default value of `security.checkOrigin` was `false`. Previously, you had to explicitly set this value to `true` to enable Cross-Site Request Forgery (CSRF) protection.

Astro v5.0 changes the default value of this option to `true`, and will automatically check that the “origin” header matches the URL sent by each request in on-demand rendered pages.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-10)

If you had previously configured `security.checkOrigin: true`, you no longer need this line in your Astro config. This is now the default.

To disable this behavior, you must explicitly set `security.checkOrigin: false`.

astro.config.mjs

```diff
export default defineConfig({
  output: "server",
+  security: {
+    checkOrigin: false
+  }
})
```

Read more about [security configuration options](/en/reference/configuration-reference/#security)

### Route priority order for injected routes and redirects

[Section titled “Route priority order for injected routes and redirects”](#route-priority-order-for-injected-routes-and-redirects)

[Implementation PR: Remove legacy route prioritization (#11798)](https://github.com/withastro/astro/pull/11798)

In Astro v4.x, `experimental.globalRoutePriority` was an optional flag that ensured that injected routes, file-based routes, and redirects were all prioritized using the [route priority order rules for all routes](/en/guides/routing/#route-priority-order). This allowed more control over routing in your project by not automatically prioritizing certain kinds of routes and standardizing the route priority order.

Astro v5.0 removes this experimental flag and makes this the new default behavior in Astro: redirects and injected routes are now prioritized equally alongside file-based project routes.

Note that this was already the default behavior in Starlight, and should not affect updated Starlight projects.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-11)

If your project includes injected routes or redirects, please check that your routes are building page URLs as expected. An example of the new expected behavior is shown below.

In a project containing the following routes:

* File-based route: `/blog/post/[pid]`
* File-based route: `/[page]`
* Injected route: `/blog/[...slug]`
* Redirect: `/blog/tags/[tag] -> /[tag]`
* Redirect: `/posts -> /blog`

The following URLs will be built (instead of following the route priority order of Astro v4.x):

* `/blog/tags/astro` is built by the redirect to `/tags/[tag]` (instead of the injected route `/blog/[...slug]`)
* `/blog/post/0` is built by the file-based route `/blog/post/[pid]` (instead of the injected route `/blog/[...slug]`)
* `/posts` is built by the redirect to `/blog` (instead of the file-based route `/[page]`)

In the event of route collisions, where two routes of equal route priority attempt to build the same URL, Astro will log a warning identifying the conflicting routes.

Read more about the [route priority order rules](/en/guides/routing/#route-priority-order).

### `<script>` tags are rendered directly as declared

[Section titled “\<script> tags are rendered directly as declared”](#script-tags-are-rendered-directly-as-declared)

[Implementation PR: Make directRenderScript the default (#11791)](https://github.com/withastro/astro/pull/11791)

In Astro v4.x, `experimental.directRenderScript` was an optional flag to directly render `<scripts>` as declared in `.astro` files (including existing features like TypeScript, importing `node_modules`, and deduplicating scripts). This strategy prevented scripts from being executed in places where they were not used. Additionally, conditionally rendered scripts were previously implicitly inlined, as if an `is:inline` directive was automatically added to them.

Astro 5.0 removes this experimental flag and makes this the new default behavior in Astro: scripts are no longer hoisted to the `<head>`, multiple scripts on a page are no longer bundled together, and a `<script>` tag may interfere with CSS styling. Additionally, conditionally rendered scripts are no longer implicitly inlined.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-12)

Please review your `<script>` tags and ensure they behave as desired.

If you previously had conditionally rendered `<script>` tags, you will need to add an `is:inline` attribute to preserve the same behavior as before:

src/components/MyComponent.astro

```astro
---
type Props = {
  showAlert: boolean
}


const { showAlert } = Astro.props;
---
{
  showAlert && <script is:inline>alert("Some very important code!!")</script>
}
```

Read more about [using `script` tags in Astro](/en/guides/client-side-scripts/).

## Breaking Changes

[Section titled “Breaking Changes”](#breaking-changes)

The following changes are considered breaking changes in Astro v5.0. Breaking changes may or may not provide temporary backwards compatibility. If you were using these features, you may have to update your code as recommended in each entry.

### Renamed: `<ViewTransitions />` component

[Section titled “Renamed: \<ViewTransitions /> component”](#renamed-viewtransitions--component)

[Implementation PR: Rename the ViewTransitions component to ClientRouter (#11980)](https://github.com/withastro/astro/pull/11980)

In Astro 4.x, Astro’s View Transitions API included a `<ViewTransitions />` router component to enable client-side routing, page transitions, and more.

Astro 5.0 renames this component to `<ClientRouter />` to clarify the role of the component within the API. This makes it more clear that the features you get from Astro’s `<ClientRouter />` routing component are slightly different from the native CSS-based MPA router.

No functionality has changed. This component has only changed its name.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-13)

Replace all occurrences of the `ViewTransitions` import and component with `ClientRouter`:

src/layouts/MyLayout.astro

```diff
-import { ViewTransitions } from 'astro:transitions';
+import { ClientRouter } from 'astro:transitions';


<html>
  <head>
    ...
   -<ViewTransitions />
   +<ClientRouter />
  </head>
</html>
```

Read more about [view transitions and client-side routing in Astro](/en/guides/view-transitions/).

### Changed: TypeScript configuration

[Section titled “Changed: TypeScript configuration”](#changed-typescript-configuration)

[Implementation PR: better tsconfig (#11859)](https://github.com/withastro/astro/pull/11859)

In Astro v4.x, Astro relied on a `src/env.d.ts` file for type inferencing and defining modules for features that relied on generated types.

Astro 5.0 instead uses a `.astro/types.d.ts` file for type inferencing, and now recommends setting `include` and `exclude` in `tsconfig.json` to benefit from Astro types and avoid checking built files.

Running `astro sync` no longer creates, nor updates, `src/env.d.ts` as it is not required for type-checking standard Astro projects.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-14)

To update your project to Astro’s recommended TypeScript settings, add the following `include` and `exclude` properties to your existing `tsconfig.json`:

tsconfig.json

```diff
{
  "extends": "astro/tsconfigs/base",
  +"include": [".astro/types.d.ts", "**/*"],
  +"exclude": ["dist"]
}
```

Note that `src/env.d.ts` is only necessary if you have added custom configurations, or if you’re not using a `tsconfig.json` file.

Read more about [TypeScript configuration in Astro](/en/guides/typescript/#setup).

### Changed: Actions submitted by HTML forms no longer use cookie redirects

[Section titled “Changed: Actions submitted by HTML forms no longer use cookie redirects”](#changed-actions-submitted-by-html-forms-no-longer-use-cookie-redirects)

[Implementation PR: Actions middleware (#12373)](https://github.com/withastro/astro/pull/12373)

In Astro 4.x, actions called from an HTML form would trigger a redirect with the result forwarded using cookies. This caused issues for large form errors and return values that exceeded the 4 KB limit of cookie-based storage.

Astro 5.0 now renders the result of an action as a POST result without any forwarding. This will introduce a “confirm form resubmission?” dialog when a user attempts to refresh the page, though it no longer imposes a 4 KB limit on action return value.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-15)

You should update handling for action results that relies on redirects, and optionally address the “confirm form resubmission?” dialog with middleware.

##### To redirect to the previous route on error

[Section titled “To redirect to the previous route on error”](#to-redirect-to-the-previous-route-on-error)

If your HTML form action is directed to a different route (i.e. `action={"/success-page" + actions.name}`), Astro will no longer redirect to the previous route on error. You can implement this behavior manually using redirects from your Astro component. This example instead redirects to a new route on success, and handles errors on the current page otherwise:

src/pages/newsletter.astro

```diff
---
import { actions } from 'astro:actions';


+const result = Astro.getActionResult(actions.newsletter);
+if (!result?.error) {
  +// Embed relevant result data in the URL if needed
  +// example: redirect(`/confirmation?email=${result.data.email}`);
  +return redirect('/confirmation');
+}
---


<form method="POST" action={'/confirmation' + actions.newsletter}>
  <label>E-mail <input required type="email" name="email" /></label>
  <button>Sign up</button>
</form>
```

##### (Optional) To remove the confirm dialog on refresh

[Section titled “(Optional) To remove the confirm dialog on refresh”](#optional-to-remove-the-confirm-dialog-on-refresh)

To address the “confirm form resubmission?” dialog on refresh, or to preserve action results across sessions, you can now [customize action result handling from middleware](/en/guides/actions/#advanced-persist-action-results-with-a-session).

We recommend using a session storage provider [as described in our Netlify Blob example](/en/guides/actions/#advanced-persist-action-results-with-a-session). However, if you prefer the cookie forwarding behavior from 4.X and accept the 4 KB size limit, you can implement the pattern as shown in this sample snippet:

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  // Skip requests for prerendered pages
  if (context.isPrerendered) return next();


  const { action, setActionResult, serializeActionResult } = getActionContext(context);


  // If an action result was forwarded as a cookie, set the result
  // to be accessible from `Astro.getActionResult()`
  const payload = context.cookies.get('ACTION_PAYLOAD');
  if (payload) {
    const { actionName, actionResult } = payload.json();
    setActionResult(actionName, actionResult);
    context.cookies.delete('ACTION_PAYLOAD', { path: '/' });
    return next();
  }


  // If an action was called from an HTML form action,
  // call the action handler and redirect with the result as a cookie.
  if (action?.calledFrom === 'form') {
    const actionResult = await action.handler();


    context.cookies.set('ACTION_PAYLOAD', {
      actionName: action.name,
      actionResult: serializeActionResult(actionResult),
    }, {
      path: '/',
      httpOnly: true,
      sameSite: 'lax',
      maxAge: 60
    });


    if (actionResult.error) {
    // Redirect back to the previous page on error
      const referer = context.request.headers.get('Referer');
      if (!referer) {
        throw new Error('Internal: Referer unexpectedly missing from Action POST request.');
      }
      return context.redirect(referer);
    }
    // Redirect to the destination page on success
    return context.redirect(context.originPathname);
  }


  return next();
})
```

### Changed: `compiledContent()` is now an async function

[Section titled “Changed: compiledContent() is now an async function”](#changed-compiledcontent-is-now-an-async-function)

[Implementation PR: Remove TLA by making compiledContent async (#11782)](https://github.com/withastro/astro/pull/11782)

In Astro 4.x, top level await was included in Markdown modules. This caused some issues with custom image services and images inside Markdown, causing Node to suddenly exit with no error message.

Astro 5.0 makes the `compiledContent()` property on Markdown import an async function, requiring an `await` to resolve the content.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-16)

Update your code to use `await` when calling `compiledContent()`.

src/pages/post.astro

```diff
---
import * as myPost from "../blog/post.md";


-const content = myPost.compiledContent();
+const content = await myPost.compiledContent();
---


<Fragment set:html={content} />
```

Read more about the [`compiledContent()` function](/en/guides/markdown-content/#importing-markdown) for returning compiled Markdown.

### Changed: `astro:content` can no longer be used on the client

[Section titled “Changed: astro:content can no longer be used on the client”](#changed-astrocontent-can-no-longer-be-used-on-the-client)

[Implementation PR: Prevent usage of \`astro:content\` in the client (#11827)](https://github.com/withastro/astro/pull/11827)

In Astro 4.x, it was possible to access the `astro:content` module on the client.

Astro 5.0 removes this access as it was never intentionally exposed for client use. Using `astro:content` this way had limitations and bloated client bundles.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-17)

If you are currently using `astro:content` in the client, pass the data you need through props to your client components instead:

src/pages/blog.astro

```astro
---
import { getCollection } from 'astro:content';
import ClientComponent from '../components/ClientComponent';


const posts = await getCollection('blog');
const postsData = posts.map(post => post.data);
---


<ClientComponent posts={postsData} />
```

Read more about [the `astro:content` API](/en/reference/modules/astro-content/).

### Renamed: Shiki `css-variables` theme color token names

[Section titled “Renamed: Shiki css-variables theme color token names”](#renamed-shiki-css-variables-theme-color-token-names)

[Implementation PR: Update to new shiki token names (#11661)](https://github.com/withastro/astro/pull/11661)

In Astro v4.x, the Shiki `css-variables` theme used the `--astro-code-color-text` and `--astro-code-color-background` tokens for styling the foreground and background colors of code blocks respectively.

Astro v5.0 renames them to `--astro-code-foreground` and `--astro-code-background` respectively to better align with the Shiki v1 defaults.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-18)

You can perform a global find and replace in your project to migrate to the new token names.

src/styles/global.css

```diff
:root {
  ---astro-code-color-text: #000;
  ---astro-code-color-background: #fff;
  +--astro-code-foreground: #000;
  +--astro-code-background: #fff;
}
```

Read more about [syntax highlighting in Astro](/en/guides/syntax-highlighting/).

### Changed: internal Shiki rehype plugin for highlighting code blocks

[Section titled “Changed: internal Shiki rehype plugin for highlighting code blocks”](#changed-internal-shiki-rehype-plugin-for-highlighting-code-blocks)

[Implementation PR: Refactor createShikiHighlighter (#11825)](https://github.com/withastro/astro/pull/11825)

In Astro 4.x, Astro’s internal Shiki rehype plugin highlighted code blocks as HTML.

Astro 5.0 updates this plugin to highlight code blocks as hast. This allows a more direct Markdown and MDX processing and improves the performance when building the project. However, this may cause issues with existing Shiki transformers.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-19)

If you are using Shiki transformers passed to `markdown.shikiConfig.transformers`, you must make sure they do not use the `postprocess` hook. This hook no longer runs on code blocks in `.md` and `.mdx` files. (See [the Shiki documentation on transformer hooks](https://shiki.style/guide/transformers#transformer-hooks) for more information).

Code blocks in `.mdoc` files and Astro’s built-in `<Code />` component do not use the internal Shiki rehype plugin and are unaffected.

Read more about [syntax highlighting in Astro](/en/guides/syntax-highlighting/).

### Changed: Automatic `charset=utf-8` behavior for Markdown and MDX pages

[Section titled “Changed: Automatic charset=utf-8 behavior for Markdown and MDX pages”](#changed-automatic-charsetutf-8-behavior-for-markdown-and-mdx-pages)

[Implementation PR: Unset charset=utf-8 content-type for md/mdx pages (#12231)](https://github.com/withastro/astro/pull/12231)

In Astro 4.0, Markdown and MDX pages (located in `src/pages/`) automatically responded with `charset=utf-8` in the `Content-Type` header, which allowed rendering non-ASCII characters in your pages.

Astro 5.0 updates the behaviour to add the `<meta charset="utf-8">` tag instead, and only for pages that do not use Astro’s special `layout` frontmatter property. Similarly for MDX pages, Astro will only add the tag if the MDX content does not import a wrapping `Layout` component.

If your Markdown or MDX pages use the `layout` frontmatter property, or if the MDX page content imports a wrapping `Layout` component, then the HTML encoding will be handled by the designated layout component instead, and the `<meta charset="utf-8">` tag will not be added to your page by default.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-20)

If you require `charset=utf-8` to render your page correctly, make sure that your layout components contain the `<meta charset="utf-8">` tag. You may need to add this if you have not already done so.

Read more about [Markdown layouts](/en/basics/layouts/#markdown-layouts).

### Changed: Astro-specific metadata attached in remark and rehype plugins

[Section titled “Changed: Astro-specific metadata attached in remark and rehype plugins”](#changed-astro-specific-metadata-attached-in-remark-and-rehype-plugins)

[Implementation PR: Clean up Astro metadata in vfile.data (#11861)](https://github.com/withastro/astro/pull/11861)

In Astro 4.x, the Astro-specific metadata attached to `vfile.data` in remark and rehype plugins was attached in different locations with inconsistent names.

Astro 5 cleans up the API and the metadata is now renamed as below:

* `vfile.data.__astroHeadings` -> `vfile.data.astro.headings`
* `vfile.data.imagePaths` -> `vfile.data.astro.imagePaths`

The types of `imagePaths` has also been updated from `Set<string>` to `string[]`. The `vfile.data.astro.frontmatter` metadata is left unchanged.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-21)

While we don’t consider these APIs public, they can be accessed by remark and rehype plugins that want to re-use Astro’s metadata. If you are using these APIs, make sure to access them in the new locations.

Read more about [using Markdown plugins in Astro](/en/guides/markdown-content/#markdown-plugins).

### Changed: image endpoint configuration

[Section titled “Changed: image endpoint configuration”](#changed-image-endpoint-configuration)

[Implementation PR: Allow customising the route of the image endpoint (#11908)](https://github.com/withastro/astro/pull/11908)

In Astro 4.x, you could set an endpoint in your `image` configuration to use for image optimization.

Astro 5.0 allows you to customize a `route` and `entrypoint` of the `image.endpoint` config. This can be useful in niche situations where the default route `/_image` conflicts with an existing route or your local server setup.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-22)

If you had previously customized `image.endpoint`, move this endpoint to the new `endpoint.entrypoint` property. Optionally, you may customize a `route`:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


defineConfig({
  image: {
-    endpoint: './src/image-endpoint.ts',
+    endpoint: {
+      route: "/image",
+      entrypoint: "./src/image_endpoint.ts"
+    }
  },
})
```

Read more about [setting an endpoint to use for image optimization](/en/reference/configuration-reference/#imageendpoint).

### Changed: `build.client` and `build.server` resolve behavior

[Section titled “Changed: build.client and build.server resolve behavior”](#changed-buildclient-and-buildserver-resolve-behavior)

[Implementation PR: Fix build.client and build.server resolve behaviour (#11916)](https://github.com/withastro/astro/pull/11916)

In Astro v4.x, the `build.client` and `build.server` options were documented to resolve relatively from the `outDir` option, but it didn’t always work as expected.

Astro 5.0 fixes the behavior to correctly resolve from the `outDir` option. For example, if `outDir` is set to `./dist/nested/`, then by default:

* `build.client` will resolve to `<root>/dist/nested/client/`
* `build.server` will resolve to `<root>/dist/nested/server/`

Previously the values were incorrectly resolved:

* `build.client` was resolved to `<root>/dist/nested/dist/client/`
* `build.server` was resolved to `<root>/dist/nested/dist/server/`

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-23)

If you were relying on the previous build paths, make sure that your project code is updated to the new build paths.

Read more about [`build` configuration options in Astro](/en/reference/configuration-reference/#build-options).

### Changed: JS dependencies in config file are no longer processed by Vite

[Section titled “Changed: JS dependencies in config file are no longer processed by Vite”](#changed-js-dependencies-in-config-file-are-no-longer-processed-by-vite)

[Implementation PR: Set external: true when loading astro config (#11819)](https://github.com/withastro/astro/pull/11819)

In Astro 4.x, locally-linked JS dependencies (e.g. `npm link`, in a monorepo, etc) were able to use Vite features like `import.meta.glob` when imported by the Astro config file.

Astro 5 updates the Astro config loading flow to ignore processing locally-linked JS dependencies with Vite. Dependencies exporting raw TypeScript files are unaffected. Instead, these JS dependencies will be normally imported by the Node.js runtime the same way as other dependencies from `node_modules`.

This change was made as the previous behavior caused confusion among integration authors who tested against a package that worked locally, but not when published. It also restricted using CJS-only dependencies because Vite required the code to be ESM. While this change only affects JS dependencies, it’s also recommended for packages to export JavaScript instead of raw TypeScript where possible to prevent accidental Vite-specific usage as it’s an implementation detail of Astro’s config loading flow.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-24)

Make sure your locally-linked JS dependencies are built before running your Astro project. Then, the config loading should work as before.

Read more about [Vite configuration settings in Astro](/en/reference/configuration-reference/#vite).

### Changed: URLs returned by `paginate()`

[Section titled “Changed: URLs returned by paginate()”](#changed-urls-returned-by-paginate)

[Implementation PR: Add base to paginate (#11253)](https://github.com/withastro/astro/pull/11253)

In Astro v4.x, the URL returned by `paginate()` (e.g. `page.url.next`, `page.url.first`, etc.) did not include the value set for `base` in your Astro config. You had to manually prepend your configured value for `base` to the URL path.

Astro 5.0 automatically includes the `base` value in `page.url`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-25)

If you are using the `paginate()` function for these URLs, remove any existing `base` value as it is now added for you:

```diff
---
export async function getStaticPaths({ paginate }) {
  const astronautPages = [{
    astronaut: 'Neil Armstrong',
  }, {
    astronaut: 'Buzz Aldrin',
  }, {
    astronaut: 'Sally Ride',
  }, {
    astronaut: 'John Glenn',
  }];
  return paginate(astronautPages, { pageSize: 1 });
}
const { page } = Astro.props;
// `base: /'docs'` configured in `astro.config.mjs`
-const prev = "/docs" + page.url.prev;
+const prev = page.url.prev;
---
<a id="prev" href={prev}>Back</a>
```

Read more about [pagination in Astro](/en/guides/routing/#pagination).

### Changed: non-boolean HTML attribute values

[Section titled “Changed: non-boolean HTML attribute values”](#changed-non-boolean-html-attribute-values)

[Implementation PR: Fix attribute rendering for boolean values (take 2) (#11660)](https://github.com/withastro/astro/pull/11660)

In Astro v4.x, non-[boolean HTML attributes](https://developer.mozilla.org/en-US/docs/Glossary/Boolean/HTML) may not have included their values when rendered to HTML.

Astro v5.0 renders the values explicitly as `="true"` or `="false"`, matching proper attribute handling in browsers.

In the following `.astro` examples, only `allowfullscreen` is a boolean attribute:

src/pages/index.astro

```astro
<!-- `allowfullscreen` is a boolean attribute -->
<p allowfullscreen={true}></p>
<p allowfullscreen={false}></p>
<!-- `inherit` is *not* a boolean attribute -->
<p inherit={true}></p>
<p inherit={false}></p>
<!-- `data-*` attributes are not boolean attributes -->
<p data-light={true}></p>
<p data-light={false}></p>
```

Astro v5.0 now preserves the full data attribute with its value when rendering the HTML of non-boolean attributes:

```diff
<p allowfullscreen></p>
<p></p>


<p inherit="true"></p>
<p inherit></p>
<p inherit="false"></p>


<p data-light></p>
<p data-light="true"></p>
<p></p>
<p data-light="false"></p>
```

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-26)

If you rely on attribute values, for example, to locate elements or to conditionally render, update your code to match the new non-boolean attribute values:

```diff
-el.getAttribute('inherit') === ''
+el.getAttribute('inherit') === 'false'


-el.hasAttribute('data-light')
+el.dataset.light === 'true'
```

Read more about [using HTML attributes in Astro](/en/reference/astro-syntax/#dynamic-attributes).

### Changed: adding values to `context.locals`

[Section titled “Changed: adding values to context.locals”](#changed-adding-values-to-contextlocals)

[Implementation PR: TODOs (#11987)](https://github.com/withastro/astro/pull/11987)

In Astro 4.x, it was possible to completely replace the entire `locals` object in middleware, API endpoints, and pages when adding new values.

Astro 5.0 requires you to append values to the existing `locals` object without deleting it. Locals in middleware, API endpoints, and pages, can no longer be completely overridden.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-27)

Where you previously were overwriting the object, you must now instead assign values to it:

src/middleware.js

```diff
-ctx.locals = {
Object.assign(ctx.locals, {
  one: 1,
  two: 2
-}
+})
```

See more about [storing data in `context.locals`](/en/guides/middleware/#storing-data-in-contextlocals).

### Changed: `params` no longer decoded

[Section titled “Changed: params no longer decoded”](#changed-params-no-longer-decoded)

[Implementation PR: decode pathname early, don't decode params (#12079)](https://github.com/withastro/astro/pull/12079)

In Astro v4.x, `params` passed to `getStaticPath()` were automatically decoded using `decodeURIComponent`.

Astro v5.0 no longer decodes the value of `params` passed to `getStaticPaths`. You must manually decode them yourself if needed.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-28)

If you were previously relying on the automatic decoding, use `decodeURI` when passing `params`.

src/pages/\[id].astro

```diff
---
export function getStaticPaths() {
  return [
-    { params: { id: "%5Bpage%5D" } },
+    { params: { id: decodeURI("%5Bpage%5D") } },
  ]
}


const { id } = Astro.params;
---
```

Note that the use of [`decodeURIComponent`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) is discouraged for `getStaticPaths` because it decodes more characters than it should, for example `/`, `?`, `#` and more.

Read more about [creating dynamic routes with `params`](/en/guides/routing/#static-ssg-mode).

### Changed: `RouteData` type replaced by `IntegrationsRouteData` (Integrations API)

[Section titled “Changed: RouteData type replaced by IntegrationsRouteData (Integrations API)”](#changed-routedata-type-replaced-by-integrationsroutedata-integrations-api)

[Implementation PR: send \`IntegrationRouteData\` to integrations (#11864)](https://github.com/withastro/astro/pull/11864)

In Astro v4.x, the `entryPoints` type inside the `astro:build:ssr` and `astro:build:done` hooks was `RouteData`.

Astro v5.0 the `entryPoints` type is now `IntegrationRouteData`, which contains a subset of the `RouteData` type. The fields `isIndex` and `fallbackRoutes` were removed.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-29)

Update your adapter to change the type of `entryPoints` from `RouteData` to `IntegrationRouteData`.

```diff
-import type {RouteData} from 'astro';
+import type {IntegrationRouteData} from "astro"


-function useRoute(route: RouteData) {
+function useRoute(route: IntegrationRouteData) {
}
```

### Changed: `distURL` is now an array (Integrations API)

[Section titled “Changed: distURL is now an array (Integrations API)”](#changed-disturl-is-now-an-array-integrations-api)

[Implementation PR: send \`IntegrationRouteData\` to integrations (#11864)](https://github.com/withastro/astro/pull/11864)

In Astro v4.x, `RouteData.distURL` was `undefined` or a `URL`.

Astro v5.0 updates the shape of `IntegrationRouteData.distURL` to be `undefined` or an array of `URL`s. This fixes a previous error because a route can generate multiple files on disk, especially when using dynamic routes such as `[slug]` or `[...slug]`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-30)

Update your code to handle `IntegrationRouteData.distURL` as an array.

```diff
if (route.distURL) {
  -if (route.distURL.endsWith('index.html')) {
    -// do something
-  }
  +for (const url of route.distURL) {
    +if (url.endsWith('index.html')) {
      +// do something
+    }
+  }
}
```

See the [API reference for `distURL`](/en/reference/integrations-reference/#routedatadisturl).

### Changed: Arguments passed to `app.render()` (Adapter API)

[Section titled “Changed: Arguments passed to app.render() (Adapter API)”](#changed-arguments-passed-to-apprender-adapter-api)

[Implementation PR: TODOs (#11987)](https://github.com/withastro/astro/pull/11987)

In Astro 4.x, The Adapter API method `app.render()` could receive three arguments: a mandatory `request`, an object of options or a `routeData` object, and `locals`.

Astro 5.0 combines these last two arguments into a single options argument named `renderOptions`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-31)

Pass an object as the second argument to `app.render()`, which can include `routeData` and `locals` as properties.

```diff
-const response = await app.render(request, routeData, locals);
+const response = await app.render(request, {routeData, locals});
```

See the [Adapter API reference for `renderOptions`](/en/reference/modules/astro-app/#renderoptions).

### Changed: Properties on `supportedAstroFeatures` (Adapter API)

[Section titled “Changed: Properties on supportedAstroFeatures (Adapter API)”](#changed-properties-on-supportedastrofeatures-adapter-api)

[Implementation PR: rework supportedAstroFeatures (#11806)](https://github.com/withastro/astro/pull/11806)

In Astro 4.x, `supportedAstroFeatures`, which allows adapter authors to specify which features their integration supports, included an `assets` property to specify which of Astro’s image services were supported.

Astro 5.0 replaces this property with a dedicated `sharpImageService` property, used to determine whether the adapter is compatible with the built-in sharp image service.

v5.0 also adds a new `limited` value for the different properties of `supportedAstroFeatures` for adapters, which indicates that the adapter is compatible with the feature, but with some limitations. This is useful for adapters that support a feature, but not in all cases or with all options.

Additionally, the value of the different properties on `supportedAstroFeatures` for adapters can now be objects, with `support` and `message` properties. The content of the `message` property will show a helpful message in the Astro CLI when the adapter is not compatible with a feature. This is notably useful with the new `limited` value, to explain to the user why support is limited.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-32)

If you were using the `assets` property, remove this as it is no longer available. To specify that your adapter supports the built-in sharp image service, replace this with `sharpImageService`.

You may also wish to update your supported features with the new `limited` option and include a message about your adapter’s support.

my-adapter.mjs

```diff
supportedAstroFeatures: {
-  assets: {
-    supportKind: "stable",
-    isSharpCompatible: true,
-    isSquooshCompatible: true,
-  },
+  sharpImageService: {
+    support: "limited",
+    message: 'This adapter supports the built-in sharp image service, but with some limitations.'
+  }
}
```

Read more about [specifying supported Astro features in an adapter](/en/reference/adapter-reference/#astro-features).

### Removed: Deprecated definition shape for dev toolbar apps (Dev Toolbar API)

[Section titled “Removed: Deprecated definition shape for dev toolbar apps (Dev Toolbar API)”](#removed-deprecated-definition-shape-for-dev-toolbar-apps-dev-toolbar-api)

[Implementation PR: Remove deprecated dev toolbar app shape (#11987)](https://github.com/withastro/astro/pull/11987)

In Astro 4.x, when building a dev toolbar app, it was still possible to use the previously deprecated `addDevToolbarApp(string);` signature. The `id`, `title`, and `icon` properties to define the app were then made available through the default export of the app’s `entrypoint`.

Astro 5.0 completely removes this option entirely in favor of the current object shape when defining a dev toolbar app in an integration that’s more intuitive and allows Astro to provide better errors when toolbar apps fail to load correctly.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-33)

If you were using the deprecated shape, update your dev toolbar app to use the new shape:

my-integration.mjs

```diff
-// Old shape
-addDevToolbarApp("./my-dev-toolbar-app.mjs");


+// New shape
+addDevToolbarApp({
+  id: "my-app",
+  name: "My App",
+  icon: "<svg>...</svg>",
+  entrypoint: "./my-dev-toolbar-app.mjs",
+});
```

my-dev-toolbar-app.mjs

```diff
export default {
-  id: 'my-dev-toolbar-app',
-  title: 'My Dev Toolbar App',
-  icon: '🚀',
  init() {
    // ...
  }
}
```

Read more about [developing a dev toolbar app for Astro using the Dev Toolbar API](/en/reference/dev-toolbar-app-reference/).

### Removed: configuring Typescript during `create-astro`

[Section titled “Removed: configuring Typescript during create-astro”](#removed-configuring-typescript-during-create-astro)

[Implementation PR: create-astro updates (#12083)](https://github.com/withastro/astro/pull/12083)

In Astro v4.x, it was possible to choose between Astro’s three TypeScript settings when creating a new project using `create astro`, either by answering a question or by passing an associated `--typescript` flag with the desired TypeScript setting.

Astro 5.0 updates the `create astro` CLI command to remove the TypeScript question and its associated `--typescript` flag. The “strict” preset is now the default for all new projects created with the command line and it is no longer possible to customize this at that time. However, the TypeScript template can still be changed manually in `tsconfig.json`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-34)

If you were using the `--typescript` flag with `create-astro`, remove it from your command.

* npm

  ```diff
  -npm create astro@latest -- --template <example-name> --typescript strict
  +npm create astro@latest -- --template <example-name>
  ```

* pnpm

  ```diff
  -pnpm create astro@latest --template <example-name> --typescript strict
  +pnpm create astro@latest --template <example-name>
  ```

* Yarn

  ```diff
  -yarn create astro --template <example-name> --typescript strict
  +yarn create astro --template <example-name>
  ```

See [all the available `create astro` command flags](https://github.com/withastro/astro/blob/main/packages/create-astro/README.md)

## Community Resources

[Section titled “Community Resources”](#community-resources)

Know a good resource for Astro v5.0? [Edit this page](https://github.com/withastro/docs/edit/main/src/content/docs/en/guides/upgrade-to/v5.mdx) and add a link below!

## Known Issues

[Section titled “Known Issues”](#known-issues)

Please check [Astro’s issues on GitHub](https://github.com/withastro/astro/issues/) for any reported issues, or to file an issue yourself.

# Upgrade to Astro v6

> How to upgrade your project to Astro v6.0.

This guide will help you migrate from Astro v5 to Astro v6.

Need to upgrade an older project to v5 first? See our [older migration guide](/en/guides/upgrade-to/v5/).

Need to see the v5 docs? Visit this [older version of the docs site (unmaintained v5.18.0 snapshot)](https://v5.docs.astro.build/).

## Upgrade Astro

[Section titled “Upgrade Astro”](#upgrade-astro)

Update your project’s version of Astro to the latest version using your package manager:

* npm

  ```shell
  # Upgrade Astro and official integrations together
  npx @astrojs/upgrade
  ```

* pnpm

  ```shell
  # Upgrade Astro and official integrations together
  pnpm dlx @astrojs/upgrade
  ```

* Yarn

  ```shell
  # Upgrade Astro and official integrations together
  yarn dlx @astrojs/upgrade
  ```

You can also [upgrade your Astro integrations manually](/en/guides/integrations/#manual-upgrading) if needed, and you may also need to upgrade other dependencies in your project.

Need to continue?

After upgrading Astro, you may not need to make any changes to your project at all!

But, if you notice errors or unexpected behavior, please check below for what has changed that might need updating in your project.

Astro v6.0 includes [potentially breaking changes](#breaking-changes), as well as the removal and deprecation of some features.

If your project doesn’t work as expected after upgrading to v6.0, check this guide for an overview of all breaking changes and instructions on how to update your codebase.

See [the Astro changelog](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md) for full release notes.

## Dependency Upgrades

[Section titled “Dependency Upgrades”](#dependency-upgrades)

Any major upgrades to Astro’s dependencies may cause breaking changes in your project.

### Node 22

[Section titled “Node 22”](#node-22)

[Implementation PR: feat!: drop node 18 and 20 (#14427)](https://github.com/withastro/astro/pull/14427)

Node 18 reached its End of Life in March 2025 and Node 20 is scheduled to reach its End of Life in April 2026.

Astro v6.0 drops Node 18 and Node 20 support entirely so that all Astro users can take advantage of Node’s more modern features.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do)

Check that both your development environment and your deployment environment are using **Node `22.12.0` or higher**.

1. Check your local version of Node using:

   ```sh
   node -v
   ```

2. Check your [deployment environment’s](/en/guides/deploy/) own documentation to verify that they support Node 22.

   You can specify Node `22.12.0` for your Astro project either in a dashboard configuration setting or a `.nvmrc` file.

   .nvmrc

   ```bash
   22.12.0
   ```

### Vite 7.0

[Section titled “Vite 7.0”](#vite-70)

[Implementation PR: feat: update vite (#14445)](https://github.com/withastro/astro/pull/14445)

Astro v6.0 upgrades to Vite v7.0 as the development server and production bundler.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-1)

If you are using Vite-specific plugins, configuration, or APIs, check the [Vite migration guide](https://vite.dev/guide/migration) for their breaking changes and upgrade your project as needed.

Using [Astro’s `getViteConfig()` helper](/en/guides/testing/#vitest) requires at least Vitest v3.2 or v4.1 beta 5.

### Vite Environment API

[Section titled “Vite Environment API”](#vite-environment-api)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

Astro v6.0 introduces significant changes to how Astro manages different runtime environments (client, server, and prerender) after an internal refactor to use [Vite’s new Environments API](https://vite.dev/guide/api-environment).

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-2)

Integration and adapter maintainers should pay special attention to changes affecting these parts of the Integration API and Adapter API (full details included below with other breaking changes to these APIs):

* [Rollup output file name config path](#changed-rollup-output-file-name-config-path-vite-config)
* [integration hooks and HMR access patterns](#changed-integration-hooks-and-hmr-access-patterns-integration-api)
* [`SSRManifest` structure](#changed-ssrmanifest-interface-structure-adapter-api)
* [generating routes with `RouteData`](#removed-routedatagenerate-adapter-api)
* [routes with percent-encoded percent signs (e.g. `%25`)](#removed-percent-encoding-in-routes)
* [`astro:ssr-manifest` virtual module](#removed-astrossr-manifest-virtual-module-integration-api)
* [`NodeApp` from `astro/app/node`](#deprecated-nodeapp-from-astroappnode-adapter-api)
* [`loadManifest()` and `loadApp()` from `astro/app/node`](#deprecated-loadmanifest-and-loadapp-from-astroappnode-adapter-api)
* [`createExports()` and `start()`](#deprecated-createexports-and-start-adapter-api)

### Zod 4

[Section titled “Zod 4”](#zod-4)

Astro v6.0 upgrades to Zod 4, a major dependency update that may require changes to custom Zod schemas in your project.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-3)

If you have custom Zod schemas in your `content.config.ts` or other configuration files, you’ll need to update them for Zod 4. Refer to the [Zod migration guide](https://zod.dev/v4/changelog) for detailed changes in the Zod API.

Notably, [many `string()` formats have been deprecated](https://zod.dev/v4/changelog#deprecates-email-etc) (e.g. `z.string().email()`, `z.string.url()`), and their APIs have been moved to the top-level `z` namespace. You may need to update how you validate form input for your Astro Actions:

src/actions/index.ts

```diff
-email: z.string().email(),
+email: z.email(),
```

Additionally, Zod has made some [changes to handling error messages](https://zod.dev/v4/changelog#error-customization) and has dropped support for a custom `errorsMap` which was useful to redefine or translate your error messages. You may need to update any custom error messages:

src/actions/index.ts

```diff
-z.string().min(5, { message: "Too short." });
+z.string().min(5, { error: "Too short." });
```

Also, if you use [`.default()` with transforms](https://zod.dev/v4/changelog#default-updates), you may need to update your schemas. In Zod 4, default values must match the output type (after transforms), not the input type. The default value short-circuits parsing when the input is `undefined`:

src/content.config.ts

```diff
import { z } from 'astro/zod';


const blog = defineCollection({
  schema: z.object({
    -// Zod 3: default matched input type (string)
    views: z.string().transform(Number).default("0"),
    +// Zod 4: default must match output type (number)
    views: z.string().transform(Number).default(0),
  })
});
```

For the old behavior where defaults are parsed, use the new `.prefault()` method.

These are only some of the many changes upgrading from Zod 3 to Zod 4. If you encounter any issues with your Zod schemas after upgrading to Astro 6, please consult the [Zod 4 changelog](https://zod.dev/v4/changelog) for complete upgrade guidance.

Additionally, a [community codemod](https://github.com/nicoespeon/zod-v3-to-v4), which can potentially automate some of these changes when migrating from Zod 3 to Zod 4, is also available.

You can ensure you’re the same version of Zod that Astro uses internally by [importing Zod from `astro/zod`](#deprecated-astroschema-and-z-from-astrocontent).

```ts
import { z } from 'astro/zod';
```

See more about [the `astro/zod` module](/en/reference/modules/astro-zod/).

### Shiki 4.0

[Section titled “Shiki 4.0”](#shiki-40)

[Implementation PR: chore(deps): update shiki to v4 (#15726)](https://github.com/withastro/astro/pull/15726)

Astro v6.0 upgrades to Shiki v4.0 for syntax highlighting.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-4)

If you are using Shiki-specific APIs, check the [Shiki migration guide](https://shiki.style/blog/v4) for their breaking changes and upgrade your project as needed.

### Official Astro integrations

[Section titled “Official Astro integrations”](#official-astro-integrations)

All of [Astro’s official server adapters](/en/guides/on-demand-rendering/#server-adapters) have also updated to a new major version to accompany the upgrade to Vite v7.0 with Vite’s Environment API as the development server and production bundler.

In particular, Astro’s Cloudflare adapter has undergone significant changes, and breaking changes to your existing Cloudflare setup are expected.

See the [Cloudflare adapter upgrade instructions](/en/guides/integrations-guide/cloudflare/#upgrading-to-v13-and-astro-6) for detailed migration guidance.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-5)

If you are using an Astro adapter for on-demand rendering or other platform-specific features, please check your specific adapter’s changelog for upgrade guidance:

* [`@astrojs/cloudflare` CHANGELOG](https://github.com/withastro/astro/blob/next/packages/integrations/cloudflare/CHANGELOG.md)
* [`@astrojs/netlify` CHANGELOG](https://github.com/withastro/astro/blob/next/packages/integrations/netlify/CHANGELOG.md)
* [`@astrojs/node` CHANGELOG](https://github.com/withastro/astro/blob/next/packages/integrations/node/CHANGELOG.md)
* [`@astrojs/vercel` CHANGELOG](https://github.com/withastro/astro/blob/next/packages/integrations/vercel/CHANGELOG.md)

## Legacy

[Section titled “Legacy”](#legacy)

The following features are now considered legacy features. They should function normally but are no longer recommended and are in maintenance mode. They will see no future improvements and documentation will not be updated. These features will eventually be deprecated, and then removed entirely.

### Legacy: content collections backwards compatibility

[Section titled “Legacy: content collections backwards compatibility”](#legacy-content-collections-backwards-compatibility)

In Astro 5.x, projects could delay upgrading to the new Content Layer API introduced for content collections because of some existing automatic backwards compatibility that was not previously behind a flag. This meant that it was possible to upgrade from Astro 4 to Astro 5 without updating your content collections, even if you had not enabled the `legacy.collections` flag. Projects would continue to build, and no errors or warnings would be displayed.

Astro v6.0 removes this automatic legacy content collections support, along with [the `legacy.collections` flag](#removed-legacy-content-collections). All content collections must now use [the Content Layer API introduced in Astro v5.0](https://astro.build/blog/content-layer-deep-dive/) that powers all content collections.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-6)

If you experience content collections errors after updating to v6, [check your project for any removed legacy features](#if-you-have) that may need updating to the Content Layer API.

See [the Astro v5 upgrade guide](/en/guides/upgrade-to/v5/#legacy-v20-content-collections-api) for detailed instructions on upgrading legacy collections to the new Content Layer API.

If you are unable to update immediately, you can enable [the `legacy.collectionsBackwardsCompat` flag](/en/reference/legacy-flags/#collectionsbackwardscompat) as a temporary migration helper:

astro.config.mjs

```js
export default defineConfig({
  legacy: {
    collectionsBackwardsCompat: true,
  },
});
```

This flag preserves some legacy v4 content collections features:

* Supports the legacy configuration file `src/content/config.ts`
* Supports `type: 'content'` and `type: 'data'` without loaders
* Preserves legacy entry API: `entry.slug` and `entry.render()`
* Uses path-based entry IDs instead of slug-based IDs

**This is a temporary migration helper.** Migrate your collections to the Content Layer API as soon as possible, then disable this flag.

## Deprecated

[Section titled “Deprecated”](#deprecated)

The following deprecated features are no longer supported and are no longer documented. Please update your project accordingly.

Some deprecated features may temporarily continue to function until they are completely removed. Others may silently have no effect, or throw an error prompting you to update your code.

### Deprecated: `Astro` in `getStaticPaths()`

[Section titled “Deprecated: Astro in getStaticPaths()”](#deprecated-astro-in-getstaticpaths)

[Implementation PR: feat: deprecate Astro in getStaticPaths (#14432)](https://github.com/withastro/astro/pull/14432)

In Astro 5.x, it was possible to access an `Astro` object inside `getStaticPaths()`. However, despite being typed the same as the `Astro` object accessible in the frontmatter, this object only had `site` and `generator` properties. This could lead to confusion about which `Astro` object properties were available inside `getStaticPaths()`.

Astro 6.0 deprecates this object for `getStaticPaths()` to avoid confusion and improves error handling when attempting to access `Astro` values that are unavailable. Using `Astro.site` or `Astro.generator` within `getStaticPaths()` will now log a deprecation warning, and accessing any other property will throw a specific error with a helpful message. In a future major version, this object will be removed entirely, and accessing `Astro.site` or `Astro.generator` will also throw an error.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-7)

Update your `getStaticPaths()` function if you were attempting to access any `Astro` properties inside its scope. Remove `Astro.generator` entirely, and replace all occurrences of `Astro.site` with `import.meta.env.SITE`:

src/pages/blog/\[slug].astro

```diff
---
import { getPages } from "../../../utils/data";


export async function getStaticPaths() {
-  console.log(Astro.generator);
  -return getPages(Astro.site);
  +return getPages(import.meta.env.SITE);
}
---
```

Read more about [built-in environment variables such as `import.meta.env.SITE`](/en/guides/environment-variables/#default-environment-variables) that are accessible when [using `getStaticPaths()` to dynamically generate static routes](/en/guides/routing/#static-ssg-mode).

### Deprecated: `import.meta.env.ASSETS_PREFIX`

[Section titled “Deprecated: import.meta.env.ASSETS\_PREFIX”](#deprecated-importmetaenvassets_prefix)

[Implementation PR: feat: deprecate import.meta.env.ASSETS\_PREFIX (#14461)](https://github.com/withastro/astro/pull/14461)

In Astro 5.x, it was possible to access `build.assetsPrefix` in your Astro config via the built-in environment variable `import.meta.env.ASSETS_PREFIX`. However, Astro v5.7.0 introduced the `astro:config` virtual module to expose a non-exhaustive, serializable, type-safe version of the Astro configuration which included access to `build.assetsPrefix` directly. This became the preferred way to access the prefix for Astro-generated asset links when set, although the environment variable still existed.

Astro 6.0 deprecates this variable in favor of `build.assetsPrefix` from the `astro:config/server` module.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-8)

Replace any occurrences of `import.meta.env.ASSETS_PREFIX` with the `build.assetsPrefix` import from `astro:config/server`. This is a drop-in replacement to provide the existing value, and no other changes to your code should be necessary:

```diff
import { someLogic } from "./utils"
+import { build } from "astro:config/server"


-someLogic(import.meta.env.ASSETS_PREFIX)
+someLogic(build.assetsPrefix)
```

Read more about the [`astro:config` virtual module](/en/reference/modules/astro-config/).

### Deprecated: `astro:schema` and `z` from `astro:content`

[Section titled “Deprecated: astro:schema and z from astro:content”](#deprecated-astroschema-and-z-from-astrocontent)

[Implementation PR: feat!: consolidate zod export (#14923)](https://github.com/withastro/astro/pull/14923)

In Astro 5.x, `astro:schema` was introduced as an alias of `astro/zod`. `z` was also exported from `astro:content` for convenience. However this occasionally created confusion for users who were unsure about where they should be importing from.

Astro 6.0 deprecates `astro:schema` and `z` from `astro:content` in favor of `astro/zod`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-9)

Replace any occurrences of `astro:schema` with `astro/zod`:

```diff
-import { z } from "astro:schema"
+import { z } from "astro/zod"
```

Remove `z` from your `astro:content` imports and import `z` separately from `astro/zod` instead:

src/content.config.ts

```diff
-import { defineCollection, z } from "astro:content"
+import { defineCollection } from "astro:content"
+import { z } from "astro/zod"
```

See more about [defining collection schemas with Zod](/en/guides/content-collections/#defining-datatypes-with-zod).

### Deprecated: exposed `astro:transitions` internals

[Section titled “Deprecated: exposed astro:transitions internals”](#deprecated-exposed-astrotransitions-internals)

[Implementation PR: feat!: deprecate transitions exports (#14989)](https://github.com/withastro/astro/pull/14989)

In Astro 5.x, some internals were exported from `astro:transitions` and `astro:transitions/client` that were not meant to be exposed for public use.

Astro 6.0 removes the following functions and types as exports from the `astro:transitions` and `astro:transitions/client` virtual modules. These can no longer be imported in your project files:

* `createAnimationScope()`
* `isTransitionBeforePreparationEvent()`
* `isTransitionBeforeSwapEvent()`
* `TRANSITION_BEFORE_PREPARATION`
* `TRANSITION_AFTER_PREPARATION`
* `TRANSITION_BEFORE_SWAP`
* `TRANSITION_AFTER_SWAP`
* `TRANSITION_PAGE_LOAD`

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-10)

Remove any occurrences of `createAnimationScope()`:

```diff
-import { createAnimationScope } from 'astro:transitions';
```

Update any occurrences of the other deprecated exports:

```diff
-import {
-  isTransitionBeforePreparationEvent,
-  TRANSITION_AFTER_SWAP,
-} from 'astro:transitions/client';


-console.log(isTransitionBeforePreparationEvent(event));
+console.log(event.type === 'astro:before-preparation');


-console.log(TRANSITION_AFTER_SWAP);
+console.log('astro:after-swap');
```

Learn more about all utilities available in the [View Transitions Router API Reference](/en/reference/modules/astro-transitions/).

### Deprecated: session driver string signature

[Section titled “Deprecated: session driver string signature”](#deprecated-session-driver-string-signature)

[Implementation PR: feat(sessions): drivers (#15006)](https://github.com/withastro/astro/pull/15006)

In Astro 5.x, any [unstorage provider](https://unstorage.unjs.io/drivers) name or a custom entrypoint could be provided to define a session driver, and options were also provided directly to the `session` configuration. However, we felt that this API was limited and inconsistent with other parts of the Astro config.

Astro 6.0 deprecates the driver string signature and options in favor of a new object shape.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-11)

Update your session config to use the newly exported `sessionDrivers`:

astro.config.mjs

```diff
-import { defineConfig } from 'astro/config'
+import { defineConfig, sessionDrivers } from 'astro/config'


export default defineConfig({
  session: {
-    driver: 'redis',
-    options: {
-      url: process.env.REDIS_URL
-    },
+    driver: sessionDrivers.redis({
+      url: process.env.REDIS_URL
+    }),
    cookie: {
      secure: true
    },
    ttl: 3600
  }
})
```

Learn more about [available session drivers](/en/reference/session-driver-reference/#building-a-session-driver).

### Deprecated: `NodeApp` from `astro/app/node` (Adapter API)

[Section titled “Deprecated: NodeApp from astro/app/node (Adapter API)”](#deprecated-nodeapp-from-astroappnode-adapter-api)

[Implementation PR: feat: deprecate NodeApp (#15535)](https://github.com/withastro/astro/pull/15535)

In Astro 5.x, adapters could implement their server entrypoint using `App` for standard web requests/responses, or `NodeApp` for node requests/responses.

Astro 6.0 deprecates `NodeApp` in favor of `createApp()` and new utilities: `createRequest()` and `writeResponse()`. This allows a more consistent API while preserving the same features as before. It also deprecates the `NodeAppHeadersJson` type.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-12)

If you have built an adapter, update any usage of `NodeApp` with `createApp()`:

my-adapter/server.js

```diff
-import { NodeApp } from 'astro/app/node';


-export function createExports(manifest) {
  -const app = new NodeApp(manifest);


  -const handler = async (req, res) => {
    const response = await app.render(req);
    await NodeApp.writeResponse(response, res);
  };


  -return { handler };
-}
+import { createApp } from 'astro/app/entrypoint';
+import { createRequest, writeResponse } from 'astro/app/node';


+const app = createApp();


+export const handler = async (req, res) => {
  const request = createRequest(req);
  const response = await app.render(request);
  await writeResponse(response, res);
+}
```

Learn more about [the `astro/app/node` module](/en/reference/modules/astro-app/#imports-from-astroappnode).

### Deprecated: `loadManifest()` and `loadApp()` from `astro/app/node` (Adapter API)

[Section titled “Deprecated: loadManifest() and loadApp() from astro/app/node (Adapter API)”](#deprecated-loadmanifest-and-loadapp-from-astroappnode-adapter-api)

[Implementation PR: feat: deprecate NodeApp (#15535)](https://github.com/withastro/astro/pull/15535)

In Astro 5.x, the `astro/app/node` exposed `loadManifest()` and `loadApp()` utilities to allow loading the SSR manifest or a `NodeApp` instance from a `URL` instance. However, these were not documented and are no longer recommended usage with the v6 Adapter API.

Astro 6.0 deprecates both functions.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-13)

If you have built an adapter, remove `loadManifest()` and replace `loadApp()` by `createApp()`:

my-adapter/server.js

```diff
-import { loadManifest, loadApp, NodeApp } from 'astro/app/node';


-const manifest = await loadManifest(new URL(import.meta.url));
-const app1 = new NodeApp(loadManifest);
-const app2 = await loadApp(new URL(import.meta.url));
+import { createApp } from 'astro/app/entrypoint';


+const app = createApp();
```

Learn more about [the `astro/app/entrypoint` module](/en/reference/modules/astro-app/#imports-from-astroappentrypoint).

### Deprecated: `createExports()` and `start()` (Adapter API)

[Section titled “Deprecated: createExports() and start() (Adapter API)”](#deprecated-createexports-and-start-adapter-api)

[Implementation PR: feat: improve naming of new adapter api (#15461)](https://github.com/withastro/astro/pull/15461)

In Astro 5.x, adapters had to provide the exports required by the host in their server entrypoint using a `createExports()` function before passing them to `setAdapter()` as an `exports` list.

Astro 6.0 introduces a simpler yet more powerful way of making server entrypoints. This relies on passing a new option `entrypointResolution: "auto"` to `setAdapter()`.

However, for backwards compatibility with existing adapters, the default value of `entrypointResolution` (`"explicit"`) mimics Astro 5.x API behavior. This means that your adapters can continue to function until you can fully migrate your adapter to the `auto` value, as shown below.

Note that `entrypointResolution: "explicit"` (maintaining v5 API behavior) is considered deprecated usage, but the option has been provided so that no immediate change to your adapter is required and to allow adapter authors time to update. This option will be removed in a future major version in favor of all adapters using `entrypointResolution: "auto"`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-14)

If you are an adapter author with a public repository and [include the `astro-adapter` keyword in your `package.json`](/en/guides/integrations/#categories), the Astro core team will attempt to make a PR to your repository directly to help you migrate your code if you have not yet followed the steps below.

If you are seeing warnings because you are using a community adapter that is not yet updated, please reach out to the adapter author directly to let them know. It is ultimately their responsibility to update their adapters. You can also let the Astro core team know in the [`#integrations` channel of our Discord](https://astro.build/chat) and we will attempt to help the adapter author upgrade.

If you have built an adapter, follow these steps to remove the legacy v5 behavior:

1. Update your `setAdapter()`: set `entrypointResolution: "auto"`, remove `exports` and `args`

   my-adapter.mjs

   ```diff
   setAdapter({
     // ...
   +  entrypointResolution: 'auto',
   -  exports: ['handler'],
   -  args: { assets: config.build.assets }
   })
   ```

2. Update your server entrypoint to provide any required exports without `createExports()`:

   my-adapter/server.js

   ```diff
   -import { App } from 'astro/app';


   -export function createExports(manifest) {
     -const app = new App(manifest);


     -const handler = (event, context) => {
       -// ...
     };


     -return { handler };
   -}
   +import { createApp } from 'astro/app/entrypoint';


   +const app = createApp();


   +export const handler = (event, context) => {
     +// ...
   +}
   ```

3. If your adapter provides a `start()` function, update your server entrypoint to call the code directly:

   my-adapter/server.js

   ```diff
   -import { App } from 'astro/app';


   -export function start(manifest) {
     -const app = new App(manifest);


     -addEventListener('fetch', event => {
       -// ...
   -  });
   -}
   +import { createApp } from 'astro/app/entrypoint';


   +const app = createApp();


   +addEventListener('fetch', event => {
     +// ...
   +});
   ```

4. If you were relying on `args`, [create a virtual module to pass the build time configuration](/en/reference/adapter-reference/#passing-build-time-configuration) and import them from the virtual module instead:

   my-adapter/server.js

   ```diff
   -export function createExports(manifest, { assets }) {
     -// ...
   -}
   +import { assets } from 'virtual:@example/my-adapter:config';
   ```

Learn more about [the Adapter API](/en/reference/adapter-reference/).

## Removed

[Section titled “Removed”](#removed)

The following features have now been entirely removed from the code base and can no longer be used. Some of these features may have continued to work in your project even after deprecation. Others may have silently had no effect.

Projects now containing these removed features will be unable to build, and there will no longer be any supporting documentation prompting you to remove these features.

### Removed: legacy content collections

[Section titled “Removed: legacy content collections”](#removed-legacy-content-collections)

[Implementation PR: fix: remove legacy content collections (#14407)](https://github.com/withastro/astro/pull/14407)

In Astro 5.x, it was still possible to use [the original Content Collections API first introduced in Astro v2.0](https://astro.build/blog/introducing-content-collections/), **either through a `legacy` configuration flag or via built-in backwards compatibility**. These methods allowed you to upgrade to Astro v5 even if you were not yet ready or able to update your existing content collections to those powered by the new Content Layer API.

Astro v6.0 removes this previously deprecated Content Collections API support entirely, including the `legacy.collections` flag **and some existing backwards compatibility that was not previously behind a flag**. All content collections must now use [the Content Layer API introduced in Astro v5.0](https://astro.build/blog/content-layer-deep-dive/) that powers all content collections. **No backwards compatibility support is available.**

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-15)

If you had previously enabled the legacy flag, you must remove it.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  legacy: {
-    collections: true,
  }
})
```

Additionally, if you did not upgrade your collections for Astro v5.0, ensure that your content collections are **fully updated** for the new API.

Astro v5.x included some automatic backwards compatibility to allow content collections to continue to work even if they had not been updated to use the new API. Therefore, your v5 collections may contain one or more legacy features that need updating to the newer API for v6, even if your project was previously error-free.

If you have [content collections errors](/en/reference/error-reference/#content-collection-errors) or warnings after upgrading to v6, use the following list to help you identify and upgrade any legacy features that may exist in your code.

##### If you have…

[Section titled “If you have…”](#if-you-have)

no content collections configuration file

Create `src/content.config.ts` and [define your collections](/en/guides/content-collections/#defining-build-time-content-collections) in it.

a configuration file located at `src/content/config.ts` / ([`LegacyContentConfigError`](/en/reference/errors/legacy-content-config-error/))

Rename and move this file to `src/content.config.ts`

a collection that does not define a `loader` / ([`ContentCollectionMissingALoaderError`](/en/reference/errors/content-collection-missing-loader/))

Import [Astro’s built-in `glob()` loader](/en/guides/content-collections/#the-glob-loader) and define the `pattern` and `base` for your collection entries:

src/content.config.ts

```diff
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';
+import { glob } from 'astro/loaders';


const blog = defineCollection({
  loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: "./src/data/blog" }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
  }),
});
```

a collection that defines a collection type (`type: 'content'` or `type: 'data'`) / ([`ContentCollectionInvalidTypeError`](/en/reference/errors/content-collection-invalid-type/))

There are no longer different types of collections. This must be deleted from your collection definition.

src/content.config.ts

```diff
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';
import { glob } from 'astro/loaders';


const blog = defineCollection({
  // For content layer you no longer define a `type`
  type: 'content',
  loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: "./src/data/blog" }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
  }),
});
```

legacy collection querying methods `getDataEntryById()` and `getEntryBySlug()` / ([`GetEntryDeprecationError`](/en/reference/errors/get-entry-deprecation-error/))

Replace both methods with [`getEntry()`](/en/reference/modules/astro-content/#getentry).

legacy collection querying and rendering methods that depend on a `slug` property / ([`ContentSchemaContainsSlugError`](/en/reference/errors/content-schema-contains-slug-error/))

Previously, the `id` was based on the filename, and there was a `slug` property that could be used in a URL. Now the [`CollectionEntry`](/en/reference/modules/astro-content/#collectionentry) `id` is a slug. If you need access to the filename (previously available as the `id`), use the `filePath` property. Replace instances of `slug` with `id`:

src/pages/\[slug].astro

```diff
---
export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map((post) => ({
-    params: { slug: post.slug },
+    params: { slug: post.id },
    props: post,
  }));
}
---
```

content rendered using `entry.render()`

Collection entries no longer have a `render()` method. Instead, import the `render()` function from `astro:content` and use `render(entry)`:

src/pages/index.astro

```diff
---
import { getEntry, render } from 'astro:content';


const post = await getEntry('pages', 'homepage');


-const { Content, headings } = await post.render();
+const { Content, headings } = await render(post);
---
<Content />
```

See [the Astro v5 upgrade guide](/en/guides/upgrade-to/v5/#legacy-v20-content-collections-api) for previous guidance about backwards compatibility of legacy collections in Astro v5 and full step-by-step instructions for upgrading legacy collections to the new Content Layer API.

### Removed: `<ViewTransitions />` component

[Section titled “Removed: \<ViewTransitions /> component”](#removed-viewtransitions--component)

[Implementation PR: Remove deprecated ViewTransitions component (#14400)](https://github.com/withastro/astro/pull/14400)

In Astro 5.0, the `<ViewTransitions />` component was renamed to `<ClientRouter />` to clarify the role of the component. The new name makes it more clear that the features you get from Astro’s `<ClientRouter />` routing component are slightly different from the native CSS-based MPA router. However, a deprecated version of the `<ViewTransitions />` component still existed and may have functioned in Astro 5.x.

Astro 6.0 removes the `<ViewTransitions />` component entirely and it can no longer be used in your project. Update to the `<ClientRouter />` component to continue to use these features.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-16)

Replace all occurrences of the `ViewTransitions` import and component with `ClientRouter`:

src/layouts/MyLayout.astro

```diff
-import { ViewTransitions } from 'astro:transitions';
+import { ClientRouter } from 'astro:transitions';


<html>
  <head>
    ...
    -<ViewTransitions />
    +<ClientRouter />
  </head>
</html>
```

Read more about [view transitions and client-side routing in Astro](/en/guides/view-transitions/).

### Removed: `emitESMImage()`

[Section titled “Removed: emitESMImage()”](#removed-emitesmimage)

[Implementation PR: feat!: remove emitESMImage() (#14426)](https://github.com/withastro/astro/pull/14426)

In Astro 5.6.2, the `emitESMImage()` function was deprecated in favor of `emitImageMetadata()`, which removes two deprecated arguments that were not meant to be exposed for public use: `_watchMode` and `experimentalSvgEnabled`.

Astro 6.0 removes `emitESMImage()` entirely. Update to `emitImageMetadata()` to keep your current behavior.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-17)

Replace all occurrences of the `emitESMImage()` with `emitImageMetadata()` and remove unused arguments:

```diff
-import { emitESMImage } from 'astro/assets/utils';
+import { emitImageMetadata } from 'astro/assets/utils';


const imageId = '/images/photo.jpg';
-const result = await emitESMImage(imageId, false, false);
+const result = await emitImageMetadata(imageId);
```

Read more about [`emitImageMetadata()`](/en/reference/modules/astro-assets/#emitimagemetadata).

### Removed: `Astro.glob()`

[Section titled “Removed: Astro.glob()”](#removed-astroglob)

[Implementation PR: feat!: remove Astro.glob (#14421)](https://github.com/withastro/astro/pull/14421)

In Astro 5.0, `Astro.glob()` was deprecated in favor of using `getCollection()` to query your collections, and `import.meta.glob()` to query other source files in your project.

Astro 6.0 removes `Astro.glob()` entirely. Update to `import.meta.glob()` to keep your current behavior.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-18)

Replace all use of `Astro.glob()` with `import.meta.glob()`. Note that `import.meta.glob()` no longer returns a `Promise`, so you may have to update your code accordingly. You should not require any updates to your [glob patterns](/en/guides/imports/#glob-patterns).

src/pages/blog.astro

```diff
---
-const posts = await Astro.glob('./posts/*.md');
+const posts = Object.values(import.meta.glob('./posts/*.md', { eager: true }));
---


{posts.map((post) => <li><a href={post.url}>{post.frontmatter.title}</a></li>)}
```

Where appropriate, consider using [content collections](/en/guides/content-collections/) to organize your content, which has its own newer, more performant querying functions.

You may also wish to consider using glob packages from NPM, such as [`fast-glob`](https://www.npmjs.com/package/fast-glob).

Learn more about [importing files with `import.meta.glob`](/en/guides/imports/#importmetaglob).

### Removed: exposed `astro:actions` internals

[Section titled “Removed: exposed astro:actions internals”](#removed-exposed-astroactions-internals)

[Implementation PR: refactor: cleanup public actions API (#14844)](https://github.com/withastro/astro/pull/14844)

In Astro 5.x, some internals were exported from `astro:actions` that were not meant to be exposed for public use.

Astro 6.0 removes the following functions, classes and types as exports from the `astro:actions` virtual module. These can no longer be imported in your project files:

* `ACTION_ERROR_CODES`
* `ActionInputError`
* `appendForwardSlash`
* `astroCalledServerError`
* `callSafely`
* `deserializeActionResult`
* `formDataToObject`
* `getActionQueryString`
* `serializeActionResult`
* `type Actions`
* `type ActionAccept`
* `type AstroActionContext`
* `type SerializedActionResult`

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-19)

Replace all imports of `serializeActionResult()` and `deserializeActionResult()` with `getActionContext()`. These two methods are now available through `getActionContext()`:

src/middleware.ts

```diff
import { defineMiddleware } from 'astro:middleware';
-import { serializeActionResult, deserializeActionResult } from 'astro:actions';
+import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  const { serializeActionResult, deserializeActionResult } = getActionContext(context);
  // ...
});
```

Remove any occurrences of the other removed exports:

```diff
-import {
-  ACTION_ERROR_CODES,
-  ActionInputError,
-  appendForwardSlash,
-  astroCalledServerError,
-  callSafely,
-  formDataToObject,
-  getActionQueryString,
  -type Actions,
  -type ActionAccept,
  -type AstroActionContext,
  -type SerializedActionResult,
-} from 'astro:actions';
```

Learn more about all utilities available in the [Actions API Reference](/en/reference/modules/astro-actions/).

### Removed: Percent-Encoding in routes

[Section titled “Removed: Percent-Encoding in routes”](#removed-percent-encoding-in-routes)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, it was possible to include a percent-encoded percent sign (`%25`) in filenames.

Astro 6.0 removes support for the characters `%25` in filenames for security reasons. This restriction prevents encoding-based security bypasses where `%25` decodes to `%`, potentially leading to ambiguous or invalid encoding sequences.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-20)

If you have route files with `%25` in the filename, rename them to use a different character:

```diff
-src/pages/test%25file.astro
+src/pages/test-file.astro
```

### Removed: `astro:ssr-manifest` virtual module (Integration API)

[Section titled “Removed: astro:ssr-manifest virtual module (Integration API)”](#removed-astrossr-manifest-virtual-module-integration-api)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, the deprecated `astro:ssr-manifest` virtual module could still be used to access configuration values.

Astro 6.0 removes the `astro:ssr-manifest` virtual module entirely. It is no longer used by integrations or internally by Astro. The manifest is now passed directly through integration hooks and adapter APIs rather than through a virtual module. For build-specific manifest data, use the `astro:build:ssr` integration hook, which receives the manifest as a parameter.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-21)

If your integration or code imports from `astro:ssr-manifest`, use `astro:config/server` instead to access configuration values:

```diff
-import { manifest } from 'astro:ssr-manifest';
+import { srcDir, outDir, root } from 'astro:config/server';
+// Use srcDir, outDir, root, etc. for configuration values
```

Learn more about [the `astro:config` virtual module](/en/reference/modules/astro-config/).

### Removed: `RouteData.generate()` (Adapter API)

[Section titled “Removed: RouteData.generate() (Adapter API)”](#removed-routedatagenerate-adapter-api)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, routes could be generated using the `generate()` method on `RouteData`.

Astro 6.0 removes `RouteData.generate()` because route generation is now handled internally by Astro.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-22)

Remove any calls to `route.generate()` in your code. This method is no longer needed:

```diff
-const generated = route.generate(params);
```

Learn more about [the Adapter API](/en/reference/adapter-reference/).

### Removed: `routes` on `astro:build:done` hook (Integration API)

[Section titled “Removed: routes on astro:build:done hook (Integration API)”](#removed-routes-on-astrobuilddone-hook-integration-api)

[Implementation PR: feat: cleanup integration api (#14446)](https://github.com/withastro/astro/pull/14446)

In Astro 5.0, accessing `routes` on the `astro:build:done` hook was deprecated.

Astro 6.0 removes the `routes` array passed to this hook entirely. Instead, the `astro:routes:resolved` hook should be used.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-23)

Remove any instance of `routes` passed to `astro:build:done` and replace it with the new `astro:routes:resolved` hook. Access `distURL` on the newly exposed `assets` map:

my-integration.mjs

```diff
const integration = () => {
    let routes
    return {
        name: 'my-integration',
        hooks: {
            +'astro:routes:resolved': (params) => {
                +routes = params.routes
            },
            'astro:build:done': ({
                -routes
                +assets
            }) => {
                for (const route of routes) {
                    const distURL = assets.get(route.pattern)
                    if (distURL) {
                        +Object.assign(route, { distURL })
                    }
                }
                console.log(routes)
            }
        }
    }
}
```

Learn more about [the Integration API `astro:routes:resolved` hook](/en/reference/integrations-reference/#astroroutesresolved) for building integrations.

### Removed: `entryPoints` on `astro:build:ssr` hook (Integration API)

[Section titled “Removed: entryPoints on astro:build:ssr hook (Integration API)”](#removed-entrypoints-on-astrobuildssr-hook-integration-api)

[Implementation PR: feat: cleanup integration api (#14446)](https://github.com/withastro/astro/pull/14446)

In Astro 5.0, [`functionPerRoute` was deprecated](/en/guides/upgrade-to/v5/#deprecated-functionperroute-adapter-api). That meant that `entryPoints` on the `astro:build:ssr` hook was always empty.

Astro 6.0 removes the `entryPoints` map passed to this hook entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-24)

Remove any instance of `entryPoints` passed to `astro:build:ssr`:

my-integration.mjs

```diff
const integration = () => {
    return {
        name: 'my-integration',
        hooks: {
            'astro:build:ssr': (params) => {
                -someLogic(params.entryPoints)
            },
        }
    }
}
```

### Removed: old `app.render()` signature (Adapter API)

[Section titled “Removed: old app.render() signature (Adapter API)”](#removed-old-apprender-signature-adapter-api)

[Implementation PR: feat: clean deprecated APIs (#14462)](https://github.com/withastro/astro/pull/14462)

In Astro 4.0, the `app.render()` signature that allowed passing `routeData` and `locals` as optional arguments was deprecated in favor of a single optional `renderOptions` argument.

Astro 6.0 removes this signature entirely. Attempting to pass these separate arguments will now cause an error in your project.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-25)

Review your `app.render()` calls and pass `routeData` and `locals` as properties of an object instead of as multiple independent arguments:

my-adapter/entrypoint.ts

```diff
-app.render(request, routeData, locals)
+app.render(request, { routeData, locals })
```

Learn more about the [Adapter API](/en/reference/adapter-reference/).

### Removed: `app.setManifestData()` (Adapter API)

[Section titled “Removed: app.setManifestData() (Adapter API)”](#removed-appsetmanifestdata-adapter-api)

[Implementation PR: chore(astro)!: remove app.setManifestData() (#14758)](https://github.com/withastro/astro/pull/14758)

In Astro 5.0, the `app.setManifestData()` method was available on `App` and `NodeApp`, but is no longer used nor needed.

Astro 6.0 removes this method entirely.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-26)

Remove any call to `app.setManifestData()`. If you need to update the manifest, create a new `App` instance.

Learn more about the [Adapter API](/en/reference/adapter-reference/).

### Removed: `handleForms` prop for the `<ClientRouter />` component

[Section titled “Removed: handleForms prop for the \<ClientRouter /> component”](#removed-handleforms-prop-for-the-clientrouter--component)

[Implementation PR: feat: clean deprecated APIs (#14462)](https://github.com/withastro/astro/pull/14462)

In Astro 4.0, the `handleForms` prop of the `<ClientRouter />` component was deprecated, as it was no longer necessary to opt in to handling `submit` events for `form` elements. This functionality has been built in by default and the property, if still included in your project, silently had no impact on form submission.

Astro 6.0 removes this prop entirely and it now must be removed to avoid errors in your project.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-27)

Remove the `handleForms` property from your `<ClientRouter />` component if it exists. It has provided no additional functionality, and so removing it should not change any behavior in your project:

src/pages/index.astro

```astro
---
import { ClientRouter } from "astro:transitions";
---
<html>
  <head>
    <ClientRouter handleForms />
  </head>
  <body>
    <!-- stuff here -->
  </body>
</html>
```

Learn more about [transitions with forms](/en/guides/view-transitions/#transitions-with-forms).

### Removed: `prefetch()` `with` option

[Section titled “Removed: prefetch() with option”](#removed-prefetch-with-option)

[Implementation PR: feat: clean deprecated APIs (#14462)](https://github.com/withastro/astro/pull/14462)

In Astro 4.8.4, the `with` option of the programmatic `prefetch()` function was deprecated in favor of a more sensible default behavior that no longer required specifying the priority of prefetching for each page.

Astro 6.0 removes this option entirely and it is no longer possible to configure the priority of prefetching by passing the `with` option. Attempting to do so will now cause errors.

By default, Astro’s prefetching now uses an automatic approach that will always try to use `<link rel="prefetch>` if supported, or will fall back to `fetch()`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-28)

Review your `prefetch()` calls and remove the `with` option if it still exists:

```diff
-prefetch('/about', { with: 'fetch' });
+prefetch('/about');
```

Learn more about [prefetching](/en/guides/prefetch/).

### Removed: `rewrite()` from Actions context

[Section titled “Removed: rewrite() from Actions context”](#removed-rewrite-from-actions-context)

[Implementation PR: feat!: remove rewrite from action context (#14477)](https://github.com/withastro/astro/pull/14477)

In Astro 5.5.6, the `ActionAPIContext.rewrite()` method was deprecated because custom endpoints should be used instead of rewrites.

Astro 6.0 removes the `rewrite()` method from `ActionAPIContext` entirely and it may no longer be used.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-29)

Review your Actions handlers and remove any call to `rewrite()`:

src/actions/index.ts

```diff
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  getGreeting: defineAction({
    input: z.object({
      // ...
    }),
    handler: async (input, context) => {
      -context.rewrite('/')
      // ...
    }
  })
}
```

Learn more about [rewrites](/en/guides/routing/#rewrites).

### Removed: schema function signature (Content Loader API)

[Section titled “Removed: schema function signature (Content Loader API)”](#removed-schema-function-signature-content-loader-api)

[Implementation PR: feat: loader.createSchema() (#14759)](https://github.com/withastro/astro/pull/14759)

In Astro 5.x, a content loader could choose to define a schema as a function instead of defining a Zod schema object for validation. This is useful to dynamically generate the schema based on the configuration options or by introspecting an API.

Astro 6.0 removes this signature and introduces a new `createSchema()` property as a replacement for those who still want to dynamically define a schema in their content loader.

Providing a schema function in the old way will log a warning message that the loader’s schema is being ignored, but otherwise the loader will continue to work as if no schema had been provided. In a future major version, loaders that provide a schema function will throw an error and cannot be used.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-30)

If you are building a content loader and using a function to dynamically return a collection `schema` property, you must remove your existing function and use the new `createSchema()` property to define your schema instead.

For example, you can reproduce Astro’s previous behavior by using `zod-to-ts` directly with `createSchema()` and any previous function logic:

```diff
import type { Loader } from 'astro/loaders'
+import { createTypeAlias, zodToTs } from 'zod-to-ts'
import { getSchemaFromApi } from './utils'


function myLoader() {
  return {
    name: 'my-loader',
    load: async (context) => {
      // ...
    },
    -schema: async () => await getSchemaFromApi(),
    +createSchema: async () => {
      +const schema = await getSchemaFromApi()
      +const identifier = 'Entry'
      +const { node } = zodToTs(schema, identifier)
      +const typeAlias = createTypeAlias(node, identifier)


      +return {
+        schema,
+        types: `export ${typeAlias}`
+      }
+    }
  } satisfies Loader
}
```

Learn more about [`createSchema()`](/en/reference/content-loader-reference/#loadercreateschema) in the Content Loader API reference.

### Removed: session `test` driver

[Section titled “Removed: session test driver”](#removed-session-test-driver)

[Implementation PR: feat(sessions): drivers (#15006)](https://github.com/withastro/astro/pull/15006)

In Astro 5.x, the internal session `test` driver was exported in the Astro config types, but it was not meant to be exposed for public use.

Astro 6.0 removes the session `test` driver as it is no longer used internally to test `context.session`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-31)

It is unlikely that you are using this internal API. If you do, you must remove any usage of the session `test` driver:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config'
-import { createMockStorage } from './utils'


export default defineConfig({
  session: {
-    driver: 'test',
-    options: {
-      mockStorage: createMockStorage()
-    }
  }
})
```

Learn more about the [Session Driver API](/en/reference/session-driver-reference/).

### Removed: support for CommonJS config files

[Section titled “Removed: support for CommonJS config files”](#removed-support-for-commonjs-config-files)

[Implementation PR: Drop cjs config support (#15192)](https://github.com/withastro/astro/pull/15192)

In Astro 5.x, the Astro config file could use any of the following extensions: `.mjs`, `.js`, `.ts`, `.mts`, `.cjs` and `.cts`.

Astro 6.0 removes `.cjs` and `.cts` extensions.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-32)

If you have a `astro.config.cjs` or `astro.config.cts` file, update it to use of the supported extensions: `.mjs`, `.js`, `.ts` or `.mts`.

Learn more about the [Astro config file](/en/guides/configuring-astro/#the-astro-config-file).

### Experimental Flags

[Section titled “Experimental Flags”](#experimental-flags)

Experimental flags allow you to opt in to features while they are in early development. Astro may also use experimental flags to test breaking changes to default behavior. The following experimental flags have been removed in Astro 6.0 and are now stable, or the new default behavior.

Remove these experimental flags from your Astro config if you were previously using them:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  experimental: {
-    csp: true,
-    fonts: true,
-    liveContentCollections: true,
-    preserveScriptOrder: true,
-    staticImportMetaEnv: true,
-    headingIdCompat: true,
-    failOnPrerenderConflict: true
  },
})
```

#### Experimental features now stable:

[Section titled “Experimental features now stable:”](#experimental-features-now-stable)

* `csp` (See the [`security.csp` configuration reference](/en/reference/configuration-reference/#securitycsp) to learn more about Content Security Policy.)
* `fonts` (See the updated [fonts guide](/en/guides/fonts/) to learn more about adding custom fonts to your project.)
* `liveContentCollections` (See the updated [content collections docs](/en/guides/content-collections/) to learn more about live collections.)
* `failOnPrerenderConflict` (See the new [`prerenderConflictBehavior`](/en/reference/configuration-reference/#prerenderconflictbehavior) configuration option.)

#### New default or recommended behavior:

[Section titled “New default or recommended behavior:”](#new-default-or-recommended-behavior)

* `preserveScriptOrder` (See below for breaking changes to [default `<script>` and `<style>` behavior](#changed-script-and-style-tags-are-rendered-in-the-order-they-are-defined).)
* `staticImportMetaEnv` (See below for breaking changes to [`import.meta.env`](#changed-importmetaenv-values-are-always-inlined).)
* `headingIdCompat` (See below for breaking changes to [Markdown heading ID generation](#changed-markdown-heading-id-generation).)

Read about exciting new features and more in [the v6.0 Blog post](https://astro.build/blog/astro-6/).

## Changed Defaults

[Section titled “Changed Defaults”](#changed-defaults)

Some default behavior has changed in Astro v6.0 and your project code may need updating to account for these changes.

In most cases, the only action needed is to review your existing project’s deployment and ensure that it continues to function as you expect, making updates to your code as necessary. In some cases, there may be a configuration setting to allow you to continue to use the previous default behavior.

### Changed: `i18n.routing.redirectToDefaultLocale` default value

[Section titled “Changed: i18n.routing.redirectToDefaultLocale default value”](#changed-i18nroutingredirecttodefaultlocale-default-value)

[Implementation PR: feat(astro)!: update i18n.redirectToDefaultLocale default (#14406)](https://github.com/withastro/astro/pull/14406)

In Astro v5.0, the `i18n.routing.redirectToDefaultLocale` default value was `true`. When combined with the `i18n.routing.prefixDefaultLocale` default value of `false`, the resulting redirects could cause infinite loops.

In Astro v6.0, `i18n.routing.redirectToDefaultLocale` now defaults to `false`. Additionally, it can now only be used if `i18n.routing.prefixDefaultLocale` is set to `true`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-33)

Review your Astro `i18n` config as you may now need to explicitly set values for `redirectToDefaultLocale` and `prefixDefaultLocale` to recreate your project’s previous behavior.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  i18n: {
    routing: {
      prefixDefaultLocale: true,
+      redirectToDefaultLocale: true
    }
  }
})
```

If you are using manual routing, you may also need to update your middleware configuration:

src/middleware.js

```diff
import { middleware } from "astro:i18n"; // Astro's own i18n routing config


export const onRequest = middleware({
  prefixDefaultLocale: false,
  prefixDefaultLocale: true,
  redirectToDefaultLocale: true,
})
```

Learn more about [Internationalization routing](/en/guides/internationalization/#routing).

### Changed: `<script>` and `<style>` tags are rendered in the order they are defined

[Section titled “Changed: \<script> and \<style> tags are rendered in the order they are defined”](#changed-script-and-style-tags-are-rendered-in-the-order-they-are-defined)

[Implementation PR: feat: stabilize experimental preserveScriptOrder option (#14480)](https://github.com/withastro/astro/pull/14480)

In Astro v5.5, the `experimental.preserveScriptOrder` flag was introduced to render multiple `<style>` and `<script>` tags in the same order as they were declared in the source code. Astro 5.x reversed their order in your generated HTML output. This could give unexpected results, for example, CSS styles being overridden by earlier defined style tags when your site was built.

Astro 6.0 removes this experimental flag and makes this the new default behavior in Astro: scripts and styles are now rendered in the order defined in your code.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-34)

If you were previously using this experimental feature, you must [remove this experimental flag from your configuration](#experimental-flags) as it no longer exists.

Review your `<script>` and `<style>` tags to make sure they behave as desired. You may need to reverse their order:

src/components/MyComponent.astro

```diff
<p>I am a component</p>
<style>
  body {
    -background: red;
    +background: yellow;
  }
</style>
<style>
  body {
    -background: yellow;
    +background: red;
  }
</style>
<script>
    -console.log("hello")
    +console.log("world")
</script>
<script>
    -console.log("world!")
    +console.log("hello!")
</script>
```

Read more about [using `script`](/en/guides/client-side-scripts/) and [`style`](/en/guides/styling/) tags.

### Changed: how responsive image styles are emitted

[Section titled “Changed: how responsive image styles are emitted”](#changed-how-responsive-image-styles-are-emitted)

[Implementation PR: support responsive images (#15407)](https://github.com/withastro/astro/pull/15407)

In Astro 5.x, images were computed at runtime and the `fit` and `pos` responsive image styles were injected in a `style` attribute. This did not allow compatibility with Astro’s Content Security Policy (CSP) for many reasons.

Astro 6 generates image styles inside a virtual module at build time based on project configuration, resulting in a hash class and `data-*` attributes to apply responsive styling to your images.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-35)

Visually inspect your images to ensure that they are rendering as expected. This is an implementation detail that should not affect the expected use of responsive images.

However, if you were relying on the inline styles previously generated for your images:

```html
<img style="--fit: <value>; --pos: <value>" >
```

then you will need to update your project code to account for the new `data-*` attributes instead:

```html
<img class="__a_HaSh350" data-astro-fit="value" data-astro-pos="value" >
```

## Breaking Changes

[Section titled “Breaking Changes”](#breaking-changes)

The following changes are considered breaking changes in Astro v6.0. Breaking changes may or may not provide temporary backwards compatibility. If you were using these features, you may have to update your code as recommended in each entry.

### Changed: endpoints with a file extension cannot be accessed with a trailing slash

[Section titled “Changed: endpoints with a file extension cannot be accessed with a trailing slash”](#changed-endpoints-with-a-file-extension-cannot-be-accessed-with-a-trailing-slash)

[Implementation PR: feat!: trailing slash never for endpoints with file extension (#14457)](https://github.com/withastro/astro/pull/14457)

In Astro v5.0, custom endpoints whose URL ended in a file extension (e.g. `/src/pages/sitemap.xml.ts` ) could be accessed with a trailing slash (`/sitemap.xml/`) or without (`/sitemap.xml`), regardless of the value configured for `build.trailingSlash`.

In Astro v6.0, these endpoints can only be accessed without a trailing slash. This is true regardless of your `build.trailingSlash` configuration.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-36)

Review your links to your custom endpoints that include a file extension in the URL and remove any trailing slashes:

src/pages/index.astro

```diff
<a href="/sitemap.xml/">Sitemap</a>
<a href="/sitemap.xml">Sitemap</a>
```

Learn more about [custom endpoints](/en/guides/endpoints/).

### Changed: `import.meta.env` values are always inlined

[Section titled “Changed: import.meta.env values are always inlined”](#changed-importmetaenv-values-are-always-inlined)

[Implementation PR: feat: stabilize static import meta env (#14485)](https://github.com/withastro/astro/pull/14485)

In Astro 5.13, the `experimental.staticImportMetaEnv` flag was introduced to update the behavior when accessing `import.meta.env` directly to align with [Vite’s handling of environment variables](https://vite.dev/guide/env-and-mode.html#env-variables) and ensures that `import.meta.env` values are always inlined.

In Astro 5.x, non-public environment variables were replaced by a reference to `process.env`. Additionally, Astro could also convert the value type of your environment variables used through `import.meta.env`, which could prevent access to some values such as the strings `"true"` (which was converted to a boolean value), and `"1"` (which was converted to a number).

Astro 6 removes this experimental flag and makes this the new default behavior in Astro: `import.meta.env` values are always inlined and never coerced.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-37)

If you were previously using this experimental feature, you must [remove this experimental flag from your configuration](#experimental-flags) as it no longer exists.

If you were relying on coercion, you may need to update your project code to apply it manually:

src/components/MyComponent.astro

```diff
-const enabled: boolean = import.meta.env.ENABLED;
+const enabled: boolean = import.meta.env.ENABLED === "true";
```

If you were relying on the transformation into `process.env`, you may need to update your project code to apply it manually:

src/components/MyComponent.astro

```diff
-const enabled: boolean = import.meta.env.DB_PASSWORD;
+const enabled: boolean = process.env.DB_PASSWORD;
```

You may also need to update types:

src/env.d.ts

```diff
interface ImportMetaEnv {
  readonly PUBLIC_POKEAPI: string;
  -readonly DB_PASSWORD: string;
  -readonly ENABLED: boolean;
  +readonly ENABLED: string;
}


interface ImportMeta {
  readonly env: ImportMetaEnv;
}


+namespace NodeJS {
  +interface ProcessEnv {
+    DB_PASSWORD: string;
+  }
+}
```

If you need more control over environment variables in Astro, we recommend you use `astro:env`.

Learn more about [environment variables](/en/guides/environment-variables/) in Astro, including `astro:env`.

### Changed: Cropping by default in default image service

[Section titled “Changed: Cropping by default in default image service”](#changed-cropping-by-default-in-default-image-service)

[Implementation PR: feat(assets): Always allow cropping and never upscale (#14629)](https://github.com/withastro/astro/pull/14629)

In Astro 5.0, the default image service would only apply cropping when the `fit` option was provided.

Astro 6.0 applies cropping by default without requiring setting the `fit` option.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-38)

No changes are needed to your existing cropped images as the `fit` property is still valid. However, if you were previously setting `fit` to `contain` (its default value) in order to crop your images, you may now remove this option and still achieve the same cropping behavior by specifying `width` and `height` alone:

src/components/MyImage.astro

```diff
---
import { Image } from 'astro:assets';
import myImage from '../assets/photo.jpg';
---
-<Image src={myImage} width={400} height={300} fit="contain" />
+<Image src={myImage} width={400} height={300} />
```

### Changed: Never upscale images in default image service

[Section titled “Changed: Never upscale images in default image service”](#changed-never-upscale-images-in-default-image-service)

[Implementation PR: feat(assets): Always allow cropping and never upscale (#14629)](https://github.com/withastro/astro/pull/14629)

In Astro 5.x, the default image service would upscale images when the requested dimensions were larger than the source image.

Astro 6.0 removes this behavior: the default image service never upscales images.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-39)

Review your images and update dimensions as needed. If you do need to upscale images, you may consider upscaling the images manually or using a custom image service that supports upscaling.

### Changed: SVG rasterization

[Section titled “Changed: SVG rasterization”](#changed-svg-rasterization)

[Implementation PR: add support for SVG rasterization (#15180)](https://github.com/withastro/astro/pull/15180)

In Astro v5.x, Astro’s default Sharp image service was unable to convert SVG files to raster files (e.g. PNG, WebP). This meant that the `<Image />` component would ignore any value set for `format` when optimizing and transforming SVG files.

Astro 6.0 now supports SVG rasterization. This is subject to [many limitations](https://github.com/lovell/sharp/issues?q=is%3Aissue%20state%3Aopen%20svg), for instance, SVGs with embedded fonts might not be converted properly. However, when the `format` property is set, the image service will now attempt to convert SVG images.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-40)

If you were previously relying on the fact that the image service would automatically skip converting SVGs, you must now check the format of your images beforehand to avoid converting SVGs to raster images:

```diff
-<Image src={imageThatMightBeAnSvg} format="avif" alt="example" />


+<Image
  +src={imageThatMightBeAnSvg}
  +format={imageThatMightBeAnSvg.format === "svg" ? "svg" : "avif"}
  +alt="example"
+/>
```

Learn more about [the `format` image property](/en/reference/modules/astro-assets/#format).

### Changed: `getImage()` throws when called on the client

[Section titled “Changed: getImage() throws when called on the client”](#changed-getimage-throws-when-called-on-the-client)

[Implementation PR: feat: disallow getImage on the client (#15800)](https://github.com/withastro/astro/pull/15800)

In Astro 5.x, calling `getImage()` from `astro:assets` on the client would silently fail or produce incorrect results.

Astro 6.0 throws a runtime error when `getImage()` is called on the client.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-41)

Call `getImage()` on the server and pass the resulting `src` to the client instead:

src/components/ClientImage.astro

```astro
---
import { getImage } from "astro:assets";
import myBackground from "../background.png";


const optimizedBackground = await getImage({ src: myBackground, format: "avif" });
---


<div id="background" data-src={optimizedBackground.src}></div>


<script>
  const src = document.getElementById("background").dataset.src;
  // use src client-side as needed
</script>
```

See [generating images with `getImage()`](/en/guides/images/#generating-images-with-getimage) for a full example.

### Changed: Markdown heading ID generation

[Section titled “Changed: Markdown heading ID generation”](#changed-markdown-heading-id-generation)

[Implementation PR: feat!: stabilize experimental.headingIdCompat (#14494)](https://github.com/withastro/astro/pull/14494)

In Astro 5.x, an additional default processing step to Markdown stripped trailing hyphens from the end of IDs for section headings ending in special characters. This provided a cleaner `id` value, but could lead to incompatibilities rendering your Markdown across platforms.

In Astro 5.5, the `experimental.headingIdCompat` flag was introduced to allow you to make the IDs generated by Astro for Markdown headings compatible with common platforms like GitHub and npm, using the popular [`github-slugger`](https://github.com/Flet/github-slugger) package.

Astro 6.0 removes this experimental flag and makes this the new default behavior in Astro: trailing hyphens from the end of IDs for headings ending in special characters are no longer removed.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-42)

If you have manual links to headings, you may need to update the anchor link value with a new trailing hyphen. For example, the following Markdown heading:

```md
## `<Picture />`
```

will now generate the following HTML with a trailing hyphen in the heading `id`:

```html
<h2 id="picture-"><code>&lt;Picture /&gt;</code></h2>
```

and must now be linked to as:

```markdown
See [the Picture component](/en/guides/images/#picture-) for more details.
```

If you were previously using the experimental feature to enforce trailing hyphens, you must [remove this experimental flag from your configuration](#experimental-flags) as it no longer exists.

If you were previously using the `rehypeHeadingIds` plugin directly to enforce compatibility, remove the `headingIdCompat` option as it no longer exists:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
import { rehypeHeadingIds } from '@astrojs/markdown-remark';
import { otherPluginThatReliesOnHeadingIDs } from 'some/plugin/source';


export default defineConfig({
  markdown: {
    rehypePlugins: [
-      [rehypeHeadingIds, { headingIdCompat: true }],
+      [rehypeHeadingIds],
      otherPluginThatReliesOnHeadingIDs,
    ],
  },
});
```

If you want to keep the old ID generation for backward compatibility reasons, you can create a custom rehype plugin that will generate headings IDs like Astro 5.x. This will allow you to continue to use your existing anchor links without adding trailing hyphens.

Create a custom rehype plugin to strip trailing hyphens

1. Install required dependencies:

   * npm

     ```sh
     npm i github-slugger hast-util-heading-rank unist-util-visit hast-util-to-string
     ```

   * pnpm

     ```sh
     pnpm add github-slugger hast-util-heading-rank unist-util-visit hast-util-to-string
     ```

   * Yarn

     ```sh
     yarn add github-slugger hast-util-heading-rank unist-util-visit hast-util-to-string
     ```

2. Create a custom rehype plugin that will generate headings IDs like Astro v5:

   plugins/rehype-slug.mjs

   ```js
   import GithubSlugger from 'github-slugger';
   import { headingRank } from 'hast-util-heading-rank';
   import { visit } from 'unist-util-visit';
   import { toString } from 'hast-util-to-string';


   const slugs = new GithubSlugger();


   export function rehypeSlug() {
     /**
      * @param {import('hast').Root} tree
      */
     return (tree) => {
       slugs.reset();
       visit(tree, 'element', (node) => {
         if (headingRank(node) && !node.properties.id) {
           let slug = slugs.slug(toString(node));
           // Strip trailing hyphens like in Astro v5 and below:
           if (slug.endsWith('-')) slug = slug.slice(0, -1);
           node.properties.id = slug;
         }
       });
     };
   }
   ```

3. Add the custom plugin to your Markdown configuration in `astro.config.mjs`:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import { rehypeSlug } from './plugins/rehype-slug';


   export default defineConfig({
     markdown: {
       rehypePlugins: [rehypeSlug],
     },
   });
   ```

Learn more about [Heading IDs](/en/guides/markdown-content/#heading-ids).

### Changed: `getStaticPaths()` cannot return `params` of type number

[Section titled “Changed: getStaticPaths() cannot return params of type number”](#changed-getstaticpaths-cannot-return-params-of-type-number)

[Implementation PR: fix!: disallow number in getStaticPaths params (#14586)](https://github.com/withastro/astro/pull/14586)

In Astro 5.x, `getStaticPaths()` could return `params` of type number, which would always be stringified by Astro. However, that could be confusing because it conflicted with `Astro.params` types.

Astro 6.0 removes this behavior: `getStaticPaths()` must now return string or undefined `params` values.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-43)

Review your dynamic routes using `getStaticPaths()` and convert any number params to strings:

src/pages/post/\[id]/\[label].astro

```diff
---
export function getStaticPaths() {
  return [
    {
      params: {
-        id: 1,
+        id: "1",
        label: "foo",
      }
    },
    {
      params: {
-        id: 2,
+        id: "2",
        label: "bar",
      }
    },
  ]
}
---
```

Learn more about [dynamic SSG routes with `getStaticPaths()`](/en/guides/routing/#static-ssg-mode).

### Changed: Astro components cannot be rendered in Vitest client environments (Container API)

[Section titled “Changed: Astro components cannot be rendered in Vitest client environments (Container API)”](#changed-astro-components-cannot-be-rendered-in-vitest-client-environments-container-api)

[Implementation PR: feat: remove Vitest workaround for client environment (#14895)](https://github.com/withastro/astro/pull/14895)

In Astro 5.x, rendering an Astro component on the client was forbidden. However we temporarily allowed this behavior in Vitest client environments such as `jsdom` or `happy-dom` using the [experimental Container API](/en/reference/container-reference/).

Astro 6.0 removes the ability to render Astro components in Vitest client environments: tests that render Astro components must now run in a server environment like `node`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-44)

If you use Vitest to run tests that render Astro components in client environments like `jsdom` or `happy-dom`, update your Vitest config to use the `node` environment for these:

vitest.config.ts

```diff
import { defineConfig } from 'vitest/config';


export default defineConfig({
  test: {
-    environment: 'jsdom',
+    environment: 'node',
  },
});
```

Learn more about [testing Astro components](/en/guides/testing/).

### Changed: Rollup output file name config path (Vite config)

[Section titled “Changed: Rollup output file name config path (Vite config)”](#changed-rollup-output-file-name-config-path-vite-config)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, custom Rollup output file name options for client assets could be configured at `vite.build.rollupOptions.output`.

Astro 6.0 scopes client build output configuration to Vite’s client environment. If you customize `entryFileNames`, `chunkFileNames`, or `assetFileNames` for client assets, use `vite.environments.client.build.rollupOptions.output`.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-45)

Move your config from `vite.build.rollupOptions.output` to `vite.environments.client.build.rollupOptions.output`:

astro.config.mjs

```js
export default defineConfig({
  vite: {
    environments: {
      client: {
        build: {
          rollupOptions: {
            output: {
              entryFileNames: 'js/[name]-[hash].js',
            },
          },
        },
      },
    },
  },
});
```

### Changed: Integration hooks and HMR access patterns (Integration API)

[Section titled “Changed: Integration hooks and HMR access patterns (Integration API)”](#changed-integration-hooks-and-hmr-access-patterns-integration-api)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, Astro relied on certain patterns for integration hooks and HMR access that were incompatible with or could be improved by integrating Vite’s Environment API.

Astro 6.0 uses Vite’s new Environment API for build configuration and dev server interactions. This primarily enables dev mode in runtimes like workerd, but means that some integration hooks and HMR access patterns have changed.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-46)

**For integrations using `astro:build:setup`:**

The hook is now called once with all environments configured (`ssr`, `client`, `prerender`), instead of being called separately for each build target. Remove the `target` parameter and use `vite.environments` to configure specific environments:

my-integration.mjs

```diff
{
  hooks: {
    -'astro:build:setup': ({ target, vite }) => {
      -if (target === 'client') {
-        vite.build.minify = false;
-      }
-    }
    +'astro:build:setup': ({ vite }) => {
+      vite.environments.client.build.minify = false;
+    }
  }
}
```

**For dev toolbar and integration code accessing HMR:**

Replace `server.hot.send()` with `server.environments.client.hot.send()`:

```diff
-server.hot.send(event)
+server.environments.client.hot.send(event)
```

Learn more about the [Vite Environment API](https://vite.dev/guide/api-environment) and Astro [integration hooks](/en/reference/integrations-reference/#astrobuildsetup).

### Changed: `SSRManifest` interface structure (Adapter API)

[Section titled “Changed: SSRManifest interface structure (Adapter API)”](#changed-ssrmanifest-interface-structure-adapter-api)

[Implementation PR: feat: integrate vite environments (#14306)](https://github.com/withastro/astro/pull/14306)

In Astro 5.x, path properties of the `SSRManifest` interface like `srcDir`, `outDir`, `cacheDir`, `publicDir`, `buildClientDir`, and `buildServerDir` were URL strings.

Astro 6.0 changes the form of these path properties to `URL` objects instead of URL strings. With this change, several new properties are now available on the manifest, and others have been updated or removed.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-47)

If you were treating these path properties as strings, you will now need to handle the `URL` object. For example, you will now need to access the `href` property of the `URL` object:

```diff
// To retrieve the same format (e.g., "file:///path/to/src"), make the following change:
-const srcPath = manifest.srcDir;
+const srcPath = manifest.srcDir.href;
```

If you were accessing the `hrefRoot` property, you will need to remove it, as it is no longer available on the manifest.

Update any use of `serverIslandMappings` and `sessionDriver`. These are now async methods:

```diff
-const mappings = manifest.serverIslandMappings;
-const driver = manifest.sessionDriver;
+const mappings = await manifest.serverIslandMappings?.();
+const driver = await manifest.sessionDriver?.();
```

Learn more about [the Adapter API](/en/reference/adapter-reference/).

### Changed: schema types are inferred instead of generated (Content Loader API)

[Section titled “Changed: schema types are inferred instead of generated (Content Loader API)”](#changed-schema-types-are-inferred-instead-of-generated-content-loader-api)

[Implementation PR: feat: loader.createSchema() (#14759)](https://github.com/withastro/astro/pull/14759)

In Astro 5.x, the types for content collections were generated using `zod-to-ts` when provided by a content loader and not defined by a user-provided schema.

Astro 6.0 removes this behavior: types are no longer generated using `zod-to-ts`. Instead, types are inferred.

#### What should I do?

[Section titled “What should I do?”](#what-should-i-do-48)

If you are providing a `schema` in a content loader, you must use the [TypeScript’ `satisfies` operator](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator):

```diff
import type { Loader } from 'astro/loaders'


-function myLoader(): Loader {
+function myLoader() {
  return {
    name: 'my-loader',
    load: async (context) => {
      // ...
    },
    schema: z.object({/* ... */})
-  }
+  } satisfies Loader
}
```

Learn more about [defining loader schema types](/en/reference/content-loader-reference/#the-loader-object).

## Known Issues

[Section titled “Known Issues”](#known-issues)

Please check [Astro’s issues on GitHub](https://github.com/withastro/astro/issues/) for any reported issues, or to file an issue yourself.

