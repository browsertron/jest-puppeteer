# jest-environment-puppeteer

[![Build Status][build-badge]][build]
[![version][version-badge]][package]
[![MIT License][license-badge]][license]

Run your tests using Jest & Puppeteer 🎪✨

```
npm install jest-environment-puppeteer puppeteer
```

## Usage

Update your Jest configuration:

```json
{
  "globalSetup": "jest-environment-puppeteer/setup",
  "globalTeardown": "jest-environment-puppeteer/teardown",
  "testEnvironment": "jest-environment-puppeteer"
}
```

Use Puppeteer in your tests:

```js
describe('Google', () => {
  beforeAll(async () => {
    await page.goto('https://google.com')
  })

  it('should display "google" text on page', async () => {
    const text = await page.evaluate(() => document.body.textContent)
    expect(text).toContain('google')
  })
})
```

## API

### `global.browser`

Give access to the [Puppeteer Browser](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-browser).

```js
it('should open a new page', async () => {
  const page = await browser.newPage()
  await page.goto('https://google.com')
})
```

### `global.page`

Give access to a [Puppeteer Page](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-page) opened at start (you will use it most of time).

```js
it('should fill an input', async () => {
  await page.type('#myinput', 'Hello')
})
```

### `global.jestPuppeteer.debug()`

Put test in debug mode.

- Jest is suspended (no timeout)
- A `debugger` instruction to Chromium, if Puppeteer has been launched with `{ devtools: true }` it will stop

```js
it('should put test in debug mode', async () => {
  await jestPuppeteer.debug()
})
```

### `jest-puppeteer.config.js`

You can specify a `jest-puppeteer.config.js` at the root of the project or define a custom path using `JEST_PUPPETEER_CONFIG` environment variable.

- `launch` <[object]> [All Puppeteer launch options](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteerlaunchoptions) can be specified in config. Since it is JavaScript, you can use all stuff you need, including environment.
- `connect` <[object]> [All Puppeteer connect options](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteerconnectoptions) can be specified in config. This is an alternative to `launch` config, allowing you to connect to an already running instance of Chrome.
- `exitOnPageError` <[boolean]> Exits page on any global error message thrown. Defaults to `true`.
- `server` <[Object]> Server options allowed by [jest-dev-server](https://github.com/smooth-code/jest-puppeteer/tree/master/packages/jest-dev-server)

#### Example 1

```js
// jest-puppeteer.config.js
module.exports = {
  launch: {
    dumpio: true,
    headless: process.env.HEADLESS !== 'false',
  },
  server: {
    command: 'node server.js',
    port: 4444,
    launchTimeout: 10000,
    debug: true,
  },
}
```

#### Example 2

This example uses an already running instance of Chrome by passing the active web socket endpoint to `connect`. This is useful, for example, when you want to connect to Chrome running in the cloud.

```js
// jest-puppeteer.config.js
const wsEndpoint = fs.readFileSync(endpointPath, 'utf8')

module.exports = {
  connect: {
    browserWSEndpoint: wsEndpoint,
  },
  server: {
    command: 'node server.js',
    port: 4444,
    launchTimeout: 10000,
    debug: true,
  },
}
```

## Inspiration

Thanks to Fumihiro Xue for his great [Jest example](https://github.com/xfumihiro/jest-puppeteer-example).

## License

MIT

[build-badge]: https://img.shields.io/travis/smooth-code/jest-puppeteer.svg?style=flat-square
[build]: https://travis-ci.org/smooth-code/jest-puppeteer
[version-badge]: https://img.shields.io/npm/v/jest-environment-puppeteer.svg?style=flat-square
[package]: https://www.npmjs.com/package/jest-environment-puppeteer
[license-badge]: https://img.shields.io/npm/l/jest-environment-puppeteer.svg?style=flat-square
[license]: https://github.com/smooth-code/jest-puppeteer/blob/master/LICENSE
