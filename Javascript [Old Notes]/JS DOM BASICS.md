JS DOM BASICS

## Blog refernce

https://blog.garstasio.com/you-dont-need-jquery/dom-manipulation/


### Math.random()

It generates a value in between 0 & 1 , but not 0 or 1.

so if you want to generate a number between 1-6

```js
dice = Math.floor(Math.random() * 6) + 1
console.log(dice)
```

## Query Selector (DOM Manipulation)

textContent : is used to modify the plain text

innerHTML: is used to modify the HTML
 
### using as Setter
 ```js
// for ids we use # prefix
// for class we use . prefix
document.querySelector('#current-0').textContent = dice


document.querySelector('#current-0').innerHTML = '<em>'+dice+'<em>'

// change style..
document.querySelector('.dice').style.display = 'none';
```

### using as getter

```js
var x = document.querySelector('#score-' + activePlayer).textContent
console.log(x)
```

## Events & Event Listeners

Events -> click,dblclick, mouseup ...  check MDN for more

Event Listeners -> are stored in message queue, and initally the function are cleared from execution stack, and only loaded when event is executed.

callback fns -> fns that are called by otherfn like here event listener is calling the function.

Toggle is used when condition is like add if does not exist otherwise remove.    similary you can use ``.add('active')  or .remove('active)``

``          document.querySelector('.player-0-panel').classList.toggle('active')``


Sample Code
```js

// here the function is also called anonymous function
document.querySelector('.btn-roll').addEventListener('click',function()
{
    dice = Math.floor(Math.random() * 6) + 1
    
    var diceDOM = document.querySelector('.dice')
    //modify the style
    diceDOM.style.display = 'block'
    
    // modfiy HTML attributes
    diceDOM.src = 'dice-'+ dice + '.png'
          
          document.querySelector('.player-0-panel').classList.toggle('active')
          document.querySelector('.player-1-panel').classList.toggle('active')
          
          document.querySelector('.dice').style.display = 'none'
    }
})
```