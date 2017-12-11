# egg-router-plus

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![David deps][david-image]][david-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-router-plus.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-router-plus
[travis-image]: https://img.shields.io/travis/atian25/egg-router-plus.svg?style=flat-square
[travis-url]: https://travis-ci.org/atian25/egg-router-plus
[codecov-image]: https://img.shields.io/codecov/c/github/atian25/egg-router-plus.svg?style=flat-square
[codecov-url]: https://codecov.io/github/atian25/egg-router-plus?branch=master
[david-image]: https://img.shields.io/david/atian25/egg-router-plus.svg?style=flat-square
[david-url]: https://david-dm.org/atian25/egg-router-plus
[snyk-image]: https://snyk.io/test/npm/egg-router-plus/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-router-plus
[download-image]: https://img.shields.io/npm/dm/egg-router-plus.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-router-plus

The missing router feature for [eggjs](https://eggjs.org)

## Install

```bash
$ npm i egg-router-plus --save
```

Then mount plugin:

```js
// {app_root}/config/plugin.js
exports.routerPlus = {
  enable: true,
  package: 'egg-router-plus',
};
```

## Features

### load `app/router/**/*.js`

this plugin will auto load router define at `app/router/**/*.js`

### app.router.namespace

```js
app.router.namespace(prefix, ...middlewares);
```

- `prefix` - {String}, the prefix string of sub router
- `middlewares` - {...Function}, optional

Support same as Router:

- `router.verb('path-match', app.controller.controller.action);`
- `router.verb('path-match', middleware1, ..., middlewareN, app.controller.controller.action);`
- `router.verb('router-name', 'path-match', app.controller.controller.action);`
- `router.verb('router-name', 'path-match', middleware1, ..., middlewareN, app.controller.controller.action);`

Note: `prefix` and `path` are not allow to be `regex`.

```js
// {app_root}/app/router.js
module.exports = app => {
  const subRouter = app.router.namespace('/sub');
  // curl localhost:7001/sub/test
  subRouter.get('/test', app.controller.sub.test);
  subRouter.get('sub_upload', '/upload', app.controller.sub.upload);

  // const subRouter = app.router.namespace('/sub/:id');
  // cont subRouter = app.router.namespace('/sub', app.middleware.jsonp());

  // output: /sub/upload
  console.log(app.url('sub_upload'));
};
```

## Known issues

- sub `redirect` is not support, use `app.router.redirect()` or redirect to a named router.

```js
const subRouter = app.router.namespace('/sub');

// will redirect `/sub/go` to `/anyway`, not `/sub/anyway`
subRouter.redirect('/go', '/anyway');

// just use router
router.redirect('/sub/go', '/sub/anyway');

// or redirect to a named router
subRouter.get('name_router', '/anyway', app.controller.sub.anyway);
// will redirect `/sub/go_name` to `/sub/anyway` which is named `name_router`
subRouter.redirect('/sub/go_name', 'name_router');
```

## Questions & Suggestions

Please open an issue [here](https://github.com/atian25/egg-router-plus/issues).

## License

[MIT](LICENSE)
