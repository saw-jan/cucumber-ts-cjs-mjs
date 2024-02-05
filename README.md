## Configure cucumber with cjs or mjs type

### CommonJs Module (cjs)

`📁 cucu-ts-cjs`: This setup is suitable for projects that work on CommonJs module system (`require`)

> cucumber.js:

```js
module.exports = {
  default: {
    requireModule: ["ts-node/register"], // transpiler
    require: ["tests/steps/**/*.ts"],
  },
};
```

> tsconfig.json

```json
{
  "compilerOptions": {
    "module": "CommonJS"
  }
}
```

> package.json

```json
"scripts": {
    "test": "cucumber-js"
}
```

And the project `package.json` SHOULD NOT have `"type": "module"`

💡**NOTE:** If the project is on `ES Module` but need cucumber to run with `CommonJs`, the following changes need to be done:

1. rename `cucumber.js` to `cucumber.cjs`
2. create an empty `package.json` file inside the `tests` directory. This allows tests dir to be treated as CommonJs module

   You can add `"type": "commonjs"` in `tests/package.json` if you like but by default it will be commonjs.

### ES Module (mjs)

`📁 cucu-ts-cjs`: This setup is suitable for projects that work on ES module system (`import`).

> cucumber.js:

```js
export default {
  import: ["tests/steps/**/*.ts"],
};
```

> tsconfig.json

```json
{
  "compilerOptions": {
    "module": "NodeNext"
  }
}
```

> package.json

> cucumber `requireModule` option will only work with CommonJs so we have to provide ESM loader externally to transpile the .ts files.

```json
"type": "module",
"scripts": {
    "test": "NODE_OPTIONS='--loader ts-node/esm' cucumber-js"
}
```

❗WARNING: `--loader` node option has an experimental warning

> (node:58657) ExperimentalWarning: Custom ESM Loaders is an experimental feature and might change at any time
