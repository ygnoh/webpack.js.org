---
title: EnvironmentPlugin
contributors:
  - simon04
  - einarlove
  - rouzbeh84
---


<!-- The `EnvironmentPlugin` is shorthand for using the [`DefinePlugin`](/plugins/define-plugin) on [`process.env`](https://nodejs.org/api/process.html#process_process_env) keys. -->
`EnvironmentPlugin`은 [`process.env`](https://nodejs.org/api/process.html#process_process_env) 에서 [`DefinePlugin`](./define-plugin)을 사용하는 것에 대한 축약형입니다.

## Usage

<!-- The `EnvironmentPlugin` accepts either an array of keys or an object mapping its keys to their default values. -->
`EnvironmentPlugin`은 키의 배열이나 키를 기본값으로 매핑하는 객체를 받습니다.

```javascript
new webpack.EnvironmentPlugin(['NODE_ENV', 'DEBUG'])
```

<!-- This is equivalent to the following `DefinePlugin` application: -->
이것은 다음과 같이 `DefinePlugin`를 통해 설정한 내용과 같습니다:

The `EnvironmentPlugin` is shorthand for using the [`DefinePlugin`](/plugins/define-plugin) on [`process.env`](https://nodejs.org/api/process.html#process_process_env) keys.

## Usage

The `EnvironmentPlugin` accepts either an array of keys or an object mapping its keys to their default values.

```javascript
new webpack.EnvironmentPlugin(['NODE_ENV', 'DEBUG'])
```

This is equivalent to the following `DefinePlugin` application:

```javascript
new webpack.DefinePlugin({
  'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
  'process.env.DEBUG': JSON.stringify(process.env.DEBUG)
})
```

T> Not specifying the environment variable raises an "`EnvironmentPlugin` - `${key}` environment variable is undefined" error.

## Usage with default values

Alternatively, the `EnvironmentPlugin` supports an object, which maps keys to their default values. The default value for a key is taken if the key is undefined in `process.env`.

```javascript
new webpack.EnvironmentPlugin({
  NODE_ENV: 'development', // use 'development' unless process.env.NODE_ENV is defined
  DEBUG: false
})
```

W> Variables coming from `process.env` are always strings.

T> Unlike [`DefinePlugin`](/plugins/define-plugin), default values are applied to `JSON.stringify` by the `EnvironmentPlugin`.

T> To specify an unset default value, use `null` instead of `undefined`.

**Example:**

Let's investigate the result when running the previous `EnvironmentPlugin` configuration on a test file `entry.js`:

```javascript
if (process.env.NODE_ENV === 'production') {
  console.log('Welcome to production');
}
if (process.env.DEBUG) {
  console.log('Debugging output');
}
```

When executing `NODE_ENV=production webpack` in the terminal to build, `entry.js` becomes this:

```javascript
if ('production' === 'production') { // <-- 'production' from NODE_ENV is taken
  console.log('Welcome to production');
}
if (false) { // <-- default value is taken
  console.log('Debugging output');
}
```

Running `DEBUG=false webpack` yields:

```javascript
if ('development' === 'production') { // <-- default value is taken
  console.log('Welcome to production');
}
if ('false') { // <-- 'false' from DEBUG is taken
  console.log('Debugging output');
}
```

## `DotenvPlugin`

The third-party [`DotenvPlugin`](https://github.com/mrsteele/dotenv-webpack) (`dotenv-webpack`) allows you to expose (a subset of) [dotenv variables](https://www.npmjs.com/package/dotenv):

``` bash
// .env
DB_HOST=127.0.0.1
DB_PASS=foobar
S3_API=mysecretkey
```

```javascript
 new Dotenv({
  path: './.env', // Path to .env file (this is the default)
  safe: true // load .env.example (defaults to "false" which does not use dotenv-safe)
})
```
