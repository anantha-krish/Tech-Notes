Simple Algorithms


## 1. Count 'n' natural numbers

Use mathematical formula instead of adding one by one in loop
```js
//Complexity: O(1)
function sumOfNum(num)
{
    return ((num*(num+1))/2)
}
```

## 2. Recursion

Recursive solution should have 2 requirements
- Should have base condition (a condition where it stops)
- Should process different Input set on each call


### Factorial
 #### Iterative Way
```js
function factorialIterative(num){
    var fact=1;
    for(var i = 1; i<= num; i++)
    {
        fact*=i;
    }

    return fact;
}
factorialIterative(4)
```
 #### Recursive Way
```js
function factorialRecursive(num)
{
    if(num===1) return 1;
    return num * factorialRecursive(num-1)
}

factorialRecursive(4)
```

### Collect Odd items in Array
Using Helper function

```js
function collectOddValues(arr){

    let result =[];
       // helper function
    function helperCollectOdd(subarr)
    {
        if(subarr.length ===0 )
        {
            return;
        }

        else if( subarr[0] % 2 === 1)
         {
            result.push(subarr[0]);  
        }
        
        helperCollectOdd(subarr.slice(1)) //process remaining element except first element
    }

    helperCollectOdd(arr)

    return result;
}

collectOddValues([1,2,3,4,5,6,7])
```

### fibonacci series

```js
function fib(n){
    if (n <= 2) return 1;
    return fib(n-1) + fib(n-2);
}
```

### product of Array

```js

function productofArray(arr)
{
    if(arr.length === 0) return 1;
    
    return arr[0] * productofArray(arr.slice(1))
}

productofArray([1,2,3,10])
```


### Flatten Array

```js

function flatten(oldArr)
{
  let newArr= []
  
   for(var i =0; i < oldArr.length ; i++)
   {
     if(Array.isArray(oldArr[i]))
     {
       newArr = newArr.concat(flatten(oldArr[i]))
     }

     else
     {
       newArr.push(oldArr[i])
     }
   }

  return newArr
}

// Example
flatten([1, [2, [3, 4], [[5,6],7]]])
// O/p -> [1, 2, 3, 4, 5, 6, 7]
```