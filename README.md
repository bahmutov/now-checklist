# Now-checklist

> Personal checklist when writing and deploying a small 
> Node.js service with [Zeit Now](https://zeit.co/now)

* [ ] Node.js version is specified in the `package.json` [engines section][engines]
    ```json
    {
      "engines": {
        "node": "8"
      }
    }
    ```
* [ ] install crash reporter to automatically forward exceptions to an external service.
  - [ ] synchronous crashes
  - [ ] asynchronous crashes
  - [ ] rejected promises
  I recommend installing [crasher][crasher] route to test crash handling on demand by going
  to a "magic" endpoint `<service>/crash`
* [ ] setup end to end tests that can work against `NOW_ULR` or default local dev url.
    ```js
    // spec.js
    const server = process.env.NOW_URL
    ? process.env.NOW_URL
    : 'http://localhost:3000'
    const got = require('got')
    const test = require('ava')
    test('server /version', async t => {
      const info = await got(toFullUrl('/version'), { json: true })
      // assert returned version
    })
    ```
* [ ] setup continuous deployment if tests pass using [now-pipeline][now-pipeline].

## Related

* [now-pipeline][now-pipeline] - automatically deploys Now service if tests pass. 
  For example see [Cypress and Immutable Deploys][immutable deploys]
* [NPM module checklist](https://github.com/bahmutov/npm-module-checklist)
* [Awesome Zeit](https://github.com/zeit/awesome-zeit) - list of resources 
  related to Zeit.

[now-pipeline]: https://github.com/bahmutov/now-pipeline
[immutable deploys]: https://www.cypress.io/blog/2017/05/30/cypress-and-immutable-deploys/
[engines section]: https://docs.npmjs.com/files/package.json#engines
[crasher]: https://github.com/bahmutov/crasher
