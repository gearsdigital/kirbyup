# Extend Vite With `kirbyup.config.js`

Since kirbyup uses Vite under the hood, you might want to add Vite plugins or customize the Vite configuration. You can do this by creating a `kirbyup.config.js` or `kirbyup.config.ts` file in the root of your project:

```js
import { defineConfig } from 'kirbyup/config'

export default defineConfig({
  alias: {
    // Custom aliases
  },
  vite: {
    // Custom Vite options to be merged with the default config
  }
})
```

::: tip
To target build environments, like `development` or `production`, use [`import.meta.env.DEV` and `import.meta.env.PROD`](/guide/environment-variables) to conditionally execute code.
:::

## Configuration Options

### `alias`

When aliasing to file system paths, always use absolute paths. Relative alias values will be used as-is and will not be resolved into file system paths.

### `vite`

You can build upon the defaults kirbup uses and extend the Vite configuration with custom plugins, etc.

For a complete list of options, take a look at the [Vite configuration options](https://vitejs.dev/config/).

## Example

Let's say a plugin has the following requirements:

- Set up an alias for the path `@/` to resolve to Kirby's Panel source directory.
- Extend the Vite configuration with a plugin to polyfill Node.js built-in modules. This is needed for an imaginary library we're importing in a component (which would break in the browser without the polyfills).

To achieve this, create a `kirbyup.config.js` file in the root of your project with the following content:

```js
import { resolve } from 'node:path'
import { fileURLToPath } from 'node:url'
import { defineConfig } from 'kirbyup/config'
import { nodePolyfills } from 'vite-plugin-node-polyfills'

const currentDir = fileURLToPath(new URL('.', import.meta.url))

export default defineConfig({
  vite: {
    resolve: {
      alias: {
        '@/': `${resolve(currentDir, 'kirby/panel/src')}/`,
      },
    },
    plugins: [
      nodePolyfills()
    ],
  },
})
```
