# Building a React Boilerplate with Webpack

To use Webpack, we need to understand the Webpack configuration. Below is a basic Webpack configuration:

```javascript
    module.exports = {
        entry: "",
        output: {},
        devServer: {},
        devtool: debug ? "cheap-module-eval-source-map" : false,
        resolve: {},
        module: {
            rules: []
        },
        plugins: [],
        optimization: {}
    };
```
* entry: This takes the name of a file as its value. It sets the entry point, where Webpack will start the bundling from. This file will be at the top of the dependency graph. We can have more than one entry file, so the value of entry can be a string, object or an array. If we don't specify the entry, Webpack sets it to src/index.js by default.

* output: This option is an object that contains several options that determine how Webpack emits results. This key determines several things about the output most importantly the directory where bundled files can be kept and how to name the outputted bundled files.

* devServer: This option is modify the behaviour of the webpack-dev-server.
webpack-dev-server enables the use of webpack with a development server that provides live reloading. Strictly for development only.

* devtool: This option is used to specify if we want source maps for our code and if we do, what kind of source maps do we want.
A source map is a file that contains information that enables us map a compressed/minified/uglified file to its uncompressed original file.
It is helpful for debugging using Developer Tools in the browser.

* resolve: This property controls how file paths, extensions, etc are resolved in our codebase. For example if JavaScript files are set to be automatically resolved:

```javascript
// We can do:
import someFunction from 'some-file'

// Instead of:
import someFunction from 'some-file.js'
```

* module: This option is used to specify how Webpack should handle and parse the different types of modules in a codebase. Webpack can handle different type of modules written in various languages, it does this using loaders written for these specific modules. So, we loaders to parse and bundle non-javascript modules/files. We use this configuration option in tandem with loaders to load, parse and bundle files like images, stylesheets (css, scss, sass, less), typescript files, etc.

* plugins: This is used to customize the build process and add external plugins.

* optimization: This is used to override the default optimizations that Webpack runs for you based on the build mode. Depending on the set build mode (development or production), Webpack runs optimizations.

## Building The Boilerplate
* Create and initialize an empty project with npm.
* Install devDependencies.


```powershell
npm install --save-dev @babel/cli @babel/core @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators @babel/plugin-proposal-object-rest-spread @babel/plugin-syntax-dynamic-import @babel/preset-env @babel/preset-react babel-loader
```    
**@babel/cli** - The babel command line interface that allows us compile files from the command line.

**Babel** - It is JavaScript toolchain compiler that converts JavaScript written in ES2015 and above into a backwards compatible version of JavaScript that works in current and     older browsers.
    
Babel provides the following:
* Syntax Transformation: Converts the syntax of modern JavaScript into backwards compatible JavaScript.
* Polyfills: Provides fallbacks for features that are not supported on the target browsers. It does this using @babel/polyfill
* Source Code Transformation.

**@babel/core**: This is core Babel compiler.<br/>
**@babel/plugin-proposal-class-properties**: @babel/plugin-proposal-** enables us use features that have been proposed but have not been approved in JavaScript.<br/>
This particular one enables us use static class properties.

Static properties of a class are called on the class itself and not an instance of the class.

```javascript
class StaticClass {
    static name = 'StaticClass';
}

const instance = new StaticClass();

console.log(instance.name);
// returns undefined

console.log(StaticClass.name);
// returns 'StaticClass'
```
**@babel/plugin-proposal-decorators**: This allows us use the @Decorator syntax in the codebase. 
**@babel/plugin-syntax-dynamic-import**: This allows parsing of import() statements.
**@babel/plugin-proposal-object-rest-spread**: This allows us use the rest and spread operators.

```javascript
/**
    SPREAD: Allows us unpack the values of an iterator(array) into different individual values. Its syntax is denoted by three ellipses (...) in front of the iterator
**/

const array = [1, 2, 3, 4, 5, 6];
console.log(...array);
// returns 1 2 3 4 5 6

/**
    REST: Packs several values into a single iterator variable. Its syntax is denoted by three ellipses (...) in front of the iterator variable.
**/

const names = ['Abraham', 'Isaac', 'Jacob'];
const [first, rest] = names;
console.log(first);
// returns 'Abraham'
console.log(rest);
// returns ['Isaac', 'Jacob']
```

**@babel/preset-env**: A babel preset that allows use the latest JavaScript without having to micromanage the syntax transforms needed for our target environment.
**@babel/preset-react**: This allows the compilation of JSX into backwards-compatible JavaScript. React is built on JSX, so this help us write React.
**babel-loader**: A loader that will be used in our Webpack configuration to process JavaScript and JSX files.

* Install webpack-related packages. These packages include loaders, presets, plugins and core webpack and webpack-dev-server packages. They are devDependencies

```powershell
npm install --save-dev circular-dependency-plugin compression-webpack-plugin copy-webpack-plugin css-loader file-loader html-webpack-plugin mini-css-extract-plugin node-sass sass-loader style-loader terser-webpack-plugin url-loader webpack webpack-bundle-analyzer webpack-cli webpack-dev-server
```
The most important to note here are **webpack**, **webpack-cli** and **webpack-dev-server**.

* Install React-related packages. These packages are not devDependencies.

```powershell
    npm install react react-dom
```
**react**: The core react UI library.<br/>
**react-dom**: This package provides DOM-specific methods that can be used from the top-level of your app. It basically allows interaction between the React UI library and the DOM.

* Create a folder src and in it add an index.html file (the single page of our single-page application)
* In the body of the index.html, add a div with the id app which is where our React components will be injected into. Also add a noscript fallback to be displayed when JavaScript is not enabled on the browser

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="description" content="React Boilerplate (JavaScript)" >
    <title>React JavaScript Boilerplate</title>
</head>
<body>
    <div id="app"></div>
    <noscript>
        You need JavaScript enabled to run this application
    </noscript>
</body>
</html>
```

* In the src directory create the index.js file which will be the entry file for Webpack.

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import App from './core/App.js'


ReactDOM.render(<App />, document.getElementById('app'));
```
* Create a directory core where the root component of our React application will reside. 
* In the core directory create the component file and stylesheet for the root component App.js and App.css.

```jsx
// App.js

import React, { Component } from 'react';
import './App.css';


class App extends Component {
    render() {
        return (
            <div className="app">
                <header className="app-header">
                    React Javascript Webpack Boilerplate
                </header>
            </div>
        )
    }
}

export default App;
```
```css
// App.css

*{
    padding: 0;
    margin: 0;
}


.app {
    background-color: #282c34;
    text-align: center;
}


.app-header {
    height: 70px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: calc(30px + 2vmin);
    color: white;
    padding: 20px;
    cursor: pointer;
}
```

After adding all this boilerplate code, we can run our application. To do so, we'll have to use webpack and the webpack-dev-server.
* Add a script in the package.json file that runs the webpack-dev-server, bundles, serves our files on the dev-server and hot-reloads on changes.

    It hot-reloads because webpack-dev-server uses wepback in watch mode, so it watches our codebase for changes and builds and reloads if changes have been detected.

```json
...  
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "serve": "webpack-dev-server --mode development --hot"
      }
...
```

* Now we can run the command npm run serve.<br/> We will get an error because we are using jsx and we have not instructed Webpack on how to parse and bundle jsx files.

```powershell
Module parse failed: Unexpected token (5:16)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
| import App from './core/App.js';
|
> ReactDOM.render(<App />, document.getElementById('app'));
```

So now we have to add our custom configuration to Webpack to fix this error and other errors.

* Add a webpack.config.js file in the root of the directory. This is where our custom Webpack configuration is going to be.
* First, we have to import some of those packages (devDependencies) we installed earlier.

```javascript
const debug = process.env.NODE_ENV !== 'production';
const webpack = require('webpack');
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CircularDependencyPlugin = require('circular-dependency-plugin');
const CompressionPlugin = require('compression-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');
```

Most of the plugins we are importing are self-explanatory. We are using require to import packages because Webpack uses Node for its bundling and previous versions of Node do not support ES modules.
* Now we create the configuration object and add the entry config 

```javascript
module.exports = {
    entry: './src/index.js'
}
```

* Then we add the output config which tells Webpack how to output the bundled JavaScript in terms of output location and naming convention.

```javascript
output: {
    publicPath: '/',
    path: path.join(__dirname, 'build'),
    filename: 'js/[name].bundle.min.js',
    chunkFilename: 'js/[name].bundle.js'
}
```
**publicPath** tells webpack to serve all files from the / path in the browser.
 
Next we add the configurations for the webpack-dev-server.

```javascript
devServer: {
    inline: true,
    contentBase: './src',
    port: 8080,
    historyApiFallback: true,
    host: '0.0.0.0'
}
```
inline is a boolean that specifies what mode the dev-server will run in. <br/>
The dev-server has two modes: inline and iframe.<br/>
In **inline** mode, build messages and notifications will appear in the browser console.<br/>
In **iframe** mode, an iframe is used under a notification bar with messages about the build.

**contentBase** tells the dev-server where to serve files from.

**port** tells the dev-server what port to serve the application on.

**historyApiFallback** is a boolean that specifies how the dev-server handles 404s. When it is true, the index.html is going to be served in the place of a 404 page. It can also take an object that can be used to control the behaviour in a more advanced way. 

**host** tells the dev-server what host to use. By default its value is 'localhost', but if we want our dev-server to be accessible externally, we can use '0.0.0.0'.

* Next we'll add the devtool config. This handles the creation of sourcemaps for our codebase.



```javascript
devtool: debug ? 'cheap-module-eval-source-map' : false
```
First we check if we are in the dev environment and if we are we use the cheap-module-eval-source-map devtool.<br/>
This devtool is slow to build but fast to rebuild and it produces sourcemaps which are just as our original code without transpilation. If we are not in the dev environment the value is set to false and no sourcemaps are created.

* Next we'll add the resolve config. We will tell Webpack how to resolve the .js and .jsx extensions.



```javascript
resolve: {
    extensions: ['.js', '.jsx']
}
```
So we can import .js and .jsx files without adding the extension in the import statement and Webpack will resolve that for us.

* Next we'll add the module config. This is where we tell Webpack how to load, parse and handle different types of files. In the module config, we have rules which is an array of rule objects that tells Webpack what type of loaders to use to handle a particular file type of set of filetypes.
* First, we'll add the rule for parsing and loading js and jsx files.

```javascript
module: {
    rules: [
        {
            test: /\.(js|jsx)$/,
            exclude: /(node_modules)/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['@babel-env', '@babel/preset-react'],
                    plugins: [
                        '@babel/plugin-proposal-class-properties',
                        '@babel/plugin-syntax-dynamic-import',
                        '@babel/plugin-proposal-object-rest-spread',
                        ['@babel/plugin-proposal-decorators', { legacy: true }]
                    ]
                }
            }
        }
    ]
}
```
```javascript
**test**: This is RegExp that finds all files in the codebase that match the RegExp pattern.
**exclude**: This tells Webpack what directories/files not to handle.
**use**: This tells Webpack what loader to use to handle these file types.
**use.loader**: The loader to use.
**user.options**: The loader options.
```

* Next we'll add the rule for loading SCSS, SASS and CSS files.

```javascript
{
    test: /\.(sa|sc|c)ss$/,
    use: debug ? ['style-loader', 'css-loader', 'sass-loader'] : [MiniCssExtractPlugin, 'css-loader', 'sass-loader'],
}
```

* Next we'll add the rule for loading fonts and SVGs.

```javascript
{
    test: /\.(eot|tff|woff|woff2|otf|svg)$/,
    use: [
        {
            loader: 'url-loader',
            options: {
                limit: 100000,
                name: './assets/fonts/[name].[ext]'
            }
        }
    ]
}
```

* Lastly we'll add the rule for loading images (png, jpeg, jpg, gif).

```javascript
{
    test: /\.(gif|bmp|png|jpe?g)$/i,
    use: {
        loader: 'file-loader',
        options: {
            outputPath: 'assets/images/'
        }
    }
}
```

* Next we'll configure plugins. We'll be doing configuring plugins based on the environment, whether develpoment or production.

```javascript
plugins: debug ? [
    new CircularDependencyPlugin({
        // exclude detection of files based on a RegExp
        exclude: /a\.js|node_modules/,
        // add errors to webpack instead of warnings
        failOnError: true,
        // set the current working directory for displaying module paths
        cwd: process.cwd()
    }),
    new HtmlWebpackPlugin({
        template: "./src/index.html"
    })
] : [
    // define NODE_ENV to remove unnecessary code
    new webpack.DefinePlugin({
        "process.env.NODE_ENV": JSON.stringify("production")
    }),
    new webpack.optimize.OccurrenceOrderPlugin(),
    new webpack.optimize.AggressiveMergingPlugin(), // Merge chunks
    // extract imported css into own file
    new MiniCssExtractPlugin({
        // Options similar to the same options in webpackOptions.output
        // both options are optional
        filename: "[name].css",
        chunkFilename: "[id].css"
    }),
    new webpack.LoaderOptionsPlugin({
        minimize: true
    }),
    new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/),
    new HtmlWebpackPlugin({
        template: "./src/index.html"
        // minify: {
        //   collapseWhitespace: true,
        //   removeAttributeQuotes: false
        // }
    }),
    new CompressionPlugin({
        test: /\.(html|css|js|gif|svg|ico|woff|ttf|eot)$/,
        exclude: /(node_modules)/
    }),
    new BundleAnalyzerPlugin()
]
```

* Finally we'll add the optimization config.

```javascript
optimization: {
    minimizer: [
        new TerserPlugin({
            cache: true,
            parallel: true,
            sourceMap: true, // Must be set to true if using source-maps in production
            terserOptions: {
                ie8: true,
                safari10: true,
                sourceMap: true
            }
        })
    ]
}
```

* Now after configuring Webpack, we can run the npm run serve and see our application in action.






