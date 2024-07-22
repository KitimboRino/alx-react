# Webpack Setup and Configuration Guide

## Introduction

Webpack is a powerful module bundler for JavaScript applications. It takes modules with dependencies and generates static assets representing those modules. This guide provides a comprehensive overview of setting up Webpack for a basic project, including configuring entry points, output, loaders, adding plugins, code splitting, and setting up a development server.

## Table of Contents

1. [Setting Up Webpack for a Basic Project](#setting-up-webpack-for-a-basic-project)
2. [Entry Points, Output, and Loaders](#entry-points-output-and-loaders)
3. [Adding Plugins](#adding-plugins)
4. [Code Splitting](#code-splitting)
5. [Setting Up a Dev Server](#setting-up-a-dev-server)

## Setting Up Webpack for a Basic Project

### Step 1: Install Webpack and Webpack CLI

First, you need to install Webpack and its CLI tool as development dependencies.

```bash
npm install --save-dev webpack webpack-cli
```

### Step 2: Create a Basic Project Structure

Create the following directory structure for your project:

```
my-webpack-project/
├── src/
│   ├── index.js
│   └── style.css
├── dist/
├── package.json
├── webpack.config.js
```

### Step 3: Create `webpack.config.js`

This file will contain the Webpack configuration.

```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
        ],
    },
};
```

## Entry Points, Output, and Loaders

### Entry Points

The entry point is the file where Webpack starts bundling your code.

```javascript
entry: './src/index.js',
```

### Output

The output property specifies the location and name of the bundled file.

```javascript
output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
},
```

### Loaders

Loaders allow you to preprocess files as you `import` or `load` them. Here’s an example of using loaders for CSS files.

```javascript
module: {
    rules: [
        {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
        },
    ],
},
```

## Adding Plugins

Plugins are used to perform more complex tasks, like optimizing the output, managing assets, and injecting environment variables.

### Example: HTMLWebpackPlugin

Install the plugin:

```bash
npm install --save-dev html-webpack-plugin
```

Update `webpack.config.js`:

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
        ],
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html',
        }),
    ],
};
```

## Code Splitting

Code splitting is used to split your code into various bundles that can be loaded on demand or in parallel.

### Example: Dynamic Imports

```javascript
// In your `src/index.js`
import('./another-module').then(module => {
    module.default();
});

// In `webpack.config.js`
module.exports = {
    entry: './src/index.js',
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
        ],
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html',
        }),
    ],
    optimization: {
        splitChunks: {
            chunks: 'all',
        },
    },
};
```

## Setting Up a Dev Server

The Webpack Dev Server provides live reloading and other development features.

### Step 1: Install Webpack Dev Server

```bash
npm install --save-dev webpack-dev-server
```

### Step 2: Update `webpack.config.js`

```javascript
module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
        ],
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html',
        }),
    ],
    devServer: {
        contentBase: path.join(__dirname, 'dist'),
        compress: true,
        port: 9000,
        open: true,
    },
};
```

### Step 3: Add a Script to `package.json`

```json
"scripts": {
    "start": "webpack serve --open",
    "build": "webpack"
}
```

## Conclusion

This guide covers the basic setup and configuration of Webpack for a project. By following these steps, you can leverage Webpack's powerful features to bundle your JavaScript applications efficiently. Happy coding!