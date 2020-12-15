Generic ties between project

index.html

```html<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Container Page</title>
</head>
<body>
    <div id="_container_root"></div>
</body>
</html>
```

bootstrap.js
```
import React from "react"
import ReactDOM from "react-dom";
import App from "./App.jsx"

ReactDOM.render(<App/> ,document.querySelector('#_container_root'));
```

app.jsx
```
import React from "react";
import MarketingApp from "./components/MarketingApp.jsx"


export default () => {
  return (
    <div>
   Hi there!!
   <hr/>
   <MarketingApp/>
    </div>
  );
};
```

marketing
```js
import React, { useEffect, useRef } from "react";
// marketing is configured in webpack
import { mount } from "marketing/MarketingApp";

export default () => {

  // implement logic to mount the MicrofrontEnd when DOM is loaded 
  useEffect(() => {
    // call mount while passing the element
    mount(ref.current);
    
  }, []);
 
  // create reference
  const ref = useRef(null);
  return <div ref={ref}></div>;

};
```

webpack.dev
```
const {merge} = require ('webpack-merge');
const commonConfig = require('./webpack.common');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')
const packageJson = require('../package.json')
const devConfig ={
    mode: 'development',
    devServer:{
        port:8080,
        //used for navigation
        historyApiFallback:{
            index:'index.html'
        }
    },
    plugins:[
        new ModuleFederationPlugin({
            name:'container',
            remotes:{
               marketing:'marketing@http://localhost:8081/remoteEntry.js' 
            },
            shared: packageJson.dependencies
        }),
    ]
}
// mentioning dev config allows to override common conconfig.
module.exports = merge(commonConfig,devConfig)
```