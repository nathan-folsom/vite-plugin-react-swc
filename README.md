# vite-plugin-swc-react-refresh [![npm](https://img.shields.io/npm/v/vite-plugin-swc-react-refresh)](https://www.npmjs.com/package/vite-plugin-swc-react-refresh)

Use the versatility of [SWC](https://swc.rs/) for development and the maturity of [esbuild](https://esbuild.github.io/) for production.

- ✅ A fast Fast Refresh (~20x faster than Babel)
- ✅ Enable [automatic JSX runtime](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
- ❌ Don't check for consistent components exports. See [this section](#consistent-components-exports)

## Installation

```sh
npm i -D vite-plugin-swc-react-refresh
```

## Usage

```ts
import { defineConfig } from "vite";
import { swcReactRefresh } from "vite-plugin-swc-react-refresh";

export default defineConfig({
  plugins: [swcReactRefresh()],
});
```

## Caveats

This plugin is only used in development and aims to be kept simple to enable good performances and be transpiler agnostic. Here is the list of non-configurable options that impact runtime behaviour:

- [useDefineForClassFields](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#the-usedefineforclassfields-flag-and-the-declare-property-modifier) is always activated, as this matches the current ECMAScript spec
- `jsx runtime` is always `automatic`
- In development:
  - esbuild is disabled, so the [esbuild configuration](https://vitejs.dev/config/shared-options.html#esbuild) has no effect
  - `target` is `es2020`
  - JS files are not transformed
  - tsconfig is not resolved, so properties other than the ones listed above behaves like TS defaults

## Consistent components exports

For React refresh to work, your file should only export React components. The best explanation I've read is the one from the [Gatsby docs](https://www.gatsbyjs.com/docs/reference/local-development/fast-refresh/#how-it-works).

However, having some components being hot updated and not others is a very frustrating experience as a developer, and I think this rule should be enforced by a linter, so I made an [eslint rule](https://github.com/ArnaudBarre/eslint-plugin-react-refresh) to go along with this plugin.

This plugin expects this rule to be respected and will always fast refresh components. If a file export something that is not a React component (TS types are ok), update to this export would not propagate and require a manual reload.