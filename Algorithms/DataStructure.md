DataStructure

## Objects

### Object Operation

Insertion -   O(1)

Removal -   O(1)

Searching -   O(N)

Access -   O(1)

When you don't need any ordering, objects are an excellent choice!


### Object Methods

*Below methods are doing linear search*

Object.keys -   O(N)

Object.values -   O(N)

Object.entries -   O(N)

hasOwnProperty -   O(1) // since accessing a single value is constant


## Arrays

### Array Operations

* Insertion -   It depends.... ** as re-indexing is required

* Removal -   It depends.... ** as re-indexing is required

* Searching -   O(N)

* Access -   O(1)

### Array Methods
* push *(insert element at end)* -   O(1)

* pop *(delete element at end)* -   O(1)

* shift *(insert element at start)* -   O(N)

* unshift *(delete element at start)* -   O(N)

* concat *(merge two arrays)*-   O(N)

* slice *(make copy of array)*-   O(N)

* splice *(delete element from anywhere)* -   O(N)

* sort *-   O(N * log N)

* forEach/map/filter/reduce/etc. -   O(N)