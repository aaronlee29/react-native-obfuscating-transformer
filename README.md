# General #
Forked from https://github.com/javascript-obfuscator/react-native-obfuscating-transformer
so I could apply needed fixes/enhancements and use it for my project without delay.

Many thanks to the original authors and maintainers.

## 2020-07-09
- Updated javascript-obfuscator dependency from ^0.13.0 to 1.3.0
- Fixed emitting of obfuscated files (previously, non-obfuscated code would be written)
- Added option to delete emitted files (use unlinkObfuscatedFiles=true)
- Added then-current pull request changes (platform-specific obfuscation, dependency upgrades)

# react-native-obfuscating-transformer

Obfuscate selected source files when building for React Native.

## Installation

    yarn add react-native-obfuscating-transformer --dev

or

    npm install react-native-obfuscating-transformer --save-dev

## Usage

### React Native >= 0.59

#### /metro.config.js

```diff
 module.exports = {
+  transformer: {
+    babelTransformerPath: require.resolve("./transformer")
+  },
 }
```

#### /transformer.js

```js
const obfuscatingTransformer = require("react-native-obfuscating-transformer")

module.exports = obfuscatingTransformer({
  /* options */
})
```

### React Native < 0.59

### /rn-cli.config.js

```diff
 module.exports = {
+  transformer: {
+    babelTransformerPath: require.resolve("./transformer")
+  },
 }
```

#### /transformer.js

```js
const obfuscatingTransformer = require("react-native-obfuscating-transformer")

module.exports = obfuscatingTransformer({
  /* options */
})
```

## Configuration

Options are:

### `upstreamTransformer: MetroTransformer`

Defines what the first pass of code transformation is. If you don't use a custom transformer already,
you don't need to set this option.

TypeScript example:

```diff
 const obfuscatingTransformer = require('react-native-obfuscating-transformer')
+ const typescriptTransformer = require('react-native-typescript-transformer')

 module.exports = obfuscatingTransformer({
+  upstreamTransformer: typescriptTransformer
 })
```

#### Default value: `require('metro/src/transformer')`

### `filter: (filename: string, source: string) => boolean`

Returns true for any files that should be obfuscated and false for any files which should not be obfuscated.

By default, it obfuscates all files in `src/**/*`

### `obfuscatorOptions: ObfuscatorOptions`

**Warning** — Not all options are guaranteed to produce working code. In particular, `stringArray` definitely breaks builds.

See the [javascript-obfuscator docs](https://github.com/javascript-obfuscator/javascript-obfuscator) for more info about what each option does.

```ts
interface ObfuscatorOptions {
  compact?: boolean
  controlFlowFlattening?: boolean
  controlFlowFlatteningThreshold?: 0.75
  deadCodeInjection?: boolean
  deadCodeInjectionThreshold?: 0.4
  debugProtection?: boolean
  debugProtectionInterval?: boolean
  disableConsoleOutput?: boolean
  domainLock?: string[]
  identifierNamesGenerator?: "hexadecimal" | "mangled"
  log?: boolean
  renameGlobals?: boolean
  reservedNames?: string[]
  rotateStringArray?: true
  seed?: 0
  selfDefending?: boolean
  sourceMap?: boolean
  sourceMapBaseUrl?: string
  sourceMapFileName?: string
  sourceMapMode?: "separate" | "inline"
  stringArray?: boolean
  stringArrayEncoding?: boolean
  stringArrayThreshold?: 0.75
  target?: "browser" | "extension" | "node"
  unicodeEscapeSequence?: boolean
}
```

### `trace: boolean`

Iff true, prints a list of files being obfuscated

#### Default value: `false`

### `emitObfuscatedFiles: boolean`

Iff true, emits the obfuscated versions of files alongside their originals, for comparison.

#### Default value: `false`

### `enableInDevelopment: boolean`

Iff true, enables obfuscation in development mode.

#### Default value: `false`

### `enableOnlyForPlatform: string | string[]`

Enables obfuscation only for the specified platform.
If falsy, enables obfuscation for all platforms.
For example `['ios']` enables obfuscation only for IOS.

#### Default value: `false`

### `unlinkObfuscatedFiles: boolean`

If true, and emitObfuscatedFiles is false, unlinks all existing emitted files.

#### Default value: `false`

## License

MIT
