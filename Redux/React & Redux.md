React & Redux

Author: Anantha Krishnan

# Basic react redux setup
Creating project, execute cmds

```shell
npx create-react-app react-redux-demo

npm add redux react-redux
```

### Vs extension

ES7 React/Redux/GraphQL snippets

type rfc to create functional component

Note: creating action & dispatch is similar refer redux Notes.

## Store

Example:
```js
import {createStore} from 'redux';
import cakeReducer from './CakeReducer';

const store = createStore(cakeReducer)
export default store;
```
Then use Provider in App.js

```js
import { Provider } from "react-redux";
import CakeContainer from "./components/CakeContainer";
import store from "./redux/cakes/CakeStore";

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <CakeContainer />
      </div>
    </Provider>
  );
}

export default App;

```

## Connecting App with redux
The redux states & dispatch is passed as props

```js
import React from 'react'
import { buyCake } from '../redux/cakes/CakeActions'
import { connect } from 'react-redux'

function CakeContainer(props) {
    return (
        <div>   
            <h2>Number of Cake{props.numberOfCakes}</h2>
            <button onClick={props.buyCake}>Buy Cake</button>     
        </div>
    )
}
//Pass redux state as props
const mapStateToProps = state => {
    return {
        numberOfCakes : state.numberOfCakes
    }
}
//Pass redux dispatch as props
const mapDispatchToProps = dispatch => {
    return {
        buyCake :()=> dispatch(buyCake())
    }
}

// use connect statement
export default connect(mapStateToProps,mapDispatchToProps) (CakeContainer)

```

# Redux with Hooks (shorter code)
With hooks, you can reduce the lines of code.
there is no need of connect stmnt.

## useSelector


```js
import { useSelector } from 'react-redux'

function HooksCakeContainer() {
     						//used instead of mapStateToProps
    const numberOfCakes = useSelector(state=>state.numberOfCakes)
    return (
        <div>
               <h2>Number of Cake{numberOfCakes}</h2>
```


## useDispatch

```js
import React from 'react'
import { useSelector,useDispatch } from 'react-redux'
import { buyCake } from '../redux/cakes/CakeActions';

function HooksCakeContainer() {

    const numberOfCakes = useSelector(state=>state.numberOfCakes)
   //used instead of mapDispatchToProps
    const dispatch = useDispatch();
    return (
        <div>
               <h2>Number of Cake{numberOfCakes}</h2>
            <button onClick={()=>dispatch(buyCake())}>Buy Cake</button>     
        </div>
    )
}

export default HooksCakeContainer

```

It's okay to use Hooks as it provides shorter codes, but there are some usage  <b>WARNINGS</b>, do check out the document.

## combining reducers in react

step1: add a root reducer as follow & pass it to createStore(). 

```js

import {combineReducers}  from "redux";
import cakeReducer from "./cakes/CakeReducer";
import iceCreamReducer from "./ice-creams/IceCreamReducer";

const rootReducers = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
})

export default rootReducers;

```

after this you need to access the states differently.

state.cake.numberOfCakes   // related to cake
state.iceCream.numberOfIceCreams  // related to ice creams


## Install Redux devtools

Step1: add redux tool extension
``
npm install --save redux-devtools-extension
``

Step2: modify store.js
```js
import {composeWithDevTools} from 'redux-devtools-extension'

import rootReducers from './rootReducer';
const store = createStore(rootReducers,composeWithDevTools(applyMiddleware(logger)))
```

Step3: Install Redux devtools chrome extension

Benefits of devtools: you can see the state of app at any given time, the actions performed, you can even dispatch an action, without ui support.

## Action PayLoad

Use this approach when you have to do variable action.

Step1: introduce a state variable

```js
  const [number,setNumber] = useState(1);
```

Step2: modify onClick event

```js
 <button onClick={() => dispatch(buyCake(number))}>Buy {number} Cakes </button>   
```

Step 3: modify action creater to accept number

```js
					// set number = 1 if undefined or null
export const buyCake = (number = 1) => 
{
    return {
        type:  BUY_CAKE,
        //payload is the convention
        payload: number
    }
}
```

step 4: update the reducer fn

```js

const cakeReducer = ( state = initialState, action) =>{
    switch(action.type)
    {
        case BUY_CAKE:
            return {
                ...state,
                                         				//modified
                numberOfCakes : state.numberOfCakes - action.payload
            }

         default:
             return state   
    }

}

```

## Conditional state & dispatch fn

- Based on the props passed you return different state variable or dispatch fn using mapStateToProps & mapDispatchToProps.

- If the component only has dispatch functionality,
	you can pass ``null`` as first parameter in connect.
   ```js
    connect(null,mapDispatchToProps) (Component)
	```       
 ## API Call in react
 
```js
import React,{useEffect} from 'react'
import { fetchUsers } from '../redux/users/UserActions';
import { connect } from 'react-redux';

function UserContainers({user,fetchUserList}) {
      useEffect(()=> fetchUserList()
      , [])

  
    return user.loading?<h2>User is loading</h2>: (
        <div>
            Users
         {user && user.users && user.users.map(
             user =>{
                 return(
                 <div key={user.id}>{user.name}</div>
                 )
             }
         )}   
        </div>
    )
}

const mapStateToProps = state =>{
    return {user: state.user}
}

const mapDispatchToProps = dispatch =>{
    return {fetchUserList : ()=>dispatch(fetchUsers()) }
}


export default connect(mapStateToProps,mapDispatchToProps) (UserContainers)

```