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
* [ ] setup end to end tests that can work against `NOW_URL` or default local development url.
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
* [ ] setup an [alias][alias], either to external domain or to sensible `<service name>.now.sh`.
  If the service should be hidden, but "stable", I suggest generating random url as an alias,
  for example `<service name-usjwy83ksyq.now.sh>` which allows other services to use stable URL.
* [ ] setup continuous deployment if tests pass using [now-pipeline][now-pipeline].
  You can set alias switch and pruning old deploys on success. Do not forget to create a new
  Zeit [API token][api token] and set it on CI as `NOW_TOKEN` environment variable.
* [ ] pass passwords, api keys, and other sensitive information via encoded environment
  [secrets][env-and-secrets]. From command line define a secret.
    ```
    $ now secret add acme-api-key my-value-here
    ```
  Then map this secret to an environment variable name in `package.json` "now" section
    ```json
    {
      "now": {
        "env": {
          "MY_VARIABLE": "@acme-api-key"
        }
      }
    }
    ```
* [ ] setup logging either by using a full features service, or with simple [now-logs][now-logs]
  if you only interested in real time logging without history.

## Related

* [now-pipeline][now-pipeline] - automatically deploys Now service if tests pass. 
  For example see [Cypress and Immutable Deploys][immutable deploys]
* [NPM module checklist](https://github.com/bahmutov/npm-module-checklist)
* [Awesome Zeit](https://github.com/zeit/awesome-zeit) - list of resources 
  related to Zeit.

[now-pipeline]: https://github.com/bahmutov/now-pipeline
[immutable deploys]: https://www.cypress.io/blog/2017/05/30/cypress-and-immutable-deploys/
[engines]: https://docs.npmjs.com/files/package.json#engines
[crasher]: https://github.com/bahmutov/crasher
[alias]: https://zeit.co/docs/features/aliases
[api token]: https://zeit.co/account/tokens
[env-and-secrets]: https://zeit.co/docs/features/env-and-secrets
[now-logs]: https://github.com/berzniz/now-logs#readme
