---
name: astro-ssr
description: Astro server-side rendering and dynamic features - Actions, on-demand rendering (SSR), middleware, server islands, sessions, API endpoints, environment variables, internationalization (i18n) routing, prefetching, and view transitions. Load this when building dynamic or server-rendered Astro sites.
---

# Astro SSR & Dynamic Features Documentation

> This skill covers Astro's server-side rendering capabilities and dynamic features.


# Actions

> Learn how to create type-safe server functions you can call from anywhere.

**Added in:** `astro@4.15`

Astro Actions allow you to define and call backend functions with type-safety. Actions perform data fetching, JSON parsing, and input validation for you. This can greatly reduce the amount of boilerplate needed compared to using an [API endpoint](/en/guides/endpoints/).

Use actions instead of API endpoints for seamless communication between your client and server code and to:

* Automatically validate JSON and form data inputs using [Zod validation](/en/reference/modules/astro-zod/).
* Generate type-safe functions to call your backend from the client and even [from HTML form actions](#call-actions-from-an-html-form-action). No need for manual `fetch()` calls.
* Standardize backend errors with the [`ActionError`](/en/reference/modules/astro-actions/#actionerror) object.

## Basic usage

[Section titled “Basic usage”](#basic-usage)

Actions are defined in a `server` object exported from `src/actions/index.ts`:

src/actions/index.ts

```ts
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  myAction: defineAction({ /* ... */ })
}
```

Your actions are available as functions from the `astro:actions` module. Import `actions` and call them client-side within a [UI framework component](/en/guides/framework-components/), [a form POST request](#call-actions-from-an-html-form-action), or by using a `<script>` tag in an Astro component.

When you call an action, it returns an object with either `data` containing the JSON-serialized result, or `error` containing thrown errors.

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

### Write your first action

[Section titled “Write your first action”](#write-your-first-action)

Follow these steps to define an action and call it in a `script` tag in your Astro page.

1. Create a `src/actions/index.ts` file and export a `server` object.

   src/actions/index.ts

   ```ts
   export const server = {
     // action declarations
   }
   ```

2. Import the `defineAction()` utility from `astro:actions`, and the `z` object from `astro/zod`.

   src/actions/index.ts

   ```diff
   +import { defineAction } from 'astro:actions';
   +import { z } from 'astro/zod';


   export const server = {
     // action declarations
   }
   ```

3. Use the `defineAction()` utility to define a `getGreeting` action. The `input` property will be used to validate input parameters with a [Zod schema](/en/reference/modules/astro-zod/#common-data-type-validators) and the `handler()` function includes the backend logic to run on the server.

   src/actions/index.ts

   ```diff
   import { defineAction } from 'astro:actions';
   import { z } from 'astro/zod';


   export const server = {
     getGreeting: defineAction({
       input: z.object({
         name: z.string(),
       }),
       +handler: async (input) => {
         return `Hello, ${input.name}!`
       }
     })
   }
   ```

4. Create an Astro component with a button that will fetch a greeting using your `getGreeting` action when clicked.

   src/pages/index.astro

   ```astro
   ---
   ---


   <button>Get greeting</button>


   <script>
   const button = document.querySelector('button');
   button?.addEventListener('click', async () => {
     // Show alert pop-up with greeting from action
   });
   </script>
   ```

5. To use your action, import `actions` from `astro:actions` and then call `actions.getGreeting()` in the click handler. The `name` option will be sent to your action’s `handler()` on the server and, if there are no errors, the result will be available as the `data` property.

   src/pages/index.astro

   ```diff
   ---
   ---


   <button>Get greeting</button>


   <script>
   +import { actions } from 'astro:actions';


   const button = document.querySelector('button');
   button?.addEventListener('click', async () => {
     // Show alert pop-up with greeting from action
     +const { data, error } = await actions.getGreeting({ name: "Houston" });
     +if (!error) alert(data);
   })
   </script>
   ```

See the full Actions API documentation for details on [`defineAction()`](/en/reference/modules/astro-actions/#defineaction) and its properties.

## Organizing actions

[Section titled “Organizing actions”](#organizing-actions)

All actions in your project must be exported from the `server` object in the `src/actions/index.ts` file. You can define actions inline or you can move action definitions to separate files and import them. You can even group related functions in nested objects.

For example, to colocate all of your user actions, you can create a `src/actions/user.ts` file and nest the definitions of both `getUser` and `createUser` inside a single `user` object.

src/actions/user.ts

```ts
import { defineAction } from 'astro:actions';


export const user = {
  getUser: defineAction(/* ... */),
  createUser: defineAction(/* ... */),
}
```

Then, you can import this `user` object into your `src/actions/index.ts` file and add it as a top-level key to the `server` object alongside any other actions:

src/actions/index.ts

```diff
+import { user } from './user';


export const server = {
  myAction: defineAction({ /* ... */ }),
  +user,
}
```

Now, all of your user actions are callable from the `actions.user` object:

* `actions.user.getUser()`
* `actions.user.createUser()`

## Handling returned data

[Section titled “Handling returned data”](#handling-returned-data)

Actions return an object containing either `data` with the type-safe return value of your `handler()`, or an `error` with any backend errors. Errors may come from validation errors on the `input` property or thrown errors within the `handler()`.

Actions return a custom data format that can handle Dates, Maps, Sets, and URLs [using the Devalue library](https://github.com/Rich-Harris/devalue). Therefore, you can’t easily inspect the response from the network like you can with regular JSON. For debugging, you can instead inspect the `data` object returned by actions.

[See the `handler()` API reference](/en/reference/modules/astro-actions/#handler-property) for full details.

### Checking for errors

[Section titled “Checking for errors”](#checking-for-errors)

It’s best to check if an `error` is present before using the `data` property. This allows you to handle errors in advance and ensures `data` is defined without an `undefined` check.

```ts
const { data, error } = await actions.example();


if (error) {
  // handle error cases
  return;
}
// use `data`
```

### Accessing `data` directly without an error check

[Section titled “Accessing data directly without an error check”](#accessing-data-directly-without-an-error-check)

To skip error handling, for example while prototyping or using a library that will catch errors for you, use the `.orThrow()` property on your action call to throw errors instead of returning an `error`. This will return the action’s `data` directly.

This example calls a `likePost()` action that returns the updated number of likes as a `number` from the action `handler`:

```ts
const updatedLikes = await actions.likePost.orThrow({ postId: 'example' });
//    ^ type: number
```

### Handling backend errors in your action

[Section titled “Handling backend errors in your action”](#handling-backend-errors-in-your-action)

You can use the provided `ActionError` to throw an error from your action `handler()`, such as “not found” when a database entry is missing, or “unauthorized” when a user is not logged in. This has two main benefits over returning `undefined`:

* You can set a status code like `404 - Not found` or `401 - Unauthorized`. This improves debugging errors in both development and in production by letting you see the status code of each request.

* In your application code, all errors are passed to the `error` object on an action result. This avoids the need for `undefined` checks on data, and allows you to display targeted feedback to the user depending on what went wrong.

#### Creating an `ActionError`

[Section titled “Creating an ActionError”](#creating-an-actionerror)

To throw an error, import the [`ActionError()` class](/en/reference/modules/astro-actions/#actionerror) from the `astro:actions` module. Pass it a human-readable status `code` (e.g. `"NOT_FOUND"` or `"BAD_REQUEST"`), and an optional `message` to provide further information about the error.

This example throws an error from a `likePost` action when a user is not logged in, after checking a hypothetical “user-session” cookie for authentication:

src/actions/index.ts

```diff
import { defineAction, ActionError } from "astro:actions";
import { z } from "astro/zod";


export const server = {
  likePost: defineAction({
    input: z.object({ postId: z.string() }),
    handler: async (input, ctx) => {
      if (!ctx.cookies.has('user-session')) {
        throw new ActionError({
          code: "UNAUTHORIZED",
          message: "User must be logged in.",
        });
      }
      // Otherwise, like the post
    },
  }),
};
```

#### Handling an `ActionError`

[Section titled “Handling an ActionError”](#handling-an-actionerror)

To handle this error, you can call the action from your application and check whether an `error` property is present. This property will be of type `ActionError` and will contain your `code` and `message`.

In the following example, a `LikeButton.tsx` component calls the `likePost()` action when clicked. If an authentication error occurs, the `error.code` attribute is used to determine whether to display a login link:

src/components/LikeButton.tsx

```tsx
import { actions } from 'astro:actions';
import { useState } from 'preact/hooks';


export function LikeButton({ postId }: { postId: string }) {
  const [showLogin, setShowLogin] = useState(false);
  return (
    <>
      {
        showLogin && <a href="/signin">Log in to like a post.</a>
      }
      <button onClick={async () => {
        const { data, error } = await actions.likePost({ postId });
        if (error?.code === 'UNAUTHORIZED') setShowLogin(true);
        // Early return for unexpected errors
        else if (error) return;
        // update likes
      }}>
        Like
      </button>
    </>
  )
}
```

### Handling client redirects

[Section titled “Handling client redirects”](#handling-client-redirects)

When calling actions from the client, you can integrate with a client-side library like `react-router`, or you can use Astro’s [`navigate()` function](/en/guides/view-transitions/#trigger-navigation) to redirect to a new page when an action succeeds.

This example navigates to the homepage after a `logout` action returns successfully:

src/pages/LogoutButton.tsx

```tsx
import { actions } from 'astro:actions';
import { navigate } from 'astro:transitions/client';


export function LogoutButton() {
  return (
    <button onClick={async () => {
      const { error } = await actions.logout();
      if (!error) navigate('/');
    }}>
      Logout
    </button>
  );
}
```

## Accepting form data from an action

[Section titled “Accepting form data from an action”](#accepting-form-data-from-an-action)

Actions accept JSON data by default. To accept form data from an HTML form, set `accept: 'form'` in your `defineAction()` call:

src/actions/index.ts

```diff
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  comment: defineAction({
    accept: 'form',
    input: z.object(/* ... */),
    handler: async (input) => { /* ... */ },
  })
}
```

### Using validators with form inputs

[Section titled “Using validators with form inputs”](#using-validators-with-form-inputs)

When your action is [configured to accept form data](/en/reference/modules/astro-actions/#accept-property), you can use any Zod validators to validate your fields (e.g. `z.coerce.date()` for date inputs). Extension functions including `.refine()`, `.transform()`, and `.pipe()` are also supported on the `z.object()` validator.

Additionally, Astro provides special handling under the hood for your convenience to validate the following types of field inputs:

* Inputs of type `number` can be validated using `z.number()`
* Inputs of type `checkbox` can be validated using `z.coerce.boolean()`
* Inputs of type `file` can be validated using `z.instanceof(File)`
* Multiple inputs of the same `name` can be validated using `z.array(/* validator */)`
* All other inputs can be validated using `z.string()`

When your form is submitted with empty inputs, the output type may not match your `input` validator. Empty values are converted to `null` except when validating arrays or booleans. For example, if an input of type `text` is submitted with an empty value, the result will be `null` instead of an empty string (`""`).

To apply a union of different validators, use the `z.discriminatedUnion()` wrapper to narrow the type based on a specific form field. This example accepts a form submission to either “create” or “update” a user, using the form field with the name `type` to determine which object to validate against:

src/actions/index.ts

```ts
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  changeUser: defineAction({
    accept: 'form',
    input: z.discriminatedUnion('type', [
      z.object({
        // Matches when the `type` field has the value `create`
        type: z.literal('create'),
        name: z.string(),
        email: z.email(),
      }),
      z.object({
        // Matches when the `type` field has the value `update`
        type: z.literal('update'),
        id: z.number(),
        name: z.string(),
        email: z.email(),
      }),
    ]),
    async handler(input) {
      if (input.type === 'create') {
        // input is { type: 'create', name: string, email: string }
      } else {
        // input is { type: 'update', id: number, name: string, email: string }
      }
    },
  }),
};
```

### Validating form data

[Section titled “Validating form data”](#validating-form-data)

Actions will parse submitted form data to an object, using the value of each input’s `name` attribute as the object keys. For example, a form containing `<input name="search">` will be parsed to an object like `{ search: 'user input' }`. Your action’s `input` schema will be used to validate this object.

To receive the raw `FormData` object in your action handler instead of a parsed object, omit the `input` property in your action definition.

The following example shows a validated newsletter registration form that accepts a user’s email and requires a “terms of service” agreement checkbox.

1. Create an HTML form component with unique `name` attributes on each input:

   src/components/Newsletter.astro

   ```astro
   <form>
     <label for="email">E-mail</label>
     <input id="email" required type="email" name="email" />
     <label>
       <input required type="checkbox" name="terms">
       I agree to the terms of service
     </label>
     <button>Sign up</button>
   </form>
   ```

2. Define a `newsletter` action to handle the submitted form. Validate the `email` field using the `z.email()` validator, and the `terms` checkbox using `z.boolean()`:

   src/actions/index.ts

   ```diff
   import { defineAction } from 'astro:actions';
   import { z } from 'astro/zod';


   export const server = {
     newsletter: defineAction({
       accept: 'form',
       input: z.object({
         email: z.email(),
         terms: z.boolean(),
       }),
       +handler: async ({ email, terms }) => { /* ... */ },
     })
   }
   ```

   See the [`input` API reference](/en/reference/modules/astro-actions/#input-validator) for all available form validators.

3. Add a `<script>` to the HTML form to submit the user input. This example overrides the form’s default submit behavior to call `actions.newsletter()`, and redirects to `/confirmation` using the `navigate()` function:

   src/components/Newsletter.astro

   ```diff
   <form>
   7 collapsed lines
     <label for="email">E-mail</label>
     <input id="email" required type="email" name="email" />
     <label>
       <input required type="checkbox" name="terms">
       I agree to the terms of service
     </label>
     <button>Sign up</button>
   </form>


   <script>
     +import { actions } from 'astro:actions';
     +import { navigate } from 'astro:transitions/client';


     +const form = document.querySelector('form');
     +form?.addEventListener('submit', async (event) => {
       +event.preventDefault();
       +const formData = new FormData(form);
       +const { error } = await actions.newsletter(formData);
       +if (!error) navigate('/confirmation');
   +  })
   </script>
   ```

   See [“Call actions from an HTML form action”](#call-actions-from-an-html-form-action) for an alternative way to submit form data.

### Displaying form input errors

[Section titled “Displaying form input errors”](#displaying-form-input-errors)

You can validate form inputs before submission using [native HTML form validation attributes](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#using_built-in_form_validation) like `required`, `type="email"`, and `pattern`. For more complex `input` validation on the backend, you can use the provided [`isInputError()`](/en/reference/modules/astro-actions/#isinputerror) utility function.

To retrieve input errors, use the `isInputError()` utility to check whether an error was caused by invalid input. Input errors contain a `fields` object with messages for each input name that failed to validate. You can use these messages to prompt your user to correct their submission.

The following example checks the error with `isInputError()`, then checks whether the error is in the email field, before finally creating a message from the errors. You can use JavaScript DOM manipulation or your preferred UI framework to display this message to users.

```js
import { actions, isInputError } from 'astro:actions';


const form = document.querySelector('form');
const formData = new FormData(form);
const { error } = await actions.newsletter(formData);
if (isInputError(error)) {
  // Handle input errors.
  if (error.fields.email) {
    const message = error.fields.email.join(', ');
  }
}
```

## Call actions from an HTML form action

[Section titled “Call actions from an HTML form action”](#call-actions-from-an-html-form-action)

Note

Pages must be on-demand rendered when calling actions using a form action. [Ensure prerendering is disabled on the page](/en/guides/on-demand-rendering/#enabling-on-demand-rendering) before using this API.

You can enable zero-JS form submissions with standard attributes on any `<form>` element. Form submissions without client-side JavaScript may be useful both as a fallback for when JavaScript fails to load, or if you prefer to handle forms entirely from the server.

Calling [Astro.getActionResult()](/en/reference/api-reference/#getactionresult) on the server returns the result of your form submission (`data` or `error`), and can be used to dynamically redirect, handle form errors, update the UI, and more.

To call an action from an HTML form, add `method="POST"` to your `<form>`, then set the form’s `action` attribute using your action, for example `action={actions.logout}`. This will set the `action` attribute to use a query string that is handled by the server automatically.

For example, this Astro component calls the `logout` action when the button is clicked and reloads the current page:

src/components/LogoutButton.astro

```astro
---
import { actions } from 'astro:actions';
---


<form method="POST" action={actions.logout}>
  <button>Log out</button>
</form>
```

Additional attributes on the `<form>` element may be necessary for proper schema validation with Zod. For example, to include file uploads, add `enctype="multipart/form-data"` to ensure that files are sent in a format correctly recognized by `z.instanceof(File)`:

src/components/FileUploadForm.astro

```astro
---
import { actions } from 'astro:actions';
---
<form method="POST" action={actions.upload} enctype="multipart/form-data" >
  <label for="file">Upload File</label>
  <input type="file" id="file" name="file" />
  <button type="submit">Submit</button>
</form>
```

### Redirect on action success

[Section titled “Redirect on action success”](#redirect-on-action-success)

If you need to redirect to a new route on success, you can use an action’s result on the server. A common example is creating a product record and redirecting to the new product’s page, e.g. `/products/[id]`.

For example, say you have a `createProduct` action that returns the generated product id:

src/actions/index.ts

```ts
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  createProduct: defineAction({
    accept: 'form',
    input: z.object({ /* ... */ }),
    handler: async (input) => {
      const product = await persistToDatabase(input);
      return { id: product.id };
    },
  })
}
```

You can retrieve the action result from your Astro component by calling `Astro.getActionResult()`. This returns an object containing `data` or `error` properties when an action is called, or `undefined` if the action was not called during this request.

Use the `data` property to construct a URL to use with `Astro.redirect()`:

src/pages/products/create.astro

```astro
---
import { actions } from 'astro:actions';


const result = Astro.getActionResult(actions.createProduct);
if (result && !result.error) {
  return Astro.redirect(`/products/${result.data.id}`);
}
---


<form method="POST" action={actions.createProduct}>
  <!--...-->
</form>
```

### Handle form action errors

[Section titled “Handle form action errors”](#handle-form-action-errors)

Calling `Astro.getActionResult()` in the Astro component containing your form gives you access to the `data` and `error` objects for custom error handling.

The following example displays a general failure message when a `newsletter` action fails:

src/pages/index.astro

```astro
---
import { actions } from 'astro:actions';


const result = Astro.getActionResult(actions.newsletter);
---


{result?.error && (
  <p class="error">Unable to sign up. Please try again later.</p>
)}
<form method="POST" action={actions.newsletter}>
  <label>
    E-mail
    <input required type="email" name="email" />
  </label>
  <button>Sign up</button>
</form>
```

For more customization, you can [use the `isInputError()` utility](#displaying-form-input-errors) to check whether an error is caused by invalid input.

The following example renders an error banner under the `email` input field when an invalid email is submitted:

src/pages/index.astro

```diff
---
import { actions, isInputError } from 'astro:actions';


const result = Astro.getActionResult(actions.newsletter);
+const inputErrors = isInputError(result?.error) ? result.error.fields : {};
---


<form method="POST" action={actions.newsletter}>
  <label>
    E-mail
    <input required type="email" name="email" aria-describedby="error" />
  </label>
  +{inputErrors.email && <p id="error">{inputErrors.email.join(',')}</p>}
  <button>Sign up</button>
</form>
```

#### Preserve input values on error

[Section titled “Preserve input values on error”](#preserve-input-values-on-error)

Inputs will be cleared whenever a form is submitted. To persist input values, you can [enable view transitions](/en/guides/view-transitions/#enabling-view-transitions-spa-mode) and apply the `transition:persist` directive to each input:

```astro
<input transition:persist required type="email" name="email" />
```

### Update the UI with a form action result

[Section titled “Update the UI with a form action result”](#update-the-ui-with-a-form-action-result)

To use an action’s return value to display a notification to the user on success, pass the action to `Astro.getActionResult()`. Use the returned `data` property to render the UI you want to display.

This example uses the `productName` property returned by an `addToCart` action to show a success message.

src/pages/products/\[slug].astro

```astro
---
import { actions } from 'astro:actions';


const result = Astro.getActionResult(actions.addToCart);
---


{result && !result.error && (
  <p class="success">Added {result.data.productName} to cart</p>
)}


<!--...-->
```

### Advanced: Persist action results with a session

[Section titled “Advanced: Persist action results with a session”](#advanced-persist-action-results-with-a-session)

**Added in:** `astro@5.0.0`

Action results are displayed as a POST submission. This means that the result will be reset to `undefined` when a user closes and revisits the page. The user will also see a “confirm form resubmission?” dialog if they attempt to refresh the page.

To customize this behavior, you can add middleware to handle the result of the action manually. You may choose to persist the action result using a cookie or session storage.

Start by [creating a middleware file](/en/guides/middleware/) and importing [the `getActionContext()` utility](/en/reference/modules/astro-actions/#getactioncontext) from `astro:actions`. This function returns an `action` object with information about the incoming action request, including the action handler and whether the action was called from an HTML form. `getActionContext()` also returns the `setActionResult()` and `serializeActionResult()` functions to programmatically set the value returned by `Astro.getActionResult()`:

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

A common practice to persist HTML form results is the [POST / Redirect / GET pattern](https://en.wikipedia.org/wiki/Post/Redirect/Get). This redirect removes the “confirm form resubmission?” dialog when the page is refreshed, and allows action results to be persisted throughout the user’s session.

This example applies the POST / Redirect / GET pattern to all form submissions using session storage with the [Netlify server adapter](/en/guides/integrations-guide/netlify/) installed. Action results are written to a session store using [Netlify Blob](https://docs.netlify.com/blobs/overview/), and retrieved after a redirect using a session ID:

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';
import { randomUUID } from "node:crypto";
import { getStore } from "@netlify/blobs";


export const onRequest = defineMiddleware(async (context, next) => {
  // Skip requests for prerendered pages
  if (context.isPrerendered) return next();


  const { action, setActionResult, serializeActionResult } =
    getActionContext(context);
  // Create a Blob store to persist action results with Netlify Blob
  const actionStore = getStore("action-session");


  // If an action result was forwarded as a cookie, set the result
  // to be accessible from `Astro.getActionResult()`
  const sessionId = context.cookies.get("action-session-id")?.value;
  const session = sessionId
    ? await actionStore.get(sessionId, {
        type: "json",
      })
    : undefined;


  if (session) {
    setActionResult(session.actionName, session.actionResult);


    // Optional: delete the session after the page is rendered.
    // Feel free to implement your own persistence strategy
    await actionStore.delete(sessionId);
    context.cookies.delete("action-session-id");
    return next();
  }


  // If an action was called from an HTML form action,
  // call the action handler and redirect to the destination page
  if (action?.calledFrom === "form") {
    const actionResult = await action.handler();


    // Persist the action result using session storage
    const sessionId = randomUUID();
    await actionStore.setJSON(sessionId, {
      actionName: action.name,
      actionResult: serializeActionResult(actionResult),
    });


    // Pass the session ID as a cookie
    // to be retrieved after redirecting to the page
    context.cookies.set("action-session-id", sessionId);


    // Redirect back to the previous page on error
    if (actionResult.error) {
      const referer = context.request.headers.get("Referer");
      if (!referer) {
        throw new Error(
          "Internal: Referer unexpectedly missing from Action POST request.",
        );
      }
      return context.redirect(referer);
    }
    // Redirect to the destination page on success
    return context.redirect(context.originPathname);
  }


  return next();
});
```

## Security when using actions

[Section titled “Security when using actions”](#security-when-using-actions)

Actions are accessible as public endpoints based on the name of the action. For example, the action `blog.like()` will be accessible from `/_actions/blog.like`. This is useful for unit testing action results and debugging production errors. However, this means you **must** use same authorization checks that you would consider for API endpoints and on-demand rendered pages.

### Authorize users from an action handler

[Section titled “Authorize users from an action handler”](#authorize-users-from-an-action-handler)

To authorize action requests, add an authentication check to your action handler. You may want to use [an authentication library](/en/guides/authentication/) to handle session management and user information.

Actions expose [a subset of the `APIContext` object](/en/reference/modules/astro-actions/#actionapicontext) to access properties passed from middleware using `context.locals`. When a user is not authorized, you can raise an `ActionError` with the `UNAUTHORIZED` code:

src/actions/index.ts

```ts
import { defineAction, ActionError } from 'astro:actions';


export const server = {
  getUserSettings: defineAction({
    handler: async (_input, context) => {
      if (!context.locals.user) {
        throw new ActionError({ code: 'UNAUTHORIZED' });
      }
      return { /* data on success */ };
    }
  })
}
```

### Gate actions from middleware

[Section titled “Gate actions from middleware”](#gate-actions-from-middleware)

**Added in:** `astro@5.0.0`

Astro recommends authorizing user sessions from your action handler to respect permission levels and rate-limiting on a per-action basis. However, you can also gate requests to all actions (or a subset of actions) from middleware.

Use the [`getActionContext()` function](/en/reference/modules/astro-actions/#getactioncontext) from your middleware to retrieve information about any inbound action requests. This includes the action name and whether that action was called using a client-side remote procedure call (RPC) function (e.g. `actions.blog.like()`) or an HTML form.

The following example rejects all action requests that do not have a valid session token. If the check fails, a “Forbidden” response is returned. Note: this method ensures that actions are only accessible when a session is present, but is *not* a substitute for secure authorization.

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';
import { getActionContext } from 'astro:actions';


export const onRequest = defineMiddleware(async (context, next) => {
  const { action } = getActionContext(context);
  // Check if the action was called from a client-side function
  if (action?.calledFrom === 'rpc') {
    // If so, check for a user session token
    if (!context.cookies.has('user-session')) {
      return new Response('Forbidden', { status: 403 });
    }
  }


  context.cookies.set('user-session', /* session token */);
  return next();
});
```

## Call actions from Astro components and server endpoints

[Section titled “Call actions from Astro components and server endpoints”](#call-actions-from-astro-components-and-server-endpoints)

You can call actions directly from Astro component scripts using the `Astro.callAction()` wrapper (or `context.callAction()` when using a [server endpoint](/en/guides/endpoints/#server-endpoints-api-routes)). This is common to reuse logic from your actions in other server code.

Pass the action as the first argument and any input parameters as the second argument. This returns the same `data` and `error` objects you receive when calling actions on the client:

src/pages/products.astro

```astro
---
import { actions } from 'astro:actions';


const searchQuery = Astro.url.searchParams.get('search');
if (searchQuery) {
  const { data, error } = await Astro.callAction(actions.findProduct, { query: searchQuery });
  // handle result
}
---
```


---

# Endpoints

> Learn how to create endpoints that serve any kind of data

Astro lets you create custom endpoints to serve any kind of data. You can use this to generate images, expose an RSS document, or use them as API Routes to build a full API for your site.

In statically-generated sites, your custom endpoints are called at build time to produce static files. If you opt in to [SSR](/en/guides/on-demand-rendering/) mode, custom endpoints turn into live server endpoints that are called on request. Static and SSR endpoints are defined similarly, but SSR endpoints support additional features.

## Static File Endpoints

[Section titled “Static File Endpoints”](#static-file-endpoints)

To create a custom endpoint, add a `.js` or `.ts` file to the `/pages` directory. The `.js` or `.ts` extension will be removed during the build process, so the name of the file should include the extension of the data you want to create. For example, `src/pages/data.json.ts` will build a `/data.json` endpoint.

Endpoints export a `GET` function (optionally `async`) that receives a [context object](/en/reference/api-reference/) with properties similar to the `Astro` global. Here, it returns a [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) object with a `name` and `url`, and Astro will call this at build time and use the contents of the body to generate the file.

src/pages/builtwith.json.ts

```ts
// Outputs: /builtwith.json
export function GET({ params, request }) {
  return new Response(
    JSON.stringify({
      name: "Astro",
      url: "https://astro.build/",
    }),
  );
}
```

Since Astro v3.0, the returned `Response` object doesn’t have to include the `encoding` property anymore. For example, to produce a binary `.png` image:

src/pages/astro-logo.png.ts

```ts
export async function GET({ params, request }) {
  const response = await fetch(
    "https://docs.astro.build/assets/full-logo-light.png",
  );


  return new Response(await response.arrayBuffer());
}
```

You can also get type safety in your endpoint functions using the `APIRoute` type with the `satisfies` operator:

```ts
import type { APIRoute } from "astro";


export const GET = (async ({ params, request }) => { /* ... */ }) satisfies APIRoute;
```

Note that endpoints whose URLs include a file extension (e.g. `src/pages/sitemap.xml.ts`) can only be accessed without a trailing slash (e.g. `/sitemap.xml`), regardless of your [`build.trailingSlash`](/en/reference/configuration-reference/#trailingslash) configuration.

### `params` and Dynamic routing

[Section titled “params and Dynamic routing”](#params-and-dynamic-routing)

Endpoints support the same [dynamic routing](/en/guides/routing/#dynamic-routes) features that pages do. Name your file with a bracketed parameter name and export a [`getStaticPaths()` function](/en/reference/routing-reference/#getstaticpaths). Then, you can access the parameter using the `params` property passed to the endpoint function:

src/pages/api/\[id].json.ts

```ts
import type { APIRoute } from "astro";


const usernames = ["Sarah", "Chris", "Yan", "Elian"];


export const GET = (({ params, request }) => {
  const id = params.id;


  return new Response(
    JSON.stringify({
      name: usernames[id],
    }),
  );
}) satisfies APIRoute;


export function getStaticPaths() {
  return [
    { params: { id: "0" } },
    { params: { id: "1" } },
    { params: { id: "2" } },
    { params: { id: "3" } },
  ];
}
```

This will generate four JSON endpoints at build time: `/api/0.json`, `/api/1.json`, `/api/2.json` and `/api/3.json`. Dynamic routing with endpoints works the same as it does with pages. In static mode, you can [pass props to the endpoint using `getStaticPaths()`](/en/reference/routing-reference/#data-passing-with-props). However, with on-demand rendering, since the endpoint is a function and not a component, passing props is not supported.

### `request`

[Section titled “request”](#request)

All endpoints receive a `request` property, but in static mode, you only have access to `request.url`. This returns the full URL of the current endpoint and works the same as [Astro.request.url](/en/reference/api-reference/#request) does for pages.

src/pages/request-path.json.ts

```ts
import type { APIRoute } from "astro";


export const GET = (({ params, request }) => {
  return new Response(
    JSON.stringify({
      path: new URL(request.url).pathname,
    }),
  );
}) satisfies APIRoute;
```

## Server Endpoints (API Routes)

[Section titled “Server Endpoints (API Routes)”](#server-endpoints-api-routes)

Everything described in the static file endpoints section can also be used in SSR mode: files can export a `GET` function which receives a [context object](/en/reference/api-reference/) with properties similar to the `Astro` global.

But, unlike in `static` mode, when you enable on-demand rendering for a route, the endpoint will be built when it is requested. This unlocks new features that are unavailable at build time, and allows you to build API routes that listen for requests and securely execute code on the server at runtime.

Your routes will be rendered on demand by default in `server` mode. In `static` mode, you must opt out of prerendering for each custom endpoint with `export const prerender = false`.

![](/houston_chef.webp) **Related recipe:** [Call endpoints from the server](/en/recipes/call-endpoints/)

Note

Be sure to [enable an on-demand rendering mode](/en/guides/on-demand-rendering/) before trying these examples, and opt out of prerendering in `static` mode.

Server endpoints can access `params` without exporting `getStaticPaths`, and they can return a `Response` object, allowing you to set status codes and headers:

src/pages/\[id].json.js

```js
import { getProduct } from "../db";


export async function GET({ params }) {
  const id = params.id;
  const product = await getProduct(id);


  if (!product) {
    return new Response(null, {
      status: 404,
      statusText: "Not found",
    });
  }


  return new Response(JSON.stringify(product), {
    status: 200,
    headers: {
      "Content-Type": "application/json",
    },
  });
}
```

This will respond to any request that matches the dynamic route. For example, if we navigate to `/helmet.json`, `params.id` will be set to `helmet`. If `helmet` exists in the mock product database, the endpoint will use a `Response` object to respond with JSON and return a successful [HTTP status code](https://developer.mozilla.org/en-US/docs/Web/API/Response/status). If not, it will use a `Response` object to respond with a `404`.

In SSR mode, certain providers require the `Content-Type` header to return an image. In this case, use a `Response` object to specify a `headers` property. For example, to produce a binary `.png` image:

src/pages/astro-logo.png.ts

```ts
export async function GET({ params, request }) {
  const response = await fetch(
    "https://docs.astro.build/assets/full-logo-light.png",
  );
  const buffer = Buffer.from(await response.arrayBuffer());


  return new Response(buffer, {
    headers: { "Content-Type": "image/png" },
  });
}
```

### HTTP methods

[Section titled “HTTP methods”](#http-methods)

In addition to the `GET` function, you can export a function with the name of any [HTTP method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). When a request comes in, Astro will check the method and call the corresponding function.

You can also export an `ALL` function to match any method that doesn’t have a corresponding exported function. If there is a request with no matching method, it will redirect to your site’s [404 page](/en/basics/astro-pages/#custom-404-error-page).

src/pages/methods.json.ts

```ts
export const GET = (({ params, request }) => {
  return new Response(
    JSON.stringify({
      message: "This was a GET!",
    }),
  );
}) satisfies APIRoute;


export const POST = (({ request }) => {
  return new Response(
    JSON.stringify({
      message: "This was a POST!",
    }),
  );
}) satisfies APIRoute;


export const DELETE = (({ request }) => {
  return new Response(
    JSON.stringify({
      message: "This was a DELETE!",
    }),
  );
}) satisfies APIRoute;


export const ALL = (({ request }) => {
  return new Response(
    JSON.stringify({
      message: `This was a ${request.method}!`,
    }),
  );
}) satisfies APIRoute;
```

If you define a `GET` function but no `HEAD` function, Astro will automatically handle `HEAD` requests by calling the `GET` function and stripping the body from the response.

![](/houston_chef.webp) **Related recipes**

* [Verify a Captcha](/en/recipes/captcha/)
* [Build forms with API routes](/en/recipes/build-forms-api/)

### `request`

[Section titled “request”](#request-1)

In SSR mode, the `request` property returns a fully usable [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) object that refers to the current request. This allows you to accept data and check headers:

src/pages/test-post.json.ts

```ts
export const POST = (async ({ request }) => {
  if (request.headers.get("Content-Type") === "application/json") {
    const body = await request.json();
    const name = body.name;


    return new Response(
      JSON.stringify({
        message: "Your name was: " + name,
      }),
      {
        status: 200,
      },
    );
  }


  return new Response(null, { status: 400 });
}) satisfies APIRoute;
```

### Redirects

[Section titled “Redirects”](#redirects)

The endpoint context exports a `redirect()` utility similar to `Astro.redirect`:

src/pages/links/\[id].js

```js
import { getLinkUrl } from "../db";


export async function GET({ params, redirect }) {
  const { id } = params;
  const link = await getLinkUrl(id);


  if (!link) {
    return new Response(null, {
      status: 404,
      statusText: "Not found",
    });
  }


  return redirect(link, 307);
}
```


---

# Using environment variables

> Learn how to use environment variables in an Astro project.

Astro gives you access to [Vite’s built-in environment variables support](#vites-built-in-support) and includes some [default environment variables for your project](#default-environment-variables) that allow you to access configuration values for your current project (e.g. `site`, `base`), whether your project is running in development or production, and more.

Astro also provides a way to [use and organize your environment variables with type safety](#type-safe-environment-variables). It is available for use inside the Astro context (e.g. Astro components, routes and endpoints, UI framework components, middleware), and managed with [a schema in your Astro configuration](/en/reference/configuration-reference/#env).

## Vite’s built-in support

[Section titled “Vite’s built-in support”](#vites-built-in-support)

Astro uses Vite’s built-in support for environment variables, which are statically replaced at build time, and lets you [use any of its methods](https://vite.dev/guide/env-and-mode.html) to work with them.

Note that while *all* environment variables are available in server-side code, only environment variables prefixed with `PUBLIC_` are available in client-side code for security purposes.

.env

```ini
SECRET_PASSWORD=password123
PUBLIC_ANYBODY=there
```

In this example, `PUBLIC_ANYBODY` (accessible via `import.meta.env.PUBLIC_ANYBODY`) will be available in server or client code, while `SECRET_PASSWORD` (accessible via `import.meta.env.SECRET_PASSWORD`) will be server-side only.

Caution

`.env` files are not loaded inside [configuration files](#in-the-astro-config-file).

### IntelliSense for TypeScript

[Section titled “IntelliSense for TypeScript”](#intellisense-for-typescript)

By default, Astro provides a type definition for `import.meta.env` in `astro/client.d.ts`.

While you can define more custom env variables in `.env.[mode]` files, you may want to get TypeScript IntelliSense for user-defined env variables which are prefixed with `PUBLIC_`.

To achieve this, you can create an `env.d.ts` in `src/` to [extend the global types](/en/guides/typescript/#extending-global-types) and configure `ImportMetaEnv` like this:

src/env.d.ts

```ts
interface ImportMetaEnv {
  readonly DB_PASSWORD: string;
  readonly PUBLIC_POKEAPI: string;
  // more env variables...
}


interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

## Default environment variables

[Section titled “Default environment variables”](#default-environment-variables)

Astro includes a few environment variables out of the box:

* `import.meta.env.MODE`: The mode your site is running in. This is `development` when running `astro dev` and `production` when running `astro build`.
* `import.meta.env.PROD`: `true` if your site is running in production; `false` otherwise.
* `import.meta.env.DEV`: `true` if your site is running in development; `false` otherwise. Always the opposite of `import.meta.env.PROD`.
* `import.meta.env.BASE_URL`: The base URL your site is being served from. This is determined by the [`base` config option](/en/reference/configuration-reference/#base).
* `import.meta.env.SITE`: This is set to [the `site` option](/en/reference/configuration-reference/#site) specified in your project’s `astro.config`.

Use them like any other environment variable.

```ts
const isProd = import.meta.env.PROD;
const isDev = import.meta.env.DEV;
```

## Setting environment variables

[Section titled “Setting environment variables”](#setting-environment-variables)

### `.env` files

[Section titled “.env files”](#env-files)

Environment variables can be loaded from `.env` files in your project directory.

Just create a `.env` file in the project directory and add some variables to it.

.env

```ini
# This will only be available when run on the server!
DB_PASSWORD="foobar"


# This will be available everywhere!
PUBLIC_POKEAPI="https://pokeapi.co/api/v2"
```

You can also add `.production`, `.development` or a custom mode name to the filename itself (e.g `.env.testing`, `.env.staging`). This allows you to use different sets of environment variables at different times.

The `astro dev` and `astro build` commands default to `"development"` and `"production"` modes, respectively. You can run these commands with the [`--mode` flag](/en/reference/cli-reference/#--mode-string) to pass a different value for `mode` and load the matching `.env` file.

This allows you to run the dev server or build your site connecting to different APIs:

* npm

  ```shell
  # Run the dev server connected to a "staging" API
  npm run astro dev -- --mode staging


  # Build a site that connects to a "production" API with additional debug information
  npm run astro build -- --devOutput


  # Build a site that connects to a "testing" API
  npm run astro build -- --mode testing
  ```

* pnpm

  ```shell
  # Run the dev server connected to a "staging" API
  pnpm astro dev --mode staging


  # Build a site that connects to a "production" API with additional debug information
  pnpm astro build --devOutput


  # Build a site that connects to a "testing" API
  pnpm astro build --mode testing
  ```

* Yarn

  ```shell
  # Run the dev server connected to a "staging" API
  yarn astro dev --mode staging


  # Build a site that connects to a "production" API with additional debug information
  yarn astro build --devOutput


  # Build a site that connects to a "testing" API
  yarn astro build --mode testing
  ```

For more on `.env` files, [see the Vite documentation](https://vite.dev/guide/env-and-mode.html#env-files).

### In the Astro config file

[Section titled “In the Astro config file”](#in-the-astro-config-file)

Astro evaluates configuration files before it loads your other files. This means that you cannot use `import.meta.env` in `astro.config.mjs` to access environment variables that were set in `.env` files.

You can use `process.env` in a configuration file to access other environment variables, like those [set by the CLI](#using-the-cli).

You can also use [Vite’s `loadEnv` helper](https://main.vite.dev/config/#using-environment-variables-in-config) to manually load `.env` files.

astro.config.mjs

```js
import { loadEnv } from "vite";


const { SECRET_PASSWORD } = loadEnv(process.env.NODE_ENV, process.cwd(), "");
```

Note

`pnpm` does not allow you to import modules that are not directly installed in your project. If you are using `pnpm`, you will need to install `vite` to use the `loadEnv` helper.

```sh
pnpm add -D vite
```

### Using the CLI

[Section titled “Using the CLI”](#using-the-cli)

You can also add environment variables as you run your project:

* npm

  ```shell
  PUBLIC_POKEAPI=https://pokeapi.co/api/v2 npm run dev
  ```

* pnpm

  ```shell
  PUBLIC_POKEAPI=https://pokeapi.co/api/v2 pnpm run dev
  ```

* Yarn

  ```shell
  PUBLIC_POKEAPI=https://pokeapi.co/api/v2 yarn run dev
  ```

## Getting environment variables

[Section titled “Getting environment variables”](#getting-environment-variables)

Environment variables in Astro are accessed with `import.meta.env`, using the [`import.meta` feature added in ES2020](https://tc39.es/ecma262/2020/#prod-ImportMeta), instead of `process.env`.

For example, use `import.meta.env.PUBLIC_POKEAPI` to get the `PUBLIC_POKEAPI` environment variable.

```js
// When import.meta.env.SSR === true
const data = await db(import.meta.env.DB_PASSWORD);


// When import.meta.env.SSR === false
const data = fetch(`${import.meta.env.PUBLIC_POKEAPI}/pokemon/squirtle`);
```

When using SSR, environment variables can be accessed at runtime based on the SSR adapter being used. With most adapters you can access environment variables with `process.env`, but some adapters work differently. For the Deno adapter, you will use `Deno.env.get()`. See how to [access the Cloudflare runtime](/en/guides/integrations-guide/cloudflare/#cloudflare-runtime) to handle environment variables when using the Cloudflare adapter. Astro will first check the server environment for variables, and if they don’t exist, Astro will look for them in `.env` files.

## Type safe environment variables

[Section titled “Type safe environment variables”](#type-safe-environment-variables)

The `astro:env` API lets you configure a type-safe schema for [environment variables you have set](#setting-environment-variables). This allows you to indicate whether they should be available on the server or the client, and define their data type and additional properties.

Developing an adapter? See how to [make an adapter compatible with `astro:env`](/en/reference/adapter-reference/#envgetsecret).

### Basic Usage

[Section titled “Basic Usage”](#basic-usage)

#### Define your schema

[Section titled “Define your schema”](#define-your-schema)

To configure a schema, add the `env.schema` option to your Astro config:

astro.config.mjs

```diff
import { defineConfig } from "astro/config";


export default defineConfig({
+  env: {
+    schema: {
      +// ...
+    }
+  }
})
```

You can then [register variables as a string, number, enum, or boolean](#data-types) using the `envField` helper. Define the [kind of environment variable](#variable-types) by providing a `context` (`"client"` or `"server"`) and `access` (`"secret"` or `"public"`) for each variable, and pass any additional properties such as `optional` or `default` in an object:

astro.config.mjs

```js
import { defineConfig, envField } from "astro/config";


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

Types will be generated for you when running `astro dev` or `astro build`, but you can run `astro sync` to generate types only.

#### Use variables from your schema

[Section titled “Use variables from your schema”](#use-variables-from-your-schema)

Import and use your defined variables from the appropriate `/client` or `/server` module:

```astro
---
import { API_URL } from "astro:env/client";
import { API_SECRET_TOKEN } from "astro:env/server";


const data = await fetch(`${API_URL}/users`, {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${API_SECRET_TOKEN}`
  },
})
---


<script>
  import { API_URL } from "astro:env/client";


  fetch(`${API_URL}/ping`)
</script>
```

### Variable types

[Section titled “Variable types”](#variable-types)

There are three kinds of environment variables, determined by the combination of `context` (`"client"` or `"server"`) and `access` (`"secret"` or `"public"`) settings defined in your schema:

* **Public client variables**: These variables end up in both your final client and server bundles, and can be accessed from both client and server through the `astro:env/client` module:

  ```js
  import { API_URL } from "astro:env/client";
  ```

* **Public server variables**: These variables end up in your final server bundle and can be accessed on the server through the `astro:env/server` module:

  ```js
  import { PORT } from "astro:env/server";
  ```

* **Secret server variables**: These variables are not part of your final bundle and can be accessed on the server through the `astro:env/server` module:

  ```js
  import { API_SECRET } from "astro:env/server";
  ```

  By default, all secrets are validated whenever anything is imported from the `astro:env/server` module. This means, secrets may be validated even when they are not imported. You may need to [pass dummy environment variables](#setting-environment-variables) to satisfy this validation during the build.

  You can also enable validating secrets on start by [configuring `validateSecrets: true`](/en/reference/configuration-reference/#envvalidatesecrets).

Note

**Secret client variables** are not supported because there is no safe way to send this data to the client. Therefore, it is not possible to configure both `context: "client"` and `access: "secret"` in your schema.

### Data types

[Section titled “Data types”](#data-types)

There are currently four data types supported: strings, numbers, enums, and booleans:

```js
import { envField } from "astro/config";


envField.string({
   // context & access
   optional: true,
   default: "foo",
})


envField.number({
   // context & access
   optional: true,
   default: 15,
})


envField.boolean({
   // context & access
   optional: true,
   default: true,
})


envField.enum({
   // context & access
   values: ["foo", "bar", "baz"],
   optional: true,
   default: "baz",
})
```

For a complete list of validation fields, see the [`envField` API reference](/en/reference/modules/astro-config/#envfield).

### Retrieving secrets dynamically

[Section titled “Retrieving secrets dynamically”](#retrieving-secrets-dynamically)

Despite defining your schema, you may want to retrieve the raw value of a given secret or to retrieve secrets not defined in your schema. In this case, you can use `getSecret()` exported from `astro:env/server`:

```js
import {
   FOO, // boolean
   getSecret
} from "astro:env/server";


getSecret("FOO"); // string | undefined
```

Learn more in [the API reference](/en/reference/modules/astro-env/#getsecret).

### Limitations

[Section titled “Limitations”](#limitations)

`astro:env` is a virtual module which means it can only be used inside the Astro context. For example, you can use it in:

* Middlewares
* Astro routes and endpoints
* Astro components
* Framework components
* Modules

You cannot use it in the following and will have to resort to `process.env`:

* `astro.config.mjs`
* Scripts


---

# Internationalization (i18n) Routing

> Learn how to use Astro’s i18n routing features to localize your site’s pages.

Astro’s internationalization (i18n) features allow you to adapt your project for an international audience. This routing API helps you generate, use, and verify the URLs that your multi-language site produces.

Astro’s i18n routing allows you to bring your multilingual content with support for configuring a default language, computing relative page URLs, and accepting preferred languages provided by your visitor’s browser. You can also specify fallback languages on a per-language basis so that your visitors can always be directed to existing content on your site.

## Routing Logic

[Section titled “Routing Logic”](#routing-logic)

Astro uses a [middleware](/en/guides/middleware/) to implement its routing logic. This middleware function is placed in the [first position](/en/guides/middleware/#chaining-middleware) where it awaits every `Response` coming from any additional middleware and each page route before finally executing its own logic.

This means that operations (e.g. redirects) from your own middleware and your page logic are run first, your routes are rendered, and then the i18n middleware performs its own actions such as verifying that a localized URL corresponds to a valid route.

You can also choose to [add your own i18n logic in addition to or instead of Astro’s i18n middleware](#manual), giving you even more control over your routes while still having access to the `astro:i18n` helper functions.

## Configure i18n routing

[Section titled “Configure i18n routing”](#configure-i18n-routing)

Both a list of all supported languages ([`locales`](/en/reference/configuration-reference/#i18nlocales)) and a default language ([`defaultLocale`](/en/reference/configuration-reference/#i18ndefaultlocale)), which must be one of the languages listed in `locales`, need to be specified in an `i18n` configuration object. Additionally, you can configure more specific routing and fallback behavior to match your desired URLs.

astro.config.mjs

```js
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
    locales: ["es", "en", "pt-br"],
    defaultLocale: "en",
  }
})
```

### Create localized folders

[Section titled “Create localized folders”](#create-localized-folders)

Organize your content folders with localized content by language. Create individual `/[locale]/` folders anywhere within `src/pages/` and Astro’s [file-based routing](/en/guides/routing/) will create your pages at corresponding URL paths.

Your folder names must match the items in `locales` exactly. Include a localized folder for your `defaultLocale` only if you configure `prefixDefaultLocale: true` to show a localized URL path for your default language (e.g. `/en/about/`).

* src

  * pages

    * about.astro

    * index.astro

    * es

      * about.astro
      * index.astro

    * pt-br

      * about.astro
      * index.astro

Note

The localized folders do not need to be at the root of the `/pages/` folder.

### Create links

[Section titled “Create links”](#create-links)

With i18n routing configured, you can now compute links to pages within your site using the helper functions such as [`getRelativeLocaleUrl()`](/en/reference/modules/astro-i18n/#getrelativelocaleurl) available from the [`astro:i18n` module](/en/reference/modules/astro-i18n/). These generated links will always provide the correct, localized route and can help you correctly use, or check, URLs on your site.

You can also still write the links manually.

src/pages/es/index.astro

```astro
---
import { getRelativeLocaleUrl } from 'astro:i18n';


// defaultLocale is "es"
const aboutURL = getRelativeLocaleUrl("es", "about");
---


<a href="/get-started/">¡Vamos!</a>
<a href={getRelativeLocaleUrl('es', 'blog')}>Blog</a>
<a href={aboutURL}>Acerca</a>
```

## `routing`

[Section titled “routing”](#routing)

Astro’s built-in file-based routing automatically creates URL routes for you based on your file structure within `src/pages/`.

When you configure i18n routing, information about this file structure (and the corresponding URL paths generated) is available to the i18n helper functions so they can generate, use, and verify the routes in your project. Many of these options can be used together for even more customization and per-language flexibility.

You can even choose to [implement your own routing logic manually](#manual) for even greater control.

### `prefixDefaultLocale`

[Section titled “prefixDefaultLocale”](#prefixdefaultlocale)

**Added in:** `astro@3.5.0`

This routing option defines whether or not your default language’s URLs should use a language prefix (e.g. `/en/about/`).

All non-default supported languages **will** use a localized prefix (e.g. `/fr/` or `/french/`) and content files must be located in appropriate folders. This configuration option allows you to specify whether your default language should also follow a localized URL structure.

This setting also determines where the page files for your default language must exist (e.g. `src/pages/about/` or `src/pages/en/about`) as the file structure and URL structure must match for all languages.

* `"prefixDefaultLocale: false"` (default): URLs in your default language will **not** have a `/[locale]/` prefix. All other locales will.

* `"prefixDefaultLocale: true"`: All URLs, including your default language, will have a `/[locale]/` prefix.

#### `prefixDefaultLocale: false`

[Section titled “prefixDefaultLocale: false”](#prefixdefaultlocale-false)

astro.config.mjs

```diff
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
    locales: ["es", "en", "fr"],
    defaultLocale: "en",
    routing: {
+        prefixDefaultLocale: false
    }
  }
})
```

This is the **default** value. Set this option when URLs in your default language will **not** have a `/[locale]/` prefix and files in your default language exist at the root of `src/pages/`:

* src

  * pages

    * about.astro

    * index.astro

    * es

      * about.astro
      * index.astro

    * fr

      * about.astro
      * index.astro

- `src/pages/about.astro` will produce the route `example.com/about/`
- `src/pages/fr/about.astro` will produce the route `example.com/fr/about/`

#### `prefixDefaultLocale: true`

[Section titled “prefixDefaultLocale: true”](#prefixdefaultlocale-true)

astro.config.mjs

```diff
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
    locales: ["es", "en", "fr"],
    defaultLocale: "en",
    routing: {
+        prefixDefaultLocale: true
    }
  }
})
```

Set this option when all routes will have their `/locale/` prefix in their URL and when all page content files, including those for your `defaultLocale`, exist in a localized folder:

* src

  * pages

    * **index.astro** // Note: this file is always required

    * en

      * index.astro
      * about.astro

    * es

      * about.astro
      * index.astro

    * pt-br

      * about.astro
      * index.astro

- URLs without a locale prefix, (e.g. `example.com/about/`) will return a 404 (not found) status code unless you specify a [fallback strategy](#fallback).

#### Opting out of redirects for the home URL

[Section titled “Opting out of redirects for the home URL”](#opting-out-of-redirects-for-the-home-url)

Even with your default locale routes prefixed, this behaviour does not apply by default to your site’s index page. This allows you to have a home page that exists outside of your configured locale structure, where all of your localized routes are prefixed except the home URL of your site.

You can opt out of this behavior so that your main site URL will also redirect to a prefixed, localized route for your default locale. When `prefixDefaultLocale: true` is set, you can additionally configure `redirectToDefaultLocale: true`. This will ensure that the home URL (`/`) generated by `src/pages/index.astro` will redirect to `/[defaultLocale]/`.

### `manual`

[Section titled “manual”](#manual)

**Added in:** `astro@4.6.0`

When this option is enabled, Astro will **disable** its i18n middleware so that you can implement your own custom logic. No other `routing` options (e.g. `prefixDefaultLocale`) may be configured with `routing: "manual"`.

You will be responsible for writing your own routing logic, or [executing Astro’s i18n middleware manually](#middleware-function) alongside your own.

astro.config.mjs

```js
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
    locales: ["es", "en", "fr"],
    defaultLocale: "en",
    routing: "manual"
  }
})
```

Astro provides helper functions for your middleware so you can control your own default routing, exceptions, fallback behavior, error catching, etc: [`redirectToDefaultLocale()`](/en/reference/modules/astro-i18n/#redirecttodefaultlocale), [`notFound()`](/en/reference/modules/astro-i18n/#notfound), and [`redirectToFallback()`](/en/reference/modules/astro-i18n/#redirecttofallback):

src/middleware.js

```js
import { defineMiddleware } from "astro:middleware";
import { redirectToDefaultLocale } from "astro:i18n"; // function available with `manual` routing
export const onRequest = defineMiddleware(async (ctx, next) => {
  if (ctx.url.startsWith("/about")) {
    return next();
  } else {
    return redirectToDefaultLocale(302);
  }
})
```

#### middleware function

[Section titled “middleware function”](#middleware-function)

The [`middleware`](#middleware-function) function manually creates Astro’s i18n middleware. This allows you to extend Astro’s i18n routing instead of completely replacing it.

You can run `middleware` with [routing options](#routing) in combination with your own middleware, using the [`sequence`](/en/reference/modules/astro-middleware/#sequence) utility to determine the order:

src/middleware.js

```js
import {defineMiddleware, sequence} from "astro:middleware";
import { middleware } from "astro:i18n"; // Astro's own i18n routing config


export const userMiddleware = defineMiddleware(async (ctx, next) => {
  // this response might come from Astro's i18n middleware, and it might return a 404
  const response = await next();
  // the /about page is an exception and we want to render it
  if (ctx.url.startsWith("/about")) {
    return new Response("About page", {
      status: 200
    });
  } else {
    return response;
  }
});




export const onRequest = sequence(
  userMiddleware,
  middleware({
    redirectToDefaultLocale: false,
    prefixDefaultLocale: true
  })
)
```

## `domains`

[Section titled “domains”](#domains)

**Added in:** `astro@4.9.0`

This routing option allows you to customize your domains on a per-language basis for `server` rendered projects using the [`@astrojs/node`](/en/guides/integrations-guide/node/) or [`@astrojs/vercel`](/en/guides/integrations-guide/vercel/) adapter with a `site` configured.

Add `i18n.domains` to map any of your supported `locales` to custom URLs:

astro.config.mjs

```diff
import { defineConfig } from "astro/config"
export default defineConfig({
  site: "https://example.com",
  output: "server", // required, with no prerendered pages
  adapter: node({
    mode: 'standalone',
  }),
  i18n: {
    locales: ["es", "en", "fr", "ja"],
    defaultLocale: "en",
    routing: {
      prefixDefaultLocale: false
    },
+    domains: {
+      fr: "https://fr.example.com",
+      es: "https://example.es"
+    }
  }
})
```

All non-mapped `locales` will follow your `prefixDefaultLocales` configuration.

With the above configuration:

* The file `/fr/about.astro` will create the URL `https://fr.example.com/about`.
* The file `/es/about.astro` will create the URL `https://example.es/about`.
* The file `/ja/about.astro` will create the URL `https://example.com/ja/about`.
* The file `/about.astro` will create the URL `https://example.com/about`.

The above URLs will also be returned by the `getAbsoluteLocaleUrl()` and `getAbsoluteLocaleUrlList()` functions.

## Fallback

[Section titled “Fallback”](#fallback)

When a page in one language doesn’t exist (e.g. a page that is not yet translated), instead of displaying a 404 page, you can choose to display fallback content from another `locale` on a per-language basis. This is useful when you do not yet have a page for every route, but you want to still provide some content to your visitors.

Your fallback strategy consists of two parts: choosing which languages should fallback to which other languages ([`i18n.fallback`](/en/reference/configuration-reference/#i18nfallback)) and choosing whether to perform a [redirect](/en/guides/routing/#redirects) or a [rewrite](/en/guides/routing/#rewrites) to show the fallback content ([`i18n.routing.fallbackType`](/en/reference/configuration-reference/#i18nroutingfallbacktype) added in Astro v4.15.0).

For example, when you configure `i18n.fallback: { fr: "es" }`, Astro will ensure that a page is built in `src/pages/fr/` for every page that exists in `src/pages/es/`.

If any page does not already exist, then a page will be created depending on your `fallbackType`:

* With a redirect to the corresponding `es` route (default behavior).
* With the content of the `/es/` page (`i18n.routing.fallbackType: "rewrite"`).

For example, the configuration below sets `es` as the fallback locale for any missing `fr` routes. This means that a user visiting `example.com/fr/my-page/` will be shown the content for `example.com/es/my-page/` (without being redirected) instead of being taken to a 404 page when `src/pages/fr/my-page.astro` does not exist.

astro.config.mjs

```diff
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
    locales: ["es", "en", "fr"],
    defaultLocale: "en",
+    fallback: {
+      fr: "es"
+    },
    routing: {
+      fallbackType: "rewrite"
    }
  }
})
```

## Custom locale paths

[Section titled “Custom locale paths”](#custom-locale-paths)

In addition to defining your site’s supported `locales` as strings (e.g. “en”, “pt-br”), Astro also allows you to map an arbitrary number of [browser-recognized language `codes`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language#syntax) to a custom URL `path`. While locales can be strings of any format as long as they correspond to your project folder structure, `codes` must follow the browser’s accepted syntax.

Pass an object to the `locales` array with a `path` key to define a custom URL prefix, and `codes` to indicate the languages mapped to this URL. In this case, your `/[locale]/` folder name must match exactly the value of the `path` and your URLs will be generated using the `path` value.

This is useful if you support multiple variations of a language (e.g. `"fr"`, `"fr-BR"`, and `"fr-CA"`) and you want to have all these variations mapped under the same URL `/fr/`, or even customize it entirely (e.g. `/french/`):

astro.config.mjs

```diff
import { defineConfig } from "astro/config"
export default defineConfig({
  i18n: {
-    locales: ["es", "en", "fr"],
+    locales: ["es", "en", {
+      path: "french", // no slashes included
+      codes: ["fr", "fr-BR", "fr-CA"]
+    }],
    defaultLocale: "en",
    routing: {
        prefixDefaultLocale: true
    }
  }
})
```

When using functions from the [`astro:i18n` virtual module](/en/reference/modules/astro-i18n/) to compute valid URL paths based on your configuration (e.g. `getRelativeLocaleUrl()`), [use the `path` as the value for `locale`](/en/reference/modules/astro-i18n/#getlocalebypath).

#### Limitations

[Section titled “Limitations”](#limitations)

This feature has some restrictions:

* The `site` option is mandatory.
* The `output` option must be set to `"server"`.
* There cannot be any individual prerendered pages.

Astro relies on the following headers in order to support the feature:

* [`X-Forwarded-Host`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-Host) and [`Host`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Host). Astro will use the former, and if not present, will try the latter.
* [`X-Forwarded-Proto`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-Proto) and [`URL#protocol`](https://developer.mozilla.org/en-US/docs/Web/API/URL/protocol) of the server request.

Make sure that your server proxy/hosting platform is able to provide this information. Failing to retrieve these headers will result in a 404 (status code) page.

## Browser language detection

[Section titled “Browser language detection”](#browser-language-detection)

Astro’s i18n routing allows you to access two properties for browser language detection in pages rendered on demand: `Astro.preferredLocale` and `Astro.preferredLocaleList`. All pages, including static prerendered pages, have access to `Astro.currentLocale`.

These combine the browser’s `Accept-Language` header, and your `locales` (strings or `codes`) to automatically respect your visitor’s preferred languages.

* [`Astro.preferredLocale`](/en/reference/api-reference/#preferredlocale): Astro can compute a **preferred locale** for your visitor if their browser’s preferred locale is included in your `locales` array. This value is undefined if no such match exists.

* [`Astro.preferredLocaleList`](/en/reference/api-reference/#preferredlocalelist): An array of all locales that are both requested by the browser and supported by your website. This produces a list of all compatible languages between your site and your visitor. The value is `[]` if none of the browser’s requested languages are found in your `locales` array. If the browser does not specify any preferred languages, then this value will be [`i18n.locales`](/en/reference/configuration-reference/#i18nlocales).

* [`Astro.currentLocale`](/en/reference/api-reference/#currentlocale): The locale computed from the current URL, using the syntax specified in your `locales` configuration. If the URL does not contain a `/[locale]/` prefix, then the value will default to [`i18n.defaultLocale`](/en/reference/configuration-reference/#i18ndefaultlocale).

In order to successfully match your visitors’ preferences, provide your `codes` using the same pattern [used by the browser](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language#syntax).


---

# Middleware

> Learn how to use middleware in Astro.

**Middleware** allows you to intercept requests and responses and inject behaviors dynamically every time a page or endpoint is about to be rendered. This rendering occurs at build time for all prerendered pages, but occurs when the route is requested for pages rendered on demand, making [additional SSR features like cookies and headers](/en/guides/on-demand-rendering/#on-demand-rendering-features) available.

Middleware also allows you to set and share request-specific information across endpoints and pages by mutating a `locals` object that is available in all Astro components and API endpoints. This object is available even when this middleware runs at build time.

## Basic Usage

[Section titled “Basic Usage”](#basic-usage)

1. Create `src/middleware.js|ts` (Alternatively, you can create `src/middleware/index.js|ts`.)

2. Inside this file, export an [`onRequest()`](/en/reference/modules/astro-middleware/#onrequest) function that can be passed a [`context` object](#the-context-object) and `next()` function. This must not be a default export.

   src/middleware.js

   ```js
   export function onRequest (context, next) {
       // intercept data from a request
       // optionally, modify the properties in `locals`
       context.locals.title = "New title";


       // return a Response or the result of calling `next()`
       return next();
   };
   ```

3. Inside any `.astro` file, access response data using `Astro.locals`.

   src/components/Component.astro

   ```astro
   ---
   const data = Astro.locals;
   ---
   <h1>{data.title}</h1>
   <p>This {data.property} is from middleware.</p>
   ```

### The `context` object

[Section titled “The context object”](#the-context-object)

The [`context`](/en/reference/api-reference/) object includes information to be made available to other middleware, API routes and `.astro` routes during the rendering process.

This is an optional argument passed to `onRequest()` that may contain the `locals` object as well as any additional properties to be shared during rendering. For example, the `context` object may include cookies used in authentication.

### Storing data in `context.locals`

[Section titled “Storing data in context.locals”](#storing-data-in-contextlocals)

`context.locals` is an object that can be manipulated inside the middleware.

This `locals` object is forwarded across the request handling process and is available as a property to [`APIContext`](/en/reference/api-reference/#locals) and [`AstroGlobal`](/en/reference/api-reference/#locals). This allows data to be shared between middlewares, API routes, and `.astro` pages. This is useful for storing request-specific data, such as user data, across the rendering step.

Integration properties

[Integrations](/en/guides/integrations/) may set properties and provide functionality through the `locals` object. If you are using an integration, check its documentation to ensure you are not overriding any of its properties or doing unnecessary work.

You can store any type of data inside `locals`: strings, numbers, and even complex data types such as functions and maps.

src/middleware.js

```js
export function onRequest (context, next) {
    // intercept data from a request
    // optionally, modify the properties in `locals`
    context.locals.user.name = "John Wick";
    context.locals.welcomeTitle = () => {
        return "Welcome back " + locals.user.name;
    };


    // return a Response or the result of calling `next()`
    return next();
};
```

Then you can use this information inside any `.astro` file with `Astro.locals`.

src/pages/orders.astro

```astro
---
const title = Astro.locals.welcomeTitle();
const orders = Array.from(Astro.locals.orders.entries());
const data = Astro.locals;
---
<h1>{title}</h1>
<p>This {data.property} is from middleware.</p>
<ul>
    {orders.map(order => {
        return <li>{/* do something with each order */}</li>;
    })}
</ul>
```

`locals` is an object that lives and dies within a single Astro route; when your route page is rendered, `locals` won’t exist anymore and a new one will be created. Information that needs to persist across multiple page requests must be stored elsewhere.

Note

The value of `locals` cannot be overridden at run time. Doing so would risk wiping out all the information stored by the user. Astro performs checks and will throw an error if `locals` are overridden.

## Example: redacting sensitive information

[Section titled “Example: redacting sensitive information”](#example-redacting-sensitive-information)

The example below uses middleware to replace “PRIVATE INFO” with the word “REDACTED” to allow you to render modified HTML on your page:

src/middleware.js

```js
export const onRequest = async (context, next) => {
    const response = await next();
    const html = await response.text();
    const redactedHtml = html.replaceAll("PRIVATE INFO", "REDACTED");


    return new Response(redactedHtml, {
        status: 200,
        headers: response.headers
    });
};
```

## Middleware types

[Section titled “Middleware types”](#middleware-types)

You can import and use the utility function [`defineMiddleware()`](/en/reference/modules/astro-middleware/#definemiddleware) to take advantage of type safety:

src/middleware.ts

```ts
import { defineMiddleware } from "astro:middleware";


// `context` and `next` are automatically typed
export const onRequest = defineMiddleware((context, next) => {


});
```

Instead, if you’re using JsDoc to take advantage of type safety, you can use `MiddlewareHandler`:

src/middleware.js

```js
/**
 * @type {import("astro").MiddlewareHandler}
 */
// `context` and `next` are automatically typed
export const onRequest = (context, next) => {


};
```

To type the information inside `Astro.locals`, which gives you autocompletion inside `.astro` files and middleware code, [extend the global types](/en/guides/typescript/#extending-global-types) by declaring a global namespace in the `env.d.ts` file:

src/env.d.ts

```ts
type User = {
  id: number;
  name: string;
};


declare namespace App {
  interface Locals {
    user: User;
    welcomeTitle: () => string;
    orders: Map<string, object>;
    session: import("./lib/server/session").Session | null;
  }
}
```

Then, inside the middleware file, you can take advantage of autocompletion and type safety.

## Chaining middleware

[Section titled “Chaining middleware”](#chaining-middleware)

Multiple middlewares can be joined in a specified order using [`sequence()`](/en/reference/modules/astro-middleware/#sequence):

src/middleware.js

```js
import { sequence } from "astro:middleware";


async function validation(_, next) {
    console.log("validation request");
    const response = await next();
    console.log("validation response");
    return response;
}


async function auth(_, next) {
    console.log("auth request");
    const response = await next();
    console.log("auth response");
    return response;
}


async function greeting(_, next) {
    console.log("greeting request");
    const response = await next();
    console.log("greeting response");
    return response;
}


export const onRequest = sequence(validation, auth, greeting);
```

This will result in the following console order:

```sh
validation request
auth request
greeting request
greeting response
auth response
validation response
```

## Rewriting

[Section titled “Rewriting”](#rewriting)

**Added in:** `astro@4.13.0`

The `APIContext` exposes a method called [`rewrite()`](/en/reference/api-reference/#rewrite) which works the same way as [Astro.rewrite](/en/guides/routing/#rewrites).

Use `context.rewrite()` inside middleware to display a different page’s content without [redirecting](/en/guides/routing/#dynamic-redirects) your visitor to a new page. This will trigger a new rendering phase, causing any middleware to be re-executed.

src/middleware.js

```js
import { isLoggedIn } from "~/auth.js"
export function onRequest (context, next) {
  if (!isLoggedIn(context)) {
    // If the user is not logged in, update the Request to render the `/login` route and
    // add header to indicate where the user should be sent after a successful login.
    // Re-execute middleware.
    return context.rewrite(new Request("/login", {
      headers: {
        "x-redirect-to": context.url.pathname
      }
    }));
  }


  return next();
};
```

You can also pass the `next()` function an optional URL path parameter to rewrite the current `Request` without retriggering a new rendering phase. The location of the rewrite path can be provided as a string, URL, or `Request`:

src/middleware.js

```js
import { isLoggedIn } from "~/auth.js"
export function onRequest (context, next) {
  if (!isLoggedIn(context)) {
    // If the user is not logged in, update the Request to render the `/login` route and
    // add header to indicate where the user should be sent after a successful login.
    // Return a new `context` to any following middlewares.
    return next(new Request("/login", {
      headers: {
        "x-redirect-to": context.url.pathname
      }
    }));
  }


  return next();
};
```

The `next()` function accepts the same payload of [the `Astro.rewrite()` function](/en/reference/api-reference/#rewrite). The location of the rewrite path can be provided as a string, URL, or `Request`.

When you have multiple middleware functions chained via [sequence()](#chaining-middleware), submitting a path to `next()` will rewrite the `Request` in place and the middleware will not execute again. The next middleware function in the chain will receive the new `Request` with its updated `context`.

Calling `next()` with this signature will create a new `Request` object using the old `ctx.request`. This means that trying to consume `Request.body`, either before or after this rewrite, will throw a runtime error. This error is often raised with [Astro Actions that use HTML forms](/en/guides/actions/#call-actions-from-an-html-form-action). In these cases, we recommend handling rewrites from your Astro templates using `Astro.rewrite()` instead of using middleware.

src/middleware.js

```js
// Current URL is https://example.com/blog


// First middleware function
async function first(context, next) {
  console.log(context.url.pathname) // this will log "/blog"
  // Rewrite to a new route, the homepage
  // Return updated `context` which is passed to next function
  return next("/")
}


// Current URL is still https://example.com/blog


// Second middleware function
async function second(context, next) {
  // Receives updated `context`
  console.log(context.url.pathname) // this will log  "/"
  return next()
}


export const onRequest = sequence(first, second);
```

## Error pages

[Section titled “Error pages”](#error-pages)

Middleware will attempt to run for all on-demand rendered pages, even when a matching route cannot be found. This includes Astro’s default (blank) 404 page and any custom 404 pages. However, it is up to the [adapter](/en/guides/on-demand-rendering/) to decide whether that code runs. Some adapters may serve a platform-specific error page instead.

Middleware will also attempt to run before serving a 500 error page, including a custom 500 page, unless the server error occurred in the execution of the middleware itself. If your middleware does not run successfully, then you will not have access to `Astro.locals` to render your 500 page.


---

# On-demand rendering

> Generate server-rendered pages and routes on demand with an adapter.

Your Astro project code must be **rendered** to HTML in order to be displayed on the web.

By default, Astro pages, routes, and API endpoints will be pre-rendered at build time as static pages. However, you can choose to render some or all of your routes on demand by a server when a route is requested.

On-demand rendered pages and routes are generated per visit, and can be customized for each viewer. For example, a page rendered on demand can show a logged-in user their account information or display freshly updated data without requiring a full-site rebuild.

On-demand rendering on the server at request time is also known as **server-side rendering (SSR)**.

## Server adapters

[Section titled “Server adapters”](#server-adapters)

To render any page on demand, you need to add an **adapter**. Each adapter allows Astro to output a script that runs your project on a specific **runtime**: the environment that runs code on the server to generate pages when they are requested (e.g. Netlify, Cloudflare).

You may also wish to add an adapter even if your site is entirely static and you are not rendering any pages on demand. For example, the [Netlify adapter](/en/guides/integrations-guide/netlify/) enables Netlify’s Image CDN, and [server islands](/en/guides/server-islands/) require an adapter installed to use `server:defer` on a component.

### Adapters

* ![](/logos/cloudflare-pages.svg)

  ### [@astrojs/​cloudflare](/en/guides/integrations-guide/cloudflare/)

* ![](/logos/netlify.svg)

  ### [@astrojs/​netlify](/en/guides/integrations-guide/netlify/)

* ![](/logos/node.svg)

  ### [@astrojs/​node](/en/guides/integrations-guide/node/)

* ![](/logos/vercel.svg)

  ### [@astrojs/​vercel](/en/guides/integrations-guide/vercel/)

Astro maintains official adapters for [Node.js](https://nodejs.org/), [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/), and [Cloudflare](https://www.cloudflare.com/). You can find both [official and community adapters in our integrations directory](https://astro.build/integrations/?search=\&categories%5B%5D=adapters). Choose the one that corresponds to your [deployment environment](/en/guides/deploy/).

### Add an Adapter

[Section titled “Add an Adapter”](#add-an-adapter)

You can add any of the [official adapter integrations maintained by Astro](/en/guides/integrations/#official-integrations) with the following `astro add` command. This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

For example, to install the Netlify adapter, run:

* npm

  ```shell
  npx astro add netlify
  ```

* pnpm

  ```shell
  pnpm astro add netlify
  ```

* Yarn

  ```shell
  yarn astro add netlify
  ```

You can also [add an adapter manually by installing the NPM package](/en/guides/integrations/#installing-an-npm-package) (e.g. `@astrojs/netlify`) and updating `astro.config.mjs` yourself.

Note that different adapters may have different configuration settings. Read each adapter’s documentation, and apply any necessary config options to your chosen adapter in `astro.config.mjs`

## Enabling on-demand rendering

[Section titled “Enabling on-demand rendering”](#enabling-on-demand-rendering)

**By default, your entire Astro site will be prerendered**, and static HTML pages will be sent to the browser. However, you may opt out of prerendering on any routes that require server rendering, for example, a page that checks for cookies and displays personalized content.

First, [add an adapter integration](#add-an-adapter) for your server runtime to enable on-demand server rendering in your Astro project.

Then, add `export const prerender = false` at the top of the individual page or endpoint you want to render on demand. The rest of your site will remain a static site:

src/pages/page-rendered-on-demand.astro

```diff
---
+export const prerender = false
---
<html>
<!--
This content will be server-rendered on demand!
Just add an adapter integration for a server runtime!
All other pages are statically-generated at build time!
-->
<html>
```

The following example shows opting out of prerendering in order to display a random number each time the endpoint is hit:

src/pages/randomnumber.js

```diff
+export const prerender = false;


export async function GET() {
  let number = Math.random();
  return new Response(
    JSON.stringify({
      number,
      message: `Here's a random number: ${number}`,
    }),
  );
}
```

### `'server'` mode

[Section titled “'server' mode”](#server-mode)

For a **highly dynamic app**, after adding an adapter, you can [set your build output configuration to `output: 'server'`](/en/reference/configuration-reference/#output) to **server-render all your pages by default**. This is the equivalent of opting out of prerendering on every page.

Then, if needed, you can choose to prerender any individual pages that do not require a server to execute, such as a privacy policy or about page.

src/pages/about-my-app.astro

```diff
---
+export const prerender = true
---
<html>
<!--
`output: 'server'` is configured, but this page is static!
The rest of my site is rendered on demand!
-->
<html>
```

Add `export const prerender = true` to any page or route to prerender a static page or endpoint:

src/pages/myendpoint.js

```diff
+export const prerender = true;


export async function GET() {
  return new Response(
    JSON.stringify({
      message: `This is my static endpoint`,
    }),
  );
}
```

Tip

Start with the default `'static'` mode until you are sure that **most or all** of your pages will be rendered on demand! This ensures that your site is as performant as possible, not relying on a server function to render static content.

The `'server'` output mode does not bring any additional functionality. It only switches the default rendering behavior.

See more about the [`output` setting](/en/reference/configuration-reference/#output) in the configuration reference.

## On-demand rendering features

[Section titled “On-demand rendering features”](#on-demand-rendering-features)

### HTML streaming

[Section titled “HTML streaming”](#html-streaming)

With HTML streaming, a document is broken up into chunks, sent over the network in order, and rendered on the page in that order. Astro uses HTML streaming in on-demand rendering to send each component to the browser as it renders them. This makes sure the user sees your HTML as fast as possible, although network conditions can cause large documents to be downloaded slowly, and waiting for data fetches can block page rendering.

![](/houston_chef.webp) **Related recipe:** [Using streaming to improve page performance](/en/recipes/streaming-improve-page-performance/)

Caution

Features that modify the [Response headers](https://developer.mozilla.org/en-US/docs/Glossary/Response_header) are only available at the **page level**. (You can’t use them inside of components, including layout components.) By the time Astro runs your component code, it has already sent the Response headers and they cannot be modified.

### Cookies

[Section titled “Cookies”](#cookies)

A page or API endpoint rendered on demand can check, set, get, and delete cookies.

The example below updates the value of a cookie for a page view counter:

src/pages/index.astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


let counter = 0


if (Astro.cookies.has('counter')) {
  const cookie = Astro.cookies.get('counter')
  const value = cookie?.number()
  if (value !== undefined && !isNaN(value)) counter = value + 1
}


Astro.cookies.set('counter', String(counter))
---
<html>
  <h1>Counter = {counter}</h1>
</html>
```

See more details about [`Astro.cookies` and the `AstroCookie` type](/en/reference/api-reference/#cookies) in the API reference.

### `Response`

[Section titled “Response”](#response)

[`Astro.response`](/en/reference/api-reference/#response) is a standard [`ResponseInit`](https://developer.mozilla.org/en-US/docs/Web/API/Response/Response#options) object. It can be used to set the response status and headers.

The example below sets a response status and status text for a product page when the product does not exist:

src/pages/product/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getProduct } from '../api';


const product = await getProduct(Astro.params.id);


// No product found
if (!product) {
  Astro.response.status = 404;
  Astro.response.statusText = 'Not found';
}
---
<html>
  <!-- Page here... -->
</html>
```

#### `Astro.response.headers`

[Section titled “Astro.response.headers”](#astroresponseheaders)

You can set headers using the `Astro.response.headers` object:

src/pages/index.astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


Astro.response.headers.set('Cache-Control', 'public, max-age=3600');
---
<html>
  <!-- Page here... -->
</html>
```

#### Return a `Response` object

[Section titled “Return a Response object”](#return-a-response-object)

You can also return a [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) object directly from any page using on-demand rendering either manually or with [`Astro.redirect`](/en/reference/api-reference/#redirect).

The example below looks up an ID in the database on a dynamic page and either it returns a 404 if the product does not exist, or it redirects the user to another page if the product is no longer available, or it displays the product:

src/pages/product/\[id].astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


import { getProduct } from '../api';


const product = await getProduct(Astro.params.id);


// No product found
if (!product) {
  return new Response(null, {
    status: 404,
    statusText: 'Not found'
  });
}


// The product is no longer available
if (!product.isAvailable) {
  return Astro.redirect("/products", 301);
}
---
<html>
  <!-- Page here... -->
</html>
```

### `Request`

[Section titled “Request”](#request)

`Astro.request` is a standard [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) object. It can be used to get the `url`, `headers`, `method`, and even the body of the request.

You can access additional information from this object for pages that are not statically generated.

#### `Astro.request.headers`

[Section titled “Astro.request.headers”](#astrorequestheaders)

The headers for the request are available on `Astro.request.headers`. This works like the browser’s [`Request.headers`](https://developer.mozilla.org/en-US/docs/Web/API/Request/headers). It is a [Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers) object where you can retrieve headers such as the cookie.

src/pages/index.astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


const cookie = Astro.request.headers.get('cookie');
// ...
---
<html>
  <!-- Page here... -->
</html>
```

#### `Astro.request.method`

[Section titled “Astro.request.method”](#astrorequestmethod)

The HTTP method used in the request is available as `Astro.request.method`. This works like the browser’s [`Request.method`](https://developer.mozilla.org/en-US/docs/Web/API/Request/method). It returns the string representation of the HTTP method used in the request.

src/pages/index.astro

```astro
---
export const prerender = false; // Not needed in 'server' mode


console.log(Astro.request.method) // GET (when navigated to in the browser)
---
```

See more details about [`Astro.request`](/en/reference/api-reference/#request) in the API reference.

### Server Endpoints

[Section titled “Server Endpoints”](#server-endpoints)

A server endpoint, also known as an **API route**, is a special function exported from a `.js` or `.ts` file within the `src/pages/` folder. A powerful feature of server-side rendering on demand, API routes are able to securely execute code on the server.

The function takes an [endpoint context](/en/reference/api-reference/) and returns a [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response).

To learn more, see our [Endpoints Guide](/en/guides/endpoints/#server-endpoints-api-routes).


---

# Prefetch

> Prefetch links for snappier navigation between pages.

Page load times play a big role in the usability and overall enjoyment of a site. Astro’s **opt-in prefetching** brings the benefits of near-instant page navigations to your multi-page application (MPA) as your visitors interact with the site.

## Enable prefetching

[Section titled “Enable prefetching”](#enable-prefetching)

You can enable prefetching with the `prefetch` config:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
+  prefetch: true
});
```

A prefetch script will be added to all pages of your site. You can then add the `data-astro-prefetch` attribute to any `<a />` links on your site to opt-in to prefetching. When you hover over the link, the script will fetch the page in the background.

```html
<a href="/about" data-astro-prefetch>
```

Note that prefetching only works for links within your site, and not external links.

## Prefetch configuration

[Section titled “Prefetch configuration”](#prefetch-configuration)

The `prefetch` config also accepts an option object to further customize prefetching.

### Prefetch strategies

[Section titled “Prefetch strategies”](#prefetch-strategies)

Astro supports 4 prefetch strategies for various use cases:

* `hover` (default): Prefetch when you hover over or focus on the link.
* `tap`: Prefetch just before you click on the link.
* `viewport`: Prefetch as the links enter the viewport.
* `load`: Prefetch all links on the page after the page is loaded.

You can specify a strategy for an individual link by passing it to the `data-astro-prefetch` attribute:

```html
<a href="/about" data-astro-prefetch="tap">About</a>
```

Each strategy is fine-tuned to only prefetch when needed and save your users’ bandwidth. For example:

* If a visitor is using [data saver mode](https://developer.mozilla.org/en-US/docs/Web/API/NetworkInformation/saveData) or has a [slow connection](https://developer.mozilla.org/en-US/docs/Web/API/NetworkInformation/effectiveType), prefetch will fallback to the `tap` strategy.
* Quickly hovering or scrolling over links will not prefetch them.

### Default prefetch strategy

[Section titled “Default prefetch strategy”](#default-prefetch-strategy)

The default prefetch strategy when adding the `data-astro-prefetch` attribute is `hover`. To change it, you can configure [`prefetch.defaultStrategy`](/en/reference/configuration-reference/#prefetchdefaultstrategy) in your `astro.config.mjs` file:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
+  prefetch: {
+    defaultStrategy: 'viewport'
+  }
});
```

### Prefetch all links by default

[Section titled “Prefetch all links by default”](#prefetch-all-links-by-default)

If you want to prefetch all links, including those without the `data-astro-prefetch` attribute, you can set [`prefetch.prefetchAll`](/en/reference/configuration-reference/#prefetchprefetchall) to `true`:

astro.config.mjs

```diff
import { defineConfig } from 'astro/config';


export default defineConfig({
+  prefetch: {
+    prefetchAll: true
+  }
});
```

You can then opt-out of prefetching for individual links by setting `data-astro-prefetch="false"`:

```html
<a href="/about" data-astro-prefetch="false">About</a>
```

The default prefetch strategy for all links can be changed with `prefetch.defaultStrategy` as shown in the [Default prefetch strategy section](#default-prefetch-strategy).

## Prefetch programmatically

[Section titled “Prefetch programmatically”](#prefetch-programmatically)

As some navigation might not always appear as `<a />` links, you can also prefetch programmatically with the `prefetch()` API from the `astro:prefetch` module:

```astro
<button id="btn">Click me</button>


<script>
  import { prefetch } from 'astro:prefetch';


  const btn = document.getElementById('btn');
  btn.addEventListener('click', () => {
    prefetch('/about');
  });
</script>
```

The `prefetch()` API includes the same [data saver mode](https://developer.mozilla.org/en-US/docs/Web/API/NetworkInformation/saveData) and [slow connection](https://developer.mozilla.org/en-US/docs/Web/API/NetworkInformation/effectiveType) detection so that it only prefetches when needed.

To ignore slow connection detection, you can use the `ignoreSlowConnection` option:

```js
// Prefetch even on data saver mode or slow connection
prefetch('/about', { ignoreSlowConnection: true });
```

### `eagerness`

[Section titled “eagerness”](#eagerness)

**Type:** `'immediate' | 'eager' | 'moderate' | 'conservative'`\
**Default:** `'immediate'`

**Added in:** `astro@5.6.0`

With the experimental [`clientPrerender`](/en/reference/experimental-flags/client-prerender/) flag enabled, you can use the `eagerness` option on `prefetch()` to suggest to the browser how eagerly it should prefetch/prerender link targets.

This follows the same API described in the [Speculation Rules API](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/speculationrules#eagerness) and defaults to `immediate` (the most eager option). In decreasing order of eagerness, the other options are `eager`, `moderate`, and `conservative`.

The `eagerness` option allows you to balance the benefit of reduced wait times against bandwidth, memory, and CPU costs for your site visitors. Some browsers, such as Chrome, have [limits in place to guard against over-speculating](https://developer.chrome.com/blog/speculation-rules-improvements#chrome-limits) (prerendering/prefetching too many links).

```astro
---
---
<script>
// Control prefetching eagerness with `experimental.clientPrerender`
import { prefetch } from 'astro:prefetch';


// This page is resource-intensive
prefetch('/data-heavy-dashboard', { eagerness: 'conservative' });


// This page is critical to the visitor's journey
prefetch('/getting-started'); // defaults to `{ eagerness: 'immediate' }`


// This page may not be visited
prefetch('/terms-of-service', { eagerness: 'moderate' });
</script>
```

To use `prefetch()` programmatically with large sets of links, you can set `eagerness: 'moderate'` to take advantage of [First In, First Out (FIFO)](https://en.wikipedia.org/wiki/FIFO_\(computing_and_electronics\)) strategies and browser heuristics to let the browser decide when to prerender/prefetch them and in what order:

```astro
<a class="link-moderate" href="/nice-link-1">A Nice Link 1</a>
<a class="link-moderate" href="/nice-link-2">A Nice Link 2</a>
<a class="link-moderate" href="/nice-link-3">A Nice Link 3</a>
<a class="link-moderate" href="/nice-link-4">A Nice Link 4</a>
...
<a class="link-moderate" href="/nice-link-20">A Nice Link 20</a>


<script>
  import { prefetch } from 'astro:prefetch';


  const linkModerate = document.getElementsByClassName('link-moderate');
  linkModerate.forEach((link) => prefetch(link.getAttribute('href'), {eagerness: 'moderate'}));


</script>
```

Make sure to only import `prefetch()` in client-side scripts as it relies on browser APIs.

## Using with View Transitions

[Section titled “Using with View Transitions”](#using-with-view-transitions)

When you use [Astro’s `<ClientRouter />`](/en/guides/view-transitions/#enabling-view-transitions-spa-mode) on a page, prefetching will also be enabled by default. It sets a default configuration of `{ prefetchAll: true }` which enables [prefetching for all links](#prefetch-all-links-by-default) on the page.

You can customize the prefetch configuration in `astro.config.mjs` to override the default. For example:

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  // Disable prefetch completely
  prefetch: false
});
```

astro.config.mjs

```js
import { defineConfig } from 'astro/config';


export default defineConfig({
  // Keep prefetch, but only prefetch for links with `data-astro-prefetch`
  prefetch: {
    prefetchAll: false
  }
});
```

## Browser support

[Section titled “Browser support”](#browser-support)

Astro’s prefetching uses [`<link rel="prefetch">`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/prefetch) if supported by the browser, and falls back to the [`fetch()` API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) otherwise.

The most common browsers support Astro’s prefetching with subtle differences:

### Chrome

[Section titled “Chrome”](#chrome)

Chrome supports `<link rel="prefetch">`. Prefetching works as intended.

It also fully supports `<script type="speculationrules">` from the [Speculation Rules API](https://developer.mozilla.org/en-US/docs/Web/API/Speculation_Rules_API), which can be used to further describe [prefetching strategies and rules](#eagerness), enhancing user experience for your Chrome users. You’ll need to enable [`clientPrerender`](/en/reference/experimental-flags/client-prerender/) experiment to utilize this functionality with `prefetch()`

### Firefox

[Section titled “Firefox”](#firefox)

Firefox supports `<link rel="prefetch">` but may display errors or fail entirely:

* Without an explicit cache header (e.g. [`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) or [`Expires`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires)), prefetching will error with `NS_BINDING_ABORTED`.
* Even in the event of an error, if the response has a proper [`ETag`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) header, it will be re-used on navigation.
* Otherwise, if it errors with no other cache headers, the prefetch will not work.

### Safari

[Section titled “Safari”](#safari)

Safari does not support `<link rel="prefetch">` and will fall back to the `fetch()` API which requires cache headers (e.g. [`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control), [`Expires`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires), and [`ETag`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)) to be set. Otherwise, the prefetch will not work.

**Edge case:** `ETag` headers do not work in private windows.

### Recommendations

[Section titled “Recommendations”](#recommendations)

To best support all browsers, make sure your pages have the proper cache headers.

For static or prerendered pages, the `ETag` header is often automatically set by the deployment platform and is expected to work out of the box.

For dynamic and server-side rendered pages, set the appropriate cache headers yourself based on the page content. Visit the [MDN documentation on HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) for more information.

## Migrating from `@astrojs/prefetch`

[Section titled “Migrating from @astrojs/prefetch”](#migrating-from-astrojsprefetch)

The `@astrojs/prefetch` integration was deprecated in v3.5.0 and is no longer maintained. Use the following instructions to migrate to Astro’s built-in prefetching which replaces this integration.

1. Remove the `@astrojs/prefetch` integration and enable the `prefetch` config in `astro.config.mjs`:

   astro.config.mjs

   ```diff
   import { defineConfig } from 'astro/config';
   -import prefetch from '@astrojs/prefetch';


   export default defineConfig({
   -  integrations: [prefetch()],
   +  prefetch: true
   });
   ```

2. Convert from `@astrojs/prefetch`’s configuration options:

   * The deprecated integration used the `selector` config option to specify which links should be prefetched upon entering the viewport.

     Add `data-astro-prefetch="viewport"` to these individual links instead.

     ```html
     <a href="/about" data-astro-prefetch="viewport">
     ```

   * The deprecated integration used the `intentSelector` config option to specify which links should be prefetched when they were hovered over or focused.

     Add `data-astro-prefetch` or `data-astro-prefetch="hover"` to these individual links instead:

     ```html
     <!-- You can omit the value if `defaultStrategy` is set to `hover` (default) -->
     <a href="/about" data-astro-prefetch>


     <!-- Otherwise, you can explicitly define the prefetch strategy -->
     <a href="/about" data-astro-prefetch="hover">
     ```

   * The `throttles` option from `@astrojs/prefetch` is no longer needed as the new prefetch feature will automatically schedule and prefetch optimally.


---

# Server islands

> Combine high performance static HTML with dynamic server-rendered content.

Server islands allow you to on-demand render dynamic or personalized “islands” individually, without sacrificing the performance of the rest of the page.

This means your visitor will see the most important parts of your page sooner, and allows your main content to be more aggressively cached, providing faster performance.

## Server island components

[Section titled “Server island components”](#server-island-components)

A server island is a normal server-rendered [Astro component](/en/basics/astro-components/) that is instructed to delay rendering until its contents are available.

Your page will be rendered immediately with any specified [fallback content as a placeholder](#server-island-fallback-content). Then, the component’s own contents are fetched on the client and displayed when available.

With [an adapter installed](/en/guides/on-demand-rendering/#server-adapters) to perform the delayed rendering, add the [`server:defer` directive](/en/reference/directives-reference/#server-directives) to any component on your page to turn it into its own island:

src/pages/index.astro

```astro
---
import Avatar from '../components/Avatar.astro';
---
<Avatar server:defer />
```

These components can do [anything you normally would in an on-demand rendered page](/en/guides/on-demand-rendering/#on-demand-rendering-features) using an adapter, such as fetch content, and access cookies:

src/components/Avatar.astro

```astro
---
import { getUserAvatar } from '../sessions';
const userSession = Astro.cookies.get('session');
const avatarURL = await getUserAvatar(userSession);
---
<img alt="User avatar" src={avatarURL} />
```

### Passing props to server islands

[Section titled “Passing props to server islands”](#passing-props-to-server-islands)

Props provided to server island components must be [serializable](https://developer.mozilla.org/en-US/docs/Glossary/Serialization): able to be translated into a format suitable for transfer over a network, or storage. Additionally, Astro does not serialize every type of serializable data structure. Therefore, there are some limitations on what can be passed as props to a server island.

Notably, functions cannot be passed to components marked with `server:defer` as they cannot be serialized. Objects with circular references are also not serializable.

The following prop types are supported: plain object, `number`, `string`, `Array`, `Map`, `Set`, `RegExp`, `Date`, `BigInt`, `URL`, `Uint8Array`, `Uint16Array`, `Uint32Array`, and `Infinity`

## Server island fallback content

[Section titled “Server island fallback content”](#server-island-fallback-content)

When using the `server:defer` attribute on a component to delay its rendering, you can “slot” in default loading content using the included named `"fallback"` slot.

Your fallback content will be rendered along with the rest of the page initially on page load and will be replaced with your component’s content when available.

To add fallback content, add `slot="fallback"` on a child (other components or HTML elements) passed to your server island component:

```astro
---
import Avatar from '../components/Avatar.astro';
import GenericAvatar from '../components/GenericAvatar.astro';
---
<Avatar server:defer>
  <GenericAvatar slot="fallback" />
</Avatar>
```

This fallback content can be things like:

* A generic avatar instead of the user’s own.
* Placeholder UI such as custom messages.
* Loading indicators such as spinners.

## How it works

[Section titled “How it works”](#how-it-works)

Server island implementation happens mostly at build-time where component content is swapped out for a small script.

Each of the islands marked with `server:defer` is split off into its own special route which the script fetches at run time. When Astro builds your site it will omit the component and inject a script in its place, and any content you’ve marked with `slot="fallback"`.

When the page loads in the browser, these components will be requested to a special endpoint that renders them and returns the HTML. This means that users will see the most critical parts of the page instantly. Fallback content will be visible for a short amount of time before the dynamic islands are then loaded.

Each island is loaded independently from the rest. This means a slower island won’t delay the rest of your personalized content from being available.

This rendering pattern was built to be portable. It does not depend on any server infrastructure so it will work with any host you have, from a Node.js server in a Docker container to the serverless provider of your choice.

## Caching

[Section titled “Caching”](#caching)

The data for server islands is retrieved via a `GET` request, passing props as an encrypted string in the URL query. This allows caching data with the [`Cache-Control` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) using standard `Cache-Control` directives.

However, [the browser limits URLs to a maximum length of 2048 bytes](https://chromium.googlesource.com/chromium/src/+/master/docs/security/url_display_guidelines/url_display_guidelines.md#url-length) for practical reasons and to avoid causing denial-of-service problems. If your query string causes your URL to exceed this limit, Astro will instead send a `POST` request that contains all props in the body.

`POST` requests are not cached by browsers because they are used to submit data, and could cause data integrity or security issues. Therefore, any existing caching logic in your project will break. Whenever possible, pass only necessary props to your server islands and avoid sending entire data objects and arrays to keep your query small.

## Accessing the page URL in a server island

[Section titled “Accessing the page URL in a server island”](#accessing-the-page-url-in-a-server-island)

In most cases you, your server island component can get information about the page rendering it by [passing props](/en/basics/astro-components/#component-props) like in normal components.

However, server islands run in their own isolated context outside of the page request. `Astro.url` and `Astro.request.url` in a server island component both return a URL that looks like `/_server-islands/Avatar` instead of the current page’s URL in the browser. Additionally, if you are prerendering the page you will not have access to information such as query parameters in order to pass as props.

To access information from the page’s URL, you can check the [Referer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) header, which will contain the address of the page that is loading the island in the browser:

```astro
---
const referer = Astro.request.headers.get('Referer');
const url = new URL(referer);
const productId = url.searchParams.get('product');
---
```

## Reusing the encryption key

[Section titled “Reusing the encryption key”](#reusing-the-encryption-key)

Astro uses [cryptography](https://developer.mozilla.org/en-US/docs/Glossary/Cryptography) to encrypt props passed to server islands, protecting sensitive data from accidental exposure. This encryption relies on a new, random key that is generated on each build and embedded in the server bundle.

Most deploy hosts will handle keeping your front end and back end in sync automatically. However, you may need a constant encryption key if you are using rolling deployments, multi-region hosting or a CDN that caches pages containing server islands.

In environments with rolling deployments (e.g., Kubernetes) where your frontend assets (which encrypt props) and your backend functions (which decrypt props) may be temporarily using different keys, or when a CDN is still serving pages built with an old key, encrypted props passed to your server island cannot be decrypted.

In these situations, use the Astro CLI to generate a reusable, encoded encryption key to set as an environment variable in your build environment:

```shell
astro create-key
```

Use this value to configure the `ASTRO_KEY` environment variable (e.g. in a `.env` file) and include it in your CI/CD or host’s build settings. This ensures the same key is always reused in the generated bundle so that encryption and decryption remain in sync.


---

# Sessions

> Share data between requests for on-demand rendered pages.

**Added in:** `astro@5.7.0`

Sessions are used to share data between requests for [on-demand rendered pages](/en/guides/on-demand-rendering/).

Unlike [`cookies`](/en/guides/on-demand-rendering/#cookies), sessions are stored on the server, so you can store larger amounts of data without worrying about size limits or security issues. They are useful for storing things like user data, shopping carts, and form state, and they work without any client-side JavaScript:

src/components/CartButton.astro

```astro
---
export const prerender = false; // Not needed with 'server' output
const cart = await Astro.session?.get('cart');
---


<a href="/checkout">🛒 {cart?.length ?? 0} items</a>
```

## Configuring sessions

[Section titled “Configuring sessions”](#configuring-sessions)

Sessions require a storage driver to store the session data. The [Node](/en/guides/integrations-guide/node/#sessions), [Cloudflare](/en/guides/integrations-guide/cloudflare/#sessions), and [Netlify](/en/guides/integrations-guide/netlify/#sessions) adapters automatically configure a default driver for you, but other adapters currently require you to [specify a driver manually](/en/reference/configuration-reference/#sessiondriver).

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

See [the `session` configuration option](/en/reference/configuration-reference/#session-options) for more details on setting a storage driver, and other configurable options.

## Interacting with session data

[Section titled “Interacting with session data”](#interacting-with-session-data)

The [`session` object](/en/reference/api-reference/#session) allows you to interact with the stored user state (e.g. adding items to a shopping cart) and the session ID (e.g. deleting the session ID cookie when logging out). The object is accessible as `Astro.session` in your Astro components and pages and as `context.session` object in API endpoints, middleware, and actions.

The session is generated automatically when it is first used and can be regenerated at any time with [`session.regenerate()`](/en/reference/api-reference/#sessionregenerate) or destroyed with [`session.destroy()`](/en/reference/api-reference/#sessiondestroy).

For many use cases, you will only need to use [`session.get()`](/en/reference/api-reference/#sessionget) and [`session.set()`](/en/reference/api-reference/#sessionset).

See [the Sessions API reference](/en/reference/api-reference/#session) for more details.

### Astro components and pages

[Section titled “Astro components and pages”](#astro-components-and-pages)

In `.astro` components and pages, you can access the session object via the global `Astro` object. For example, to display the number of items in a shopping cart:

src/components/CartButton.astro

```astro
---
export const prerender = false; // Not needed with 'server' output
const cart = await Astro.session?.get('cart');
---


<a href="/checkout">🛒 {cart?.length ?? 0} items</a>
```

### API endpoints

[Section titled “API endpoints”](#api-endpoints)

In API endpoints, the session object is available on the `context` object. For example, to add an item to a shopping cart:

src/pages/api/addToCart.ts

```ts
export async function POST(context: APIContext) {
  const cart = await context.session?.get('cart') || [];
  const data = await context.request.json<{ item: string }>();
  if(!data?.item) {
    return new Response('Item is required', { status: 400 });
  }
  cart.push(data.item);
  await context.session?.set('cart', cart);
  return Response.json(cart);
}
```

### Actions

[Section titled “Actions”](#actions)

In actions, the session object is available on the `context` object. For example, to add an item to a shopping cart:

src/actions/addToCart.ts

```ts
import { defineAction } from 'astro:actions';
import { z } from 'astro/zod';


export const server = {
  addToCart: defineAction({
    input: z.object({ productId: z.string() }),
    handler: async (input, context) => {
      const cart = await context.session?.get('cart');
      cart.push(input.productId);
      await context.session?.set('cart', cart);
      return cart;
    },
  }),
};
```

### Middleware

[Section titled “Middleware”](#middleware)

Note

Sessions are not supported in edge middleware.

In middleware, the session object is available on the `context` object. For example, to set the last visit time in the session:

src/middleware.ts

```ts
import { defineMiddleware } from 'astro:middleware';


export const onRequest = defineMiddleware(async (context, next) => {
  context.session?.set('lastVisit', new Date());
  return next();
});
```

## Session data types

[Section titled “Session data types”](#session-data-types)

By default session data is untyped, and you can store arbitrary data in any key. Values are serialized and deserialized using [devalue](https://github.com/Rich-Harris/devalue), which is the same library used in content collections and actions. This means that supported types are the same, and include strings, numbers, `Date`, `Map`, `Set`, `URL`, arrays, and plain objects.

You can optionally [define TypeScript types](/en/guides/typescript/#extending-global-types) for your session data by creating a `src/env.d.ts` file and adding a declaration for the `App.SessionData` type:

src/env.d.ts

```ts
declare namespace App {
  interface SessionData {
    user: {
      id: string;
      name: string;
    };
    cart: string[];
  }
}
```

This will allow you to access the session data with type-checking and auto-completion in your editor:

src/components/CartButton.astro

```ts
---
const cart = await Astro.session?.get('cart');
// const cart: string[] | undefined


const something = await Astro.session?.get('something');
// const something: any


Astro.session?.set('user', { id: 1, name: 'Houston' });
// Error: Argument of type '{ id: number; name: string }' is not assignable to parameter of type '{ id: string; name: string; }'.
---
```

Caution

This is only used for type-checking and does not affect the runtime behavior of the session. Take extra care if you change the type when users have stored data in the session, as this could cause runtime errors.


---

# View transitions

> Enable seamless navigation between pages in Astro with view transitions.

[View transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API) are animated transitions between different website views. They are a popular design choice for preserving visual continuity as visitors move between states or views of an application.

Astro’s view transitions and client-side routing support is powered by the [View Transitions browser API](https://developer.chrome.com/docs/web-platform/view-transitions/) and also includes:

* A few [built-in animation options](#built-in-animation-directives), such as `fade`, `slide`, and `none`.
* Support for both forwards and backwards navigation animations.
* The ability to fully [customize all aspects of transition animation](#customizing-animations), and build your own animations.
* A way to carry HTML elements from the current page to the next during navigation.
* The option to [prevent client-side navigation for non-page links](#preventing-client-side-navigation).
* [Control over fallback behavior](#fallback-control) for browsers that do not yet support the View Transition APIs.
* Automatic support for [`prefers-reduced-motion`](#prefers-reduced-motion).

Note

By default, every page will use regular, full-page, browser navigation. You must opt in to view transitions and can use them either on a per-page basis or site-wide.

## Differences between browser-native view transitions and Astro’s `<ClientRouter />`

[Section titled “Differences between browser-native view transitions and Astro’s \<ClientRouter />”](#differences-between-browser-native-view-transitions-and-astros-clientrouter-)

[Browser-native, cross-document view transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API/Using#basic_mpa_view_transition) can be used in Astro to animate the navigation between documents in a multi-page app (MPA), often providing the experience of client-side routing of single-page applications. They don’t alter the core functionality of a multi-page application, nor do they affect any existing scripts or add additional JavaScript to your page load. They simply add animations.

For enhanced client-side routing and view transition features not yet fully supported by the View Transition API, Astro provides a built-in, lightweight component to enable client-side routing and turn your multi-page app into a [single-page app](#enabling-view-transitions-spa-mode) with smooth animations on navigation.

That comes with some benefits, like shared state across pages and persistent elements, and some drawbacks, such as needing to manually reinitialize scripts or state after navigation.

Adding Astro’s built-in `<ClientRouter />` component:

* [intercepts page navigation](#client-side-navigation-process) and gives you considerable control over this process.
* extends and enhances some View Transition/Navigation API features.
* allows you to [configure fallback strategies](#fallback-control) for when [native browser support is lacking](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API#browser_compatibility).

However, as browser APIs and web standards evolve, using Astro’s `<ClientRouter />` for this additional functionality [will increasingly become unnecessary](https://astro.build/blog/future-of-astro-zero-js-view-transitions/). We recommend keeping up with the current state of browser APIs so you can [decide whether you still need Astro’s client-side routing](https://events-3bg.pages.dev/jotter/astro-view-transitions/) for the specific features you use.

## Enabling view transitions (SPA mode)

[Section titled “Enabling view transitions (SPA mode)”](#enabling-view-transitions-spa-mode)

Import and add the `<ClientRouter />` component to your common `<head>` or shared layout component. Astro will create default page animations based on the similarities between the old and new page, and will also provide fallback behavior for unsupported browsers.

The example below shows adding Astro’s default page navigation animations site-wide, including the default fallback control option for non-supporting browsers, by importing and adding this component to a `<CommonHead />` Astro component:

src/components/CommonHead.astro

```diff
---
+import { ClientRouter } from "astro:transitions";
---
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
<meta name="generator" content={Astro.generator} />


<!-- Primary Meta Tags -->
<title>{title}</title>
<meta name="title" content={title} />
<meta name="description" content={description} />


+<ClientRouter />
```

No other configuration is necessary to enable Astro’s default client-side navigation!

Use [transition directives](#transition-directives) or [override default client-side navigation](#preventing-client-side-navigation) on individual elements for finer control.

## Transition Directives

[Section titled “Transition Directives”](#transition-directives)

Astro will automatically assign corresponding elements found in both the old page and the new page a shared, unique `view-transition-name`. This pair of matching elements is inferred by both the type of element and its location in the DOM.

Use optional `transition:*` directives on page elements in your `.astro` components for finer control over the page transition behaviour during navigation.

* `transition:name`: Allows you to override Astro’s default element matching for old/new content animation and [specify a transition name](#naming-a-transition) to associate a pair of DOM elements.
* `transition:animate`: Allows you to override Astro’s default animation while replacing the old element with the new one by specifying an animation type. Use Astro’s [built-in animation directives](#built-in-animation-directives) or [create custom transition animations](#customizing-animations).
* `transition:persist`: Allows you to override Astro’s default replacing old elements for new ones and instead [persist components and HTML elements](#maintaining-state) when navigating to another page.

### Naming a transition

[Section titled “Naming a transition”](#naming-a-transition)

In some cases, you may want or need to identify the corresponding view transition elements yourself. You can specify a name for a pair of elements using the `transition:name` directive.

src/pages/old-page.astro

```astro
<aside transition:name="hero">
```

src/pages/new-page.astro

```astro
<aside transition:name="hero">
```

Note that the provided `transition:name` value can only be used once on each page. Set this manually when Astro can’t infer a proper name itself, or for more fine control over matching elements.

### Maintaining State

[Section titled “Maintaining State”](#maintaining-state)

**Added in:** `astro@2.10.0`

You can persist components and HTML elements (instead of replacing them) across page navigations using the `transition:persist` directive.

For example, the following `<video>` will continue to play as you navigate to another page that contains the same video element. This works for both forwards and backwards navigation.

src/components/Video.astro

```astro
<video controls muted autoplay transition:persist>
  <source
    src="https://ia804502.us.archive.org/33/items/GoldenGa1939_3/GoldenGa1939_3_512kb.mp4"
    type="video/mp4"
  />
</video>
```

You can also place the directive on an [Astro island](/en/concepts/islands/) (a UI framework component with a [`client:` directive](/en/reference/directives-reference/#client-directives)). If that component exists on the next page, the island from the old page **with its current state** will continue to be displayed, instead of replacing it with the island from the new page.

In the example below, the component’s internal state of the count will not be reset when navigating back and forth across pages that contain the `<Counter />` component with the `transition:persist` attribute.

components/Header.astro

```astro
<Counter client:load transition:persist initialCount={5} />
```

Known limitations

Not all state can be preserved in this way. The restart of CSS animations and the reload of iframes cannot be avoided during view transitions even when using `transition:persist`.

You can also [manually identify corresponding elements](#naming-a-transition) if the island/element is in a different component between the two pages.

src/pages/old-page.astro

```astro
<video
  controls
  muted
  autoplay
  transition:name="media-player"
  transition:persist
/>
```

src/pages/new-page.astro

```astro
<MyVideo
  controls
  muted
  autoplay
  transition:name="media-player"
  transition:persist
/>
```

As a convenient shorthand, `transition:persist` can alternatively take a transition name as a value.

src/pages/index.astro

```astro
<video controls muted autoplay transition:persist="media-player">
```

#### `transition:persist-props`

[Section titled “transition:persist-props”](#transitionpersist-props)

**Added in:** `astro@4.5.0`

This allows you to control whether or not an island’s props should be persisted upon navigation.

By default, when you add `transition:persist` to an island, the state is retained upon navigation, but your component will re-render with new props. This is useful, for example, when a component receives page-specific props such as the current page’s `title`.

You can override this behavior by setting `transition:persist-props` in addition to `transition:persist`. Adding this directive will keep an island’s existing props (not re-render with new values) in addition to maintaining its existing state.

### Built-in Animation Directives

[Section titled “Built-in Animation Directives”](#built-in-animation-directives)

Astro comes with a few built-in animations to override the default `fade` transition. Add the `transition:animate` directive to individual elements to customize the behavior of specific transitions.

* `fade` (default): An opinionated crossfade animation. The old content fades out and the new content fades in.
* `initial`: Opt out of Astro’s opinionated crossfade animation and use the browser’s default styling.
* `slide`: An animation where the old content slides out to the left and new content slides in from the right. On backwards navigation, the animations are the opposite.
* `none`: Disable the browser’s default animations. Use on a page’s `<html>` element to disable the default fade for every element on the page.

Combine directives for full control over your page animation. Set a page default on the `<html>` element, and override on any individual elements as desired.

The example below produces a slide animation for the body content while disabling the browser’s default fade animation for the rest of the page:

```astro
---
import CommonHead from "../components/CommonHead.astro";
---


<html transition:name="root" transition:animate="none">
  <head>
    <CommonHead />
  </head>
  <body>
    <header>
      ...
    </header>
    <!-- Override your page default on a single element -->
    <main transition:animate="slide">
      ...
    </main>
  </body>
</html>
```

### Customizing Animations

[Section titled “Customizing Animations”](#customizing-animations)

You can customize all aspects of a transition with any CSS animation properties.

To customize a built-in animation, first import the animation from `astro:transitions`, and then pass in customization options.

The example below customizes the duration of the built-in `fade` animation:

```astro
---
import { fade } from "astro:transitions";
---
<header transition:animate={fade({ duration: "0.4s" })}>
```

You can also define your own animations for use with `transition:animate` by defining both the forwards and backwards behavior, as well as new and old pages, according to the following types:

```ts
export interface TransitionAnimation {
  name: string; // The name of the keyframe
  delay?: number | string;
  duration?: number | string;
  easing?: string;
  fillMode?: string;
  direction?: string;
}


export interface TransitionAnimationPair {
  old: TransitionAnimation | TransitionAnimation[];
  new: TransitionAnimation | TransitionAnimation[];
}


export interface TransitionDirectionalAnimations {
  forwards: TransitionAnimationPair;
  backwards: TransitionAnimationPair;
}
```

The following example shows all the necessary properties to define a custom `bump` animation inside a `<style is:global>` tag in your root layout file:

src/layouts/Layout.astro

```astro
---
import { ClientRouter } from "astro:transitions";
---
<html lang="en">
  <head>
    <ClientRouter />
  </head>
  <body>
    <slot />
  </body>
</html>


<style is:global>
  @keyframes bump {
    0% {
      opacity: 0;
      transform: scale(1) translateX(200px);
    }
    50% {
      opacity: 0.5;
      transform: scale(1.1);
    }
    100% {
      opacity: 1;
      transform: scale(1) translateX(0);
    }
  }
</style>
```

The animation’s behavior must be defined in the frontmatter of every component using the animation:

src/pages/index.astro

```astro
---
const anim = {
  old: {
    name: "bump",
    duration: "0.5s",
    easing: "ease-in",
    direction: "reverse",
  },
  new: {
    name: "bump",
    duration: "0.5s",
    easing: "ease-in-out",
  },
};


const customTransition = {
  forwards: anim,
  backwards: anim,
};
---
<header transition:animate={customTransition}> ... </header>
```

You have great flexibility when defining custom animations. To achieve your desired result, you may wish to consider unusual combinations such as using different objects for forward and backward, or providing separate keyframe animations for old and new.

## Router control

[Section titled “Router control”](#router-control)

The `<ClientRouter />` router handles navigation by listening to:

* Clicks on `<a>` elements.
* Backwards and forwards navigation events.

The following options allow you to further control when navigation occurs within the router:

* `data-astro-reload`: an `<a>` tag attribute to [force a full-page navigation](#preventing-client-side-navigation)
* `data-astro-history="auto | push | replace"`: an `<a>` tag attribute to [control the browser’s history](#replace-entries-in-the-browser-history)
* `navigate(href, options)`: a method available to any client script or client component to [trigger navigation](#trigger-navigation)

### Preventing client-side navigation

[Section titled “Preventing client-side navigation”](#preventing-client-side-navigation)

There are some cases where you cannot navigate via client-side routing since both pages involved must use the `<ClientRouter />` router to prevent a full-page reload. You may also not want client-side routing on every navigation change and would prefer a traditional page navigation on select routes instead.

You can opt out of client-side routing on a per-link basis by adding the `data-astro-reload` attribute to any `<a>` or `<form>` tag. This attribute will override any existing `<ClientRouter />` component and instead trigger a browser refresh during navigation.

The following example shows preventing client-side routing when navigating to an article from the home page only. This still allows you to have animation on shared elements, such as a hero image, when navigating to the same page from an article listing page:

src/pages/index.astro

```astro
<a href="/articles/emperor-penguins" data-astro-reload>
```

src/pages/articles.astro

```astro
<a href="/articles/emperor-penguins">
```

Links with the `data-astro-reload` attribute will be ignored by the router and a full-page navigation will occur.

### Trigger navigation

[Section titled “Trigger navigation”](#trigger-navigation)

You can also trigger client-side navigation via events not normally listened to by the `<ClientRouter />` router using [`navigate()`](/en/reference/modules/astro-transitions/#navigate). This function from the `astro:transitions/client` module can be used in scripts, and in framework components that are hydrated with a [client directive](/en/reference/directives-reference/#client-directives).

The following example shows an Astro component that navigates a visitor to another page they select from a menu:

src/components/Form.astro

```astro
<script>
  import { navigate } from "astro:transitions/client";


  // Navigate to the selected option automatically.
  document.querySelector("select").onchange = (event) => {
    let href = event.target.value;
    navigate(href);
  };
</script>
<select>
  <option value="/play">Play</option>
  <option value="/blog">Blog</option>
  <option value="/about">About</option>
  <option value="/contact">Contact</option>
</select>
```

src/pages/index.astro

```astro
---
import Form from "../components/Form.astro";
import { ClientRouter } from "astro:transitions";
---
<html>
  <head>
    <ClientRouter />
  </head>
  <body>
    <Form />
  </body>
</html>
```

The following example implements the same using `navigate()` in a React `<Form />` component:

src/components/Form.jsx

```js
import { navigate } from "astro:transitions/client";


export default function Form() {
  return (
    <select onChange={(e) => navigate(e.target.value)}>
      <option value="/play">Play</option>
      <option value="/blog">Blog</option>
      <option value="/about">About</option>
      <option value="/contact">Contact</option>
    </select>
  );
}
```

The `<Form />` component can then be rendered on an Astro page that uses the `<ClientRouter />` router, with a client directive:

src/pages/index.astro

```astro
---
import Form from "../components/Form.jsx";
import { ClientRouter } from "astro:transitions";
---
<html>
  <head>
    <ClientRouter />
  </head>
  <body>
    <Form client:load />
  </body>
</html>
```

For backward and forward navigation through the browser history, you can combine `navigate()` with the built-in `history.back()`, `history.forward()` and `history.go()` functions of the browser. If `navigate()` is called during the server-side render of your component, it has no effect.

See the `astro:transitions` reference for more information about the [`navigate()` options](/en/reference/modules/astro-transitions/#navigate).

### Replace entries in the browser history

[Section titled “Replace entries in the browser history”](#replace-entries-in-the-browser-history)

Normally, each time you navigate, a new entry is written to the browser’s history. This allows navigation between pages using the browser’s `back` and `forward` buttons.

The `<ClientRouter />` router allows you to overwrite history entries by adding the `data-astro-history` attribute to any individual `<a>` tag.

The `data-astro-history` attribute can be set to the same three values as the [`history` option of the `navigate()` function](/en/reference/modules/astro-transitions/#history-option):

* `"push"`: the router will use `history.pushState` to create a new entry in the browser history.
* `"replace"`: the router will use `history.replaceState` to update the URL without adding a new entry into navigation.
* `"auto"` (default): the router will attempt `history.pushState`, but if the URL is not one that can be transitioned to, the current URL will remain with no changes to the browser history.

The following example navigates to the `/main` page but does not add a new entry to the browsing history. Instead, it reuses the current entry in the history (`/confirmation`) and overwrites it.

src/pages/confirmation.astro

```astro
<a href="/main" data-astro-history="replace">
```

This has the effect that if you go back from the `/main` page, the browser will not display the `/confirmation` page, but the page before it.

### Transitions with forms

[Section titled “Transitions with forms”](#transitions-with-forms)

**Added in:** `astro@4.0.0`

The `<ClientRouter />` router will trigger in-page transitions from `<form>` elements, supporting both `GET` and `POST` requests.

By default, Astro submits your form data as `multipart/form-data` when your `method` is set to `POST`. If you want to match the default behavior of web browsers, use the `enctype` attribute to submit your data encoded as `application/x-www-form-urlencoded`:

src/components/Form.astro

```astro
<form
  action="/contact"
  method="POST"
  enctype="application/x-www-form-urlencoded"
>
```

You can opt out of router transitions on any individual form using the `data-astro-reload` attribute:

src/components/Form.astro

```astro
<form action="/contact" data-astro-reload>
```

### Navigating with user input

[Section titled “Navigating with user input”](#navigating-with-user-input)

The `navigate()` API does not perform sanitization on the URLs passed to it. If you are using user input to determine the URL to navigate to, you should validate the input before passing it to `navigate()`.

For example, a `?redirect` query parameter could be used to navigate away from your site (`?redirect=http://example.com`) or to execute arbitrary code (`?redirect=javascript:alert('Evil code')`) if the value is not sanitized before use.

One way to implement this safely is to ensure only a set of known paths can be redirected to:

src/pages/index.astro

```astro
<script>
  import { navigate } from 'astro:transitions/client';


  const params = new URLSearchParams(window.location.search);
  const redirect = params.get('redirect');
  const allowedPaths = ['/home', '/about', '/contact'];


  if (allowedPaths.includes(redirect)) {
    navigate(redirect);
  }
</script>
```

The exact kind of sanitization you need will depend on your site and what you want to allow.

Consider enabling Astro’s [Content Security Policy feature](/en/reference/configuration-reference/#securitycsp) to help protect against cross-site scripting (XSS) risks if using user input with the `navigate()` API.

## Fallback control

[Section titled “Fallback control”](#fallback-control)

The `<ClientRouter />` router works best in browsers that support View Transitions (i.e. Chromium browsers), but also includes default fallback support for other browsers. Even if the browser does not support the View Transitions API, Astro’s client router can still provide in-browser navigation using one of the fallback options.

Depending on browser support, you may need to explicitly set the `name` or `animate` [transition directives](#transition-directives) on the elements you wish to animate for a comparable experience across all browsers:

src/pages/about.astro

```astro
---
import Layout from "../layouts/LayoutUsingClientRouter.astro";
---
<title transition:animate="fade">About my site</title>
```

You can override Astro’s default fallback support by adding a `fallback` property on the `<ClientRouter />` component and setting it to `swap` or `none`:

* `animate` (default, recommended): Astro will simulate view transitions using custom attributes before updating page content.
* `swap`: Astro will not attempt to animate the page. Instead, the old page will be immediately replaced by the new one.
* `none`: Astro will not do any animated page transitions at all. Instead, you will get full page navigation in non-supporting browsers.

```astro
---
import { ClientRouter } from "astro:transitions";
---
<title>My site</title>


<ClientRouter fallback="swap" />
```

Known limitations

The `initial` browser animation is not simulated by Astro. So any element using this animation will not currently be animated.

## Client-side navigation process

[Section titled “Client-side navigation process”](#client-side-navigation-process)

When using the `<ClientRouter />` router, the following steps occur to produce Astro’s client-side navigation:

1. A visitor to your site triggers navigation by any of the following actions:

   * Clicking an `<a>` tag linking internally to another page on your site.
   * Clicking the back button.
   * Clicking the forward button.

2. The router starts fetching the next page.

3. The router adds the `data-astro-transition` attribute to the HTML element with a value of `"forward"` or `"back"` as appropriate.

4. The router calls `document.startViewTransition`. This triggers the browser’s own [view transition process](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API/Using#the_view_transition_process). Importantly, the browser screenshots the current state of the page.

5. Inside the `startViewTransition` callback, the router performs a **swap**, which consists of the following sequence of events:

   * The contents of the `<head>` are swapped out, with some elements kept:

     * Stylesheet DOM nodes are left in if they exist on the new page, to prevent FOUC.
     * Scripts are left in if they exist on the new page.
     * Any other head elements with `transition:persist` are left in if there is a corresponding element in the new page.

   * The `<body>` is completely replaced with the new page’s body.

   * Elements marked `transition:persist` are moved over to the new DOM if they exist on the new page.

   * Scroll position is restored if necessary.

   * The `astro:after-swap` event is triggered on the `document`. This is the end of the **swap** process.

6. The router waits for any new stylesheets to load before resolving the transition.

7. The router executes any new scripts added to the page.

8. The `astro:page-load` event fires. This is the end of the navigation process.

## Script behavior with view transitions

[Section titled “Script behavior with view transitions”](#script-behavior-with-view-transitions)

When you add view transitions to an existing Astro project, some of your scripts may no longer re-run after page navigation like they did with full-page browser refreshes. Use the following information to ensure that your scripts execute as expected.

### Script order

[Section titled “Script order”](#script-order)

When navigating between pages with the `<ClientRouter />` component, scripts are run in sequential order to match browser behavior.

### Script re-execution

[Section titled “Script re-execution”](#script-re-execution)

[Bundled module scripts](/en/guides/client-side-scripts/#script-processing), which are the default scripts in Astro, are only ever executed once. After initial execution they will be ignored, even if the script exists on the new page after a transition.

Unlike bundled module scripts, [inline scripts](/en/guides/client-side-scripts/#unprocessed-scripts) have the potential to be re-executed during a user’s visit to a site if they exist on a page that is visited multiple times. Inline scripts might also re-execute when a visitor navigates to a page without the script, and then back to one with the script.

With view transitions, some scripts may no longer re-run after page navigation like they do with full-page browser refreshes. There are several [events during client-side routing that you can listen for](#lifecycle-events), and fire events when they occur. You can wrap an existing script in an event listener to ensure it runs at the proper time in the navigation cycle.

The following example wraps a script for a mobile “hamburger” menu in an event listener for `astro:page-load` which runs at the end of page navigation to make the menu responsive to being clicked after navigating to a new page:

src/scripts/menu.js

```diff
+document.addEventListener("astro:page-load", () => {
  document.querySelector(".hamburger").addEventListener("click", () => {
    document.querySelector(".nav-links").classList.toggle("expanded");
  });
+});
```

The following example shows a function that runs in response to the `astro:after-swap` event, which happens immediately after the new page has replaced the old page and before the DOM elements are painted to the screen. This avoids a flash of light mode theme after page navigation by checking and, if necessary, setting the dark mode theme before the new page is rendered:

src/components/ThemeToggle.astro

```astro
<script is:inline>
  function applyTheme() {
    localStorage.theme === "dark"
      ? document.documentElement.classList.add("dark")
      : document.documentElement.classList.remove("dark");
  }


  document.addEventListener("astro:after-swap", applyTheme);
  applyTheme();
</script>
```

#### `data-astro-rerun`

[Section titled “data-astro-rerun”](#data-astro-rerun)

**Added in:** `astro@4.5.0`

To force inline scripts to re-execute after every transition, add the `data-astro-rerun` property. Adding any attribute to a script also implicitly adds `is:inline`, so this is only available for scripts that are not bundled and processed by Astro.

```astro
<script is:inline data-astro-rerun>...</script>
```

To ensure that a script runs every time a page is loaded during client-side navigation, it should be executed by a [lifecycle event](#lifecycle-events). For example, event listeners for `DOMContentLoaded` can be replaced by the [`astro:page-load`](/en/guides/view-transitions/#astropage-load) lifecycle event.

If you have code that sets up a global state in an inline script, this state will need to take into account that the script might execute more than once. Check for the global state in your `<script>` tag, and conditionally execute your code where possible. This works because `window` is preserved.

```astro
<script is:inline>
  if (!window.SomeGlobal) {
    window.SomeGlobal = {};
  }
</script>
```

## Lifecycle events

[Section titled “Lifecycle events”](#lifecycle-events)

The `<ClientRouter />` router fires a number of events on the `document` during navigation. These events provide hooks into the lifecycle of navigation, allowing you to do things like show indicators that a new page is loading, override default behavior, and restore state as navigation is completing.

The navigation process involves a **preparation** phase, when new content is loaded; a **DOM swap** phase, where the old page’s content is replaced by the new page’s content; and a **completion** phase where scripts are executed, loading is reported as completed and clean-up work is carried out.

Astro’s View Transition API lifecycle events in order are:

* [`astro:before-preparation`](#astrobefore-preparation)
* [`astro:after-preparation`](#astroafter-preparation)
* [`astro:before-swap`](#astrobefore-swap)
* [`astro:after-swap`](#astroafter-swap)
* [`astro:page-load`](#astropage-load)

Tip

`before-` events allow you to influence and modify actions that are about to take place, and `after-` events are notifications that a phase is complete.

While some actions can be triggered during any event, some tasks can only be performed during a specific event for best results, such as displaying a loading spinner before preparation or overriding animation pairs before swapping content.

### `astro:before-preparation`

[Section titled “astro:before-preparation”](#astrobefore-preparation)

**Added in:** `astro@3.6.0`

An event that fires at the beginning of the preparation phase, after navigation has started (e.g. after the user has clicked a link), but before content is loaded.

This event is used:

* To do something before loading has started, such as showing a loading spinner.
* To alter loading, such as loading content you’ve defined in a template rather than from the external URL.
* To change the `direction` of the navigation (which is usually either `forward` or `backward`) for custom animation.

Here is an example of using the `astro:before-preparation` event to load a spinner before the content is loaded and stop it immediately after loading. Note that using the [`loader` callback](/en/reference/modules/astro-transitions/#loader) in this way allows asynchronous execution of code.

```astro
<script is:inline>
  document.addEventListener("astro:before-preparation", (event) => {
    const originalLoader = event.loader;
    event.loader = async function () {
      const { startSpinner } = await import("./spinner.js");
      const stop = startSpinner();
      await originalLoader();
      stop();
    };
  });
</script>
```

### `astro:after-preparation`

[Section titled “astro:after-preparation”](#astroafter-preparation)

**Added in:** `astro@3.6.0`

An event that fires at the end of the preparation phase, after the new page’s content has been loaded and parsed into a document. This event occurs before the view transitions phase.

This example uses the `astro:before-preparation` event to start a loading indicator and the `astro:after-preparation` event to stop it:

```astro
<script is:inline>
  document.addEventListener("astro:before-preparation", () => {
    document.querySelector("#loading").classList.add("show");
  });
  document.addEventListener("astro:after-preparation", () => {
    document.querySelector("#loading").classList.remove("show");
  });
</script>
```

This is a simpler version of loading a spinner than the example shown above: if all of the listener’s code can be executed synchronously, there is no need to hook into the `loader` callback.

### `astro:before-swap`

[Section titled “astro:before-swap”](#astrobefore-swap)

**Added in:** `astro@3.6.0`

An event that fires before the new document (which is populated during the preparation phase) replaces the current document. This event occurs inside of the view transition, where the user is still seeing a snapshot of the old page.

This event can be used to make changes before the swap occurs. The `newDocument` property on the event represents the incoming document. Here is an example of ensuring the browser’s light or dark mode preference in `localStorage` is carried over to the new page:

```astro
<script>
  document.addEventListener("astro:before-swap", (event) => {
    event.newDocument.documentElement.dataset.theme =
      localStorage.getItem("darkMode") ? "dark" : "light";
  });
</script>
```

The `astro:before-swap` event can also be used to change the *implementation* of the swap. The default swap implementation diffs head content, moves **persistent** elements from the old document to the `newDocument`, and then replaces the entire `body` with the body of the new document.

At this point of the lifecycle, you could choose to define your own swap implementation, for example to diff the entire contents of the existing document (which some other routers do):

```astro
<script is:inline>
  document.addEventListener("astro:before-swap", (event) => {
    event.swap = () => {
      diff(document, event.newDocument);
    };
  });
</script>
```

#### Building a custom swap function

[Section titled “Building a custom swap function”](#building-a-custom-swap-function)

**Added in:** `astro@4.15.0`

The [`swapFunctions` object](/en/reference/modules/astro-transitions/#swapfunctions) of the `astro:transitions/client` module provides five utility functions that handle specific swap-related tasks, including handling document attributes, page elements, and script execution. These functions can be used directly to define a custom swap implementation.

The following example demonstrates how to use these functions to recreate Astro’s built-in swap implementation:

```astro
<script>
  import { swapFunctions } from "astro:transitions/client";


  // substitutes `window.document` with `doc`
  function mySwap(doc: Document) {
    swapFunctions.deselectScripts(doc);
    swapFunctions.swapRootAttributes(doc);
    swapFunctions.swapHeadElements(doc);
    const restoreFocusFunction = swapFunctions.saveFocus();
    swapFunctions.swapBodyElement(doc.body, document.body);
    restoreFocusFunction();
  }


  document.addEventListener("astro:before-swap", (event) => {
    event.swap = () => mySwap(event.newDocument);
  });
<script>
```

Custom swap implementations can start with this template and add or replace individual steps with custom logic as needed.

### `astro:after-swap`

[Section titled “astro:after-swap”](#astroafter-swap)

An event that fires immediately after the new page replaces the old page. You can listen to this event on the `document` and trigger actions that will occur before the new page’s DOM elements render and scripts run.

This event, when listened to on the **outgoing page**, is useful to pass along and restore any state on the DOM that needs to transfer over to the new page.

This is the latest point in the lifecycle where it is still safe to, for example, add a dark mode class name (`<html class="dark-mode">`), though you may wish to do so in an earlier event.

The `astro:after-swap` event occurs immediately after the browser history has been updated and the scroll position has been set. Therefore, one use of targeting this event is to override the default scroll restore for history navigation. The following example resets the horizontal and vertical scroll position to the top left corner of the page for each navigation.

```js
document.addEventListener("astro:after-swap", () =>
  window.scrollTo({ left: 0, top: 0, behavior: "instant" }),
);
```

### `astro:page-load`

[Section titled “astro:page-load”](#astropage-load)

An event that fires at the end of page navigation, after the new page is visible to the user and blocking styles and scripts are loaded. You can listen to this event on the `document`.

The `<ClientRouter />` component fires this event both on initial page navigation for a pre-rendered page and on any subsequent navigation, either forwards or backwards.

You can use this event to run code on every page navigation, for example to set up event listeners that would otherwise be lost during navigation.

```astro
<script>
  document.addEventListener("astro:page-load", () => {
    // This runs on first page load and after every navigation.
    setupStuff(); // e.g. add event listeners
  });
</script>
```

## Accessibility

[Section titled “Accessibility”](#accessibility)

Enabling client-side routing and animating page transitions both come with accessibility challenges, and Astro aims to make sites opting in to View Transitions as accessible-by-default as possible.

### Route announcement

[Section titled “Route announcement”](#route-announcement)

**Added in:** `astro@3.2.0`

The `<ClientRouter />` component includes a route announcer for page navigation during client-side routing. No configuration or action is needed to enable this.

Assistive technologies let visitors know that the page has changed by announcing the new page title after navigation. When using server-side routing with traditional full-page browser refreshes, this happens by default after the new page loads. In client-side routing, the `<ClientRouter />` component performs this action.

To add route announcement to client-side routing, the component adds an element to the new page with the `aria-live` attribute set to `assertive`. This tells AT (assistive technology) to announce immediately. The component also checks for the following, in priority order, to determine the announcement text:

* The `<title>`, if it exists.
* The first `<h1>` it finds.
* The `pathname` of the page.

We strongly recommend you always include a `<title>` in each page for accessibility.

### `prefers-reduced-motion`

[Section titled “prefers-reduced-motion”](#prefers-reduced-motion)

Astro’s `<ClientRouter />` component includes a CSS media query that disables *all* view transition animations, including fallback animation, whenever the [`prefers-reduced-motion`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) setting is detected. Instead, the browser will simply swap the DOM elements without an animation.

