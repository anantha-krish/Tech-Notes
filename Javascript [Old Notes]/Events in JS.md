Events in JS

## Modularisation

always use IIFE for modularistion

example
```js
var uiController = (function(){
    //code
})();

var dataController = (function(){
    //code
})();
```

## Event Listeners

```js
   var addItem = function()
   {
       console.log('It works')
   }
  // mouse clicks
  document.querySelector('.add__btn').addEventListener('click',addItem)
    
// keyboard events , event is passed by browser either use e or event
   document.addEventListener('keypress', function(event){
       if(event.key === "Enter"|| event.code ==="Enter")
       {
           addItem();
       }
   })
```
## Capture the value of text/dropdown

```js
 getInput: function () {
      return {
        type: document.querySelector(DOMStrings.inputType).value,
        description: document.querySelector(DOMStrings.inputDesc).value,
        value: document.querySelector(DOMStrings.inputValue).value,
      };
    }
```

## InsertAdjacentHtml

used when we need to put html before or after, or as a sibiling.
```js

 element = DOMStrings.expensesContainer;
            html=    '<div class="item clearfix" id="expense-%id%">'
                    + '<div class="item__description">%description%</div>'
                    + '<div class="right clearfix"><div class="item__value">- %value%</div>'
                    + '<div class="item__percentage">21%</div>'
                    + '<div class="item__delete">'
                    + '<button class="item__delete--btn"><i class="ion-ios-close-outline"></i></button>'
                    + '</div></div></div>'
        }  
        // placeholder variable are give %id% to quickly detect
        newHtml = html.replace('%id%',obj.id); 
        //update newHtml for other variables    
        newHtml = newHtml.replace('%description%',obj.description);   
        newHtml = newHtml.replace('%value%',obj.value);
        //insertAdjacentHTML(position,modified html)   
        document.querySelector(element).insertAdjacentHTML("beforeend",newHtml)
```
## QuerySelectorAll

Pass a list of class seperated by comma, returns a list

Use Array.prototype.slice to convert list to array

```js
 var fieldsList, fieldsArr;

        fieldsList = document.querySelectorAll(DOMStrings.inputDesc +', ' + DOMStrings.inputValue);
         
        //convert list to array using prototype of array
        fieldsArr = Array.prototype.slice.call(fieldsList);
        
        // below is the shortcut for clearing all the fields
        fieldsArr.forEach( function(field)
        {
            field.value = "";
        })

        fieldsArr[0].focus();

```

## Event Delegation

- used when we lots of child target elements
- used when there are items that will be in dom in future

```js
 var ctrlDelItem = function(event){

     var itemID,type,ID,splitResult;
     //event bubbling
     itemID = event.target.parentNode.parentNode.parentNode.parentNode.id;

    if(itemID)
    {
     splitResult = itemID.split('-');
     type = splitResult[0];
     ID = splitResult[1];
     console.log(type,ID)
    }
    
  }
```

## Removing an element
- we can only remove child, we cannot delete element by itself

```js
   removeListItem:function(id){

      var el;
      el = document.getElementById(id);
      el.parentNode.removeChild(el);

    }
```
