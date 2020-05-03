<h1 align="center">🏗️ Nuxt Composition API</h1>
<p align="center">Composition API hooks for Nuxt</p>

<p align="center">
<a href="https://npmjs.com/package/nuxt-composition-api">
    <img alt="" src="https://img.shields.io/npm/v/nuxt-composition-api/latest.svg?style=flat-square">
</a>
<a href="https://bundlephobia.com/result?p=nuxt-composition-api">
    <img alt="" src="https://img.shields.io/bundlephobia/minzip/nuxt-composition-api?style=flat-square">
</a>
<a href="https://npmjs.com/package/nuxt-composition-api">
    <img alt="" src="https://img.shields.io/npm/dt/nuxt-composition-api.svg?style=flat-square">
</a>
<a href="https://lgtm.com/projects/g/danielroe/nuxt-composition-api">
    <img alt="" src="https://img.shields.io/lgtm/alerts/github/danielroe/nuxt-composition-api?style=flat-square">
</a>
<a href="https://lgtm.com/projects/g/danielroe/nuxt-composition-api">
    <img alt="" src="https://img.shields.io/lgtm/grade/javascript/github/danielroe/nuxt-composition-api?style=flat-square">
</a>
<a href="https://david-dm.org/danielroe/nuxt-composition-api">
    <img alt="" src="https://img.shields.io/david/danielroe/nuxt-composition-api.svg?style=flat-square">
</a>
</p>

<div align="center">

[Live demo](https://composition-api.now.sh) · [CodeSandbox](https://codesandbox.io/s/github/danielroe/nuxt-composition-api/tree/master/example)

</div>

> `nuxt-composition-api` provides a way to use the Vue 3 Composition API in with Nuxt-specific features.

**Note**: the main aim is to allow experimentation and feedback before the final release of Nuxt 3. Think carefully before using this package in production.

## Features

- 🏃 **Fetch**: Support for the new Nuxt `fetch()` in v2.12+
- ℹ️ **Context**: Easy access to `router`, `app`, `store` within `setup()`
- 📝 **SSR support**: Allows using the Composition API with SSR
- 💪 **TypeScript**: Written in TypeScript

## Quick Start

1. First, install `nuxt-composition-api`:

   ```bash
   yarn add nuxt-composition-api

   # or npm

   npm install nuxt-composition-api --save
   ```

2. Enable the module in your `nuxt.config.js`.

   ```
   {
     buildModules: [
       'nuxt-composition-api'
     ]
   }
   ```

   Note that [using `buildModules`](https://nuxtjs.org/api/configuration-modules#-code-buildmodules-code-) requires Nuxt >= 2.9.

The module automatically installs [`@vue/composition-api`](https://github.com/vuejs/composition-api) as a plugin, so you do not need to enable it separately.

## Hooks

### useFetch

Versions of Nuxt newer than v2.12 support a [custom hook called `fetch`](https://nuxtjs.org/api/pages-fetch/) that allows server-side and client-side asynchronous data-fetching.

You can access this with this package as follows:

```ts
import { defineComponent, ref, useFetch } from 'nuxt-composition-api'
import axios from 'axios'

export default defineComponent({
  setup() {
    const name = ref('')

    useFetch(async () => {
      name.value = await axios.get('https://myapi.com/name')
    })

    return { name }
  },
})
```

**Note**: `useFetch` must be called synchronously within `setup()`. Any changes made to component data - that is, to properties _returned_ from `setup()` - will be sent to the client and directly loaded. Other side-effects of `useFetch` hook will not be persisted.

### withContext

You can access the Nuxt context more easily using `withContext`, which will immediately call the callback and pass it the Nuxt context.

```ts
import { defineComponent, ref, withContext } from 'nuxt-composition-api'

export default defineComponent({
  setup() {
    withContext(({ store }) => {
      store.dispatch('myAction')
    })
  },
})
```

### Additional `@vue/composition-api` functions

For convenience, this package also exports the [`@vue/composition-api`](https://github.com/vuejs/composition-api) methods and hooks, so you can import directly from `nuxt-composition-api`.

## Contributors

Contributions are very welcome.

1. Clone this repo

   ```bash
   git clone git@github.com:danielroe/nuxt-composition-api.git
   ```

2. Install dependencies and build project

   ```bash
   yarn
   # Compile library and watch for changes
   yarn watch
   # Start a test Nuxt fixture with hot reloading
   node test/start-fixture.js
   ```

**Tip:** You can also use `yarn link` to test the module locally with your Nuxt project.

## License

[MIT License](./LICENSE) - Copyright &copy; Daniel Roe
