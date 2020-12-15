Module Federation

# Module Federation

### Planning

![9483785f39618bf49294169f671ad843.png](../_resources/d298e26eb11443d5867b00f5a6ad6d65.png)

Here host is container & remote is products
## Steps

1. Configure remote webpack
```js 
const HtmlWebpackplugin = require('html-webpack-plugin');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
    mode: 'development',
    devServer: {
       port: 8081 
    },
    plugins: [
		//newly added
        new ModuleFederationPlugin({
            name:'products',
            filename: 'remoteEntry.js',
            exposes:{
                './ProductsIndex':'./src/index'
            },
        }),

        new HtmlWebpackplugin({
            template:'./public/index.html'
        })
    ]
};
```
3. configure host webpack
```js
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
    mode: 'development',
    devServer:{
        port: 8080
    },
    plugins:[
		//newly addeded
        new ModuleFederationPlugin({
         name:'container',
         remotes:{
             products:'products@http://localhost:8081/remoteEntry.js'
         },import('./bootstrap')
        }),
```
5. Refactor  index.js (bootstrapping)

bootstrap.js
```js
import 'products/ProductsIndex'
console.log('container!!!')
```
index.js
```js
//note here we are using functional import, so that 
//webpack will acquire all required dependencies
import('./bootstrap')
```


7. Import the files & changes ain index.html
```html
<div id="dev-products"></div>
```
output
![3c696c273422d79201b5a6ea52832a53.png](../_resources/95ba784ee68d413d91034dd50fb566e9.png)

