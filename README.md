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

### adding it as a script to a package.json
![package.json](images/webpack_in_packagejson.png)

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
- all loaders are named as `<type>-loader`
- They are written as functions that accept source code as a parameter and return a new version of that code with transformations applied.
- https://webpack.js.org/loaders/
- sample loaders:
   - `eslint-loader` PreLoader for linting code using ESLint
   - `sass-loader` - required to load and compile a SASS/SCSS file
   - `css-loader` - required to load CSS file with resolved imports and returns CSS code
   - `html-loader` Exports HTML as string, require references to static resources
   - `babel-loader` - required to load ES2015+ code and transpile it to ES5 using Babel
   
#### babel-loader
- https://webpack.js.org/loaders/babel-loader/
- there's been a change in how to specify loaders from `babel 6.x` to `babel.7.x`
   - webpack 4.x | babel-loader 8.x | babel 7.x:
      - `npm install -D babel-loader @babel/core @babel/preset-env webpack`
   - webpack 4.x | babel-loader 7.x | babel 6.x
      - `npm install -D babel-loader@7 babel-core babel-preset-env webpack`
   ```
   {
      loader: 'babel-loader',
      options: {
        presets: ['@babel/preset-env'],
        plugins: ['@babel/plugin-transform-runtime']
      }
    }
    ```
   - https://www.npmjs.com/package/babel-preset-es2015 - DEPRECATED - use @babel-preset-env

#### modules
https://webpack.js.org/configuration/module/

## es5 vs es6 - flavours of javascript

## express vs react-router-dom
`express` works on the server-side.  It handles the server API routes, accepts client requests, retrieve data from databases, prepare views and send back responses.

`react-router-dom` works on the client-side.  It helps organize client-side pages - it interprets a request and shows the appropriate react component.

In Single Page Applications, when a user navigates to a link, a request to the server is typically not sent. Instead, the client side router - `react-router-dom` handles the request and re-routes the client to the reaact component matching their request.

## react-router-dom
`react-router-dom` is a client side routing library composed of a collection of navigational components.

### react-router-dom vs react-router vs react-router-native
If you're on the web then `react-router-dom` should have everything you need as it also exports the common components you'll need. If you're using React Native, `react-router-native` should have everything you need for the same reason. So you'll probably never have to import anything directly from `react-router`.

In fact, in v4 `react-router` exports the core components and functions. `react-router-dom export`s DOM-aware components, like <Link> (which renders an <a>) and <BrowserRouter> (which interacts with the browser's window.history) and it re-exports all of `react-router`'s exports, so you only need to import from `react-router-dom` in your project.  https://github.com/ReactTraining/react-router/issues/4648#issuecomment-284479720

### types of routers in `react-router-dom`
- there are two types of routers that are recommended for web applications.  Both will create a specialized history object for you.
   1. `<BrowserRouter>`: a `<Router>` that uses the HTML5 history API (pushState, replaceState and the popstate event) to keep your UI in sync with the URL.
   1. `<HashRouter>`: a `<Router>` that uses the hash portion of the URL (i.e. window.location.hash) to keep your UI in sync with the URL.

#### which router to use?
Generally speaking, you should use a `<BrowserRouter>` if you have a server that responds to requests and a `<HashRouter>` if you are using a static file server.

### How to set it up
- define the parent router in `index.js`, the entry point to the client app
   - if using webpack, this would be the `entry` specified in the webpack config file
   - it'll wrap the `App.jsx` (i.e., the parent React component for the client-side web application)
   - Example:
      ```
      import React from 'react';
      import { render } from 'react-dom';
      
      render((
         <Router>
            <App />
         </Router>
      ), document.getElementById('app')); // the unique id for the div element in index.html where the react client should be rendered
      ```
   - questions:
      - there are different routers that can be used - Router, BrowserRouter, HashRouter, etc - when to use which?
 
 ### Sample patterns
 ```
<BrowserRouter>
<Switch>
   <Route exact path={match.url} render={() => {
   <Route path={`${match.url}/a`} render={() => <A />} />
   <Route path={`${match.url}/b`} render={() => <B />} />
   <Route path={`${match.url}/c`} render={() => <C />} />
</Switch>
</BrowserRouter>
```

 ```
<Router>
   <Route path="a" component={<AView} />
   <Route path="b" component={<BView} />
   <Route path="c" component={<CView} />
</Router>
```


### Packages
https://github.com/ReactTraining/react-router#packages

## Why use express with react
to serve your `index.html` and your `bundle.js` files, when a user first visits the site, (www.example.com)
to redirect the user to www.example.com when someone directly visits www.example.com/subpage, which is typically handled by react-router-dom on the client,
to serve static assets like icons and images on your page
as an API backend for getting data from the server, etc 

## Where to put your html files?
What I frequently see when people use Node with Express to act as their webserver is the debate on if you should put your html files with node or with the client code.

Where you store your files on the server doesn't matter. What does matter is that you don't generally want to serve static files from your Node.js application. Tools like express.static are for convenience only. Sometimes, you may have a low traffic application. In these cases, it is perfectly acceptable to serve files with your Node.js app. For anything with a decent traffic load, it's best to leave static serving up to a real web server such as Nginx, since these servers are far more efficient than your Node.js application.

You should keep your application code (code that serves dynamic responses, such as an API server) within your Node.js application.

It's also a good idea to put your Node.js application behind a proxy like Nginx so that the proxy can handle all of the client interaction (such as spoon-feeding slow clients) leaving your Node.js application to do what it does best. Again though, in low traffic situations it doesn't matter.
https://stackoverflow.com/questions/28918845/what-exactly-does-serving-static-files-mean

## Questions
1. if a `sass-loader` is specified in `webpack.config.js`, is it necessary to have something specified in the package.json to convert scss to css?
