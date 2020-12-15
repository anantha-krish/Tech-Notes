Integrating vue project

## Vue component


vue dashboard

[components (1).zip](../_resources/fd17745279934031a04f3b7e2dec6cd8.zip)

add to components folder
## Webpack Config
- common
```js
const { VueLoaderPlugin } = require('vue-loader');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: '[name].[contenthash].js',
  },
  resolve: {
    extensions: ['.js', '.vue'],
  },
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|woff|svg|eot|ttf)$/i,
        use: [{ loader: 'file-loader' }],
      },
      {
        test: /\.vue$/,
        use: 'vue-loader',
      },
      {
        test: /\.scss|\.css$/,
        use: ['vue-style-loader', 'style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-transform-runtime'],
          },
        },
      },
    ],
  },
  plugins: [new VueLoaderPlugin()],
};

```
- dev
```
const {merge} = require ('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const commonConfig = require('./webpack.common');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')
//imported for shared dependecy config
const packageJson = require('../package.json');

const devConfig ={
    mode: 'development',
    output:{
        publicPath: 'http://localhost:8083/'
    },
    devServer:{
        port:8083,
        //used for navigation
        historyApiFallback:{
            index:'index.html'
        },
		//required for resolving CORS issue
        headers:{
            'Access-Control-Allow-Origin':'*'
        }
    },
    plugins:[
        new ModuleFederationPlugin({
            name:'dashboard',
            filename:'remoteEntry.js',
            exposes:{
                './DashboardApp':'./src/bootstrap.js'
            },
            /* Shortcut method to ensure single copy 
            No need to update shared Dependecy list, everytime
            new dependecy is added
            But its better if you can specify the package to share,
            but then you have to update it evrytime new dependency added.
            */
            shared: packageJson.dependencies
        }),

        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
}
// mentioning dev config allows to override common conconfig.
module.exports = merge(commonConfig,devConfig)
```
- production
```
const {merge} = require('webpack-merge');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');
const commonConfig = require('./webpack.common');
const packageJson = require('../package.json');

const prodConfig = {
    // slower but ensure production optimized build
    mode:'production',
    output :{
        // contenthash resolves caching issue
      filename: '[name].[contenthash].js',
      publicPath: '/dashboard/latest/'
    },
    plugins:[
        new ModuleFederationPlugin({
            name:'dashboard',
            filename:'remoteEntry.js',
            exposes:{
                './DashboardApp':'./src/bootstrap.js'
            },
            shared: packageJson.dependencies
        })
    ]
}


module.exports= merge(commonConfig,prodConfig);
```

## bootstrap.js
```
import {createApp} from "vue";
import Dashboard from "./components/Dashboard.vue";


const mount =(el) => {
    const app = createApp(Dashboard)
    app.mount(el);
};

if (process.env.NODE_ENV === "development") {
    const devRoot = document.querySelector("#_dashboard_dev_root");
    if (devRoot) {
      mount(devRoot);
    }
  }
  
  export {mount}
  ```
  ## package.json
 ```json
  {
  "name": "dashboard",
  "version": "1.0.0",
  "scripts": {
    "start":"webpack serve --config ./config/webpack.dev.js",
    "build":"webpack --config ./config/webpack.prod.js"
  },
  "dependencies": {
    "chart.js": "^2.9.4",
    "primeflex": "^2.0.0",
    "primeicons": "^4.0.0",
    "primevue": "^3.0.1",
    "vue": "^3.0.0"
  },
  "devDependencies": {
    "@babel/core": "^7.12.3",
    "@babel/plugin-transform-runtime": "^7.12.1",
    "@babel/preset-env": "^7.12.1",
    "@vue/compiler-sfc": "^3.0.2",
    "babel-loader": "^8.1.0",
    "css-loader": "^5.0.0",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^4.5.0",
    "node-sass": "^4.14.1",
    "sass-loader": "^10.0.4",
    "style-loader": "^2.0.0",
    "vue-loader": "^16.0.0-beta.9",
    "vue-style-loader": "^4.1.2",
    "webpack": "^5.4.0",
    "webpack-cli": "^4.1.0",
    "webpack-dev-server": "^3.11.0",
    "webpack-merge": "^5.2.0"
  }
}
```