Basic Redux Concept

# Basic Redux Notes 
#### Author: Anantha Krishnan M 
Following are the notes for redux using Nodejs
Start npm project
-
```
npm init
```

Install redux
---
```
npm add redux
```
Three concepts of redux
-
1. Store : holds the state of ur action
2. Action : describe the change in state of app
3. Reducers: carries out state transition depending on action

Redux pattern
-
The application can't update state directly.
1. You have to dispatch an action (action contains the type)
2. The action will be processed by reducer to update store.
3. Update in the state is reflected to app as its subscribed. 
![](image-keee20c5.png)

Actions
--
- json must contain a type key (field), and any no of fields u can add

```js
const BUY_CAKE='BUY_CAKE'
//function is called action creator
function buyCake()
{
  return {
      type:BUY_CAKE,
      info:'First action for redux'
  }
}
```
Reducers
--
Reducer accepts the prevState & action -> return state variable based on action 
```js
const initialState =
{
    numberOfCakes:10
}

const reducers = ( state = initialState, action)=>{
  switch(action.type){
 //returning a new state object not mutating existing one
  case BUY_CAKE:return{
     //convention to make the copy of state & then modify the key 
      ...state,
      numberOfCakes: state.numberOfCakes - 1
  }
  default: return state  
}
}
```
Store
--
 + step 1:
 Import redux library. Below is example for nodejs app
```js
const redux = require('redux')
const createStore = redux.createStore
``` 
+ step 2: 
  create a store
```js
const store = createStore(reducers)
```
+ step 3: access the state using get state
```js
console.log('Initial State',store.getState())
```
+ step 4: subscribe the app to the store 
```js
const unsubscribe = 
    store.subscribe( ()=> console.log('update state', store.getState() ))
```
+ step 5: dispatch actions to the store
```js
store.dispatch(buyCake())
store.dispatch(buyCake())
store.dispatch(buyCake())
```
+ step 6: unsubscribe the app
```js
//unsubscribe is variable declared in step 4
unsubscribe()
```

### OUTPUT 
```
PS D:\ReactProjects\redux-demo> node index.js
Initial State { numberOfCakes: 10 }
update state { numberOfCakes: 9 }
update state { numberOfCakes: 8 }
update state { numberOfCakes: 7 }
```
Combine Reducers
--
Step1: Create multiple initial states

```js
const initialCakeState =
{
    numberOfCakes:10
}

const initialIceCreamState =
{
    numberOfIcecreams:20
}
```

Step2: Create different reducers accepting different initial state

Step3: use combineReducer
```js
//import stmnt
const combineReducers = redux.combineReducers

//implementation
const rootReducer = combineReducers({
  //define key pair
  cake: cakeReducers,
  iceCream: iceCreamReducers 
})
const store = createStore(rootReducer)
```

//sample code

```js
console.log('Initial State',store.getState())
const unsubscribe = store.subscribe(
    ()=>console.log('update state',store.getState()))
store.dispatch(buyCake())
store.dispatch(buyCake())
store.dispatch(buyCake())
//calling second action
store.dispatch(buyIceCream())
store.dispatch(buyIceCream())
unsubscribe()
```
//output

```js
PS D:\ReactProjects\redux-demo> node index.js
Initial State { cake: { numberOfCakes: 10 }, iceCream: { numberOfIcecreams: 20 } }
update state { cake: { numberOfCakes: 9 }, iceCream: { numberOfIcecreams: 20 } }  
update state { cake: { numberOfCakes: 8 }, iceCream: { numberOfIcecreams: 20 } }  
update state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 20 } }
update state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 19 } }
update state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 18 } }
```
Using Middlewares in Redux
--
Example: redux-logger

1. Install Redux-logger

```js
npm add redux-logger
```

2. Import Statements
```js
const reduxLogger = require('redux-logger')
const logger = reduxLogger.createLogger()
const applyMiddleWare = redux.applyMiddleware
```
3. modify the code 
```js
//use applyMiddleware in store
const store = createStore(rootReducer,applyMiddleWare(logger))
console.log('Initial State',store.getState())
const unsubscribe = store.subscribe(()=>{})
store.dispatch(buyCake())
store.dispatch(buyCake())
store.dispatch(buyCake())
store.dispatch(buyIceCream())
store.dispatch(buyIceCream())
unsubscribe()
```
\\output

```
PS D:\ReactProjects\redux-demo> node index.js
Initial State { cake: { numberOfCakes: 10 }, iceCream: { numberOfIcecreams: 20 } }
 action BUY_CAKE @ 16:12:09.589
   prev state { cake: { numberOfCakes: 10 }, iceCream: { numberOfIcecreams: 20 } }
   action     { type: 'BUY_CAKE', info: 'First action for redux' }
   next state { cake: { numberOfCakes: 9 }, iceCream: { numberOfIcecreams: 20 } }
 action BUY_CAKE @ 16:12:09.614
   prev state { cake: { numberOfCakes: 9 }, iceCream: { numberOfIcecreams: 20 } }
   action     { type: 'BUY_CAKE', info: 'First action for redux' }
   next state { cake: { numberOfCakes: 8 }, iceCream: { numberOfIcecreams: 20 } }
 action BUY_CAKE @ 16:12:09.619
   prev state { cake: { numberOfCakes: 8 }, iceCream: { numberOfIcecreams: 20 } }
   action     { type: 'BUY_CAKE', info: 'First action for redux' }
   next state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 20 } }
 action BUY_ICECREAM @ 16:12:09.624
   prev state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 20 } }
   action     { type: 'BUY_ICECREAM', info: 'Second action for redux' }
   next state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 19 } }
 action BUY_ICECREAM @ 16:12:09.655
   prev state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 19 } }
   action     { type: 'BUY_ICECREAM', info: 'Second action for redux' }
   next state { cake: { numberOfCakes: 7 }, iceCream: { numberOfIcecreams: 18 } }
```
Setting up for async calls
--

```js
const redux = require('redux')
const createStore = redux.createStore
const applyMiddleWare = redux.applyMiddleware

const initialState ={
    loading:true,
    users:[],
    errors:''
}

const FETCH_USERS_LIST = 'FETCH_USERS_LIST'
const FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS'
const FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE'

const fetchUserList = () =>{
    return {
       type: FETCH_USERS_LIST
    }
}

const fetchUserSuccess = (users) =>{
    return {
      type: FETCH_USERS_SUCCESS,
      payload: users
    }
}

const fetchUserFailure = (error) =>{
    return {
      type: FETCH_USERS_FAILURE,
      payload: error
    }
}


const reducers = (state = initialState,action) =>{
    switch (action.type)
    {
    case FETCH_USERS_LIST:
    return{
        ...state,
        loading:true,
    } 
    case FETCH_USERS_SUCCESS:
        return{
            ...state,
            loading:false,
            users: action.payload
        } 

        case FETCH_USERS_FAILURE:
            return{
                ...state,
                loading:false,
                error: action.payload
            }    

        default:
        return state
   }

}

const store = createStore(reducers);
```

API call & Redux Thunk
--

Step1 : add axios & thunk
```js
npm add axios redux-thunk
```


Step2 : Import the libraries
```js
const thunkMiddleWare = require('redux-thunk').default
const axios = require('axios')
```
Step3 : Add below code

```js
//action creater with thunk middleware support
const fetchUsers=() =>{
    // bcoz of thunk middleware it expects a function not a json
    return function(dispatch)  // argument is dispatch
    
    {    // set the initial Loading state 
         dispatch(fetchUserList())
          // fetch dummy data from json placeholder
         axios.get('https://jsonplaceholder.typicode.com/users')
         .then( (res) => 
         { let users = res.data.map(user => user.id)
            //if response successful proceed to success  
           dispatch(fetchUserSuccess(users))}
         )
         .catch((err) => 
            //if error proceed to failure    
        dispatch(fetchUserFailure(err.message)))
    }
}
//use thunk middleware
const store = createStore(reducers,applyMiddleWare(thunkMiddleWare));
const unsubscribe = store.subscribe(()=> { console.log(store.getState())})
//start the fetch user process
store.dispatch(fetchUsers())

```
### // Output
```
PS D:\ReactProjects\redux-demo> node async
{ loading: true, users: [], errors: '' }
{
  loading: false,
  users: [
    1, 2, 3, 4,  5,
    6, 7, 8, 9, 10
  ],
  errors: ''
}
PS D:\ReactProjects\redux-demo> node async
{ loading: true, users: [], errors: '' }
{
  loading: false,
  users: [],
  errors: 'getaddrinfo ENOTFOUND jsonplaceholder.typicode.cm'
}
```


