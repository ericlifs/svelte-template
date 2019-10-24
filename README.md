# Enhanced sapper-template

The default [Sapper](https://github.com/sveltejs/sapper) template, with absolute imports (aliases), scss pre-processor, prettier + husky.

## Absolute imports

This project uses [`rollup-plugin-alias`](https://github.com/rollup/rollup-plugin-alias) for letting you import .svelte and .js files via absolute paths. With this approach, you will not have `../../../` long strings anymore when importing files.

Out of the box, this project is only configured with an alias for `components` path: if you import files from `@components/...` (notice the `@`) from any other file, it will be automatically replaced with `./src/components/...`

#### Creating new aliases

If you want to configure new aliases, you need to modify the `rollup.aliases.js` file which is located at the root of the project. The new aliases have to have this structure:

```
{
  find: `absolute path that you want`,
  replacement: `${__dirname}/src/any-folder`
}
```

## SCSS

This project allows you to use scss into your Svelte components. In order to enable this feature, the `<style>` tag of the component should have a `lang="scss"` defined:

```
<style lang='scss'>
  .info {
    h1 {
      color: $cl-black;
    }
  }
</style>
```

#### Mixins

The project also includes the following mixins in order to use them on your components:

- mobile
- mobile-landscape
- mobile-and-mobile-landscape
- mobile-and-tablet
- taablet
- tablet-portrait
- tablet-landscape
- big-tablet
- desktop
- print
- with-count
- disable-scrollbars
- internet-explorer

At the moment, for using these mixins you have to include the scss file on your svelte component styles:

```
<style lang="scss">
  @import './styles/mixins.scss';

  h1 {
    color: red;

    @include mobile-and-tablet {
      color: black;
    }
  }
</style>
```

#### Variables

If you want to define custom variables so you can use them on your components, you can do it at the `./src/styles/variables.scss` file (then you will need to import it on your component as we did for the mixins). You can also create any scss files that you want/need (e.g `color.scss`)

```
$cl-black: #000
```

And then in your component:

```
<style lang="scss">
  @import './styles/colors.scss';

  h1 {
    color: $cl-black;
  }
</style>
```

## Husky & Prettier

This project is configured with Prettier in order to keep the code baseline consistent. It will automatically prettify your staged files before you commit (and then push) them to your repo.

**Important: You need to be using node v8.6.0 at least in this project to run husky hooks**

#### Adding new options

You can simply add new options by modifying the `prettier.config.js` file. You can take a look at all of them [`here`](https://prettier.io/docs/en/options.html).

#### Disabling Husky

If you don't want this awesome feature, you can remove the `husky` configuration from the `package.json` file. Also feel free to delete all dev-dependencies from it (`husky` & `pretty-quick`).

### Running the project

However you get the code, you can install dependencies and run the project in development mode with:

```bash
cd my-app
npm install # or yarn
npm run dev
```

Open up [localhost:3000](http://localhost:3000) and start clicking around.

Consult [sapper.svelte.dev](https://sapper.svelte.dev) for help getting started.

## Structure

Sapper expects to find two directories in the root of your project — `src` and `static`.

### src

The [src](src) directory contains the entry points for your app — `client.js`, `server.js` and (optionally) a `service-worker.js` — along with a `template.html` file and a `routes` directory.

#### src/routes

This is the heart of your Sapper app. There are two kinds of routes — _pages_, and _server routes_.

**Pages** are Svelte components written in `.svelte` files. When a user first visits the application, they will be served a server-rendered version of the route in question, plus some JavaScript that 'hydrates' the page and initialises a client-side router. From that point forward, navigating to other pages is handled entirely on the client for a fast, app-like feel. (Sapper will preload and cache the code for these subsequent pages, so that navigation is instantaneous.)

**Server routes** are modules written in `.js` files, that export functions corresponding to HTTP methods. Each function receives Express `request` and `response` objects as arguments, plus a `next` function. This is useful for creating a JSON API, for example.

There are three simple rules for naming the files that define your routes:

- A file called `src/routes/about.svelte` corresponds to the `/about` route. A file called `src/routes/blog/[slug].svelte` corresponds to the `/blog/:slug` route, in which case `params.slug` is available to the route
- The file `src/routes/index.svelte` (or `src/routes/index.js`) corresponds to the root of your app. `src/routes/about/index.svelte` is treated the same as `src/routes/about.svelte`.
- Files and directories with a leading underscore do _not_ create routes. This allows you to colocate helper modules and components with the routes that depend on them — for example you could have a file called `src/routes/_helpers/datetime.js` and it would _not_ create a `/_helpers/datetime` route

### static

The [static](static) directory contains any static assets that should be available. These are served using [sirv](https://github.com/lukeed/sirv).

In your [service-worker.js](src/service-worker.js) file, you can import these as `files` from the generated manifest...

```js
import { files } from '@sapper/service-worker'
```

...so that you can cache them (though you can choose not to, for example if you don't want to cache very large files).

## Bundler config

Sapper uses Rollup or webpack to provide code-splitting and dynamic imports, as well as compiling your Svelte components. With webpack, it also provides hot module reloading. As long as you don't do anything daft, you can edit the configuration files to add whatever plugins you'd like.

## Production mode and deployment

To start a production version of your app, run `npm run build && npm start`. This will disable live reloading, and activate the appropriate bundler plugins.

You can deploy your application to any environment that supports Node 8 or above. As an example, to deploy to [Now](https://zeit.co/now), run these commands:

```bash
npm install -g now
now
```

## Using external components

When using Svelte components installed from npm, such as [@sveltejs/svelte-virtual-list](https://github.com/sveltejs/svelte-virtual-list), Svelte needs the original component source (rather than any precompiled JavaScript that ships with the component). This allows the component to be rendered server-side, and also keeps your client-side app smaller.

Because of that, it's essential that the bundler doesn't treat the package as an _external dependency_. You can either modify the `external` option under `server` in [rollup.config.js](rollup.config.js) or the `externals` option in [webpack.config.js](webpack.config.js), or simply install the package to `devDependencies` rather than `dependencies`, which will cause it to get bundled (and therefore compiled) with your app:

```bash
npm install -D @sveltejs/svelte-virtual-list
```

## Bugs and feedback

Sapper is in early development, and may have the odd rough edge here and there. Please be vocal over on the [Sapper issue tracker](https://github.com/sveltejs/sapper/issues).
