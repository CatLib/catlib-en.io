# CatLib Documentation [![Build Status](https://www.travis-ci.com/CatLib/catlib-en.io.svg?branch=2.0)](https://www.travis-ci.com/CatLib/catlib-en.io)

This is CatLib's documentation site.The document was built using [hexo](http://hexo.io/). Document content in the `source` folder, Written in Markdown format.

Browse the online version of the [CatLib documentation](https://catlib.io).

## Environmental

- [Nodejs(8.0.2+)](https://nodejs.org/en/)
- [Git(2.13.1+)](https://nodejs.org/en/)

## Get Starting

- `Clone` Documentation

```shell
$ git clone https://github.com/CatLib/catlib-en.io.git
```

- Installation dependency package

```shell
$ cd catlib-en.io && npm install
```

- Start the developer server

```shell
$ hexo s
```

If you open your browser and access `http://localhost:4000`, you should see the documentation site is up and running.

## Search support(Optional)

**Environmental: node-gyp**

- Installation `windows-build-tools`(Only windows)

```shell
npm install -g -production windows-build-tools
```

- Installation `node-gyp`

```shell
npm install -g node-gyp
```

- Restart your command line

**Installation dependency package**

- Installation `js-yaml`

```shell
$ npm install js-yaml --save-dev
```

- Installation `nodejieba`

```shell
$ npm install nodejieba --save-dev
```

> The search function will take effect automatically when the dependency package is installed.

## Contributing

We welcome contributions in any form and your contributions will be included in the contributor list. If you want to contribute, please `fork` this library and submit the contribution as `pull request`.
