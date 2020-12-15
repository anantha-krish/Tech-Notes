Authentication Process

auth/webpack
```js
const {merge} = require ('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const commonConfig = require('./webpack.common');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')
//imported for shared dependecy config
const packageJson = require('../package.json')

const devConfig ={
    mode: 'development',
    output:{
        publicPath: 'http://localhost:8082/'
    },
    devServer:{
        port:8082,
        //used for navigation
        historyApiFallback:{
            index:'index.html'
        }
    },
    plugins:[
        new ModuleFederationPlugin({
            name:'auth',
            filename:'remoteEntry.js',
            exposes:{
                './AuthApp':'./src/bootstrap.js'
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
auth/bootstrap
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { createMemoryHistory, createBrowserHistory } from "history";

const mount = (el, { onSignIn, onNavigate, defaultHistory, initialPath }) => {
  //use browsererhistory in isolation(dev) otherwise use memoryhistory
  const history =
    defaultHistory ||
    createMemoryHistory({
      initialEntries: [initialPath],
    });

  if (onNavigate) {
    history.listen(onNavigate);
  }
  ReactDOM.render(<App history={history} onSignIn={onSignIn} />, el);

  return {
    onParentNavigate: ({ pathname: nextPathname }) => {
      const { pathname } = history.location;
      if (pathname !== nextPathname) {
        history.push(nextPathname);
      }
    },
  };
};

if (process.env.NODE_ENV === "development") {
  const devRoot = document.querySelector("#_auth_dev_root");
  if (devRoot) {
    mount(devRoot, { defaultHistory: createBrowserHistory() });
  }
}

export { mount };

```
auth/App.js
communication is done by call backs
```
import React from "react";
import { Switch, Router, Route } from "react-router-dom";

import { StylesProvider ,createGenerateClassName} from "@material-ui/core";
import SignIn from "./components/Signin";
import SignUp from "./components/Signup";

const generateClassName  = createGenerateClassName(
  {
    productionPrefix:'au'
  }
)
//accepts history as props
export default ({history,onSignIn}) => {
  return (
    <div>
      <StylesProvider generateClassName={generateClassName}>
        <Router history={history}>
          <Switch>
              <Route path="/auth/signin">
                <SignIn onSignIn={onSignIn}/>
                </Route>
              <Route path="/auth/signup">
                <SignUp onSignIn={onSignIn}/>
                </Route> 
          </Switch>
        </Router>
      </StylesProvider>
    </div>
  );
};

```
## container updates
- update webpack.dev
```js
            remotes:{
               marketing:'marketing@http://localhost:8081/remoteEntry.js',
               auth :'auth@http://localhost:8082/remoteEntry.js'
            },
```
- update webpack.prod
```js
       remotes: {
                marketing : `marketing@${domain}/marketing/latest/remoteEntry.js`,
                auth :`auth@${domain}/auth/latest/remoteEntry.js`
            },
```	
- update app.js

```export default () => {
 
  const [isSignedIn, setIsSignedIn] = useState(false);
  
  return (
    
      <BrowserRouter>
        <StylesProvider generateClassName={generateClassName}>
          <div>
            <Header isSignedIn={isSignedIn} onSignOut={()=>setIsSignedIn(false)}/>
            <Suspense fallback={<Progress/>}>
            <Switch>
              <Route path="/auth">
                <AuthApp onSignIn={()=>setIsSignedIn(true)} />
                </Route> 
              <Route path="/" component={MarketingApp} />
            </Switch>
            </Suspense>
          </div>
        </StylesProvider>
      </BrowserRouter>

  );
};
```
- authApp
```js
import React, { useEffect, useRef } from "react";
// marketing is configured in webpack
import { mount } from "auth/AuthApp";
import { useHistory } from "react-router-dom";

export default ({onSignIn}) => {
  // create reference
  const ref = useRef(null);
  const history = useHistory();

  // implement logic to mount the MicrofrontEnd when DOM is loaded
  useEffect(() => {
    // call mount while passing the element 
    const { onParentNavigate } = mount(ref.current, {
      // destructioring window.location
      onNavigate: ({ pathname: nextPathname }) => {
        const { pathname } = history.location;
        if (pathname !== nextPathname) {
          history.push(nextPathname);
        }
      },
      onSignIn,
      initialPath: history.location.pathname
    });
    //listen to the path changes
    if (onParentNavigate) {
      history.listen(onParentNavigate);
    }
  }, []);

  return <div ref={ref}></div>;
};

```


