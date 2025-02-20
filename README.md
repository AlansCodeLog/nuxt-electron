<p align="center">
  <img width="170" src="https://github.com/caoxiemeihao/nuxt-electron/raw/main/logo.svg?raw=true">
</p>

# Nuxt Electron

[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]
[![License][license-src]][license-href]

> Integrate Nuxt and Electron

## Features

- 📦 Out of the box
- 🔥 Hot restart <sub><sup>(Main process)</sup></sub>
- 🚀 Hot reload <sub><sup>(Preload script)</sup></sub>

## Quick Setup

1. Add the following dependency to your project

```sh
# Using pnpm
pnpm add -D nuxt-electron vite-plugin-electron vite-plugin-electron-renderer electron electron-builder
```

```sh
# Using yarn
yarn add --dev nuxt-electron vite-plugin-electron vite-plugin-electron-renderer electron electron-builder
```

```sh
# Using npm
npm install --save-dev nuxt-electron vite-plugin-electron vite-plugin-electron-renderer electron electron-builder
```

2. Add `nuxt-electron` to the `modules` section of `nuxt.config.ts`

```ts
export default defineNuxtConfig({
  modules: ['nuxt-electron'],
  electron: {
    build: [
      {
        // Main-Process entry file of the Electron App.
        entry: 'electron/main.ts',
      },
    ],
  },
})
```

3. Create the `electron/main.ts` file and type the following code

```ts
import { app, BrowserWindow } from 'electron'

app.whenReady().then(() => {
  if (process.env.VITE_DEV_SERVER_URL) {
		win.loadURL(process.env.VITE_DEV_SERVER_URL)
	} else {
		win.loadFile(path.join(__dirname, '../dist-electron', '.output/index.html'))
	}
})
```

4. Add the `main` entry to `package.json`

```diff
{
+ "main": "dist-electron/main.js"
}
```

That's it! You can now use Electron in your Nuxt app ✨

## Electron Options

> This is based on the `vite-plugin-electron`, see the **[Documents](https://github.com/electron-vite/vite-plugin-electron)** for more detailed options

```ts
export interface ElectronOptions {
  /**
   * `build` can specify multiple entry builds, which can be Main process, Preload scripts, Worker process, etc.
   * 
   * @example
   * 
   * ```js
   * export default defineNuxtConfig({
   *   modules: ['nuxt-electron'],
   *   electron: {
   *     build: [
   *       {
   *         // Main-Process entry file of the Electron App.
   *         entry: 'electron/main.ts',
   *       },
   *     ],
   *   },
   * })
   * ```
   */
  build: import('vite-plugin-electron').Configuration[],
  /**
   * @see https://github.com/electron-vite/vite-plugin-electron-renderer
   */
  renderer?: Parameters<typeof import('vite-plugin-electron-renderer').default>[0]
  /**
   * nuxt-electron will modify some options by default
   * 
   * @defaultValue
   * 
   * ```js
   * export default defineNuxtConfig({
   *   ssr: false,
   *   app: {
   *     baseURL: './',
   *     buildAssetsDir: '/',
   *   },
   *   runtimeConfig: {
   *     app: {
   *       baseURL: './',
   *       buildAssetsDir: '/',
   *     },
   *   },
   * })
   * ```
   */
  disableDefaultOptions?: boolean
}
```

## Recommend Structure

Let's use the official [nuxt-starter-v3](https://codeload.github.com/nuxt/starter/tar.gz/refs/heads/v3) template as an example

```diff
+ ├─┬ electron
+ │ └── main.ts
  ├─┬ public
  │ └── favicon.ico
  ├── .gitignore
  ├── .npmrc
  ├── index.html
  ├── app.vue
  ├── nuxt.config.ts
  ├── package.json
  ├── README.md
  └── tsconfig.json
```

## [Examples](https://github.com/caoxiemeihao/nuxt-electron/tree/main/examples)

- [quick-start](https://github.com/caoxiemeihao/nuxt-electron/tree/main/examples/quick-start)
- [nuxt-electron-trpc-prisma](https://github.com/gurvancampion/nuxt-electron-trpc-prisma)

## Notes
By default, we force the App to run in SPA mode since we don't need SSR for desktop apps

If you want to fully customize the default behavior, you can disable it by `disableDefaultOptions`

<!-- Badges -->
[npm-version-src]: https://img.shields.io/npm/v/nuxt-electron/latest.svg?style=flat&colorA=18181B&colorB=28CF8D
[npm-version-href]: https://npmjs.com/package/nuxt-electron

[npm-downloads-src]: https://img.shields.io/npm/dm/nuxt-electron.svg?style=flat&colorA=18181B&colorB=28CF8D
[npm-downloads-href]: https://npmjs.com/package/nuxt-electron

[license-src]: https://img.shields.io/npm/l/nuxt-electron.svg?style=flat&colorA=18181B&colorB=28CF8D
[license-href]: https://npmjs.com/package/nuxt-electron

## TODO

- [ ] write test
