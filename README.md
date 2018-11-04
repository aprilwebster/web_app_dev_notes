# web_app_dev_notes

## npm - package manager
### installing packages
- use `npm i <package_name> --save` for modules required in a production bundle
- use `npm i <package_name> --save-dev` for modules used for development purposes (e.g., linter, testing libraries, etc)

### npm scripts that can be added to `package.json`
- https://docs.npmjs.com/misc/scripts
- accessed using `npm run <script_name>`
- default script values:
   - `"start"` - if not specified and if there's a `server.js` file in the root of `package.json`, then npm will 
   default the `npm start` command to run `node server.js`



## webpack - static module bundler for JavaScript applications
### overview
- **transpiles** (through "loaders") and **bundles** source code (`/src`) into a single minimized and optimized file for distribution purposes (`/dist`) to be loaded into the browser
   - will create a dependency graph of and uses it to generate an optimized bundle where scripts are executed in the correct order
- core concepts: [Entry](https://webpack.js.org/concepts/#entry), [Output](https://webpack.js.org/concepts/#output), [Loaders](https://webpack.js.org/loaders/), [Mode](https://webpack.js.org/concepts/#mode), Plugins

### basic usage
- run it using `npx webpack` - expects input at `src/index.js` and generates a `dist/main.js` file

### support for transpiling [ES6 (aka ES2015)](http://www.ecma-international.org/ecma-262/6.0/)
- some older browsers don't support javascript modules (e.g., import and export key words)
- webpack has support for transpiling import, export and other JS module syntax to code that older browsers can run
- details in webpack's Module API
- if using other ES2015 features, will need to use transpiler middleware such as Babel or Buble and specify that in webpack's loader system.

### configuration
- webpack (v4+) doesn't require any configuration for basic use
- if want to specify other configuration webpack has a standard configuration file: `webpack.config.js`
   - if present, the webpack command picks it up by default
- can also use a different name for the config file for more complex configurations
   - in this case, use the `--config` option to pass a config of any name
- other things that you can configure:
   - loader rules
   - plugins
   - resolve options
   - https://webpack.js.org/configuration/

#### fields
- `entry` - input file
- `output` - directory and filename to persist the optimized and minimized code
- `mode` - {"production" (default), "development", "none"} - tells webpack to use its built-in optimizations accordingly.
- `module` - rules for how to handle differnt types of modules within a project
   
#### basic `webpack.config.js`
- to use it: `npx webpack --config webpack.config.js`

   
```
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
### loaders
- Loaders are transformations that are applied to the source code of a module. 
- They are written as functions that accept source code as a parameter and return a new version of that code with transformations applied.
- https://webpack.js.org/loaders/
- sample loaders:
   - `eslint-loader` PreLoader for linting code using ESLint
   - `sass-loader` - required to load and compile a SASS/SCSS file
   - `css-loader` - required to load CSS file with resolved imports and returns CSS code
   - `html-loader` Exports HTML as string, require references to static resources
   - `babel-loader` - required to load ES2015+ code and transpile it to ES5 using Babel

#### modules
https://webpack.js.org/configuration/module/

## es5 vs es6 - flavours of javascript

## Questions
1. if a `sass-loader` is specified in `webpack.config.js`, is it necessary to have something specified in the package.json to convert scss to css?
