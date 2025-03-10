---
id: api-information
---

# General API info
You can add basics such as the title, introductory text, base URL and authentication information in your `.scribe.config.js` config file.

## Title
To set the HTML `<title>` for the generated docs, use the `title` key. This title will also be used in the Postman collection and OpenAPI spec.

```js title=.scribe.config.js
  title: 'The SideProject API',
```

## Description and introductory text
You can add a description of your API using the `description` key. This description will be displayed in the docs' "Introduction" section, and in the Postman collection and OpenAPI spec.

The `introText` key is where you'll set the text shown in the "Introduction" section of your docs (after the `description`).

Markdown and HTML are also supported (see [HTML helpers](../reference/html))

```js title=.scribe.config.js
  description: 'Start (and never finish) side projects with this API.',
  introText: `
This documentation will provide all the information you need to work with our API.

<aside>
As you scroll, you'll see code examples for working with the API in different programming languages in the dark area to the right (or as part of the content on mobile).
You can switch the language used with the tabs at the top right (or from the nav menu at the top left on mobile).
</aside>
`,
```

![](/img/screenshots/docs-intro.png)

## Base URL
You can set the base URL to be displayed in your docs (also known as the _display URL_) with the `baseUrl` key. For example, setting the `baseUrl` to this:

```js title=.scribe.config.js
  baseUrl: 'http://sideprojects.knuckles.wtf',
```

...means that `http://sideprojects.knuckles.wtf` will be shown in the generated docs, even if you ran the `generate` command on localhost or in CI.

:::note
You can also set the URL used in the API tester (Try It Out) and for response calls with the `tryItOut.baseUrl` and `responseCalls.baseUrl` keys.
:::

## Logo
Maybe you've got a pretty logo for your API or company, and you'd like to display that on your documentation page. No worries! To add a logo, set the `logo` key in `.scribe.config.js` to the path of the logo. Here are your options:

- To point to an image on an external public URL, set `logo` to that URL.
   ```js
   logo: 'http://your-company/logo.png',
   ```
- To point to an image in your codebase, pass in the path to the image relative to your `outputPath` directory (by default, `public/docs`). 

  For example, if your logo is in `public/images`:
   ```js
   logo: '../img/logo.png',
   ```
- If you don't want a logo, set `logo` to `false`.

## Authentication
You can add authentication information for your API using the `auth` section in `.scribe.config.js`. 

:::important
Scribe uses the auth information you specify for four things:
  - Generating an "Authentication" section in your docs
  - Adding auth information to the Postman collection and OpenAPI spec
  - Adding authentication parameters to your example requests for endpoints that use authentication
  - Adding the necessary auth parameters with the specified value to response calls for endpoints that use authentication
:::

Here's how you'd configure auth with a query parameter named `apiKey`:

```js title=.scribe.config.js
module.exports = {
  // ...
  auth: {
    enabled: true,
    default: false,
    in: 'query',
    name: 'apiKey',
    useValue: () => process.env.SCRIBE_API_KEY,
    placeholder: 'YOUR-API-KEY',
    extraInfo: 'You can retrieve your key by going to settings and clicking <b>Generate API key</b>.',
  },
};
```

If `apiKey` were to be a body parameter, the config would be same. Just set `in` to `'body'`.

Here's an example with a bearer token (also applies to basic auth, if you change `in` to `'basic'`):


```js title=.scribe.config.js
module.exports = {
  // ...
  auth: {
    enabled: true,
    default: false,
    in: 'bearer',
    name: 'hahaha', // <--- This value is ignored for bearer and basic auth
    useValue: () => process.env.SCRIBE_AUTH_KEY,
    placeholder: '{ACCESS_TOKEN}',
    extraInfo: 'You can retrieve your token by visiting your dashboard and clicking <b>Generate API token</b>.',
  },
};
```

And here's an example with a custom header:


```js title=.scribe.config.js
module.exports = {
  // ...
  auth: {
    enabled: true,
    default: false,
    in: 'header',
    name: 'Api-Key', // <--- The name of the header
    useValue: () => process.env.SCRIBE_AUTH_KEY,
    placeholder: 'YOUR-API-KEY',
    extraInfo: 'You can retrieve your token by visiting your dashboard and clicking <b>Generate API token</b>.',
  },
};
```
The `default` field is the default behaviour of our API. If your endpoints are authenticated by default, set this to `true`, then use `@unauthenticated` on the method doc block if you need to turn off auth for specific endpoints. If your endpoints are open by default, leave this as `false`, then use `@authenticated` on the method doc block if you need to turn on auth for specific endpoints.

You can set whatever you want as the `extraInfo`. A good idea would be to tell your users where to get their auth key. 

The `useValue` field is only used by Scribe for response calls. It won't be included in the generated output or examples. You can set it to a static value, or a function to be called to fetch that value
The `placeholder` is the opposite of `useValue`. It will be used only as a placeholder in the generated example requests.

For more information, see the [reference documentation on the auth section](../reference/config#auth).


