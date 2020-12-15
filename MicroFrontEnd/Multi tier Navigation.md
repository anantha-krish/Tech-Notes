Multi tier Navigation

Routing issue
when clicked on App, it doesnot load homepage
![8af5f75af19d62da3125626d5d4abd2a.png](../_resources/825d39fac74f42f5be46d402dcd11644.png)

# History Implementation

![e1bf6c43d2941aa22b4eeb2f02f6c19e.png](../_resources/3165b67aaa194e318445d62b2b7a3947.png)

Use browserHistory for container(parent)
( i.e  BrowserRouter)

& MemoryHistory for the childrens
(i.e Router)

# Memory History Implementation

1. update App.jsx
```js
import { Switch, Router, Route } from "react-router-dom";
//accepts history as props
export default ({history}) => {
  return (
    <div>
      <StylesProvider generateClassName={generateClassName}>
      //added  
	  <Router history={history}>
          <Switch>
              <Route exact path="/pricing" component={Pricing}/>
```
2. Update bootstrap.js
```js
import {createMemoryHistory} from "history"

const mount = (el) =>{
    const history = createMemoryHistory();
    ReactDOM.render(
    <App history={history}/> ,el)
}

```

### Sync issue between Browser History & Memory History

After above integration , you may find that routing logic wont work properly as there is no proper synce between browser & memory History.
Example:

![e476b49fd5235f7aa73cfd7c906b9e9e.png](../_resources/e2db31e45e1b42e0bb41145cfd19b65e.png)

## Two way communication
marketing/bootstrap.js
```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App.jsx";
import { createMemoryHistory, createBrowserHistory } from "history";

const mount = (el, { onNavigate, defaultHistory }) => {
    //use browsererhistory in isolation(dev) otherwise use memoryhistory
  const history = defaultHistory || createMemoryHistory();
  if (onNavigate) {
    history.listen(onNavigate);
  }
  ReactDOM.render(<App history={history} />, el);

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
  const devRoot = document.querySelector("#_marketing_dev_root");
  if (devRoot) {
    mount(devRoot, { defaultHistory: createBrowserHistory() });
  }
}

export { mount };
```


2. parent container
container/components/MarketingApp.jsx
```js
import React, { useEffect, useRef } from "react";
// marketing is configured in webpack
import { mount } from "marketing/MarketingApp";
import { useHistory } from "react-router-dom";

export default () => {
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
    });
    //listen to the path changes
    if (onParentNavigate) {
      history.listen(onParentNavigate);
    }
  }, []);

  return <div ref={ref}></div>;
};

```