Async Call


## Simple Async Example
```js
		const second = () => {
            setTimeout(() => {
                console.log('Async Hey there');
            }, 2000);
        }

        const first = () => {
            console.log('Hey there');
            second();
            console.log('The end');
        }

        first();

Output
Hey there
The end
Async Hey there
```


## Promise Hell (Triangluar Wait)

as there will be more promises ,
code maintainence will be difficult
```js
        function getRecipe() {
            setTimeout(() => {
                const recipeID = [523, 883, 432, 974];
                console.log(recipeID);

                setTimeout(id => {
                    const recipe = {title: 'Fresh tomato pasta', publisher: 'Jonas'};
                    console.log(`${id}: ${recipe.title}`);

                    setTimeout(publisher => {
                        const recipe2 = {title: 'Italian Pizza', publisher: 'Jonas'};
                        console.log(recipe);
                    }, 1500, recipe.publisher);

                }, 1500, recipeID[2]);

            }, 1500);
        }
        getRecipe();


```

## Promise Approach (Solution to Promise Hell)

```js

//Define seperate function that returns  promises
const getIDs = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve([523, 883, 432, 974]);
            }, 1500);
        });


const getRecipe = recID => {
    return new Promise((resolve, reject) => {
        setTimeout(ID => {
            const recipe = {title: 'Fresh tomato pasta', publisher: 'Jonas'};
            resolve(`${ID}: ${recipe.title}`);
        }, 1500, recID);
    });
};

const getRelated = publisher => {
    return new Promise((resolve, reject) => {
        setTimeout(pub => {
            const recipe = {title: 'Italian Pizza', publisher: 'Jonas'};
            resolve(`${pub}: ${recipe.title}`);
        }, 1500, publisher);
    });
};

// sequentially apply then instead of nesting

getIDs
.then(IDs => {
    //path to resolved promises
    console.log(IDs);
    return getRecipe(IDs[2]);
})
.then(recipe => {
    console.log(recipe);
    return getRelated('Jonas Schmedtmann');
})
.then(recipe => {
    console.log(recipe);
})
.catch(error => {
    //path to rejected promises
    console.log('Error!!');
});
```


## Soln in async await approach (best Approach)

No need of sequential .then statements...
```js
async function getRecipesAW() {
    const IDs = await getIDs;
    console.log(IDs);
    const recipe = await getRecipe(IDs[2]);
    console.log(recipe);
    const related = await getRelated('Jonas Schmedtmann');
    console.log(related);

    return recipe;
}

getRecipesAW().then(result => console.log(`${result} is the best ever!`));
```



## Example weather API

```js
async function getWeatherAW(woeid) {
try {
    const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}/`);
    const data = await result.json();
    const tomorrow = data.consolidated_weather[1];
    console.log(`Temperatures tomorrow in ${data.title} stay between ${tomorrow.min_temp} and ${tomorrow.max_temp}.`);
    return data;
} catch(error) {
    alert(error);
}
}

getWeatherAW(2487956);

let dataLondon;
getWeatherAW(44418).then(data => {
dataLondon = data
console.log(dataLondon);
});
```