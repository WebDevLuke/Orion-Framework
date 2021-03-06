<h1>
	 <img height="67" width="391" src="https://cdn.rawgit.com/WebDevLuke/OrionBP/master/misc/orionbp-logo.svg">
</h1>

[![CircleCI](https://circleci.com/gh/WebDevLuke/OrionBP/tree/master.svg?style=shield)](https://circleci.com/gh/WebDevLuke/OrionBP/tree/master)

OrionBP is a simple front-end boilerplate for projects using [OrionCSS](https://github.com/WebDevLuke/OrionCSS) and [OrionJS](https://github.com/WebDevLuke/OrionJS). It includes a suite of useful Gulp tasks allowing you to compile, compress and concatenate your SASS, JS and image assets.

## Getting Started
OrionBP uses [Gulp](http://gulpjs.com/) as its build system. If you've never used Gulp before, you need to first install its client using NPM:-

```
npm install --global gulp-cli
``` 

Next you need to install OrionBP's dependencies, which includes [OrionCSS](https://github.com/WebDevLuke/OrionCSS) and [OrionJS](https://github.com/WebDevLuke/OrionJS):-

```
npm install
```

Finally, run the below command:-

```
gulp setup
```

This command:-

1. Generates all required [OrionCSS](https://github.com/WebDevLuke/OrionCSS) directories in your dev directory.
2. Adds an [OrionCSS](https://github.com/WebDevLuke/OrionCSS) compatible `main.scss` to your SASS dev directory, though if you've modified the default SASS directory structure you may then need to edit the `@import` paths to correctly point to `node_modules`.
3. Adds a sample SASS component `sample.component.mycomponent` to the `06 - components` directory.
4. Creates your JS directory and adds a `main.js` starter file with [OrionJS](https://github.com/WebDevLuke/OrionJS) imports.

## Configuration

In `gulpfile.js` you can configure various options to tweak the behaviour of gulp tasks.

`minify` - if `true` then CSS & JS will be minified once compiled and will have a .min suffix before the file extension. For example `style.min.css`.

`lint` - If `true` then SASS will be linted by [stylelint](https://github.com/stylelint/stylelint) to enforce style guidelines. These rules can be tweaked in `.stylelintrc`.

You can also configure the paths used by Gulp to align with your project's directory structure. By default, these paths are:-

```js
// Development root
const dev = "dev";

// Distribution root
const dist = "dist";

// HTML directories
const htmlDev = "dev/html";
const htmlDist = dist;

// Image directories
const imgDev = "dev/img";
const imgDist = "dist/img";

// SASS directories
const sassDev = "dev/sass";
const sassDist = "dist/css";

// JS directories
const jsDev = "dev/js";
const jsDist = "dist/js";
```

## Gulp Tasks

A breakdown of the production Gulp tasks included with OrionBP can be found below:-

### Build

`gulp build` generates a freshly compiled build of your project in your chosen distribution directory (default: `dist`).

During a build, the following happens:-

#### Cleanup
- Any existing builds are deleted

#### HTML
- HTML is copied from its development directory (default: `dev/html`) to its dist directory (default: `dist`)
- OrionBP supports [nunjucks](https://mozilla.github.io/nunjucks/), so if any relevant syntax is found its also compiled

#### PHP / SQL
- PHP is left intact; copied with its directory structure from dev directory to the dist directory

#### SASS
- SASS is compiled and minified (if `minify` is true)
- It is then autoprefixed using [autoprefixer](https://github.com/postcss/autoprefixer)
- It is then linted for errors using [stylelint](https://github.com/stylelint/stylelint) (if `lint` is true)
- A source map is generated using [gulp-sourcemaps](https://www.npmjs.com/package/gulp-sourcemaps) and appended to the minified CSS
- Unused CSS classes are removed using [UNCSS](https://github.com/giakki/uncss)
- A compiled CSS file is created in the sass dist directory (default: `dist/css`)

#### JS
- Gulp looks for files in the root development JS directory (default: `dev/js`)
- [Browerify](http://browserify.org/) grabs all dependencies and bundles everthing together into one file
- The bundle is minified (If `minify` is true)
- It's deposited in the js dist directory (default: `dist/js`)

#### Misc
- Any miscellaneous files with a xml, txt or json file extention found in the dev directory are copied with their directory structure intact to the dist directory
- If a `fonts` directory is found in the dev directory, it's copied to the dist directory with its contents
- If a `.htaccess` file is found in the dev directory, it's copied to the dist directory

#### Images
- Bitmap images are copied to the image dist directory (default: `dist/images`) and optimised using [imagemin](https://github.com/imagemin/imagemin)
- SVG images are concatenated into one and embed directly in the HTML as an icon system. [More info](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)

### Watch

`gulp watch` watches for any file changes and if detected runs a relevant gulp task. For example if `gulp watch` is active and any `.scss` files change, a SASS task will then automatically run to compile the changes.

**Note** - The SASS watch task doesn't run UNCSS. This is simply to speed it up a little as UNCSS is quite a taxing process when there are many pages to process.

### Debug Tasks

- `gulp sass-debug` - Generates a non-minified build of your SASS.
- `gulp sass-build-debug` - The same as above, but also runs UNCSS and lint (if `lint` is true).

## About the Developer
I'm Luke Harrison, a Sheffield-based Web Designer &amp; Developer from the UK, currently working at [Evolution Funding](https://github.com/EvolutionFunding). Read more about me at [lukeharrison.net](http://www.lukeharrison.net) and/or follow me on twitter at [@WebDevLuke](https://twitter.com/WebDevLuke).
