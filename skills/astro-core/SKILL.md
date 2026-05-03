---
name: astro-core
description: Astro framework core concepts and essential guides - installation, project structure, components, pages, layouts, routing, styling, content collections, data fetching, scripts, images, markdown, dev toolbar, and common recipes. Load this for any general Astro development task.
---

# Astro Core Documentation

> This skill contains the essential Astro framework documentation for day-to-day development.


<SYSTEM>This is the full developer documentation for Astro</SYSTEM>

# Why Astro?

> Astro is the web framework for building content-driven websites like blogs, marketing, and e-commerce. Learn why Astro might be a good choice for your next website.

**Astro** is the web framework for building **content-driven websites** like blogs, marketing, and e-commerce. Astro is best-known for pioneering a new [frontend architecture](/en/concepts/islands/) to reduce JavaScript overhead and complexity compared to other frameworks. If you need a website that loads fast and has great SEO, then Astro is for you.

## Features

[Section titled “Features”](#features)

**Astro is an all-in-one web framework.** It includes everything you need to create a website, built-in. There are also hundreds of different [integrations](https://astro.build/integrations/) and [API hooks](/en/reference/integrations-reference/) available to customize a project to your exact use case and needs.

Some highlights include:

* **[Islands](/en/concepts/islands/):** A component-based web architecture optimized for content-driven websites.
* **[UI-agnostic](/en/guides/framework-components/):** Supports React, Preact, Svelte, Vue, Solid, HTMX, web components, and more.
* **[Server-first](/en/guides/on-demand-rendering/):** Moves expensive rendering off of your visitors’ devices.
* **[Zero JS, by default](/en/basics/astro-components/):** Less client-side JavaScript to slow your site down.
* **[Content collections](/en/guides/content-collections/):** Organize, validate, and provide TypeScript type-safety for your Markdown content.
* **[Customizable](/en/guides/integrations/):** Partytown, MDX, and hundreds of integrations to choose from.

## Design Principles

[Section titled “Design Principles”](#design-principles)

Here are five core design principles to help explain why we built Astro, the problems that it exists to solve, and why Astro may be the best choice for your project or team.

Astro is…

1. **[Content-driven](#content-driven):** Astro was designed to showcase your content.
2. **[Server-first](#server-first):** Websites run faster when they render HTML on the server.
3. **[Fast by default](#fast-by-default):** It should be impossible to build a slow website in Astro.
4. **[Easy to use](#easy-to-use):** You don’t need to be an expert to build something with Astro.
5. **[Developer-focused](#developer-focused):** You should have the resources you need to be successful.

### Content-driven

[Section titled “Content-driven”](#content-driven)

**Astro was designed for building content-rich websites.** This includes marketing sites, publishing sites, documentation sites, blogs, portfolios, landing pages, community sites, and e-commerce sites. If you have content to show, it needs to reach your reader quickly.

By contrast, most modern web frameworks were designed for building *web applications*. These frameworks excel at building more complex, application-like experiences in the browser: logged-in admin dashboards, inboxes, social networks, todo lists, and even native-like applications like [Figma](https://figma.com/) and [Ping](https://ping.gg/). However with that complexity, they can struggle to provide great performance when delivering your content.

Astro’s focus on content from its beginnings as a static site builder have allowed Astro to **sensibly scale up to performant, powerful, dynamic web applications** that still respect your content and your audience. Astro’s unique focus on content lets Astro make tradeoffs and deliver unmatched performance features that wouldn’t make sense for more application-focused web frameworks to implement.

### Server-first

[Section titled “Server-first”](#server-first)

**Astro leverages server rendering over client-side rendering in the browser as much as possible.** This is the same approach that traditional server-side frameworks -- PHP, WordPress, Laravel, Ruby on Rails, etc. -- have been using for decades. But you don’t need to learn a second server-side language to unlock it. With Astro, everything is still just HTML, CSS, and JavaScript (or TypeScript, if you prefer).

This approach stands in contrast to other modern JavaScript web frameworks like Next.js, SvelteKit, Nuxt, Remix, and others. These frameworks were built for client-side rendering of your entire website and include server-side rendering mainly to address performance concerns. This approach has been dubbed the **Single-Page App (SPA)**, in contrast with Astro’s **Multi-Page App (MPA)** approach.

The SPA model has its benefits. However, these come at the expense of additional complexity and performance tradeoffs. These tradeoffs harm page performance -- critical metrics like [Time to Interactive (TTI)](https://web.dev/interactive/) -- which doesn’t make much sense for content-focused websites where first-load performance is essential.

Astro’s server-first approach allows you to opt in to client-side rendering only if, and exactly as, necessary. You can choose to add UI framework components that run on the client. You can take advantage of Astro’s view transitions router for finer control over select page transitions and animations. Astro’s server-first rendering, either pre-rendered or on-demand, provides performant defaults that you can enhance and extend.

### Fast by default

[Section titled “Fast by default”](#fast-by-default)

Good performance is always important, but it is *especially* critical for websites whose success depends on displaying your content. It has been well-proven that poor performance loses you engagement, conversions, and money. For example:

* Every 100ms faster → 1% more conversions ([Mobify](https://web.dev/why-speed-matters/), earning +$380,000/yr)
* 50% faster → 12% more sales ([AutoAnything](https://www.digitalcommerce360.com/2010/08/19/web-accelerator-revs-conversion-and-sales-autoanything/))
* 20% faster → 10% more conversions ([Furniture Village](https://www.thinkwithgoogle.com/intl/en-gb/marketing-strategies/app-and-mobile/furniture-village-and-greenlight-slash-page-load-times-boosting-user-experience/))
* 40% faster → 15% more sign-ups ([Pinterest](https://medium.com/pinterest-engineering/driving-user-growth-with-performance-improvements-cfc50dafadd7))
* 850ms faster → 7% more conversions ([COOK](https://web.dev/why-speed-matters/))
* Every 1 second slower → 10% fewer users ([BBC](https://www.creativebloq.com/features/how-the-bbc-builds-websites-that-scale))

In many web frameworks, it is easy to build a website that looks great during development only to load painfully slow once deployed. JavaScript is often the culprit, since many phones and lower-powered devices rarely match the speed of a developer’s laptop.

Astro’s magic is in how it combines the two values explained above -- a content focus with a server-first architecture -- to make tradeoffs and deliver features that other frameworks cannot. The result is amazing web performance for every website, out of the box. Our goal: **It should be nearly impossible to build a slow website with Astro.**

An Astro website can [load 40% faster with 90% less JavaScript](https://twitter.com/t3dotgg/status/1437195415439360003) than the same site built with the most popular React web framework. But don’t take our word for it: watch Astro’s performance leave Ryan Carniato (creator of Solid.js and Marko) [speechless](https://youtu.be/2ZEMb_H-LYE?t=8163).

### Easy to use

[Section titled “Easy to use”](#easy-to-use)

**Astro’s goal is to be accessible to every web developer.** Astro was designed to feel familiar and approachable regardless of skill level or past experience with web development.

The `.astro` UI language is a superset of HTML: any valid HTML is valid Astro templating syntax! So, if you can write HTML, you can write Astro components! But, it also combines some of our favorite features borrowed from other component languages like JSX expressions (React) and CSS scoping by default (Svelte and Vue). This closeness to HTML also makes it easier to use progressive enhancement and common accessibility patterns without any overhead.

We then made sure that you could also use your favorite UI component languages that you already know, and even reuse components you might already have. React, Preact, Svelte, Vue, Solid, and others, including web components, are all supported for authoring UI components in an Astro project.

Astro was designed to be less complex than other UI frameworks and languages. One big reason for this is that Astro was designed to render on the server, not in the browser. That means that you don’t need to worry about hooks (React), stale closures (also React), refs (Vue), observables (Svelte), atoms, selectors, reactions, or derivations. There is no reactivity on the server, so all of that complexity melts away.

One of our favorite sayings is **opt in to complexity.** We designed Astro to remove as much “required complexity” as possible from the developer experience, especially as you onboard for the first time. You can build a “Hello World” example website in Astro with just HTML and CSS. Then, when you need to build something more powerful, you can incrementally reach for new features and APIs as you go.

### Developer-focused

[Section titled “Developer-focused”](#developer-focused)

We strongly believe that Astro is only a successful project if people love using it. Astro has everything you need to support you as you build with Astro.

Astro invests in developer tools like a great CLI experience from the moment you open your terminal, an official VS Code extension for syntax highlighting, TypeScript and Intellisense, and documentation actively maintained by hundreds of community contributors and available in 14 languages.

Our welcoming, respectful, inclusive community on Discord is ready to provide support, motivation, and encouragement. Open a `#support` thread to get help with your project. Visit our dedicated `#showcase` channel for sharing your Astro sites, blog posts, videos, and even work-in-progress for safe feedback and constructive criticism. Participate in regular live events such as our weekly community call, “Talking and Doc’ing,” and API/bug bashes.

As an open-source project, we welcome contributions of all types and sizes from community members of all experience levels. You are invited to join in roadmap discussions to shape the future of Astro, and we hope you’ll contribute fixes and features to the core codebase, compiler, docs, language tools, websites, and other projects.

# Islands architecture

> Learn about how Astro's islands architecture helps keep sites fast.

Astro helped pioneer and popularize a new frontend architecture pattern called **Islands Architecture.** Islands architecture works by rendering the majority of your page to fast, static HTML with smaller “islands” of JavaScript added when interactivity or personalization is needed on the page (an image carousel, for example). This avoids the monolithic JavaScript payloads that slow down the responsiveness of many other, modern JavaScript web frameworks.

## A brief history

[Section titled “A brief history”](#a-brief-history)

The term “component island” was first coined by Etsy’s frontend architect [Katie Sylor-Miller](https://sylormiller.com/) in 2019. This idea was then expanded on and documented in [this post](https://jasonformat.com/islands-architecture/) by Preact creator Jason Miller on August 11, 2020.

> The general idea of an “Islands” architecture is deceptively simple: render HTML pages on the server, and inject placeholders or slots around highly dynamic regions \[…] that can then be “hydrated” on the client into small self-contained widgets, reusing their server-rendered initial HTML.\
> — Jason Miller, Creator of Preact

The technique that this architectural pattern builds on is also known as **partial** or **selective hydration.**

In contrast, most JavaScript-based web frameworks hydrate & render an entire website as one large JavaScript application (also known as a single-page application, or SPA). SPAs provide simplicity and power but suffer from page-load performance problems due to heavy client-side JavaScript usage.

SPAs have their place, even [embedded inside an Astro page](/en/guides/migrate-to-astro/from-create-react-app/). But, SPAs lack the native ability to selectively and strategically hydrate, making them a heavy-handed choice for most projects on the web today.

Astro became popular as the first mainstream JavaScript web framework with selective hydration built-in, using that same component islands pattern first coined by Sylor-Miller. We’ve since expanded and evolved on Sylor-Miller’s original work, which helped to inspire a similar component island approach to dynamically server-rendered content.

## What is an island?

[Section titled “What is an island?”](#what-is-an-island)

In Astro, an island is an enhanced UI component on an otherwise static page of HTML.

A [**client island**](#client-islands) is an interactive JavaScript UI component that is hydrated separately from the rest of the page, while a [**server island**](#server-islands) is a UI component that server-renders its dynamic content separately from the rest of the page.

Both islands run expensive or slower processes independently, on a per-component basis, for optimized page loads.

## Island components

[Section titled “Island components”](#island-components)

Astro components are the building blocks of your page template. They render to static HTML with no client-side runtime.

Think of a client island as an interactive widget floating in a sea of otherwise static, lightweight, server-rendered HTML. Server islands can be added for personalized or dynamic server-rendered elements, such as a logged in visitor’s profile picture.

Header (interactive island)

Sidebar (static HTML)

Static content like text, images, etc.

Image carousel (interactive island)

Footer (static HTML)

Source: [Islands Architecture: Jason Miller](https://jasonformat.com/islands-architecture/)

An island always runs in isolation from other islands on the page, and multiple islands can exist on a page. Client islands can still share state and communicate with each other, even though they run in different component contexts.

This flexibility allows Astro to support multiple UI frameworks like [React](https://react.dev/), [Preact](https://preactjs.com/), [Svelte](https://svelte.dev/), [Vue](https://vuejs.org/), and [SolidJS](https://www.solidjs.com/). Because they are independent, you can even mix several frameworks on each page.

Tip

Although most developers will stick to just one UI framework, Astro supports multiple frameworks in the same project. This allows you to:

* Choose the framework that is best for each component.
* Learn a new framework without needing to start a new project.
* Collaborate with others even when working in different frameworks.
* Incrementally convert an existing site to another framework with no downtime.

## Client Islands

[Section titled “Client Islands”](#client-islands)

By default, Astro will automatically render every UI component to just HTML & CSS, **stripping out all client-side JavaScript automatically.**

src/pages/index.astro

```astro
<MyReactComponent />
```

This may sound strict, but this behavior is what keeps Astro websites fast by default and protects developers from accidentally sending unnecessary or unwanted JavaScript that might slow down their website.

Turning any static UI component into an interactive island requires only a `client:*` directive. Astro then automatically builds and bundles your client-side JavaScript for optimized performance.

src/pages/index.astro

```astro
<!-- This component is now interactive on the page!
     The rest of your website remains static. -->
<MyReactComponent client:load />
```

With islands, client-side JavaScript is only loaded for the explicit interactive components that you mark using `client:*` directives.

And because interaction is configured at the component-level, you can handle different loading priorities for each component based on its usage. For example, `client:idle` tells a component to load when the browser becomes idle, and `client:visible` tells a component to load only once it enters the viewport.

### Benefits of client islands

The most obvious benefit of building with Astro Islands is performance: the majority of your website is converted to fast, static HTML and JavaScript is only loaded for the individual components that need it. JavaScript is one of the slowest assets that you can load per-byte, so every byte counts.

Another benefit is parallel loading. In the example illustration above, the low-priority “image carousel” island doesn’t need to block the high-priority “header” island. The two load in parallel and hydrate in isolation, meaning that the header becomes interactive immediately without having to wait for the heavier carousel lower down the page.

Even better, you can tell Astro exactly how and when to render each component. If that image carousel is really expensive to load, you can attach a special [client directive](/en/reference/directives-reference/#client-directives) that tells Astro to only load the carousel when it becomes visible on the page. If the user never sees it, it never loads.

In Astro, it’s up to you as the developer to explicitly tell Astro which components on the page need to also run in the browser. Astro will only hydrate exactly what’s needed on the page and leave the rest of your site as static HTML.

**Client islands are the secret to Astro’s fast-by-default performance story!**

Read more about [using JavaScript framework components](/en/guides/framework-components/) in your project.

## Server islands

[Section titled “Server islands”](#server-islands)

Server islands are a way to move expensive or slow server-side code out of the way of the main rendering process, making it easy to combine high-performance static HTML and dynamic server-generated components.

Add the [`server:defer` directive](/en/reference/directives-reference/#server-directives) to any Astro component on your page to turn it into its own server island:

src/pages/index.astro

```astro
---
import Avatar from "../components/Avatar.astro";
---
<Avatar server:defer />
```

This breaks up your page with smaller areas of server-rendered content that each load in parallel.

Your page’s main content can be rendered immediately with placeholder content, such as a generic avatar, until your island’s own content is available. With server islands, having small components of personalized content does not delay the rendering of an otherwise static page.

This rendering pattern was built to be portable. It does not depend on any server infrastructure so it will work with any host, from a Node.js server in a Docker container to the serverless provider of your choice.

### Benefits of server islands

One benefit of server islands is the ability to render the more highly dynamic parts of your page on the fly. This allows the outer shell and main content to be more aggressively cached, providing faster performance.

Another benefit is providing a great visitor experience. Server islands are optimized and load quickly, often even before the browser has even painted the page. But in the short time it takes for your islands to render, you can display custom fallback content and prevent any layout shift.

An example of a site that benefits from Astro’s server islands is an e-commerce storefront. Although the main content of product pages change infrequently, these pages typically have some dynamic pieces:

* The user’s avatar in the header.
* Special deals and sales for the product.
* User reviews.

Using server islands for these elements, your visitor will see the most important part of the page, your product, immediately. Generic avatars, loading spinners, and store announcements can be displayed as fallback content until the personalized parts are available.

Read more about [using server islands](/en/guides/server-islands/) in your project.

# Install Astro

> How to install Astro and start a new project.

The [`create astro` CLI command](#install-from-the-cli-wizard) is the fastest way to start a new Astro project from scratch. It will walk you through every step of setting up your new Astro project and allow you to choose from a few different official starter templates.

You can also run the CLI command with the `template` flag to begin your project using any existing theme or starter template. Explore our [themes and starters showcase](https://astro.build/themes/) where you can browse themes for blogs, portfolios, documentation sites, landing pages, and more!

To install Astro manually instead, see our [step-by-step manual installation guide](#manual-setup).

Online previews

Prefer to try Astro in your browser? Visit [astro.new](https://astro.new/) to browse our starter templates and spin up a new Astro project without ever leaving your browser.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* **Node.js** - `v22.12.0` or higher. Odd-numbered versions like `v23` are not supported.
* **Text editor** - We recommend [VS Code](https://code.visualstudio.com/) with our [Official Astro extension](https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode).
* **Terminal** - Astro is accessed through its command-line interface (CLI).

## Browser compatibility

[Section titled “Browser compatibility”](#browser-compatibility)

Astro is built with Vite which targets browsers with modern JavaScript support by default. For a complete reference, you can see the [list of currently supported browser versions in Vite](https://vite.dev/guide/build.html#browser-compatibility).

## Install from the CLI wizard

[Section titled “Install from the CLI wizard”](#install-from-the-cli-wizard)

You can run `create astro` anywhere on your machine, so there’s no need to create a new empty directory for your project before you begin. If you don’t have an empty directory yet for your new project, the wizard will help create one for you automatically.

1. Run the following command in your terminal to start the install wizard:

   * npm

     ```shell
     # create a new project with npm
     npm create astro@latest
     ```

   * pnpm

     ```shell
     # create a new project with pnpm
     pnpm create astro@latest
     ```

   * Yarn

     ```shell
     # create a new project with yarn
     yarn create astro
     ```

   If all goes well, you will see a success message followed by some recommended next steps.

2. Now that your project has been created, you can `cd` into your new project directory to begin using Astro.

3. If you skipped the “Install dependencies?” step during the CLI wizard, then be sure to install your dependencies before continuing.

   * npm

     ```shell
     npm install
     ```

   * pnpm

     ```shell
     pnpm install
     ```

   * Yarn

     ```shell
     yarn install
     ```

4. You can now [start the Astro dev server](/en/develop-and-build/#start-the-astro-dev-server) and see a live preview of your project while you build!

## CLI installation flags

[Section titled “CLI installation flags”](#cli-installation-flags)

You can run the `create astro` command with additional flags to customize the setup process (e.g. answering “yes” to all questions, skipping the Houston animation) or your new project (e.g. install git or not, add integrations).

See [all the available `create astro` command flags](https://github.com/withastro/astro/blob/main/packages/create-astro/README.md).

### Add integrations

[Section titled “Add integrations”](#add-integrations)

You can start a new Astro project and install any [official integrations](/en/guides/integrations/) or community integrations that support the `astro add` command at the same time by passing the `--add` argument to the `create astro` command.

Run the following command in your terminal, substituting any integration that supports the `astro add` command:

* npm

  ```shell
  # create a new project with React and Partytown
  npm create astro@latest -- --add react --add partytown
  ```

* pnpm

  ```shell
  # create a new project with React and Partytown
  pnpm create astro@latest --add react --add partytown
  ```

* Yarn

  ```shell
  # create a new project with React and Partytown
  yarn create astro --add react --add partytown
  ```

### Use a theme or starter template

[Section titled “Use a theme or starter template”](#use-a-theme-or-starter-template)

You can start a new Astro project based on an [official example](https://github.com/withastro/astro/tree/main/examples) or the `main` branch of any GitHub repository by passing a `--template` argument to the `create astro` command.

Run the following command in your terminal, substituting the official Astro starter template name, or the GitHub username and repository of the theme you want to use:

* npm

  ```shell
  # create a new project with an official example
  npm create astro@latest -- --template <example-name>


  # create a new project based on a GitHub repository’s main branch
  npm create astro@latest -- --template <github-username>/<github-repo>
  ```

* pnpm

  ```shell
  # create a new project with an official example
  pnpm create astro@latest --template <example-name>


  # create a new project based on a GitHub repository’s main branch
  pnpm create astro@latest --template <github-username>/<github-repo>
  ```

* Yarn

  ```shell
  # create a new project with an official example
  yarn create astro --template <example-name>


  # create a new project based on a GitHub repository’s main branch
  yarn create astro --template <github-username>/<github-repo>
  ```

By default, this command will use the template repository’s `main` branch. To use a different branch name, pass it as part of the `--template` argument: `<github-username>/<github-repo>#<branch>`.

## Manual Setup

[Section titled “Manual Setup”](#manual-setup)

This guide will walk you through the steps to manually install and configure a new Astro project.

If you prefer not to use our automatic `create astro` CLI tool, you can set up your project yourself by following the guide below.

1. Create your directory

   Create an empty directory with the name of your project, and then navigate into it.

   ```bash
   mkdir my-astro-project
   cd my-astro-project
   ```

   Once you are in your new directory, create your project `package.json` file. This is how you will manage your project dependencies, including Astro. If you aren’t familiar with this file format, run the following command to create one.

   * npm

     ```shell
     npm init --yes
     ```

   * pnpm

     ```shell
     pnpm init
     ```

   * Yarn

     ```shell
     yarn init --yes
     ```

2. Install Astro

   First, install the Astro project dependencies inside your project.

   Important

   Astro must be installed locally, not globally. Make sure you are *not* running `npm install -g astro` `pnpm add -g astro` or `yarn add global astro`.

   * npm

     ```shell
     npm install astro
     ```

   * pnpm

     ```shell
     pnpm add astro
     ```

   * Yarn

     ```shell
     yarn add astro
     ```

   Then, replace any placeholder “scripts” section of your `package.json` with the following:

   package.json

   ```diff
   {
     "scripts": {
       -"test": "echo \"Error: no test specified\" && exit 1",
       +"dev": "astro dev",
       +"build": "astro build",
       +"preview": "astro preview"
     },
   }
   ```

   You’ll use these scripts later in the guide to start Astro and run its different commands.

3. Create your first page

   In your text editor, create a new file in your directory at `src/pages/index.astro`. This will be your first Astro page in the project.

   For this guide, copy and paste the following code snippet (including `---` dashes) into your new file:

   src/pages/index.astro

   ```astro
   ---
   // Welcome to Astro! Everything between these triple-dash code fences
   // is your "component frontmatter". It never runs in the browser.
   console.log('This runs in your terminal, not the browser!');
   ---
   <!-- Below is your "component template." It's just HTML, but with
       some magic sprinkled in to help you build great templates. -->
   <html>
     <body>
       <h1>Hello, World!</h1>
     </body>
   </html>
   <style>
     h1 {
       color: orange;
     }
   </style>
   ```

4. Create your first static asset

   You will also want to create a `public/` directory to store your static assets. Astro will always include these assets in your final build, so you can safely reference them from inside your component templates.

   In your text editor, create a new file in your directory at `public/robots.txt`. `robots.txt` is a simple file that most sites will include to tell search bots like Google how to treat your site.

   For this guide, copy and paste the following code snippet into your new file:

   public/robots.txt

   ```diff
   # Example: Allow all bots to scan and index your site.
   # Full syntax: https://developers.google.com/search/docs/advanced/robots/create-robots-txt
   User-agent: *
   Allow: /
   ```

5. Create `astro.config.mjs`

   Astro is configured using `astro.config.mjs`. This file is optional if you do not need to configure Astro, but you may wish to create it now.

   Create `astro.config.mjs` at the root of your project, and copy the code below into it:

   astro.config.mjs

   ```js
   import { defineConfig } from "astro/config";


   // https://astro.build/config
   export default defineConfig({});
   ```

   If you want to include [UI framework components](/en/guides/framework-components/) such as React, Svelte, etc. or use other tools such as MDX or Partytown in your project, here is where you will [manually import and configure integrations](/en/guides/integrations/).

   Read Astro’s [API configuration reference](/en/reference/configuration-reference/) for more information.

6. Add TypeScript support

   TypeScript is configured using `tsconfig.json`. Even if you don’t write TypeScript code, this file is important so that tools like Astro and VS Code know how to understand your project. Some features (like npm package imports) aren’t fully supported in the editor without a `tsconfig.json` file.

   If you do intend to write TypeScript code, using Astro’s `strict` or `strictest` template is recommended. You can view and compare the three template configurations at [astro/tsconfigs/](https://github.com/withastro/astro/blob/main/packages/astro/tsconfigs/).

   Create `tsconfig.json` at the root of your project, and copy the code below into it. (You can use `base`, `strict`, or `strictest` for your TypeScript template):

   tsconfig.json

   ```json
   {
     "extends": "astro/tsconfigs/base"
   }
   ```

   Read Astro’s [TypeScript setup guide](/en/guides/typescript/#setup) for more information.

7. Next Steps

   If you have followed the steps above, your project directory should now look like this:

   * node\_modules/

     * …

   * public/

     * robots.txt

   * src/

     * pages/

       * index.astro

   * astro.config.mjs

   * package-lock.json or `yarn.lock`, `pnpm-lock.yaml`, etc.

   * package.json

   * tsconfig.json

8. You can now [start the Astro dev server](/en/develop-and-build/#start-the-astro-dev-server) and see a live preview of your project while you build!

# Project structure

> An introduction to the basic file structure of an Astro project.

Your new Astro project generated from the `create astro` CLI wizard already includes some files and folders. Others, you will create yourself and add to Astro’s existing file structure.

Here’s how an Astro project is organized, and some files you will find in your new project.

## Directories and Files

[Section titled “Directories and Files”](#directories-and-files)

Astro leverages an opinionated folder layout for your project. Every Astro project root should include the following directories and files:

* `src/*` - Your project source code (components, pages, styles, images, etc.)
* `public/*` - Your non-code, unprocessed assets (fonts, icons, etc.)
* `package.json` - A project manifest.
* `astro.config.mjs` - An Astro configuration file. (recommended)
* `tsconfig.json` - A TypeScript configuration file. (recommended)

### Example Project Tree

[Section titled “Example Project Tree”](#example-project-tree)

A common Astro project directory might look like this:

* public/

  * robots.txt
  * favicon.svg
  * my-cv.pdf

* src/

  * blog/

    * post1.md
    * post2.md
    * post3.md

  * components/

    * Header.astro
    * Button.jsx

  * images/

    * image1.jpg
    * image2.jpg
    * image3.jpg

  * layouts/

    * PostLayout.astro

  * pages/

    * posts/

      * \[post].astro

    * about.astro

    * **index.astro**

    * rss.xml.js

  * styles/

    * global.css

  * content.config.ts

* astro.config.mjs

* package.json

* tsconfig.json

### `src/`

[Section titled “src/”](#src)

The `src/` folder is where most of your project source code lives. This includes:

* [Pages](/en/basics/astro-pages/)
* [Layouts](/en/basics/layouts/)
* [Astro components](/en/basics/astro-components/)
* [UI framework components (React, etc.)](/en/guides/framework-components/)
* [Styles (CSS, Sass)](/en/guides/styling/)
* [Markdown](/en/guides/markdown-content/)
* [Images to be optimized and processed by Astro](/en/guides/images/)

Astro processes, optimizes, and bundles your `src/` files to create the final website that is shipped to the browser. Unlike the static `public/` directory, your `src/` files are built and handled for you by Astro.

Some files (like Astro components) are not even sent to the browser as written but are instead rendered to static HTML. Other files (like CSS) are sent to the browser but may be optimized or bundled with other CSS files for performance.

Tip

While this guide describes some popular conventions used in the Astro community, the only directory reserved by Astro is `src/pages/`. You are free to rename and reorganize any other directories in a way that works best for you.

### `src/pages`

[Section titled “src/pages”](#srcpages)

Pages routes are created for your site by adding [supported file types](/en/basics/astro-pages/#supported-page-files) to this directory.

Caution

`src/pages` is a **required** sub-directory in your Astro project. Without it, your site will have no pages or routes!

### `src/components`

[Section titled “src/components”](#srccomponents)

**Components** are reusable units of code for your HTML pages. These could be [Astro components](/en/basics/astro-components/), or [UI framework components](/en/guides/framework-components/) like React or Vue. It is common to group and organize all of your project components together in this folder.

This is a common convention in Astro projects, but it is not required. Feel free to organize your components however you like!

### `src/layouts`

[Section titled “src/layouts”](#srclayouts)

[Layouts](/en/basics/layouts/) are Astro components that define the UI structure shared by one or more [pages](/en/basics/astro-pages/).

Just like `src/components`, this directory is a common convention but not required.

### `src/styles`

[Section titled “src/styles”](#srcstyles)

It is a common convention to store your CSS or Sass files in a `src/styles` directory, but this is not required. As long as your styles live somewhere in the `src/` directory and are imported correctly, Astro will handle and optimize them.

### `public/`

[Section titled “public/”](#public)

The `public/` directory is for files and assets in your project that do not need to be processed during Astro’s build process. The files in this folder will be copied into the build folder untouched, and then your site will be built.

This behavior makes `public/` ideal for common assets that do not require any processing, like some images and fonts, or special files such as `robots.txt` and `manifest.webmanifest`.

You can place CSS and JavaScript in your `public/` directory, but be aware that those files will not be bundled or optimized in your final build.

Tip

As a general rule, any CSS or JavaScript that you write yourself should live in your `src/` directory.

### `package.json`

[Section titled “package.json”](#packagejson)

This is a file used by JavaScript package managers to manage your dependencies. It also defines the scripts that are commonly used to run Astro (ex: `npm run dev`, `npm run build`).

There are [two kinds of dependencies](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file) you can specify in a `package.json`: `dependencies` and `devDependencies`. In most cases, these work the same: Astro needs all dependencies at build time, and your package manager will install both. We recommend putting all of your dependencies in `dependencies` to start, and only use `devDependencies` if you find a specific need to do so.

For help creating a new `package.json` file for your project, check out the [manual setup](/en/install-and-setup/#manual-setup) instructions.

### `astro.config.mjs`

[Section titled “astro.config.mjs”](#astroconfigmjs)

This file is generated in every starter template and includes configuration options for your Astro project. Here you can specify integrations to use, build options, server options, and more.

Astro supports several file formats for its JavaScript configuration file: `astro.config.js`, `astro.config.mjs` and `astro.config.ts`. We recommend using `.mjs` in most cases or `.ts` if you want to write TypeScript in your config file.

TypeScript config file loading is handled using [`tsm`](https://github.com/lukeed/tsm) and will respect your project’s `tsconfig` options.

See the [configuration reference](/en/reference/configuration-reference/) for complete details.

### `tsconfig.json`

[Section titled “tsconfig.json”](#tsconfigjson)

This file is generated in every starter template and includes TypeScript configuration options for your Astro project. Some features (like npm package imports) aren’t fully supported in the editor without a `tsconfig.json` file.

See the [TypeScript Guide](/en/guides/typescript/) for details on setting configurations.

# Develop and build

> How to start working on a new project.

Once you have an Astro project, now you’re ready to build with Astro! 🚀

## Edit your project

[Section titled “Edit your project”](#edit-your-project)

To make changes to your project, open your project folder in your code editor. Working in development mode with the dev server running allows you to see updates to your site as you edit the code.

You can also [customize aspects of your development environment](#configure-your-dev-environment) such as configuring TypeScript or installing the official Astro editor extensions.

### Start the Astro dev server

[Section titled “Start the Astro dev server”](#start-the-astro-dev-server)

Astro comes with a built-in development server that has everything you need for project development. The `astro dev` CLI command will start the local development server so that you can see your new website in action for the very first time.

Every starter template comes with a pre-configured script that will run `astro dev` for you. After navigating into your project directory, use your favorite package manager to run this command and start the Astro development server.

* npm

  ```shell
  npm run dev
  ```

* pnpm

  ```shell
  pnpm run dev
  ```

* Yarn

  ```shell
  yarn run dev
  ```

If all goes well, Astro will now be serving your project on <http://localhost:4321/>. Visit that link in your browser and see your new site!

### Work in development mode

[Section titled “Work in development mode”](#work-in-development-mode)

Astro will listen for live file changes in your `src/` directory and update your site preview as you build, so you will not need to restart the server as you make changes during development. You will always be able to see an up-to-date version of your site in your browser when the dev server is running.

When viewing your site in the browser, you’ll have access to the [Astro dev toolbar](/en/guides/dev-toolbar/). As you build, it will help you inspect your [islands](/en/concepts/islands/), spot accessibility issues, and more.

If you aren’t able to open your project in the browser after starting the dev server, go back to the terminal where you ran the `dev` command and check the message displayed. It should tell you if an error occurred, or if your project is being served at a different URL than <http://localhost:4321/>.

## Build and preview your site

[Section titled “Build and preview your site”](#build-and-preview-your-site)

To check the version of your site that will be created at build time, quit the dev server (`Ctrl` + `C`) and run the appropriate build command for your package manager in your terminal:

* npm

  ```shell
  npm run build
  ```

* pnpm

  ```shell
  pnpm build
  ```

* Yarn

  ```shell
  yarn run build
  ```

Astro will build a deploy-ready version of your site in a separate folder (`dist/` by default) and you can watch its progress in the terminal. This will alert you to any build errors in your project before you deploy to production. If TypeScript is configured to `strict` or `strictest`, the `build` script will also check your project for type errors.

When the build is finished, run the appropriate `preview` command (e.g. `npm run preview`) in your terminal and you can view the built version of your site locally in the same browser preview window.

Note that this previews your code as it existed when the build command was last run. This is meant to give you a preview of how your site will look when it is deployed to the web. Any later changes you make to your code after building will **not** be reflected while you preview your site until you run the build command again.

Use (`Ctrl` + `C`) to quit the preview and run another terminal command, such as restarting the dev server to go back to [working in development mode](#work-in-development-mode) which does update as you edit to show a live preview of your code changes.

Read more about [the Astro CLI](/en/reference/cli-reference/) and the terminal commands you will use as you build with Astro.

Tip

You may wish to [deploy your new site right away](/en/guides/deploy/), before you begin to add or change too much code. This is helpful to get a minimal, working version of your site published and can save you extra time and effort troubleshooting your deployment later.

## Next Steps

[Section titled “Next Steps”](#next-steps)

Success! You are now ready to start building with Astro! 🥳

Here are a few things that we recommend exploring next. You can read them in any order. You can even leave our documentation for a bit and go play in your new Astro project codebase, coming back here whenever you run into trouble or have a question.

### Configure your dev environment

[Section titled “Configure your dev environment”](#configure-your-dev-environment)

Explore the guides below to customize your development experience.

[Editor Setup ](/en/editor-setup/)Customize your code editor to improve the Astro developer experience and unlock new features.

[Dev Toolbar ](/en/guides/dev-toolbar/)Explore the helpful features of the dev toolbar.

[TypeScript Configuration ](/en/guides/typescript/)Configure options for type-checking, IntelliSense, and more.

### Explore Astro’s Features

[Section titled “Explore Astro’s Features”](#explore-astros-features)

[Understand your codebase ](/en/basics/project-structure/)Learn about Astro’s file structure in our Project Structure guide.

[Create content collections ](/en/guides/content-collections/)Add content to your new site with frontmatter validation and automatic type-safety.

[Add view transitions ](/en/guides/view-transitions/)Create seamless page transitions and animations.

[Learn about Islands ](/en/concepts/islands/)Read about Astro's islands architecture.

### Take the introductory tutorial

[Section titled “Take the introductory tutorial”](#take-the-introductory-tutorial)

Build a fully functional Astro blog starting from a single blank page in our [introductory tutorial](/en/tutorial/0-introduction/).

This is a great way to see how Astro works and walks you through the basics of pages, layouts, components, routing, islands, and more. It also includes an optional, beginner-friendly unit for those newer to web development concepts in general, which will guide you through installing the necessary applications on your computer, creating a GitHub account, and deploying your site.

# Configuration overview

> Get to know the ways you can configure and customize your new project and your development experience.

Astro is a flexible, unopinionated framework that allows you to configure your project in many different ways. This means that getting started with a new project might feel overwhelming: there is no “one best way” to set up your Astro project!

The guides in this “Configuration” section will help you familiarize yourself with the various files that allow you to configure and customize aspects of your project and development environment.

If this is your first Astro project, or if it’s been a while since you’ve set up a new project, use the following guides and reference in the documentation for assistance.

## The Astro config File

[Section titled “The Astro config File”](#the-astro-config-file)

The [Astro config file](/en/reference/configuration-reference/) is a JavaScript file included at the root of every starter project:

astro.config.mjs

```js
import { defineConfig } from "astro/config";


export default defineConfig({
  // your configuration options here...
});
```

It is only required if you have something to configure, but most projects will use this file. The `defineConfig()` helper provides automatic IntelliSense in your IDE and is where you will add all your configuration options to tell Astro how to build and render your project to HTML.

We recommend using the default file format `.mjs` in most cases, or `.ts` if you want to write TypeScript in your config file. However, `astro.config.js` is also supported.

Read Astro’s [configuration reference](/en/reference/configuration-reference/) for a full overview of all supported configuration options.

## The TypeScript config File

[Section titled “The TypeScript config File”](#the-typescript-config-file)

Every Astro starter project includes a `tsconfig.json` file in your project. Astro’s [component script](/en/basics/astro-components/#the-component-script) is Typescript, which provides Astro’s editor tooling and allows you to optionally add syntax to your JavaScript for type checking of your own project code.

Use the `tsconfig.json` file to configure the TypeScript template that will perform type checks on your code, configure TypeScript plugins, set import aliases, and more.

Read Astro’s [TypeScript guide](/en/guides/typescript/) for a full overview of TypeScript options and Astro’s built-in utility types.

## Development Experience

[Section titled “Development Experience”](#development-experience)

While you work in development mode, you can take advantage of your code editor and other tools to improve the Astro developer experience.

Astro provides its own official VS Code extension and is compatible with several other popular editor tools. Astro also provides a customizable toolbar that displays in your browser preview while the dev server is running. You can install and even build your own toolbar apps for additional functionality.

Read Astro’s guides to [editor setup options](/en/editor-setup/) and [using the dev toolbar](/en/guides/dev-toolbar/) to learn how to customize your development experience.

## Common new project tasks

[Section titled “Common new project tasks”](#common-new-project-tasks)

Here are some first steps you might choose to take with a new Astro project.

### Add your deployment domain

[Section titled “Add your deployment domain”](#add-your-deployment-domain)

For generating your sitemap and creating canonical URLs, configure your deployment URL in the [`site`](/en/reference/configuration-reference/#site) option. If you are deploying to a path (e.g. `www.example.com/docs`), you can also configure a [`base`](/en/reference/configuration-reference/#base) for the root of your project.

Additionally, different deployment hosts may have different behavior regarding trailing slashes at the end of your URLs. (e.g. `example.com/about` vs `example.com/about/`). Once your site is deployed, you may need to configure your [`trailingSlash`](/en/reference/configuration-reference/#trailingslash) preference.

astro.config.mjs

```js
import { defineConfig } from "astro/config";


export default defineConfig({
  site: "https://www.example.com",
  base: "/docs",
  trailingSlash: "always",
});
```

### Add site metadata

[Section titled “Add site metadata”](#add-site-metadata)

Astro does not use its configuration file for common SEO or meta data, only for information required to build your project code and render it to HTML.

Instead, this information is added to your page `<head>` using standard HTML `<link>` and `<meta>` tags, just as if you were writing plain HTML pages.

One common pattern for Astro sites is to create a `<Head />` [`.astro` component](/en/basics/astro-components/) that can be added to a common [layout component](/en/basics/layouts/) so it can apply to all your pages.

src/components/MainLayout.astro

```astro
---
import Head from "./Head.astro";


const { ...props } = Astro.props;
---
<html>
  <head>
    <meta charset="utf-8">
    <Head />
    <!-- Additional head elements -->
  </head>
  <body>
    <!-- Page content goes here -->
  </body>
</html>
```

Because `Head.astro` is just a regular Astro component, you can import files and receive props passed from other components, such as a specific page title.

src/components/Head.astro

```astro
---
import Favicon from "../assets/Favicon.astro";
import SomeOtherTags from "./SomeOtherTags.astro";


const { title = "My Astro Website", ...props } = Astro.props;
---
<link rel="sitemap" href="/sitemap-index.xml">
<title>{title}</title>
<meta name="description" content="Welcome to my new Astro site!">


<!-- Web analytics -->
<script data-goatcounter="https://my-account.goatcounter.com/count" async src="//gc.zgo.at/count.js"></script>


<!-- Open Graph tags -->
<meta property="og:title" content="My New Astro Website" />
<meta property="og:type" content="website" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:description" content="Welcome to my new Astro site!" />
<meta property="og:image" content="https://www.example.com/_astro/seo-banner.BZD7kegZ.webp">
<meta property="og:image:alt" content="">


<SomeOtherTags />


<Favicon />
```

# Astro Courses

> Learn Astro with out-of-this-world courses and tutorials.

Want to get started learning Astro with a course or tutorial?

You can learn the basics of Astro with our [official docs Build a Blog tutorial](/en/tutorial/0-introduction/), or explore our collection of recommended Astro educational content.

## Education Partners

[Section titled “Education Partners”](#education-partners)

Support Astro while you learn

Use Astro’s affiliate links for discounts with our education partners and some of your purchase goes directly back to funding development of the Astro open source project!

Learn Astro from trusted Astro educators, with video lessons, interactive challenges, and projects!

[Learn Astro with Coding in Public ](https://learnastro.dev/?code=ASTRO_PROMO)A premium interactive course with 150+ video lessons that teaches you how to use Astro’s built-in tooling and features.

[Learn Astro with James Q Quick ](https://scrimba.com/intro-to-astro-c00ar0fi5u?via=astro)Build your first Astro site with 35 interactive Scrimba lessons, with video and IDE merged into one unique learning platform.

## Community learning resources

[Section titled “Community learning resources”](#community-learning-resources)

Learn from your fellow astronauts with curated collections of guides, articles, and blog posts.

[Astro Tips ](https://astro-tips.dev/)Advanced, unusual, experimental, and community-written recipes, tutorials, and quick tips.

[Astro Support Squid ](https://supportsquid.ink/)An open, public Astro support forum and knowledge base outside of the Astro Discord.

# Components

> An introduction to Astro components.

**Astro components** are the basic building blocks of any Astro project. They are HTML-only templating components with no client-side runtime and use the `.astro` file extension.

Note

If you know HTML, you already know enough to write your first Astro component.

Learn more in the [Astro syntax reference](/en/reference/astro-syntax/).

Astro components are extremely flexible. An Astro component can be as small as a snippet of HTML, like a collection of common `<meta>` tags that make SEO easy to work with. Components can be reusable UI elements, like a header or a profile card. Astro components can even contain an entire page layout or, when located in the special `src/pages/` folder, be an entire page itself.

The most important thing to know about Astro components is that they **don’t render on the client**. They render to HTML either at build-time or on-demand. You can include JavaScript code inside of your component frontmatter, and all of it will be stripped from the final page sent to your users’ browsers. The result is a faster site, with zero JavaScript footprint added by default.

When your Astro component does need client-side interactivity, you can add [standard HTML `<script>` tags](/en/guides/client-side-scripts/) or [UI Framework components](/en/guides/framework-components/#hydrating-interactive-components) as “client islands”.

For components that need to render personalized or dynamic content, you can defer their server rendering by adding a [server directive](/en/reference/directives-reference/#server-directives). These “server islands” will render their content when it is available, without delaying the entire page load.

## Component Structure

[Section titled “Component Structure”](#component-structure)

An Astro component is made up of two main parts: the **Component Script** and the **Component Template**. Each part performs a different job, but together they provide a framework that is both easy to use and expressive enough to handle whatever you might want to build.

src/components/EmptyComponent.astro

```astro
---
// Component Script (JavaScript)
---
<!-- Component Template (HTML + JS Expressions) -->
```

### The Component Script

[Section titled “The Component Script”](#the-component-script)

Astro uses a code fence (`---`) to identify the component script in your Astro component. If you’ve ever written Markdown before, you may already be familiar with a similar concept called *frontmatter.* Astro’s idea of a component script was directly inspired by this concept.

You can use the component script to write any JavaScript code that you need to render your template. This can include:

* importing other Astro components
* importing other framework components, like React
* importing data, like a JSON file
* fetching content from an API or database
* creating variables that you will reference in your template

src/components/MyComponent.astro

```astro
---
import SomeAstroComponent from '../components/SomeAstroComponent.astro';
import SomeReactComponent from '../components/SomeReactComponent.jsx';
import someData from '../data/pokemon.json';


// Access passed-in component props, like `<X title="Hello, World" />`
const { title } = Astro.props;


// Fetch external data, even from a private API or database
const data = await fetch('SOME_SECRET_API_URL/users').then(r => r.json());
---
<!-- Your template here! -->
```

The code fence is designed to guarantee that the JavaScript that you write in it is “fenced in.” It won’t escape into your frontend application, or fall into your user’s hands. You can safely write code here that is expensive or sensitive (like a call to your private database) without worrying about it ever ending up in your user’s browser.

Note

The Astro component script is TypeScript, which allows you to add additional syntax to JavaScript for editor tooling, and error checking.

Read more about Astro’s [built-in TypeScript support](/en/guides/typescript/).

### The Component Template

[Section titled “The Component Template”](#the-component-template)

The component template is below the code fence and determines the HTML output of your component.

If you write plain HTML here, your component will render that HTML in any Astro page it is imported and used.

However, [Astro’s component template syntax](/en/reference/astro-syntax/) also supports **JavaScript expressions**, Astro [`<style>`](/en/guides/styling/#styling-in-astro) and [`<script>`](/en/guides/client-side-scripts/) tags, **imported components**, and [**special Astro directives**](/en/reference/directives-reference/). Data and values defined in the component script can be used in the component template to produce dynamically-created HTML.

src/components/MyFavoritePokemon.astro

```astro
---
// Your component script here!
import Banner from '../components/Banner.astro';
import Avatar from '../components/Avatar.astro';
import ReactPokemonComponent from '../components/ReactPokemonComponent.jsx';
const myFavoritePokemon = [/* ... */];
const { title } = Astro.props;
---
<!-- HTML comments supported! -->
{/* JS comment syntax is also valid! */}


<Banner />
<h1>Hello, world!</h1>


<!-- Use props and other variables from the component script: -->
<p>{title}</p>


<!-- Delay component rendering and provide fallback loading content: -->
<Avatar server:defer>
  <svg slot="fallback" class="generic-avatar" transition:name="avatar">...</svg>
</Avatar>


<!-- Include other UI framework components with a `client:` directive to hydrate: -->
<ReactPokemonComponent client:visible />


<!-- Mix HTML with JavaScript expressions, similar to JSX: -->
<ul>
  {myFavoritePokemon.map((data) => <li>{data.name}</li>)}
</ul>


<!-- Use a template directive to build class names from multiple strings or even objects! -->
<p class:list={["add", "dynamic", { classNames: true }]} />
```

## Component-based design

[Section titled “Component-based design”](#component-based-design)

Components are designed to be **reusable** and **composable**. You can use components inside of other components to build more and more advanced UI. For example, a `Button` component could be used to create a `ButtonGroup` component:

src/components/ButtonGroup.astro

```astro
---
import Button from './Button.astro';
---
<div>
  <Button title="Button 1" />
  <Button title="Button 2" />
  <Button title="Button 3" />
</div>
```

## Component Props

[Section titled “Component Props”](#component-props)

An Astro component can define and accept props. These props then become available to the component template for rendering HTML. Props are available on the `Astro.props` global in your frontmatter script.

Here is an example of a component that receives a `greeting` prop and a `name` prop. Notice that the props to be received are destructured from the global `Astro.props` object.

src/components/GreetingHeadline.astro

```astro
---
// Usage: <GreetingHeadline greeting="Howdy" name="Partner" />
const { greeting, name } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

This component, when imported and rendered in other Astro components, layouts or pages, can pass these props as attributes:

src/components/GreetingCard.astro

```astro
---
import GreetingHeadline from './GreetingHeadline.astro';
const name = 'Astro';
---
<h1>Greeting Card</h1>
<GreetingHeadline greeting="Hi" name={name} />
<p>I hope you have a wonderful day!</p>
```

You can also define your props with TypeScript with a `Props` type interface. Astro will automatically pick up the `Props` interface in your frontmatter and give type warnings/errors. These props can also be given default values when destructured from `Astro.props`.

src/components/GreetingHeadline.astro

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

Component props can be given default values to use when none are provided.

src/components/GreetingHeadline.astro

```astro
---
const { greeting = "Hello", name = "Astronaut" } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

## Slots

[Section titled “Slots”](#slots)

The `<slot />` element is a placeholder for external HTML content, allowing you to inject (or “slot”) child elements from other files into your component template.

By default, all child elements passed to a component will be rendered in its `<slot />`.

Note

Unlike *props*, which are attributes passed to an Astro component available for use throughout your component with `Astro.props`, *slots* render child HTML elements where they are written.

src/components/Wrapper.astro

```astro
---
import Header from './Header.astro';
import Logo from './Logo.astro';
import Footer from './Footer.astro';


const { title } = Astro.props;
---
<div id="content-wrapper">
  <Header />
  <Logo />
  <h1>{title}</h1>
  <slot />  <!-- children will go here -->
  <Footer />
</div>
```

src/pages/fred.astro

```astro
---
import Wrapper from '../components/Wrapper.astro';
---
<Wrapper title="Fred's Page">
  <h2>All about Fred</h2>
  <p>Here is some stuff about Fred.</p>
</Wrapper>
```

This pattern is the basis of an [Astro layout component](/en/basics/layouts/): an entire page of HTML content can be “wrapped” with `<SomeLayoutComponent></SomeLayoutComponent>` tags and sent to the component to render inside of common page elements defined there.

See the [`Astro.slots` utility functions](/en/reference/astro-syntax/#astroslots) for more ways to access and render slot content.

### Named Slots

[Section titled “Named Slots”](#named-slots)

An Astro component can also have named slots. This allows you to pass only HTML elements with the corresponding slot name into a slot’s location.

Slots are named using the `name` attribute:

src/components/Wrapper.astro

```astro
---
import Header from './Header.astro';
import Logo from './Logo.astro';
import Footer from './Footer.astro';


const { title } = Astro.props;
---
<div id="content-wrapper">
  <Header />
  <!--  children with the `slot="after-header"` attribute will go here -->
  <slot name="after-header" />
  <Logo />
  <h1>{title}</h1>
  <!--  children without a `slot`, or with `slot="default"` attribute will go here -->
  <slot />
  <Footer />
  <!--  children with the `slot="after-footer"` attribute will go here -->
  <slot name="after-footer" />
</div>
```

To inject HTML content into a particular slot, use the `slot` attribute on any child element to specify the name of the slot. All other child elements of the component will be injected into the default (unnamed) `<slot />`.

src/pages/fred.astro

```astro
---
import Wrapper from '../components/Wrapper.astro';
---
<Wrapper title="Fred's Page">
  <img src="https://my.photo/fred.jpg" slot="after-header" />
  <h2>All about Fred</h2>
  <p>Here is some stuff about Fred.</p>
  <p slot="after-footer">Copyright 2022</p>
</Wrapper>
```

Tip

Use a `slot="my-slot"` attribute on the child element that you want to pass through to a matching `<slot name="my-slot" />` placeholder in your component.

To pass multiple HTML elements into a component’s `<slot/>` placeholder without a wrapping `<div>`, use the `slot=""` attribute on [Astro’s `<Fragment/>` component](/en/reference/astro-syntax/#fragments):

src/components/CustomTable.astro

```astro
---
// Create a custom table with named slot placeholders for header and body content
---
<table class="bg-white">
  <thead class="sticky top-0 bg-white"><slot name="header" /></thead>
  <tbody class="[&_tr:nth-child(odd)]:bg-gray-100"><slot name="body" /></tbody>
</table>
```

Inject multiple rows and columns of HTML content using a `slot=""` attribute to specify the `"header"` and `"body"` content. Individual HTML elements can also be styled:

src/components/StockTable.astro

```astro
---
import CustomTable from './CustomTable.astro';
---
<CustomTable>
  <Fragment slot="header"> <!-- pass table header -->
    <tr><th>Product name</th><th>Stock units</th></tr>
  </Fragment>


  <Fragment slot="body"> <!-- pass table body -->
    <tr><td>Flip-flops</td><td>64</td></tr>
    <tr><td>Boots</td><td>32</td></tr>
    <tr><td>Sneakers</td><td class="text-red-500">0</td></tr>
  </Fragment>
</CustomTable>
```

Note that named slots must be an immediate child of the component. You cannot pass named slots through nested elements.

Tip

Named slots can also be passed to [UI framework components](/en/guides/framework-components/)!

Note

It is not possible to dynamically generate an Astro slot name, such as within a [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function. If this feature is needed within UI framework components, it might be best to generate these dynamic slots within the framework itself.

### Fallback Content for Slots

[Section titled “Fallback Content for Slots”](#fallback-content-for-slots)

Slots can also render **fallback content**. When there are no matching children passed to a slot, a `<slot />` element will render its own placeholder children.

src/components/Wrapper.astro

```astro
---
import Header from './Header.astro';
import Logo from './Logo.astro';
import Footer from './Footer.astro';


const { title } = Astro.props;
---
<div id="content-wrapper">
  <Header />
  <Logo />
  <h1>{title}</h1>
  <slot>
    <p>This is my fallback content, if there is no child passed into slot</p>
  </slot>
  <Footer />
</div>
```

Fallback content will only be displayed when there are no matching elements with the `slot="name"` attribute being passed in to a named slot.

Astro will pass an empty slot when a slot element exists but has no content to pass. Fallback content cannot be used as a default when an empty slot is passed. Fallback content is only displayed when no slot element can be found.

### Transferring slots

[Section titled “Transferring slots”](#transferring-slots)

Slots can be transferred to other components. For example, when creating nested layouts:

src/layouts/BaseLayout.astro

```astro
---
---
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <slot name="head" />
  </head>
  <body>
    <slot />
  </body>
</html>
```

src/layouts/HomeLayout.astro

```astro
---
import BaseLayout from './BaseLayout.astro';
---
<BaseLayout>
  <slot name="head" slot="head" />
  <slot />
</BaseLayout>
```

Note

Named slots can be transferred to another component using both the `name` and `slot` attributes on a `<slot />` tag.

Now, the default and `head` slots passed to `HomeLayout` will be transferred to the `BaseLayout` parent.

src/pages/index.astro

```astro
---
import HomeLayout from '../layouts/HomeLayout.astro';
---
<HomeLayout>
  <title slot="head">Astro</title>
  <h1>Astro</h1>
</HomeLayout>
```

## HTML Components

[Section titled “HTML Components”](#html-components)

Astro supports importing and using `.html` files as components or placing these files within the `src/pages/` subdirectory as pages. You may want to use HTML components if you’re reusing code from an existing site built without a framework, or if you want to ensure that your component has no dynamic features.

HTML components must contain only valid HTML, and therefore lack key Astro component features:

* They don’t support frontmatter, server-side imports, or dynamic expressions.
* Any `<script>` tags are left unbundled, treated as if they had an [`is:inline` directive](/en/reference/directives-reference/#isinline).
* They can only [reference assets that are in the `public/` folder](/en/basics/project-structure/#public).

Note

A [`<slot />` element](/en/basics/astro-components/#slots) inside an HTML component will work as it would in an Astro component. In order to use the [HTML Web Component Slot](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) element instead, add `is:inline` to your `<slot>` element.

## Next Steps

[Section titled “Next Steps”](#next-steps)

Read more about using [UI framework components](/en/guides/framework-components/) in your Astro project.

# Pages

> An introduction to Astro pages.

**Pages** are files that live in the `src/pages/` subdirectory of your Astro project. They are responsible for handling routing, data loading, and overall page layout for every page in your website.

## Supported page files

[Section titled “Supported page files”](#supported-page-files)

Astro supports the following file types in the `src/pages/` directory:

* [`.astro`](#astro-pages)
* [`.md`](#markdownmdx-pages)
* `.mdx` (with the [MDX Integration installed](/en/guides/integrations-guide/mdx/#installation))
* [`.html`](#html-pages)
* `.js`/`.ts` (as [endpoints](/en/guides/endpoints/))

## File-based routing

[Section titled “File-based routing”](#file-based-routing)

Astro leverages a routing strategy called **file-based routing**. Each file in your `src/pages/` directory becomes an endpoint on your site based on its file path.

A single file can also generate multiple pages using [dynamic routing](/en/guides/routing/#dynamic-routes). This allows you to create pages even if your content lives outside of the special `/pages/` directory, such as in a [content collection](/en/guides/content-collections/) or a [CMS](/en/guides/cms/).

Read more about [Routing in Astro](/en/guides/routing/).

### Link between pages

[Section titled “Link between pages”](#link-between-pages)

Write standard HTML [`<a>` elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) in your Astro pages to link to other pages on your site. Use a **URL path relative to your root domain** as your link, not a relative file path.

For example, to link to `https://example.com/authors/sonali/` from any other page on `example.com`:

src/pages/index.astro

```astro
Read more <a href="/authors/sonali/">about Sonali</a>.
```

## Astro Pages

[Section titled “Astro Pages”](#astro-pages)

Astro pages use the `.astro` file extension and support the same features as [Astro components](/en/basics/astro-components/).

src/pages/index.astro

```astro
---
---
<html lang="en">
  <head>
    <title>My Homepage</title>
  </head>
  <body>
    <h1>Welcome to my website!</h1>
  </body>
</html>
```

A page must produce a full HTML document. If not explicitly included, Astro will add the necessary `<!DOCTYPE html>` declaration and `<head>` content to any `.astro` component located within `src/pages/` by default. You can opt-out of this behavior on a per-component basis by marking it as a [partial](#page-partials) page.

To avoid repeating the same HTML elements on every page, you can move common `<head>` and `<body>` elements into your own [layout components](/en/basics/layouts/). You can use as many or as few layout components as you’d like.

src/pages/index.astro

```astro
---
import MySiteLayout from "../layouts/MySiteLayout.astro";
---
<MySiteLayout>
  <p>My page content, wrapped in a layout!</p>
</MySiteLayout>
```

Read more about [layout components](/en/basics/layouts/) in Astro.

## Markdown/MDX Pages

[Section titled “Markdown/MDX Pages”](#markdownmdx-pages)

Astro also treats any Markdown (`.md`) files inside of `src/pages/` as pages in your final website. If you have the [MDX Integration installed](/en/guides/integrations-guide/mdx/#installation), it also treats MDX (`.mdx`) files the same way.

Tip

Consider creating [content collections](/en/guides/content-collections/) instead of pages for directories of related Markdown files that share a similar structure, such as blog posts or product items.

Markdown files can use the special `layout` frontmatter property to specify a [layout component](/en/basics/layouts/) that will wrap their Markdown content in a full `<html>...</html>` page document.

src/pages/page.md

```md
---
layout: ../layouts/MySiteLayout.astro
title: My Markdown page
---
# Title


This is my page, written in **Markdown.**
```

Read more about [Markdown](/en/guides/markdown-content/) in Astro.

## HTML Pages

[Section titled “HTML Pages”](#html-pages)

Files with the `.html` file extension can be placed in the `src/pages/` directory and used directly as pages on your site. Note that some key Astro features are not supported in [HTML Components](/en/basics/astro-components/#html-components).

## Custom 404 Error Page

[Section titled “Custom 404 Error Page”](#custom-404-error-page)

For a custom 404 error page, you can create a `404.astro` or `404.md` file in `src/pages`.

This will build to a `404.html` page. Most [deploy services](/en/guides/deploy/) will find and use it.

## Custom 500 Error Page

[Section titled “Custom 500 Error Page”](#custom-500-error-page)

For a custom 500 error page to show for pages that are [rendered on demand](/en/guides/on-demand-rendering/), create the file `src/pages/500.astro`. This custom page is not available for prerendered pages.

If an error occurs rendering this page, your host’s default 500 error page will be shown to your visitor.

**Added in:** `astro@4.10.3`

During development, if you have a `500.astro`, the error thrown at runtime is logged in your terminal, as opposed to being shown in the error overlay.

### `error`

[Section titled “error”](#error)

**Added in:** `astro@4.11.0`

`src/pages/500.astro` is a special page that is automatically passed an `error` prop for any error thrown during rendering. This allows you to use the details of an error (e.g. from a page, from middleware, etc.) to display information to your visitor.

The `error` prop’s data type can be anything, which may affect how you type or use the value in your code:

src/pages/500.astro

```astro
---
interface Props {
  error: unknown;
}


const { error } = Astro.props;
---
<div>{error instanceof Error ? error.message : "Unknown error"}</div>
```

To avoid leaking sensitive information when displaying content from the `error` prop, consider evaluating the error first, and returning appropriate content based on the error thrown. For example, you should avoid displaying the error’s stack as it contains information about how your code is structured on the server.

## Page Partials

[Section titled “Page Partials”](#page-partials)

**Added in:** `astro@3.4.0`

Caution

Page partials are intended to be used in conjunction with a front-end library, such as [htmx](https://htmx.org/) or [Unpoly](https://unpoly.com/). You can also use them if you are comfortable writing low-level front-end JavaScript. For this reason they are an advanced feature.

Additionally, partials should not be used if the component contains scoped styles or scripts, as these elements will be stripped from the HTML output. If you need scoped styles, it is better to use regular, non-partial pages along with a frontend library that knows how to merge the contents into the head.

Partials are page components located within `src/pages/` that are not intended to render as full pages.

Like components located outside of this folder, these files do not automatically include the `<!DOCTYPE html>` declaration, nor any `<head>` content such as scoped styles and scripts.

However, because they are located in the special `src/pages/` directory, the generated HTML is available at a URL corresponding to its file path. This allows a rendering library (e.g. [htmx](https://htmx.org/), [Stimulus](https://stimulus.hotwired.dev/), [jQuery](https://jquery.com/)) to access it on the client and load sections of HTML dynamically on a page without a browser refresh or page navigation.

Partials, when combined with a rendering library, provide an alternative to [Astro islands](/en/concepts/islands/) and [`<script>` tags](/en/guides/client-side-scripts/) for building dynamic content in Astro.

Page files that can export a value for [`partial`](/en/reference/routing-reference/#partial) (e.g. `.astro` and `.mdx`, but not `.md`) can be marked as partials.

src/pages/partial.astro

```diff
---
+export const partial = true;
---
<li>I'm a partial!</li>
```

### Using with a library

[Section titled “Using with a library”](#using-with-a-library)

Partials are used to dynamically update a section of a page using a library such as [htmx](https://htmx.org/).

The following example shows an `hx-post` attribute set to a partial’s URL. The content from the partial page will be used to update the targeted HTML element on this page.

src/pages/index.astro

```astro
<html>
  <head>
    <title>My page</title>
    <script src="https://unpkg.com/htmx.org@1.9.6"
      integrity="sha384-FhXw7b6AlE/jyjlZH5iHa/tTe9EpJ1Y55RjcgPbjeWMskSxZt1v9qkxLJWNJaGni"
      crossorigin="anonymous"></script>
  </head>
  <body>
    <section>
      <div id="parent-div">Target here</div>


      <button hx-post="/partials/clicked/"
        hx-trigger="click"
        hx-target="#parent-div"
        hx-swap="innerHTML"
      >
        Click Me!
      </button>
    </section>
  </body>
</html>
```

The `.astro` partial must exist at the corresponding file path, and include an export defining the page as a partial:

src/pages/partials/clicked.astro

```astro
---
export const partial = true;
---
<div>I was clicked!</div>
```

See the [htmx documentation](https://htmx.org/docs/) for more details on using htmx.

# Layouts

> An introduction to layouts in Astro.

**Layouts** are [Astro components](/en/basics/astro-components/) used to provide a reusable UI structure, such as a page template.

We conventionally use the term “layout” for Astro components that provide common UI elements shared across pages such as headers, navigation bars, and footers. A typical Astro layout component provides [Astro, Markdown or MDX pages](/en/basics/astro-pages/) with:

* a **page shell** (`<html>`, `<head>` and `<body>` tags)
* a [**`<slot />`**](/en/basics/astro-components/#slots) to specify where individual page content should be injected.

But, there is nothing special about a layout component! They can [accept props](/en/basics/astro-components/#component-props) and [import and use other components](/en/basics/astro-components/#component-structure) like any other Astro component. They can include [UI frameworks components](/en/guides/framework-components/) and [client-side scripts](/en/guides/client-side-scripts/). They do not even have to provide a full page shell, and can instead be used as partial UI templates.

However, if a layout component does contain a page shell, its `<html>` element must be the parent of all other elements in the component.

Layout components are commonly placed in a `src/layouts` directory in your project for organization, but this is not a requirement; you can choose to place them anywhere in your project. You can even colocate layout components alongside your pages by [prefixing the layout names with `_`](/en/guides/routing/#excluding-pages).

## Sample Layout

[Section titled “Sample Layout”](#sample-layout)

src/layouts/MySiteLayout.astro

```astro
---
import BaseHead from '../components/BaseHead.astro';
import Footer from '../components/Footer.astro';
const { title } = Astro.props;
---
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <BaseHead title={title}/>
  </head>
  <body>
    <nav>
      <a href="#">Home</a>
      <a href="#">Posts</a>
      <a href="#">Contact</a>
    </nav>
    <h1>{title}</h1>
    <article>
      <slot /> <!-- your content is injected here -->
    </article>
    <Footer />
  </body>
  <style>
    h1 {
      font-size: 2rem;
    }
  </style>
</html>
```

src/pages/index.astro

```astro
---
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout title="Home Page">
  <p>My page content, wrapped in a layout!</p>
</MySiteLayout>
```

Learn more about [slots](/en/basics/astro-components/#slots).

## Using TypeScript with layouts

[Section titled “Using TypeScript with layouts”](#using-typescript-with-layouts)

Any Astro layout can be modified to introduce type safety & autocompletion by providing the types for your props:

src/components/MyLayout.astro

```diff
---
+interface Props {
+  title: string;
+  description: string;
+  publishDate: string;
+  viewCount: number;
+}
const { title, description, publishDate, viewCount } = Astro.props;
---
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="description" content={description}>
    <title>{title}</title>
  </head>
  <body>
    <header>
      <p>Published on {publishDate}</p>
      <p>Viewed by {viewCount} folks</p>
    </header>
    <main>
      <slot />
    </main>
  </body>
</html>
```

## Markdown Layouts

[Section titled “Markdown Layouts”](#markdown-layouts)

Page layouts are especially useful for individual Markdown pages which otherwise would not have any page formatting.

Astro provides a special `layout` frontmatter property intended for [individual `.md` files located within `src/pages/` using file-based routing](/en/guides/markdown-content/#individual-markdown-pages) to specify which `.astro` component to use as the page layout. This component allows you to provide `<head>` content like meta tags (e.g. `<meta charset="utf-8">`) and styles for the Markdown page. By default, this specified component can automatically access data from the Markdown file.

This is not recognized as a special property when using [content collections](/en/guides/content-collections/) to query and render your content.

src/pages/page.md

```markdown
---
layout: ../layouts/BlogPostLayout.astro
title: "Hello, World!"
author: "Matthew Phillips"
date: "09 Aug 2022"
---
All frontmatter properties are available as props to an Astro layout component.


The `layout` property is the only special one provided by Astro.


You can use it in Markdown files located within `src/pages/`.
```

A typical layout for a Markdown page includes:

1. The `frontmatter` prop to access the Markdown page’s frontmatter and other data.
2. A default [`<slot />`](/en/basics/astro-components/#slots) to indicate where the page’s Markdown content should be rendered.

src/layouts/BlogPostLayout.astro

```astro
---
// 1. The frontmatter prop gives access to frontmatter and other data
const { frontmatter } = Astro.props;
---
<html>
  <head>
    <!-- Add other Head elements here, like styles and meta tags. -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta charset="utf-8">
    <title>{frontmatter.title}</title>
  </head>
  <body>
    <!-- Add other UI components here, like common headers and footers. -->
    <h1>{frontmatter.title} by {frontmatter.author}</h1>
    <!-- 2. Rendered HTML will be passed into the default slot. -->
    <slot />
    <p>Written on: {frontmatter.date}</p>
  </body>
</html>
```

You can set a layout’s [`Props` type](/en/guides/typescript/#component-props) with the `MarkdownLayoutProps` helper:

src/layouts/BlogPostLayout.astro

```diff
---
+import type { MarkdownLayoutProps } from 'astro';


+type Props = MarkdownLayoutProps<{
  +// Define frontmatter props here
+  title: string;
+  author: string;
+  date: string;
+}>;


// Now, `frontmatter`, `url`, and other Markdown layout properties
// are accessible with type safety
const { frontmatter, url } = Astro.props;
---
<html>
  <head>
    <meta charset="utf-8">
    <link rel="canonical" href={new URL(url, Astro.site).pathname}>
    <title>{frontmatter.title}</title>
  </head>
  <body>
    <h1>{frontmatter.title} by {frontmatter.author}</h1>
    <slot />
    <p>Written on: {frontmatter.date}</p>
  </body>
</html>
```

### Markdown Layout Props

[Section titled “Markdown Layout Props”](#markdown-layout-props)

A Markdown layout will have access to the following information via `Astro.props`:

* **`file`** - The absolute path of this file (e.g. `/home/user/projects/.../file.md`).

* **`url`** - The URL of the page (e.g. `/en/guides/markdown-content`).

* **`frontmatter`** - All frontmatter from the Markdown or MDX document.

  * **`frontmatter.file`** - The same as the top-level `file` property.
  * **`frontmatter.url`** - The same as the top-level `url` property.

* **`headings`** - A list of headings (`h1 -> h6`) in the Markdown or MDX document with associated metadata. This list follows the type: `{ depth: number; slug: string; text: string }[]`.

* **`rawContent()`** - A function that returns the raw Markdown document as a string.

* **`compiledContent()`** - An async function that returns the Markdown document compiled to an HTML string.

Note

A Markdown layout will have access to all the Markdown file’s [available properties](/en/guides/markdown-content/#available-properties) from `Astro.props` **with two key differences:**

* Heading information (i.e. `h1 -> h6` elements) is available via the `headings` array, rather than a `getHeadings()` function.

* `file` and `url` are *also* available as nested `frontmatter` properties (i.e. `frontmatter.url` and `frontmatter.file`).

### Importing Layouts Manually (MDX)

[Section titled “Importing Layouts Manually (MDX)”](#importing-layouts-manually-mdx)

You can also use the special Markdown layout property in the frontmatter of MDX files to pass `frontmatter` and `headings` props directly to a specified layout component in the same way.

To pass information to your MDX layout that does not (or cannot) exist in your frontmatter, you can instead import and use a `<Layout />` component. This works like any other Astro component, and will not receive any props automatically. Pass it any necessary props directly:

src/pages/posts/first-post.mdx

```diff
---
-layout: ../../layouts/BaseLayout.astro
title: 'My first MDX post'
publishDate: '21 September 2022'
---
+import BaseLayout from '../../layouts/BaseLayout.astro';


export function fancyJsHelper() {
  return "Try doing that with YAML!";
}


<BaseLayout title={frontmatter.title} fancyJsHelper={fancyJsHelper}>
  Welcome to my new Astro blog, using MDX!
</BaseLayout>
```

Then, your values are available to you through `Astro.props` in your layout, and your MDX content will be injected into the page where your `<slot />` component is written:

src/layouts/BaseLayout.astro

```astro
---
const { title, fancyJsHelper } = Astro.props;
---
<html>
  <head>
    <!-- -->
    <meta charset="utf-8">
  </head>
  <body>
    <!-- -->
    <h1>{title}</h1>
    <slot /> <!-- your content is injected here -->
    <p>{fancyJsHelper()}</p>
    <!-- -->
  </body>
</html>
```

When using any layout (either through the frontmatter `layout` property or by importing a layout), you must include the `<meta charset="utf-8">` tag in your layout as Astro will no longer add it automatically to your MDX page.

Learn more about Astro’s Markdown and MDX support in our [Markdown guide](/en/guides/markdown-content/).

## Nesting Layouts

[Section titled “Nesting Layouts”](#nesting-layouts)

Layout components do not need to contain an entire page worth of HTML. You can break your layouts into smaller components, and combine layout components to create even more flexible, page templates. This pattern is useful when you want to share some code across multiple layouts.

For example, a `BlogPostLayout.astro` layout component could style a post’s title, date and author. Then, a site-wide `BaseLayout.astro` could handle the rest of your page template, like navigation, footers, SEO meta tags, global styles, and fonts. You can also pass props received from your post to another layout, just like any other nested component.

src/layouts/BlogPostLayout.astro

```astro
---
import BaseLayout from './BaseLayout.astro';
const { frontmatter } = Astro.props;
---
<BaseLayout url={frontmatter.url}>
  <h1>{frontmatter.title}</h1>
  <h2>Post author: {frontmatter.author}</h2>
  <slot />
</BaseLayout>
```


---

# Scripts and event handling

> How to add client-side interactivity to Astro components using native browser JavaScript APIs.

You can send JavaScript to the browser and add functionality to your Astro components using `<script>` tags in the component template.

Scripts add interactivity to your site, such as handling events or updating content dynamically, without the need for a [UI framework](/en/guides/framework-components/) like React, Svelte, or Vue. This avoids the overhead of shipping framework JavaScript and doesn’t require you to know any additional framework to create a full-featured website or application.

## Client-Side Scripts

[Section titled “Client-Side Scripts”](#client-side-scripts)

Scripts can be used to add event listeners, send analytics data, play animations, and everything else JavaScript can do on the web.

Astro automatically enhances the HTML standard `<script>` tag with bundling, TypeScript, and more. See [how astro processes scripts](#script-processing) for more details.

src/components/ConfettiButton.astro

```astro
<button data-confetti-button>Celebrate!</button>


<script>
  // Import from npm package.
  import confetti from 'canvas-confetti';


  // Find our component DOM on the page.
  const buttons = document.querySelectorAll('[data-confetti-button]');


  // Add event listeners to fire confetti when a button is clicked.
  buttons.forEach((button) => {
    button.addEventListener('click', () => confetti());
  });
</script>
```

See [when your scripts will not be processed](#unprocessed-scripts) to troubleshoot script behavior, or to learn how to opt-out of this processing intentionally.

## Script processing

[Section titled “Script processing”](#script-processing)

By default, Astro processes `<script>` tags that contain no attributes (other than `src`) in the following ways:

* **TypeScript support:** All scripts are TypeScript by default.
* **Import bundling:** Import local files or npm modules, which will be bundled together.
* **Type Module:** Processed scripts become [`type="module"`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) automatically.
* **Deduplication:** If a component that contains a `<script>` is used multiple times on a page, the script will only be included once.
* **Automatic inlining:** If the script is small enough, Astro will inline it directly into the HTML to reduce the number of requests.

src/components/Example.astro

```astro
<script>
  // Processed! Bundled! TypeScript!
  // Importing local scripts and from npm packages works.
</script>
```

### Unprocessed scripts

[Section titled “Unprocessed scripts”](#unprocessed-scripts)

Astro will not process a `<script>` tag if it has any attribute other than `src`.

You can add the [`is:inline`](/en/reference/directives-reference/#isinline) directive to intentionally opt out of processing for a script.

src/components/InlineScript.astro

```astro
<script is:inline>
  // Will be rendered into the HTML exactly as written!
  // Not transformed: no TypeScript and no import resolution by Astro.
  // If used inside a component, this code is duplicated for each instance.
</script>
```

### Include JavaScript files on your page

[Section titled “Include JavaScript files on your page”](#include-javascript-files-on-your-page)

You may want to write your scripts as separate `.js`/`.ts` files or need to reference an external script on another server. You can do this by referencing these in a `<script>` tag’s `src` attribute.

#### Import local scripts

[Section titled “Import local scripts”](#import-local-scripts)

**When to use this:** when your script lives inside of `src/`.

Astro will process these scripts according to the [script processing rules](#script-processing).

src/components/LocalScripts.astro

```astro
<!-- relative path to script at `src/scripts/local.js` -->
<script src="../scripts/local.js"></script>


<!-- also works for local TypeScript files -->
<script src="./script-with-types.ts"></script>
```

#### Load external scripts

[Section titled “Load external scripts”](#load-external-scripts)

**When to use this:** when your JavaScript file lives inside of `public/` or on a CDN.

To load scripts outside of your project’s `src/` folder, include the `is:inline` directive. This approach skips the JavaScript processing, bundling, and optimizations that are provided by Astro when you import scripts as described above.

src/components/ExternalScripts.astro

```astro
<!-- absolute path to a script at `public/my-script.js` -->
<script is:inline src="/my-script.js"></script>


<!-- full URL to a script on a remote server -->
<script is:inline src="https://my-analytics.com/script.js"></script>
```

## Common script patterns

[Section titled “Common script patterns”](#common-script-patterns)

### Handle `onclick` and other events

[Section titled “Handle onclick and other events”](#handle-onclick-and-other-events)

Some UI frameworks use custom syntax for event handling like `onClick={...}` (React/Preact) or `@click="..."` (Vue). Astro follows standard HTML more closely and does not use custom syntax for events.

Instead, you can use [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) in a `<script>` tag to handle user interactions.

src/components/AlertButton.astro

```astro
<button class="alert">Click me!</button>


<script>
  // Find all buttons with the `alert` class on the page.
  const buttons = document.querySelectorAll('button.alert');


  // Handle clicks on each button.
  buttons.forEach((button) => {
    button.addEventListener('click', () => {
      alert('Button was clicked!');
    });
  });
</script>
```

If you have multiple `<AlertButton />` components on a page, Astro will not run the script multiple times. Scripts are bundled and only included once per page. Using `querySelectorAll` ensures that this script attaches the event listener to every button with the `alert` class found on the page.

### Web components with custom elements

[Section titled “Web components with custom elements”](#web-components-with-custom-elements)

You can create your own HTML elements with custom behavior using the Web Components standard. Defining a [custom element](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements) in a `.astro` component allows you to build interactive components without needing a UI framework library.

In this example, we define a new `<astro-heart>` HTML element that tracks how many times you click the heart button and updates the `<span>` with the latest count.

src/components/AstroHeart.astro

```astro
<!-- Wrap the component elements in our custom element “astro-heart”. -->
<astro-heart>
  <button aria-label="Heart">💜</button> × <span>0</span>
</astro-heart>


<script>
  // Define the behaviour for our new type of HTML element.
  class AstroHeart extends HTMLElement {
    connectedCallback() {
      let count = 0;


      const heartButton = this.querySelector('button');
      const countSpan = this.querySelector('span');


      // Each time the button is clicked, update the count.
      heartButton.addEventListener('click', () => {
        count++;
        countSpan.textContent = count.toString();
      });
    }
  }


  // Tell the browser to use our AstroHeart class for <astro-heart> elements.
  customElements.define('astro-heart', AstroHeart);
</script>
```

There are two advantages to using a custom element here:

1. Instead of searching the whole page using `document.querySelector()`, you can use `this.querySelector()`, which only searches within the current custom element instance. This makes it easier to work with only the children of one component instance at a time.

2. Although a `<script>` only runs once, the browser will run our custom element’s `connectedCallback()` method each time it finds `<astro-heart>` on the page. This means you can safely write code for one component at a time, even if you intend to use this component multiple times on a page.

You can learn more about custom elements in [web.dev’s Reusable Web Components guide](https://web.dev/custom-elements-v1/) and [MDN’s introduction to custom elements](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements).

### Pass frontmatter variables to scripts

[Section titled “Pass frontmatter variables to scripts”](#pass-frontmatter-variables-to-scripts)

In Astro components, the code in [the frontmatter](/en/basics/astro-components/#the-component-script) (between the `---` fences) runs on the server and is not available in the browser.

To pass server-side variables to client-side scripts, store them in [`data-*` attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes) on HTML elements. Scripts can then access these values using the `dataset` property.

In this example component, a `message` prop is stored in a `data-message` attribute, so the custom element can read `this.dataset.message` and get the value of the prop in the browser.

src/components/AstroGreet.astro

```astro
---
const { message = 'Welcome, world!' } = Astro.props;
---


<!-- Store the message prop as a data attribute. -->
<astro-greet data-message={message}>
  <button>Say hi!</button>
</astro-greet>


<script>
  class AstroGreet extends HTMLElement {
    connectedCallback() {
      // Read the message from the data attribute.
      const message = this.dataset.message;
      const button = this.querySelector('button');
      button.addEventListener('click', () => {
        alert(message);
      });
    }
  }


  customElements.define('astro-greet', AstroGreet);
</script>
```

Now we can use our component multiple times and be greeted by a different message for each one.

src/pages/example.astro

```astro
---
import AstroGreet from '../components/AstroGreet.astro';
---


<!-- Use the default message: “Welcome, world!” -->
<AstroGreet />


<!-- Use custom messages passed as a props. -->
<AstroGreet message="Lovely day to build components!" />
<AstroGreet message="Glad you made it! 👋" />
```

Did you know?

This is actually what Astro does behind the scenes when you pass props to a component written using a UI framework like React! For components with a `client:*` directive, Astro creates an `<astro-island>` custom element with a `props` attribute that stores your server-side props in the HTML output.

### Combining scripts and UI Frameworks

[Section titled “Combining scripts and UI Frameworks”](#combining-scripts-and-ui-frameworks)

Elements rendered by a UI framework may not be available yet when a `<script>` tag executes. If your script also needs to handle [UI framework components](/en/guides/framework-components/), using a custom element is recommended.


---

# Dev toolbar

> A guide to using the dev toolbar in Astro

While the dev server is running, Astro includes a dev toolbar at the bottom of every page in your local browser preview.

This toolbar includes a number of useful tools for debugging and inspecting your site during development and can be [extended with more dev toolbar apps](#extending-the-dev-toolbar) found in the integrations directory. You can even [build your own toolbar apps](/en/recipes/making-toolbar-apps/) using the [Dev Toolbar API](/en/reference/dev-toolbar-app-reference/)!

This toolbar is enabled by default and appears when you hover over the bottom of the page. It is a development tool only and will not appear on your published site.

## Built-in apps

[Section titled “Built-in apps”](#built-in-apps)

### Astro Menu

[Section titled “Astro Menu”](#astro-menu)

The Astro Menu app provides easy access to various information about the current project and links to extra resources. Notably, it provides one-click access to the Astro documentation, GitHub repository, and Discord server.

This app also includes a “Copy debug info” button which will run the [`astro info`](/en/reference/cli-reference/#astro-info) command and copy the output to your clipboard. This can be useful when asking for help or reporting issues.

### Inspect

[Section titled “Inspect”](#inspect)

The Inspect app provides information about any [islands](/en/concepts/islands/) on the current page. This will show you the properties passed to each island, and the client directive that is being used to render them.

### Audit

[Section titled “Audit”](#audit)

The Audit app automatically runs a series of audits on the current page, checking for the most common performance and accessibility issues. When an issue is found, a red dot will appear in the toolbar. Clicking on the app will pop up a list of results from the audit and will highlight the related elements directly in the page.

Note

The basic performance and accessibility audits performed by the dev toolbar are not a replacement for dedicated tools like [Pa11y](https://pa11y.org/) or [Lighthouse](https://developers.google.com/web/tools/lighthouse), or even better, humans!

The dev toolbar aims to provide a quick and easy way to catch common issues during development, without needing to context-switch to a different tool.

### Settings

[Section titled “Settings”](#settings)

The Settings app allows you to configure options for the dev toolbar, such as verbose logging, disabling notifications, and adjusting its placement on your screen.

## Extending the dev toolbar

[Section titled “Extending the dev toolbar”](#extending-the-dev-toolbar)

Astro integrations can add new apps to the dev toolbar, allowing you to extend it with custom tools that are specific to your project. You can find [more dev tool apps to install in the integrations directory](https://astro.build/integrations/?search=\&categories%5B%5D=toolbar) or using the [Astro Menu](#astro-menu).

Install additional dev toolbar app integrations in your project just like any other [Astro integration](/en/guides/integrations/) according to its own installation instructions.

![](/houston_chef.webp) **Related recipe:** [Create a dev toolbar app](/en/recipes/making-toolbar-apps/)

## Disabling the dev toolbar

[Section titled “Disabling the dev toolbar”](#disabling-the-dev-toolbar)

The dev toolbar is enabled by default for every site. You can choose to disable it for individual projects and/or users as needed.

### Per-project

[Section titled “Per-project”](#per-project)

To disable the dev toolbar for everyone working on a project, set `devToolbar: false` in the [Astro config file](/en/reference/configuration-reference/#devtoolbarenabled).

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
+  devToolbar: {
+    enabled: false
+  }
});
```

To enable the dev toolbar again, remove these lines from your configuration, or set `enabled: true`.

### Per-user

[Section titled “Per-user”](#per-user)

To disable the dev toolbar for yourself on a specific project, run the [`astro preferences`](/en/reference/cli-reference/#astro-preferences) command.

```shell
astro preferences disable devToolbar
```

To disable the dev toolbar in all Astro projects for a user on the current machine, add the `--global` flag when running `astro-preferences`:

```shell
astro preferences disable --global devToolbar
```

The dev toolbar can later be enabled with:

```shell
astro preferences enable devToolbar
```


---

# Images

> Learn how to use images in Astro.

Astro provides several ways for you to use images on your site, whether they are stored locally inside your project, linked to from an external URL, or managed in a CMS or CDN.

Astro provides built-in [`<Image />`](#image-) and [`<Picture />`](#picture-) Astro components, [Markdown image syntax](#images-in-markdown-files) (`![]()`) processing, [SVG components](#svg-components), and [an image generating function](#generating-images-with-getimage) to optimize and/or transform your images. Additionally, you can configure [automatically resizing responsive images](#responsive-image-behavior) by default, or set responsive properties on individual image and picture components.

You can always choose to use images and SVG files using native HTML elements in `.astro` or Markdown files, or the standard way for your file type (e.g. `<img />` in MDX and JSX). However, Astro does not perform any processing or optimization of these images.

There is also no native video support in Astro, and we recommend choosing a [hosted video service](/en/guides/media/) to handle the demands of optimizing and streaming video content.

See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

## Where to store images

[Section titled “Where to store images”](#where-to-store-images)

### `src/` vs `public/`

[Section titled “src/ vs public/”](#src-vs-public)

We recommend that local images are kept in `src/` when possible so that Astro can transform, optimize, and bundle them. Files in the `public/` directory are always served or copied into the build folder as-is, with no processing.

Your local images stored in `src/` can be used by all files in your project: `.astro`, `.md`, `.mdx`, `.mdoc`, and other UI frameworks as file imports. Images can be stored in any folder, including alongside your content.

Store your images in the `public/` folder if you want to avoid any processing. These images are available to your project files as URL paths on your domain and allow you to have a direct public link to them. For example, your site favicon will commonly be placed in the root of this folder where browsers can identify it.

### Remote images

[Section titled “Remote images”](#remote-images)

You can also choose to store your images remotely, in a [content management system (CMS)](/en/guides/cms/) or [digital asset management (DAM)](/en/guides/media/) platform. Astro can fetch your data remotely using APIs or display images from their full URL path.

For extra protection when dealing with external sources, Astro’s image components and helper function will only process (e.g. optimize, transform) images from [authorized image sources specified in your configuration](#authorizing-remote-images). Remote images from other sources will be displayed with no processing.

## Images in `.astro` files

[Section titled “Images in .astro files”](#images-in-astro-files)

**Options:** `<Image />`, `<Picture />`, `<img>`, `<svg>`, SVG components

Astro’s templating language allows you to render optimized images with the Astro [`<Image />`](/en/reference/modules/astro-assets/#image-) component and generate multiple sizes and formats with the Astro [`<Picture />`](/en/reference/modules/astro-assets/#picture-) component. Both components also accept [responsive image properties](#responsive-image-behavior) for resizing based on container size and responding to device screen size and resolution.

Additionally, you can import and use [SVG files as Astro components](#svg-components) in `.astro` components.

All native HTML tags, including `<img>` and `<svg>`, are also available in `.astro` components. [Images rendered with HTML tags](#display-unprocessed-images-with-the-html-img-tag) will not be processed (e.g. optimized, transformed) and will be copied into your build folder as-is.

For all images in `.astro` files, **the value of the image `src` attribute is determined by the location of your image file**:

* A local image from your project `src/` folder uses an import from the file’s relative path.

  The image and picture components use the named import directly (e.g. `src={rocket}`), while the `<img>` tag uses the `src` object property of the import (e.g. `src={rocket.src}`).

* Remote and `public/` images use a URL path.

  Provide a full URL for remote images (e.g. `src="https://www.example.com/images/my-remote-image.jpg"`), or a relative URL path on your site that corresponds to your file’s location in your `public/` folder (e.g. `src="/images/my-public-image.jpg"` for an image located in `public/images/my-public-image.jpg`).

src/pages/blog/my-images.astro

```astro
---
import { Image } from 'astro:assets';
import localBirdImage from '../../images/subfolder/localBirdImage.png';
---
<Image src={localBirdImage} alt="A bird sitting on a nest of eggs." />
<Image src="/images/bird-in-public-folder.jpg" alt="A bird." width="50" height="50" />
<Image src="https://example.com/remote-bird.jpg" alt="A bird." width="50" height="50" />


<img src={localBirdImage.src} alt="A bird sitting on a nest of eggs.">
<img src="/images/bird-in-public-folder.jpg" alt="A bird.">
<img src="https://example.com/remote-bird.jpg" alt="A bird.">
```

See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components including required and optional properties.

![](/houston_chef.webp) **Related recipe:** [Dynamically import images](/en/recipes/dynamically-importing-images/)

## Images in Markdown files

[Section titled “Images in Markdown files”](#images-in-markdown-files)

**Options:** `![]()`, `<img>` (with public or remote images)

Use standard Markdown `![alt](src)` syntax in your `.md` files. Your local images stored in `src/` and remote images will be processed and optimized. When you [configure responsive images globally](/en/reference/configuration-reference/#imagelayout), these images will also be [responsive](#responsive-image-behavior).

Images stored in the `public/` folder are never optimized.

src/pages/post-1.md

```md
# My Markdown Page


<!-- Local image stored in src/assets/ -->
<!-- Use a relative file path or import alias -->
![A starry night sky.](../assets/stars.png)


<!-- Image stored in public/images/ -->
<!-- Use the file path relative to public/ -->
![A starry night sky.](/images/stars.png)


<!-- Remote image on another server -->
<!-- Use the full URL of the image -->
![Astro](https://example.com/images/remote-image.png)
```

The HTML `<img>` tag can also be used to display images stored in `public/` or remote images without any image optimization or processing. However, `<img>` is not supported for your local images in `src`.

The `<Image />` and `<Picture />` components are unavailable in `.md` files. If you require more control over your image attributes, we recommend using [Astro’s MDX integration](/en/guides/integrations-guide/mdx/) to add support for `.mdx` file format. MDX allows additional [image options available in MDX](#images-in-mdx-files), including combining components with Markdown syntax.

## Images in MDX files

[Section titled “Images in MDX files”](#images-in-mdx-files)

**Options:** `<Image />`, `<Picture />`, `<img />`, `![]()`, SVG components

You can use Astro’s `<Image />` and `<Picture />` components in your `.mdx` files by importing both the component and your image. Use them just as they are [used in `.astro` files](#images-in-astro-files). The JSX `<img />` tag is also supported for unprocessed images and [uses the same image import as the HTML `<img>` tag](#display-unprocessed-images-with-the-html-img-tag).

Additionally, there is support for [standard Markdown `![alt](src)` syntax](#images-in-markdown-files) with no import required.

src/pages/post-1.mdx

```mdx
---
title: My Page title
---
import { Image } from 'astro:assets';
import rocket from '../assets/rocket.png';


# My MDX Page


// Local image stored in the the same folder
![Houston in the wild](houston.png)


// Local image stored in src/assets/
<Image src={rocket} alt="A rocketship in space." />
<img src={rocket.src} alt="A rocketship in space." />
![A rocketship in space](../assets/rocket.png)


// Image stored in public/images/
<Image src="/images/stars.png" alt="A starry night sky." />
<img src="/images/stars.png" alt="A starry night sky." />
![A starry night sky.](/images/stars.png)


// Remote image on another server
<Image src="https://example.com/images/remote-image.png" />
<img src="https://example.com/images/remote-image.png" />
![Astro](https://example.com/images/remote-image.png)
```

See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

## Images in UI framework components

[Section titled “Images in UI framework components”](#images-in-ui-framework-components)

**Image options:** the framework’s own image syntax (e.g. `<img />` in JSX, `<img>` in Svelte)

[Local images must first be imported](#display-unprocessed-images-with-the-html-img-tag) to access their image properties such as `src`. Then, they can be rendered as you normally would in that framework’s own image syntax:

src/components/ReactImage.jsx

```jsx
import stars from "../assets/stars.png";


export default function ReactImage() {
  return (
    <img src={stars.src} alt="A starry night sky." />
  )
}
```

src/components/SvelteImage.svelte

```svelte
<script>
  import stars from '../assets/stars.png';
</script>


<img src={stars.src} alt="A starry night sky." />
```

Astro components (e.g. `<Image />`, `<Picture />`, SVG components) are unavailable inside UI framework components because [a client island must contain only valid code for its own framework](/en/guides/framework-components/#can-i-use-astro-components-inside-my-framework-components).

But, you can pass the static content generated by these components to a framework component inside a `.astro` file [as children](/en/guides/framework-components/#passing-children-to-framework-components) or using a [named `<slot/>`](/en/guides/framework-components/#can-i-use-astro-components-inside-my-framework-components):

src/components/ImageWrapper.astro

```astro
---
import ReactComponent from './ReactComponent.jsx';
import { Image } from 'astro:assets';
import stars from '~/stars/docline.png';
---


<ReactComponent>
  <Image src={stars} alt="A starry night sky." />
</ReactComponent>
```

## Astro components for images

[Section titled “Astro components for images”](#astro-components-for-images)

Astro provides two built-in Astro components for images (`<Image />` and `<Picture />`) and also allows you to import SVG files and use them as Astro components. These components may be used in any files that can import and render `.astro` components.

### `<Image />`

[Section titled “\<Image />”](#image-)

Use the built-in `<Image />` Astro component to display optimized versions of:

* your local images located within the `src/` folder
* [configured remote images](#authorizing-remote-images) from authorized sources

`<Image />` can transform a local or authorized remote image’s dimensions, file type, and quality for control over your displayed image. This transformation happens at build time for prerendered pages. When your page is rendered on demand, this transformation will occur on the fly when the page is viewed. The resulting `<img>` tag includes `alt`, `loading`, and `decoding` attributes and infers image dimensions to avoid Cumulative Layout Shift (CLS).

What is Cumulative Layout Shift?

[Cumulative Layout Shift (CLS)](https://web.dev/cls/) is a Core Web Vital metric for measuring how much content shifted on your page during loading. The `<Image />` component optimizes for CLS by automatically setting the correct `width` and `height` for your images.

src/components/MyComponent.astro

```astro
---
// import the Image component and the image
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png'; // Image is 1600x900
---


<!-- `alt` is mandatory on the Image component -->
<Image src={myImage} alt="A description of my image." />
```

```html
<!-- Prerendered output -->
<!-- Image is optimized, proper attributes are enforced -->
<img
  src="/_astro/my_image.hash.webp"
  width="1600"
  height="900"
  decoding="async"
  loading="lazy"
  alt="A description of my image."
/>


<!-- Output rendered on demand-->
<!-- src will use an endpoint generated on demand-->
<img
  src="/_image?href=%2F_astro%2Fmy_image.hash.webp&amp;w=1600&amp;h=900&amp;f=webp"
  <!-- ... -->
/>
```

The `<Image />` component accepts [several component properties](/en/reference/modules/astro-assets/#image-) as well as any attributes accepted by the HTML `<img>` tag.

The following example provides a `class` to the image component which will apply to the final `<img>` element.

src/pages/index.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---


<!-- `alt` is mandatory on the Image component -->
<Image src={myImage} alt="" class="my-class" />
```

```html
<!-- Prerendered output -->
<img
  src="/_astro/my_image.hash.webp"
  width="1600"
  height="900"
  decoding="async"
  loading="lazy"
  class="my-class"
  alt=""
/>
```

Tip

You can also use the `<Image />` component for images in the `public/` folder, or remote images not specifically configured in your project, even though these images will not be optimized or processed. The resulting image will be the same as using the HTML `<img>`.

However, using the image component for all images provides a consistent authoring experience and prevents Cumulative Layout Shift (CLS) even for your unoptimized images.

### `<Picture />`

[Section titled “\<Picture />”](#picture-)

**Added in:** `astro@3.3.0`

Use the built-in `<Picture />` Astro component to generate a `<picture>` tag with multiple formats and/or sizes of your image. This allows you to specify preferred file formats to display and at the same time, provide a fallback format. Like the [`<Image />` component](#image-), images will be processed at build time for prerendered pages. When your page is rendered on demand, processing will occur on the fly when the page is viewed.

The following example uses the `<Picture />` component to transform a local `.png` file into a web-friendly `avif` and `webp` format as well as the `.png` `<img>` that can be displayed as a fallback when needed:

src/pages/index.astro

```astro
---
import { Picture } from 'astro:assets';
import myImage from '../assets/my_image.png'; // Image is 1600x900
---


<!-- `alt` is mandatory on the Picture component -->
<Picture src={myImage} formats={['avif', 'webp']} alt="A description of my image." />
```

```html
<!-- Prerendered output -->
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

See details about [the `<Picture />` component properties](/en/reference/modules/astro-assets/#picture-) in the `astro:assets` reference.

### Responsive image behavior

[Section titled “Responsive image behavior”](#responsive-image-behavior)

**Added in:** `astro@5.10.0`

Responsive images are images that adjust to improve performance across different devices. These images can resize to fit their container, and can be served in different sizes depending on your visitor’s screen size and resolution.

With the [layout property](/en/reference/modules/astro-assets/#layout) applied to the `<Image />` or `<Picture />` components, Astro will automatically generate the required `srcset` and `sizes` values for your images, and apply the necessary [styles to ensure they resize correctly](#responsive-image-styles).

When this responsive behavior is [configured globally with `image.layout`](/en/reference/configuration-reference/#imagelayout), it will apply to all image components and also to any local and remote images using [the Markdown `![]()` syntax](/en/guides/images/#images-in-markdown-files).

Images in your `public/` folder are never optimized, and responsive images are not supported.

Note

A single responsive image will generate multiple images of different sizes so that the browser can show the best one to your visitor.

For prerendered pages, this happens during the build and may increase the build time of your project, especially if you have a large number of images.

For pages rendered on-demand, the images are generated as-needed when a page is visited. This has no impact on build times but may increase the number of image transformations performed when an image is displayed. Depending on your image service this may incur additional costs.

Read more about [responsive images on MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Responsive_images).

#### Generated HTML output for responsive images

[Section titled “Generated HTML output for responsive images”](#generated-html-output-for-responsive-images)

When a layout is set, either by default or on an individual component, images have automatically generated `srcset` and `sizes` attributes based on the image’s dimensions and the layout type. Images with `constrained` and `full-width` layouts will have styles applied to ensure they resize according to their container.

src/components/MyComponent.astro

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my_image.png';
---
<Image src={myImage} alt="A description of my image." layout='constrained' width={800} height={600} />
```

This `<Image />` component will generate the following HTML output on a prerendered page:

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

#### Responsive image styles

[Section titled “Responsive image styles”](#responsive-image-styles)

Setting [`image.responsiveStyles: true`](/en/reference/configuration-reference/#imageresponsivestyles) applies a small number of global styles to ensure that your images resize correctly. In most cases, you will want to enable these as a default; your images will not be responsive without additional styles.

However, if you prefer to handle responsive image styling yourself, or need to [override these defaults when using Tailwind 4](#responsive-images-with-tailwind-4), leave the default `false` value configured.

The global styles applied by Astro will depend on the layout type, and are designed to produce the best result for the generated `srcset` and `sizes` attributes. These are the default styles:

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

The styles use the [`:where()` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/:where), which has a [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity) of 0, meaning that it is easy to override with your own styles. Any CSS selector will have a higher specificity than `:where()`, so you can easily override the styles by adding your own styles to target the image.

You can override the `object-fit` and `object-position` styles on a per-image basis by setting the `fit` and `position` props on the `<Image />` or `<Picture />` component.

#### Responsive images with Tailwind 4

[Section titled “Responsive images with Tailwind 4”](#responsive-images-with-tailwind-4)

Tailwind 4 is compatible with Astro’s default responsive styles. However, Tailwind uses [cascade layers](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer), meaning that its rules are always lower specificity than rules that don’t use layers, including Astro’s responsive styles. Therefore, Astro’s styling will take precedence over Tailwind styling. To use Tailwind rules instead of Astro’s default styling, do not enable [Astro’s default responsive styles](/en/reference/configuration-reference/#imageresponsivestyles).

### SVG components

[Section titled “SVG components”](#svg-components)

**Added in:** `astro@5.7.0`

Astro allows you to import SVG files and use them as Astro components. Astro will inline the SVG content into your HTML output.

Reference the default import of any local `.svg` file. Since this import is treated as an Astro component, you must use the same conventions (e.g. capitalization) as when [using dynamic tags](/en/reference/astro-syntax/#dynamic-tags).

src/components/MyAstroComponent.astro

```astro
---
import Logo from './path/to/svg/file.svg';
---


<Logo />
```

Your SVG component, like `<Image />` or any other Astro component, is unavailable inside UI framework components, but can [be passed to a framework component](#images-in-ui-framework-components) inside a `.astro` component.

#### SVG component attributes

[Section titled “SVG component attributes”](#svg-component-attributes)

You can pass props such as `width`, `height`, `fill`, `stroke`, and any other attribute accepted by the [native `<svg>` element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/svg). These attributes will automatically be applied to the underlying `<svg>` element. If a property is present in the original `.svg` file and is passed to the component, the value passed to the component will override the original value.

src/components/MyAstroComponent.astro

```astro
---
import Logo from '../assets/logo.svg';
---


<Logo width={64} height={64} fill="currentColor" />
```

#### `SvgComponent` Type

[Section titled “SvgComponent Type”](#svgcomponent-type)

**Added in:** `astro@5.14.0`

You can also enforce type safety for your `.svg` assets using the `SvgComponent` type:

src/components/Logo.astro

```astro
---
import type { SvgComponent } from "astro/types";
import HomeIcon from "./Home.svg";


interface Link {
  url: string;
  text: string;
  icon: SvgComponent;
}


const links: Link[] = [
  {
    url: "/",
    text: "Home",
    icon: HomeIcon,
  },
];
---
```

### Creating custom image components

[Section titled “Creating custom image components”](#creating-custom-image-components)

You can create a custom, reusable image component by wrapping the `<Image />` or `<Picture/>` component in another Astro component. This allows you to set default attributes and styles only once.

For example, you could create a component for your blog post images that receives attributes as props and applies consistent styles to each image:

src/components/BlogPostImage.astro

```astro
---
import { Image } from 'astro:assets';


const { src, ...attrs } = Astro.props;
---
<Image src={src} {...attrs} />


<style>
  img {
    margin-block: 2.5rem;
    border-radius: 0.75rem;
  }
</style>
```

## Display unprocessed images with the HTML `<img>` tag

[Section titled “Display unprocessed images with the HTML \<img> tag”](#display-unprocessed-images-with-the-html-img-tag)

The [Astro template syntax](/en/reference/astro-syntax/) also supports writing an `<img>` tag directly, with full control over its final output. These images will not be processed and optimized. It accepts all HTML `<img>` tag properties, and the only required property is `src`. However, it is strongly recommended to include [the `alt` property for accessibility](#alt-text).

### images in `src/`

[Section titled “images in src/”](#images-in-src)

Local images must be imported from the relative path from the existing `.astro` file, or you can configure and use an [import alias](/en/guides/imports/#aliases). Then, you can access the image’s `src` and other properties to use in the `<img>` tag.

Imported image assets match the [`ImageMetadata` type](/en/reference/modules/astro-assets/#imagemetadata-1) and have the following signature:

```ts
interface ImageMetadata {
  src: string;
  width: number;
  height: number;
  format: string;
}
```

The following example uses the image’s own `height` and `width` properties to avoid Cumulative Layout Shift (CLS) and improve Core Web Vitals:

src/pages/posts/post-1.astro

```astro
---
// import local images
import myDog from '../../images/pets/local-dog.jpg';
---
// access the image properties
<img src={myDog.src} width={myDog.width} height={myDog.height} alt="A barking dog." />
```

### Images in `public/`

[Section titled “Images in public/”](#images-in-public)

For images located within `public/` use the image’s file path relative to the public folder as the `src` value:

```astro
<img src="/images/public-cat.jpg" alt="A sleeping cat." >
```

### Remote images

[Section titled “Remote images”](#remote-images-1)

For remote images, use the image’s full URL as the `src` value:

```astro
<img src="https://example.com/remote-cat.jpg" alt="A sleeping cat." >
```

### Choosing `<Image />` vs `<img>`

[Section titled “Choosing \<Image /> vs \<img>”](#choosing-image--vs-img)

The `<Image />` component optimizes your image and infers width and height (for images it can process) based on the original aspect ratio to avoid CLS. It is the preferred way to use images in `.astro` files whenever possible.

Use the HTML `<img>` element when you cannot use the `<Image />` component, for example:

* for unsupported image formats
* when you do not want your image optimized by Astro
* to access and change the `src` attribute dynamically client-side

## Using Images from a CMS or CDN

[Section titled “Using Images from a CMS or CDN”](#using-images-from-a-cms-or-cdn)

Image CDNs work with [all Astro image options](#images-in-astro-files). Use an image’s full URL as the `src` attribute in the `<Image />` component, an `<img>` tag, or in Markdown notation. For image optimization with remote images, also [configure your authorized domains or URL patterns](#authorizing-remote-images).

Alternatively, the CDN may provide its own SDKs to more easily integrate in an Astro project. For example, Cloudinary supports an [Astro SDK](https://astro.cloudinary.dev/) which allows you to easily drop in images with their `CldImage` component or a [Node.js SDK](https://cloudinary.com/documentation/node_integration) that can generate URLs to use with an `<img>` tag in a Node.js environment.

See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

## Authorizing remote images

[Section titled “Authorizing remote images”](#authorizing-remote-images)

You can configure lists of authorized image source URL domains and patterns for image optimization using [`image.domains`](/en/reference/configuration-reference/#imagedomains) and [`image.remotePatterns`](/en/reference/configuration-reference/#imageremotepatterns). This configuration is an extra layer of safety to protect your site when showing images from an external source.

Remote images from other sources will not be optimized, but using the `<Image />` component for these images will prevent Cumulative Layout Shift (CLS).

For example, the following configuration will only allow remote images from `astro.build` to be optimized:

astro.config.mjs

```ts
export default defineConfig({
  image: {
    domains: ["astro.build"],
  }
});
```

The following configuration will only allow remote images from HTTPS hosts:

astro.config.mjs

```ts
export default defineConfig({
  image: {
    remotePatterns: [{ protocol: "https" }],
  }
});
```

## Images in content collections

[Section titled “Images in content collections”](#images-in-content-collections)

You can declare an associated image for a content collections entry, such as a blog post’s cover image, in your frontmatter using its path relative to the current folder:

src/content/blog/my-post.md

```md
---
title: "My first blog post"
cover: "./firstpostcover.jpeg" # will resolve to "src/content/blog/firstpostcover.jpeg"
coverAlt: "A photograph of a sunset behind a mountain range."
---


This is a blog post
```

The `image` helper for the content collections schema lets you validate and import the image.

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';


const blogCollection = defineCollection({
  schema: ({ image }) => z.object({
    title: z.string(),
    cover: image(),
    coverAlt: z.string(),
  }),
});


export const collections = {
  blog: blogCollection,
};
```

The image will be imported and transformed into metadata, allowing you to pass it as a `src` to `<Image/>`, `<img>`, or `getImage()` in an Astro component.

The example below shows a blog index page that renders the cover photo and title of each blog post from the previous schema:

src/pages/blog.astro

```astro
---
import { Image } from "astro:assets";
import { getCollection } from "astro:content";
const allBlogPosts = await getCollection("blog");
---


{
  allBlogPosts.map((post) => (
    <div>
      <Image src={post.data.cover} alt={post.data.coverAlt} />
      <h2>
        <a href={"/blog/" + post.id}>{post.data.title}</a>
      </h2>
    </div>
  ))
}
```

## Generating images with `getImage()`

[Section titled “Generating images with getImage()”](#generating-images-with-getimage)

The `getImage()` function is intended for generating images destined to be used somewhere else than directly in HTML, for example in an [API Route](/en/guides/endpoints/#server-endpoints-api-routes). When you need options that the `<Picture>` and `<Image>` components do not currently support, you can use the `getImage()` function to create your own custom `<Image />` component.

`getImage()` can only be used on the server. If you need to use the resulting image URL on the client (e.g. in a client-side script or framework component), call `getImage()` inside the frontmatter and pass the resulting `src` to the client:

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

See more in the [`getImage()` reference](/en/reference/modules/astro-assets/#getimage).

![](/houston_chef.webp) **Related recipe:** [Build a custom image component](/en/recipes/build-custom-img-component/)

## Alt Text

[Section titled “Alt Text”](#alt-text)

Not all users can see images in the same way, so accessibility is an especially important concern when using images. Use the `alt` attribute to provide [descriptive alt text](https://www.w3.org/WAI/tutorials/images/) for images.

This attribute is required for both the `<Image />` and `<Picture />` components. If no alt text is provided, a helpful error message will be provided reminding you to include the `alt` attribute.

If the image is merely decorative (i.e. doesn’t contribute to the understanding of the page), set `alt=""` so that screen readers know to ignore the image.

## Default image service

[Section titled “Default image service”](#default-image-service)

[Sharp](https://github.com/lovell/sharp) is the default image service used for `astro:assets`. You can further configure the image service using the [`image.service`](/en/reference/configuration-reference/#imageservice) option.

Note

When using a [strict package manager](https://pnpm.io/pnpm-vs-npm#npms-flat-tree) like `pnpm`, you may need to manually install Sharp into your project even though it is an Astro dependency:

```bash
pnpm add sharp
```

### Configure no-op passthrough service

[Section titled “Configure no-op passthrough service”](#configure-no-op-passthrough-service)

If your [adapter](https://astro.build/integrations/?search=\&categories%5B%5D=adapters) does not support Astro’s built-in Sharp image optimization (e.g. Cloudflare), you can configure a no-op image service to allow you to use the `<Image />` and `<Picture />` components. Note that Astro does not perform any image transformation and processing in these environments. However, you can still enjoy the other benefits of using `astro:assets`, including no Cumulative Layout Shift (CLS), the enforced `alt` attribute, and a consistent authoring experience.

Configure the `passthroughImageService()` to avoid Sharp image processing:

astro.config.mjs

```diff
import { defineConfig, passthroughImageService } from 'astro/config';


export default defineConfig({
+  image: {
+    service: passthroughImageService()
+  }
});
```

## Asset Caching

[Section titled “Asset Caching”](#asset-caching)

Astro stores processed image assets in a cache directory during site builds for both local and [remote images from authorized sources](#authorizing-remote-images). By preserving the cache directory between builds, processed assets are reused, improving build time and bandwidth usage.

The default cache directory is `./node_modules/.astro`, however this can be changed using the [`cacheDir`](/en/reference/configuration-reference/#cachedir) configuration setting.

### Remote Images

[Section titled “Remote Images”](#remote-images-2)

Remote images in the asset cache are managed based on [HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching), and respect the [Cache-Control header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) returned by the remote server. Images are cached if the Cache-Control header allows, and will be used until they are no longer [fresh](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh_and_stale_based_on_age).

#### Revalidation

[Section titled “Revalidation”](#revalidation)

**Added in:** `astro@5.1.0`

[Revalidation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#validation) reduces bandwidth usage and build time by checking with the remote server whether an expired cached image is still up-to-date. If the server indicates that the image is still fresh, the cached version is reused, otherwise the image is redownloaded.

Revalidation requires that the remote server send [Last-Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Last-Modified) and/or [Etag (entity tag)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) headers with its responses. This feature is available for remote servers that support the [If-Modified-Since](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since) and [If-None-Match](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None-Match) headers.

## Community Integrations

[Section titled “Community Integrations”](#community-integrations)

There are several third-party [community image integrations](https://astro.build/integrations?search=images) for optimizing and working with images in your Astro project.


---

# Markdown in Astro

> Learn about Astro's built-in support for Markdown.

[Markdown](https://daringfireball.net/projects/markdown/) is commonly used to author text-heavy content like blog posts and documentation. Astro includes built-in support for Markdown files that can also include [frontmatter YAML](https://dev.to/paulasantamaria/introduction-to-yaml-125f) (or [TOML](https://toml.io)) to define custom properties such as a title, description, and tags.

In Astro, you can author content in [GitHub Flavored Markdown](https://github.github.com/gfm/), then render it in `.astro` components. This combines a familiar writing format designed for content with the flexibility of Astro’s component syntax and architecture.

Tip

For additional functionality, such as including components and JSX expressions in Markdown, add the [`@astrojs/mdx` integration](/en/guides/integrations-guide/mdx/) to write your Markdown content using [MDX](https://mdxjs.com/).

## Organizing Markdown files

[Section titled “Organizing Markdown files”](#organizing-markdown-files)

Your local Markdown files can be kept anywhere within your `src/` directory. Markdown files located within `src/pages/` will automatically generate [Markdown pages on your site](#individual-markdown-pages).

Your Markdown content and frontmatter properties are available to use in components through [local file imports](#importing-markdown) or when [queried and rendered from data fetched by a content collections helper function](#markdown-from-content-collections-queries).

### File imports vs content collections queries

[Section titled “File imports vs content collections queries”](#file-imports-vs-content-collections-queries)

Local Markdown can be imported into `.astro` components using an `import` statement for a single file and [Vite’s `import.meta.glob()`](/en/guides/imports/#importmetaglob) to query multiple files at once. The [exported data from these Markdown files](#importing-markdown) can then be used in the `.astro` component.

If you have groups of related Markdown files, consider [defining them as collections](/en/guides/content-collections/). This gives you several advantages, including the ability to store Markdown files anywhere on your filesystem or remotely.

Collections use content-specific, optimized APIs for [querying and rendering your Markdown content](#markdown-from-content-collections-queries) instead of file imports. Collections are intended for sets of data that share the same structure, such as blog posts or product items. When you define that shape in a schema, you additionally get validation, type safety, and Intellisense in your editor.

See more about [when to use content collections](/en/guides/content-collections/#when-to-create-a-collection) instead of file imports.

## Dynamic JSX-like expressions

[Section titled “Dynamic JSX-like expressions”](#dynamic-jsx-like-expressions)

After importing or querying Markdown files, you can write dynamic HTML templates in your `.astro` components that include frontmatter data and body content.

src/pages/posts/great-post.md

```md
---
title: 'The greatest post of all time'
author: 'Ben'
---


Here is my _great_ post!
```

src/pages/my-posts.astro

```astro
---
import * as greatPost from './posts/great-post.md';
const posts = Object.values(import.meta.glob('./posts/*.md', { eager: true }));
---


<p>{greatPost.frontmatter.title}</p>
<p>Written by: {greatPost.frontmatter.author}</p>


{greatPost.compiledContent()}


<p>Post Archive:</p>
<ul>
  {posts.map(post => <li><a href={post.url}>{post.frontmatter.title}</a></li>)}
</ul>
```

### Available Properties

[Section titled “Available Properties”](#available-properties)

#### Markdown from content collections queries

[Section titled “Markdown from content collections queries”](#markdown-from-content-collections-queries)

When fetching data from your collections with the helper functions `getCollection()` or `getEntry()`, your Markdown’s frontmatter properties are available on a `data` object (e.g. `post.data.title`). Additionally, `body` contains the raw, uncompiled body content as a string.

The [`render()`](/en/reference/modules/astro-content/#render) function returns your Markdown body content, a generated list of headings, as well as a modified frontmatter object after any remark or rehype plugins have been applied.

Read more about [using content returned by a collections query](/en/guides/content-collections/#using-content-in-astro-templates).

#### Importing Markdown

[Section titled “Importing Markdown”](#importing-markdown)

The following exported properties are available in your `.astro` component when importing Markdown using `import` or `import.meta.glob()`:

* **`file`** - The absolute file path (e.g. `/home/user/projects/.../file.md`).
* **`url`** - The URL of the page (e.g. `/en/guides/markdown-content`).
* **`frontmatter`** - Contains any data specified in the file’s YAML (or TOML) frontmatter.
* **`<Content />`** - A component that returns the full, rendered contents of the file.
* **`rawContent()`** - A function that returns the raw Markdown document as a string.
* **`compiledContent()`** - An async function that returns the Markdown document compiled to an HTML string.
* **`getHeadings()`** - An async function that returns an array of all headings (`<h1>` to `<h6>`) in the file with the type: `{ depth: number; slug: string; text: string }[]`. Each heading’s `slug` corresponds to the generated ID for a given heading and can be used for anchor links.

An example Markdown blog post may pass the following `Astro.props` object:

```js
Astro.props = {
  file: "/home/user/projects/.../file.md",
  url: "/en/guides/markdown-content/",
  frontmatter: {
    /** Frontmatter from a blog post */
    title: "Astro 0.18 Release",
    date: "Tuesday, July 27 2021",
    author: "Matthew Phillips",
    description: "Astro 0.18 is our biggest release since Astro launch.",
  },
  getHeadings: () => [
    {"depth": 1, "text": "Astro 0.18 Release", "slug": "astro-018-release"},
    {"depth": 2, "text": "Responsive partial hydration", "slug": "responsive-partial-hydration"}
    /* ... */
  ],
  rawContent: () => "# Astro 0.18 Release\nA little over a month ago, the first public beta [...]",
  compiledContent: () => "<h1>Astro 0.18 Release</h1>\n<p>A little over a month ago, the first public beta [...]</p>",
}
```

## The `<Content />` Component

[Section titled “The \<Content /> Component”](#the-content--component)

The `<Content />` component is available by importing `Content` from a Markdown file. This component returns the file’s full body content, rendered to HTML. You can optionally rename `Content` to any component name you prefer.

You can similarly [render the HTML content of a Markdown collection entry](/en/guides/content-collections/#rendering-body-content) by rendering a `<Content />` component.

src/pages/content.astro

```astro
---
// Import statement
import {Content as PromoBanner} from '../components/promoBanner.md';


// Collections query
import { getEntry, render } from 'astro:content';


const product = await getEntry('products', 'shirt');
const { Content } = await render(product);
---
<h2>Today's promo</h2>
<PromoBanner />


<p>Sale Ends: {product.data.saleEndDate.toDateString()}</p>
<Content />
```

## Heading IDs

[Section titled “Heading IDs”](#heading-ids)

Writing headings in Markdown will automatically give you anchor links so you can link directly to certain sections of your page.

src/pages/page-1.md

```markdown
---
title: My page of content
---
## Introduction


I can link internally to [my conclusion](#conclusion) on the same page when writing Markdown.


## Conclusion


I can visit `https://example.com/page-1/#introduction` in a browser to navigate directly to my Introduction.
```

Astro generates heading `id`s based on `github-slugger`. You can find more examples in [the github-slugger documentation](https://github.com/Flet/github-slugger#usage).

### Heading IDs and plugins

[Section titled “Heading IDs and plugins”](#heading-ids-and-plugins)

Astro injects an `id` attribute into all heading elements (`<h1>` to `<h6>`) in Markdown and MDX files. You can retrieve this data from the `getHeadings()` utility available as a [Markdown exported property](#available-properties) from an imported file, or from the `render()` function when [using Markdown returned from a content collections query](#markdown-from-content-collections-queries).

You can customize these heading IDs by adding a rehype plugin that injects `id` attributes (e.g. `rehype-slug`). Your custom IDs, instead of Astro’s defaults, will be reflected in the HTML output and the items returned by `getHeadings()`.

By default, Astro injects `id` attributes after your rehype plugins have run. If one of your custom rehype plugins needs to access the IDs injected by Astro, you can import and use Astro’s `rehypeHeadingIds` plugin directly. Be sure to add `rehypeHeadingIds` before any plugins that rely on it:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
+import { rehypeHeadingIds } from '@astrojs/markdown-remark';
import { otherPluginThatReliesOnHeadingIDs } from 'some/plugin/source';


export default defineConfig({
  markdown: {
    rehypePlugins: [
+      rehypeHeadingIds,
      otherPluginThatReliesOnHeadingIDs,
    ],
  },
});
```

## Markdown Plugins

[Section titled “Markdown Plugins”](#markdown-plugins)

Markdown support in Astro is powered by [remark](https://remark.js.org/), a powerful parsing and processing tool with an active ecosystem. Other Markdown parsers like Pandoc and markdown-it are not currently supported.

Astro applies the [GitHub-flavored Markdown](https://github.com/remarkjs/remark-gfm) and [SmartyPants](https://github.com/silvenon/remark-smartypants) plugins by default. This brings some niceties like generating clickable links from text, and formatting for [quotations and em-dashes](https://daringfireball.net/projects/smartypants/).

You can customize how remark parses your Markdown in `astro.config.mjs`. See the full list of [Markdown configuration options](/en/reference/configuration-reference/#markdown-options).

### Adding remark and rehype plugins

[Section titled “Adding remark and rehype plugins”](#adding-remark-and-rehype-plugins)

Astro supports adding third-party [remark](https://github.com/remarkjs/remark) and [rehype](https://github.com/rehypejs/rehype) plugins for Markdown. These plugins allow you to extend your Markdown with new capabilities, like [auto-generating a table of contents](https://github.com/remarkjs/remark-toc), [applying accessible emoji labels](https://github.com/florianeckerstorfer/remark-a11y-emoji), and [styling your Markdown](/en/guides/styling/#markdown-styling).

We encourage you to browse [awesome-remark](https://github.com/remarkjs/awesome-remark) and [awesome-rehype](https://github.com/rehypejs/awesome-rehype) for popular plugins! See each plugin’s own README for specific installation instructions.

This example applies [`remark-toc`](https://github.com/remarkjs/remark-toc) and [`rehype-accessible-emojis`](https://www.npmjs.com/package/rehype-accessible-emojis) to Markdown files:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';
import remarkToc from 'remark-toc';
import { rehypeAccessibleEmojis } from 'rehype-accessible-emojis';


export default defineConfig({
  markdown: {
    remarkPlugins: [ [remarkToc, { heading: 'toc', maxDepth: 3 } ] ],
    rehypePlugins: [rehypeAccessibleEmojis],
  },
});
```

### Customizing a plugin

[Section titled “Customizing a plugin”](#customizing-a-plugin)

In order to customize a plugin, provide an options object after it in a nested array.

The example below adds the [heading option to the `remarkToc` plugin](https://github.com/remarkjs/remark-toc#options) to change where the table of contents is placed, and the [`behavior` option to the `rehype-autolink-headings` plugin](https://github.com/rehypejs/rehype-autolink-headings#options) in order to add the anchor tag after the headline text.

astro.config.mjs

```js
import remarkToc from 'remark-toc';
import rehypeSlug from 'rehype-slug';
import rehypeAutolinkHeadings from 'rehype-autolink-headings';


export default {
  markdown: {
    remarkPlugins: [ [remarkToc, { heading: "contents"} ] ],
    rehypePlugins: [rehypeSlug, [rehypeAutolinkHeadings, { behavior: 'append' }]],
  },
}
```

### Modifying frontmatter programmatically

[Section titled “Modifying frontmatter programmatically”](#modifying-frontmatter-programmatically)

You can add frontmatter properties to all of your Markdown and MDX files by using a [remark or rehype plugin](#markdown-plugins).

1. Append a `customProperty` to the `data.astro.frontmatter` property from your plugin’s `file` argument:

   example-remark-plugin.mjs

   ```js
   export function exampleRemarkPlugin() {
     // All remark and rehype plugins return a separate function
     return function (tree, file) {
       file.data.astro.frontmatter.customProperty = 'Generated property';
     }
   }
   ```

   Tip

   **Added in:** `astro@2.0.0`

   `data.astro.frontmatter` contains all properties from a given Markdown or MDX document. This allows you to modify existing frontmatter properties, or compute new properties from this existing frontmatter.

2. Apply this plugin to your `markdown` or `mdx` integration config:

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import { exampleRemarkPlugin } from './example-remark-plugin.mjs';


   export default defineConfig({
     markdown: {
       remarkPlugins: [exampleRemarkPlugin]
     },
   });
   ```

   or

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import { exampleRemarkPlugin } from './example-remark-plugin.mjs';


   export default defineConfig({
     integrations: [
       mdx({
         remarkPlugins: [exampleRemarkPlugin],
       }),
     ],
   });
   ```

Now, every Markdown or MDX file will have `customProperty` in its frontmatter, making it available when [importing your markdown](#importing-markdown) and from [the `Astro.props.frontmatter` property in your layouts](#frontmatter-layout-property).

![](/houston_chef.webp) **Related recipe:** [Add reading time](/en/recipes/reading-time/)

### Extending Markdown config from MDX

[Section titled “Extending Markdown config from MDX”](#extending-markdown-config-from-mdx)

Astro’s MDX integration will extend [your project’s existing Markdown configuration](/en/reference/configuration-reference/#markdown-options) by default. To override individual options, you can specify their equivalent in your MDX configuration.

The following example disables GitHub-Flavored Markdown and applies a different set of remark plugins for MDX files:

astro.config.mjs

```ts
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';


export default defineConfig({
  markdown: {
    syntaxHighlight: 'prism',
    remarkPlugins: [remarkPlugin1],
    gfm: true,
  },
  integrations: [
    mdx({
      // `syntaxHighlight` inherited from Markdown


      // Markdown `remarkPlugins` ignored,
      // only `remarkPlugin2` applied.
      remarkPlugins: [remarkPlugin2],
      // `gfm` overridden to `false`
      gfm: false,
    })
  ]
});
```

To avoid extending your Markdown config from MDX, set [the `extendMarkdownConfig` option](/en/guides/integrations-guide/mdx/#extendmarkdownconfig) (enabled by default) to `false`:

astro.config.mjs

```ts
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';


export default defineConfig({
  markdown: {
    remarkPlugins: [remarkPlugin],
  },
  integrations: [
    mdx({
      // Markdown config now ignored
      extendMarkdownConfig: false,
      // No `remarkPlugins` applied
    })
  ]
});
```

## Individual Markdown pages

[Section titled “Individual Markdown pages”](#individual-markdown-pages)

Tip

[Content collections](/en/guides/content-collections/) and [importing Markdown into `.astro` components](#dynamic-jsx-like-expressions) provide more features for rendering your Markdown and are the recommended way to handle most of your content. However, there may be times when you want the convenience of just adding a file to `src/pages/` and having a simple page automatically created for you.

Astro treats [any supported file inside of the `/src/pages/` directory](/en/basics/astro-pages/#supported-page-files) as a page, including `.md` and other Markdown file types.

Placing a file in this directory, or any sub-directory, will automatically build a page route using the pathname of the file and display the Markdown content rendered to HTML. Astro will automatically add a `<meta charset="utf-8">` tag to your page to allow easier authoring of non-ASCII content.

src/pages/page-1.md

```markdown
---
title: Hello, World
---



---

# Content collections

> Manage your content with type safety.

**Added in:** `astro@2.0.0`

**Content collections** are the best way to manage sets of content in any Astro project: blog posts, product descriptions, character profiles, recipes, or any structured content. Collections help to organize and query your documents, enable Intellisense and type checking in your editor, and provide automatic TypeScript type-safety for all of your content.

Astro provides performant, scalable APIs to load, query, and render content from anywhere: stored locally in your project, hosted remotely, or fetched live from frequently-updating sources.

## What are Content Collections?

[Section titled “What are Content Collections?”](#what-are-content-collections)

A content collection is a set of related, structurally identical data. This data can be stored in one or several files locally (e.g. a folder of individual Markdown files of blog posts, a single JSON file of product descriptions) or fetched from remote sources such as a database, CMS, or API endpoint. Each member of the collection is called an entry.

* src/

  * …

* **newsletter/** the “newsletter” collection

  * week-1.md a collection entry
  * week-2.md a collection entry
  * week-3.md a collection entry

* **authors/** the “author” collection

  * authors.json a single file containing all collection entries

Collections are defined by the location and shape of its entries and provide a convenient way to query and render your content and associated metadata. You can create a collection any time you have a group of related data or content, stored in the same location, that shares a common structure.

[Two types of content collections](#types-of-collections) are available to allow you to work with data fetched either at build time or at request time. Both build-time collections and live updating collections use:

* A required `loader` to retrieve your content and metadata from wherever it is stored and make it available to your project through content-focused APIs.
* An optional collection `schema` that allows you to define the expected shape of each entry for type safety, autocomplete, and validation in your editor.

Collections stored locally in your project or on your filesystem can use one of Astro’s [provided build-time loaders](#build-time-collection-loaders) to fetch data from Markdown, MDX, Markdoc, YAML, TOML, or JSON files. Point Astro to the location of your content, define your data shape, and you’re good to go with a blog or similarly content-heavy, mostly static site in no time!

With [community-built loaders](https://astro.build/integrations/?search=\&categories%5B%5D=loaders) or by building a [custom build-time collection loader](#custom-build-time-loaders) or [live loader](#creating-a-live-loader) yourself, you can fetch remote data from any external source, such as a CMS, database, or headless payment system, either at build time or live on demand.

### Types of collections

[Section titled “Types of collections”](#types-of-collections)

[Build-time content collections](#defining-build-time-content-collections) are updated at build time, and data is saved to a storage layer. This provides excellent performance for most content, but may not be suitable for frequently updating data sources requiring up-to-the-moment data freshness, such as live stock prices.

For the best performance and scalability, use build-time content collections when one or more of these is true:

* **Performance is critical** and you want to prerender data at build time.
* **Your data is relatively static** (e.g., blog posts, documentation, product descriptions).
* **You want to benefit from build-time optimization** and caching.
* **You need to process MDX** or **perform image optimization**.
* **Your data can be fetched once and reused** across multiple builds.

Quick start

See [the official Astro blog starter template](https://github.com/withastro/astro/tree/latest/examples/blog) to get up and running quickly with an example of using the [built-in `glob()` loader](#the-glob-loader) and [defining a schema](#defining-the-collection-schema) for a collection of local Markdown or MDX blog posts.

[Live content collections](#live-content-collections) fetch their data at runtime rather than build time. This allows you to access frequently updated data from CMSs, APIs, databases, or other sources using a unified API, without needing to rebuild your site when the data changes. However, this can come at a performance cost since data is fetched at each request and returned directly with no data store persistence.

Live content collections are designed for data that changes frequently and needs to be up-to-date when a page is requested. Consider using them when one or more of these is true:

* **You need real-time information** (e.g. user-specific data, current stock levels).
* **You want to avoid constant rebuilds** for content that changes often.
* **Your data updates frequently** (e.g. up-to-the-minute product inventory, prices, availability).
* **You need to pass dynamic filters** to your data source based on user input or request parameters.
* **You’re building preview functionality** for a CMS where editors need to see draft content immediately.

Both kinds of collections can exist in the same project, so you can always choose the best type of collection for each individual data source. For example, a build-time collection can manage product descriptions, while a live collection can manage content inventory.

Both types of collections use similar APIs (e.g. `getCollection()` and `getLiveCollection()`), so that working with collections will feel familiar no matter which one you choose, while still ensuring that you always know which type of collection you are working with.

We suggest using build-time content collections whenever possible, and using live collections when your content needs updating in real time and the performance tradeoffs are acceptable. Additionally, live content collections have some limitations compared to build-time collections:

* **No MDX support**: MDX cannot be rendered at runtime
* **No image optimization**: Images cannot be processed at runtime
* **Performance considerations**: Data is fetched on each request (unless cached)
* **No data store persistence**: Data is not saved to the content layer data store

### When to create a collection

[Section titled “When to create a collection”](#when-to-create-a-collection)

Define your data as a collection when:

* You have multiple files or data to organize that share the same overall structure (e.g. a directory of blog posts written in Markdown which all have the same frontmatter properties).
* You have existing content stored remotely, such as in a CMS, and want to take advantage of the collections helper functions instead of using `fetch()` or SDKs.
* You need to fetch (tens of) thousands of related pieces of data at build time, and need a querying and caching method that handles at scale.

Much of the benefit of using collections comes from:

* Defining a common data shape to validate that an individual entry is “correct” or “complete”, avoiding errors in production.
* Content-focused APIs designed to make querying intuitive (e.g. `getCollection()` instead of `import.meta.glob()`) when importing and rendering content on your pages.
* Access to both built-in loaders and access to the low-level [Content Loader API](/en/reference/content-loader-reference/) for retrieving your content. There are additionally several third-party and community-built loaders available, and you can build your own custom loader to fetch data from anywhere.
* Performance and scalability. Build-time content collections data can be cached between builds and is suitable for tens of thousands of content entries.

### When not to create a collection

[Section titled “When not to create a collection”](#when-not-to-create-a-collection)

Collections provide excellent structure, safety, and organization when you have multiple pieces of content that must share the same properties.

Collections may not be your solution if:

* You have only one or a small number of different content pages. Consider [making individual page components](/en/basics/astro-pages/) such as `src/pages/about.astro` with your content directly instead.
* You are displaying files that are not processed by Astro, such as PDFs. Place these static assets in the [`public/` directory](/en/basics/project-structure/#public) of your project instead.
* Your data source has its own SDK/client library for imports that is incompatible with or does not offer a content loader, and you prefer to use it directly.

## TypeScript configuration for collections

[Section titled “TypeScript configuration for collections”](#typescript-configuration-for-collections)

Content collections rely on TypeScript to provide Zod validation, Intellisense, and type checking in your editor. By default, Astro configures a [`strict` TypeScript template](/en/guides/typescript/#tsconfig-templates) when you create a new project using the `create astro` CLI command. Both of Astro’s `strict` and `strictest` templates include the TypeScript settings your project needs for content collections.

If you changed this setting to `base` because you are not writing TypeScript in your project, or are not using any of Astro’s built-in templates, you will need to also add the following `compilerOptions` in your `tsconfig.json` to use content collections:

tsconfig.json

```diff
{
  "extends": "astro/tsconfigs/base",
  // not needed for `strict` or `strictest`
  +"compilerOptions": {
    +"strictNullChecks": true,
    +"allowJs": true
+  }
}
```

## Defining build-time content collections

[Section titled “Defining build-time content collections”](#defining-build-time-content-collections)

All of your build-time content collections are defined in a special `src/content.config.ts` file (`.js` and `.mjs` extensions are also supported) using `defineCollection()`, and then a single collections object is exported for use in your project.

Each individual collection configures:

* [a build-time `loader`](#build-time-collection-loaders) for a data source (required)
* [a build-time `schema`](#defining-the-collection-schema) for type safety (optional, but highly recommended!)

src/content.config.ts

```ts
// 1. Import utilities from `astro:content`
import { defineCollection } from 'astro:content';


// 2. Import loader(s)
import { glob, file } from 'astro/loaders';


// 3. Import Zod
import { z } from 'astro/zod';


// 4. Define a `loader` and `schema` for each collection
const blog = defineCollection({
  loader: glob({ base: './src/content/blog', pattern: '**/*.{md,mdx}' }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
  }),
});


// 5. Export a single `collections` object to register your collection(s)
export const collections = { blog };
```

You can then use the dedicated `getCollection()` and `getEntry()` functions to [query your content collections data](#querying-build-time-collections) and render your content.

You can choose to [generate page routes](#generating-routes-from-content) from your build-time collection entries at build time for an entirely static, prerendered site. Or, you can render your build-time collections on demand, choosing to delay building your page until it is first requested. This is useful when you have a large number of pages (e.g. thousands or tens of thousands) and want to delay building a static page until it is needed.

## Build-time collection loaders

[Section titled “Build-time collection loaders”](#build-time-collection-loaders)

Astro provides two built-in loaders (`glob()` and `file()`) for fetching your local content at build time. Pass the location of your data in your project or on your filesystem, and these loaders will automatically handle your data and update the persistent data store content layer.

To fetch remote data at build time, you can [build a custom loader](#custom-build-time-loaders) to retrieve your data and update the data store. Or, you can use any [third-party or community-published loader integration](https://astro.build/integrations/2/?search=\&categories%5B%5D=loaders). Several already exist for popular content management systems as well as common data sources such as Obsidian vaults, GitHub repositories, or Bluesky posts.

### The `glob()` loader

[Section titled “The glob() loader”](#the-glob-loader)

The [`glob()` loader](/en/reference/content-loader-reference/#glob-loader) fetches entries from directories of Markdown, MDX, Markdoc, JSON, YAML, or TOML files from anywhere on the filesystem. If you store your content entries locally as separate files, such as a directory of blog posts, then the `glob()` loader is all you need to access your content.

This loader requires a `pattern` of entry files to match using glob patterns supported by [micromatch](https://github.com/micromatch/micromatch#matching-features), and a `base` file path of where your files are located. A unique `id` for each entry will be automatically generated from its file name, but you can [define custom IDs](#defining-custom-ids) if needed.

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';


const blog = defineCollection({
  loader: glob({ pattern: "**/*.md", base: "./src/data/blog" }),
});


export const collections = { blog };
```

#### Defining custom IDs

[Section titled “Defining custom IDs”](#defining-custom-ids)

When using the [`glob()` loader](#the-glob-loader) with Markdown, MDX, Markdoc, JSON, or TOML files, every content entry [`id`](/en/reference/modules/astro-content/#collectionentryid) is automatically generated in an URL-friendly format based on the content filename. This unique `id` is used to query the entry directly from your collection. It is also useful when [creating new pages and URLs from your content](#generating-routes-from-content).

You can override a single entry’s generated `id` by adding your own `slug` property to the file frontmatter or data object for JSON files. This is similar to the “permalink” feature of other web frameworks.

src/blog/1.md

```md
---
title: My Blog Post
slug: my-custom-id/supports/slashes
---
Your blog post content here.
```

src/categories/1.json

```json
{
  "title": "My Category",
  "slug": "my-custom-id/supports/slashes",
  "description": "Your category description here."
}
```

You can also pass options to the `glob()` loader’s [`generateID()` helper function](/en/reference/content-loader-reference/#generateid) when you define your build-time collection to adjust how `id`s are generated. For example, you may wish to revert the default behavior of converting uppercase letters to lowercase for each collection entry:

src/content.config.ts

```js
const authors = defineCollection({
  /* Retrieve all JSON files in your authors directory while retaining
   * uppercase letters in the ID. */
  loader: glob({
    pattern: '**/*.json',
    base: "./src/data/authors",
    generateId: ({ entry }) => entry.replace(/\.json$/, ''),
  }),
});
```

### The `file()` loader

[Section titled “The file() loader”](#the-file-loader)

The [`file()` loader](/en/reference/content-loader-reference/#file-loader) fetches multiple entries from a single local file defined in your collection. The `file()` loader will automatically detect and parse (based on the file extension) a single array of objects from JSON and YAML files, and will treat each top-level table as an independent entry in TOML files.

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { file } from 'astro/loaders';


const dogs = defineCollection({
  loader: file("src/data/dogs.json"),
});


export const collections = { dogs };
```

Each entry object in the file must have a unique `id` key property so that the entry can be identified and queried. Unlike the `glob()` loader, the `file()` loader will not automatically generate IDs for each entry.

You can provide your entries as an array of objects with an `id` property, or in object form where the unique `id` is the key:

src/data/dogs.json

```json
// Specify an `id` property in each object of an array
[
  { "id": "poodle", "coat": "curly", "shedding": "low" },
  { "id": "afghan", "coat": "short", "shedding": "low" }
]
```

src/data/dogs.json

```json
// Each key will be used as the `id`
{
  "poodle": { "coat": "curly", "shedding": "low" },
  "afghan": { "coat": "silky", "shedding": "low" }
}
```

#### Parsing other data formats

[Section titled “Parsing other data formats”](#parsing-other-data-formats)

Support for parsing single JSON, YAML, and TOML files into collection entries with the `file()` loader is built-in (unless you have a [nested JSON document](#nested-json-documents)). To load your collection from unsupported file types, such as `.csv`, you will need to create a [parser function](/en/reference/content-loader-reference/#parser). This function can be made async if required (e.g. to fetch files from the web, or if your parser is asyncronous).

The following example shows importing a third-party CSV parser then passing a custom `parser` function to the `file()` loader:

src/content.config.ts

```typescript
import { defineCollection } from "astro:content";
import { file } from "astro/loaders";
import { parse as parseCsv } from "csv-parse/sync";


const cats = defineCollection({
  loader: file("src/data/cats.csv", {
    parser: (text) => parseCsv(text, { columns: true, skipEmptyLines: true }),
  }),
});
```

##### Nested `.json` documents

[Section titled “Nested .json documents”](#nested-json-documents)

The `parser()` argument can be used to load a single collection from a nested JSON document. For example, this JSON file contains multiple collections:

src/data/pets.json

```json
{"dogs": [{}], "cats": [{}]}
```

You can separate these collections by passing a custom `parser()` function to the `file()` loader for each collection, using Astro’s built-in JSON parsing:

src/content.config.ts

```typescript
import { file } from "astro/loaders";
import { defineCollection } from "astro:content";


const dogs = defineCollection({
  loader: file("src/data/pets.json", { parser: (text) => JSON.parse(text).dogs })
});
const cats = defineCollection({
  loader: file("src/data/pets.json", { parser: (text) => JSON.parse(text).cats })
});
```

### Custom build-time loaders

[Section titled “Custom build-time loaders”](#custom-build-time-loaders)

You can [build a custom loader](/en/reference/content-loader-reference/#building-a-loader) using the Content Loader API to fetch remote content from any data source, such as a CMS, a database, or an API endpoint.

Then you can import and define your custom loader in your collections config, passing any required values:

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { myLoader } from './loader.ts';


const blog = defineCollection({
  loader: myLoader({
    url: "https://api.example.com/posts",
    apiKey: "my-secret",
  }),
});
```

Tip

Find community-built and third-party loaders in the [Astro integrations directory](https://astro.build/integrations/?search=\&categories%5B%5D=loaders).

Using a custom loader to fetch your data will automatically create a collection from your remote data. This gives you all the benefits of local collections, including collection-specific API helpers such as `getCollection()` and `render()` to [query and display your data](#querying-build-time-collections), as well as schema validation.

Similar to creating an Astro integration or Vite plugin, you can [distribute your loader as an npm package](/en/guides/integrations/) that others can use in their projects.

See the full [Content Loader API](/en/reference/content-loader-reference/) for examples of how to build your own loader.

## Defining the collection schema

[Section titled “Defining the collection schema”](#defining-the-collection-schema)

Schemas enforce consistent frontmatter or entry data within a collection through Zod validation. A schema **guarantees** that this data exists in a predictable form when you need to reference or query it. If any file violates its collection schema, Astro will provide a helpful error to let you know.

Schemas also power Astro’s automatic TypeScript typings for your content. When you define a schema for your collection, Astro will automatically generate and apply a TypeScript interface to it. The result is full TypeScript support when you query your collection, including property autocompletion and type-checking.

Tip

In order for Astro to recognize a new or updated schema, you may need to restart the dev server or [sync the content layer](/en/reference/cli-reference/#astro-dev) (`s + enter`) to define the `astro:content` module.

Providing a `schema` is optional, but highly recommended! If you choose to use a schema, then every frontmatter or data property of your collection entries must be defined using a [Zod data type](/en/reference/modules/astro-zod/#common-data-type-validators):

src/content.config.ts

```ts
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';
import { glob, file } from 'astro/loaders';


const blog = defineCollection({
  loader: glob({ pattern: "**/*.md", base: "./src/data/blog" }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
  })
});
const dogs = defineCollection({
  loader: file("src/data/dogs.json"),
  schema: z.object({
    id: z.string(),
    breed: z.string(),
    temperament: z.array(z.string()),
  }),
});


export const collections = { blog, dogs };
```

### Defining datatypes with Zod

[Section titled “Defining datatypes with Zod”](#defining-datatypes-with-zod)

Astro uses [Zod](https://github.com/colinhacks/zod) to power its content schemas. With Zod, Astro is able to validate every file’s data within a collection *and* provide automatic TypeScript types when you query content from inside your project.

To use Zod in Astro, import the `z` utility from `"astro/zod"`. This is a re-export of the Zod library, and it supports all of the features of Zod 4.

See the [`z` utility reference](/en/reference/modules/astro-zod/) for a cheatsheet of common datatypes and to learn how Zod works and what features are available.

#### Zod schema methods

[Section titled “Zod schema methods”](#zod-schema-methods)

All [Zod schema methods](/en/reference/modules/astro-zod/#using-zod-methods) (e.g. `.parse()`, `.transform()`) are available, with some limitations. Notably, performing custom validation checks on images using `image().refine()` is unsupported.

### Defining collection references

[Section titled “Defining collection references”](#defining-collection-references)

Collection entries can also “reference” other related entries.

With the [`reference()` function](/en/reference/modules/astro-content/#reference), you can define a property in a collection schema as an entry from another collection. For example, you can require that every `space-shuttle` entry includes a `pilot` property which uses the `pilot` collection’s own schema for type checking, autocomplete, and validation.

A common example is a blog post that references reusable author profiles stored as JSON, or related post URLs stored in the same collection:

src/content.config.ts

```ts
import { defineCollection, reference } from 'astro:content';
import { glob } from 'astro/loaders';
import { z } from 'astro/zod';


const blog = defineCollection({
  loader: glob({ base: './src/content/blog', pattern: '**/*.{md,mdx}' }),
  schema: z.object({
    title: z.string(),
    // Reference a single author from the `authors` collection by `id`
    author: reference('authors'),
    // Reference an array of related posts from the `blog` collection by `id`
    relatedPosts: z.array(reference('blog')),
  })
});


const authors = defineCollection({
  loader: glob({ pattern: '**/*.json', base: "./src/data/authors" }),
  schema: z.object({
    name: z.string(),
    portfolio: z.url(),
  })
});


export const collections = { blog, authors };
```

This example blog post specifies the `id`s of related posts and the `id` of the post author:

src/content/blog/welcome.md

```yaml
---
title: "Welcome to my blog"
author: ben-holmes # references `src/data/authors/ben-holmes.json`
relatedPosts:
- about-me # references `src/content/blog/about-me.md`
- my-year-in-review # references `src/content/blog/my-year-in-review.md`
---
```

These references will be transformed into objects containing a `collection` key and an `id` key, allowing you to easily [query them in your templates](#accessing-referenced-data).

## Querying build-time collections

[Section titled “Querying build-time collections”](#querying-build-time-collections)

Astro provides helper functions to query a build-time collection and return one or more content entries.

* [`getCollection()`](/en/reference/modules/astro-content/#getcollection) fetches an entire collection and returns an array of entries.
* [`getEntry()`](/en/reference/modules/astro-content/#getentry) fetches a single entry from a collection.

These return entries with a unique `id`, a `data` object with all defined properties, and will also return a `body` containing the raw, uncompiled body of a Markdown, MDX, or Markdoc document.

src/pages/index.astro

```astro
---
import { getCollection, getEntry } from 'astro:content';


// Get all entries from a collection.
// Requires the name of the collection as an argument.
const allBlogPosts = await getCollection('blog');


// Get a single entry from a collection.
// Requires the name of the collection and `id`
const poodleData = await getEntry('dogs', 'poodle');
---
```

The sort order of generated collections is non-deterministic and platform-dependent. This means that if you are calling `getCollection()` and need your entries returned in a specific order (e.g. blog posts sorted by date), you must sort the collection entries yourself:

src/pages/blog.astro

```astro
---
import { getCollection } from 'astro:content';


const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf(),
);
---
```

See the full list of properties returned by the [`CollectionEntry` type](/en/reference/modules/astro-content/#collectionentry).

### Using content in Astro templates

[Section titled “Using content in Astro templates”](#using-content-in-astro-templates)

After querying your collections, you can access each entry’s content and metadata directly inside of your Astro component template.

For example, you can create a list of links to your blog posts, displaying information from your entry’s frontmatter using the `data` property:

src/pages/index.astro

```astro
---
import { getCollection } from 'astro:content';
const posts = await getCollection('blog');
---
<h1>My posts</h1>
<ul>
  {posts.map(post => (
    <li><a href={`/blog/${post.id}`}>{post.data.title}</a></li>
  ))}
</ul>
```

### Rendering body content

[Section titled “Rendering body content”](#rendering-body-content)

Once queried, you can render Markdown and MDX entries to HTML using the [`render()`](/en/reference/modules/astro-content/#render) function from `astro:content`. Calling this function gives you access to rendered HTML content, including both a `<Content />` component and a list of all rendered headings.

src/pages/blog/post-1.astro

```astro
---
import { getEntry, render } from 'astro:content';


const entry = await getEntry('blog', 'post-1');


const { Content } = await render(entry);
---
<h1>{entry.data.title}</h1>
<p>Published on: {entry.data.published.toDateString()}</p>
<Content />
```

When working with MDX entries, you can also [pass your own components to `<Content />`](/en/guides/integrations-guide/mdx/#passing-components-to-mdx-content) to replace HTML elements with custom alternatives.

#### Passing content as props

[Section titled “Passing content as props”](#passing-content-as-props)

A component can also pass an entire collection entry as a prop.

You can use the [`CollectionEntry`](/en/reference/modules/astro-content/#collectionentry) utility to correctly type your component’s props using TypeScript. This utility takes a string argument that matches the name of your collection schema and will inherit all of the properties of that collection’s schema.

src/components/BlogCard.astro

```astro
---
import type { CollectionEntry } from 'astro:content';
interface Props {
  post: CollectionEntry<'blog'>;
}


// `post` will match your 'blog' collection schema type
const { post } = Astro.props;
---
```

### Filtering collection queries

[Section titled “Filtering collection queries”](#filtering-collection-queries)

`getCollection()` takes an optional “filter” callback that allows you to filter your query based on an entry’s `id` or `data` properties.

You can use this to filter by any content criteria you like. For example, you can filter by properties like `draft` to prevent any draft blog posts from publishing to your blog:

src/pages/blog.astro

```astro
---
// Example: Filter out content entries with `draft: true`
import { getCollection } from 'astro:content';
const publishedBlogEntries = await getCollection('blog', ({ data }) => {
  return data.draft !== true;
});
---
```

You can also create draft pages that are available when running the dev server, but not built in production:

src/pages/blog.astro

```astro
---
// Example: Filter out content entries with `draft: true` only when building for production
import { getCollection } from 'astro:content';
const blogEntries = await getCollection('blog', ({ data }) => {
  return import.meta.env.PROD ? data.draft !== true : true;
});
---
```

The filter argument also supports filtering by nested directories within a collection. Since the `id` includes the full nested path, you can filter by the start of each `id` to only return items from a specific nested directory:

src/pages/blog.astro

```astro
---
// Example: Filter entries by sub-directory in the collection
import { getCollection } from 'astro:content';
const englishDocsEntries = await getCollection('docs', ({ id }) => {
  return id.startsWith('en/');
});
---
```

### Accessing referenced data

[Section titled “Accessing referenced data”](#accessing-referenced-data)

To access [references defined in your schema](#defining-collection-references), first query your collection entry. Your references will be available on the returned `data` object. (e.g. `entry.data.author` and `entry.data.relatedPosts`)

Then, you can use the `getEntry()` function again (or `getEntries()` to retrieve multiple referenced entries) by passing those returned values. The `reference()` function in your schema transforms those values into one or more `collection` and `id` objects as a convenient way to query this related data.

src/pages/blog/adventures-in-space.astro

```astro
---
import { getEntry, getEntries } from 'astro:content';


// First, query a blog post
const blogPost = await getEntry('blog', 'Adventures in Space');


// Retrieve a single reference item: the blog post's author
// Equivalent to querying `{collection: "authors", id: "ben-holmes"}`
const author = await getEntry(blogPost.data.author);


// Retrieve an array of referenced items: all the related posts
// Equivalent to querying `[{collection: "blog", id: "visiting-mars"}, {collection: "blog", id: "leaving-earth-for-the-first-time"}]`
const relatedPosts = await getEntries(blogPost.data.relatedPosts);
---


<h1>{blogPost.data.title}</h1>
<p>Author: {author.data.name}</p>


<!-- ... -->


<h2>You might also like:</h2>
{relatedPosts.map(post => (
  <a href={post.id}>{post.data.title}</a>
))}
```

## Generating Routes from Content

[Section titled “Generating Routes from Content”](#generating-routes-from-content)

Content collections are stored outside of the `src/pages/` directory. This means that no pages or routes are generated for your collection items by default by Astro’s [file-based routing](/en/guides/routing/).

You will need to manually create a new [dynamic route](/en/guides/routing/#dynamic-routes) if you want to generate HTML pages for each of your collection entries, such as individual blog posts. Your dynamic route will map the incoming request param (e.g. `Astro.params.id` in `src/pages/blog/[...id].astro`) to fetch the correct entry for each page.

The exact method for generating routes will depend on whether your pages are prerendered (default) or rendered on demand by a server.

### Building for static output (default)

[Section titled “Building for static output (default)”](#building-for-static-output-default)

If you are building a static website (Astro’s default behavior) with build-time collections, use the [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths) function to create multiple pages from a single page component (e.g. `src/pages/[id].astro`) during your build.

Call `getCollection()` inside of `getStaticPaths()` to have your collection data available for building static routes. Then, create the individual URL paths using the `id` property of each content entry. Each page receives the entire collection entry as a prop for [use in your page template](#using-content-in-astro-templates).

src/pages/posts/\[id].astro

```astro
---
import { getCollection, render } from 'astro:content';
// 1. Generate a new path for every collection entry
export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map(post => ({
    params: { id: post.id },
    props: { post },
  }));
}
// 2. For your template, you can get the entry directly from the prop
const { post } = Astro.props;
const { Content } = await render(post);
---
<h1>{post.data.title}</h1>
<Content />
```

This will generate a page route for every entry in the `blog` collection. For example, an entry at `src/blog/hello-world.md` will have an `id` of `hello-world`, and therefore its final URL will be `/posts/hello-world/`.

Note

If your custom slugs contain the `/` character to produce URLs with multiple path segments, you must use a [rest parameter (e.g. `[...id]`)](/en/guides/routing/#rest-parameters) in the `.astro` filename for this dynamic routing page.

### Building routes on demand at request time

[Section titled “Building routes on demand at request time”](#building-routes-on-demand-at-request-time)

With an adapter installed for [on-demand rendering](/en/guides/on-demand-rendering/), you can generate your dynamic page routes at request time. First, examine the request (using `Astro.request` or `Astro.params`) to find the slug on demand, and then fetch it using one of Astro’s content collection helper functions:

* [`getEntry()`](/en/reference/modules/astro-content/#getentry) for build-time collection pages that are generated once, upon first request.
* [`getLiveEntry()`](/en/reference/modules/astro-content/#getliveentry) for live collection pages where data is (re)fetched at each request time.

src/pages/posts/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getEntry, render } from "astro:content";


// 1. Get the slug from the incoming server request
const { id } = Astro.params;
if (id === undefined) {
  return Astro.redirect("/404");
}


// 2. Query for the entry directly using the request slug
const post = await getEntry("blog", id);


// 3. Redirect if the entry does not exist
if (post === undefined) {
  return Astro.redirect("/404");
}


// 4. Render the entry to HTML in the template
const { Content } = await render(post);
---
<h1>{post.data.title}</h1>
<Content />
```

Tip

Explore the `src/pages/` folder of the [blog tutorial demo code on GitHub](https://github.com/withastro/blog-tutorial-demo/tree/content-collections/src/pages) to see full examples of creating dynamic pages from your collections for blog features like a list of blog posts, tags pages, and more!

## Live content collections

[Section titled “Live content collections”](#live-content-collections)

Live collections use a different API than build-time content collections, although the configuration and helper functions are designed to feel familiar.

Key differences include:

1. **Execution time**: Run at request time instead of build time
2. **Configuration file**: Use `src/live.config.ts` instead of `src/content.config.ts`
3. **Collection definition**: Use `defineLiveCollection()` instead of `defineCollection()`
4. **Loader API**: Implement `loadCollection` and `loadEntry` methods instead of the `load` method
5. **Data return**: Return data directly instead of storing in the data store
6. **User-facing functions**: Use `getLiveCollection()`/`getLiveEntry()` instead of `getCollection()`/`getEntry()`

Additionally, you must have an adapter configured for [on-demand rendering](/en/guides/on-demand-rendering/) of live collection data.

Define your live collections in the special file `src/live.config.ts` (separate from your `src/content.config.ts` for build-time collections, if you have one).

Each individual collection configures:

* a [live `loader`](#creating-a-live-loader) for your data source, and optionally for type safety (required)
* a [live collection `schema`](#using-zod-schemas-with-live-collections) for type safety (optional)

Unlike for build-time collections, there are no built-in live loaders available. You will need to [create a custom live loader](#creating-a-live-loader) for your specific data source or find a third-party loader to pass to your live collection’s `loader` property.

You can optionally [include type safety in your live loaders](/en/reference/content-loader-reference/#the-liveloader-object). Therefore, [defining a Zod `schema`](#using-zod-schemas-with-live-collections) for live collections is optional. However, if you provide one, it will take precedence over the live loader’s types.

src/live.config.ts

```ts
// Define live collections for accessing real-time data
import { defineLiveCollection } from 'astro:content';
import { storeLoader } from '@mystore/astro-loader';


const products = defineLiveCollection({
  loader: storeLoader({
    apiKey: process.env.STORE_API_KEY,
    endpoint: 'https://api.mystore.com/v1',
  }),
});


// Export a single `collections` object to register your collection(s)
export const collections = { products };
```

You can then use the dedicated `getLiveCollection()` and `getLiveEntry()` functions to [access your live data](#accessing-live-data) and render your content.

You can [generate page routes](#generating-routes-from-content) from your live collection entries on demand, fetching your data fresh at runtime upon each request without needing a rebuild of your site like [build-time collections](#defining-build-time-content-collections) do. This is useful when accessing live, up-to-the-moment data is more important than having your content available in a performant data storage layer that persists between site builds.

### Creating a live loader

[Section titled “Creating a live loader”](#creating-a-live-loader)

You can build a custom [live loader](/en/reference/content-loader-reference/#live-loaders) using the Live Loader API to fetch remote content fresh upon request from any data source, such as a CMS, a database or an API endpoint. You will have to tell your live loader how to fetch and return content entries from your desired data source, as well as provide error handling for unsuccessful data requests.

Using a live loader to fetch your data will automatically create a collection from your remote data. This gives you all the benefits of Astro’s content collections, including collection-specific API helpers such as `getLiveCollection()` and `render()` to [query and display your data](#querying-build-time-collections), as well as helpful error handling.

Tip

Find community-built and third-party live loaders in the [Astro integrations directory](https://astro.build/integrations/?search=\&categories%5B%5D=loaders).

See the basics of [building a live loader](/en/reference/content-loader-reference/#building-a-live-loader) using the Live Loader API

### Using Zod schemas with live collections

[Section titled “Using Zod schemas with live collections”](#using-zod-schemas-with-live-collections)

You can use Zod schemas with live collections to validate and transform data at runtime. This Zod validation works the same way as [schemas for build-time collections](#defining-the-collection-schema).

When you define a schema for a live collection, it takes precedence over [the live loader’s types](/en/reference/content-loader-reference/#the-liveloader-object) when you query the collection:

src/live.config.ts

```ts
import { defineLiveCollection } from 'astro:content';
import { z } from 'astro/zod';
import { apiLoader } from './loaders/api-loader';


const products = defineLiveCollection({
  loader: apiLoader({ endpoint: process.env.API_URL }),
  schema: z
    .object({
      id: z.string(),
      name: z.string(),
      price: z.number(),
      // Transform the API's category format
      category: z.string().transform((str) => str.toLowerCase().replace(/\s+/g, '-')),
      // Coerce the date to a Date object
      createdAt: z.coerce.date(),
    })
    .transform((data) => ({
      ...data,
      // Add a formatted price field
      displayPrice: `$${data.price.toFixed(2)}`,
    })),
});


export const collections = { products };
```

When using Zod schemas with live collections, validation errors are automatically caught and returned as `AstroError` objects:

src/pages/store/index.astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { LiveCollectionValidationError } from 'astro/content/runtime';
import { getLiveEntry } from 'astro:content';


const { entry, error } = await getLiveEntry('products', '123');


// You can handle validation errors specifically
if (LiveCollectionValidationError.is(error)) {
  console.error(error.message);
  return Astro.rewrite('/500');
}


// TypeScript knows entry.data matches your Zod schema, not the loader's type
console.log(entry?.data.displayPrice); // e.g., "$29.99"
---
```

See [Zod’s README](https://github.com/colinhacks/zod) for complete documentation on how Zod works and what features are available.

### Accessing live data

[Section titled “Accessing live data”](#accessing-live-data)

Astro provides live collection helper functions to access live data on each request and return one (or more) content entries. These can be used similarly to their [build-time collection counterparts](#querying-build-time-collections).

* [`getLiveCollection()`](/en/reference/modules/astro-content/#getlivecollection) fetches an entire collection and returns an array of entries.
* [`getLiveEntry()`](/en/reference/modules/astro-content/#getliveentry) fetches a single entry from a collection.

These return entries with a unique `id`, and `data` object with all defined properties from the live loader. When using third-party or community loaders distributed as npm packages, check their own documentation for the expected shape of data returned.

You can use these functions to access your live data, passing the name of the collection and optionally filtering conditions.

src/pages/store/\[slug].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getLiveCollection, getLiveEntry } from 'astro:content';


// Use loader-specific filters
const { entries: draftArticles } = await getLiveCollection('articles', {
  status: 'draft',
  author: 'john-doe',
});


// Get a specific product by ID
const { entry: product } = await getLiveEntry('products', Astro.params.slug);
---
```

#### Rendering content

[Section titled “Rendering content”](#rendering-content)

If your live loader [returns a `rendered` property](/en/reference/content-loader-reference/#livedataentryrendered), you can use [the `render()` function and `<Content />` component](#rendering-body-content) to render your content directly in your pages, using the same method as build-time collections.

You also have access to any [error returned by the live loader](/en/reference/content-loader-reference/#error-handling-in-live-loaders), for example, to rewrite to a 404 page when content cannot be displayed:

src/pages/store/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getLiveEntry, render } from 'astro:content';
const { entry, error } = await getLiveEntry('articles', Astro.params.id);
if (error) {
  return Astro.rewrite('/404');
}


const { Content } = await render(entry);
---


<h1>{entry.data.title}</h1>
<Content />
```

#### Error handling

[Section titled “Error handling”](#error-handling)

Live loaders can fail due to network issues, API errors, or validation problems. The API is designed to make error handling explicit.

When you call `getLiveCollection()` or `getLiveEntry()`, the error will be one of:

* The error type defined by the loader (if it returned an error)
* A `LiveEntryNotFoundError` if the entry was not found
* A `LiveCollectionValidationError` if the collection data does not match the expected schema
* A `LiveCollectionCacheHintError` if the cache hint is invalid
* A `LiveCollectionError` for other errors, such as uncaught errors thrown in the loader

You can use `instanceof` to check the type of an error at runtime:

src/pages/store/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { LiveEntryNotFoundError } from 'astro/content/runtime';
import { getLiveEntry } from 'astro:content';


const { entry, error } = await getLiveEntry('products', Astro.params.id);


if (error) {
  if (error instanceof LiveEntryNotFoundError) {
    console.error(`Product not found: ${error.message}`);
    Astro.response.status = 404;
  } else {
    console.error(`Error loading product: ${error.message}`);
    return Astro.redirect('/500');
  }
}
---
```

## Using JSON Schema files in your editor

[Section titled “Using JSON Schema files in your editor”](#using-json-schema-files-in-your-editor)

**Added in:** `astro@4.13.0`

Astro auto-generates [JSON Schema](https://json-schema.org/) files for collections, which you can use in your editor to get IntelliSense and type-checking for data files.

A JSON Schema file is generated for each collection in your project and output to the `.astro/collections/` directory. For example, if you have two collections, one named `authors` and another named `posts`, Astro will generate `.astro/collections/authors.schema.json` and `.astro/collections/posts.schema.json`.

### Use JSON Schemas in JSON files

You can manually point to an Astro-generated schema by setting the `$schema` field in your JSON file. The value should be a relative file path from the data file to the schema. In the following example, a data file in `src/data/authors/` uses the schema generated for the `authors` collection:

src/data/authors/armand.json

```diff
{
  +"$schema": "../../../.astro/collections/authors.schema.json",
  "name": "Armand",
  "skills": ["Astro", "Starlight"]
}
```

#### Use a schema for a group of JSON files in VS Code

In VS Code, you can configure a schema to apply to all files in a collection using the [`json.schemas` setting](https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings). In the following example, all files in the `src/data/authors/` directory will use the schema generated for the `authors` collection:

```json
{
  "json.schemas": [
    {
      "fileMatch": ["/src/data/authors/**"],
      "url": "./.astro/collections/authors.schema.json"
    }
  ]
}
```

### Use schemas in YAML files in VS Code

In VS Code, you can add support for using JSON schemas in YAML files using the [Red Hat YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) extension. With this extension installed, you can reference a schema in a YAML file using a special comment syntax:

src/data/authors/armand.yml

```diff
+# yaml-language-server: $schema=../../../.astro/collections/authors.schema.json
name: Armand
skills:
  - Astro
  - Starlight
```

#### Use schemas for a group of YAML files in VS Code

With the Red Hat YAML extension, you can configure a schema to apply to all YAML files in a collection using the `yaml.schemas` setting. In the following example, all YAML files in the `src/data/authors/` directory will use the schema generated for the `authors` collection:

```json
{
  "yaml.schemas": {
    "./.astro/collections/authors.schema.json": ["/src/content/authors/*.yml"]
  }
}
```

See [“Associating schemas”](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml#associating-schemas) in the Red Hat YAML extension documentation for more details.

# Data fetching

> Learn how to fetch remote data with Astro using the fetch API.

`.astro` files can fetch remote data to help you generate your pages.

## `fetch()` in Astro

[Section titled “fetch() in Astro”](#fetch-in-astro)

All [Astro components](/en/basics/astro-components/) have access to the [global `fetch()` function](https://developer.mozilla.org/en-US/docs/Web/API/fetch) in their component script to make HTTP requests to APIs using the full URL (e.g. `https://example.com/api`). Additionally, you can construct a URL to your project’s pages and endpoints that are rendered on demand on the server using [`new URL("/api", Astro.url)`](/en/reference/api-reference/#url).

This fetch call will be executed at build time, and the data will be available to the component template for generating dynamic HTML. If [SSR](/en/guides/on-demand-rendering/) mode is enabled, any fetch calls will be executed at runtime.

💡 Take advantage of [**top-level `await`**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) inside of your Astro component script.

💡 Pass fetched data to both Astro and framework components, as props.

src/components/User.astro

```astro
---
import Contact from "../components/Contact.jsx";
import Location from "../components/Location.astro";


const response = await fetch("https://randomuser.me/api/");
const data = await response.json();
const randomUser = data.results[0];
---
<!-- Data fetched at build can be rendered in HTML -->
<h1>User</h1>
<h2>{randomUser.name.first} {randomUser.name.last}</h2>


<!-- Data fetched at build can be passed to components as props -->
<Contact client:load email={randomUser.email} />
<Location city={randomUser.location.city} />
```

Note

Remember, all data in Astro components is fetched when a component is rendered.

Your deployed Astro site will fetch data **once, at build time**. In dev, you will see data fetches on component refreshes. If you need to re-fetch data multiple times client-side, use a [framework component](/en/guides/framework-components/) or a [client-side script](/en/guides/client-side-scripts/) in an Astro component.

## `fetch()` in Framework Components

[Section titled “fetch() in Framework Components”](#fetch-in-framework-components)

The `fetch()` function is also globally available to any [framework components](/en/guides/framework-components/):

src/components/Movies.tsx

```tsx
import type { FunctionalComponent } from 'preact';


const data = await fetch('https://example.com/movies.json').then((response) => response.json());


// Components that are build-time rendered also log to the CLI.
// When rendered with a `client:*` directive, they also log to the browser console.
console.log(data);


const Movies: FunctionalComponent = () => {
  // Output the result to the page
  return <div>{JSON.stringify(data)}</div>;
};


export default Movies;
```

## GraphQL queries

[Section titled “GraphQL queries”](#graphql-queries)

Astro can also use `fetch()` to query a GraphQL server with any valid GraphQL query.

src/components/Film.astro

```astro
---
const response = await fetch(
  "https://swapi-graphql.netlify.app/.netlify/functions/index",
  {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      query: `
        query getFilm ($id:ID!) {
          film(id: $id) {
            title
            releaseDate
          }
        }
      `,
      variables: {
        id: "ZmlsbXM6MQ==",
      },
    }),
  }
);




const json = await response.json();
const { film } = json.data;
---
<h1>Fetching information about Star Wars: A New Hope</h1>
<h2>Title: {film.title}</h2>
<p>Year: {film.releaseDate}</p>
```

## Fetch from a Headless CMS

[Section titled “Fetch from a Headless CMS”](#fetch-from-a-headless-cms)

Astro components can fetch data from your favorite CMS and then render it as your page content. Using [dynamic routes](/en/guides/routing/#dynamic-routes), components can even generate pages based on your CMS content.

See our [CMS Guides](/en/guides/cms/) for full details on integrating Astro with headless CMSes including Storyblok, Contentful, and WordPress.

## Community resources

[Section titled “Community resources”](#community-resources)

* [Creating a fullstack app with Astro + GraphQL](https://robkendal.co.uk/blog/how-to-build-astro-site-with-graphql/)


---

# Routing

> An intro to routing with Astro.

Astro uses **file-based routing** to generate your build URLs based on the file layout of your project `src/pages/` directory.

## Navigating between pages

[Section titled “Navigating between pages”](#navigating-between-pages)

Astro uses standard HTML [`<a>` elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) to navigate between routes. There is no framework-specific `<Link>` component provided.

src/pages/index.astro

```astro
<p>Read more <a href="/about/">about</a> Astro!</p>


<!-- With `base: "/docs"` configured -->
<p>Learn more in our <a href="/docs/reference/">reference</a> section!</p>
```

## Static routes

[Section titled “Static routes”](#static-routes)

`.astro` [page components](/en/basics/astro-pages/) as well as Markdown and MDX Files (`.md`, `.mdx`) within the `src/pages/` directory **automatically become pages on your website**. Each page’s route corresponds to its path and filename within the `src/pages/` directory.

```diff
# Example: Static routes
src/pages/index.astro        -> mysite.com/
src/pages/about.astro        -> mysite.com/about
src/pages/about/index.astro  -> mysite.com/about
src/pages/about/me.astro     -> mysite.com/about/me
src/pages/posts/1.md         -> mysite.com/posts/1
```

Tip

There is no separate “routing config” to maintain in an Astro project! When you add a file to the `src/pages/` directory, a new route is automatically created for you. In static builds, you can customize the file output format using the [`build.format`](/en/reference/configuration-reference/#buildformat) configuration option.

## Dynamic routes

[Section titled “Dynamic routes”](#dynamic-routes)

An Astro page file can specify dynamic route parameters in its filename to generate multiple, matching pages. For example, `src/pages/authors/[author].astro` generates a bio page for every author on your blog. `author` becomes a *parameter* that you can access from inside the page.

In Astro’s default static output mode, these pages are generated at build time, and so you must predetermine the list of `author`s that get a corresponding file. In SSR mode, a page will be generated on request for any route that matches.

### Static (SSG) Mode

[Section titled “Static (SSG) Mode”](#static-ssg-mode)

Because all routes must be determined at build time, a dynamic route must export a `getStaticPaths()` that returns an array of objects with a `params` property. Each of these objects will generate a corresponding route.

`[dog].astro` defines the dynamic `dog` parameter in its filename, so the objects returned by `getStaticPaths()` must include `dog` in their `params`. The page can then access this parameter using `Astro.params`.

src/pages/dogs/\[dog].astro

```astro
---
export function getStaticPaths() {
  return [
    { params: { dog: "clifford" }},
    { params: { dog: "rover" }},
    { params: { dog: "spot" }},
  ];
}


const { dog } = Astro.params;
---
<div>Good dog, {dog}!</div>
```

This will generate three pages: `/dogs/clifford`, `/dogs/rover`, and `/dogs/spot`, each displaying the corresponding dog name.

The filename can include multiple parameters, which must all be included in the `params` objects in `getStaticPaths()`:

src/pages/\[lang]-\[version]/info.astro

```astro
---
export function getStaticPaths() {
  return [
    { params: { lang: "en", version: "v1" }},
    { params: { lang: "fr", version: "v2" }},
  ];
}


const { lang, version } = Astro.params;
---
```

This will generate `/en-v1/info` and `/fr-v2/info`.

Parameters can be included in separate parts of the path. For example, the file `src/pages/[lang]/[version]/info.astro` with the same `getStaticPaths()` above will generate the routes `/en/v1/info` and `/fr/v2/info`.

#### Decoding `params`

[Section titled “Decoding params”](#decoding-params)

`params` returned by a `getStaticPaths()` function are not decoded. Use [`decodeURI()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURI) when you need to decode parameter values.

src/pages/\[slug].astro

```astro
---
export function getStaticPaths() {
  return [
    { params: { slug: decodeURI("%5Bpage%5D") }}, // decodes to "[page]"
  ]
}
---
```

Learn more about [`getStaticPaths()`](/en/reference/routing-reference/#getstaticpaths).

![](/houston_chef.webp) **Related recipe:** [Add i18n features](/en/recipes/i18n/)

#### Rest parameters

[Section titled “Rest parameters”](#rest-parameters)

If you need more flexibility in your URL routing, you can use a [rest parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) (`[...path]`) in your `.astro` filename to match file paths of any depth:

src/pages/sequences/\[...path].astro

```astro
---
export function getStaticPaths() {
  return [
    { params: { path: "one/two/three" }},
    { params: { path: "four" }},
    { params: { path: undefined }}
  ]
}


const { path } = Astro.params;
---
```

This will generate `/sequences/one/two/three`, `/sequences/four`, and `/sequences`. (Setting the rest parameter to `undefined` allows it to match the top level page.)

Rest parameters can be used with **other named parameters**. For example, GitHub’s file viewer can be represented with the following dynamic route:

```plaintext
/[org]/[repo]/tree/[branch]/[...file]
```

In this example, a request for `/withastro/astro/tree/main/docs/public/favicon.svg` would be split into the following named parameters:

```js
{
  org: "withastro",
  repo: "astro",
  branch: "main",
  file: "docs/public/favicon.svg"
}
```

#### Example: Dynamic pages at multiple levels

[Section titled “Example: Dynamic pages at multiple levels”](#example-dynamic-pages-at-multiple-levels)

In the following example, a rest parameter (`[...slug]`) and the [`props`](/en/reference/routing-reference/#data-passing-with-props) feature of `getStaticPaths()` generate pages for slugs of different depths.

src/pages/\[...slug].astro

```astro
---
export function getStaticPaths() {
  const pages = [
    {
      slug: undefined,
      title: "Astro Store",
      text: "Welcome to the Astro store!",
    },
    {
      slug: "products",
      title: "Astro products",
      text: "We have lots of products for you",
    },
    {
      slug: "products/astro-handbook",
      title: "The ultimate Astro handbook",
      text: "If you want to learn Astro, you must read this book.",
    },
  ];


  return pages.map(({ slug, title, text }) => {
    return {
      params: { slug },
      props: { title, text },
    };
  });
}


const { title, text } = Astro.props;
---
<html>
  <head>
    <title>{title}</title>
  </head>
  <body>
    <h1>{title}</h1>
    <p>{text}</p>
  </body>
</html>
```

### On-demand dynamic routes

[Section titled “On-demand dynamic routes”](#on-demand-dynamic-routes)

For [on-demand rendering](/en/guides/on-demand-rendering/) with an adapter, dynamic routes are defined the same way: include `[param]` or `[...path]` brackets in your file names to match arbitrary strings or paths. But because the routes are no longer built ahead of time, the page will be served to any matching route. Since these are not “static” routes, `getStaticPaths` should not be used.

For on-demand rendered routes, only one rest parameter using the spread notation may be used in the file name (e.g. `src/pages/[locale]/[...slug].astro` or `src/pages/[...locale]/[slug].astro`, but not `src/pages/[...locale]/[...slug].astro`).

src/pages/resources/\[resource]/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode
const { resource, id } = Astro.params;
---
<h1>{resource}: {id}</h1>
```

This page will be served for any value of `resource` and `id`: `resources/users/1`, `resources/colors/blue`, etc.

#### Modifying the `[...slug]` example for SSR

[Section titled “Modifying the \[...slug\] example for SSR”](#modifying-the-slug-example-for-ssr)

Because SSR pages can’t use `getStaticPaths()`, they can’t receive props. The [previous example](#example-dynamic-pages-at-multiple-levels) can be adapted for SSR mode by looking up the value of the `slug` param in an object. If the route is at the root (”/”), the `slug` param will be `undefined`. If the value doesn’t exist in the object, we redirect to a 404 page.

src/pages/\[...slug].astro

```astro
---
const pages = [
  {
    slug: undefined,
    title: 'Astro Store',
    text: 'Welcome to the Astro store!',
  },
  {
    slug: 'products',
    title: 'Astro products',
    text: 'We have lots of products for you',
  },
  {
    slug: 'products/astro-handbook',
    title: 'The ultimate Astro handbook',
    text: 'If you want to learn Astro, you must read this book.',
  }
];


const { slug } = Astro.params;
const page = pages.find((page) => page.slug === slug);
if (!page) return Astro.redirect("/404");
const { title, text } = page;
---
<html>
  <head>
    <title>{title}</title>
  </head>
  <body>
    <h1>{title}</h1>
    <p>{text}</p>
  </body>
</html>
```

## Redirects

[Section titled “Redirects”](#redirects)

Sometimes you will need to redirect your readers to a new page, either permanently because your site structure has changed or in response to an action such as logging in to an authenticated route.

You can define rules to [redirect users to permanently-moved pages](#configured-redirects) in your Astro config. Or, [redirect users dynamically](#dynamic-redirects) as they use your site.

### Configured Redirects

[Section titled “Configured Redirects”](#configured-redirects)

**Added in:** `astro@2.9.0`

You can specify a mapping of permanent redirects in your Astro config with the [`redirects`](/en/reference/configuration-reference/#redirects) value.

For internal redirects, this is a mapping of an old route path to the new route. As of Astro v5.2.0, it is also possible to redirect to external URLs that start with `http` or `https` and [can be parsed](https://developer.mozilla.org/en-US/docs/Web/API/URL/canParse_static):

astro.config.mjs

```js
import { defineConfig } from "astro/config";


export default defineConfig({
  redirects: {
    "/old-page": "/new-page",
    "/blog": "https://example.com/blog"
  }
});
```

These redirects follow [the same priority rules as file-based routes](#route-priority-order) and will always take lower precedence than an existing page file of the same name in your project. For example, `/old-page` will not redirect to `/new-page` if your project contains the file `src/pages/old-page.astro`.

Dynamic routes are allowed as long as both the new and old routes contain the same parameters, for example:

```js
{
  "/blog/[...slug]": "/articles/[...slug]"
}
```

Using SSR or a static adapter, you can also provide an object as the value, allowing you to specify the `status` code in addition to the new `destination`:

astro.config.mjs

```js
import { defineConfig } from "astro/config";


export default defineConfig({
  redirects: {
    "/old-page": {
      status: 302,
      destination: "/new-page"
    },
    "/news": {
      status: 302,
      destination: "https://example.com/news"
    }
  }
});
```

When running `astro build`, Astro will output HTML files with the [meta refresh](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#examples) tag by default. Supported adapters will instead write out the host’s configuration file with the redirects.

The status code is `301` by default. If building to HTML files the status code is not used by the server.

### Dynamic redirects

[Section titled “Dynamic redirects”](#dynamic-redirects)

On the `Astro` global, the `Astro.redirect` method allows you to redirect to another page dynamically. You might do this after checking if the user is logged in by getting their session from a cookie.

src/pages/account.astro

```astro
---
import { isLoggedIn } from "../utils";


const cookie = Astro.request.headers.get("cookie");


// If the user is not logged in, redirect them to the login page
if (!isLoggedIn(cookie)) {
  return Astro.redirect("/login");
}
---
```

Because Astro uses [HTML streaming](/en/guides/on-demand-rendering/#html-streaming) in on-demand rendering, redirects must be done at the page level, not inside child components.

## Rewrites

[Section titled “Rewrites”](#rewrites)

**Added in:** `astro@4.13.0`

A rewrite allows you to serve a different route without redirecting the browser to a different page. The browser will show the original address in the URL bar, but will instead display the content of the URL provided to [`Astro.rewrite()`](/en/reference/api-reference/#rewrite).

Tip

For content that has permanently moved, or to direct your user to a different page with a new URL (e.g. a user dashboard after logging in), use a [redirect](#redirects) instead.

Rewrites can be useful for showing the same content at multiple paths (e.g. `/products/shoes/men/` and `/products/men/shoes/`) without needing to maintain two different source files.

Rewrites are also useful for SEO purposes and user experience. They allow you to display content that otherwise would require redirecting your visitor to a different page or would return a 404 status. One common use of rewrites is to show the same localized content for different variants of a language.

The following example uses a rewrite to render the `/es/` version of a page when the `/es-CU/` (Cuban Spanish) URL path is visited. When a visitor navigates to the URL `/es-cu/articles/introduction`, Astro will render the content generated by the file `src/pages/es/articles/introduction.astro`.

src/pages/es-cu/articles/introduction.astro

```astro
---
return Astro.rewrite("/es/articles/introduction");
---
```

Use `context.rewrite()` in your endpoint files to reroute to a different page:

src/pages/api.js

```js
export function GET(context) {
  if (!context.locals.allowed) {
    return context.rewrite("/");
  }
}
```

If the URL passed to `Astro.rewrite()` emits a runtime error, Astro will show the overlay error in development and return a 500 status code in production. If the URL does not exist in your project, a 404 status code will be returned.

You can intentionally create a rewrite to render your `/404` page, for example to indicate that a product in your e-commerce shop is no longer available:

src/pages/\[item].astro

```astro
---
const { item } = Astro.params;


if (!itemExists(item)) {
  return Astro.rewrite("/404");
}
---
```

You can also conditionally rewrite based on an HTTP response status, for example to display a certain page on your site when visiting a URL that doesn’t exist:

src/middleware.mjs

```js
export const onRequest = async (context, next) => {
  const response = await next();
  if (response.status === 404) {
    return context.rewrite("/");
  }
  return response;
}
```

Before displaying the content from the specified rewrite path, the function `Astro.rewrite()` will trigger a new, complete rendering phase. This re-executes any middleware for the new route/request.

See the [`Astro.rewrite()` API reference](/en/reference/api-reference/#rewrite) for more information.

## Route Priority Order

[Section titled “Route Priority Order”](#route-priority-order)

It’s possible for multiple defined routes to attempt to build the same URL path. For example, all of these routes could build `/posts/create`:

* src/pages/

  * \[…slug].astro

  * posts/

    * create.astro
    * \[page].astro
    * \[pid].ts
    * \[…slug].astro

Astro needs to know which route should be used to build the page. To do so, it sorts them according to the following rules in order:

* Astro [reserved routes](#reserved-routes)
* Routes with more path segments will take precedence over less specific routes. In the example above, all routes under `/posts/` take precedence over `/[...slug].astro` at the root.
* Static routes without path parameters will take precedence over dynamic routes. E.g. `/posts/create.astro` takes precedence over all the other routes in the example.
* Dynamic routes using named parameters take precedence over rest parameters. E.g. `/posts/[page].astro` takes precedence over `/posts/[...slug].astro`.
* Pre-rendered dynamic routes take precedence over server dynamic routes.
* Endpoints take precedence over pages.
* File-based routes take precedence over redirects.
* If none of the rules above decide the order, routes are sorted alphabetically based on the default locale of your Node installation.

Given the example above, here are a few examples of how the rules will match a requested URL to the route used to build the HTML:

* `pages/posts/create.astro` - Will build only `/posts/create`
* `pages/posts/[pid].ts` - Will build `/posts/abc`, `/posts/xyz`, etc. But not `/posts/create`
* `pages/posts/[page].astro` - Will build `/posts/1`, `/posts/2`, etc. But not `/posts/create`, `/posts/abc` nor `/posts/xyz`
* `pages/posts/[...slug].astro` - Will build `/posts/1/2`, `/posts/a/b/c`, etc. But not `/posts/create`, `/posts/1`, `/posts/abc`, etc.
* `pages/[...slug].astro` - Will build `/abc`, `/xyz`, `/abc/xyz`, etc. But not `/posts/create`, `/posts/1`, `/posts/abc`, etc.

### Reserved routes

[Section titled “Reserved routes”](#reserved-routes)

Internal routes take priority over any user-defined or integration-defined routes as they are required for Astro features to work. The following are Astro’s reserved routes:

* `_astro/`: Serves all of the static assets to the client, including CSS documents, bundled client scripts, optimized images, and any Vite assets.
* `_server_islands/`: Serves the dynamic components deferred into a [server island](/en/guides/server-islands/).
* `_actions/`: Serves any defined [actions](/en/guides/actions/).

## Pagination

[Section titled “Pagination”](#pagination)

Astro supports built-in pagination for large collections of data that need to be split into multiple pages. Astro will generate common pagination properties, including previous/next page URLs, total number of pages, and more.

Paginated route names should use the same `[bracket]` syntax as a standard dynamic route. For instance, the file name `/astronauts/[page].astro` will generate routes for `/astronauts/1`, `/astronauts/2`, etc, where `[page]` is the generated page number.

You can use the `paginate()` function to generate these pages for an array of values like so:

src/pages/astronauts/\[page].astro

```astro
---
export function getStaticPaths({ paginate }) {
  const astronautPages = [
    { astronaut: "Neil Armstrong" },
    { astronaut: "Buzz Aldrin" },
    { astronaut: "Sally Ride" },
    { astronaut: "John Glenn" },
  ];


  // Generate pages from our array of astronauts, with 2 to a page
  return paginate(astronautPages, { pageSize: 2 });
}
// All paginated data is passed on the "page" prop
const { page } = Astro.props;
---
<!-- Display the current page number. `Astro.params.page` can also be used! -->
<h1>Page {page.currentPage}</h1>
<ul>
  <!-- List the array of astronaut info -->
  {page.data.map(({ astronaut }) => <li>{astronaut}</li>)}
</ul>
```

This generates the following pages, with 2 items to a page:

* `/astronauts/1` - Page 1: Displays “Neil Armstrong” and “Buzz Aldrin”
* `/astronauts/2` - Page 2: Displays “Sally Ride” and “John Glenn”

### The `page` prop

[Section titled “The page prop”](#the-page-prop)

When you use the `paginate()` function, each page will be passed its data via a `page` prop. The `page` prop has many useful properties that you can use to build pages and links between them:

```ts
interface Page<T = any> {
  /** array containing the page’s slice of data that you passed to the paginate() function */
  data: T[];
  /** metadata */
  /** the count of the first item on the page, starting from 0 */
  start: number;
  /** the count of the last item on the page, starting from 0 */
  end: number;
  /** total number of results */
  total: number;
  /** the current page number, starting from 1 */
  currentPage: number;
  /** number of items per page (default: 10) */
  size: number;
  /** number of last page */
  lastPage: number;
  url: {
    /** url of the current page */
    current: string;
    /** url of the previous page (if there is one) */
    prev: string | undefined;
    /** url of the next page (if there is one) */
    next: string | undefined;
    /** url of the first page (if the current page is not the first page) */
    first: string | undefined;
    /** url of the last page (if the current page in not the last page) */
    last: string | undefined;
  };
}
```

The following example displays current information for the page along with links to navigate between pages:

src/pages/astronauts/\[page].astro

```astro
---
// Paginate same list of `{ astronaut }` objects as the previous example
export function getStaticPaths({ paginate }) { /* ... */ }
const { page } = Astro.props;
---
<h1>Page {page.currentPage}</h1>
<ul>
  {page.data.map(({ astronaut }) => <li>{astronaut}</li>)}
</ul>
{page.url.first ? <a href={page.url.first}>First</a> : null}
{page.url.prev ? <a href={page.url.prev}>Previous</a> : null}
{page.url.next ? <a href={page.url.next}>Next</a> : null}
{page.url.last ? <a href={page.url.last}>Last</a> : null}
```

Learn more about [the pagination `page` prop](/en/reference/routing-reference/#the-pagination-page-prop).

### Nested Pagination

[Section titled “Nested Pagination”](#nested-pagination)

A more advanced use-case for pagination is **nested pagination.** This is when pagination is combined with other dynamic route params. You can use nested pagination to group your paginated collection by some property or tag.

For example, if you want to group your paginated Markdown posts by some tag, you would use nested pagination by creating a `/src/pages/[tag]/[page].astro` page that would match the following URLS:

* `/red/1` (tag=red)
* `/red/2` (tag=red)
* `/blue/1` (tag=blue)
* `/green/1` (tag=green)

Nested pagination works by returning an array of `paginate()` results from `getStaticPaths()`, one for each grouping.

In the following example, we will implement nested pagination to build the URLs listed above:

src/pages/\[tag]/\[page].astro

```astro
---
export function getStaticPaths({ paginate }) {
  const allTags = ["red", "blue", "green"];
  const allPosts = Object.values(import.meta.glob("../pages/post/*.md", { eager: true }));
  // For every tag, return a `paginate()` result.
  // Make sure that you pass `{ params: { tag }}` to `paginate()`
  // so that Astro knows which tag grouping the result is for.
  return allTags.flatMap((tag) => {
    const filteredPosts = allPosts.filter((post) => post.frontmatter.tag === tag);
    return paginate(filteredPosts, {
      params: { tag },
      pageSize: 10
    });
  });
}


const { page } = Astro.props;
const params = Astro.params;
```

## Excluding pages

[Section titled “Excluding pages”](#excluding-pages)

You can exclude pages or directories within `src/pages` from being built by prefixing their names with an underscore (`_`). Files with the `_` prefix won’t be recognized by the router and won’t be placed into the `dist/` directory.

You can use this to temporarily disable pages, and also to put tests, utilities, and components in the same folder as their related pages.

In this example, only `src/pages/index.astro` and `src/pages/projects/project1.md` will be built as page routes and HTML files.

* src/pages/

  * \_hidden-directory/

    * page1.md
    * page2.md

  * \_hidden-page.astro

  * **index.astro**

  * projects/

    * \_SomeComponent.astro
    * \_utils.js
    * **project1.md**


---

# Styles and CSS

> Learn how to style components in Astro with scoped styles, external CSS, and tooling like Sass and PostCSS.

Astro was designed to make styling and writing CSS a breeze. Write your own CSS directly inside of an Astro component or import your favorite CSS library like [Tailwind](https://tailwindcss.com/docs/installation/framework-guides/astro). Advanced styling languages like [Sass](https://sass-lang.com/) and [Less](https://lesscss.org/) are also supported.

## Styling in Astro

[Section titled “Styling in Astro”](#styling-in-astro)

Styling an Astro component is as easy as adding a `<style>` tag to your component or page template. When you place a `<style>` tag inside of an Astro component, Astro will detect the CSS and handle your styles for you, automatically.

src/components/MyComponent.astro

```astro
<style>
  h1 { color: red; }
</style>
```

### Scoped Styles

[Section titled “Scoped Styles”](#scoped-styles)

Astro `<style>` CSS rules are automatically **scoped by default**. Scoped styles are compiled behind-the-scenes to only apply to HTML written inside of that same component. The CSS that you write inside of an Astro component is automatically encapsulated inside of that component.

This CSS:

src/pages/index.astro

```astro
<style>
  h1 {
    color: red;
  }


  .text {
    color: blue;
  }
</style>
```

Compiles to this:

```astro
<style>
  h1[data-astro-cid-hhnqfkh6] {
     color: red;
  }


  .text[data-astro-cid-hhnqfkh6] {
    color: blue;
  }
</style>
```

Scoped styles don’t leak and won’t impact the rest of your site. In Astro, it is okay to use low-specificity selectors like `h1 {}` or `p {}` because they will be compiled with scopes in the final output.

Scoped styles also won’t apply to other Astro components contained inside of your template. If you need to style a child component, consider wrapping that component in a `<div>` (or other element) that you can then style.

The specificity of scoped styles is preserved, allowing them to work consistently alongside other CSS files or CSS libraries while still preserving the exclusive boundaries that prevent styles from applying outside the component.

### Global Styles

[Section titled “Global Styles”](#global-styles)

While we recommend scoped styles for most components, you may eventually find a valid reason to write global, unscoped CSS. You can opt-out of automatic CSS scoping with the `<style is:global>` attribute.

src/components/GlobalStyles.astro

```astro
<style is:global>
  /* Unscoped, delivered as-is to the browser.
     Applies to all <h1> tags on your site. */
  h1 { color: red; }
</style>
```

You can also mix global & scoped CSS rules together in the same `<style>` tag using the `:global()` selector. This becomes a powerful pattern for applying CSS styles to children of your component.

src/components/MixedStyles.astro

```astro
<style>
  /* Scoped to this component, only. */
  h1 { color: red; }
  /* Mixed: Applies to child `h1` elements only. */
  article :global(h1) {
    color: blue;
  }
</style>
<h1>Title</h1>
<article><slot /></article>
```

This is a great way to style things like blog posts, or documents with CMS-powered content where the contents live outside of Astro. But be careful: components whose appearance differs based on whether or not they have a certain parent component can become difficult to troubleshoot.

Scoped styles should be used as often as possible. Global styles should be used only as-needed.

### Combining classes with `class:list`

[Section titled “Combining classes with class:list”](#combining-classes-with-classlist)

If you need to combine classes on an element dynamically, you can use the `class:list` utility attribute in `.astro` files.

src/components/ClassList.astro

```astro
---
const { isRed } = Astro.props;
---
<!-- If `isRed` is truthy, class will be "box red". -->
<!-- If `isRed` is falsy, class will be "box". -->
<div class:list={['box', { red: isRed }]}><slot /></div>


<style>
  .box { border: 1px solid blue; }
  .red { border-color: red; }
</style>
```

See our [directives reference](/en/reference/directives-reference/#classlist) page to learn more about `class:list`.

### CSS Variables

[Section titled “CSS Variables”](#css-variables)

**Added in:** `astro@0.21.0`

The Astro `<style>` can reference any CSS variables available on the page. You can also pass CSS variables directly from your component frontmatter using the `define:vars` directive.

src/components/DefineVars.astro

```astro
---
const foregroundColor = "rgb(221 243 228)";
const backgroundColor = "rgb(24 121 78)";
---
<style define:vars={{ foregroundColor, backgroundColor }}>
  h1 {
    background-color: var(--backgroundColor);
    color: var(--foregroundColor);
  }
</style>
<h1>Hello</h1>
```

See our [directives reference](/en/reference/directives-reference/#definevars) page to learn more about `define:vars`.

### Passing a `class` to a child component

[Section titled “Passing a class to a child component”](#passing-a-class-to-a-child-component)

In Astro, HTML attributes like `class` do not automatically pass through to child components.

Instead, accept a `class` prop in the child component and apply it to the root element. When destructuring, you must rename it, because `class` is a [reserved word](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#reserved_words) in JavaScript.

Using the default scoped style strategy, you must also pass the `data-astro-cid-*` attribute. You can do this by passing the `...rest` of the props to the component. If you have changed `scopedStyleStrategy` to `'class'` or `'where'`, the `...rest` prop is not necessary.

src/components/MyComponent.astro

```astro
---
const { class: className, ...rest } = Astro.props;
---
<div class={className} {...rest}>
  <slot/>
</div>
```

src/pages/index.astro

```astro
---
import MyComponent from "../components/MyComponent.astro"
---
<style>
  .red {
    color: red;
  }
</style>
<MyComponent class="red">This will be red!</MyComponent>
```

Scoped styles from parent components

Because the `data-astro-cid-*` attribute includes the child in its parent’s scope, it is possible for styles to cascade from parent to child. To avoid this having unintended side effects, ensure you use unique class names in the child component.

### Inline styles

[Section titled “Inline styles”](#inline-styles)

You can style HTML elements inline using the `style` attribute. This can be a CSS string or an object of CSS properties:

src/pages/index.astro

```astro
// These are equivalent:
<p style={{ color: "brown", textDecoration: "underline" }}>My text</p>
<p style="color: brown; text-decoration: underline;">My text</p>
```

## External Styles

[Section titled “External Styles”](#external-styles)

There are two ways to resolve external global stylesheets: an ESM import for files located within your project source, and an absolute URL link for files in your `public/` directory, or hosted outside of your project.

Read more about using [static assets](/en/guides/imports/) located in `public/` or `src/`.

### Import a local stylesheet

[Section titled “Import a local stylesheet”](#import-a-local-stylesheet)

Using an npm package?

You may need to update your `astro.config` when importing from npm packages. See the [“import stylesheets from an npm package” section](#import-a-stylesheet-from-an-npm-package) below.

You can import stylesheets in your Astro component frontmatter using ESM import syntax. CSS imports work like [any other ESM import in an Astro component](/en/basics/astro-components/#the-component-script), which should be referenced as **relative to the component** and must be written at the **top** of your component script, with any other imports.

src/pages/index.astro

```astro
---
// Astro will bundle and optimize this CSS for you automatically
// This also works for preprocessor files like .scss, .styl, etc.
import '../styles/utils.css';
---
<html><!-- Your page here --></html>
```

CSS `import` via ESM are supported inside of any JavaScript file, including JSX components like React & Preact. This can be useful for writing granular, per-component styles for your React components.

### Import a stylesheet from an npm package

[Section titled “Import a stylesheet from an npm package”](#import-a-stylesheet-from-an-npm-package)

You may also need to load stylesheets from an external npm package. This is especially common for utilities like [Open Props](https://open-props.style/). If your package **recommends using a file extension** (i.e. `package-name/styles.css` instead of `package-name/styles`), this should work like any local stylesheet:

src/pages/random-page.astro

```astro
---
import 'package-name/styles.css';
---
<html><!-- Your page here --></html>
```

If your package **does not suggest using a file extension** (i.e. `package-name/styles`), you’ll need to update your Astro config first!

Say you are importing a CSS file from `package-name` called `normalize` (with the file extension omitted). To ensure we can prerender your page correctly, add `package-name` to [the `vite.ssr.noExternal` array](https://vite.dev/config/ssr-options.html#ssr-noexternal):

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
  vite: {
    ssr: {
+      noExternal: ['package-name'],
    }
  }
})
```

Note

This is a [Vite-specific setting](https://vite.dev/config/ssr-options.html#ssr-noexternal) that does *not* relate to (or require) [Astro SSR](/en/guides/on-demand-rendering/).

Now, you are free to import `package-name/normalize`. This will be bundled and optimized by Astro like any other local stylesheet.

src/pages/random-page.astro

```astro
---
import 'package-name/normalize';
---
<html><!-- Your page here --></html>
```

### Load a static stylesheet via “link” tags

[Section titled “Load a static stylesheet via “link” tags”](#load-a-static-stylesheet-via-link-tags)

You can also use the `<link>` element to load a stylesheet on the page. This should be an absolute URL path to a CSS file located in your `/public` directory, or an URL to an external website. Relative `<link>` href values are not supported.

src/pages/index.astro

```astro
<head>
  <!-- Local: /public/styles/global.css -->
  <link rel="stylesheet" href="/styles/global.css" />
  <!-- External -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/prismjs@1.24.1/themes/prism-tomorrow.css" />
</head>
```

Because this approach uses the `public/` directory, it skips the normal CSS processing, bundling and optimizations that are provided by Astro. If you need these transformations, use the [Import a Stylesheet](#import-a-local-stylesheet) method above.

## Cascading Order

[Section titled “Cascading Order”](#cascading-order)

Astro components will sometimes have to evaluate multiple sources of CSS. For example, your component might import a CSS stylesheet, include its own `<style>` tag, *and* be rendered inside a layout that imports CSS.

When conflicting CSS rules apply to the same element, browsers first use *specificity* and then *order of appearance* to determine which value to show.

If one rule is more *specific* than another, no matter where the CSS rule appears, its value will take precedence:

src/components/MyComponent.astro

```astro
<style>
  h1 { color: red }
  div > h1 {
    color: purple
  }
</style>
<div>
  <h1>
    This header will be purple!
  </h1>
</div>
```

If two rules have the same specificity, then the *order of appearance* is evaluated, and the last rule’s value will take precedence:

src/components/MyComponent.astro

```astro
<style>
  h1 { color: purple }
  h1 { color: red }
</style>
<div>
  <h1>
    This header will be red!
  </h1>
</div>
```

Astro CSS rules are evaluated in this order of appearance:

* **`<link>` tags in the head** (lowest precedence)
* **imported styles**
* **scoped styles** (highest precedence)

### Scoped Styles

[Section titled “Scoped Styles”](#scoped-styles-1)

Depending on your chosen value for [`scopedStyleStrategy`](/en/reference/configuration-reference/#scopedstylestrategy), scoped styles may or may not increase the [CLASS column specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity#class_column).

However, [scoped styles](#scoped-styles) will always come last in the order of appearance. These styles will therefore take precedence over other styles of the same specificity. For example, if you import a stylesheet that conflicts with a scoped style, the scoped style’s value will apply:

src/components/make-it-purple.css

```css
h1 {
  color: purple;
}
```

src/components/MyComponent.astro

```astro
---
import "./make-it-purple.css"
---
<style>
  h1 { color: red }
</style>
<div>
  <h1>
    This header will be red!
  </h1>
</div>
```

Scoped styles will be overwritten if the imported style is more specific. The style with a higher specificity will take precedence over the scoped style:

src/components/make-it-purple.css

```css
#intro {
  color: purple;
}
```

src/components/MyComponent.astro

```astro
---
import "./make-it-purple.css"
---
<style>
  h1 { color: red }
</style>
<div>
  <h1 id="intro">
    This header will be purple!
  </h1>
</div>
```

### Import Order

[Section titled “Import Order”](#import-order)

When importing multiple stylesheets in an Astro component, the CSS rules are evaluated in the order that they are imported. A higher specificity will always determine which styles to show, no matter when the CSS is evaluated. But, when conflicting styles have the same specificity, the *last one imported* wins:

src/components/make-it-purple.css

```css
div > h1 {
  color: purple;
}
```

src/components/make-it-green.css

```css
div > h1 {
  color: green;
}
```

src/components/MyComponent.astro

```astro
---
import "./make-it-green.css"
import "./make-it-purple.css"
---
<style>
  h1 { color: red }
</style>
<div>
  <h1>
    This header will be purple!
  </h1>
</div>
```

While `<style>` tags are scoped and only apply to the component that declares them, *imported* CSS can “leak”. Importing a component applies any CSS it imports, even if the component is never used:

src/components/PurpleComponent.astro

```astro
---
import "./make-it-purple.css"
---
<div>
  <h1>I import purple CSS.</h1>
</div>
```

src/components/MyComponent.astro

```astro
---
import "./make-it-green.css"
import PurpleComponent from "./PurpleComponent.astro";
---
<style>
  h1 { color: red }
</style>
<div>
  <h1>
    This header will be purple!
  </h1>
</div>
```

Tip

A common pattern in Astro is to import global CSS inside a [Layout component](/en/basics/layouts/). Be sure to import the Layout component before other imports so that it has the lowest precedence.

### Link Tags

[Section titled “Link Tags”](#link-tags)

Style sheets loaded via [link tags](#load-a-static-stylesheet-via-link-tags) are evaluated in order, before any other styles in an Astro file. Therefore, these styles will have lower precedence than imported stylesheets and scoped styles:

src/pages/index.astro

```astro
---
import "../components/make-it-purple.css"
---


<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro</title>
    <link rel="stylesheet" href="/styles/make-it-blue.css" />
  </head>
  <body>
    <div>
      <h1>This will be purple</h1>
    </div>
  </body>
</html>
```

## Tailwind

[Section titled “Tailwind”](#tailwind)

Astro comes with support for adding popular CSS libraries, tools, and frameworks to your project like [Tailwind](https://tailwindcss.com) and more!

Astro supports both Tailwind 3 and 4. You can [add Tailwind 4 support through a Vite plugin](#add-tailwind-4) to your project with a CLI command, or install legacy dependencies manually to add [Tailwind 3 support through an Astro integration](#legacy-tailwind-3-support).

To [upgrade your Astro project from Tailwind 3 to 4](#upgrade-from-tailwind-3) you will need to both add Tailwind 4 support, and remove legacy Tailwind 3 support.

### Add Tailwind 4

[Section titled “Add Tailwind 4”](#add-tailwind-4)

In Astro `>=5.2.0`, use the `astro add tailwind` command for your package manager to install the official Vite Tailwind plugin. To add Tailwind 4 support to earlier versions of Astro, follow the [instructions in the Tailwind docs](https://tailwindcss.com/docs/installation/framework-guides/astro) to add the `@tailwindcss/vite` Vite plugin manually.

* npm

  ```shell
  npx astro add tailwind
  ```

* pnpm

  ```shell
  pnpm astro add tailwind
  ```

* Yarn

  ```shell
  yarn astro add tailwind
  ```

Then, import `tailwindcss` into `src/styles/global.css` (or another CSS file of your choosing) to make Tailwind classes available to your Astro project. This file including the import will be created by default if you used the `astro add tailwind` command to install the Vite plugin.

src/styles/global.css

```css
@import "tailwindcss";
```

Import this file in the pages where you want Tailwind to apply. This is often done in a layout component so that Tailwind styles can be used on all pages sharing that layout:

src/layouts/Layout.astro

```astro
---
import "../styles/global.css";
---
```

### Upgrade from Tailwind 3

[Section titled “Upgrade from Tailwind 3”](#upgrade-from-tailwind-3)

Follow the steps to update an existing Astro project using Tailwind v3 (using the `@astrojs/tailwind` integration) to Tailwind 4 (using [the `@tailwindcss/vite` plugin](https://tailwindcss.com/docs/installation/framework-guides/astro)).

1. [Add Tailwind 4 support to your project](#add-tailwind-4) through the CLI for the latest version of Astro, or by adding the Vite plugin manually.

2. Uninstall the `@astrojs/tailwind` integration from your project:

   * npm

     ```shell
     npm uninstall @astrojs/tailwind
     ```

   * pnpm

     ```shell
     pnpm remove @astrojs/tailwind
     ```

   * Yarn

     ```shell
     yarn remove @astrojs/tailwind
     ```

3. Remove the `@astrojs/tailwind` integration from your `astro.config.mjs`:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   -import tailwind from '@astrojs/tailwind';


   export default defineConfig({
     // ...
     integrations: [tailwind()],
     // ...
   });
   ```

4. Then, upgrade your project according to [Tailwind’s v4 upgrade guide](https://tailwindcss.com/docs/upgrade-guide#changes-from-v3).

### Legacy Tailwind 3 support

[Section titled “Legacy Tailwind 3 support”](#legacy-tailwind-3-support)

To add (or keep) support for Tailwind 3, you will need to have both `tailwindcss@3` and the official Astro Tailwind integration `@astrojs/tailwind` installed. Installing these dependencies manually is only used for legacy Tailwind 3 compatibility, and is not required for Tailwind 4. You will also need a [legacy Tailwind configuration](https://v3.tailwindcss.com/docs/configuration#creating-your-configuration-file):

1. Install Tailwind and the Astro Tailwind integration to your project dependencies using your preferred package manager:

   * npm

     ```shell
     npm install tailwindcss@3 @astrojs/tailwind
     ```

   * pnpm

     ```shell
     pnpm add tailwindcss@3 @astrojs/tailwind
     ```

   * Yarn

     ```shell
     yarn add tailwindcss@3 @astrojs/tailwind
     ```

2. Import the integration to your `astro.config.mjs` file, and add it to your `integrations[]` array:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import tailwind from '@astrojs/tailwind';


   export default defineConfig({
     // ...
     integrations: [tailwind()],
     // ...
   });
   ```

3. Create a `tailwind.config.mjs` file in your project’s root directory. You can use the following command to generate a basic configuration file for you:

   * npm

     ```shell
     npx tailwindcss init
     ```

   * pnpm

     ```shell
     pnpm dlx tailwindcss init
     ```

   * Yarn

     ```shell
     yarn dlx tailwindcss init
     ```

4. Add the following basic configuration to your `tailwind.config.mjs` file:

   tailwind.config.mjs

   ```diff
   /** @type {import('tailwindcss').Config} */
   export default {
   +  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
     theme: {
       extend: {},
     },
     plugins: [],
   };
   ```

![](/houston_chef.webp) **Related recipe:** [Style rendered Markdown with Tailwind Typography](/en/recipes/tailwind-rendered-markdown/)

## CSS Preprocessors

[Section titled “CSS Preprocessors”](#css-preprocessors)

Astro supports CSS preprocessors such as [Sass](https://sass-lang.com/), [Stylus](https://stylus-lang.com/), and [Less](https://lesscss.org/) through [Vite](https://vite.dev/guide/features.html#css-pre-processors).

### Sass and SCSS

[Section titled “Sass and SCSS”](#sass-and-scss)

```shell
npm install sass
```

Use `<style lang="scss">` or `<style lang="sass">` in `.astro` files.

### Stylus

[Section titled “Stylus”](#stylus)

```shell
npm install stylus
```

Use `<style lang="styl">` or `<style lang="stylus">` in `.astro` files.

### Less

[Section titled “Less”](#less)

```shell
npm install less
```

Use `<style lang="less">` in `.astro` files.

### LightningCSS

[Section titled “LightningCSS”](#lightningcss)

```shell
npm install lightningcss
```

Update your `vite` configuration in `astro.config.mjs`:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config'


export default defineConfig({
+  vite: {
+    css: {
+      transformer: "lightningcss",
+    },
+  },
})
```

### In framework components

[Section titled “In framework components”](#in-framework-components)

You can also use all of the above CSS preprocessors within JS frameworks as well! Be sure to follow the patterns each framework recommends:

* **React** / **Preact**: `import Styles from './styles.module.scss';`
* **Vue**: `<style lang="scss">`
* **Svelte**: `<style lang="scss">`

## PostCSS

[Section titled “PostCSS”](#postcss)

Astro comes with PostCSS included as part of [Vite](https://vite.dev/guide/features.html#postcss). To configure PostCSS for your project, create a `postcss.config.cjs` file in the project root. You can import plugins using `require()` after installing them (for example `npm install autoprefixer`).

postcss.config.cjs

```diff
module.exports = {
  plugins: [
    +require('autoprefixer'),
    +require('cssnano'),
  ],
};
```

## Frameworks and Libraries

[Section titled “Frameworks and Libraries”](#frameworks-and-libraries)

### 📘 React / Preact

[Section titled “📘 React / Preact”](#-react--preact)

`.jsx` files support both global CSS and CSS Modules. To enable the latter, use the `.module.css` extension (or `.module.scss`/`.module.sass` if using Sass).

src/components/MyReactComponent.jsx

```jsx
import './global.css'; // include global CSS
import Styles from './styles.module.css'; // Use CSS Modules (must end in `.module.css`, `.module.scss`, or `.module.sass`!)
```

### 📗 Vue

[Section titled “📗 Vue”](#-vue)

Vue in Astro supports the same methods as `vue-loader` does:

* [vue-loader - Scoped CSS](https://vue-loader.vuejs.org/guide/scoped-css.html)
* [vue-loader - CSS Modules](https://vue-loader.vuejs.org/guide/css-modules.html)

### 📕 Svelte

[Section titled “📕 Svelte”](#-svelte)

Svelte in Astro also works exactly as expected: [Svelte Styling Docs](https://svelte.dev/docs#component-format-style).

## Markdown Styling

[Section titled “Markdown Styling”](#markdown-styling)

Any Astro styling methods are available to a [Markdown layout component](/en/basics/layouts/#markdown-layouts), but different methods will have different styling effects on your page.

You can apply global styles to your Markdown content by adding [imported stylesheets](#external-styles) to the layout that wraps your page content. It is also possible to style your Markdown with [`<style is:global>` tags](#global-styles) in the layout component. Note that any styles added are subject to [Astro’s cascading order](#cascading-order), and you should check your rendered page carefully to ensure your styles are being applied as intended.

You can also add CSS integrations including [Tailwind](/en/recipes/tailwind-rendered-markdown/). If you are using Tailwind, the [typography plugin](https://tailwindcss.com/docs/typography-plugin) can be useful for styling Markdown.

## Production

[Section titled “Production”](#production)

### Bundle control

[Section titled “Bundle control”](#bundle-control)

When Astro builds your site for production deployment, it minifies and combines your CSS into chunks. Each page on your site gets its own chunk, and additionally, CSS that is shared between multiple pages is further split off into their own chunks for reuse.

However, when you have several pages sharing styles, some shared chunks can become really small. If all of them were sent separately, it would lead to many stylesheets requests and affect site performance. Therefore, by default Astro will link only those in your HTML above 4kB in size as `<link rel="stylesheet">` tags, while inlining smaller ones into `<style type="text/css">`. This approach provides a balance between the number of additional requests and the volume of CSS that can be cached between pages.

You can configure the size at which stylesheets will be linked externally (in bytes) using the `assetsInlineLimit` vite build option. Note that this option affects script and image inlining as well.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  vite: {
    build: {
      assetsInlineLimit: 1024,
    }
  };
});
```

If you would rather all project styles remain external, you can configure the `inlineStylesheets` build option.

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  build: {
    inlineStylesheets: 'never'
  }
});
```

You can also set this option to `'always'` which will inline all stylesheets.

## Advanced

[Section titled “Advanced”](#advanced)

Caution

Be careful when bypassing Astro’s built-in CSS bundling! Styles won’t be automatically included in the built output, and it is your responsibility to make sure that the referenced file is properly included in the final page output.

### `?raw` CSS Imports

[Section titled “?raw CSS Imports”](#raw-css-imports)

For advanced use cases, CSS can be read directly from disk without being bundled or optimized by Astro. This can be useful when you need complete control over some snippet of CSS, and need to bypass Astro’s automatic CSS handling.

This is not recommended for most users.

src/components/RawInlineStyles.astro

```astro
---
// Advanced example! Not recommended for most users.
import rawStylesCSS from '../styles/main.css?raw';
---
<style is:inline set:html={rawStylesCSS}></style>
```

See [Vite’s docs](https://vite.dev/guide/assets.html#importing-asset-as-string) for full details.

### `?url` CSS Imports

[Section titled “?url CSS Imports”](#url-css-imports)

For advanced use cases, you can import a direct URL reference for a CSS file inside of your project `src/` directory. This can be useful when you need complete control over how a CSS file is loaded on the page. However, this will prevent the optimization of that CSS file with the rest of your page CSS .

This is not recommended for most users. Instead, place your CSS files inside of `public/` to get a consistent URL reference.

Caution

Importing a smaller CSS file with `?url` may return the base64 encoded contents of the CSS file as a data URL in your final build. Either write your code to support encoded data URLs (`data:text/css;base64,...`) or set the [`vite.build.assetsInlineLimit`](https://vite.dev/config/#build-assetsinlinelimit) config option to `0` to disable this feature.

src/components/RawStylesUrl.astro

```astro
---
// Advanced example! Not recommended for most users.
import stylesUrl from '../styles/main.css?url';
---
<link rel="preload" href={stylesUrl} as="style">
<link rel="stylesheet" href={stylesUrl}>
```

See [Vite’s docs](https://vite.dev/guide/assets.html#importing-asset-as-url) for full details.

# Syntax Highlighting

> Learn how to highlight your code blocks in Astro.

Astro comes with built-in support for [Shiki](https://shiki.style/) and [Prism](https://prismjs.com/). This provides syntax highlighting for:

* all [code fences (\`\`\`)](#markdown-code-blocks) used in a Markdown or MDX file.
* content within the [built-in `<Code />` component](#code-) (powered by Shiki) in `.astro` files.
* content within the [`<Prism />` component](#prism-) (powered by Prism) in `.astro` files.

Add [community integrations such as Expressive Code](https://astro.build/integrations/?search=syntax+highlight) for even more text marking and annotation options in your code blocks.

## Markdown code blocks

[Section titled “Markdown code blocks”](#markdown-code-blocks)

A Markdown code block is indicated by a block with three backticks \`\`\` at the start and end. You can indicate the programming language being used after the opening backticks to indicate how to color and style your code to make it easier to read.

````markdown
```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l);
  return true;
};
```
````

Astro’s Markdown code blocks are styled by Shiki by default, preconfigured with the `github-dark` theme. The compiled output will be limited to inline `style`s without any extraneous CSS classes, stylesheets, or client-side JS.

You can [add a Prism stylesheet and switch to Prism’s highlighting](#add-a-prism-stylesheet), or disable Astro’s syntax highlighting entirely, with the [`markdown.syntaxHighlight`](/en/reference/configuration-reference/#markdownsyntaxhighlight) configuration option.

See the full [`markdown.shikiConfig` reference](/en/reference/configuration-reference/#markdownshikiconfig) for the complete set of Markdown syntax highlighting options available when using Shiki.

### Setting a default Shiki theme

[Section titled “Setting a default Shiki theme”](#setting-a-default-shiki-theme)

You can configure any [built-in Shiki theme](https://shiki.style/themes) for your Markdown code blocks in your Astro config:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
    shikiConfig: {
      theme: 'dracula',
    },
  },
});
```

See the full [Shiki config reference](/en/reference/configuration-reference/#markdownshikiconfig) for the complete set of Markdown code block options.

### Setting light and dark mode themes

[Section titled “Setting light and dark mode themes”](#setting-light-and-dark-mode-themes)

You can specify dual Shiki themes for light and dark mode in your Astro config:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  markdown: {
    shikiConfig: {
      themes: {
        light: 'github-light',
        dark: 'github-dark',
      },
    },
  },
});
```

Then, [add Shiki’s dark mode CSS variables via media query or classes](https://shiki.style/guide/dual-themes#query-based-dark-mode) to apply to all your Markdown code blocks by default. Replace the `.shiki` class in the examples from Shiki’s documentation with `.astro-code`:

src/styles/global.css

```diff
@media (prefers-color-scheme: dark) {
  -.shiki,
  -.shiki span {
+  .astro-code,
+  .astro-code span {
    color: var(--shiki-dark) !important;
    background-color: var(--shiki-dark-bg) !important;
    /* Optional, if you also want font styles */
    font-style: var(--shiki-dark-font-style) !important;
    font-weight: var(--shiki-dark-font-weight) !important;
    text-decoration: var(--shiki-dark-text-decoration) !important;
  }
}
```

See the full [Shiki config reference](/en/reference/configuration-reference/#markdownshikiconfig) for the complete set of Markdown code block options.

### Adding your own Shiki theme

[Section titled “Adding your own Shiki theme”](#adding-your-own-shiki-theme)

Instead of using one of Shiki’s predefined themes, you can import a custom Shiki theme from a local file.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
+import customTheme from './my-shiki-theme.json';


export default defineConfig({
  markdown: {
    shikiConfig: {
+      theme: customTheme,
    },
  },
});
```

### Customizing Shiki themes

[Section titled “Customizing Shiki themes”](#customizing-shiki-themes)

You can follow [Shiki’s own theme documentation](https://shiki.style/themes) for more customization options for themes, [light vs dark mode toggles](https://shiki.style/guide/dual-themes), or styling via [CSS variables](https://shiki.style/guide/theme-colors#css-variables-theme).

You will need to adjust the examples from Shiki’s documentation for your Astro project by making the following substitutions:

* Code blocks are styled using the `.astro-code` class instead of `.shiki`
* When using the `css-variables` theme, custom properties are prefixed with `--astro-code-` instead of `--shiki-`

## Components for code blocks

[Section titled “Components for code blocks”](#components-for-code-blocks)

There are two Astro components available for `.astro` and `.mdx` files to render code blocks: [`<Code />`](#code-) and [`<Prism />`](#prism-).

You can reference the `Props` of these components using the [`ComponentProps` type](/en/guides/typescript/#componentprops-type) utility.

### `<Code />`

[Section titled “\<Code />”](#code-)

This component is powered internally by Shiki. It supports all popular Shiki themes and languages as well as several other Shiki options such as custom themes, languages, [transformers](#transformers), and default colors.

These values are passed to the `<Code />` component using the `theme`, `lang`, [`embeddedLangs`](#embeddedlangs), [`transformers`](#transformers), and `defaultColor` attributes respectively as props. The `<Code />` component will not inherit your `shikiConfig` settings for Markdown code blocks.

```astro
---
import { Code } from 'astro:components';
---
<!-- Syntax highlight some JavaScript code. -->
<Code code={`const foo = 'bar';`} lang="js" />
<!-- Optional: Customize your theme. -->
<Code code={`const foo = 'bar';`} lang="js" theme="dark-plus" />
<!-- Optional: Enable word wrapping. -->
<Code code={`const foo = 'bar';`} lang="js" wrap />
<!-- Optional: Output inline code. -->
<p>
  <Code code={`const foo = 'bar';`} lang="js" inline />
  will be rendered inline.
</p>
<!-- Optional: defaultColor -->
<Code code={`const foo = 'bar';`} lang="js" defaultColor={false} />
```

#### `embeddedLangs`

[Section titled “embeddedLangs”](#embeddedlangs)

**Type:** `string[] | undefined`

**Added in:** `astro@6.0.0`

Any additional languages to be included for syntax highlighting by Shiki.

A `lang` value may include support for highlighting some additional languages by default (e.g. `lang="svelte"` will also provide highlighting for `ts`).

Use `embeddedLangs` to include support for additional, non-standard language combinations (e.g. `jsx` support when `lang="vue"`).

src/pages/index.astro

```astro
---
import { Code } from 'astro:components'


const code = `<script setup lang="tsx">
const Text = ({ text }: { text: string }) => <div>{text}</div>;
</script>


<template>
  <Text text="Hello world" />
</template>
`
---
<Code
  lang="vue"
  embeddedLangs={["tsx"]}
  code={code}
/>
```

#### `transformers`

[Section titled “transformers”](#transformers)

**Type:** `ShikiTransformer[] | undefined`

**Added in:** `astro@4.11.0`

An array of [Shiki transformers](https://shiki.style/packages/transformers#shikijs-transformers) to be applied to your `code`. Since Astro v4.14.0, you can also provide a string for [Shiki’s `meta` attribute](https://shiki.style/guide/transformers#meta) to pass options to transformers.

Note that `transformers` only applies classes and you must provide your own CSS rules to target the elements of your code block.

src/pages/index.astro

```astro
---
import { transformerNotationFocus, transformerMetaHighlight } from '@shikijs/transformers'
import { Code } from 'astro:components'
const code = `const foo = 'hello'
const bar = ' world'
console.log(foo + bar) // [!code focus]
`
---
<Code
  code={code}
  lang="js"
  transformers={[transformerMetaHighlight()]}
  meta="{1,3}"
/>


<style is:global>
  pre.has-focused .line:not(.focused) {
    filter: blur(1px);
  }
</style>
```

### `<Prism />`

[Section titled “\<Prism />”](#prism-)

This component provides language-specific syntax highlighting for code blocks by applying Prism’s CSS classes. Note that you must [provide a Prism CSS stylesheet](#add-a-prism-stylesheet) (or bring your own) to style the classes.

To use the `Prism` highlighter component, you must install the `@astrojs/prism` package:

* npm

  ```shell
  npm install @astrojs/prism
  ```

* pnpm

  ```shell
  pnpm add @astrojs/prism
  ```

* Yarn

  ```shell
  yarn add @astrojs/prism
  ```

Then, you can import and use the `<Prism />` component like any other Astro component, passing a language and the code to render.

```astro
---
import { Prism } from '@astrojs/prism';
---
<Prism lang="js" code={`const foo = 'bar';`} />
```

In addition to the [list of languages supported by Prism](https://prismjs.com/#supported-languages), you can also use `lang="astro"` to display Astro code blocks.

## Add a Prism stylesheet

[Section titled “Add a Prism stylesheet”](#add-a-prism-stylesheet)

If you opt to use Prism (either by configuring `markdown.syntaxHighlight: 'prism'` or with the `<Prism />` component), Astro will apply Prism’s CSS classes instead of Shiki’s to your code. You will need to bring your own CSS stylesheet for syntax highlighting to appear.

1. Choose a premade stylesheet from the available [Prism Themes](https://github.com/PrismJS/prism-themes).

2. Add this stylesheet to [your project’s `public/` directory](/en/basics/project-structure/#public).

3. Load this into your page’s `<head>` in a [layout component](/en/basics/layouts/) via a `<link>` tag. (See [Prism basic usage](https://prismjs.com/#basic-usage).)

You can also visit the [list of languages supported by Prism](https://prismjs.com/#supported-languages) for options and usage.


---

# Astro recipes

> Short, focused how-to guides.

See guided examples of adding features to your Astro project.

## Official Recipes

[Section titled “Official Recipes”](#official-recipes)

Astro’s official recipes are short, focused how-to guides that walk a reader through completing a working example of a specific task. Recipes are a great way to add new features or behavior to your Astro project by following step-by-step instructions!

* ### [Installing a Vite or Rollup plugin](/en/recipes/add-yaml-support/)

  Learn how you can import YAML data by adding a Rollup plugin to your project.

* ### [Analyze bundle size](/en/recipes/analyze-bundle-size/)

  Learn how to analyze the bundle generated by Astro using \`rollup-plugin-visualizer\`.

* ### [Build a custom image component](/en/recipes/build-custom-img-component/)

  Learn how to build a custom image component that supports media queries using the getImage function.

* ### [Build HTML forms in Astro pages](/en/recipes/build-forms/)

  Learn how to build HTML forms and handle submissions in your frontmatter.

* ### [Build forms with API routes](/en/recipes/build-forms-api/)

  Learn how to use JavaScript to send form submissions to an API Route.

* ### [Use Bun with Astro](/en/recipes/bun/)

  Learn how to use Bun with your Astro site.

* ### [Call endpoints from the server](/en/recipes/call-endpoints/)

  Learn how to call endpoints from the server in Astro.

* ### [Verify a Captcha](/en/recipes/captcha/)

  Learn how to create an API route and fetch it from the client.

* ### [Customize file names in the build output](/en/recipes/customizing-output-filenames/)

  Learn how to change the default naming pattern for your built assets like JavaScript, CSS, and images in Astro using Vite's Rollup options.

* ### [Build your Astro site with Docker](/en/recipes/docker/)

  Learn how to build your Astro site using Docker.

* ### [Dynamically import images](/en/recipes/dynamically-importing-images/)

  Learn how to dynamically import images using Vite's import.meta.glob function.

* ### [Add icons to external links](/en/recipes/external-links/)

  Learn how to install a rehype plugin to add icons to external links in your Markdown files.

* ### [Add i18n features](/en/recipes/i18n/)

  Use dynamic routing and content collections to add internationalization support to your Astro site.

* ### [Create a dev toolbar app](/en/recipes/making-toolbar-apps/)

  Learn how to create a dev toolbar app for your site.

* ### [Add last modified time](/en/recipes/modified-time/)

  Build a remark plugin to add the last modified time to your Markdown and MDX.

* ### [Add reading time](/en/recipes/reading-time/)

  Build a remark plugin to add reading time to your Markdown or MDX files.

* ### [Add an RSS feed](/en/recipes/rss/)

  Add an RSS feed to your Astro site to let users subscribe to your content.

* ### [Share state between Astro components](/en/recipes/sharing-state/)

  Learn how to share state across Astro components with Nano Stores.

* ### [Share state between islands](/en/recipes/sharing-state-islands/)

  Learn how to share state across framework components with Nano Stores.

* ### [Using streaming to improve page performance](/en/recipes/streaming-improve-page-performance/)

  Learn how to use streaming to improve page performance.

* ### [Style rendered Markdown with Tailwind Typography](/en/recipes/tailwind-rendered-markdown/)

  Learn how to use @tailwind/typography to style your rendered Markdown.

## Community Resources

[Section titled “Community Resources”](#community-resources)

Find more recipes written and submitted by the community at [Astro Tips](https://astro-tips.dev).

# Installing a Vite or Rollup plugin

> Learn how you can import YAML data by adding a Rollup plugin to your project.

Astro builds on top of Vite, and supports both Vite and Rollup plugins. This recipe uses a Rollup plugin to add the ability to import a YAML (`.yml`) file in Astro.

## Recipe

[Section titled “Recipe”](#recipe)

1. Install `@rollup/plugin-yaml`:

   * npm

     ```shell
     npm install @rollup/plugin-yaml --save-dev
     ```

   * pnpm

     ```shell
     pnpm add @rollup/plugin-yaml --save-dev
     ```

   * Yarn

     ```shell
     yarn add @rollup/plugin-yaml --dev
     ```

2. Import the plugin in your `astro.config.mjs` and add it to the Vite plugins array:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import yaml from '@rollup/plugin-yaml';


   export default defineConfig({
   +  vite: {
   +    plugins: [yaml()]
   +  }
   });
   ```

3. Finally, you can import YAML data using an `import` statement:

   ```js
   import yml from './data.yml';
   ```

   Note

   While you can now import YAML data in your Astro project, your editor will not provide types for the imported data. To add types, create or find an existing `*.d.ts` file in the `src` directory of your project and add the following:

   src/files.d.ts

   ```ts
   // Specify the file extension you want to import
   declare module "*.yml" {
     const value: any; // Add type definitions here if desired
     export default value;
   }
   ```

   This will allow your editor to provide type hints for your YAML data.

# Analyze bundle size

> Learn how to analyze the bundle generated by Astro using `rollup-plugin-visualizer`.

Understanding what is a part of an Astro bundle is important for improving site performance. Visualizing the bundle can give clues as to where changes can be made in your project to reduce the bundle size.

## Recipe

[Section titled “Recipe”](#recipe)

The [`rollup-plugin-visualizer` library](https://github.com/btd/rollup-plugin-visualizer) allows you to visualize and analyze your Rollup bundle to see which modules are taking up space.

1. Install `rollup-plugin-visualizer`:

   * npm

     ```shell
     npm install rollup-plugin-visualizer --save-dev
     ```

   * pnpm

     ```shell
     pnpm add rollup-plugin-visualizer --save-dev
     ```

   * Yarn

     ```shell
     yarn add rollup-plugin-visualizer --save-dev
     ```

2. Add the plugin to the `astro.config.mjs` file:

   ```js
   // @ts-check
   import { defineConfig } from 'astro/config';
   import { visualizer } from "rollup-plugin-visualizer";


   export default defineConfig({
   vite: {
       plugins: [visualizer({
           emitFile: true,
           filename: "stats.html",
       })]
   }
   });
   ```

3. Run the build command:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm build
     ```

   * Yarn

     ```shell
     yarn build
     ```

4. Find the `stats.html` file(s) for your project.

   This will be at the root of your `dist/` directory for entirely static sites and will allow you to see what is included in the bundle.

   If your Astro project uses on-demand rendering, you will have two `stats.html` files. One will be for the client, and the other for the server, and each will be located at the root of the `dist/client` and `dist/server/` directories.

   See [the Rollup Plugin Visualizer documentation](https://github.com/btd/rollup-plugin-visualizer#how-to-use-generated-files) for guidance on how to interpret these files, or configure specific options.

Note

Given Astro’s unique approach to hydration, the build isn’t necessarily representative of the bundle that the client will receive.

The Rollup visualizer shows all dependencies that are used across the site, but it does not break down the bundle size on a per-page basis.

# Build a custom image component

> Learn how to build a custom image component that supports media queries using the getImage function.

Astro provides two built-in components that you can use to display and optimize your images. The `<Picture>` component allows you to display responsive images and work with different formats and sizes. The `<Image>` component will optimize your images and allow you to pass in different formats and quality properties.

When you need options that the `<Picture>` and `<Image>` components do not currently support, you can use the `getImage()` function to create a custom component.

In this recipe, you will use the [`getImage()` function](/en/guides/images/#generating-images-with-getimage) to create your own custom image component that displays different source images based on media queries.

## Recipe

[Section titled “Recipe”](#recipe)

1. Create a new Astro component and import the `getImage()` function

   src/components/MyCustomImageComponent.astro

   ```astro
   ---
    import { getImage } from "astro:assets";
   ---
   ```

2. Create a new component for your custom image. `MyCustomComponent.astro` will receive three `props` from `Astro.props`. The `mobileImgUrl` and `desktopImgUrl` props are used for creating your image at different viewport sizes. The `alt` prop is used for the image’s alt text. These props will be passed wherever you render your custom image components. Add the following imports and define the props that you will use in your component. You can also use TypeScript to type the props.

   src/components/MyCustomImageComponent.astro

   ```diff
   ---
   import type { ImageMetadata } from "astro";
   +import { getImage } from "astro:assets";


   interface Props {
       mobileImgUrl: string | ImageMetadata;
       desktopImgUrl: string | ImageMetadata;
       alt: string;
   }


   +const { mobileImgUrl, desktopImgUrl, alt } = Astro.props;
   ---
   ```

3. Define each of your responsive images by calling the `getImage()` function with your desired properties.

   src/components/MyCustomImageComponent.astro

   ```diff
   ---
   import type { ImageMetadata } from "astro";
   import { getImage } from "astro:assets";


   interface Props {
       mobileImgUrl: string | ImageMetadata;
       desktopImgUrl: string | ImageMetadata;
       alt: string;
   }


   const { mobileImgUrl, desktopImgUrl, alt } = Astro.props;


   +const mobileImg = await getImage({
       src: mobileImgUrl,
       format: "webp",
       width: 200,
       height: 200,
   +});


   +const desktopImg = await getImage({
       src: desktopImgUrl,
       format: "webp",
       width: 800,
       height: 200,
   +});
   ---
   ```

4. Create a `<picture>` element that generates a `srcset` with your different images based on your desired media queries.

   src/components/MyCustomImageComponent.astro

   ```diff
   ---
   import type { ImageMetadata } from "astro";
   import { getImage } from "astro:assets";


   interface Props {
       mobileImgUrl: string | ImageMetadata;
       desktopImgUrl: string | ImageMetadata;
       alt: string;
   }


   const { mobileImgUrl, desktopImgUrl, alt } = Astro.props;


   const mobileImg = await getImage({
       src: mobileImgUrl,
       format: "webp",
       width: 200,
       height: 200,
   });


   const desktopImg = await getImage({
       src: desktopImgUrl,
       format: "webp",
       width: 800,
       height: 200,
   });
   ---


   <picture>
       <source media="(max-width: 799px)" srcset={mobileImg.src} />
       <source media="(min-width: 800px)" srcset={desktopImg.src} />
       <img src={desktopImg.src} alt={alt} />
   </picture>
   ```

5. Import and use `<MyCustomImageComponent />` in any `.astro` file. Be sure to pass the necessary props for generating two different images at the different viewport sizes:

   src/pages/index.astro

   ```astro
   ---
   import MyCustomImageComponent from "../components/MyCustomImageComponent.astro";
   import mobileImage from "../images/mobile-profile-image.jpg";
   import desktopImage from "../images/desktop-profile-image.jpg";
   ---


   <MyCustomImageComponent
       mobileImgUrl={mobileImage}
       desktopImgUrl={desktopImage}
       alt="user profile picture"
   />
   ```

# Build HTML forms in Astro pages

> Learn how to build HTML forms and handle submissions in your frontmatter.

Astro pages that are rendered on demand can both display and handle forms. In this recipe, you’ll use a standard HTML form to submit data to the server. Your frontmatter script will handle the data on the server, sending no JavaScript to the client.

Build forms with Astro Actions

In v4.15, Astro added actions which provide several benefits over basic HTML forms including validating your form data and updating your UI based on the form submission. To use this method for building forms instead, see our [actions guide](/en/guides/actions/) to learn more about these features.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* An Astro project with a [server adapter](/en/guides/on-demand-rendering/#server-adapters) installed.

## Recipe

[Section titled “Recipe”](#recipe)

1. Create or identify a `.astro` page which will contain your form and your handling code. For example, you could add a registration page:

   src/pages/register.astro

   ```astro
   ---
   ---
   <h1>Register</h1>
   ```

2. Add a `<form>` tag with some inputs to the page. Each input should have a `name` attribute that describes the value of that input.

   Be sure to include a `<button>` or `<input type="submit">` element to submit the form.

   src/pages/register.astro

   ```astro
   ---
   ---
   <h1>Register</h1>
   <form>
     <label>
       Username:
       <input type="text" name="username" />
     </label>
     <label>
       Email:
       <input type="email" name="email" />
     </label>
     <label>
       Password:
       <input type="password" name="password" />
     </label>
     <button>Submit</button>
   </form>
   ```

3. Use [validation attributes](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#using_built-in_form_validation) to provide basic client-side validation that works even if JavaScript is disabled.

   In this example,

   * `required` prevents form submission until the field is filled.
   * `minlength` sets a minimum required length for the input text.
   * `type="email"` also introduces validation that will only accept a valid email format.

   src/pages/register.astro

   ```astro
   ---
   ---
   <h1>Register</h1>
   <form>
     <label>
       Username:
       <input type="text" name="username" required />
     </label>
     <label>
       Email:
       <input type="email" name="email" required />
     </label>
     <label>
       Password:
       <input type="password" name="password" required minlength="6" />
     </label>
     <button>Submit</button>
   </form>
   ```

   Tip

   You can add custom validation logic that refers to multiple fields using a `<script>` tag and the [Constraint Validation API](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation#complex_constraints_using_the_constraint_validation_api).

   To write complex validation logic more easily, you can build your form using a [frontend framework](/en/guides/framework-components/) and choose a form library like [React Hook Form](https://react-hook-form.com/) or [Felte](https://felte.dev/).

4. The form submission will cause the browser to request the page again. Change the form’s data transfer `method` to `POST` to send the form data as part of the `Request` body, rather than as URL parameters.

   src/pages/register.astro

   ```astro
   ---
   ---
   <h1>Register</h1>
   <form method="POST">
     <label>
       Username:
       <input type="text" name="username" required />
     </label>
     <label>
       Email:
       <input type="email" name="email" required />
     </label>
     <label>
       Password:
       <input type="password" name="password" required minlength="6" />
     </label>
     <button>Submit</button>
   </form>
   ```

5. Check for the `POST` method in the frontmatter and access the form data using `Astro.request.formData()`. Wrap this in a `try ... catch` block to handle cases when the `POST` request wasn’t sent by a form and the `formData` is invalid.

   src/pages/register.astro

   ```diff
   ---
   +export const prerender = false; // Not needed in 'server' mode


   +if (Astro.request.method === "POST") {
     +try {
       +const data = await Astro.request.formData();
       +const name = data.get("username");
       +const email = data.get("email");
       +const password = data.get("password");
       +// Do something with the data
   +  } catch (error) {
       +if (error instanceof Error) {
   +      console.error(error.message);
   +    }
   +  }
   +}
   ---
   <h1>Register</h1>
   <form method="POST">
     <label>
       Username:
       <input type="text" name="username" required />
     </label>
     <label>
       Email:
       <input type="email" name="email" required />
     </label>
     <label>
       Password:
       <input type="password" name="password" required minlength="6" />
     </label>
     <button>Submit</button>
   </form>
   ```

6. Validate the form data on the server. This should include the same validation done on the client to prevent malicious submissions to your endpoint and to support the rare legacy browser that doesn’t have form validation.

   It can also include validation that can’t be done on the client. For example, this example checks if the email is already in the database.

   Error messages can be sent back to the client by storing them in an `errors` object and accessing it in the template.

   src/pages/register.astro

   ```diff
   ---
   export const prerender = false; // Not needed in 'server' mode


   import { isRegistered, registerUser } from "../../data/users"
   import { isValidEmail } from "../../utils/isValidEmail";


   +const errors = { username: "", email: "", password: "" };
   if (Astro.request.method === "POST") {
     try {
       const data = await Astro.request.formData();
       const name = data.get("username");
       const email = data.get("email");
       const password = data.get("password");
       +if (typeof name !== "string" || name.length < 1) {
   +      errors.username += "Please enter a username. ";
   +    }
       +if (typeof email !== "string" || !isValidEmail(email)) {
   +      errors.email += "Email is not valid. ";
   +    } else if (await isRegistered(email)) {
   +      errors.email += "Email is already registered. ";
   +    }
       +if (typeof password !== "string" || password.length < 6) {
   +      errors.password += "Password must be at least 6 characters. ";
   +    }
       const hasErrors = Object.values(errors).some(msg => msg)
       if (!hasErrors) {
         await registerUser({name, email, password});
         return Astro.redirect("/login");
       }
     } catch (error) {
       if (error instanceof Error) {
         console.error(error.message);
       }
     }
   }
   ---
   <h1>Register</h1>
   <form method="POST">
     <label>
       Username:
       <input type="text" name="username" />
     </label>
     +{errors.username && <p>{errors.username}</p>}
     <label>
       Email:
       <input type="email" name="email" required />
     </label>
     +{errors.email && <p>{errors.email}</p>}
     <label>
       Password:
       <input type="password" name="password" required minlength="6" />
     </label>
     +{errors.password && <p>{errors.password}</p>}
     <button>Register</button>
   </form>
   ```

# Build forms with API routes

> Learn how to use JavaScript to send form submissions to an API Route.

An HTML form causes the browser to refresh the page or navigate to a new one. To send form data to an API endpoint instead, you must intercept the form submission using JavaScript.

This recipe shows you how to send form data to an API endpoint and handle that data.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A project with [an adapter for on-demand rendering](/en/guides/on-demand-rendering/)
* A [UI Framework integration](/en/guides/framework-components/) installed

## Recipe

[Section titled “Recipe”](#recipe)

1. Create a `POST` API endpoint at `/api/feedback` that will receive the form data. Use `request.formData()` to process it. Be sure to validate the form values before you use them.

   This example sends a JSON object with a message back to the client.

   src/pages/api/feedback.ts

   ```ts
   export const prerender = false; // Not needed in 'server' mode
   import type { APIRoute } from "astro";


   export const POST: APIRoute = async ({ request }) => {
     const data = await request.formData();
     const name = data.get("name");
     const email = data.get("email");
     const message = data.get("message");
     // Validate the data - you'll probably want to do more than this
     if (!name || !email || !message) {
       return new Response(
         JSON.stringify({
           message: "Missing required fields",
         }),
         { status: 400 }
       );
     }
     // Do something with the data, then return a success response
     return new Response(
       JSON.stringify({
         message: "Success!"
       }),
       { status: 200 }
     );
   };
   ```

2. Create a form component using your UI framework. Each input should have a `name` attribute that describes the value of that input.

   Be sure to include a `<button>` or `<input type="submit">` element to submit the form.

   * Preact

     src/components/FeedbackForm.tsx

     ```tsx
     export default function Form() {
       return (
         <form>
           <label>
             Name
             <input type="text" id="name" name="name" required />
           </label>
           <label>
             Email
             <input type="email" id="email" name="email" required />
           </label>
           <label>
             Message
             <textarea id="message" name="message" required />
           </label>
           <button>Send</button>
         </form>
       );
     }
     ```

   * React

     src/components/FeedbackForm.tsx

     ```tsx
     export default function Form() {
       return (
         <form>
           <label>
             Name
             <input type="text" id="name" name="name" required />
           </label>
           <label>
             Email
             <input type="email" id="email" name="email" required />
           </label>
           <label>
             Message
             <textarea id="message" name="message" required />
           </label>
           <button>Send</button>
         </form>
       );
     }
     ```

   * Solid

     src/components/FeedbackForm.tsx

     ```tsx
     export default function Form() {
       return (
         <form>
           <label>
             Name
             <input type="text" id="name" name="name" required />
           </label>
           <label>
             Email
             <input type="email" id="email" name="email" required />
           </label>
           <label>
             Message
             <textarea id="message" name="message" required />
           </label>
           <button>Send</button>
         </form>
       );
     }
     ```

   * Svelte

     src/components/FeedbackForm.svelte

     ```svelte
     <form>
       <label>
         Name
         <input type="text" id="name" name="name" required />
       </label>
       <label>
         Email
         <input type="email" id="email" name="email" required />
       </label>
       <label>
         Message
         <textarea id="message" name="message" required />
       </label>
       <button>Send</button>
     </form>
     ```

   * Vue

     src/components/FeedbackForm.vue

     ```vue
     <template>
       <form>
         <label>
           Name
           <input type="text" id="name" name="name" required />
         </label>
         <label>
           Email
           <input type="email" id="email" name="email" required />
         </label>
         <label>
           Message
           <textarea id="message" name="message" required />
         </label>
         <button>Send</button>
       </form>
     </template>
     ```

3. Create a function that accepts a submit event, then pass it as a `submit` handler to your form.

   In the function:

   * Call `preventDefault()` on the event to override the browser’s default submission process.
   * Create a `FormData` object and send it in a `POST` request to your endpoint using `fetch()`.

   - Preact

     src/components/FeedbackForm.tsx

     ```diff
     +import { useState } from "preact/hooks";


     export default function Form() {
       +const [responseMessage, setResponseMessage] = useState("");


       +async function submit(e: SubmitEvent) {
     +    e.preventDefault();
         +const formData = new FormData(e.target as HTMLFormElement);
         +const response = await fetch("/api/feedback", {
           method: "POST",
           body: formData,
         });
         +const data = await response.json();
         +if (data.message) {
           +setResponseMessage(data.message);
     +    }
     +  }


       return (
         <form onSubmit={submit}>
           <label>
             Name
             <input type="text" id="name" name="name" required />
           </label>
           <label>
             Email
             <input type="email" id="email" name="email" required />
           </label>
           <label>
             Message
             <textarea id="message" name="message" required />
           </label>
           <button>Send</button>
           +{responseMessage && <p>{responseMessage}</p>}
         </form>
       );
     }
     ```

   - React

     src/components/FeedbackForm.tsx

     ```diff
     +import { useState } from "react";
     +import type { FormEvent } from "react";


     export default function Form() {
       +const [responseMessage, setResponseMessage] = useState("");


       +async function submit(e: FormEvent<HTMLFormElement>) {
     +    e.preventDefault();
         +const formData = new FormData(e.target as HTMLFormElement);
         +const response = await fetch("/api/feedback", {
           method: "POST",
           body: formData,
         });
         +const data = await response.json();
         +if (data.message) {
           +setResponseMessage(data.message);
     +    }
     +  }


       return (
         <form onSubmit={submit}>
           <label htmlFor="name">
             Name
             <input type="text" id="name" name="name" autoComplete="name" required />
           </label>
           <label htmlFor="email">
             Email
             <input type="email" id="email" name="email" autoComplete="email" required />
           </label>
           <label htmlFor="message">
             Message
             <textarea id="message" name="message" autoComplete="off" required />
           </label>
           <button>Send</button>
           +{responseMessage && <p>{responseMessage}</p>}
         </form>
       );
     }
     ```

   - Solid

     src/components/FeedbackForm.tsx

     ```diff
     +import { createSignal, createResource, Suspense } from "solid-js";


     +async function postFormData(formData: FormData) {
       +const response = await fetch("/api/feedback", {
         method: "POST",
         body: formData,
       });
       +const data = await response.json();
       +return data;
     }


     export default function Form() {
       +const [formData, setFormData] = createSignal<FormData>();
       +const [response] = createResource(formData, postFormData);


       +function submit(e: SubmitEvent) {
     +    e.preventDefault();
         +setFormData(new FormData(e.target as HTMLFormElement));
     +  }


       return (
         <form onSubmit={submit}>
           <label>
             Name
             <input type="text" id="name" name="name" required />
           </label>
           <label>
             Email
             <input type="email" id="email" name="email" required />
           </label>
           <label>
             Message
             <textarea id="message" name="message" required />
           </label>
           <button>Send</button>
           +<Suspense>{response() && <p>{response().message}</p>}</Suspense>
         </form>
       );
     }
     ```

   - Svelte

     src/components/FeedbackForm.svelte

     ```diff
     <script lang="ts">
       +let responseMessage: string;


       +async function submit(e: SubmitEvent) {
     +    e.preventDefault();
         +const formData = new FormData(e.currentTarget as HTMLFormElement);
         +const response = await fetch("/api/feedback", {
           method: "POST",
           body: formData,
         });
         +const data = await response.json();
     +    responseMessage = data.message;
     +  }
     </script>


     <form on:submit={submit}>
       <label>
         Name
         <input type="text" id="name" name="name" required />
       </label>
       <label>
         Email
         <input type="email" id="email" name="email" required />
       </label>
       <label>
         Message
         <textarea id="message" name="message" required />
       </label>
       <button>Send</button>
     +  {#if responseMessage}
         <p>{responseMessage}</p>
     +  {/if}
     </form>
     ```

   - Vue

     src/components/FeedbackForm.vue

     ```diff
     <script setup lang="ts">
     +import { ref } from "vue";


     +const responseMessage = ref<string>();


     +async function submit(e: Event) {
     +  e.preventDefault();
       +const formData = new FormData(e.currentTarget as HTMLFormElement);
       +const response = await fetch("/api/feedback", {
         method: "POST",
         body: formData,
       });
       +const data = await response.json();
     +  responseMessage.value = data.message;
     +}
     </script>


     <template>
       <form @submit="submit">
         <label>
           Name
           <input type="text" id="name" name="name" required />
         </label>
         <label>
           Email
           <input type="email" id="email" name="email" required />
         </label>
         <label>
           Message
           <textarea id="message" name="message" required />
         </label>
         <button>Send</button>
         <p v-if="responseMessage">{{ responseMessage }}</p>
       </form>
     </template>
     ```

4. Import and include your `<FeedbackForm />` component on a page. Be sure to use a `client:*` directive to ensure that the form logic is hydrated when you want it to be.

   * Preact

     src/pages/index.astro

     ```astro
     ---
     import FeedbackForm from "../components/FeedbackForm"
     ---
     <FeedbackForm client:load />
     ```

   * React

     src/pages/index.astro

     ```astro
     ---
     import FeedbackForm from "../components/FeedbackForm"
     ---
     <FeedbackForm client:load />
     ```

   * Solid

     src/pages/index.astro

     ```astro
     ---
     import FeedbackForm from "../components/FeedbackForm"
     ---
     <FeedbackForm client:load />
     ```

   * Svelte

     src/pages/index.astro

     ```astro
     ---
     import FeedbackForm from "../components/FeedbackForm.svelte"
     ---
     <FeedbackForm client:load />
     ```

   * Vue

     src/pages/index.astro

     ```astro
     ---
     import FeedbackForm from "../components/FeedbackForm.vue"
     ---
     <FeedbackForm client:load />
     ```

# Use Bun with Astro

> Learn how to use Bun with your Astro site.

[Bun](https://bun.sh/) is an all-in-one JavaScript runtime & toolkit. See [Bun’s documentation](https://bun.sh/docs) for more information.

Caution

Using Bun with Astro may reveal rough edges. Some integrations may not work as expected. Consult [Bun’s official documentation for working with Astro](https://bun.sh/guides/ecosystem/astro) for details.

If you have any problems using Bun, please [open an Issue on GitHub with Bun directly](https://github.com/oven-sh/bun/issues/new/choose).

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* Bun installed locally on your machine. See the [installation instructions](https://bun.sh/docs/installation) in Bun’s official documentation.

## Create a new Astro project with Bun

[Section titled “Create a new Astro project with Bun”](#create-a-new-astro-project-with-bun)

Create a new Astro project with Bun using the following `create-astro` command:

```bash
bun create astro my-astro-project-using-bun
```

## Install dependencies

[Section titled “Install dependencies”](#install-dependencies)

If you skipped the “Install dependencies?” step during the CLI wizard, then be sure to install your dependencies before continuing.

```bash
bun install
```

## Add Types

[Section titled “Add Types”](#add-types)

Bun publishes the [`@types/bun`](https://www.npmjs.com/package/@types/bun) package, containing the runtime types for Bun.

Install `@types/bun` using the following command:

```sh
bun add -d @types/bun
```

## CLI installation flags

[Section titled “CLI installation flags”](#cli-installation-flags)

### Using integrations

[Section titled “Using integrations”](#using-integrations)

You can also use any of the official Astro integrations with the `astro add` command:

```bash
bun astro add react
```

### Use a theme or starter template

[Section titled “Use a theme or starter template”](#use-a-theme-or-starter-template)

You can start a new Astro project based on an [official example](https://github.com/withastro/astro/tree/main/examples) or the main branch of any GitHub repository by passing a `--template` argument to the `create astro` command.

Run the following command in your terminal, substituting the official Astro starter template name, or the GitHub username and repository of the theme you want to use:

```bash
# create a new project with an official example
bun create astro@latest --template <example-name>
# create a new project based on a GitHub repository’s main branch
bun create astro@latest --template <github-username>/<github-repo>
```

## Develop and build

[Section titled “Develop and build”](#develop-and-build)

To run the development server, use following command:

```bash
bun run dev
```

### Build and preview your site

[Section titled “Build and preview your site”](#build-and-preview-your-site)

To build your site, use the following command:

```bash
bun run build
```

When the build is finished, run the appropriate preview command (e.g. `bun run preview`) in your terminal and you can view the built version of your site locally in the same browser preview window.

## Testing

[Section titled “Testing”](#testing)

Bun ships with a fast, built-in, Jest-compatible test runner through the [`bun test` command](https://bun.sh/docs/cli/test). You can also use any other [testing tools for Astro](/en/guides/testing/).

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Build an app with Astro and Bun](https://bun.sh/guides/ecosystem/astro)

## Community Resources

[Section titled “Community Resources”](#community-resources)

Using Bun with Astro? Add your blog post or video to this page!

* [Using Bun with Astro and Cloudflare Pages](https://handerson.hashnode.dev/using-bun-with-astro-and-cloudflare-pages) - blog post

# Call endpoints from the server

> Learn how to call endpoints from the server in Astro.

Endpoints can be used to serve many kinds of data. This recipe calls a server endpoint from a page’s component script to display a greeting, without requiring an additional fetch request.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A project with [SSR](/en/guides/on-demand-rendering/) (output: ‘server’) enabled

## Recipe

[Section titled “Recipe”](#recipe)

1. Create an endpoint in a new file `src/pages/api/hello.ts` that returns some data:

   src/pages/api/hello.ts

   ```ts
   import type { APIRoute } from 'astro'


   export const GET: APIRoute = () => {
     return new Response(
       JSON.stringify({
         greeting: 'Hello',
       }),
     )
   }
   ```

2. On any Astro page, import the `GET()` method from the endpoint. Call it with the [`Astro` global](/en/reference/api-reference/) to provide the request context, and use the response on the page:

   src/pages/index.astro

   ```astro
   ---
   import { GET } from './api/hello.ts'


   let response = await GET(Astro)
   const data = await response.json()
   ---


   <h1>{data.greeting} world!</h1>
   ```

# Verify a Captcha

> Learn how to create an API route and fetch it from the client.

[Server endpoints](/en/guides/endpoints/#server-endpoints-api-routes) can be used as REST API endpoints to run functions such as authentications, database access, and verifications without exposing sensitive data to the client.

In this recipe, an API route is used to verify Google reCAPTCHA v3 without exposing the secret to clients.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A project with [SSR](/en/guides/on-demand-rendering/) (`output: 'server'`) enabled

## Recipe

[Section titled “Recipe”](#recipe)

1. Create a `POST` endpoint that accepts recaptcha data, then verifies it with reCAPTCHA’s API. Here, you can safely define secret values or read environment variables.

   src/pages/recaptcha.js

   ```js
   export async function POST({ request }) {
     const data = await request.json();


     const recaptchaURL = 'https://www.google.com/recaptcha/api/siteverify';
     const requestHeaders = {
       'Content-Type': 'application/x-www-form-urlencoded'
     };
     const requestBody = new URLSearchParams({
       secret: "YOUR_SITE_SECRET_KEY",   // This can be an environment variable
       response: data.recaptcha          // The token passed in from the client
     });


     const response = await fetch(recaptchaURL, {
       method: "POST",
       headers: requestHeaders,
       body: requestBody.toString()
     });


     const responseData = await response.json();


     return new Response(JSON.stringify(responseData), { status: 200 });
   }
   ```

2. Access your endpoint using `fetch` from a client script:

   src/pages/index.astro

   ```astro
   <html>
     <head>
       <script is:inline src="https://www.google.com/recaptcha/api.js"></script>
     </head>


     <body>
       <button class="g-recaptcha"
         data-sitekey="PUBLIC_SITE_KEY"
         data-callback="onSubmit"
         data-action="submit"> Click me to verify the captcha challenge! </button>


       <script is:inline>
         function onSubmit(token) {
           fetch("/recaptcha", {
             method: "POST",
             body: JSON.stringify({ recaptcha: token })
           })
           .then((response) => response.json())
           .then((gResponse) => {
             if (gResponse.success) {
               // Captcha verification was a success
             } else {
               // Captcha verification failed
             }
           })
         }
       </script>
     </body>
   </html>
   ```

# Customize file names in the build output

> Learn how to change the default naming pattern for your built assets like JavaScript, CSS, and images in Astro using Vite's Rollup options.

By default, the `astro build` command outputs your built assets from [your project source](/en/basics/project-structure/#src), like JavaScript and CSS files located in the `src/` directory, into an `_astro` directory with hashed filenames (e.g. `_astro/index.DRf8L97S.js`) which are excellent for long-term caching.

Although it is normally not necessary, you can customise the output file names when needed. For example, this can be helpful if you have scripts with names that might trigger ad blockers (e.g. `ads.js`), or if you want to organize your assets with a particular naming convention. By customizing Rollup output options, you can gain more control over your project’s build structure, allowing you to meet specific organizational or deployment requirements.

## Recipe

[Section titled “Recipe”](#recipe)

This recipe configures `vite.environments.client.build.rollupOptions` to output built assets with the following structure and naming pattern:

* JavaScript entry files (e.g. scripts directly associated with your pages or layouts): `dist/js/[name]-[hash].js`
* JavaScript code-split chunks (e.g. dynamically imported components or shared modules): `dist/js/chunks/[name]-[hash].js`
* Other assets (e.g. CSS, images, fonts): `dist/static/[name]-[hash][extname]` (e.g. `dist/static/styles-a1b2c3d4.css`, `dist/static/logo-e5f6g7h8.svg`)

1. Add Vite Rollup Output Options.

   Modify your `astro.config.mjs` to include the following `vite.environments.client.build.rollupOptions.output` configuration. This is where you can define the custom naming patterns for your assets using Rollup’s [`entryFileNames`](https://rollupjs.org/configuration-options/#output-entryfilenames), [`chunkFileNames`](https://rollupjs.org/configuration-options/#output-chunkfilenames), and [`assetFileNames`](https://rollupjs.org/configuration-options/#output-assetfilenames):

   astro.config.mjs

   ```javascript
   import { defineConfig } from 'astro/config';


   export default defineConfig({
     // ...
     vite: {
       environments: {
         client: {
           build: {
             rollupOptions: {
               output: {
                 // path names relative to `outDir`
                 entryFileNames: 'js/[name]-[hash].js',
                 chunkFileNames: 'js/chunks/[name]-[hash].js',
                 assetFileNames: 'static/[name]-[hash][extname]',
               },
             },
           },
         },
       },
     },
   });
   ```

   This example uses the following file name placeholders:

   * `[name]`: The original name of the file (without the extension and path).
   * `[hash]`: A content-based hash generated for the file, crucial for cache busting. You can also specify a length, e.g. `[hash:8]`. This ensures that when you update an asset, the filename changes, forcing browsers to download the new version instead of serving a stale cached version.
   * `[extname]`: The original file extension, including the leading dot (e.g. `.js`, `.css`, `.svg`).

   For a full list of available placeholders and advanced patterns for these options, refer to the [Rollup configuration documentation](https://rollupjs.org/configuration-options/).

2. Build your project.

   Since these filename customizations apply to the production build output only, you will need to run your project’s build command:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm build
     ```

   * Yarn

     ```shell
     yarn build
     ```

3. After the build completes, inspect your [output directory](/en/reference/configuration-reference/#outdir) (`dist/` by default).

   Verify that the build assets from your project `src` are named and organized according to the new patterns. (Files from [your `public/` directory](/en/basics/project-structure/#public) are copied directly to the output directory and are not affected by these Rollup naming options.)

   Depending on your project’s specific contents, your build folder will now look something like this:

   * dist/

     * js/

       * index-a1b2c3d4.js

       * chunks/

         * common-e5f6g7h8.js

     * img/

       * logo-i9j0k1l2.png

     * fonts/

       * myfont-q2w3e4r5.woff2

     * static\_assets/

       * styles-m3n4o5p6.css

     * index.html

     * about/

       * index.html

     * … (other HTML files and public assets)

# Build your Astro site with Docker

> Learn how to build your Astro site using Docker.

[Docker](https://docker.com) is a tool to build, deploy, and run applications using containers.

Docker images and containers can be deployed to many different platforms, like AWS, Azure, and [Google Cloud](/en/guides/deploy/google-cloud/#cloud-run-ssr-and-static). This recipe won’t cover how to deploy your site to a specific platform but will show you how to set up Docker for your project.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* Docker installed on your local machine. You can find [installation instructions for your operating system here](https://docs.docker.com/get-docker/).
* A Dockerfile in your project. You can [learn more about Dockerfiles here](https://docs.docker.com/engine/reference/builder/) and use the Dockerfiles in the following section as a starting point.

## Creating a Dockerfile

[Section titled “Creating a Dockerfile”](#creating-a-dockerfile)

Create a file called `Dockerfile` in your project’s root directory. This file contains the instructions to build your site, which will differ depending on your needs. This guide can’t show all possible options but will give you starting points for SSR and static mode.

If you’re using another package manager than npm, you’ll need to adjust the commands accordingly.

### SSR

[Section titled “SSR”](#ssr)

This Dockerfile will build your site and serve it using Node.js on port `4321` and therefore requires the [Node adapter](/en/guides/integrations-guide/node/) installed in your Astro project.

Dockerfile

```docker
FROM node:lts AS runtime
WORKDIR /app


COPY . .


RUN npm install
RUN npm run build


ENV HOST=0.0.0.0
ENV PORT=4321
EXPOSE 4321
CMD ["node", "./dist/server/entry.mjs"]
```

Keep this in mind

These are just examples of Dockerfiles. You can customize them to your needs. For example, you could use another image, like `node:lts-alpine`:

Dockerfile

```diff
-FROM node:lts as runtime
+FROM node:lts-alpine as runtime
```

### Adding a .dockerignore

[Section titled “Adding a .dockerignore”](#adding-a-dockerignore)

Adding a `.dockerignore` file to your project is best practice. This file describes which files or folders should be ignored in the Docker `COPY` or `ADD` commands, very similar to how `.gitignore` works. This speeds up the build process and reduces the size of the final image.

.dockerignore

```docker
.DS_Store
node_modules
dist
```

This file should go in the same directory as the `Dockerfile` itself. [Read the `.dockerignore` documentation for extra info](https://docs.docker.com/engine/reference/builder/#dockerignore-file)

### Static

[Section titled “Static”](#static)

#### Apache (httpd)

[Section titled “Apache (httpd)”](#apache-httpd)

The following Dockerfile will build your site and serve it using Apache httpd on port `80` with the default configuration.

Dockerfile

```docker
FROM node:lts AS build
WORKDIR /app
COPY . .
RUN npm i
RUN npm run build


FROM httpd:2.4 AS runtime
COPY --from=build /app/dist /usr/local/apache2/htdocs/
EXPOSE 80
```

Recommendation

Use this approach for simple websites that don’t need any special configuration. For more complex websites, it is recommended to use a custom configuration, either in Apache or NGINX.

#### NGINX

[Section titled “NGINX”](#nginx)

Dockerfile

```docker
FROM node:lts AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build


FROM nginx:alpine AS runtime
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 8080
```

In order to build the Dockerfile above, you’ll also need to create a configuration file for NGINX. Create a folder called `nginx` in your project’s root directory and create a file called `nginx.conf` inside.

nginx.conf

```nginx
worker_processes  1;


events {
  worker_connections  1024;
}


http {
  server {
    listen 8080;
    server_name   _;


    root   /usr/share/nginx/html;
    index  index.html index.htm;
    include /etc/nginx/mime.types;


    gzip on;
    gzip_min_length 1000;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;


    error_page 404 /404.html;
    location = /404.html {
            root /usr/share/nginx/html;
            internal;
    }


    location / {
            try_files $uri $uri/index.html =404;
    }
  }
}
```

### Multi-stage build (using SSR)

[Section titled “Multi-stage build (using SSR)”](#multi-stage-build-using-ssr)

Here’s an example of a more advanced Dockerfile that, thanks to Docker’s [multi-stage builds](https://docs.docker.com/build/building/multi-stage/), optimizes the build process for your site by not reinstalling the npm dependencies when only the source code changes. This can reduce the build time even by minutes, depending on the size of your dependencies.

Dockerfile

```docker
FROM node:lts AS base
WORKDIR /app


# By copying only the package.json and package-lock.json here, we ensure that the following `-deps` steps are independent of the source code.
# Therefore, the `-deps` steps will be skipped if only the source code changes.
COPY package.json package-lock.json ./


FROM base AS prod-deps
RUN npm install --omit=dev


FROM base AS build-deps
RUN npm install


FROM build-deps AS build
COPY . .
RUN npm run build


FROM base AS runtime
COPY --from=prod-deps /app/node_modules ./node_modules
COPY --from=build /app/dist ./dist


ENV HOST=0.0.0.0
ENV PORT=4321
EXPOSE 4321
CMD ["node", "./dist/server/entry.mjs"]
```

## Recipe

[Section titled “Recipe”](#recipe)

1. Build your container by running the following command in your project’s root directory. Use any name for `<your-astro-image-name>`:

   ```bash
   docker build -t <your-astro-image-name> .
   ```

   This will output an image, which you can run locally or deploy to a platform of your choice.

2. To run your image as a local container, use the following command.

   Replace `<local-port>` with an open port on your machine. Replace `<container-port>` with the port exposed by your Docker container (`4321`, `80`, or `8080` in the above examples.)

   ```bash
   docker run -p <local-port>:<container-port> <your-astro-image-name>
   ```

   You should be able to access your site at `http://localhost:<local-port>`.

3. Now that your website is successfully built and packaged in a container, you can deploy it to a cloud provider. See the [Google Cloud](/en/guides/deploy/google-cloud/#cloud-run-ssr-and-static) deployment guide for one example, and the [Deploy your app](https://docs.docker.com/language/nodejs/deploy/) page in the Docker docs.

# Dynamically import images

> Learn how to dynamically import images using Vite's import.meta.glob function.

Local [images](/en/guides/images/) must be imported into `.astro` files in order to display them. There will be times where you will want or need to dynamically import the image paths of your images instead of explicitly importing each individual image.

In this recipe, you will learn how to dynamically import your images using Vite’s `import.meta.glob` function. You will build a card component that displays the name, age, and photo of a person.

## Recipe

[Section titled “Recipe”](#recipe)

1. Create a new `assets` folder under the `src` directory and add your images inside that new folder.

   * src/

     * assets/

       * avatar-1.jpg
       * avatar-2.png
       * avatar-3.jpeg

   Note

   `assets` is a popular folder name convention for placing images but you are free to name the folder whatever you like.

2. Create a new Astro component for your card and import the `<Image />` component.

   src/components/MyCustomCardComponent.astro

   ```astro
   ---
   import { Image } from 'astro:assets';
   ---
   ```

3. Specify the `props` that your component will receive in order to display the necessary information on each card. You can optionally define their types, if you are using TypeScript in your project.

   src/components/MyCustomCardComponent.astro

   ```diff
   ---
   import { Image } from 'astro:assets';


   +interface Props {
   +   imagePath: string;
   +   altText: string;
   +   name: string;
   +   age: number;
   +}


   +const { imagePath, altText, name, age } = Astro.props;
   ---
   ```

4. Create a new `images` variable and use the `import.meta.glob` function which returns an object of all of the image paths inside the `assets` folder. You will also need to import `ImageMetadata` type to help define the type of the `images` variable.

   src/components/MyCustomCardComponent.astro

   ```diff
   ---
   +import type { ImageMetadata } from 'astro';
   import { Image } from 'astro:assets';


   interface Props {
      imagePath: string;
      altText: string;
      name: string;
      age: number;
   }


   const { imagePath, altText, name, age } = Astro.props;
   +const images = import.meta.glob<{ default: ImageMetadata }>('/src/assets/*.{jpeg,jpg,png,gif}')
   ---
   ```

5. Use the props to create the markup for your card component.

   src/components/MyCustomCardComponent.astro

   ```diff
   ---
   import type { ImageMetadata } from 'astro';
   import { Image } from 'astro:assets';


   interface Props {
      imagePath: string;
      altText: string;
      name: string;
      age: number;
   }


   const { imagePath, altText, name, age } = Astro.props;
   const images = import.meta.glob<{ default: ImageMetadata }>('/src/assets/*.{jpeg,jpg,png,gif}');
   ---
   <div class="card">
       <h2>{name}</h2>
       <p>Age: {age}</p>
       +<Image src={} alt={altText} />
   </div>
   ```

6. Inside the `src` attribute, pass in the `images` object and use bracket notation for the image path. Then make sure to invoke the glob function.

   Since you are accessing the `images` object which has an unknown type, you should also `throw` an error in case an invalid file path is passed as a prop.

   src/components/MyCustomCardComponent.astro

   ```diff
   ---
   import type { ImageMetadata } from 'astro';
   import { Image } from 'astro:assets';


   interface Props {
      imagePath: string;
      altText: string;
      name: string;
      age: number;
   }


   const { imagePath, altText, name, age } = Astro.props;
   const images = import.meta.glob<{ default: ImageMetadata }>('/src/assets/*.{jpeg,jpg,png,gif}');
   +if (!images[imagePath]) throw new Error(`"${imagePath}" does not exist in glob: "src/assets/*.{jpeg,jpg,png,gif}"`);
   ---
   <div class="card">
       <h2>{name}</h2>
       <p>Age: {age}</p>
       <Image src={images[imagePath]()} alt={altText} />
   </div>
   ```

   Note

   `images` is an object that contains all of the image paths inside the `assets` folder.

   ```js
   const images = {
     './assets/avatar-1.jpg': () => import('./assets/avatar-1.jpg'),
     './assets/avatar-2.png': () => import('./assets/avatar-2.png'),
     './assets/avatar-3.jpeg': () => import('./assets/avatar-3.jpeg')
   }
   ```

   The `imagePath` prop is a string that contains the path to the image that you want to display. The `import.meta.glob()` is doing the work of finding the image path that matches the `imagePath` prop and handling the import for you.

7. Import and use the card component inside an Astro page, passing in the values for the `props`.

   src/pages/index.astro

   ```astro
   ---
   import MyCustomCardComponent from '../components/MyCustomCardComponent.astro';
   ---
   <MyCustomCardComponent
       imagePath="/src/assets/avatar-1.jpg"
       altText="A headshot of Priya against a brick wall background."
       name="Priya"
       age={25}
   />
   ```

# Add icons to external links

> Learn how to install a rehype plugin to add icons to external links in your Markdown files.

Using a rehype plugin, you can identify and modify links in your Markdown files that point to external sites. This example adds icons to the end of each external link, so that visitors will know they are leaving your site.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* An Astro project using Markdown for content pages.

## Recipe

[Section titled “Recipe”](#recipe)

1. Install the `rehype-external-links` plugin.

   * npm

     ```shell
     npm install rehype-external-links
     ```

   * pnpm

     ```shell
     pnpm add rehype-external-links
     ```

   * Yarn

     ```shell
     yarn add rehype-external-links
     ```

2. Import the plugin into your `astro.config.mjs` file.

   Pass `rehypeExternalLinks` to the `rehypePlugins` array, along with an options object that includes a content property. Set this property’s `type` to `text` if you want to add plain text to the end of the link. To add HTML to the end of the link instead, set the property `type` to `raw`.

   ```ts
   // ...
   import rehypeExternalLinks from 'rehype-external-links';


   export default defineConfig({
     // ...
     markdown: {
       rehypePlugins: [
         [
           rehypeExternalLinks,
           {
             content: { type: 'text', value: ' 🔗' }
           }
         ],
       ]
     },
   });
   ```

   Note

   The value of the `content` property is [not represented in the accessibility tree](https://developer.mozilla.org/en-US/docs/Web/CSS/content#accessibility_concerns). As such, it’s best to make clear that the link is external in the surrounding content, rather than relying on the icon alone.

## Resources

[Section titled “Resources”](#resources)

* [rehype-external-links](https://www.npmjs.com/package/rehype-external-links)

# Add i18n features

> Use dynamic routing and content collections to add internationalization support to your Astro site.

In this recipe, you will learn how to use content collections and dynamic routing to build your own internationalization (i18n) solution and serve your content in different languages.

Tip

In v4.0, Astro added built-in support for i18n routing that allows you to configure default and supported languages and includes valuable helper functions to assist you in serving an international audience. If you want to use this instead, see our [internationalization guide](/en/guides/internationalization/) to learn about these features.

This example serves each language at its own subpath, e.g. `example.com/en/blog` for English and `example.com/fr/blog` for French.

If you prefer the default language to not be visible in the URL unlike other languages, there are [instructions to hide the default language](/en/recipes/i18n/#hide-default-language-in-the-url) below.

See the [resources section](#resources) for external links to related topics such as right-to-left (RTL) styling and choosing language tags.

## Recipe

[Section titled “Recipe”](#recipe)

### Set up pages for each language

[Section titled “Set up pages for each language”](#set-up-pages-for-each-language)

1. Create a directory for each language you want to support. For example, `en/` and `fr/` if you are supporting English and French:

   * src/

     * pages/

       * **en/**

         * about.astro
         * index.astro

       * **fr/**

         * about.astro
         * index.astro

       * index.astro

2. Set up `src/pages/index.astro` to redirect to your default language.

   * Static

     src/pages/index.astro

     ```astro
     <meta http-equiv="refresh" content="0;url=/en/" />
     ```

     This approach uses a [meta refresh](https://en.wikipedia.org/wiki/Meta_refresh) and will work however you deploy your site. Some static hosts also let you configure server redirects with a custom configuration file. See your deploy platform’s documentation for more details.

   * On demand

     If you are using an SSR adapter, you can use [`Astro.redirect`](/en/guides/routing/#dynamic-redirects) to redirect to the default language on the server.

     src/pages/index.astro

     ```astro
     ---
     return Astro.redirect('/en/');
     ---
     ```

### Use collections for translated content

[Section titled “Use collections for translated content”](#use-collections-for-translated-content)

1. Create a folder in `src/content/` for each type of content you want to include and add subdirectories for each supported language. For example, to support English and French blog posts:

   * src/

     * content/

       * blog/

         * **en/** Blog posts in English

           * post-1.md
           * post-2.md

         * **fr/** Blog posts in French

           * post-1.md
           * post-2.md

2. Create a `src/content.config.ts` file and export a collection for each type of content.

   src/content.config.ts

   ```ts
   import { defineCollection } from 'astro:content';
   import { z } from 'astro/zod';


   const blogCollection = defineCollection({
     schema: z.object({
       title: z.string(),
       author: z.string(),
       date: z.date()
     })
   });


   export const collections = {
     'blog': blogCollection
   };
   ```

   Read more about [Content Collections](/en/guides/content-collections/).

3. Use [dynamic routes](/en/guides/routing/#dynamic-routes) to fetch and render content based on a `lang` and a `slug` parameter.

   * Static

     In static rendering mode, use `getStaticPaths` to map each content entry to a page:

     src/pages/\[lang]/blog/\[...slug].astro

     ```astro
     ---
     import { getCollection, render } from 'astro:content';


     export async function getStaticPaths() {
       const pages = await getCollection('blog');


       const paths = pages.map(page => {
         const [lang, ...slug] = page.id.split('/');
         return { params: { lang, slug: slug.join('/') || undefined }, props: page };
       });


       return paths;
     }


     const { lang, slug } = Astro.params;
     const page = Astro.props;
     const formattedDate = page.data.date.toLocaleString(lang);
     const { Content } = await render(page);
     ---
     <h1>{page.data.title}</h1>
     <p>by {page.data.author} • {formattedDate}</p>
     <Content/>
     ```

   * On demand

     In [SSR mode](/en/guides/on-demand-rendering/), fetch the requested entry directly:

     src/pages/\[lang]/blog/\[...slug].astro

     ```astro
     ---
     import { getEntry, render } from 'astro:content';


     const { lang, slug } = Astro.params;
     const page = await getEntry('blog', `${lang}/${slug}`);


     if (!page) {
       return Astro.redirect('/404');
     }


     const formattedDate = page.data.date.toLocaleString(lang);
     const { Content, headings } = await render(page);
     ---
     <h1>{page.data.title}</h1>
     <p>by {page.data.author} • {formattedDate}</p>
     <Content/>
     ```

   Read more about [dynamic routing](/en/guides/routing/#dynamic-routes).

   Date formatting

   The example above uses the built-in [`toLocaleString()` date-formatting method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) to create a human-readable string from the frontmatter date. This ensures the date and time are formatted to match the user’s language.

### Translate UI strings

[Section titled “Translate UI strings”](#translate-ui-strings)

Create dictionaries of terms to translate the labels for UI elements around your site. This allows your visitors to experience your site fully in their language.

1. Create a `src/i18n/ui.ts` file to store your translation strings:

   src/i18n/ui.ts

   ```ts
   export const languages = {
     en: 'English',
     fr: 'Français',
   };


   export const defaultLang = 'en';


   export const ui = {
     en: {
       'nav.home': 'Home',
       'nav.about': 'About',
       'nav.twitter': 'Twitter',
     },
     fr: {
       'nav.home': 'Accueil',
       'nav.about': 'À propos',
     },
   } as const;
   ```

2. Create two helper functions: one to detect the page language based on the current URL, and one to get translations strings for different parts of the UI in `src/i18n/utils.ts`:

   src/i18n/utils.ts

   ```js
   import { ui, defaultLang } from './ui';


   export function getLangFromUrl(url: URL) {
     const [, lang] = url.pathname.split('/');
     if (lang in ui) return lang as keyof typeof ui;
     return defaultLang;
   }


   export function useTranslations(lang: keyof typeof ui) {
     return function t(key: keyof typeof ui[typeof defaultLang]) {
       return ui[lang][key] || ui[defaultLang][key];
     }
   }
   ```

   Did you notice?

   In step 1, the `nav.twitter` string was not translated to French. You may not want every term translated, such as proper names or common industry terms. The `useTranslations` helper will return the default language’s value if a key is not translated. In this example, French users will also see “Twitter” in the nav bar.

3. Import the helpers where needed and use them to choose the UI string that corresponds to the current language. For example, a nav component might look like:

   src/components/Nav.astro

   ```astro
   ---
   import { getLangFromUrl, useTranslations } from '../i18n/utils';


   const lang = getLangFromUrl(Astro.url);
   const t = useTranslations(lang);
   ---
   <ul>
       <li>
           <a href={`/${lang}/home/`}>
             {t('nav.home')}
           </a>
       </li>
       <li>
           <a href={`/${lang}/about/`}>
             {t('nav.about')}
           </a>
       </li>
       <li>
           <a href="https://twitter.com/astrodotbuild">
             {t('nav.twitter')}
           </a>
       </li>
   </ul>
   ```

4. Each page must have a `lang` attribute on the `<html>` element that matches the language on the page. In this example, a [reusable layout](/en/basics/layouts/) extracts the language from the current route:

   src/layouts/Base.astro

   ```astro
   ---
   import { getLangFromUrl } from '../i18n/utils';


   const lang = getLangFromUrl(Astro.url);
   ---
   <html lang={lang}>
       <head>
           <meta charset="utf-8" />
           <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
           <meta name="viewport" content="width=device-width" />
           <title>Astro</title>
       </head>
       <body>
           <slot />
       </body>
   </html>
   ```

   You can then use this base layout to ensure that pages use the correct `lang` attribute automatically.

   src/pages/en/about.astro

   ```astro
   ---
   import Base from '../../layouts/Base.astro';
   ---
   <Base>
       <h1>About me</h1>
       ...
   </Base>
   ```

### Let users switch between languages

[Section titled “Let users switch between languages”](#let-users-switch-between-languages)

Create links to the different languages you support so users can choose the language they want to read your site in.

1. Create a component to show a link for each language:

   src/components/LanguagePicker.astro

   ```astro
   ---
   import { languages } from '../i18n/ui';
   ---
   <ul>
     {Object.entries(languages).map(([lang, label]) => (
       <li>
         <a href={`/${lang}/`}>{label}</a>
       </li>
     ))}
   </ul>
   ```

2. Add `<LanguagePicker />` to your site so it is shown on every page. The example below adds it to the site footer in a base layout:

   src/layouts/Base.astro

   ```diff
   ---
   +import LanguagePicker from '../components/LanguagePicker.astro';
   import { getLangFromUrl } from '../i18n/utils';


   const lang = getLangFromUrl(Astro.url);
   ---
   <html lang={lang}>
       <head>
           <meta charset="utf-8" />
           <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
           <meta name="viewport" content="width=device-width" />
           <title>Astro</title>
       </head>
       <body>
           <slot />
           <footer>
             +<LanguagePicker />
           </footer>
       </body>
   </html>
   ```

### Hide default language in the URL

[Section titled “Hide default language in the URL”](#hide-default-language-in-the-url)

1. Create a directory for each language except the default language. For example, store your default language pages directly in `pages/`, and your translated pages in `fr/`:

   * src/

     * pages/

       * about.astro

       * index.astro

       * **fr/**

         * about.astro
         * index.astro

2. Add another line to the `src/i18n/ui.ts` file to toggle the feature:

   src/i18n/ui.ts

   ```ts
   export const showDefaultLang = false;
   ```

3. Add a helper function to `src/i18n/utils.ts`, to translate paths based on the current language:

   src/i18n/utils.ts

   ```js
   import { ui, defaultLang, showDefaultLang } from './ui';


   export function useTranslatedPath(lang: keyof typeof ui) {
     return function translatePath(path: string, l: string = lang) {
       return !showDefaultLang && l === defaultLang ? path : `/${l}${path}`
     }
   }
   ```

4. Import the helper where needed. For example, a `nav` component might look like:

   src/components/Nav.astro

   ```astro
   ---
   import { getLangFromUrl, useTranslations, useTranslatedPath } from '../i18n/utils';


   const lang = getLangFromUrl(Astro.url);
   const t = useTranslations(lang);
   const translatePath = useTranslatedPath(lang);
   ---
   <ul>
       <li>
           <a href={translatePath('/home/')}>
             {t('nav.home')}
           </a>
       </li>
       <li>
           <a href={translatePath('/about/')}>
             {t('nav.about')}
           </a>
       </li>
       <li>
           <a href="https://twitter.com/astrodotbuild">
             {t('nav.twitter')}
           </a>
       </li>
   </ul>
   ```

5. The helper function can also be used to translate paths for a specific language. For example, when users switch between languages:

   src/components/LanguagePicker.astro

   ```astro
   ---
   import { languages } from '../i18n/ui';
   import { getLangFromUrl, useTranslatedPath } from '../i18n/utils';


   const lang = getLangFromUrl(Astro.url);
   const translatePath = useTranslatedPath(lang);
   ---
   <ul>
     {Object.entries(languages).map(([lang, label]) => (
       <li>
         <a href={translatePath('/', lang)}>{label}</a>
       </li>
     ))}
   </ul>
   ```

### Translate Routes

[Section titled “Translate Routes”](#translate-routes)

Translate the routes of your pages for each language.

1. Add route mappings to `src/i18n/ui.ts`:

   src/i18n/ui.ts

   ```ts
   export const routes = {
     de: {
       'services': 'leistungen',
     },
     fr: {
       'services': 'prestations-de-service',
     },
   }
   ```

2. Update the `useTranslatedPath` helper function in `src/i18n/utils.ts` to add router translation logic.

   src/i18n/utils.ts

   ```js
   import { ui, defaultLang, showDefaultLang, routes } from './ui';


   export function useTranslatedPath(lang: keyof typeof ui) {
     return function translatePath(path: string, l: string = lang) {
       const pathName = path.replaceAll('/', '')
       const hasTranslation = defaultLang !== l && routes[l] !== undefined && routes[l][pathName] !== undefined
       const translatedPath = hasTranslation ? '/' + routes[l][pathName] : path


       return !showDefaultLang && l === defaultLang ? translatedPath : `/${l}${translatedPath}`
     }
   }
   ```

3. Create a helper function to get the route, if it exists based on the current URL, in `src/i18n/utils.ts`:

   src/i18n/utils.ts

   ```js
   import { ui, defaultLang, showDefaultLang, routes } from './ui';


   export function getRouteFromUrl(url: URL): string | undefined {
     const pathname = new URL(url).pathname;
     const parts = pathname?.split('/');
     const path = parts.pop() || parts.pop();


     if (path === undefined) {
       return undefined;
     }


     const currentLang = getLangFromUrl(url);


     if (defaultLang === currentLang) {
       const route = Object.values(routes)[0];
       return route[path] !== undefined ? route[path] : undefined;
     }


     const getKeyByValue = (obj: Record<string, string>, value: string): string | undefined  => {
         return Object.keys(obj).find((key) => obj[key] === value);
     }


     const reversedKey = getKeyByValue(routes[currentLang], path);


     if (reversedKey !== undefined) {
       return reversedKey;
     }


     return undefined;
   }
   ```

4. The helper function can be used to get a translated route. For example, when no translated route is defined, the user will be redirected to the home page:

   src/components/LanguagePicker.astro

   ```astro
   ---
   import { languages } from '../i18n/ui';
   import { getRouteFromUrl, useTranslatedPath } from '../i18n/utils';


   const route = getRouteFromUrl(Astro.url);
   ---
   <ul>
     {Object.entries(languages).map(([lang, label]) => {
       const translatePath = useTranslatedPath(lang);
       return (
         <li>
           <a href={translatePath(`/${route ? route : ''}`)}>{label}</a>
         </li>
       )
     })}
   </ul>
   ```

## Resources

[Section titled “Resources”](#resources)

* [Choosing a Language Tag](https://www.w3.org/International/questions/qa-choosing-language-tags)
* [Right-to-left (RTL) Styling 101](https://rtlstyling.com/)

## Community libraries

[Section titled “Community libraries”](#community-libraries)

Find [community-built i18n utilities](https://astro.build/integrations/?search=i18n) that you can add to your Astro project in our integrations directory.

# Create a dev toolbar app

> Learn how to create a dev toolbar app for your site.

Astro includes a [development toolbar](/en/guides/dev-toolbar/) that you can use to inspect your site, check for accessibility and performance issues, and more. This toolbar can be extended with custom apps.

## Build a motivational dev toolbar app

[Section titled “Build a motivational dev toolbar app”](#build-a-motivational-dev-toolbar-app)

In this recipe, you’ll learn how to create a dev toolbar app that helps you stay motivated while working on your site. This app will display a motivational message every time you toggle it on.

Tip

Just want to get started quickly? Jump start your app by creating a new Astro project with the `toolbar-app` template.

* npm

  ```shell
  npm create astro@latest -- --template toolbar-app
  ```

* pnpm

  ```shell
  pnpm create astro -- --template toolbar-app
  ```

* Yarn

  ```shell
  yarn create astro -- --template toolbar-app
  ```

Or, keep reading to learn how to build an app from scratch.

### Creating the Astro integration

[Section titled “Creating the Astro integration”](#creating-the-astro-integration)

Dev toolbar apps can only be added by [Astro Integrations](/en/guides/integrations/) using [the `astro:config:setup` hook](/en/reference/integrations-reference/#astroconfigsetup). You will need to create both a toolbar app and the integration that will add it to the toolbar of your existing Astro project.

1. In the root of your existing Astro project, create a new folder named `my-toolbar-app/` for your app and integration files. Create two new files in this folder: `app.ts` and `my-integration.ts`.

   * **my-toolbar-app/**

     * **app.ts**
     * **my-integration.ts**

   * src/

     * pages/

       * …

     * …

   * astro.config.mjs

   * package.json

   * tsconfig.json

2. In `my-integration.ts`, add the following code to provide both the name of your integration and the [`addDevToolbarApp()` function](/en/reference/dev-toolbar-app-reference/#toolbar-app-integration-setup) needed to add your dev toolbar app with the `astro:config:setup` hook:

   my-toolbar-app/my-integration.ts

   ```ts
   import { fileURLToPath } from 'node:url';
   import type { AstroIntegration } from 'astro';


   export default {
     name: 'my-astro-integration',
     hooks: {
       'astro:config:setup': ({ addDevToolbarApp }) => {
         addDevToolbarApp({
           id: "my-toolbar-app",
           name: "My Toolbar App",
           icon: "🚀",
           entrypoint: fileURLToPath(new URL('./app.ts', import.meta.url))
         });
       },
     },
   } satisfies AstroIntegration;
   ```

   Using relative paths to the entrypoint

   The `entrypoint` is the path to your dev toolbar app file **relative to the root of your existing Astro project**, not to the integration folder (`my-toolbar-app`) itself.

   To use relative paths for entrypoints, get the path to the current file using `import.meta.url` and resolve the path to the entrypoint from there.

3. To use this integration in your project, add it to the `integrations` array in your `astro.config.mjs` file.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import myIntegration from './my-toolbar-app/my-integration.ts';


   export default defineConfig({
   +  integrations: [myIntegration],
   })
   ```

4. If not already running, start the dev server. If your integration has been successfully added to your project, you should see a new “undefined” app in the dev toolbar.

   But, you will also see an error message that your dev toolbar app has failed to load. This is because you have not yet built the app itself. You will do that in the next section.

See the [Astro Integration API documentation](/en/reference/integrations-reference/) for more about building Astro integrations.

### Creating the app

[Section titled “Creating the app”](#creating-the-app)

Dev toolbar apps are defined using the `defineToolbarApp()` function from the `astro/toolbar` module. This function takes an object with an `init()` function that will be called when the dev toolbar app is loaded.

This `init()` function contains your app logic to render elements to the screen, send and receive client-side events from the dev toolbar, and communicate with the server.

app.ts

```ts
import { defineToolbarApp } from "astro/toolbar";


export default defineToolbarApp({
    init(canvas, app, server) {
      // ...
    },
});
```

To display motivational messages on the screen, you will use the `canvas` property to access a standard [ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot). Elements can be created and added to the ShadowRoot using the standard DOM APIs.

1. Copy the following code into `my-toolbar-app/app.ts`. This provides a list of motivational messages, and the logic to create a new `<h1>` element with a random message:

   my-toolbar-app/app.ts

   ```ts
   import { defineToolbarApp } from "astro/toolbar";


   const motivationalMessages = [
     "You're doing great!",
     "Keep up the good work!",
     "You're awesome!",
     "You're a star!",
   ];


   export default defineToolbarApp({
       init(canvas) {
         const h1 = document.createElement('h1');
         h1.textContent = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];


         canvas.append(h1);
       },
   });
   ```

2. Start the dev server if it is not already running and toggle the app on in the dev toolbar. If your app is working successfully, you will see a motivational message displayed in the top-left corner of the screen. (And, it’s true!)

   However, this message will not change when the app is toggled on and off, as the `init()` function is only called once when the app is loaded.

3. To add client-side interactivity to your app, add the `app` argument and use `onAppToggled()` to select a new random message each time your toolbar app is toggled on:

   app.ts

   ```diff
   import { defineToolbarApp } from "astro/toolbar";


   const motivationalMessages = [
     "You're doing great!",
     "Keep up the good work!",
     "You're awesome!",
     "You're a star!",
   ];


   export default defineToolbarApp({
       init(canvas, app) {
         const h1 = document.createElement('h1');
         h1.textContent = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];


         canvas.append(h1);


         +// Display a random message when the app is toggled
   +      app.onToggled(({ state }) => {
           +const newMessage = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];
   +        h1.textContent = newMessage;
   +      });
       },
   });
   ```

4. In your browser preview, toggle your app on and off several times. With this change, a new random message will be selected every time you toggle the app on, providing you with an infinite source of motivation!

See the [Astro Dev Toolbar API documentation](/en/reference/dev-toolbar-app-reference/) for more about building dev toolbar apps.

## Building apps with a UI framework

[Section titled “Building apps with a UI framework”](#building-apps-with-a-ui-framework)

UI frameworks like React, Vue, or Svelte can also be used to create dev toolbar apps. These frameworks provide a more declarative way to create UIs and can make your code more maintainable and easier to read.

The same motivational dev toolbar app built into your existing Astro project earlier on this page with JavaScript can be built using a UI framework (e.g. Preact) instead. Depending on your chosen framework, you may or may not require a build step.

Note

However you choose to build your dev toolbar app, using JavaScript or a UI framework, you will still need to [create the integration](#creating-the-astro-integration) that adds your app to the dev toolbar.

### Without a build step

[Section titled “Without a build step”](#without-a-build-step)

If your framework supports it, you can create a dev toolbar app without a build step. For example, you can use Preact’s `h` function to create elements and render them directly to the ShadowRoot:

app.ts

```ts
import { defineToolbarApp } from "astro/toolbar";
import { render, h } from "preact";


const motivationalMessages = [
  "You're doing great!",
  "Keep up the good work!",
  "You're awesome!",
  "You're a star!",
];


export default defineToolbarApp({
    init(canvas) {
      const message = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];
      render(h('h1', null, message), canvas);
    },
});
```

Alternatively, the [`htm` package](https://github.com/developit/htm) is a good choice for creating dev toolbar apps without a build step, offering native integration for React and Preact and support for other frameworks:

app.ts

```diff
import { defineToolbarApp } from "astro/toolbar";
import { render } from "preact";
+import { html } from 'htm/preact';


const motivationalMessages = [
  "You're doing great!",
  "Keep up the good work!",
  "You're awesome!",
  "You're a star!",
];


export default defineToolbarApp({
    init(canvas) {
      const message = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];
      +render(html`<h1>${message}</h1>`, canvas);
    },
});
```

In both cases, you can now start your project and see the motivational message displayed in the top-left corner of the screen when you toggle the app on.

### With a build step

[Section titled “With a build step”](#with-a-build-step)

Astro does not preprocess JSX code in dev toolbar apps, so a build step is required in order to use JSX components in your dev toolbar app.

The following steps will use TypeScript to do this, but any other tools that compile JSX code will also work (e.g. Babel, Rollup, ESBuild).

1. Install TypeScript inside your project:

   * npm

     ```shell
     npm install --save-dev typescript
     ```

   * pnpm

     ```shell
     pnpm install --save-dev typescript
     ```

   * Yarn

     ```shell
     yarn add --dev typescript
     ```

2. Create a `tsconfig.json` file in the root of your toolbar app’s folder with the appropriate settings to build and for the framework you’re using ([React](https://react-typescript-cheatsheet.netlify.app/docs/basic/setup), [Preact](https://preactjs.com/guide/v10/typescript), [Solid](https://www.solidjs.com/guides/typescript)). For example, for Preact:

   my-toolbar-app/tsconfig.json

   ```json
   {
     "compilerOptions": {
       "skipLibCheck": true,
       "module": "NodeNext",
       "jsx": "react-jsx",
       "jsxImportSource": "preact",
     }
   }
   ```

3. Adjust the `entrypoint` in your integration to point to the compiled file, remembering that this file is relative to the root of your Astro project:

   my-integration.ts

   ```ts
   addDevToolbarApp({
     id: "my-toolbar-app",
     name: "My Toolbar App",
     icon: "🚀",
     entrypoint: join(__dirname, "./app.js"),
   });
   ```

4. Run `tsc` to build your toolbar app, or `tsc --watch` to automatically rebuild your app when you make changes.

   With these changes, you can now rename your `app.ts` file to `app.tsx` (or `.jsx`) and use JSX syntax to create your dev toolbar app:

   app.tsx

   ```tsx
   import { defineToolbarApp } from "astro/toolbar";
   import { render } from "preact";


   const motivationalMessages = [
     "You're doing great!",
     "Keep up the good work!",
     "You're awesome!",
     "You're a star!",
   ];


   export default defineToolbarApp({
       init(canvas) {
         const message = motivationalMessages[Math.floor(Math.random() * motivationalMessages.length)];
         render(<h1>{message}</h1>, canvas);
       },
   });
   ```

You should now have all the tools you need to create a dev toolbar app using a UI framework of your choice!

# Add last modified time

> Build a remark plugin to add the last modified time to your Markdown and MDX.

Learn how to build a [remark plugin](https://github.com/remarkjs/remark) that adds the last modified time to the frontmatter of your Markdown and MDX files. Use this property to display the modified time in your pages.

Uses Git history

This recipe calculates time based on your repository’s Git history and may not be accurate on some deployment platforms. Your host may be performing **shallow clones** which do not retrieve the full git history.

## Recipe

[Section titled “Recipe”](#recipe)

1. Install Helper Packages

   Install [`Day.js`](https://www.npmjs.com/package/dayjs) to modify and format times:

   * npm

     ```shell
     npm install dayjs
     ```

   * pnpm

     ```shell
     pnpm add dayjs
     ```

   * Yarn

     ```shell
     yarn add dayjs
     ```

2. Create a Remark Plugin

   This plugin uses `execSync` to run a Git command that returns the timestamp of the latest commit in ISO 8601 format. The timestamp is then added to the frontmatter of the file.

   remark-modified-time.mjs

   ```js
   import { execSync } from "child_process";


   export function remarkModifiedTime() {
     return function (tree, file) {
       const filepath = file.history[0];
       const result = execSync(`git log -1 --pretty="format:%cI" "${filepath}"`);
       file.data.astro.frontmatter.lastModified = result.toString();
     };
   }
   ```

   Using the file system instead of Git

   Although using Git is the recommended way to get the last modified timestamp from a file, it is possible to use the file system modified time. This plugin uses `statSync` to get the `mtime` (modified time) of the file in ISO 8601 format. The timestamp is then added to the frontmatter of the file.

   remark-modified-time.mjs

   ```js
   import { statSync } from "fs";


   export function remarkModifiedTime() {
     return function (tree, file) {
       const filepath = file.history[0];
       const result = statSync(filepath);
       file.data.astro.frontmatter.lastModified = result.mtime.toISOString();
     };
   }
   ```

3. Add the plugin to your config

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import { remarkModifiedTime } from './remark-modified-time.mjs';


   export default defineConfig({
     markdown: {
       remarkPlugins: [remarkModifiedTime],
     },
   });
   ```

   Now all Markdown documents will have a `lastModified` property in their frontmatter.

4. Display Last Modified Time

   If your content is stored in a [content collection](/en/guides/content-collections/), access the `remarkPluginFrontmatter` from the `render(entry)` function. Then render `lastModified` in your template wherever you would like it to appear.

   src/pages/posts/\[slug].astro

   ```astro
   ---
   import { getCollection, render } from 'astro:content';
   import dayjs from "dayjs";
   import utc from "dayjs/plugin/utc";


   dayjs.extend(utc);


   export async function getStaticPaths() {
     const blog = await getCollection('blog');
     return blog.map(entry => ({
       params: { slug: entry.id },
       props: { entry },
     }));
   }


   const { entry } = Astro.props;
   const { Content, remarkPluginFrontmatter } = await render(entry);


   const lastModified = dayjs(remarkPluginFrontmatter.lastModified)
     .utc()
     .format("HH:mm:ss DD MMMM YYYY UTC");
   ---


   <html>
     <head>...</head>
     <body>
       ...
       <p>Last Modified: {lastModified}</p>
       ...
     </body>
   </html>
   ```

   If you’re using a [Markdown layout](/en/basics/layouts/#markdown-layouts), use the `lastModified` frontmatter property from `Astro.props` in your layout template.

   src/layouts/BlogLayout.astro

   ```astro
   ---
   import dayjs from "dayjs";
   import utc from "dayjs/plugin/utc";


   dayjs.extend(utc);


   const lastModified = dayjs()
     .utc(Astro.props.frontmatter.lastModified)
     .format("HH:mm:ss DD MMMM YYYY UTC");
   ---


   <html>
     <head>...</head>
     <body>
       <p>{lastModified}</p>
       <slot />
     </body>
   </html>
   ```

# Add reading time

> Build a remark plugin to add reading time to your Markdown or MDX files.

Create a [remark plugin](https://github.com/remarkjs/remark) which adds a reading time property to the frontmatter of your Markdown or MDX files. Use this property to display the reading time for each page.

## Recipe

[Section titled “Recipe”](#recipe)

1. Install Helper Packages

   Install these two helper packages:

   * [`reading-time`](https://www.npmjs.com/package/reading-time) to calculate minutes read
   * [`mdast-util-to-string`](https://www.npmjs.com/package/mdast-util-to-string) to extract all text from your markdown

   - npm

     ```shell
     npm install reading-time mdast-util-to-string
     ```

   - pnpm

     ```shell
     pnpm add reading-time mdast-util-to-string
     ```

   - Yarn

     ```shell
     yarn add reading-time mdast-util-to-string
     ```

2. Create a remark plugin.

   This plugin uses the `mdast-util-to-string` package to get the Markdown file’s text. This text is then passed to the `reading-time` package to calculate the reading time in minutes.

   remark-reading-time.mjs

   ```js
   import getReadingTime from 'reading-time';
   import { toString } from 'mdast-util-to-string';


   export function remarkReadingTime() {
     return function (tree, { data }) {
       const textOnPage = toString(tree);
       const readingTime = getReadingTime(textOnPage);
       // readingTime.text will give us minutes read as a friendly string,
       // i.e. "3 min read"
       data.astro.frontmatter.minutesRead = readingTime.text;
     };
   }
   ```

3. Add the plugin to your config:

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import { remarkReadingTime } from './remark-reading-time.mjs';


   export default defineConfig({
     markdown: {
       remarkPlugins: [remarkReadingTime],
     },
   });
   ```

   Now all Markdown documents will have a calculated `minutesRead` property in their frontmatter.

4. Display Reading Time

   If your blog posts are stored in a [content collection](/en/guides/content-collections/), access the `remarkPluginFrontmatter` from the `render(entry)` function. Then, render `minutesRead` in your template wherever you would like it to appear.

   src/pages/posts/\[slug].astro

   ```astro
   ---
   import { getCollection, render } from 'astro:content';


   export async function getStaticPaths() {
     const blog = await getCollection('blog');
     return blog.map(entry => ({
       params: { slug: entry.id },
       props: { entry },
     }));
   }


   const { entry } = Astro.props;
   const { Content, remarkPluginFrontmatter } = await render(entry);
   ---


   <html>
     <head>...</head>
     <body>
       ...
       <p>{remarkPluginFrontmatter.minutesRead}</p>
       ...
     </body>
   </html>
   ```

   If you’re using a [Markdown layout](/en/basics/layouts/#markdown-layouts), use the `minutesRead` frontmatter property from `Astro.props` in your layout template.

   src/layouts/BlogLayout.astro

   ```astro
   ---
   const { minutesRead } = Astro.props.frontmatter;
   ---


   <html>
     <head>...</head>
     <body>
       <p>{minutesRead}</p>
       <slot />
     </body>
   </html>
   ```

# Add an RSS feed

> Add an RSS feed to your Astro site to let users subscribe to your content.

Astro supports fast, automatic RSS feed generation for blogs and other content websites. RSS feeds provide an easy way for users to subscribe to your content.

## Setting up `@astrojs/rss`

[Section titled “Setting up @astrojs/rss”](#setting-up-astrojsrss)

The package [`@astrojs/rss`](https://github.com/withastro/astro/tree/main/packages/astro-rss) provides helpers for generating RSS feeds using [API endpoints](/en/guides/endpoints/#static-file-endpoints). This unlocks both static builds *and* on-demand generation when using an [SSR adapter](/en/guides/on-demand-rendering/).

1. Install `@astrojs/rss` using your preferred package manager:

   * npm

     ```shell
     npm install @astrojs/rss
     ```

   * pnpm

     ```shell
     pnpm add @astrojs/rss
     ```

   * Yarn

     ```shell
     yarn add @astrojs/rss
     ```

   Tip

   Ensure you’ve [configured a `site`](/en/reference/configuration-reference/#site) in your project’s `astro.config`. This will be used to generate links to your RSS articles.

2. Create a file in `src/pages/` with a name of your choice and the extension `.xml.js` to be used as the output URL for your feed. Some common RSS feed URL names are `feed.xml` or `rss.xml`.

   The example file below `src/pages/rss.xml.js` will create an RSS feed at `site/rss.xml`.

3. Import the `rss()` helper from the `@astrojs/rss` package into your `.xml.js` file and export a function that returns it using the following parameters:

   src/pages/rss.xml.js

   ```js
   import rss from '@astrojs/rss';


   export function GET(context) {
     return rss({
       // `<title>` field in output xml
       title: 'Buzz’s Blog',
       // `<description>` field in output xml
       description: 'A humble Astronaut’s guide to the stars',
       // Pull in your project "site" from the endpoint context
       // https://docs.astro.build/en/reference/api-reference/#site
       site: context.site,
       // Array of `<item>`s in output xml
       // See "Generating items" section for examples using content collections and glob imports
       items: [],
       // (optional) inject custom xml
       customData: `<language>en-us</language>`,
     });
   }
   ```

See the [`@astrojs/rss` README](https://github.com/withastro/astro/tree/main/packages/astro-rss) for the full configuration reference.

## Generating `items`

[Section titled “Generating items”](#generating-items)

The `items` field accepts a list of RSS feed objects, which can be generated from content collections entries using `getCollection()` or from your page files using `pagesGlobToRssItems()`.

The RSS feed standard format includes metadata for each published item, including values such as:

* `title`: The title of the entry. Optional only if a `description` is set. Otherwise, required.
* `description`: A short excerpt from or describing the entry. Optional only if a `title` is set. Otherwise, required.
* `link`: A URL to the original source of the entry. (optional)
* `pubDate`: The date of publication of the entry. (optional)
* `content`: The full content of your post. (optional)

See the [`items` configuration reference](https://github.com/withastro/astro/tree/main/packages/astro-rss#items) for a complete list of options.

### Using content collections

[Section titled “Using content collections”](#using-content-collections)

To create an RSS feed of pages managed in [content collections](/en/guides/content-collections/), use the `getCollection()` function to retrieve the data required for your `items` array. You will need to specify the values for each desired property (e.g. `title`, `description`) from the returned data.

src/pages/rss.xml.js

```js
import rss from '@astrojs/rss';
import { getCollection } from 'astro:content';


export async function GET(context) {
  const blog = await getCollection('blog');
  return rss({
    title: 'Buzz’s Blog',
    description: 'A humble Astronaut’s guide to the stars',
    site: context.site,
    items: blog.map((post) => ({
      title: post.data.title,
      pubDate: post.data.pubDate,
      description: post.data.description,
      // Compute RSS link from post `id`
      // This example assumes all posts are rendered as `/blog/[id]` routes
      link: `/blog/${post.id}/`,
    })),
  });
}
```

Optional: replace your existing blog collection schema to enforce the expected RSS properties.

To ensure that every blog entry produces a valid RSS feed item, you can optionally import and apply `rssSchema` instead of defining each individual property of your schema.

src/content.config.ts

```js
import { defineCollection } from 'astro:content';
import { rssSchema } from '@astrojs/rss';


const blog = defineCollection({
  schema: rssSchema,
});


export const collections = { blog };
```

### Using glob imports

[Section titled “Using glob imports”](#using-glob-imports)

**Added in:** `@astrojs/rss@2.1.0`

To create an RSS feed from documents in `src/pages/`, use the `pagesGlobToRssItems()` helper. This accepts an [`import.meta.glob`](https://vite.dev/guide/features.html#glob-import) result and outputs an array of valid RSS feed items (see [more about writing glob patterns](/en/guides/imports/#glob-patterns) for specifying which pages to include).

Caution

This function assumes, but does not verify, that all necessary feed properties are present in each document’s frontmatter. If you encounter errors, verify each page frontmatter manually.

src/pages/rss.xml.js

```js
import rss, { pagesGlobToRssItems } from '@astrojs/rss';


export async function GET(context) {
  return rss({
    title: 'Buzz’s Blog',
    description: 'A humble Astronaut’s guide to the stars',
    site: context.site,
    items: await pagesGlobToRssItems(
      import.meta.glob('./blog/*.{md,mdx}'),
    ),
  });
}
```

Using an older version?

In versions of `@astrojs/rss` before v2.1.0, pass your glob result straight to `items` without the `pagesGlobToRssItems()` wrapper:

```js
items: import.meta.glob('./blog/*.{md,mdx}'),
```

This method is deprecated for all versions of Astro since v2.1.0, and cannot be used on modern projects.

### Including full post content

[Section titled “Including full post content”](#including-full-post-content)

**Added in:** `astro@1.6.14`

Set the `content` key on `rss.items` to provide the full content of a post as HTML. This allows `@astrojs/rss` to make the full Markdown text of your post available to RSS feed readers. Images and links with full URL paths are also supported. However, images and internal links to other pages using relative paths are not.

When rendering full post content, you will have to consider images, relative links, styles, scripts, and other elements beyond standard Markdown text that you may have in your posts. You may need to include additional logic in your `src/pages/rss.xml.js` endpoint to account for these, or to remove elements that are unnecessary for an RSS feed (e.g. those that are used only for styling or interaction on your website).

You can see [one specific community implementation](https://github.com/delucis/astro-blog-full-text-rss/blob/latest/src/pages/rss.xml.ts) that addresses some of these concerns for an example of how to proceed.

Tip

A package like [`sanitize-html`](https://www.npmjs.com/package/sanitize-html) will make sure that your content is properly sanitized, escaped, and encoded. In the process, such a package might also remove some harmless elements and attributes, so make sure to verify the output and configure the package according to your needs.

When using content collections, render the post `body` using a standard Markdown parser like [`markdown-it`](https://github.com/markdown-it/markdown-it) and sanitize the result, including any extra tags (e.g. `<img>`) needed to render your content:

src/pages/rss.xml.js

```diff
import rss from '@astrojs/rss';
import { getCollection } from 'astro:content';
+import sanitizeHtml from 'sanitize-html';
+import MarkdownIt from 'markdown-it';
+const parser = new MarkdownIt();


export async function GET(context) {
  const blog = await getCollection('blog');
  return rss({
    title: 'Buzz’s Blog',
    description: 'A humble Astronaut’s guide to the stars',
    site: context.site,
    items: blog.map((post) => ({
      link: `/blog/${post.id}/`,
      // Note: this will not process components or JSX expressions in MDX files.
+      content: sanitizeHtml(parser.render(post.body), {
        allowedTags: sanitizeHtml.defaults.allowedTags.concat(['img'])
      }),
      ...post.data,
    })),
  });
}
```

When using glob imports with Markdown, you may use the `compiledContent()` helper to retrieve the rendered HTML for sanitization. Note: this feature is **not** supported for MDX files.

src/pages/rss.xml.js

```diff
import rss from '@astrojs/rss';
+import sanitizeHtml from 'sanitize-html';


export async function GET(context) {
  const postImportResult = import.meta.glob('../posts/**/*.md', { eager: true });
  const posts = Object.values(postImportResult);
  return rss({
    title: 'Buzz’s Blog',
    description: 'A humble Astronaut’s guide to the stars',
    site: context.site,
    items: await Promise.all(posts.map(async (post) => ({
      link: post.url,
+      content: sanitizeHtml((await post.compiledContent())),
      ...post.frontmatter,
    }))),
  });
}
```

## Removing trailing slashes

[Section titled “Removing trailing slashes”](#removing-trailing-slashes)

Astro’s RSS feed produces links with a trailing slash by default, no matter what value you have configured for `trailingSlash`. This means that your RSS links may not match your post URLs exactly.

If you have set `trailingSlash: "never"` on your `astro.config.mjs`, set `trailingSlash: false` in the `rss()` helper so that your feed matches your project configuration.

src/pages/rss.xml.js

```diff
import rss from '@astrojs/rss';


export function GET(context) {
  const posts = Object.values(postImportResult);
  return rss({
    title: 'Buzz’s Blog',
    description: 'A humble Astronaut’s guide to the stars',
    site: context.site,
+    trailingSlash: false,
    items: posts.map((post) => ({
      link: post.url,
      ...post.frontmatter,
    })),
  });
}
```

## Adding a stylesheet

[Section titled “Adding a stylesheet”](#adding-a-stylesheet)

Style your RSS feed for a more pleasant user experience when viewing the file in your browser.

Use the `rss` function’s `stylesheet` option to specify an absolute path to your stylesheet.

```js
rss({
  // ex. use your stylesheet from "public/rss/styles.xsl"
  stylesheet: '/rss/styles.xsl',
  // ...
});
```

Tip

If you’d prefer not to create your own stylesheet, you may use a premade stylesheet such as the [Pretty Feed v3 default stylesheet](https://github.com/genmon/aboutfeeds/blob/main/tools/pretty-feed-v3.xsl). Download the stylesheet from GitHub and save into your project’s `public/` directory.

## Enabling RSS feed auto-discovery

[Section titled “Enabling RSS feed auto-discovery”](#enabling-rss-feed-auto-discovery)

[RSS autodiscovery](https://www.rssboard.org/rss-autodiscovery) allows browsers and other software to automatically find a site’s RSS feed from the main URL.

To enable, add a `<link>` tag with the following attributes to your site’s `head` element:

```jsx
<link
    rel="alternate"
    type="application/rss+xml"
    title="Your Site's Title"
    href={new URL("rss.xml", Astro.site)}
/>
```

With this tag, readers of your blog can enter your site’s base URL into their RSS reader to subscribe to your posts without needing the specific URL of your RSS feed.

## Next Steps

[Section titled “Next Steps”](#next-steps)

After visiting your feed in the browser at `your-domain.com/rss.xml` and confirming that you can see data for each of your posts, you can now [promote your feed on your website](https://medium.com/samsung-internet-dev/add-rss-feeds-to-your-website-to-keep-your-core-readers-engaged-3179dca9c91e#:~:text=com/~deno%2Drss-,Advertising%20your%20RSS%20feed,-Now%20you%20have). Adding the standard RSS icon to your site lets your readers know that they can subscribe to your posts in their own feed reader.

## Resources

[Section titled “Resources”](#resources)

* [RSS Feeds](https://aboutfeeds.com/)

# Share state between Astro components

> Learn how to share state across Astro components with Nano Stores.

Tip

Using framework components? See [how to share state between Islands](/en/recipes/sharing-state-islands/)!

When building an Astro website, you may need to share state across components. Astro recommends the use of [Nano Stores](https://github.com/nanostores/nanostores) for shared client storage.

## Recipe

[Section titled “Recipe”](#recipe)

1. Install Nano Stores:

   * npm

     ```shell
     npm install nanostores
     ```

   * pnpm

     ```shell
     pnpm add nanostores
     ```

   * Yarn

     ```shell
     yarn add nanostores
     ```

2. Create a store. In this example, the store tracks whether a dialog is open or not:

   src/store.js

   ```ts
   import { atom } from 'nanostores';


   export const isOpen = atom(false);
   ```

3. Import and use the store in a `<script>` tag in the components that will share state.

   The following `Button` and `Dialog` components each use the shared `isOpen` state to control whether a particular `<div>` is hidden or displayed:

   src/components/Button.astro

   ```astro
   <button id="openDialog">Open</button>


   <script>
     import { isOpen } from '../store.js';


     // Set the store to true when the button is clicked
     function openDialog() {
       isOpen.set(true);
     }


     // Add an event listener to the button
     document.getElementById('openDialog').addEventListener('click', openDialog);
   </script>
   ```

   src/components/Dialog.astro

   ```astro
   <div id="dialog" style="display: none">Hello world!</div>


   <script>
     import { isOpen } from '../store.js';


     // Listen to changes in the store, and show/hide the dialog accordingly
     isOpen.subscribe(open => {
       if (open) {
         document.getElementById('dialog').style.display = 'block';
       } else {
         document.getElementById('dialog').style.display = 'none';
       }
     })
   </script>
   ```

## Resources

[Section titled “Resources”](#resources)

* [Nano Stores on NPM](https://www.npmjs.com/package/nanostores)
* [Nano Stores documentation for Vanilla JS](https://github.com/nanostores/nanostores#vanilla-js)

# Share state between islands

> Learn how to share state across framework components with Nano Stores.

When building an Astro website with [islands architecture / partial hydration](/en/concepts/islands/), you may have run into this problem: **I want to share state between my components.**

UI frameworks like React or Vue may encourage [“context” providers](https://react.dev/learn/passing-data-deeply-with-context) for other components to consume. But when [partially hydrating components](/en/guides/framework-components/#hydrating-interactive-components) within Astro or Markdown, you can’t use these context wrappers.

Astro recommends a different solution for shared client-side storage: [**Nano Stores**](https://github.com/nanostores/nanostores).

![](/houston_chef.webp) **Related recipe:** [Share state between Astro components](/en/recipes/sharing-state/)

## Why Nano Stores?

[Section titled “Why Nano Stores?”](#why-nano-stores)

The [Nano Stores](https://github.com/nanostores/nanostores) library allows you to author stores that any component can interact with. We recommend Nano Stores because:

* **They’re lightweight.** Nano Stores ship the bare minimum JS you’ll need (less than 1 KB) with zero dependencies.
* **They’re framework-agnostic.** This means sharing state between frameworks will be seamless! Astro is built on flexibility, so we love solutions that offer a similar developer experience no matter your preference.

Still, there are a number of alternatives you can explore. These include:

* [Svelte’s built-in stores](https://svelte.dev/tutorial/writable-stores)
* [Solid signals](https://www.solidjs.com/docs/latest) outside of a component context
* [Vue’s reactivity API](https://vuejs.org/guide/scaling-up/state-management.html#simple-state-management-with-reactivity-api)
* [Sending custom browser events](https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events) between components

FAQ

**🙋 Can I use Nano Stores in `.astro` files or other server-side components?**

Nano Stores can be used in `<script>` tags to [share state between `.astro` components](/en/recipes/sharing-state/). However, Using Nano Stores in the frontmatter of server-side components is not recommended because of the following restrictions:

* Writing to a store from a `.astro` file or [non-hydrated component](/en/guides/framework-components/#hydrating-interactive-components) will *not* affect the value received by [client-side components](/en/reference/directives-reference/#client-directives).
* You cannot pass a Nano Store as a “prop” to client-side components.
* You cannot subscribe to store changes from a `.astro` file, since Astro components do not re-render.

If you understand these restrictions and still find a use case, you can give Nano Stores a try! Just remember that Nano Stores are built for reactivity to changes on the **client** specifically.

**🙋 How do Svelte stores compare to Nano Stores?**

**Nano Stores and [Svelte stores](https://svelte.dev/tutorial/writable-stores) are very similar!** In fact, [nanostores allow you to use the same `$` shortcut](https://github.com/nanostores/nanostores#svelte) for subscriptions that you might use with Svelte stores.

If you want to avoid third-party libraries, [Svelte stores](https://svelte.dev/tutorial/writable-stores) are a great cross-island communication tool on their own. Still, you might prefer Nano Stores if a) you like their add-ons for [“objects”](https://github.com/nanostores/nanostores#maps) and [async state](https://github.com/nanostores/nanostores#lazy-stores), or b) you want to communicate between Svelte and other UI frameworks like Preact or Vue.

**🙋 How do Solid signals compare to Nano Stores?**

If you’ve used Solid for a while, you may have tried moving [signals](https://www.solidjs.com/docs/latest#createsignal) or [stores](https://www.solidjs.com/docs/latest#createstore) outside of your components. This is a great way to share state between Solid islands! Try exporting signals from a shared file:

sharedStore.js

```js
import { createSignal } from 'solid-js';


export const sharedCount = createSignal(0);
```

…and all components importing `sharedCount` will share the same state. Though this works well, you might prefer Nano Stores if a) you like their add-ons for [“objects”](https://github.com/nanostores/nanostores#maps) and [async state](https://github.com/nanostores/nanostores#lazy-stores), or b) you want to communicate between Solid and other UI frameworks like Preact or Vue.

## Installing Nano Stores

[Section titled “Installing Nano Stores”](#installing-nano-stores)

To get started, install Nano Stores alongside their helper package for your favorite UI framework:

* Preact

  ```shell
  npm install nanostores @nanostores/preact
  ```

* React

  ```shell
  npm install nanostores @nanostores/react
  ```

* Solid

  ```shell
  npm install nanostores @nanostores/solid
  ```

* Svelte

  ```shell
  npm install nanostores
  ```

  Note

  No helper package here! Nano Stores can be used like standard Svelte stores.

* Vue

  ```shell
  npm install nanostores @nanostores/vue
  ```

You can jump into the [Nano Stores usage guide](https://github.com/nanostores/nanostores#guide) from here, or follow along with our example below!

## Usage example - ecommerce cart flyout

[Section titled “Usage example - ecommerce cart flyout”](#usage-example---ecommerce-cart-flyout)

Let’s say we’re building a simple ecommerce interface with three interactive elements:

* An “add to cart” submission form
* A cart flyout to display those added items
* A cart flyout toggle

[](/videos/stores-example.mp4)

*[**Try the completed example**](https://github.com/withastro/astro/tree/main/examples/with-nanostores) on your machine or online via StackBlitz.*

Your base Astro file may look like this:

src/pages/index.astro

```astro
---
import CartFlyoutToggle from '../components/CartFlyoutToggle';
import CartFlyout from '../components/CartFlyout';
import AddToCartForm from '../components/AddToCartForm';
---


<!DOCTYPE html>
<html lang="en">
<head>...</head>
<body>
  <header>
    <nav>
      <a href="/">Astro storefront</a>
      <CartFlyoutToggle client:load />
    </nav>
  </header>
  <main>
    <AddToCartForm client:load>
    <!-- ... -->
    </AddToCartForm>
  </main>
  <CartFlyout client:load />
</body>
</html>
```

### Using “atoms”

[Section titled “Using “atoms””](#using-atoms)

Let’s start by opening our `CartFlyout` whenever `CartFlyoutToggle` is clicked.

First, create a new JS or TS file to contain our store. We’ll use an [“atom”](https://github.com/nanostores/nanostores#atoms) for this:

src/cartStore.js

```js
import { atom } from 'nanostores';


export const isCartOpen = atom(false);
```

Now, we can import this store into any file that needs to read or write. We’ll start by wiring up our `CartFlyoutToggle`:

* Preact

  src/components/CartFlyoutToggle.jsx

  ```jsx
  import { useStore } from '@nanostores/preact';
  import { isCartOpen } from '../cartStore';


  export default function CartButton() {
    // read the store value with the `useStore` hook
    const $isCartOpen = useStore(isCartOpen);
    // write to the imported store using `.set`
    return (
      <button onClick={() => isCartOpen.set(!$isCartOpen)}>Cart</button>
    )
  }
  ```

* React

  src/components/CartFlyoutToggle.jsx

  ```jsx
  import { useStore } from '@nanostores/react';
  import { isCartOpen } from '../cartStore';


  export default function CartButton() {
    // read the store value with the `useStore` hook
    const $isCartOpen = useStore(isCartOpen);
    // write to the imported store using `.set`
    return (
      <button onClick={() => isCartOpen.set(!$isCartOpen)}>Cart</button>
    )
  }
  ```

* Solid

  src/components/CartFlyoutToggle.jsx

  ```jsx
  import { useStore } from '@nanostores/solid';
  import { isCartOpen } from '../cartStore';


  export default function CartButton() {
    // read the store value with the `useStore` hook
    const $isCartOpen = useStore(isCartOpen);
    // write to the imported store using `.set`
    return (
      <button onClick={() => isCartOpen.set(!$isCartOpen())}>Cart</button>
    )
  }
  ```

* Svelte

  src/components/CartFlyoutToggle.svelte

  ```svelte
  <script>
    import { isCartOpen } from '../cartStore';
  </script>


  <!--use "$" to read the store value-->
  <button on:click={() => isCartOpen.set(!$isCartOpen)}>Cart</button>
  ```

* Vue

  src/components/CartFlyoutToggle.vue

  ```vue
  <template>
    <!--write to the imported store using `.set`-->
    <button @click="isCartOpen.set(!$isCartOpen)">Cart</button>
  </template>


  <script setup>
    import { isCartOpen } from '../cartStore';
    import { useStore } from '@nanostores/vue';


    // read the store value with the `useStore` hook
    const $isCartOpen = useStore(isCartOpen);
  </script>
  ```

Then, we can read `isCartOpen` from our `CartFlyout` component:

* Preact

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/preact';
  import { isCartOpen } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);


    return $isCartOpen ? <aside>...</aside> : null;
  }
  ```

* React

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/react';
  import { isCartOpen } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);


    return $isCartOpen ? <aside>...</aside> : null;
  }
  ```

* Solid

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/solid';
  import { isCartOpen } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);


    return $isCartOpen() ? <aside>...</aside> : null;
  }
  ```

* Svelte

  src/components/CartFlyout.svelte

  ```svelte
  <script>
    import { isCartOpen } from '../cartStore';
  </script>


  {#if $isCartOpen}
  <aside>...</aside>
  {/if}
  ```

* Vue

  src/components/CartFlyout.vue

  ```vue
  <template>
    <aside v-if="$isCartOpen">...</aside>
  </template>


  <script setup>
    import { isCartOpen } from '../cartStore';
    import { useStore } from '@nanostores/vue';


    const $isCartOpen = useStore(isCartOpen);
  </script>
  ```

### Using “maps”

[Section titled “Using “maps””](#using-maps)

Tip

**[Maps](https://github.com/nanostores/nanostores#maps) are a great choice for objects you write to regularly!** Alongside the standard `get()` and `set()` helpers an `atom` provides, you’ll also have a `.setKey()` function to efficiently update individual object keys.

Now, let’s keep track of the items inside your cart. To avoid duplicates and keep track of “quantity,” we can store your cart as an object with the item’s ID as a key. We’ll use a [Map](https://github.com/nanostores/nanostores#maps) for this.

Let’s add a `cartItem` store to our `cartStore.js` from earlier. You can also switch to a TypeScript file to define the shape if you’re so inclined.

* JavaScript

  src/cartStore.js

  ```js
  import { atom, map } from 'nanostores';


  export const isCartOpen = atom(false);


  /**
   * @typedef {Object} CartItem
   * @property {string} id
   * @property {string} name
   * @property {string} imageSrc
   * @property {number} quantity
   */


  /** @type {import('nanostores').MapStore<Record<string, CartItem>>} */
  export const cartItems = map({});
  ```

* TypeScript

  src/cartStore.ts

  ```ts
  import { atom, map } from 'nanostores';


  export const isCartOpen = atom(false);


  export type CartItem = {
    id: string;
    name: string;
    imageSrc: string;
    quantity: number;
  }


  export const cartItems = map<Record<string, CartItem>>({});
  ```

Now, let’s export an `addCartItem` helper for our components to use.

* **If that item doesn’t exist in your cart**, add the item with a starting quantity of 1.
* **If that item *does* already exist**, bump the quantity by 1.

- JavaScript

  src/cartStore.js

  ```js
  ...
  export function addCartItem({ id, name, imageSrc }) {
    const existingEntry = cartItems.get()[id];
    if (existingEntry) {
      cartItems.setKey(id, {
        ...existingEntry,
        quantity: existingEntry.quantity + 1,
      })
    } else {
      cartItems.setKey(
        id,
        { id, name, imageSrc, quantity: 1 }
      );
    }
  }
  ```

- TypeScript

  src/cartStore.ts

  ```ts
  ...
  type ItemDisplayInfo = Pick<CartItem, 'id' | 'name' | 'imageSrc'>;
  export function addCartItem({ id, name, imageSrc }: ItemDisplayInfo) {
    const existingEntry = cartItems.get()[id];
    if (existingEntry) {
      cartItems.setKey(id, {
        ...existingEntry,
        quantity: existingEntry.quantity + 1,
      });
    } else {
      cartItems.setKey(
        id,
        { id, name, imageSrc, quantity: 1 }
      );
    }
  }
  ```

Note

**🙋 Why use `.get()` here instead of a `useStore` helper?**

You may have noticed we’re calling `cartItems.get()` here, instead of grabbing that `useStore` helper from our React / Preact / Solid / Vue examples. This is because **useStore is meant to trigger component re-renders.** In other words, `useStore` should be used whenever the store value is being rendered to the UI. Since we’re reading the value when an **event** is triggered (`addToCart` in this case), and we aren’t trying to render that value, we don’t need `useStore` here.

With our store in place, we can call this function inside our `AddToCartForm` whenever that form is submitted. We’ll also open the cart flyout so you can see a full cart summary.

* Preact

  src/components/AddToCartForm.jsx

  ```jsx
  import { addCartItem, isCartOpen } from '../cartStore';


  export default function AddToCartForm({ children }) {
    // we'll hardcode the item info for simplicity!
    const hardcodedItemInfo = {
      id: 'astronaut-figurine',
      name: 'Astronaut Figurine',
      imageSrc: '/images/astronaut-figurine.png',
    }


    function addToCart(e) {
      e.preventDefault();
      isCartOpen.set(true);
      addCartItem(hardcodedItemInfo);
    }


    return (
      <form onSubmit={addToCart}>
        {children}
      </form>
    )
  }
  ```

* React

  src/components/AddToCartForm.jsx

  ```jsx
  import { addCartItem, isCartOpen } from '../cartStore';


  export default function AddToCartForm({ children }) {
    // we'll hardcode the item info for simplicity!
    const hardcodedItemInfo = {
      id: 'astronaut-figurine',
      name: 'Astronaut Figurine',
      imageSrc: '/images/astronaut-figurine.png',
    }


    function addToCart(e) {
      e.preventDefault();
      isCartOpen.set(true);
      addCartItem(hardcodedItemInfo);
    }


    return (
      <form onSubmit={addToCart}>
        {children}
      </form>
    )
  }
  ```

* Solid

  src/components/AddToCartForm.jsx

  ```jsx
  import { addCartItem, isCartOpen } from '../cartStore';


  export default function AddToCartForm({ children }) {
    // we'll hardcode the item info for simplicity!
    const hardcodedItemInfo = {
      id: 'astronaut-figurine',
      name: 'Astronaut Figurine',
      imageSrc: '/images/astronaut-figurine.png',
    }


    function addToCart(e) {
      e.preventDefault();
      isCartOpen.set(true);
      addCartItem(hardcodedItemInfo);
    }


    return (
      <form onSubmit={addToCart}>
        {children}
      </form>
    )
  }
  ```

* Svelte

  src/components/AddToCartForm.svelte

  ```svelte
  <form on:submit|preventDefault={addToCart}>
    <slot></slot>
  </form>


  <script>
    import { addCartItem, isCartOpen } from '../cartStore';


    // we'll hardcode the item info for simplicity!
    const hardcodedItemInfo = {
      id: 'astronaut-figurine',
      name: 'Astronaut Figurine',
      imageSrc: '/images/astronaut-figurine.png',
    }


    function addToCart() {
      isCartOpen.set(true);
      addCartItem(hardcodedItemInfo);
    }
  </script>
  ```

* Vue

  src/components/AddToCartForm.vue

  ```vue
  <template>
    <form @submit="addToCart">
      <slot></slot>
    </form>
  </template>


  <script setup>
    import { addCartItem, isCartOpen } from '../cartStore';


    // we'll hardcode the item info for simplicity!
    const hardcodedItemInfo = {
      id: 'astronaut-figurine',
      name: 'Astronaut Figurine',
      imageSrc: '/images/astronaut-figurine.png',
    }


    function addToCart(e) {
      e.preventDefault();
      isCartOpen.set(true);
      addCartItem(hardcodedItemInfo);
    }
  </script>
  ```

Finally, we’ll render those cart items inside our `CartFlyout`:

* Preact

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/preact';
  import { isCartOpen, cartItems } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);
    const $cartItems = useStore(cartItems);


    return $isCartOpen ? (
      <aside>
        {Object.values($cartItems).length ? (
          <ul>
            {Object.values($cartItems).map(cartItem => (
              <li>
                <img src={cartItem.imageSrc} alt={cartItem.name} />
                <h3>{cartItem.name}</h3>
                <p>Quantity: {cartItem.quantity}</p>
              </li>
            ))}
          </ul>
        ) : <p>Your cart is empty!</p>}
      </aside>
    ) : null;
  }
  ```

* React

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/react';
  import { isCartOpen, cartItems } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);
    const $cartItems = useStore(cartItems);


    return $isCartOpen ? (
      <aside>
        {Object.values($cartItems).length ? (
          <ul>
            {Object.values($cartItems).map(cartItem => (
              <li>
                <img src={cartItem.imageSrc} alt={cartItem.name} />
                <h3>{cartItem.name}</h3>
                <p>Quantity: {cartItem.quantity}</p>
              </li>
            ))}
          </ul>
        ) : <p>Your cart is empty!</p>}
      </aside>
    ) : null;
  }
  ```

* Solid

  src/components/CartFlyout.jsx

  ```jsx
  import { useStore } from '@nanostores/solid';
  import { isCartOpen, cartItems } from '../cartStore';


  export default function CartFlyout() {
    const $isCartOpen = useStore(isCartOpen);
    const $cartItems = useStore(cartItems);


    return $isCartOpen() ? (
      <aside>
        {Object.values($cartItems()).length ? (
          <ul>
            {Object.values($cartItems()).map(cartItem => (
              <li>
                <img src={cartItem.imageSrc} alt={cartItem.name} />
                <h3>{cartItem.name}</h3>
                <p>Quantity: {cartItem.quantity}</p>
              </li>
            ))}
          </ul>
        ) : <p>Your cart is empty!</p>}
      </aside>
    ) : null;
  }
  ```

* Svelte

  src/components/CartFlyout.svelte

  ```svelte
  <script>
    import { isCartOpen, cartItems } from '../cartStore';
  </script>


  {#if $isCartOpen}
    {#if Object.values($cartItems).length}
      <aside>
        {#each Object.values($cartItems) as cartItem}
        <li>
          <img src={cartItem.imageSrc} alt={cartItem.name} />
          <h3>{cartItem.name}</h3>
          <p>Quantity: {cartItem.quantity}</p>
        </li>
        {/each}
      </aside>
    {:else}
      <p>Your cart is empty!</p>
    {/if}
  {/if}
  ```

* Vue

  src/components/CartFlyout.vue

  ```vue
  <template>
    <aside v-if="$isCartOpen">
      <ul v-if="Object.values($cartItems).length">
        <li v-for="cartItem in Object.values($cartItems)" v-bind:key="cartItem.name">
          <img :src=cartItem.imageSrc :alt=cartItem.name />
          <h3>{{cartItem.name}}</h3>
          <p>Quantity: {{cartItem.quantity}}</p>
        </li>
      </ul>
      <p v-else>Your cart is empty!</p>
    </aside>
  </template>


  <script setup>
    import { cartItems, isCartOpen } from '../cartStore';
    import { useStore } from '@nanostores/vue';


    const $isCartOpen = useStore(isCartOpen);
    const $cartItems = useStore(cartItems);
  </script>
  ```

Now, you should have a fully interactive ecommerce example with the smallest JS bundle in the galaxy 🚀

[**Try the completed example**](https://github.com/withastro/astro/tree/main/examples/with-nanostores) on your machine or online via StackBlitz!

# Using streaming to improve page performance

> Learn how to use streaming to improve page performance.

Astro’s SSR uses HTML streaming to send each component to the browser when available for faster page loading. To improve your page’s performance even further, you can build your components strategically to optimize their loading by avoiding blocking data fetches.

The following refactoring example demonstrates how to improve page performance by moving fetch calls to other components, moving them out of a component where they block page rendering.

The following page `await`s some data in its frontmatter. Astro will wait for all of the `fetch` calls to resolve before sending any HTML to the browser.

src/pages/index.astro

```astro
---
const personResponse = await fetch('https://randomuser.me/api/');
const personData = await personResponse.json();
const randomPerson = personData.results[0];
const factResponse = await fetch('https://catfact.ninja/fact');
const factData = await factResponse.json();
---
<html>
  <head>
    <title>A name and a fact</title>
  </head>
  <body>
    <h2>A name</h2>
    <p>{randomPerson.name.first}</p>
    <h2>A fact</h2>
    <p>{factData.fact}</p>
  </body>
</html>
```

Moving the `await` calls into smaller components allows you to take advantage of Astro’s streaming. Using the following components to perform the data fetches, Astro can render some HTML first, such as the title, and then the paragraphs when the data is ready.

src/components/RandomName.astro

```astro
---
const personResponse = await fetch('https://randomuser.me/api/');
const personData = await personResponse.json();
const randomPerson = personData.results[0];
---
<p>{randomPerson.name.first}</p>
```

src/components/RandomFact.astro

```astro
---
const factResponse = await fetch('https://catfact.ninja/fact');
const factData = await factResponse.json();
---
<p>{factData.fact}</p>
```

The Astro page below using these components can render parts of the page sooner. The `<head>`, `<body>`, and `<h2>` tags are no longer blocked by data fetches. The server will then fetch data for `RandomName` and `RandomFact` in parallel and stream the resulting HTML to the browser.

src/pages/index.astro

```astro
---
import RandomName from '../components/RandomName.astro';
import RandomFact from '../components/RandomFact.astro';
---
<html>
  <head>
    <title>A name and a fact</title>
  </head>
  <body>
    <h2>A name</h2>
    <RandomName />
    <h2>A fact</h2>
    <RandomFact />
  </body>
</html>
```

#### Including Promises directly

[Section titled “Including Promises directly”](#including-promises-directly)

You can also include promises directly in the template. Instead of blocking the entire component, it will resolve the promise in parallel and only block the markup that comes after it.

src/pages/index.astro

```astro
---
const personPromise = fetch('https://randomuser.me/api/')
  .then(response => response.json())
  .then(personData => personData.results[0].name.first);
const factPromise = fetch('https://catfact.ninja/fact')
  .then(response => response.json())
  .then(factData => factData.fact);
---
<html>
  <head>
    <title>A name and a fact</title>
  </head>
  <body>
    <h2>A name</h2>
    <p>{personPromise}</p>
    <h2>A fact</h2>
    <p>{factPromise}</p>
  </body>
</html>
```

In this example, `A name` will render while `personPromise` and `factPromise` are loading. Once `personPromise` has resolved, `A fact` will appear and `factPromise` will render when it’s finished loading.

# Style rendered Markdown with Tailwind Typography

> Learn how to use @tailwind/typography to style your rendered Markdown.

You can use [Tailwind](https://tailwindcss.com)’s Typography plugin to style rendered Markdown from sources such as Astro’s [**content collections**](/en/guides/content-collections/).

This recipe will teach you how to create a reusable Astro component to style your Markdown content using Tailwind’s utility classes.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

An Astro project that:

* has [Tailwind’s Vite plugin](/en/guides/styling/#tailwind) installed.
* uses Astro’s [content collections](/en/guides/content-collections/).

## Setting Up `@tailwindcss/typography`

[Section titled “Setting Up @tailwindcss/typography”](#setting-up-tailwindcsstypography)

First, install `@tailwindcss/typography` using your preferred package manager.

* npm

  ```shell
  npm install -D @tailwindcss/typography
  ```

* pnpm

  ```shell
  pnpm add -D @tailwindcss/typography
  ```

* Yarn

  ```shell
  yarn add --dev @tailwindcss/typography
  ```

Then, add the package as a plugin in your Tailwind configuration file.

src/styles/global.css

```diff
@import 'tailwindcss';
+@plugin '@tailwindcss/typography';
```

## Recipe

[Section titled “Recipe”](#recipe)

1. Create a `<Prose />` component to provide a wrapping `<div>` with a `<slot />` for your rendered Markdown. Add the style class `prose` alongside any desired [Tailwind element modifiers](https://tailwindcss.com/docs/typography-plugin#element-modifiers) in the parent element.

   src/components/Prose.astro

   ```astro
   ---
   ---
   <div
     class="prose dark:prose-invert
     prose-h1:font-bold prose-h1:text-xl
     prose-a:text-blue-600 prose-p:text-justify prose-img:rounded-xl
     prose-headings:underline">
     <slot />
   </div>
   ```

   Tip

   The `@tailwindcss/typography` plugin uses [**element modifiers**](https://tailwindcss.com/docs/typography-plugin#element-modifiers) to style child components of a container with the `prose` class.

   These modifiers follow the following general syntax:

   ```plaintext
   prose-[element]:class-to-apply
   ```

   For example, `prose-h1:font-bold` gives all `<h1>` tags the `font-bold` Tailwind class.

2. Query your collection entry on the page you want to render your Markdown. Pass the `<Content />` component from `await render(entry)` to `<Prose />` as a child to wrap your Markdown content in Tailwind styles.

   src/pages/index.astro

   ```astro
   ---
   import Prose from '../components/Prose.astro';
   import Layout from '../layouts/Layout.astro';
   import { getEntry, render } from 'astro:content';


   const entry = await getEntry('collection', 'entry');
   const { Content } = await render(entry);
   ---
   <Layout>
     <Prose>
       <Content />
     </Prose>
   </Layout>
   ```

## Resources

[Section titled “Resources”](#resources)

* [Tailwind Typography Documentation](https://tailwindcss.com/docs/typography-plugin)

