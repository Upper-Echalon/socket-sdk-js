# @socketsecurity/sdk

[![Socket Badge](https://socket.dev/api/badge/npm/package/@socketsecurity/sdk)](https://socket.dev/npm/package/@socketsecurity/sdk)
[![npm version](https://img.shields.io/npm/v/@socketsecurity/sdk.svg?style=flat)](https://www.npmjs.com/package/@socketsecurity/sdk)
[![TypeScript types](https://img.shields.io/npm/types/@socketsecurity/sdk.svg?style=flat)](https://www.npmjs.com/package/@socketsecurity/sdk)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](https://github.com/SocketDev/eslint-config)
[![Follow @SocketSecurity](https://img.shields.io/twitter/follow/SocketSecurity?style=social)](https://twitter.com/SocketSecurity)

SDK for the Socket API client, generated by `api`.

## Usage

```bash
npm install @socketsecurity/sdk
```

### ESM / TypeScript

```javascript
import { SocketSdk } from '@socketsecurity/sdk'

const client = new SocketSdk('yourApiKeyHere')

const res = await client.getQuota()

if (res.success) {
  // Will output { quota: 123 } if the quota you have left is 123
  console.log(res.data)
}
```

### CommonJS

```javascript
const { SocketSdk } = require('@socketsecurity/sdk')
```

## SocketSdk Methods

### Package methods

* `getIssuesByNPMPackage(packageName, version)`
  * `packageName`: A `string` representing the name of the npm package you want the issues for
  * `version`:  A `string` representing the version of the npm package to return the issues for
* `getScoreByNPMPackage(packageName, version)`
  * `packageName`: A `string` representing the name of the npm package you want the score for
  * `version`:  A `string` representing the version of the npm package to return the score for

### Report methods

* `createReportFromFilepaths(filePaths, pathsRelativeTo=., [issueRules])`
  * `filePaths`: An `array` of absolute or relative `string` paths to `package.json` and any corresponding `package-lock.json` files
  * `pathsRelativeTo`: A `string` path that the absolute paths `filePaths` are relative to. This to calculate where in your project the `package.json`/`package-lock.json` files lives
  * `issueRules`: An object that follows the format of the [`socket.yml`](https://docs.socket.dev/docs/socket-yml) issue rules. Keys being issue names, values being a boolean that activates or deactivates it. Is applied on top of default config and organization config.
* `getReportList()`
* `getReportSupportedFiles()`
* `getReport(id)`
  * `id`: A `string` representing the id of a created report

### Utility methods

* `getQuota()`
* `getOrganizations()`
* `postSettings(selectors)`
  * `selectors`: An array of settings selectors, e.g. `[{ organization: 'id' }]`
* `getOrgSecurityPolicy(orgSlug)`
  * `orgSlug`: the slug of the organization

## Additional exports

* `createUserAgentFromPkgJson(pkgJson)`
  * `pkgJson`: The content of the `package.json` you want to create a `User-Agent` string for

## Advanced

### Specifying custom user agent

The `SocketSdk` constructor accepts an `options` object as its second argument and there a `userAgent` key with a string value can be specified. If specified then that user agent will be prepended to the SDK user agent. See this example:

```js
const client = new SocketSdk('yourApiKeyHere', {
  userAgent: 'example/1.2.3 (http://example.com/)'
})
```

Which results in the [HTTP `User-Agent` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent):

```
User-Agent: example/1.2.3 (http://example.com/) socketsecurity-sdk/0.5.2 (https://github.com/SocketDev/socket-sdk-js)
```

To easily create a user agent for your code you can use the additional export `createUserAgentFromPkgJson()` like this, assuming `pkgJson` contains your parsed `package.json`:

```js
const client = new SocketSdk('yourApiKeyHere', {
  userAgent: createUserAgentFromPkgJson(pkgJson)
})
```

Specifying a custom user agent is good practice when shipping a piece of code that others can use to make requests. Eg. [our CLI](https://github.com/SocketDev/socket-cli-js) uses this option to identify requests coming from it + mentioning which version of it that is used.

## See also

* [Socket API Reference](https://docs.socket.dev/reference)
* [Socket.dev](https://socket.dev/)
* [Socket GitHub App](https://github.com/apps/socket-security)
* [Socket CLI](https://github.com/SocketDev/socket-cli-js)
