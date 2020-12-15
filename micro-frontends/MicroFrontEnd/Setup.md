Setup

Create  main folder ecomm


## Micro Frontend 1 (List of products)

* create sub folder products

```
npm init -y

npm install webpack@5.4.0 webpack-cli@4.2.0 webpack-dev-server@3.11.0 faker@5.1.0 html-webpack-plugin@4.5.0
```
* create index.js (fake Products) in src 

```js
import faker from 'faker';

let products = '';

for(let i=0;i<5;i++)
{
    const name = faker.commerce.productName();
    products += `<div>${name}</div>`;
}

document.querySelector('#dev-products').innerHTML = products;
```
* create index.html in public folder
```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    <div id="dev-products"></div>
</body>
</html>
```
* add webpack configuration

```js
const HtmlWebpackplugin = require('html-webpack-plugin');

module.exports = {
    mode: 'development',
    devServer: {
       port: 8081 
    },
    plugins: [
        new HtmlWebpackplugin({
            template:'./public/index.html'
        })
    ]
};

```

* update package configuration

```json

  "scripts": {
    "start": "webpack serve"
  },
```


## Micro Frontend 2 (Container)

setup after creating a container folder

```
npm install html-webpack-plugin@4.5.0 nodemon webpack@5.3.2 webpack-cli@4.2.0 webpack-dev-server@3.11.0

```

follow previous steps of creating index.js,index.html, webpack.config.js(with different port)
