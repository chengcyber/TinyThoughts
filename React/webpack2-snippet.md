# Webpack 2

install

```
npm i -g webpack webpack-dev-server@2
yarn add --dev webpack webpack-dev-server@2

yarn add --dev babel-loader babel-core babel-preset-es2015 babel-preset-react css-loader style-loader

```

index.html

```html
  <script src="dist/vendor.bundle.js"></script>
  <script src="dist/app.bundle.js"></script>
```

webpack.config.js

```js
const path = require('path');
const webpack = require('webpack');
module.exports = {
  context: path.resolve(__dirname, './app'),
  entry: {
    app: './index.js',
    vendor: ['react', 'react-dom'],
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].bundle.js',
    // publicPath: '/assets',
  },
  devServer: {
    contentBase: path.resolve(__dirname, './src'),  // New
  },
  module: {
    rules: [

      {
        test: /\.jsx?$/,
        exclude: [/node_modules/],
        use: [{
          loader: 'babel-loader',
          options: { presets: ['es2015', 'react'] },
        }],
      },

      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  resolve: {
    // options for resolving module requests
    // (does not apply to resolving to loaders)

    modules: [
      "node_modules",
      path.resolve(__dirname, "app")
    ],
    // directories where to look for modules

    extensions: [".js", ".json", ".jsx", ".css"],
    // extensions that are used

    alias: {
      // a list of module name aliases

      "module": "new-module",
      // alias "module" -> "new-module" and "module/path/file" -> "new-module/path/file"

      "only-module$": "new-module",
      // alias "only-module" -> "new-module", but not "module/path/file" -> "new-module/path/file"

      "module": path.resolve(__dirname, "app/third/module.js"),
      // alias "module" -> "./app/third/module.js" and "module/file" results in error
      // modules aliases are imported relative to the current context
    },
    /* alternative alias syntax (click to show) */

    /* Advanced resolve configuration (click to show) */
  },
  // plugins: [
  //   new webpack.optimize.CommonsChunkPlugin({
  //     names: 'commons',
  //     filename: 'commons.js',
  //     minChunks: 2,
  //   }),
  // ],
};
```



