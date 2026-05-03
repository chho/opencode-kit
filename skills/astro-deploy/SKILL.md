---
name: astro-deploy
description: Astro deployment guides for 20+ hosting platforms - AWS, Cloudflare, Deno, Fly.io, GitHub Pages, GitLab Pages, Google Cloud, Heroku, Netlify, Railway, Render, Vercel, Zeabur, Zerops, and more. Load this when deploying or configuring hosting for an Astro site.
---

# Astro Deployment Guides

> This skill contains deployment instructions for all supported hosting platforms.


# Deploy your Astro Site

> How to deploy your Astro site to the web.

**Ready to build and deploy your Astro site?** Follow one of our guides to different deployment services or scroll down for general guidance about deploying an Astro site.

## Deployment Guides

[Section titled “Deployment Guides”](#deployment-guides)

* ![](/logos/aws.svg)

  ### [AWS](/en/guides/deploy/aws/)

  On demandStatic

* ![](/logos/flightcontrol.svg)

  ### [AWS via Flightcontrol](/en/guides/deploy/aws-via-flightcontrol/)

  On demandStatic

* ![](/logos/sst.svg)

  ### [AWS via SST](/en/guides/deploy/aws-via-sst/)

  On demandStatic

* ![](/logos/azion.svg)

  ### [Azion](/en/guides/deploy/azion/)

  On demandStatic

* ![](/logos/buddy.svg)

  ### [Buddy](/en/guides/deploy/buddy/)

  Static

* ![](/logos/cleavr.svg)

  ### [Cleavr](/en/guides/deploy/cleavr/)

  On demandStatic

* ![](/logos/clever-cloud.svg)

  ### [Clever Cloud](/en/guides/deploy/clever-cloud/)

  StaticOn demand

* ![](/logos/cloudflare-pages.svg)

  ### [Cloudflare](/en/guides/deploy/cloudflare/)

  On demandStatic

* ![](/logos/cloudray.svg)

  ### [CloudRay](/en/guides/deploy/cloudray/)

  Static

* ![](/logos/deno.svg)

  ### [Deno Deploy](/en/guides/deploy/deno/)

  On demandStatic

* ![](/logos/deployhq.svg)

  ### [DeployHQ](/en/guides/deploy/deployhq/)

  Static

* ![](/logos/edgeone-pages.svg)

  ### [EdgeOne Pages](/en/guides/deploy/edgeone-pages/)

  On demandStatic

* ![](/logos/firebase.svg)

  ### [Firebase](/en/guides/deploy/firebase/)

  On demandStatic

* ![](/logos/fleek.svg)

  ### [Fleek](/en/guides/deploy/fleek/)

  Static

* ![](/logos/flyio.svg)

  ### [Fly.io](/en/guides/deploy/flyio/)

  On demandStatic

* ![](/logos/github.svg)

  ### [GitHub Pages](/en/guides/deploy/github/)

  Static

* ![](/logos/gitlab.svg)

  ### [GitLab Pages](/en/guides/deploy/gitlab/)

  Static

* ![](/logos/google-cloud.svg)

  ### [Google Cloud](/en/guides/deploy/google-cloud/)

  On demandStatic

* ![](/logos/heroku.svg)

  ### [Heroku](/en/guides/deploy/heroku/)

  Static

* ![](/logos/juno.svg)

  ### [Juno](/en/guides/deploy/juno/)

  Static

* ![](/logos/microsoft-azure.svg)

  ### [Microsoft Azure](/en/guides/deploy/microsoft-azure/)

  Static

* ![](/logos/netlify.svg)

  ### [Netlify](/en/guides/deploy/netlify/)

  On demandStatic

* ![](/logos/railway.svg)

  ### [Railway](/en/guides/deploy/railway/)

  On demandStatic

* ![](/logos/render.svg)

  ### [Render](/en/guides/deploy/render/)

  Static

* ![](/logos/seenode.svg)

  ### [Seenode](/en/guides/deploy/seenode/)

  On demand

* ![](/logos/sevalla.svg)

  ### [Sevalla](/en/guides/deploy/sevalla/)

  On demandStatic

* ![](/logos/stormkit.svg)

  ### [Stormkit](/en/guides/deploy/stormkit/)

  Static

* ![](/logos/surge.svg)

  ### [Surge](/en/guides/deploy/surge/)

  Static

* ![](/logos/vercel.svg)

  ### [Vercel](/en/guides/deploy/vercel/)

  On demandStatic

* ![](/logos/zeabur.svg)

  ### [Zeabur](/en/guides/deploy/zeabur/)

  On demandStatic

* ![](/logos/zephyr.svg)

  ### [Zephyr Cloud](/en/guides/deploy/zephyr/)

  Static

* ![](/logos/zerops.svg)

  ### [Zerops](/en/guides/deploy/zerops/)

  On demandStatic

## Quick Deploy Options

[Section titled “Quick Deploy Options”](#quick-deploy-options)

You can build and deploy an Astro site to a number of hosts quickly using either their website’s dashboard UI or a CLI.

### Website UI

[Section titled “Website UI”](#website-ui)

A quick way to deploy your website is to connect your Astro project’s online Git repository (e.g. GitHub, GitLab, Bitbucket) to a host provider and take advantage of continuous deployment using Git.

These host platforms automatically detect pushes to your Astro project’s source repository, build your site and deploy it to the web at a custom URL or your personal domain. Often, setting up a deployment on these platforms will follow steps something like the following:

1. Add your repository to an online Git provider (e.g. in GitHub, GitLab, Bitbucket)

2. Choose a host that supports **continuous deployment** (e.g. [Netlify](/en/guides/deploy/netlify/) or [Vercel](/en/guides/deploy/vercel/)) and import your Git repository as a new site/project.

   Many common hosts will recognize your project as an Astro site, and should choose the appropriate configuration settings to build and deploy your site as shown below. (If not, these settings can be changed.)

   Deploy settings

   * **Build Command:** `astro build` or `npm run build`
   * **Publish directory:** `dist`

3. Click “Deploy” and your new website will be created at a unique URL for that host (e.g. `new-astro-site.netlify.app`).

The host will be automatically configured to watch your Git provider’s main branch for changes, and to rebuild and republish your site at each new commit. These settings can typically be configured in your host provider’s dashboard UI.

### CLI Deployment

[Section titled “CLI Deployment”](#cli-deployment)

Some hosts will have their own command line interface (CLI) you can install globally to your machine using npm. Often, using a CLI to deploy looks something like the following:

1. Install your host’s CLI globally, for example:

   * npm

     ```shell
     npm install --global netlify-cli
     ```

   * pnpm

     ```shell
     pnpm add --global netlify-cli
     ```

   * Yarn

     ```shell
     yarn global add netlify-cli
     ```

2. Run the CLI and follow any instructions for authorization, setup etc.

3. Build your site and deploy to your host

   Many common hosts will build and deploy your site for you. They will usually recognize your project as an Astro site, and should choose the appropriate configuration settings to build and deploy as shown below. (If not, these settings can be changed.)

   Deploy settings

   * **Build Command:** `astro build` or `npm run build`
   * **Publish directory:** `dist`

   Other hosts will require you to [build your site locally](#building-your-site-locally) and deploy using the command line.

## Building Your Site Locally

[Section titled “Building Your Site Locally”](#building-your-site-locally)

Many hosts like Netlify and Vercel will build your site for you and then publish that build output to the web. But, some sites will require you to build locally and then run a deploy command or upload your build output.

You may also wish to build locally to preview your site, or to catch any potential errors and warnings in your own environment.

Run the command `npm run build` to build your Astro site.

* npm

  ```shell
  npm run build
  ```

* pnpm

  ```shell
  pnpm run build
  ```

* Yarn

  ```shell
  yarn run build
  ```

By default, the build output will be placed at `dist/`. This location can be changed using the [`outDir` configuration option](/en/reference/configuration-reference/#outdir).

## Adding an Adapter for on-demand rendering

[Section titled “Adding an Adapter for on-demand rendering”](#adding-an-adapter-for-on-demand-rendering)

Note

Before deploying your Astro site with [on-demand rendering](/en/guides/on-demand-rendering/) enabled, make sure you have:

* Installed the [appropriate adapter](/en/guides/on-demand-rendering/) to your project dependencies (either manually, or using the adapter’s `astro add` command, e.g. `npx astro add netlify`).
* [Added the adapter](/en/reference/configuration-reference/#integrations) to your `astro.config.mjs` file’s import and default export when installing manually. (The `astro add` command will take care of this step for you!)

# Deploy your Astro Site to AWS

> How to deploy your Astro site to the web using AWS.

[AWS](https://aws.amazon.com/) is a full-featured web app hosting platform that can be used to deploy an Astro site.

Deploying your project to AWS requires using the [AWS console](https://aws.amazon.com/console/). (Most of these actions can also be done using the [AWS CLI](https://aws.amazon.com/cli/)). This guide will walk you through the steps to deploy your site to AWS using [AWS Amplify](https://aws.amazon.com/amplify/), [S3 static website hosting](https://aws.amazon.com/s3/), and [CloudFront](https://aws.amazon.com/cloudfront/).

## AWS Amplify

[Section titled “AWS Amplify”](#aws-amplify)

AWS Amplify is a set of purpose-built tools and features that lets frontend web and mobile developers quickly and easily build full-stack applications on AWS. You can either deploy your Astro project as a static site, or as a server-rendered site.

### Static Site

[Section titled “Static Site”](#static-site)

Your Astro project is a static site by default.

1. Create a new Amplify Hosting project.

2. Connect your repository to Amplify.

3. Modify your build settings to match your project’s build process.

   * npm

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - npm ci
         build:
           commands:
             - npm run build
       artifacts:
         baseDirectory: /dist
         files:
           - '**/*'
       cache:
         paths:
           - node_modules/**/*
     ```

   * pnpm

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - npm i -g pnpm
             - pnpm config set store-dir .pnpm-store
             - pnpm i
         build:
           commands:
             - pnpm run build
       artifacts:
         baseDirectory: /dist
         files:
           - '**/*'
       cache:
         paths:
           - .pnpm-store/**/*
     ```

   * Yarn

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - yarn install
         build:
           commands:
             - yarn build
       artifacts:
         baseDirectory: /dist
         files:
           - '**/*'
       cache:
         paths:
           - node_modules/**/*
     ```

Amplify will automatically deploy your website and update it when you push a commit to your repository.

### Adapter for on-demand rendering

[Section titled “Adapter for on-demand rendering”](#adapter-for-on-demand-rendering)

In order to deploy your project as a server-rendered site, you will need to use the third-party, [community-maintained AWS Amplify adapter](https://github.com/alexnguyennz/astro-aws-amplify) and make some changes to your config.

First, install the Amplify adapter.

* npm

  ```shell
  npm install astro-aws-amplify
  ```

* pnpm

  ```shell
  pnpm add astro-aws-amplify
  ```

* Yarn

  ```shell
  yarn add astro-aws-amplify
  ```

Then, in your `astro.config.*` file, add the adapter and set the output to `server`.

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
+import awsAmplify from 'astro-aws-amplify';


export default defineConfig({
  // ...
+  output: "server",
+  adapter: awsAmplify(),
});
```

Once the adapter has been installed, you can set up your Amplify project.

1. Create a new Amplify Hosting project.

2. Connect your repository to Amplify.

3. Modify your build settings to match the adapter’s build process by either editing the build settings in the AWS console, or by adding an `amplify.yaml` in the root of your project.

   * npm

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - npm ci --cache .npm --prefer-offline
         build:
           commands:
             - npm run build
             - mv node_modules ./.amplify-hosting/compute/default
       artifacts:
         baseDirectory: .amplify-hosting
         files:
           - '**/*'
       cache:
         paths:
           - .npm/**/*
     ```

   * pnpm

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - npm i -g pnpm
             - pnpm config set store-dir .pnpm-store
             - pnpm i
         build:
           commands:
             - pnpm run build
             - mv node_modules ./.amplify-hosting/compute/default
       artifacts:
         baseDirectory: .amplify-hosting
         files:
           - '**/*'
       cache:
         paths:
           - .pnpm-store/**/*
     ```

   * Yarn

     ```yaml
     version: 1
     frontend:
       phases:
         preBuild:
           commands:
             - yarn install
         build:
           commands:
             - yarn build
             - mv node_modules ./.amplify-hosting/compute/default
       artifacts:
         baseDirectory: .amplify-hosting
         files:
           - '**/*'
       cache:
         paths:
           - node_modules/**/*
     ```

Amplify will automatically deploy your website and update it when you push a commit to your repository.

See [AWS’s Astro deployment guide](https://docs.aws.amazon.com/amplify/latest/userguide/get-started-astro.html) for more info.

## S3 static website hosting

[Section titled “S3 static website hosting”](#s3-static-website-hosting)

S3 is the starting point of any application. It is where your project files and other assets are stored. S3 charges for file storage and number of requests. You can find more information about S3 in the [AWS documentation](https://aws.amazon.com/s3/).

1. Create an S3 bucket with your project’s name.

   Tip

   The bucket name should be globally unique. We recommend a combination of your project name and the domain name of your site.

2. Disable **“Block all public access”**. By default, AWS sets all buckets to be private. To make it public, you need to uncheck the “Block public access” checkbox in the bucket’s properties.

3. Upload your built files located in `dist` to S3. You can do this manually in the console or use the AWS CLI. If you use the AWS CLI, use the following command after [authenticating with your AWS credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html):

   ```plaintext
   aws s3 cp dist/ s3://<BUCKET_NAME>/ --recursive
   ```

4. Update your bucket policy to allow public access. You can find this setting in the bucket’s **Permissions > Bucket policy**.

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::<BUCKET_NAME>/*"
       }
     ]
   }
   ```

   Caution

   Do not forget to replace `<BUCKET_NAME>` with the name of your bucket.

5. Enable website hosting for your bucket. You can find this setting in the bucket’s **Properties > Static website hosting**. Set your index document to `index.html` and your error document to `404.html`. Finally, you can find your new website URL in the bucket’s **Properties > Static website hosting**.

   Note

   If you are deploying a single-page application (SPA), set your error document to `index.html`.

## S3 with CloudFront

[Section titled “S3 with CloudFront”](#s3-with-cloudfront)

CloudFront is a web service that provides content delivery network (CDN) capabilities. It is used to cache content of a web server and distribute it to end users. CloudFront charges for the amount of data transferred. Adding CloudFront to your S3 bucket is more cost-effective and provides a faster delivery.

To connect S3 with CloudFront, create a CloudFront distribution with the following values:

* **Origin domain:** Your S3 bucket static website endpoint. You can find your endpoint in your S3 bucket’s **Properties > Static website hosting**. Alternative, you can select your s3 bucket and click on the callout to replace your bucket address with your bucket static endpoint.
* **Viewer protocol policy:** “Redirect to HTTPS”

This configuration will serve your site using the CloudFront CDN network. You can find your CloudFront distribution URL in the bucket’s **Distributions > Domain name**.

Note

When connecting CloudFront to an S3 static website endpoint, you rely on S3 bucket policies for access control. See [S3 static website hosting](#s3-static-website-hosting) section for more information about bucket policies.

## Continuous deployment with GitHub Actions

[Section titled “Continuous deployment with GitHub Actions”](#continuous-deployment-with-github-actions)

There are many ways to set up continuous deployment for AWS. One possibility for code hosted on GitHub is to use [GitHub Actions](https://github.com/features/actions) to deploy your website every time you push a commit.

1. Create a new policy in your AWS account using [IAM](https://aws.amazon.com/iam/) with the following permissions. This policy will allow you to upload built files to your S3 bucket and invalidate the CloudFront distribution files when you push a commit.

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
         {
             "Sid": "VisualEditor0",
             "Effect": "Allow",
             "Action": [
                 "s3:PutObject",
                 "s3:ListBucket",
                 "s3:DeleteObject",
                 "cloudfront:CreateInvalidation"
             ],
             "Resource": [
                 "<DISTRIBUTION_ARN>",
                 "arn:aws:s3:::<BUCKET_NAME>/*",
                 "arn:aws:s3:::<BUCKET_NAME>"
             ]
         }
     ]
   }
   ```

   Caution

   Do not forget to replace `<DISTRIBUTION_ARN>` and `<BUCKET_NAME>`. You can find the DISTRIBUTION\_ARN in **CloudFront > Distributions > Details**.

2. Create a new IAM user and attach the policy to the user. This will provide your `AWS_SECRET_ACCESS_KEY` and `AWS_ACCESS_KEY_ID`.

3. Add this sample workflow to your repository at `.github/workflows/deploy.yml` and push it to GitHub. You will need to add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `BUCKET_ID`, and `DISTRIBUTION_ID` as “secrets” to your repository on GitHub under **Settings** > **Secrets** > **Actions**. Click `New repository secret` to add each one.

   ```yaml
   name: Deploy Website


   on:
     push:
       branches:
         - main


   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout
           uses: actions/checkout@v4
         - name: Configure AWS Credentials
           uses: aws-actions/configure-aws-credentials@v1
           with:
             aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
             aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
             aws-region: us-east-1
         - name: Install modules
           run: npm ci
         - name: Build application
           run: npm run build
         - name: Deploy to S3
           run: aws s3 sync --delete ./dist/ s3://${{ secrets.BUCKET_ID }}
         - name: Create CloudFront invalidation
           run: aws cloudfront create-invalidation --distribution-id ${{ secrets.DISTRIBUTION_ID }} --paths "/*"
   ```

   Note

   Your `BUCKET_ID` is the name of your S3 bucket. Your `DISTRIBUTION_ID` is your CloudFront distribution ID. You can find your CloudFront distribution ID in **CloudFront > Distributions > ID**

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Deploy Astro to AWS Amplify](https://www.launchfa.st/blog/deploy-astro-aws-amplify)
* [Deploy Astro to AWS Elastic Beanstalk](https://www.launchfa.st/blog/deploy-astro-aws-elastic-beanstalk)
* [Deploy Astro to Amazon ECS on AWS Fargate](https://www.launchfa.st/blog/deploy-astro-aws-fargate)
* [Troubleshooting SSR Amplify Deployments](https://docs.aws.amazon.com/amplify/latest/userguide/troubleshooting-ssr-deployment.html)

# Deploy your Astro Site to AWS with Flightcontrol

> How to deploy your Astro site to AWS with Flightcontrol

You can deploy an Astro site using [Flightcontrol](https://www.flightcontrol.dev?ref=astro), which provides fully-automated deployments to your AWS account.

Supports both static and SSR Astro sites.

## How to Deploy

[Section titled “How to Deploy”](#how-to-deploy)

1. Create a Flightcontrol account at [app.flightcontrol.dev/signup](https://app.flightcontrol.dev/signup?ref=astro)

2. Go to [app.flightcontrol.dev/projects/new/1](https://app.flightcontrol.dev/projects/new/1)

3. Connect your GitHub account and select your repo

4. Select your desired “Config Type”:

   * `GUI` (all config managed through Flightcontrol dashboard) where you will select the `Astro Static` or `Astro SSR` preset
   * `flightcontrol.json` (“infrastructure as code” option where all config is in your repo) where you will select an Astro example config, then add it to your codebase as `flightcontrol.json`

5. Adjust any configuration as needed

6. Click “Create Project” and complete any required steps (like linking your AWS account).

### SSR Setup

[Section titled “SSR Setup”](#ssr-setup)

To deploy with SSR support, make sure you first set up the [`@astrojs/node`](/en/guides/integrations-guide/node/) adapter. Then, follow the steps above, choosing the appropriate configurations for Astro SSR.

# Deploy your Astro Site to AWS with SST

> How to deploy your Astro site to AWS with SST

You can deploy an Astro site to AWS using [SST](https://sst.dev), an open-source framework for deploying modern full-stack applications with SSG and SSR support.

You can also use any additional SST components like cron jobs, Buckets, Queues, etc while maintaining type-safety.

## Quickstart

[Section titled “Quickstart”](#quickstart)

1. Create an astro project.

2. Run `npx sst@latest init`.

3. It should detect that you are using Astro and ask you to confirm.

4. Once you’re ready for deployment you can run `npx sst deploy --stage production`.

You can also read [the full Astro on AWS with SST tutorial](https://sst.dev/docs/start/aws/astro) that will guide you through the steps.

### SST components

[Section titled “SST components”](#sst-components)

To use any [additional SST components](https://sst.dev/docs/), add them to `sst.config.ts`.

sst.config.ts

```ts
const bucket = new sst.aws.Bucket("MyBucket", {
  access: "public",
});
new sst.aws.Astro("MyWeb", {
  link: [bucket],
});
```

And then access them in your `.astro` file.

```astro
---
import { Resource } from "sst"
console.log(Resource.MyBucket.name)
---
```

Consult the [SST docs on linking resources](https://sst.dev/docs/linking) to learn more.

If you have any questions, you can [ask in the SST Discord](https://discord.gg/sst).

# Deploy your Astro Site to Azion

> How to deploy your Astro site to the web using Azion.

You can deploy your Astro project on [Azion](https://console.azion.com/), a platform for frontend developers to collaborate and deploy static (JAMstack) and SSR websites.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

To get started, you will need:

* An [Azion account](https://www.azion.com/). If you don’t have one, you can sign up for a free account.
* Your app code stored in a [GitHub](https://github.com/) repository.
* [Azion CLI](https://www.azion.com/en/documentation/products/azion-cli/overview/) installed for faster project setup and deployment.

## How to Deploy through Azion Console Dashboard

[Section titled “How to Deploy through Azion Console Dashboard”](#how-to-deploy-through-azion-console-dashboard)

To start building, follow these steps:

1. Access [Azion Console](https://console.azion.com).

2. On the homepage, click the **+ Create** button.
   * This opens a modal with the options to create new applications and resources.

3. Select the **Import from GitHub** option and click the card.
   * This action opens the settings page.

4. Connect your Azion account with GitHub.
   * A pop-up window will appear asking for authorization.

5. Select the repository you want to import from GitHub.

6. Configure the build settings:

   * **Framework preset:** Select the appropriate framework (e.g., `Astro`).
   * **Root Directory:** This refers to the directory in which your code is located. Your code must be located at the root directory, not a subdirectory. A ./ symbol appears in this field, indicating it’s a root directory.
   * **Install Command:** the command that compiles your settings to build for production. Build commands are executed through scripts. For example: npm run build or npm install for an NPM package.

7. Click **Save and Deploy**.

8. Monitor the deployment using **Azion Real-Time Metrics** and verify your site is live on the edge.

## How to Deploy a Static Site Using the Azion CLI

[Section titled “How to Deploy a Static Site Using the Azion CLI”](#how-to-deploy-a-static-site-using-the-azion-cli)

1. **Install the Azion CLI:**

   * Download and install the [Azion CLI](https://www.azion.com/en/documentation/products/azion-cli/overview/) for easier management and deployment.

   Caution

   The Azion CLI does not currently support native Windows environments. However, you can use it on Windows through the Windows Subsystem for Linux (WSL). Follow the [WSL installation guide](https://docs.microsoft.com/en-us/windows/wsl/install) to set up a Linux environment on your Windows machine.

2. **Authenticate the CLI:**

   * Run the following command to authenticate your CLI with your Azion account.

   ```bash
   azion login
   ```

3. **Set Up Your Application:**

   * Use the following commands to initialize and configure your project:

   ```bash
   azion init
   ```

4. **Build Your Astro Project:**

   * Run your build command locally:

   ```bash
   azion build
   ```

5. **Deploy Your Static Files:**

   * Deploy your static files using the Azion CLI:

   ```bash
   azion deploy
   ```

This guide provides an overview of deploying static applications.

## Enabling Local Development Using Azion CLI

[Section titled “Enabling Local Development Using Azion CLI”](#enabling-local-development-using-azion-cli)

For the preview to work, you must execute the following command:

```bash
azion dev
```

Once you’ve initialized the local development server, the application goes through the `build` process.

```bash
Building your Edge Application. This process may take a few minutes
Running build step command:
...
```

Then, when the build is complete, the access to the application is prompted:

```bash
[Azion Bundler] [Server] › ✔  success   Function running on port http://localhost:3000
```

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

### Node.js runtime APIs

[Section titled “Node.js runtime APIs”](#nodejs-runtime-apis)

A project using an NPM package fails to build with an error message such as `[Error] Could not resolve "XXXX. The package "XXXX" wasn't found on the file system but is built into node.`:

This means that a package or import you are using is not compatible with Azion’s runtime APIs.

If you are directly importing a Node.js runtime API, please refer to the [Azion Node.js compatibility](https://www.azion.com/en/documentation/products/azion-edge-runtime/compatibility/node/) for further steps on how to resolve this.

If you are importing a package that imports a Node.js runtime API, check with the author of the package to see if they support the `node:*` import syntax. If they do not, you may need to find an alternative package.

# Deploy your Astro Site with Buddy

> How to deploy your Astro site to the web using Buddy.

You can deploy your Astro project using [Buddy](https://buddy.works/), a CI/CD solution that can build your site and push it to many different deploy targets including FTP servers and cloud hosting providers.

Note

Buddy itself will not host your site. Instead, it helps you manage the build process and deliver the result to a deploy platform of your choice.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. [Create a **Buddy** account](https://buddy.works/sign-up).

2. Create a new project and connect it with a git repository (GitHub, GitLab, BitBucket, any private Git Repository or you can use Buddy Git Hosting).

3. Add a new pipeline.

4. In the newly created pipeline add a **[Node.js](https://buddy.works/actions/node-js)** action.

5. In this action add:

   ```bash
   npm install
   npm run build
   ```

6. Add a deployment action — there are many to choose from, you can browse them in [Buddy’s actions catalog](https://buddy.works/actions). Although their settings can differ, remember to set the **Source path** to `dist`.

7. Press the **Run** button.

# Deploy your Astro Site with Cleavr

> How to deploy your Astro site to your VPS server using Cleavr.

You can deploy your Astro project to your own Virtual Private Server (VPS) using [Cleavr](https://cleavr.io/), a server and app deployment management tool.

Tip

Check out [the Astro guide in Cleavr’s docs](https://docs.cleavr.io/guides/astro)!

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

To get started, you will need:

* A Cleavr account
* A server on your VPS provider using Cleavr

## Add your site

[Section titled “Add your site”](#add-your-site)

1. In Cleavr, navigate to the server you want to add your Astro project to.

2. Select **Add Site** and fill in the details for your application, such as domain name.

3. For **App Type**, select ‘NodeJS Static’ or ‘NodeJS SSR’ according to how you are setting up your Astro app.

4. For Static apps, set **Artifact Folder** to `dist`.

5. For SSR apps:

   * Set **Entry Point** to `entry.mjs`.
   * Set **Artifact Folder** to `dist/server`.

6. Select **Add** to add the site to your server.

## Setup and deploy

[Section titled “Setup and deploy”](#setup-and-deploy)

1. Once your new site is added, click **Setup and deploy**.

2. Select the **VC Profile**, **Repo**, and **Branch** for your Astro Project.

3. Make any additional configurations necessary for your project.

4. Click on the **Deployments** tab and then click on **Deploy**.

Congratulations, you’ve just deployed your Astro app!

# Deploy your Astro Site to Clever Cloud

> How to deploy your Astro site to the web on Clever Cloud.

[Clever Cloud](https://clevercloud.com) is a European cloud platform that provides automated, scalable services.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

You can deploy both fully static and on-demand rendered Astro projects on Clever Cloud. Regardless of your `output` mode (pre-rendered or [on-demand](/en/guides/on-demand-rendering/)), you can choose to deploy as a **static application** which runs using a post-build hook, or as a **Node.js** application, which requires some manual configuration in your `package.json`.

### Scripts

[Section titled “Scripts”](#scripts)

If you’re running an on-demand Node.js application, update your `start` script to run the Node server. Applications on Clever Cloud listen on port **8080**.

package.json

```json
"scripts": {
  "start": "node ./dist/server/entry.mjs --host 0.0.0.0 --port 8080",
}
```

## Deploy Astro from the Console

[Section titled “Deploy Astro from the Console”](#deploy-astro-from-the-console)

To deploy your Astro project to Clever Cloud, you will need to **create a new application**. The application wizard will walk you through the necessary configuration steps.

1. From the lateral menubar, click **Create** > **An application**

2. Choose how to deploy:

   * **Create a brand new app**: to deploy from a local repository with Git

   or

   * **Select a GitHub repository**: to deploy from GitHub

3. Select a **Node.js** application, or a **static** one.

4. Set up the minimal size for your instance and scalability options. Astro sites can typically be deployed using the **Nano** instance. Depending on your project’s specifications and dependencies, you may need to adjust accordingly as you watch the metrics from the **Overview** page.

5. Select a **region** to deploy your instance.

6. Skip [connecting **Add-ons** to your Clever application](https://www.clevercloud.com/developers/doc/addons/) unless you’re using a database or Keycloak.

7. Inject **environment variables**:

   * For **Node.js**, set the following environment variables based on your package manager:

   - npm

     ```shell
     CC_NODE_BUILD_TOOL="npm"
     CC_PRE_BUILD_HOOK="npm install && npm run astro telemetry disable && npm run build"
     ```

   - pnpm

     ```shell
     CC_NODE_BUILD_TOOL="custom"
     CC_PRE_BUILD_HOOK="npm install -g pnpm && pnpm install"
     CC_CUSTOM_BUILD_TOOL="pnpm run astro telemetry disable && pnpm build"
     ```

   - Yarn

     ```shell
     CC_NODE_BUILD_TOOL="yarn"
     CC_PRE_BUILD_HOOK="yarn && yarn run astro telemetry disable && yarn build"
     ```

   * For a **static** application, add these variables:

   - npm

     ```shell
     CC_POST_BUILD_HOOK="npm run build"
     CC_PRE_BUILD_HOOK="npm install && npm run astro telemetry disable"
     CC_WEBROOT="/dist"
     ```

   - pnpm

     ```shell
     CC_POST_BUILD_HOOK="pnpm build"
     CC_PRE_BUILD_HOOK="npm install -g pnpm && pnpm install && pnpm run astro telemetry disable"
     CC_WEBROOT="/dist"
     ```

   - Yarn

     ```shell
     CC_POST_BUILD_HOOK="yarn build"
     CC_PRE_BUILD_HOOK="yarn && yarn run astro telemetry disable"
     CC_WEBROOT="/dist"
     ```

8. **Deploy!** If you’re deploying from **GitHub**, your deployment should start automatically. If you’re using **Git**, copy the remote and push on the **master** branch.

Other Branches

To deploy from branches other than `master`, use `git push clever <branch>:master`.

For example, if you want to deploy your local `main` branch without renaming it, use `git push clever main:master`.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Clever Cloud documentation for deploying a Node.js application](https://www.clevercloud.com/developers/doc/applications/nodejs/)
* [Clever Cloud documentation for deploying Astro as a static application](https://www.clevercloud.com/developers/guides/astro/)

# Deploy your Astro Site to Cloudflare

> How to deploy your Astro site to the web using Cloudflare

You can deploy full-stack applications, including front-end static assets and back-end APIs, as well as on-demand rendered sites, to [Cloudflare Workers](https://developers.cloudflare.com/workers/static-assets/).

Note

Cloudflare recommends using Cloudflare Workers for new projects. For existing Pages projects, refer to [Cloudflare’s migration guide](https://developers.cloudflare.com/workers/static-assets/migration-guides/migrate-from-pages/) and [compatibility matrix](https://developers.cloudflare.com/workers/static-assets/migration-guides/migrate-from-pages/#compatibility-matrix).

Read more about [using the Cloudflare runtime](/en/guides/integrations-guide/cloudflare/) in your Astro project.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

To get started, you will need:

* A Cloudflare account. If you don’t already have one, you can create a free Cloudflare account during the process.

## Cloudflare Workers

[Section titled “Cloudflare Workers”](#cloudflare-workers)

### How to deploy with Wrangler

[Section titled “How to deploy with Wrangler”](#how-to-deploy-with-wrangler)

1. Install [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/get-started/).

   ```bash
   npm install wrangler@latest --save-dev
   ```

2. If your site uses on-demand rendering, install the [`@astrojs/cloudflare` adapter](/en/guides/integrations-guide/cloudflare/).

   This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

   * npm

     ```sh
     npx astro add cloudflare
     ```

   * pnpm

     ```sh
     pnpm astro add cloudflare
     ```

   * Yarn

     ```sh
     yarn astro add cloudflare
     ```

   Read more about [on-demand rendering in Astro](/en/guides/on-demand-rendering/).

3. Create a [Wrangler configuration file](https://developers.cloudflare.com/workers/wrangler/configuration/).

   Running `astro add cloudflare` will create this for you; if you are not using the adapter, you’ll need to create it yourself.

   * Static

     wrangler.jsonc

     ```jsonc
     {
       "name": "my-astro-app",
       "compatibility_date": "YYYY-MM-DD", // Update to the day you deploy
       "assets": {
         "directory": "./dist",
       }
     }
     ```

   * On demand

     wrangler.jsonc

     ```jsonc
     {
       "main": "dist/_worker.js/index.js",
       "name": "my-astro-app",
       "compatibility_date": "YYYY-MM-DD", // Update to the day you deploy
       "compatibility_flags": [
         "nodejs_compat",
         "global_fetch_strictly_public"
       ],
       "assets": {
         "binding": "ASSETS",
         "directory": "./dist"
       },
       "observability": {
         "enabled": true
       }
     }
     ```

4. Preview your project locally with Wrangler.

   ```bash
   npx astro build && npx wrangler dev
   ```

5. Deploy using `npx wrangler deploy`.

   ```bash
   npx astro build && npx wrangler deploy
   ```

After your assets are uploaded, Wrangler will give you a preview URL to inspect your site.

Read more about using [Cloudflare runtime APIs](/en/guides/integrations-guide/cloudflare/) such as bindings.

### How to deploy with CI/CD

[Section titled “How to deploy with CI/CD”](#how-to-deploy-with-cicd)

You can also use a CI/CD system such as [Workers Builds](https://developers.cloudflare.com/workers/ci-cd/builds/) to automatically build and deploy your site on push.

If you’re using Workers Builds:

1. Follow Steps 1-3 from the Wrangler section above.

2. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/) and navigate to `Compute > Workers & Pages`. Select `Create application`.

3. Under `Import a repository`, select a Git account and then the repository containing your Astro project.

4. Configure your project with:

   * Build command: `npx astro build`
   * Deploy command: `npx wrangler deploy`

5. Click `Save and Deploy`. You can now preview your Worker at its provided `workers.dev` subdomain.

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

### 404 behavior

[Section titled “404 behavior”](#404-behavior)

For Workers projects, you will need to set `not_found_handling` if you want to serve a custom 404 page. You can read more about this in the [Routing behavior section](https://developers.cloudflare.com/workers/static-assets/#routing-behavior) of Cloudflare’s documentation.

wrangler.jsonc

```jsonc
{
  "assets": {
    "directory": "./dist",
    "not_found_handling": "404-page"
  }
}
```

### Client-side hydration

[Section titled “Client-side hydration”](#client-side-hydration)

Client-side hydration may fail as a result of Cloudflare’s Auto Minify setting. If you see `Hydration completed but contains mismatches` in the console, make sure to disable Auto Minify under Cloudflare settings.

### Node.js runtime APIs

[Section titled “Node.js runtime APIs”](#nodejs-runtime-apis)

If you are building a project that is using on-demand rendering with [the Cloudflare adapter](/en/guides/integrations-guide/cloudflare/) and the server fails to build with an error message such as `[Error] Could not resolve "XXXX. The package "XXXX" wasn't found on the file system but is built into node.`:

* This means that a package or import you are using in the server-side environment is not compatible with the [Cloudflare runtime APIs](https://developers.cloudflare.com/workers/runtime-apis/nodejs/).

* If you are directly importing a Node.js runtime API, please refer to the Astro documentation on Cloudflare’s [Node.js compatibility](/en/guides/integrations-guide/cloudflare/#nodejs-compatibility) for further steps on how to resolve this.

* If you are importing a package that imports a Node.js runtime API, check with the author of the package to see if they support the `node:*` import syntax. If they do not, you may need to find an alternative package.

# Deploy your Astro Site with CloudRay

> How to deploy your Astro site to your Ubuntu Server using CloudRay

You can deploy your Astro project using [CloudRay](https://cloudray.io), a centralized platform that helps you manage your servers, organize Bash scripts, and automate deployment tasks across virtual machines and cloud servers.

Note

CloudRay itself does not host your site. Instead, it provides automation tools to run deployment scripts on your own infrastructure (e.g., Ubuntu servers) using a connected agent.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

To get started, you will need:

* A [CloudRay Account](https://app.cloudray.io)
* Your app code stored in a [GitHub](https://github.com/) repository

## How to Deploy through CloudRay Dashboard

[Section titled “How to Deploy through CloudRay Dashboard”](#how-to-deploy-through-cloudray-dashboard)

Deploying with CloudRay typically involves three main steps:

1. Install the [CloudRay Agent](https://cloudray.io/docs/agent) on your server to securely register your machine and enable remote automation.

2. In your CloudRay Dashboard, write a reusable Bash script that clones your Astro repo, installs dependencies, builds your site, and configures a web server. Define any repo-specific values using [CloudRay’s variable groups](https://cloudray.io/docs/variable-groups).

3. Use CloudRay’s Runlog interface to execute your script on your connected server and monitor the deployment in real time.

## Official Resources

[Section titled “Official Resources”](#official-resources)

Check out [the Astro guide in CloudRay’s docs](https://cloudray.io/articles/how-to-deploy-your-astro-site).

# Deploy your Astro Site with Deno

> How to deploy your Astro site to the web using Deno.

You can deploy a static or on-demand rendered Astro site using Deno, either on your own server, or to [Deno Deploy](https://deno.com/deploy), a distributed system that runs JavaScript, TypeScript, and WebAssembly at the edge, worldwide.

This guide includes instructions for running your Astro site on your own server with Deno, and deploying to Deno Deploy through GitHub Actions or the Deno Deploy CLI.

## Requirements

[Section titled “Requirements”](#requirements)

This guide assumes you already have [Deno](https://deno.com/) installed.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

Your Astro project can be deployed as a static site, or as an on-demand rendered site.

### Static Site

[Section titled “Static Site”](#static-site)

Your Astro project is a static site by default. You don’t need any extra configuration to deploy a static Astro site with Deno, or to Deno Deploy.

### Adapter for on-demand rendering

[Section titled “Adapter for on-demand rendering”](#adapter-for-on-demand-rendering)

To enable on-demand rendering in your Astro project using Deno, and to deploy on Deno Deploy:

1. Install [the `@deno/astro-adapter` adapter](https://github.com/denoland/deno-astro-adapter) to your project’s dependencies using your preferred package manager:

   * npm

     ```shell
     npm install @deno/astro-adapter
     ```

   * pnpm

     ```shell
     pnpm install @deno/astro-adapter
     ```

   * Yarn

     ```shell
     yarn add @deno/astro-adapter
     ```

2. Update your `astro.config.mjs` project configuration file with the changes below.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import deno from '@deno/astro-adapter';


   export default defineConfig({
   +  output: 'server',
   +  adapter: deno(),
   });
   ```

3. Update your `preview` script in `package.json` with the change below.

   package.json

   ```diff
   {
     // ...
     "scripts": {
       "dev": "astro dev",
       "start": "astro dev",
       "build": "astro build",
       -"preview": "astro preview"
       +"preview": "deno run --allow-net --allow-read --allow-env ./dist/server/entry.mjs"
     }
   }
   ```

   You can now use this command to preview your production Astro site locally with Deno.

   * npm

     ```shell
     npm run preview
     ```

   * pnpm

     ```shell
     pnpm run preview
     ```

   * Yarn

     ```shell
     yarn run preview
     ```

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can run your Astro site on your own server, or deploy to Deno Deploy through GitHub Actions or using Deno Deploy’s CLI (command line interface).

### On your own server

[Section titled “On your own server”](#on-your-own-server)

1. Copy your project onto your server.

2. Install the project dependencies using your preferred package manager:

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
     yarn
     ```

3. Build your Astro site with your preferred package manager:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm run build
     ```

   * Yarn

     ```shell
     yarn run build
     ```

4. Start your application with the following command:

   * Static

     ```bash
     deno run -A jsr:@std/http/file-server dist
     ```

   * On demand

     ```bash
     deno run -A ./dist/server/entry.mjs
     ```

### GitHub Actions Deployment

[Section titled “GitHub Actions Deployment”](#github-actions-deployment)

If your project is stored on GitHub, the [Deno Deploy website](https://dash.deno.com/) will guide you through setting up GitHub Actions to deploy your Astro site.

1. Push your code to a public or private GitHub repository.

2. Sign in on [Deno Deploy](https://dash.deno.com/) with your GitHub account, and click on [New Project](https://dash.deno.com).

3. Select your repository, the branch you want to deploy from, and select **GitHub Action** mode. (Your Astro site requires a build step, and cannot use Automatic mode.)

4. In your Astro project, create a new file at `.github/workflows/deploy.yml` and paste in the YAML below. This is similar to the YAML given by Deno Deploy, with the additional steps needed for your Astro site.

   * Static

     .github/workflows/deploy.yml

     ```yaml
     name: Deploy
     on: [push]


     jobs:
       deploy:
         name: Deploy
         runs-on: ubuntu-latest
         permissions:
           id-token: write # Needed for auth with Deno Deploy
           contents: read # Needed to clone the repository


         steps:
           - name: Clone repository
             uses: actions/checkout@v4


           # Not using npm? Change `npm ci` to `yarn install` or `pnpm i`
           - name: Install dependencies
             run: npm ci


           # Not using npm? Change `npm run build` to `yarn build` or `pnpm run build`
           - name: Build Astro
             run: npm run build


           - name: Upload to Deno Deploy
             uses: denoland/deployctl@v1
             with:
               project: my-deno-project # TODO: replace with Deno Deploy project name
               entrypoint: jsr:@std/http/file-server
               root: dist
     ```

   * On demand

     .github/workflows/deploy.yml

     ```yaml
     name: Deploy
     on: [push]


     jobs:
       deploy:
         name: Deploy
         runs-on: ubuntu-latest
         permissions:
           id-token: write # Needed for auth with Deno Deploy
           contents: read # Needed to clone the repository


         steps:
           - name: Clone repository
             uses: actions/checkout@v4


           # Not using npm? Change `npm ci` to `yarn install` or `pnpm i`
           - name: Install dependencies
             run: npm ci


           # Not using npm? Change `npm run build` to `yarn build` or `pnpm run build`
           - name: Build Astro
             run: npm run build


           - name: Upload to Deno Deploy
             uses: denoland/deployctl@v1
             with:
               project: my-deno-project # TODO: replace with Deno Deploy project name
               entrypoint: dist/server/entry.mjs
     ```

5. After committing this YAML file, and pushing to GitHub on your configured deploy branch, the deploy should begin automatically!

   You can track the progress using the “Actions” tab on your GitHub repository page, or on [Deno Deploy](https://dash.deno.com).

### CLI Deployment

[Section titled “CLI Deployment”](#cli-deployment)

1. Install the [Deno Deploy CLI](https://docs.deno.com/deploy/manual/deployctl).

   ```bash
   deno install -gArf jsr:@deno/deployctl
   ```

2. Build your Astro site with your preferred package manager:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm run build
     ```

   * Yarn

     ```shell
     yarn run build
     ```

3. Run `deployctl` to deploy!

   * Static

     ```bash
     cd dist && deployctl deploy jsr:@std/http/file-server
     ```

   * On demand

     ```bash
     deployctl deploy ./dist/server/entry.mjs
     ```

   You can track all your deploys on [Deno Deploy](https://dash.deno.com).

4. (Optional) To simplify the build and deploy into one command, add a `deploy-deno` script in `package.json`.

   * Static

     package.json

     ```diff
     {
       // ...
       "scripts": {
       "dev": "astro dev",
       "start": "astro dev",
       "build": "astro build",
       "preview": "astro preview",
       +"deno-deploy": "npm run build && cd dist && deployctl deploy jsr:@std/http/file-server"
       }
     }
     ```

   * On demand

     package.json

     ```diff
     {
       // ...
       "scripts": {
         "dev": "astro dev",
         "start": "astro dev",
         "build": "astro build",
         "preview": "deno run --allow-net --allow-read --allow-env ./dist/server/entry.mjs",
         +"deno-deploy": "npm run build && deployctl deploy ./dist/server/entry.mjs"
       }
     }
     ```

   Then you can use this command to build and deploy your Astro site in one step.

   ```bash
   npm run deno-deploy
   ```

# Deploy your Astro Site with DeployHQ

> How to deploy your Astro site to the web using DeployHQ.

You can deploy your Astro project to your own servers using [DeployHQ](https://www.deployhq.com/), a deployment automation platform that builds your code and pushes it to SSH/SFTP servers, FTP servers, cloud storage (e.g. Amazon S3, Cloudflare R2), and modern hosting platforms (e.g. Netlify, Heroku).

Note

DeployHQ does not host your site. It automates building your Astro project and deploying the built files to your chosen hosting provider or server.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. If you do not already have one, sign up for a [DeployHQ account](https://www.deployhq.com/).

2. From the DeployHQ web interface, create a new project and connect the Git repository for your Astro project (GitHub, GitLab, Bitbucket, or any private repository). You will also need to authorize DeployHQ to access your repository.

3. Add a server and enter your server details:

   * Give your server a name.
   * Select your protocol (SSH/SFTP, FTP, or cloud platform).
   * Enter your server hostname, username, and password/SSH key.
   * Set **Deployment Path** to your web root (e.g. `public_html/`).

4. In your project settings, navigate to **Build Pipeline** and add your build commands:

   ```bash
   npm install
   npm run build
   ```

5. Click **Deploy Project**, then select your server and click **Deploy** to start your first deployment.

Your Astro site will be built and deployed to your server. You can enable automatic deployments to deploy on every Git push, or schedule deployments for specific times.

See [DeployHQ’s documentation](https://www.deployhq.com/support) for more info on advanced deployment features.

# Deploy your Astro Site to EdgeOne Pages

> How to deploy your Astro site to the web on EdgeOne Pages.

You can deploy your Astro project on [EdgeOne Pages](https://pages.edgeone.ai/), a fullstack development platform with a global edge network that can build your site from a Git connection or from the EdgeOne CLI.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [EdgeOne Astro Framework Guide](https://pages.edgeone.ai/document/framework-astro)
* [EdgeOne Astro template](https://github.com/TencentEdgeOne/astro-template)

# Deploy your Astro Site to Google’s Firebase Hosting

> How to deploy your Astro site to the web using Google’s Firebase Hosting.

[Firebase Hosting](https://firebase.google.com/products/hosting) is a service provided by Google’s [Firebase](https://firebase.google.com/) app development platform, which can be used to deploy an Astro site.

See our separate guide for [adding Firebase backend services](/en/guides/backend/firebase/) such as databases, authentication, and storage.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

Your Astro project can be deployed to Firebase as a static site, or as a server-side rendered site (SSR).

### Static Site

[Section titled “Static Site”](#static-site)

Your Astro project is a static site by default. You don’t need any extra configuration to deploy a static Astro site to Firebase.

### Adapter for SSR

[Section titled “Adapter for SSR”](#adapter-for-ssr)

To enable SSR in your Astro project and deploy on Firebase add the [Node.js adapter](/en/guides/integrations-guide/node/).

Note

Deploying an SSR Astro site to Firebase requires the [Blaze plan](https://firebase.google.com/pricing) or higher.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. Install the [Firebase CLI](https://github.com/firebase/firebase-tools). This is a command-line tool that allows you to interact with Firebase from the terminal.

   * npm

     ```shell
     npm install firebase-tools
     ```

   * pnpm

     ```shell
     pnpm add firebase-tools
     ```

   * Yarn

     ```shell
     yarn add firebase-tools
     ```

2. Authenticate the Firebase CLI with your Google account. This will open a browser window where you can log in to your Google account.

   * npm

     ```shell
     npx firebase login
     ```

   * pnpm

     ```shell
     pnpm exec firebase login
     ```

   * Yarn

     ```shell
     yarn firebase login
     ```

3. Enable experimental web frameworks support. This is an experimental feature that allows the Firebase CLI to detect and configure your deployment settings for Astro.

   * npm

     ```shell
     npx firebase experiments:enable webframeworks
     ```

   * pnpm

     ```shell
     pnpm exec firebase experiments:enable webframeworks
     ```

   * Yarn

     ```shell
     yarn firebase experiments:enable webframeworks
     ```

4. Initialize Firebase Hosting in your project. This will create a `firebase.json` and `.firebaserc` file in your project root.

   * npm

     ```shell
     npx firebase init hosting
     ```

   * pnpm

     ```shell
     pnpm exec firebase init hosting
     ```

   * Yarn

     ```shell
     yarn firebase init hosting
     ```

5. Deploy your site to Firebase Hosting. This will build your Astro site and deploy it to Firebase.

   * npm

     ```shell
     npx firebase deploy --only hosting
     ```

   * pnpm

     ```shell
     pnpm exec firebase deploy --only hosting
     ```

   * Yarn

     ```shell
     yarn firebase deploy --only hosting
     ```

# Deploy your Astro Site to Fleek

> How to deploy your Astro site to the web on Fleek.

You can use [Fleek](http://fleek.xyz/) to deploy a static Astro site to their edge-optimized decentralized network.

This guide gives a complete walkthrough of deploying your Astro site to Fleek using the Fleek UI and CLI.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

Your Astro project can be deployed to Fleek as a static site.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy to Fleek through the website UI or using Fleek’s CLI (command line interface).

### Platform UI Deployment

[Section titled “Platform UI Deployment”](#platform-ui-deployment)

1. Create a [Fleek](https://app.fleek.xyz) account.

2. Push your code to your online Git repository (GitHub).

3. Import your project into Fleek.

4. Fleek will automatically detect Astro and then you can configure the correct settings.

5. Your application is deployed!

### Fleek CLI

[Section titled “Fleek CLI”](#fleek-cli)

1. Install the Fleek CLI.

   ```bash
   # You need to have Nodejs >= 18.18.2
   npm install -g @fleek-platform/cli
   ```

2. Log in to your Fleek account from your terminal.

   ```bash
   fleek login
   ```

3. Run the build command to generate the static files. By default, these will be located in the `dist/` directory.

   ```bash
   npm run build
   ```

4. Initialize your project. This will generate a configuration file.

   ```bash
   fleek sites init
   ```

5. You will be prompted to either create a new Fleek Site or use an existing one. Give the site a name and select the directory where your project is located.

6. Deploy your site.

   ```bash
   fleek sites deploy
   ```

## Learn more

[Section titled “Learn more”](#learn-more)

[Deploy site from Fleek UI](https://fleek.xyz/docs/platform/deployments/)

[Deploy site from Fleek CLI](https://fleek.xyz/docs/cli/hosting/)

# Deploy your Astro Site to Fly.io

> How to deploy your Astro site to the web using Fly.io.

You can deploy your Astro project to [Fly.io](https://fly.io/), a platform for running full stack apps and databases close to your users.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

Your Astro project can be deployed to Fly.io as a static site, or as a server-side rendered site (SSR).

### Static Site

[Section titled “Static Site”](#static-site)

Your Astro project is a static site by default. You don’t need any extra configuration to deploy a static Astro site to Fly.io.

### Adapter for SSR

[Section titled “Adapter for SSR”](#adapter-for-ssr)

To enable on-demand rendering in your Astro project and deploy on Fly.io, add [the Node.js adapter](/en/guides/integrations-guide/node/).

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. [Sign up for Fly.io](https://fly.io/docs/getting-started/log-in-to-fly/#first-time-or-no-fly-account-sign-up-for-fly) if you haven’t already.

2. [Install `flyctl`](https://fly.io/docs/hands-on/install-flyctl/), your Fly.io app command center.

3. Run the following command in your terminal.

   ```bash
   fly launch
   ```

   `flyctl` will automatically detect Astro, configure the correct settings, build your image, and deploy it to the Fly.io platform.

## Generating your Astro Dockerfile

[Section titled “Generating your Astro Dockerfile”](#generating-your-astro-dockerfile)

If you don’t already have a Dockerfile, `fly launch` will generate one for you, as well as prepare a `fly.toml` file. For pages rendered on demand, this Dockerfile will include the appropriate start command and environment variables.

You can instead create your own Dockerfile using [Dockerfile generator](https://www.npmjs.com/package/@flydotio/dockerfile) and then run using the command `npx dockerfile` for Node applications or `bunx dockerfile` for Bun applications.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* Check out [the official Fly.io docs](https://fly.io/docs/js/frameworks/astro/)

# Deploy your Astro Site to GitHub Pages

> How to deploy your Astro site to the web using GitHub Pages.

You can use [GitHub Pages](https://pages.github.com/) to host a static, prerendered Astro website directly from a repository on [GitHub.com](https://github.com/) using [GitHub Actions](https://github.com/features/actions).

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

Astro maintains an [official Astro GitHub Action to deploy your project to a GitHub Pages](https://github.com/withastro/action) with very little configuration and is the recommended way to deploy to GitHub Pages.

Follow the instructions below to use the GitHub Action to deploy your Astro site to GitHub Pages. This will create a website from your repository at a GitHub URL (e.g. `https://<username>.github.io/<my-repo>`). Once deployed, you can optionally [configure a custom domain](#change-your-github-url-to-a-custom-domain) to deploy your GitHub Pages site at your preferred domain (e.g. `https://example.com`).

1. Create a new file in your project at `.github/workflows/deploy.yml` and paste in the YAML below.

   deploy.yml

   ```yaml
   name: Deploy to GitHub Pages


   on:
     # Trigger the workflow every time you push to the `main` branch
     # Using a different branch name? Replace `main` with your branch’s name
     push:
       branches: [ main ]
     # Allows you to run this workflow manually from the Actions tab on GitHub.
     workflow_dispatch:


   # Allow this job to clone the repo and create a page deployment
   permissions:
     contents: read
     pages: write
     id-token: write


   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout your repository using git
           uses: actions/checkout@v5
         - name: Install, build, and upload your site
           uses: withastro/action@v5
           # with:
             # path: . # The root location of your Astro project inside the repository. (optional)
             # node-version: 24 # The specific version of Node that should be used to build your site. Defaults to 22. (optional)
             # package-manager: pnpm@latest # The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile. (optional)
             # build-cmd: pnpm run build # The command to run to build your site. Runs the package build script/task by default. (optional)
           # env:
             # PUBLIC_POKEAPI: 'https://pokeapi.co/api/v2' # Use single quotation marks for the variable value. (optional)


     deploy:
       needs: build
       runs-on: ubuntu-latest
       environment:
         name: github-pages
         url: ${{ steps.deployment.outputs.page_url }}
       steps:
         - name: Deploy to GitHub Pages
           id: deployment
           uses: actions/deploy-pages@v4
   ```

   The Astro action can be configured with optional inputs. Provide these by uncommenting the `with:` line and the input you want to use.

   If your site requires any public environment variables, uncomment the `env:` line and add them there. (See the [GitHub documentation on setting secrets](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables#creating-configuration-variables-for-a-repository) for adding private environment variables.)

   Caution

   The official Astro [action](https://github.com/withastro/action) scans for a lockfile to detect your preferred package manager (`npm`, `yarn`, `pnpm`, or `bun`). You should commit your package manager’s automatically generated `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, or `bun.lockb` file to your repository.

2. In your Astro config file, set [`site`](/en/reference/configuration-reference/#site) to the GitHub URL of your deployed site.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config'


   export default defineConfig({
   +  site: 'https://astronaut.github.io',
   })
   ```

   The value for `site` must be one of the following:

   * The following URL based on your username: `https://<username>.github.io`
   * The random URL autogenerated for a [GitHub Organization’s private page](https://docs.github.com/en/enterprise-cloud@latest/pages/getting-started-with-github-pages/changing-the-visibility-of-your-github-pages-site): `https://<random-string>.pages.github.io/`

3. In `astro.config.mjs`, configure a value for [`base`](/en/reference/configuration-reference/#base) (usually required).

   GitHub Pages will publish your website at an address that depends on both your username and your repository name (e.g. `https://<username>/github.io/<my-repo>/`). Set a value for `base` that specifies the repository for your website. This is so that Astro understands your website’s root is `/my-repo`, rather than the default `/`. You can skip this if your repository name matches the special `<username>.github.io` pattern (e.g. `https://github.com/username/username.github.io/`)

   Configure `base` as the repository’s name starting with a forward slash ( e.g. `/my-repo`):

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config'


   export default defineConfig({
   +  site: 'https://astronaut.github.io',
   +  base: '/my-repo',
   })
   ```

   Internal links with `base` configured

   When this value is configured, all of your internal page links must be prefixed with your `base` value:

   ```astro
   <a href="/my-repo/about">About</a>
   ```

   See more about [configuring a `base` value](/en/reference/configuration-reference/#base).

4. On GitHub, go to your repository’s **Settings** tab and find the **Pages** section of the settings.

5. Choose **GitHub Actions** as the **Source** of your site.

When you push changes to your Astro project’s repository, the GitHub Action will automatically deploy them for you at your GitHub URL.

## Change your GitHub URL to a custom domain

[Section titled “Change your GitHub URL to a custom domain”](#change-your-github-url-to-a-custom-domain)

Once your Astro project is [deployed to GitHub pages at a GitHub URL](#how-to-deploy) following the previous instructions, you can configure a custom domain. This means that users can visit your site at your custom domain `https://example.com` instead of `https://<username>.github.io`.

1. [Configure DNS for your domain provider](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain).

2. Add a `./public/CNAME` record to your project.

   Create the following file in your `public/` folder with a single line of text that specifies your custom domain:

   public/CNAME

   ```js
   sub.example.com
   ```

   This will deploy your site at your custom domain instead of `user.github.io`.

3. In your Astro config, update the value for `site` with your custom domain. Do not set a value for `base`, and remove one if it exists:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config'


   export default defineConfig({
   +  site: 'https://example.com',
   -  base: '/my-repo'
   })
   ```

4. If necessary, update all your page internal links to remove the `base` prefix:

   ```astro
   <a href="/my-repo/about">About</a>
   ```

## Examples

[Section titled “Examples”](#examples)

* [Github Pages Deployment starter template](https://github.com/hkbertoson/github-pages)
* [Starlight Flexoki Theme (production site)](https://delucis.github.io/starlight-theme-flexoki/)
* [Expressive Code Color Chips (production site)](https://delucis.github.io/expressive-code-color-chips/)
* [Starlight Markdown Blocks (production site)](https://delucis.github.io/starlight-markdown-blocks/)

# Deploy your Astro Site to GitLab Pages

> How to deploy your Astro site to the web using GitLab Pages.

You can use [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/) to host an Astro site for your [GitLab](https://about.gitlab.com/) projects, groups, or user account.

Looking for an example?

Check out [the official GitLab Pages Astro example project](https://gitlab.com/pages/astro)!

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy an Astro site to GitLab Pages by using GitLab CI/CD to automatically build and deploy your site. To do this, your source code must be hosted on GitLab and you need to make the following changes to your Astro project:

1. Set up [`site`](/en/reference/configuration-reference/#site) and [`base`](/en/reference/configuration-reference/#base) options in `astro.config.mjs`.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';


   export default defineConfig({
   +  site: 'https://<username>.gitlab.io',
   +  base: '/<my-repo>',
     outDir: 'public',
     publicDir: 'static',
   });
   ```

   `site`

   The value for `site` must be one of the following:

   * The following URL based on your username: `https://<username>.gitlab.io`
   * The following URL based on your group name: `https://<groupname>.gitlab.io`
   * Your custom domain if you have it configured in your GitLab project’s settings: `https://example.com`

   For GitLab self-managed instances, replace `gitlab.io` with your instance’s Pages domain.

   `base`

   A value for `base` may be required so that Astro will treat your repository name (e.g. `/my-repo`) as the root of your website.

   Note

   Don’t set a `base` parameter if your page is served from the root folder.

   The value for `base` should be your repository’s name starting with a forward slash, for example `/my-blog`. This is so that Astro understands your website’s root is `/my-repo`, rather than the default `/`.

   Caution

   When this value is configured, all of your internal page links must be prefixed with your `base` value:

   ```astro
   <a href="/my-repo/about">About</a>
   ```

   See more about [configuring a `base` value](/en/reference/configuration-reference/#base)

2. Rename the `public/` directory to `static/`.

3. Set `outDir: 'public'` in `astro.config.mjs`. This setting instructs Astro to put the static build output in a folder called `public`, which is the folder required by GitLab Pages for exposed files.

   If you were using the [`public/` directory](/en/basics/project-structure/#public) as a source of static files in your Astro project, rename it and use that new folder name in `astro.config.mjs` for the value of `publicDir`.

   For example, here are the correct `astro.config.mjs` settings when the `public/` directory is renamed to `static/`:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';


   export default defineConfig({
   +  outDir: 'public',
   +  publicDir: 'static',
   });
   ```

4. Change the build output in `.gitignore`. In our example we need to change `dist/` to `public/`:

   .gitignore

   ```diff
   # build output
   dist/
   public/
   ```

5. Create a file called `.gitlab-ci.yml` in the root of your project with the content below. This will build and deploy your site whenever you make changes to your content:

   .gitlab-ci.yml

   ```yaml
   pages:
     # The Docker image that will be used to build your app
     image: node:lts


     before_script:
       - npm ci


     script:
       # Specify the steps involved to build your app here
       - npm run build


     artifacts:
       paths:
         # The folder that contains the built files to be published.
         # This must be called "public".
         - public


     only:
       # Trigger a new build and deploy only when there is a push to the
       # branch(es) below
       - main
   ```

6. Commit your changes and push them to GitLab.

7. On GitLab, go to your repository’s **Deploy** menu and select **Pages**. Here you will see the full URL of your GitLab Pages website. To make sure you are using the URL format `https://username.gitlab.io/my-repo`, uncheck the **Use unique domain** setting on this page.

Your site should now be published! When you push changes to your Astro project’s repository, the GitLab CI/CD pipeline will automatically deploy them for you.

# Deploy your Astro Site to Google Cloud

> How to deploy your Astro site to the web using Google Cloud.

[Google Cloud](https://cloud.google.com/) is a full-featured web app hosting platform that can be used to deploy an Astro site.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

### Cloud Storage (static only)

[Section titled “Cloud Storage (static only)”](#cloud-storage-static-only)

1. [Create a new GCP project](https://console.cloud.google.com/projectcreate), or select one you already have.

2. Create a new bucket under [Cloud Storage](https://cloud.google.com/storage).

3. Give it a name and the other required settings.

4. Upload your `dist` folder into it or upload using [Cloud Build](https://cloud.google.com/build).

5. Enable public access by adding a new permission to `allUsers` called `Storage Object Viewer`.

6. Edit the website configuration and add `ìndex.html` as the entrypoint and `404.html` as the error page.

### Cloud Run (SSR and static)

[Section titled “Cloud Run (SSR and static)”](#cloud-run-ssr-and-static)

Cloud Run is a serverless platform that allows you to run a container without having to manage any infrastructure. It can be used to deploy both static and SSR sites.

#### Prepare the Service

[Section titled “Prepare the Service”](#prepare-the-service)

1. [Create a new GCP project](https://console.cloud.google.com/projectcreate), or select one you already have.

2. Make sure the [Cloud Run API](https://console.cloud.google.com/apis/library/run.googleapis.com) is enabled.

3. Create a new service.

#### Create Dockerfile & Build the Container

[Section titled “Create Dockerfile & Build the Container”](#create-dockerfile--build-the-container)

Before you can deploy your Astro site to Cloud Run, you need to create a Dockerfile that will be used to build the container. Find more information about [how to use Docker with Astro](/en/recipes/docker/#creating-a-dockerfile) in our recipe section.

Once the Dockerfile is created, build it into an image and push it to Google Cloud. There are a few ways to accomplish this:

**Build locally using Docker**:

Use the `docker build` command to build the image, `docker tag` to give it a tag, then `docker push` to push it to a registry. In the case of Google Cloud, [`Artifact Registry`](https://cloud.google.com/artifact-registry/docs/docker/pushing-and-pulling) is the easiest option, but you can also use [Docker Hub](https://hub.docker.com/).

```bash
# build your container
docker build .


docker tag SOURCE_IMAGE HOSTNAME/PROJECT-ID/TARGET-IMAGE:TAG


# Push your image to a registry
docker push HOSTNAME/PROJECT-ID/IMAGE:TAG
```

Change the following values in the commands above to match your project:

* `SOURCE_IMAGE`: the local image name or image ID.
* `HOSTNAME`: the registry host (`gcr.io`, `eu.gcr.io`, `asia.gcr.io`, `us.gcr.io`, `docker.io`).
* `PROJECT`: your Google Cloud project ID.
* `TARGET-IMAGE`: the name for the image when it’s stored in the registry.
* `TAG` is the version associated with the image.

[Read more in the Google Cloud docs.](https://cloud.google.com/artifact-registry/docs/docker/pushing-and-pulling)

**Using another tool**:

You can use a CI/CD tool that supports Docker, like [GitHub Actions](https://github.com/marketplace/actions/push-to-gcr-github-action).

**Build using [Cloud Build](https://cloud.google.com/build)**:

Instead of building the Dockerfile locally, you can instruct Google Cloud to build the image remotely. See the [Google Cloud Build documentation here](https://cloud.google.com/build/docs/build-push-docker-image).

#### Deploying the container

[Section titled “Deploying the container”](#deploying-the-container)

Deployment can be handled manually in your terminal [using `gcloud`](https://cloud.google.com/run/docs/deploying#service) or automatically using [Cloud Build](https://cloud.google.com/build) or any other CI/CD system.

Need public access?

Don’t forget to add the permission `Cloud Run Invoker` to the `allUsers` group in the Cloud Run permissions settings!

# Deploy your Astro Site to Heroku

> How to deploy your Astro site to the web using Heroku.

[Heroku](https://www.heroku.com/) is a platform-as-a-service for building, running, and managing modern apps in the cloud. You can deploy an Astro site to Heroku using this guide.

Caution

The following instructions use [the deprecated `heroku-static-buildpack`](https://github.com/heroku/heroku-buildpack-static#warning-heroku-buildpack-static-is-deprecated). Please see [Heroku’s documentation for using `heroku-buildpack-nginx`](https://github.com/dokku/heroku-buildpack-nginx) instead.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. Install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli).

2. Create a Heroku account by [signing up](https://signup.heroku.com/).

3. Run `heroku login` and fill in your Heroku credentials:

   ```bash
   $ heroku login
   ```

4. Create a file called `static.json` in the root of your project with the below content:

   static.json

   ```json
   {
     "root": "./dist"
   }
   ```

   This is the configuration of your site; read more at [heroku-buildpack-static](https://github.com/heroku/heroku-buildpack-static).

5. Set up your Heroku git remote:

   ```bash
   # version change
   $ git init
   $ git add .
   $ git commit -m "My site ready for deployment."


   # creates a new app with a specified name
   $ heroku apps:create example


   # set buildpack for static sites
   $ heroku buildpacks:set https://github.com/heroku/heroku-buildpack-static.git
   ```

6. Deploy your site:

   ```bash
   # publish site
   $ git push heroku master


   # opens a browser to view the Dashboard version of Heroku CI
   $ heroku open
   ```

# Deploy your Astro Site to Juno

> How to deploy your Astro site to the web using Juno.

[Juno](https://juno.build) is an open-source serverless platform for hosting static websites, building web applications, and running serverless functions with the privacy and control of self-hosting.

## Create your container

[Section titled “Create your container”](#create-your-container)

1. Log in to the [Juno Console](https://console.juno.build).

2. Click the **Launch a new satellite** button (the container for your project) from the launchpad

3. Enter a **name** and select **Website**

4. Confirm with **Create a Satellite**

5. The platform will then provision its resources.

6. Once the process is complete, click Continue to access the overview page.

## Configure your project

[Section titled “Configure your project”](#configure-your-project)

Your Astro project can be deployed to Juno as a static site.

Create a `juno.config.mjs` file at the root of your project, and replace the `PROD_SATELLITE_ID` with the ID of the Satellite you created earlier.

juno.config.mjs

```javascript
import { defineConfig } from '@junobuild/config';


/** @type {import('@junobuild/config').JunoConfig} */
export default defineConfig({
  satellite: {
    ids: {
      production: '<PROD_SATELLITE_ID>'
    },
    source: 'dist',
    predeploy: ['npm run build']
  }
});
```

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy using either GitHub Actions or CLI (command line interface).

### GitHub Actions deployment

[Section titled “GitHub Actions deployment”](#github-actions-deployment)

1. From your Satellite’s overview, navigate to the **Setup** tab.

2. Click on **Add an access key**.

3. Generate a new key with the default option. Click **Submit**.

4. Upon successful creation, a **Secret token** will be displayed. Copy the value and save it as an [encrypted secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) in your GitHub repository or organization, using the key `JUNO_TOKEN`.

5. Create a `deploy.yml` file in the `.github/workflows` subfolder of your repo.

6. Add the following workflow configuration:

   .github/workflows/deploy.yml

   ```yaml
   name: Deploy to Juno


   on:
     workflow_dispatch:
     push:
       branches: [main]


   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Check out the repo
           uses: actions/checkout@v4


         - uses: actions/setup-node@v4
           with:
             node-version: 24
             registry-url: "https://registry.npmjs.org"


         - name: Install Dependencies
           run: npm ci


         - name: Deploy to Juno
           uses: junobuild/juno-action@main
           with:
             args: hosting deploy
           env:
             JUNO_TOKEN: ${{ secrets.JUNO_TOKEN }}
   ```

### CLI deployment

[Section titled “CLI deployment”](#cli-deployment)

1. Install the CLI

   * npm

     ```shell
     npm i -g @junobuild/cli
     ```

   * pnpm

     ```shell
     pnpm add -g @junobuild/cli
     ```

   * Yarn

     ```shell
     yarn global add @junobuild/cli
     ```

2. Authenticate the CLI. This will open the Juno Console.

   ```bash
   juno login
   ```

   Tip

   An access token is used to identify your terminal. That’s why the CLI asks whether you want to encrypt it with a password. For security reasons, it’s recommended that you do so.

3. In the browser window, click **Authorize** to grant permission.

4. Deploy your site:

   ```bash
   juno hosting deploy
   ```

## Guides

[Section titled “Guides”](#guides)

* [Use Juno with Astro](https://juno.build/docs/guides/astro)
* [Deployment with GitHub Actions](https://juno.build/docs/category/deployment)
* [Manual Deployment](https://juno.build/docs/guides/manual-deployment)

## Examples

[Section titled “Examples”](#examples)

Quickly scaffold a website with a ready-made Astro template.

* npm

  ```shell
  npm create juno@latest -- --template astro-starter
  ```

* pnpm

  ```shell
  pnpm create juno -- --template astro-starter
  ```

* Yarn

  ```shell
  yarn create juno -- --template astro-starter
  ```

# Deploy your Astro Site to Microsoft Azure

> How to deploy your Astro site to the web using Microsoft Azure.

[Azure](https://azure.microsoft.com/) is a cloud platform from Microsoft. You can deploy your Astro site with Microsoft Azure’s [Static Web Apps](https://aka.ms/staticwebapps) service.

This guide takes you through deploying your Astro site stored in GitHub using Visual Studio Code. Please see Microsoft guides for using an [Azure Pipelines Task](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/azure-static-web-app-v0?view=azure-pipelines) for other setups.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

To follow this guide, you will need:

* An Azure account and a subscription key. You can create a [free Azure account here](https://azure.microsoft.com/free).
* Your app code pushed to [GitHub](https://github.com/).
* The [SWA Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps) in [Visual Studio Code](https://code.visualstudio.com/).

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. Open your project in VS Code.

2. Open the Static Web Apps extension, sign in to Azure, and click the **+** button to create a new Static Web App. You will be prompted to designate which subscription key to use.

3. Follow the wizard started by the extension to give your app a name, choose a framework preset, and designate the app root (usually `/`) and built file location (use `/dist`). Astro is not listed in the built-in templates in Azure so you will need to select `custom`. The wizard will run and will create a [GitHub Action](https://github.com/features/actions) in the `.github` folder of your repo. (This folder will be automatically created if it does not already exist.)

The GitHub Action will deploy your app (you can see its progress in your repo’s Actions tab on GitHub). When successfully completed, you can view your app at the address shown in the SWA Extension’s progress window by clicking the **Browse Website** button (this will appear after the GitHub Action has run).

## Known Issues

[Section titled “Known Issues”](#known-issues)

The GitHub action yaml that is created for you assumes the use of node 14. This means the Astro build fails. To resolve this update your projects package.json file with this snippet.

```plaintext
  "engines": {
    "node": ">=22.12.0"
  },
```

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Microsoft Azure Static Web Apps documentation](https://learn.microsoft.com/en-us/azure/static-web-apps/)

## Community Resources

[Section titled “Community Resources”](#community-resources)

* [Deploying an Astro Website to Azure Static Web Apps](https://www.blueboxes.co.uk/deploying-an-astro-website-to-azure-static-web-apps)
* [Deploying a Static Astro Site to Azure Static Web Apps using GitHub Actions](https://agramont.net/blog/create-static-site-astro-azure-ssg/#automate-deployment-with-github-actions)
* [Astro site deployment to Azure Static Web Apps with the CLI from GitHub Actions](https://www.eliostruyf.com/deploy-astro-azure-static-web-apps-github-cli/)

# Deploy your Astro Site to Netlify

> How to deploy your Astro site to the web on Netlify.

[Netlify](https://netlify.com) offers hosting and serverless backend services for web applications and static websites. Any Astro site can be hosted on Netlify!

This guide includes instructions for deploying to Netlify through the website UI or Netlify’s CLI.

## Project configuration

[Section titled “Project configuration”](#project-configuration)

Your Astro project can be deployed to Netlify in three different ways: as a static site, a server-rendered site, or an edge-rendered site.

### Static site

[Section titled “Static site”](#static-site)

Your Astro project is a static site by default. You don’t need any extra configuration to deploy a static Astro site to Netlify.

### Adapter for on-demand rendering

[Section titled “Adapter for on-demand rendering”](#adapter-for-on-demand-rendering)

Add [the Netlify adapter](/en/guides/integrations-guide/netlify/) to enable on-demand rendering in your Astro project and deploy to Netlify with the following `astro add` command. This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

```bash
npx astro add netlify
```

See the [Netlify adapter guide](/en/guides/integrations-guide/netlify/) to install manually instead, or for more configuration options, such as deploying your project’s Astro middleware using Netlify’s Edge Functions.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy to Netlify through the website UI or using Netlify’s CLI (command line interface). The process is the same for both static and on-demand rendered Astro sites.

### Website UI deployment

[Section titled “Website UI deployment”](#website-ui-deployment)

If your project is stored in GitHub, GitLab, BitBucket, or Azure DevOps, you can use the Netlify website UI to deploy your Astro site.

1. Click `Add a new site` in your [Netlify dashboard](https://app.netlify.com/)

2. Choose `Import an existing project`

   When you import your Astro repository from your Git provider, Netlify should automatically detect and pre-fill the correct configuration settings for you.

3. Make sure that the following settings are entered, then press the `Deploy` button:

   * **Build Command:** `astro build` or `npm run build`
   * **Publish directory:** `dist`

   After deploying, you will be redirected to the site overview page. There, you can edit the details of your site.

Any future changes to your source repository will trigger preview and production deploys based on your deployment configuration.

#### `netlify.toml` file

[Section titled “netlify.toml file”](#netlifytoml-file)

You can optionally create a new `netlify.toml` file at the top level of your project repository to configure your build command and publish directory, as well as other project settings including environment variables and redirects. Netlify will read this file and automatically configure your deployment.

To configure the default settings, create a `netlify.toml` file with the following contents:

```toml
[build]
  command = "npm run build"
  publish = "dist"
```

More info at [“Deploying an existing Astro Git repository”](https://www.netlify.com/blog/how-to-deploy-astro/#deploy-an-existing-git-repository-to-netlify) on Netlify’s blog

### CLI deployment

[Section titled “CLI deployment”](#cli-deployment)

You can also create a new site on Netlify and link up your Git repository by installing and using the [Netlify CLI](https://cli.netlify.com/).

1. Install Netlify’s CLI globally

   ```bash
   npm install --global netlify-cli
   ```

2. Run `netlify login` and follow the instructions to log in and authorize Netlify

3. Run `netlify init` and follow the instructions

4. Confirm your build command (`astro build`)

   The CLI will automatically detect the build settings (`astro build`) and deploy directory (`dist`), and will offer to automatically generate [a `netlify.toml` file](#netlifytoml-file) with those settings.

5. Build and deploy by pushing to Git

   The CLI will add a deploy key to the repository, which means your site will be automatically rebuilt on Netlify every time you `git push`.

More details from Netlify on [Deploy an Astro site using the Netlify CLI](https://www.netlify.com/blog/how-to-deploy-astro/#link-your-astro-project-and-deploy-using-the-netlify-cli)

### Set a Node.js version

[Section titled “Set a Node.js version”](#set-a-nodejs-version)

If you are using a legacy [build image](https://docs.netlify.com/configure-builds/get-started/#build-image-selection) (Xenial) on Netlify, make sure that your Node.js version is set. Astro requires `v22.12.0` or higher.

You can [specify your Node.js version in Netlify](https://docs.netlify.com/configure-builds/manage-dependencies/#node-js-and-javascript) using:

* a [`.nvmrc`](https://github.com/nvm-sh/nvm#nvmrc) file in your base directory.
* a `NODE_VERSION` environment variable in your site’s settings using the Netlify project dashboard.

## Using Netlify Functions

[Section titled “Using Netlify Functions”](#using-netlify-functions)

No special configuration is required to use Netlify Functions with Astro. Add a `netlify/functions` directory to your project root and follow [the Netlify Functions documentation](https://docs.netlify.com/functions/overview/) to get started!

## Examples

[Section titled “Examples”](#examples)

* [Deploy An Astro site with Forms, Serverless Functions, and Redirects](https://www.netlify.com/blog/deploy-an-astro-site-with-forms-serverless-functions-and-redirects/) — Netlify Blog
* [Deployment Walkthrough Video](https://youtu.be/GrSLYq6ZTes) — Netlify YouTube channel

# Deploy your Astro Site with Railway

> How to deploy your Astro site using the Railway web interface.

[Railway](https://railway.com?utm_medium=integration\&utm_source=button\&utm_campaign=astro) is a deployment platform built to simplify your infrastructure stack from servers to observability with a unified developer experience.

This guide is for deploying an Astro static site to Railway using either the web interface or Railway CLI tool.

Tip

To deploy an Astro site with on-demand rendering (SSR) using the Node adapter, you can follow [Railway’s guide to deploying an Astro site](https://docs.railway.com/guides/astro?utm_medium=integration\&utm_source=button\&utm_campaign=astro).

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

Railway’s default build system, [Railpack](https://docs.railway.com/reference/railpack), automatically builds your Astro project as a static site.

## Deploy via a Railway template

[Section titled “Deploy via a Railway template”](#deploy-via-a-railway-template)

If you do not already have an Astro project, and are starting from scratch:

1. Go to the Astro template on Railway: [railway.com/deploy/astro-starter](https://railway.com/deploy/astro-starter?utm_medium=integration\&utm_source=docs\&utm_campaign=astro).

2. Click “Deploy Now” and sign in with your GitHub account to authorize Railway. This will deploy the Astro template into your new Railway account.

3. Eject the service code into your own Github repository by following [this guide](https://docs.railway.com/guides/deploy#eject-from-template-repository?utm_medium=integration\&utm_source=docs\&utm_campaign=astro). This will allow you to keep the repo deployed but customize it with your own code.

## Deploy via the web interface

[Section titled “Deploy via the web interface”](#deploy-via-the-web-interface)

If you have an existing Astro project you would like to deploy but not a Railway account yet:

1. Create a [Railway account](https://railway.com/dashboard) and sign in.

2. From the Railway dashboard, create a new [project](https://docs.railway.com/guides/projects).

3. Select the option to deploy from a GitHub repository, and select your Astro project.

4. Generate or add a custom domain from your project’s [network settings](https://docs.railway.com/guides/public-networking#railway-provided-domain).

## Deploy via Railway CLI

[Section titled “Deploy via Railway CLI”](#deploy-via-railway-cli)

If you have an existing Astro project you would like to deploy and an existing Railway account:

1. [Install](https://docs.railway.com/guides/cli#installing-the-cli) the Railway CLI tool.

2. Login with the command `railway login`.

3. From within your Astro project, run `railway init` and choose a workspace and project name.

4. Run `railway up` to deploy your project on Railway.

5. Run `railway domain` to generate a Railway provided service domain.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Railway guide to deploying an Astro app](https://docs.railway.com/guides/astro?utm_medium=integration\&utm_source=docs\&utm_campaign=astro)
* [Railway Astro starter template](https://railway.com/deploy/astro-starter?utm_medium=integration\&utm_source=docs\&utm_campaign=astro)

## Community Resources

[Section titled “Community Resources”](#community-resources)

[How to host an Astro site on Railway](https://jacksmith.xyz/blog/how-to-host-astro-site-on-railway)

# Deploy your Astro Site to Render

> How to deploy your Astro site to the web using Render.

You can deploy your Astro project to [Render](https://render.com/), a service to build websites with free TLS certificates, a global CDN, DDoS protection, private networks, and auto deploys from Git.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. Create a [render.com account](https://dashboard.render.com/) and sign in

2. Click the **New +** button from your dashboard and select **Static Site**

3. Connect your [GitHub](https://github.com/) or [GitLab](https://about.gitlab.com/) repository or alternatively enter the public URL of a public repository

4. Give your website a name, select the branch and specify the build command and publish directory

   * **Build Command:** `npm run build`
   * **Publish Directory:** `dist`, for static sites; `dist/client` if you have any pages rendered on demand.

5. Click the **Create Static Site** button

# Deploy your Astro Site to Seenode

> How to deploy your Astro site to the web on Seenode.

[Seenode](https://seenode.com) is a deployment platform for building and deploying web applications with databases, built-in observability, and auto-scaling. Astro sites can be deployed to Seenode using server-side rendering (SSR).

This guide includes instructions for deploying to Seenode through the web interface.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

### Adapter for SSR

[Section titled “Adapter for SSR”](#adapter-for-ssr)

To enable on-demand rendering in your Astro project and deploy to Seenode, add [the Node.js adapter](/en/guides/integrations-guide/node/) with the following `astro add` command. This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

* npm

  ```shell
  npx astro add node
  ```

* pnpm

  ```shell
  pnpm astro add node
  ```

* Yarn

  ```shell
  yarn astro add node
  ```

After installing the adapter, update your `astro.config.mjs` to configure the server for Seenode’s requirements:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';
import node from '@astrojs/node';


export default defineConfig({
  output: 'server',
+  adapter: node({
+    mode: 'standalone'
+  }),
+  server: {
+    port: process.env.NODE_ENV === 'production' ? (Number(process.env.PORT) || 80) : 4321,
+    host: true
  }
});
```

Update your `package.json` to include a start script that runs the built server:

package.json

```diff
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    +"start": "NODE_ENV=production node ./dist/server/entry.mjs"
  }
}
```

See [Seenode’s Astro deployment guide](https://seenode.com/docs/frameworks/javascript/astro/) for more configuration options and troubleshooting.

## How to Deploy

[Section titled “How to Deploy”](#how-to-deploy)

You can deploy to Seenode through the web interface by connecting your Git repository.

### Web Interface Deployment

[Section titled “Web Interface Deployment”](#web-interface-deployment)

1. Create a [Seenode account](https://cloud.seenode.com) and sign in.

2. Push your code to your Git repository (GitHub or GitLab).

3. From the [Seenode Dashboard](https://cloud.seenode.com/dashboard/applications/web/create), create a new **Web Service** and connect your repository.

4. Seenode will automatically detect your Astro project. Configure the deployment settings:

   * **Build Command:** `npm ci && npm run build` (or use `pnpm` / `yarn` equivalents)
   * **Start Command:** `npm start`
   * **Port:** `80` (required for web services)

5. Select your preferred instance size and click **Create Web Service**.

6. Your application will be built and deployed. Once complete, you’ll receive a URL to access your live Astro site after which you can link your domain.

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Seenode Cloud](https://cloud.seenode.com) — Seenode dashboard
* [Seenode Documentation](https://seenode.com/docs) — complete platform documentation
* [Seenode Astro Guide](https://seenode.com/docs/frameworks/javascript/astro/) — detailed deployment guide and troubleshooting
* [Seenode Astro Template](https://github.com/seenode/example-astro) — pre-configured starter template

# Deploy your Astro Site to Sevalla

> How to deploy your Astro site to the web using Sevalla.

[Sevalla](https://sevalla.com/) is an all-in-one hosting and management platform for static sites, applications, and databases.

This guide details how to deploy your Astro project to Sevalla.

## Prerequisites

[Section titled “Prerequisites”](#prerequisites)

* A [**Sevalla** account](https://sevalla.com/signup/).
* Your Astro project pushed to a [public or supported private Git repository](https://docs.sevalla.com/applications/git/overview) (GitHub, GitLab, or Bitbucket).

## Static Site Deployment

[Section titled “Static Site Deployment”](#static-site-deployment)

Sevalla’s Static Site Hosting deploys your site directly to a global edge network.

1. Create a new [**Static Site**](https://docs.sevalla.com/static-sites/get-started/add-a-static-site) in the Sevalla dashboard.

2. Connect your Git repository (GitHub, GitLab, or Bitbucket).

3. Select your repository and branch (e.g., `main`).

4. Configure the build settings:

   * **Build command:** `npm run build`
   * **Publish directory:** `dist`

5. Click **Create Static Site** to deploy.

Note

Astro defaults to `output: 'static'`, so no additional configuration is required for static deployment.

## SSR Deployment

[Section titled “SSR Deployment”](#ssr-deployment)

Sevalla’s Application Hosting supports full-stack applications. You can deploy Astro projects using [on-demand rendering](/en/guides/on-demand-rendering/) (server-side rendering) via the Node.js adapter.

1. Add the [`@astrojs/node` adapter](/en/guides/integrations-guide/node/) to your Astro project.

   ```bash
   npx astro add node
   ```

2. Configure the adapter in `astro.config.mjs`. Set `mode: 'standalone'` and ensure `host: true` is set so the server listens on all addresses (required for containerized environments).

   astro.config.mjs

   ```js
   import { defineConfig } from 'astro/config';
   import node from '@astrojs/node';


   export default defineConfig({
     output: 'server',
     adapter: node({
       mode: 'standalone'
     }),
     server: {
       host: true
     }
   });
   ```

3. Ensure your `package.json` has a `start` script that runs the built server:

   package.json

   ```json
   "scripts": {
     "start": "node ./dist/server/entry.mjs"
   }
   ```

4. Create a new [**Application**](https://docs.sevalla.com/applications/get-started/add-an-application) in the Sevalla dashboard.

5. Connect your Git repository.

6. Configure the build settings:

   * **Build Method:** Sevalla automatically detects Node.js projects (via Nixpacks).
   * **Build command:** `npm run build`
   * **Start command:** `npm run start`

7. Click **Create Application** to deploy.

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

### Build Failures

[Section titled “Build Failures”](#build-failures)

Check the [**Build Logs**](https://docs.sevalla.com/applications/runtime-logs) in the Sevalla dashboard for error messages. Ensure all dependencies are in `dependencies` (not `devDependencies` if needed at runtime).

### Node Version

[Section titled “Node Version”](#node-version)

Ensure the Node.js version selected in Sevalla matches your local development version (check `node -v`).

# Deploy your Astro Site to Stormkit

> Deploy your Astro site to Stormkit

You can deploy your Astro project to [Stormkit](https://stormkit.io/), a deployment platform for static websites, single-page applications (SPAs), and serverless functions.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. [Log in to Stormkit](https://app.stormkit.io/auth).

2. Using the user interface, import your Astro project from one of the three supported Git providers (GitHub, GitLab, or Bitbucket).

3. Navigate to the project’s production environment in Stormkit or create a new environment if needed.

4. Verify the build command in your [Stormkit configuration](https://stormkit.io/docs/deployments/configuration). By default, Stormkit CI will run `npm run build` but you can specify a custom build command on this page.

5. Click the “Deploy Now” button to deploy your site.

Read more in the [Stormkit Documentation](https://stormkit.io/docs).

# Deploy your Astro Site to Surge

> How to deploy your Astro site to the web using Surge

You can deploy your Astro project to [Surge](https://surge.sh/), a single-command web publishing platform designed for front-end developers.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

1. Install [the Surge CLI](https://www.npmjs.com/package/surge) globally from the terminal, if you haven’t already.

   ```shell
   npm install -g surge
   ```

2. Build your Astro site from your project’s root directory.

   ```shell
   npm run build
   ```

3. Deploy to Surge using the CLI.

   ```shell
   surge dist
   ```

   You can [use a custom domain with Surge](http://surge.sh/help/adding-a-custom-domain) when deploying by running `surge dist yourdomain.com`.

# Deploy your Astro Site to Vercel

> How to deploy your Astro site to the web on Vercel.

You can use [Vercel](http://vercel.com/) to deploy an Astro site to their global edge network with zero configuration.

This guide includes instructions for deploying to Vercel through the website UI or Vercel’s CLI.

## Project configuration

[Section titled “Project configuration”](#project-configuration)

Your Astro project can be deployed to Vercel as a static site, or a server-rendered site.

### Static site

[Section titled “Static site”](#static-site)

Your Astro project is a static site by default. You don’t need any extra configuration to deploy a static Astro site to Vercel.

### Adapter for on-demand rendering

[Section titled “Adapter for on-demand rendering”](#adapter-for-on-demand-rendering)

Add [the Vercel adapter](/en/guides/integrations-guide/vercel/) to enable [on-demand rendering](/en/guides/on-demand-rendering/) in your Astro project with the following `astro add` command. This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

* npm

  ```shell
  npx astro add vercel
  ```

* pnpm

  ```shell
  pnpm astro add vercel
  ```

* Yarn

  ```shell
  yarn astro add vercel
  ```

See the [Vercel adapter guide](/en/guides/integrations-guide/vercel/) to install manually instead, or for more configuration options, such as deploying your project’s Astro middleware using Vercel Edge Functions.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy to Vercel through the website UI or using Vercel’s CLI (command line interface). The process is the same for both static and on-demand rendered Astro sites.

### Website UI deployment

[Section titled “Website UI deployment”](#website-ui-deployment)

1. Push your code to your online Git repository (GitHub, GitLab, BitBucket).

2. [Import your project](https://vercel.com/new) into Vercel.

3. Vercel will automatically detect Astro and configure the right settings.

4. Your application is deployed! (e.g. [astro.vercel.app](https://astro.vercel.app/))

After your project has been imported and deployed, all subsequent pushes to branches will generate [Preview Deployments](https://vercel.com/docs/concepts/deployments/preview-deployments), and all changes made to the Production Branch (commonly “main”) will result in a [Production Deployment](https://vercel.com/docs/concepts/deployments/environments#production).

Learn more about Vercel’s [Git Integration](https://vercel.com/docs/concepts/git).

### CLI deployment

[Section titled “CLI deployment”](#cli-deployment)

1. Install the [Vercel CLI](https://vercel.com/cli) and run `vercel` to deploy.

   * npm

     ```shell
     npm install -g vercel
     vercel
     ```

   * pnpm

     ```shell
     pnpm add -g vercel
     vercel
     ```

   * Yarn

     ```shell
     yarn global add vercel
     vercel
     ```

2. Vercel will automatically detect Astro and configure the right settings.

3. When asked `Want to override the settings? [y/N]`, choose `N`.

4. Your application is deployed! (e.g. [astro.vercel.app](https://astro.vercel.app/))

### Project config with `vercel.json`

[Section titled “Project config with vercel.json”](#project-config-with-verceljson)

You can use `vercel.json` to override the default behavior of Vercel and to configure additional settings. For example, you may wish to attach headers to HTTP responses from your Deployments.

Learn more about [Vercel’s project configuration](https://vercel.com/docs/project-configuration).

# Deploy your Astro Site to Zeabur

> How to deploy your Astro site to the web on Zeabur.

[Zeabur](https://zeabur.com) offers hosting for full-stack web applications. Astro sites can be hosted as both SSR or static output.

This guide includes instructions for deploying to Zeabur through the website UI.

## Project Configuration

[Section titled “Project Configuration”](#project-configuration)

### Static Site

[Section titled “Static Site”](#static-site)

Astro outputs a static site by default. There is no need for any extra configuration to deploy a static Astro site to Zeabur.

### Adapter for SSR

[Section titled “Adapter for SSR”](#adapter-for-ssr)

To enable SSR in your Astro project and deploy on Zeabur:

1. Install [the `@zeabur/astro-adapter` adapter](https://www.npmjs.com/package/@zeabur/astro-adapter) to your project’s dependencies using your preferred package manager. If you’re using npm or aren’t sure, run this in the terminal:

   ```bash
     npm install @zeabur/astro-adapter
   ```

2. Add two new lines to your `astro.config.mjs` project configuration file.

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   +import zeabur from '@zeabur/astro-adapter/serverless';


   export default defineConfig({
   +  output: 'server',
   +  adapter: zeabur(),
   });
   ```

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

You can deploy your Astro site to Zeabur if the project is stored in GitHub.

1. Click `Create new project` in the [Zeabur dashboard](https://dash.zeabur.com).

2. Configure GitHub installation and import the repository.

3. Zeabur will automatically detect that your project is an Astro project and will build it using the `astro build` command.

4. Once the build is complete, you can bind a domain to your site and visit it.

After your project has been imported and deployed, all subsequent pushes to branches will generate new builds.

Learn more about Zeabur’s [Deployment Guide](https://zeabur.com/docs/get-started/).

# Deploy your Astro Site to Zephyr Cloud

> How to deploy your Astro site to the web using Zephyr Cloud.

You can use [Zephyr Cloud](https://zephyr-cloud.io) to deploy an Astro site with intelligent asset management, comprehensive build analytics, and first-class support for Module Federation architectures.

Zephyr operates on a **Bring Your Own Cloud (BYOC)** model, deploy to your choice of [supported clouds](https://docs.zephyr-cloud.io/cloud) through a unified interface without vendor lock-in. Switch providers anytime without changing your deployment workflow.

## How to deploy

[Section titled “How to deploy”](#how-to-deploy)

### Automatic Installation

[Section titled “Automatic Installation”](#automatic-installation)

1. Add the Zephyr integration to your Astro project with the following command. This will install the integration and update your `astro.config.mjs` file automatically:

   * npm

     ```shell
     npx with-zephyr@latest
     ```

   * pnpm

     ```shell
     pnpm dlx with-zephyr@latest
     ```

   * Yarn

     ```shell
     yarn dlx with-zephyr@latest
     ```

2. Build and deploy your Astro site:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm run build
     ```

   * Yarn

     ```shell
     yarn run build
     ```

3. Your application is deployed! Zephyr will provide a deployment URL and comprehensive build analytics.

### Manual Installation

[Section titled “Manual Installation”](#manual-installation)

1. Install the Zephyr Astro integration:

   * npm

     ```shell
     npm install zephyr-astro-integration
     ```

   * pnpm

     ```shell
     pnpm add zephyr-astro-integration
     ```

   * Yarn

     ```shell
     yarn add zephyr-astro-integration
     ```

2. Add the integration to your `astro.config.mjs`:

   ```js
   import { defineConfig } from 'astro/config';
   import { withZephyr } from 'zephyr-astro-integration';


   export default defineConfig({
     integrations: [
       withZephyr(),
     ],
   });
   ```

3. Build and deploy your Astro site:

   * npm

     ```shell
     npm run build
     ```

   * pnpm

     ```shell
     pnpm run build
     ```

   * Yarn

     ```shell
     yarn run build
     ```

4. Your application is deployed! Zephyr will provide a deployment URL and comprehensive build analytics.

### More details

[Section titled “More details”](#more-details)

For more detailed information refer to the [Zephyr Cloud documentation on deploying with Astro](https://docs.zephyr-cloud.io/meta-frameworks/astro).

## What happens during deployment

[Section titled “What happens during deployment”](#what-happens-during-deployment)

When you build your Astro site with the Zephyr integration, the following process occurs:

1. **Build Context Extraction**: Zephyr captures Git information (commit, branch, author) and package metadata
2. **Asset Hashing**: All build outputs are hashed using SHA-256 for content-addressable storage
3. **Delta Detection**: Zephyr queries the CDN edge to identify which assets already exist
4. **Optimized Upload**: Only new or modified assets are uploaded
5. **Snapshot Creation**: An immutable deployment snapshot is created with all asset references
6. **Analytics Upload**: Build statistics, module graphs, and dependency information are sent to the dashboard
7. **CDN Deployment**: Assets are published to your configured CDN with permanent cache headers

## Official Resources

[Section titled “Official Resources”](#official-resources)

* [Zephyr Cloud Documentation](https://docs.zephyr-cloud.io)
* [Zephyr Astro Integration on GitHub](https://github.com/ZephyrCloudIO/zephyr-packages/tree/main/libs/zephyr-astro-integration)
* [Zephyr Cloud Platform](https://zephyr-cloud.io)

# Deploy your Astro Site to Zerops

> How to deploy your Astro site to the web using Zerops.

[Zerops](https://zerops.io/) is a dev-first cloud platform that can be used to deploy both Static and SSR Astro site.

This guide will walk you through setting up and deploying both Static and SSR Astro sites on Zerops.

Astro x Zerops Quickrun

Want to test running Astro on Zerops without installing or setting up anything? Using repositories [Zerops x Astro - Static](https://github.com/zeropsio/recipe-astro-static) or [Zerops x Astro - SSR on Node.js](https://github.com/zeropsio/recipe-astro-nodejs) you can deploy example Astro site with a single click.

Running apps on Zerops requires two steps:

1. Creating a project
2. Triggering build & deploy pipeline

Note

One Zerops project can contain multiple Astro sites.

## Astro Static site on Zerops

[Section titled “Astro Static site on Zerops”](#astro-static-site-on-zerops)

### Creating a project and a service for Astro Static

[Section titled “Creating a project and a service for Astro Static”](#creating-a-project-and-a-service-for-astro-static)

Projects and services can be added either through a [`Project add`](https://app.zerops.io/dashboard/project-add) wizard or imported using a yaml structure:

```yaml
# see https://docs.zerops.io/references/import for full reference
project:
  name: recipe-astro
services:
  - hostname: app
    type: static
```

This will create a project called `recipe-astro` with a Zerops Static service called `app`.

### Deploying your Astro Static site

[Section titled “Deploying your Astro Static site”](#deploying-your-astro-static-site)

To tell Zerops how to build and run your site, add a `zerops.yml` to your repository:

* npm

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - npm i
          - npm build
        deployFiles:
          - dist/~
      run:
        base: static
  ```

* pnpm

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - pnpm i
          - pnpm build
        deployFiles:
          - dist/~
      run:
        base: static
  ```

* Yarn

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - yarn
          - yarn build
        deployFiles:
          - dist/~
      run:
        base: static
  ```

Now you can [trigger the build & deploy pipeline using the Zerops CLI](#trigger-the-pipeline-using-zerops-cli-zcli) or by connecting the `app` service with your [GitHub](https://docs.zerops.io/references/github-integration/) / [GitLab](https://docs.zerops.io/references/gitlab-integration) repository from inside the service detail.

## Astro SSR site on Zerops

[Section titled “Astro SSR site on Zerops”](#astro-ssr-site-on-zerops)

### Update scripts

[Section titled “Update scripts”](#update-scripts)

Update your `start` script to run the server output from the Node adapter.

package.json

```json
"scripts": {
  "start": "node ./dist/server/entry.mjs",
}
```

### Creating a project and a service for Astro SSR (Node.js)

[Section titled “Creating a project and a service for Astro SSR (Node.js)”](#creating-a-project-and-a-service-for-astro-ssr-nodejs)

Projects and services can be added either through a [`Project add`](https://app.zerops.io/dashboard/project-add) wizard or imported using a yaml structure:

```yaml
# see https://docs.zerops.io/references/import for full reference
project:
  name: recipe-astro
services:
  - hostname: app
    type: nodejs@20
```

This will create a project called `recipe-astro` with Zerops Node.js service called `app`.

### Deploying your Astro SSR site

[Section titled “Deploying your Astro SSR site”](#deploying-your-astro-ssr-site)

To tell Zerops how to build and run your site using the official [Astro Node.js adapter](/en/guides/integrations-guide/node/) in `standalone` mode, add a `zerops.yml` file to your repository:

* npm

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - npm i
          - npm run build
        deployFiles:
          - dist
          - package.json
          - node_modules
      run:
        base: nodejs@20
        ports:
          - port: 3000
            httpSupport: true
        envVariables:
          PORT: 3000
          HOST: 0.0.0.0
        start: npm start
  ```

* pnpm

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - pnpm i
          - pnpm run build
        deployFiles:
          - dist
          - package.json
          - node_modules
      run:
        base: nodejs@20
        ports:
          - port: 3000
            httpSupport: true
        envVariables:
          PORT: 3000
          HOST: 0.0.0.0
        start: pnpm start
  ```

* Yarn

  zerops.yml

  ```yaml
  # see https://docs.zerops.io/zerops-yml/specification for full reference
  zerops:
    - setup: app
      build:
        base: nodejs@20
        buildCommands:
          - yarn
          - yarn build
        deployFiles:
          - dist
          - package.json
          - node_modules
      run:
        base: nodejs@20
        ports:
          - port: 3000
            httpSupport: true
        envVariables:
          PORT: 3000
          HOST: 0.0.0.0
        start: yarn start
  ```

Now you can [trigger the build & deploy pipeline using the Zerops CLI](#trigger-the-pipeline-using-zerops-cli-zcli) or by connecting the `app` service with your [GitHub](https://docs.zerops.io/references/github-integration/) / [GitLab](https://docs.zerops.io/references/gitlab-integration) repository from inside the service detail.

## Trigger the pipeline using Zerops CLI (zcli)

[Section titled “Trigger the pipeline using Zerops CLI (zcli)”](#trigger-the-pipeline-using-zerops-cli-zcli)

1. Install the Zerops CLI.

   ```shell
   # To download the zcli binary directly,
   # use https://github.com/zeropsio/zcli/releases
   npm i -g @zerops/zcli
   ```

2. Open [`Settings > Access Token Management`](https://app.zerops.io/settings/token-management) in the Zerops app and generate a new access token.

3. Log in using your access token with the following command:

   ```shell
   zcli login <token>
   ```

4. Navigate to the root of your app (where `zerops.yml` is located) and run the following command to trigger the deploy:

   ```shell
   zcli push
   ```

## Resources

[Section titled “Resources”](#resources)

### Official

[Section titled “Official”](#official)

* [Create Zerops account](https://app.zerops.io/registration)
* [Zerops Documentation](https://docs.zerops.io)
* [Zerops Astro recipe](https://app.zerops.io/recipe/astro)

### Community

[Section titled “Community”](#community)

* [Deploying Astro to Zerops in 3 mins](https://medium.com/@arjunaditya/how-to-deploy-astro-to-zerops-4230816a62b4)
* [Deploying Astro SSG with Node.js on Zerops with One Click Deploy](https://youtu.be/-4KTa4VWtBE)
* [Deploying Astro SSR with Node.js on Zerops with One Click Deploy](https://youtu.be/eR6b_JnDH6g)

