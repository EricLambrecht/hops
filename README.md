<p align="center">
  <img
    width="200"
    height="217"
    src="https://raw.githubusercontent.com/wiki/xing/hops/logo.png"
  />
</p>

<h1 align="center">Hops Development Environment</h1>

<p align="center">
  <a href="https://gitter.im/xing-hops/Lobby">
    <img src="https://img.shields.io/gitter/room/xing-hops/Lobby.svg">
  </a>
  <a href="https://www.npmjs.com/package/hops">
    <img src="https://img.shields.io/npm/v/hops/next.svg">
  </a>
  <a href="https://travis-ci.org/xing/hops/branches">
    <img src="https://img.shields.io/travis/xing/hops/master.svg">
  </a>
</p>
<p>&nbsp;</p>

Hops is everything you need to develop and deploy a production grade universal web application with [React](https://facebook.github.io/react/). It provides both a **universal runtime** as well as the necessary **build tooling**.

Hops targets beginners and experts alike and follows React mainstream best practices.

The two guiding principles are:

1.  Hops scales with your requirements, from an easy start up to large scale applications with many teams
2.  Hops fits a broad need by being modular and extensible, it comes with reasonable defaults, but allows you to customize, configure, and extend almost everything when needed

## Quick start

Install the `hops` CLI globally:

```shell
$ yarn global add hops
# OR npm install --global hops
```

Or use our [`create-hops-app`](https://github.com/xing/hops/packages/create-hops-app) package with [`yarn create`](https://yarnpkg.com/lang/en/docs/cli/create/) or [`npx`](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b):

```shell
$ yarn create hops-app@next --hops-version next --template hops-template-react@next my-awesome-project
# OR npx create-hops-app@next --hops-version next --template hops-template-react@next my-awesome-project
$ cd my-awesome-project
$ yarn start
```

This will start hops in development mode. Visit [http://localhost:8080](http://localhost:8080) to see your app in the browser and make some changes to the code in your editor to see it live-reloading.

### Templates

Hops provides a few templates as starting points, which can be installed using the `--template` option of the [Hops CLI `init` command](https://github.com/xing/hops/tree/master/packages/cli).

#### [`hops-template-react`](https://github.com/xing/hops/tree/master/packages/template-react) (default)

This is the default template, it consists of a simple React starting point and it is being used when you execute:

```bash
$ hops init --hops-version next --template hops-template-react@next my-awesome-project
```

#### [`hops-template-redux`](https://github.com/xing/hops/tree/master/packages/template-redux)

In case you want to build a Redux project, you can use this template as a starting point.

```bash
$ hops init --hops-version next --template hops-template-redux@next my-awesome-project
```

#### [`hops-template-graphql`](https://github.com/xing/hops/tree/master/packages/template-graphql)

In case you want to build a GraphQL project, you can use this template as a starting point.

```bash
$ hops init --hops-version next --template hops-template-graphql@next my-awesome-project
```

## Presets

Almost everything in Hops is a preset that just needs to be installed / configured in order to start using it.

There are a few core presets ([`@untool/yargs`](https://github.com/untool/untool/tree/master/packages/yargs), [`@untool/express`](https://github.com/untool/untool/tree/master/packages/express), [`@untool/webpack`](https://github.com/untool/untool/tree/master/packages/webpack), [`hops-express`](https://github.com/xing/hops/tree/master/packages/express) and [`hops-webpack`](https://github.com/xing/hops/tree/master/packages/webpack)) that we recommend be always installed. They take care of setting up [Express.js](https://expressjs.com/), [webpack](https://webpack.js.org/) and [yargs](http://yargs.js.org/) to supply the basic building blocks of Hops.

**Therefore we have bundled them together in the convenience preset [`hops-preset-defaults`](https://github.com/xing/hops/tree/master/packages/preset-defaults)**.

Together these core presets expose the following CLI commands:

- `hops build [-p, --production] [-s, --static]` - execute a build and write assets to filesystem.
  - `--production` is an alias for `NODE_ENV=production` and will create a production build (minifies the bundle, externalizes the source maps, etc).
  - `--static` will render the configured `locations` to static HTML files.
- `hops develop` - start a development server with hot module reloading enabled.
- `hops serve` - start an Express.js server for your application (don't forget to execute `hops build` before).
- `hops start [-p, --production] [-s, --static]` - either starts a development server or executes a production build and serves that.
  - `--production` is an alias for `NODE_ENV=production` and will create a production build and afterwards start a production ready Express.js server.
  - `--static` is only relevant when executed together with the `--production` argument and will do a static build instead (renders static HTML pages) and serve them afterwards.

We also recommend that you always install the [`hops-react`](https://github.com/xing/hops/tree/master/packages/react) preset in order to start using [React](https://reactjs.org/) in your application.

### Installing / configuring a preset

In order to install or configure a preset you need to add it as a dependency to your application:

```bash
$ yarn add hops-redux@next
# OR npm install --save hops-redux@next
```

Sometimes presets have `peerDependencies` which need to be installed as well - take a look at the [indiviual preset sections](#available-presets) or watch out for peer dependency warnings in your terminal.

By default Hops will find installed presets automatically (only when it is listed as a top-level dependency in your `package.json` - we won't attempt to find presets in dependencies of dependencies).

If you prefer, you can also explicitly list the presets that you want to use under the `presets` key in your [application configuration](#configuration). This will disable automatic discovery of hops presets.

```json
{
  "hops": {
    "presets": ["hops-preset-defaults", "hops-react", "hops-redux"]
  }
}
```

Some presets require (or allow) additional configuration. Read the sections below for each of the presets you are using to find out what options are available to you.

### Available presets

In addition to the core presets that [we mentioned earlier](#presets) these following presets are available to help you with the struggles of universal rendering, deployments, service workers and such.

#### `hops-react`

This preset enables usage of React.js with JSX, React-Helmet and React-Router support in Hops applications.

Install it to your project:

```bash
$ yarn add hops-react@next react-dom react-helmet
```

And write your main entry file (which should either be named `index.js` or should be referenced in your `package.json`s `main` field):

**`package.json`**

```json
{
  "main": "main.js"
}
```

**`main.js`**

```javascript
import { render } from 'hops-react';
import React from 'react';

export default render(<h1>Hello World!</h1>);
```

For more details and advanced use-cases read the [full readme of `hops-react`](https://github.com/xing/hops/tree/master/packages/react).

#### `hops-redux`

This preset will set-up a Redux store, take care of dehydration / rehydration and wrapping your application in a `<Provider />`.

Install it to your project:

```bash
$ yarn add hops-redux@next@next react-redux redux redux-thunk
```

And pass your reducers as [render options](https://github.com/xing/hops/tree/master/packages/redux#render-options):

```javascript
import { render } from 'hops-react';
export default render(<MyApp />, { redux: { reducers: myReducers } });
```

For more details and more advanced use-cases, head over to the [full readme of `hops-redux`](https://github.com/xing/hops/tree/master/packages/redux).

#### `hops-graphql`

This preset will create an [Apollo client](https://www.apollographql.com/docs/react/) and take care of dehydration / rehydration and wrapping your application in an `<ApolloProvider />`.

Install it to your project:

```bash
$ yarn add hops-graphql@next graphql-tag react-apollo
```

And configure your GraphQL endpoint URI:

```json
{
  "hops": {
    "graphqlUri": "https://www.graphqlhub.com/graphql"
  }
}
```

Now you can use GraphQL in your project. Check out [this integration test](https://github.com/xing/hops/blob/master/packages/spec/integration/graphql) or [the `hops-graphql` template](https://github.com/xing/hops/tree/master/packages/template-graphql) to see usage examples and head over to [the `hops-graphql` readme](https://github.com/xing/hops/tree/master/packages/graphql) for more details.

#### `hops-postcss`

This preset will enable PostCSS support with [CSS modules](https://github.com/css-modules/css-modules) via a [css-loader](https://github.com/webpack-contrib/css-loader) and add the [PostCSS Preset Env](https://github.com/csstools/postcss-preset-env) to your project.

Install it to your project:

```bash
$ yarn add hops-postcss@next
```

Now you can use `import`/`require` to load `.css` files and style your components.\
At the end they will be combined to a single CSS file and loaded automatically.

```javascript
import { render } from 'hops-react';
import styles from './styles.css';

export default render(<h1 className={styles.headline}>hello world</h1>).
```

For more details take a look at the [`hops-postcss` readme](https://github.com/xing/hops/tree/master/packages/postcss).

#### `hops-styled-components`

This preset will enable support for server-side rendering of [styled-components](https://www.styled-components.com/) and set-up a `<ThemeProvider />` for you.

Install it to your project:

```bash
$ yarn add hops-styled-components@next styled-components
```

Now you can use styled-components in your app and it will work out of the box with server-side rendering.

```javascript
import { render } from 'hops-react';
import styled from 'styled-components';

const H1 = styled.h1`
  position: sticky;
`;

export default render(<H1>hello</H1>);
```

Read the [`hops-styled-components` readme](https://github.com/xing/hops/tree/master/packages/styled-components) for more detailed examples.

#### `hops-typescript`

This preset will enable you to write your Hops application using [TypeScript](https://www.typescriptlang.org/).

Install it to your project:

```bash
$ yarn add hops-typescript@next
```

And create a `tsconfig.json` file in your application root folder (you are free to extend our [minimal `tsconfig.json`](https://github.com/xing/hops/blob/master/packages/typescript/tsconfig.json) that we ship with this module or write it yourself).

```json
{
  "extends": "./node_modules/hops-typescript/tsconfig.json"
}
```

Take a look at this [integration test](https://github.com/xing/hops/tree/master/packages/spec/integration/typescript) to see an example of this in action and head over to the [readme of `hops-typescript`](https://github.com/xing/hops/tree/master/packages/typescript) for more details.

#### `hops-pwa`

This preset enables PWA features, such as web app manifest and service workers for Hops projects.

Install it to your project:

```bash
$ yarn add hops-pwa@next
```

Now you can `import`/`require` your web app manifest and render a `<link />` tag for it:

```javascript
import { render } from 'hops-react';
import manifest from './manifest.webmanifest';

export default render(
  <Helmet>
    <link rel="manifest" href={manifest} />
  </Helmet>
);
```

For more details and examples on how to use service workers read the [full readme for `hops-pwa`](https://github.com/xing/hops/tree/master/packages/pwa).

#### `hops-preset-defaults`

This is the base preset which provides the building blocks of Hops by combining several other presets ([`@untool/express`](https://github.com/untool/untool/tree/master/packages/express), [`@untool/webpack`](https://github.com/untool/untool/tree/master/packages/webpack) and [`@untool/yargs`](https://github.com/untool/untool/tree/master/packages/yargs)).

These presets provide configration for the development and production server, the webpack build configurations and much more.

Check out [this readme](https://github.com/xing/hops/tree/master/packages/preset-defaults) to learn more about how to configure HTTPS support, host, port, static site generation and how you can modify the Express.js middleware and server instances and webpack build configurations.

#### `hops-development-proxy`

Hops apps are often served on the same host as their backend/API, so during development we provide this preset, that sets up an HTTP proxy to forward any unknown requests to the configured remote URL.

Install it to your project:

```bash
$ yarn add hops-development-proxy@next
```

And configure your remote endpoint:

```json
{
  "hops": {
    "proxy": "https://example.org/api/"
  }
}
```

This will proxy all requests that are not assets and don't have `text/html` in its `Accept` header to the configured proxy endpoint.

For more details and advanced use-cases head over to the [full readme of `hops-development-proxy`](https://github.com/xing/hops/tree/master/packages/development-proxy).

#### `hops-lambda`

This preset enables simple deployment workflows to AWS Lambda.

Install it to your project:

```bash
$ yarn add hops-lambda@next
```

Now all you need to do is [configure your AWS credentials](https://github.com/xing/hops/tree/master/packages/lambda#aws-configuration)\
and set your `basePath` to `prod` and then you are all set to deploy your application:

```bash
$ hops lambda deploy
```

For more advanced deployments and other scenarios take a look at the [full readme of `hops-lambda`](https://github.com/xing/hops/tree/master/packages/lambda).

## Configuration

Hops uses [cosmiconfig](https://github.com/davidtheclark/cosmiconfig) for its configuration, therefore you can configure Hops using any of the following ways:

- a `hops` key in your `package.json`
- a `.hopsrc` file in your applications root dir
- a `.hopsrc.{json,yaml,yml,js}` file in your applications root dir
- a `hops.config.js` file in your applications root dir

## FAQ

### When is Hops the framework for you?

When you want to share a consistent setting over more than one project. Either now or in the future. This applies to companies with more than one development team or when you constantly set up projects that should be consistent, e.g. training projects, tutorials, consultant work, etc.

Even without striving for consistency across multiple projects you may want to use Hops for a (single) new project when you do not want to write/copy/maintain your own webpack configuration. We feel your pain and have done that for you. However, we try hard to give you all the customizability you may ever need.

### When is Hops **not** the framework for you?

If you are new to React and just want a small sample app or mainly want to learn, we recommend to get started with [Create React App](https://github.com/facebook/create-react-app). Once you are getting more serious about your application you can still switch to Hops.

### Why not just use Create React App?

[Create React App](https://github.com/facebook/create-react-app) is great when you want to experiment with or learn React. For simplicity it is neither configurable nor extensible and does not evolve with you when you are ready to create a production ready app. Hops on the other hand might be a bit harder to get started, but has the complete potential for even large scale applications.

Additionally, contrary to Create React App, Hops is geared towards universal apps.

### Why a framework and not just a few code snippets describing best practices?

There are two main reasons. Firstly, snippets get outdated. Hops, however, is committed to best practices and to evolve with them. Secondly, Hops is organized in modules that need to run both on the client and on the server. Keeping a track of both sides and at the same time maintaining an in-sync version of a client-side and server-side snippet per module can be really tedious. Using a framework like Hops takes away that pain.

### Why does Hops not support "eject"?

[Create React App](https://github.com/facebook/create-react-app) offers you the option to "eject". This means dumping Webpack configuration files and scripts, leaving you with a stand-alone project. We Hops people do not think, this is a good idea. Those files still force their philosophy on you while at the same time they are hard to understand. We thus do not believe this to be a practical way to overcome vendor lock-in. Hops instead allows you to customize either by configuration or adding modules.

### Can we trust in Hops to be around for the forseeable future?

Just as you might put trust into React as Facebook has committed itself to it and eats its own dog food, Hops is being backed and used by a European professional networking site.

Hops being primarily an (extensible) bunch of mainstream best practices, it ought to be rather simple to move off it and do something that better fits the user's requirements. So, the lock-in ought to be way less problematic than, say, with React. Porting a non-trivial app from React to Angular or Vue certainly is a lot more work than replacing Hops with its actual building blocks (Webpack, Babel, etc.)

### I like Hops, but I want to use something that is not quite mainstream

Hops allows you to write a package of your own and plug it into Hops' build process. Take a look at all the packages in this repository and read [the `@untool/core` documentation](https://github.com/untool/untool/blob/master/packages/core/README.md#presets) for more information about presets and mixins.

### How can I extend the babel and other build-related config

Hops provides a [`configureBuild`](https://github.com/xing/hops/tree/master/packages/preset-defaults#configurebuildwebpackconfig-loaderconfigs-target-webpackconfig-pipe) [core mixin](https://github.com/untool/untool/blob/master/packages/core/README.md#mixins) hook that allows you to alter the [webpack config](https://webpack.js.org/configuration/) to fit your needs, including adding babel plugins.

## Contributing

Hops uses [lerna](https://github.com/lerna/lerna) and [yarn](https://yarnpkg.com/en/) for development and publishing of the packages. Therefore it is required to have `yarn` globally available.

If you want to contribute to this project, create a fork of its [repository](https://github.com/xing/hops/fork) using the GitHub UI. Check out your new fork to your computer:

```bash
mkdir hops && cd $_
git clone git@github.com:user/hops.git .
```

Afterwards, run `yarn install` to install the required dependencies.

Using `yarn start` will execute the `start` script in the [hops-template-react](https://github.com/xing/hops/tree/master/packages/template-react) so that you can verify your changes in the browser.

Hops is written using modern ECMAScript features (ES 2017) and uses [CommonJS](https://nodejs.org/docs/latest/api/modules.html) as its module system. The code is formatted using [prettier](https://prettier.io) and checked via [ESlint](https://eslint.org/) and [Jest](https://jestjs.io/). You can check your code by executing: `yarn lint`, `yarn test` and format it using `yarn fmt`.

We are using the [Conventional Commits spec](https://conventionalcommits.org/) for our commit messages.

When you are finished, please do go ahead and create a [pull request](https://help.github.com/articles/creating-a-pull-request/).

## Thanks!

The beautiful hops icon used in the logo was created by [The Crew at Fusionary](https://thenounproject.com/fusionary/) and provided via [The Noun Project](https://thenounproject.com/term/hops/9254/). It is licensed under a [Creative Commons](http://creativecommons.org/licenses/by/3.0/us/) license.
