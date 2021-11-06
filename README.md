# Welcome to Astro Script 👋

Astro Script is a simple component to make it easier to manage script tags in your Astro components and pages.

## Installing the plugin

Once you have setup your Astro project simply run the following command:

```bash
# yarn
yarn add astro-script

# npm
npm i astro-script
```

You can then import the component like this:

```astro
---
import { AstroScript } from "astro-script";
// or import Spa from 'astro-spa/Spa.astro'
---

<AstroScript src="/script.js" />
```

And that's it, you're now ready to go!

## How to add scripts

There are two ways to add scripts to **AstroScript**. You can either add them to the `src` prop or put them inside the `<AstroScript></AstroScript>` tags. The following example shows both ways:

```astro
---
import { AstroScript } from "astro-script";
---

// Add script to src prop
<AstroScript src="/script.js" />
<AstroScript src={Astro.resolve("../relative/path/to/script.js")} />

// Add script to <AstroScript />
<AstroScript>
  <script> console.log("Hello World"); </script>
</AstroScript>
```

The src prop also supports an array of paths. The following example shows how to add multiple scripts:

```astro
---
import { AstroScript } from "astro-script";
---

// Add multiple scripts to src prop
<AstroScript src={["/script1.js", "/script2.js"]} />
```

You can also pass multiple script tags inside the `<AstroScript></AstroScript>` tag. The following example shows how to add multiple scripts:

```astro
---
import { AstroScript } from "astro-script";
---

// Add multiple scripts to <AstroScript />
<AstroScript>
  <script> console.log("Hello World"); </script>
  <script> console.log("Hello World"); </script>
</AstroScript>
```

AstroScript also works with external scripts. The following example shows how to add an external script:

```astro
---
import { AstroScript } from "astro-script";
---

// Add external script to src prop
<AstroScript src="https://example.com/script.js" />
<AstroScript src={["https://example.com/script1.js", "https://example.com/script2.js"]} />
```

## How `AstroScript` works

If the `src` prop is present, **AstroScript** will first fetch the external scripts and read the internal scripts from the filesystem referenced in the `src` prop. It'll then merge the scripts and minify & inline them if enabled. If `inline` is set to `false`, it'll create a `.js` file at `${publicPath}/astro-script`. The name of the file will a 8 digit truncated hash of the scripts.

If you have passed the scripts inside the `<AstroScript></AstroScript>` tag, **AstroScript** will read the scripts from the `<script>` tags and merge them & minify them if enabled. Any elements other than `<script>` tags will be ignored.

By default, **AstroScript** won't run in development mode. If the `src` prop is present, it'll loop through them and create a `script` tag for each script. And if you have passed the scripts inside the `<AstroScript></AstroScript>` tag, the scripts will be left as is.

## `<script hoist>` vs `<AstroScript>`

Starting with `v0.20.2`, **Astro** provides the ability to hoist your script tags to the top of the page and bundle them. On the other hand, **AstroScript** will not hoist the scripts to the top of the page. Instead it will allow you to bundle them at the exact point you want them to be bundled. It also allows you to bundle external scripts.

## Supported Component Props

### src

Type: `string` or `string[]`

Default: `undefined`

The source(s) of the script(s). You can resolve the sources using the `Astro.resolve` API or you can pass a path relative to the public folder. If the path is relative to the public folder, it must start with `/`.

### inline

Type: `boolean`

Default: `true`

Whether or not the scripts should be inline.

### minify

Type: `boolean`

Default: `true`

Whether or not the scripts should be minified. If true the scripts will be minified using `terser`.

### minifyOptions

Type: `MinifyOptions` (check the link below)

Default: `{}`

The minify configuration options supported by `terser`. For more info [check the minify options supported by terser](https://github.com/terser/terser#minify-options).

### publicPath

Type: `string`

Default: `public`

The public path of the project.

### dev

Type: `boolean`

Default: `import.meta.env !== "development"`

Whether or not the optimizations will be performed in development mode.

## Warnings

- If the `src` prop is present, any `<script>` tags inside the `<AstroScript></AstroScript>` tag will be ignored.
- Files resolved with `Astro.resolve` can't have extensions other than `.js`. Because **AstroScript** doesn't have ability to perform transformation of `.jsx`, `.ts`, `.tsx`, `.mjs`, `.cjs` files.
- You may encounter issues with HMR if you have set `dev` to `true`.
- If you have set `dev` to true you should add the `${publicPath}/astro-script` directory to your `.gitignore` file. Otherwise, you may end up pushing lots of redundant files to your Github repo and CI.
