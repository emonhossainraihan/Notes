## Different ways to Exit loops 

### Break 

```js
let numbers = [1,2,3,4,5,6]
for(const number of numbers){
 console.log(number)
 if(number===3) break
}
//output => 1 2 3
```

As you can see that when the number is equals to `3` the condition satisfies in if statement, the break statement is next and then 
`for...of` exit the loop

### some method 

```js
let numbers = [1,2,3,4,5,6]
numbers.some(number=>{
 console.log(number)
 return number === 3
})
//output => 1 2 3
```

`Array.some()` method lets you check if at least one of the elements of a given array satidfies the given condition and return 
truthy value then it exit the loop 


### every method 

```js
let numbers = [1,2,3,4,5,6]
numbers.every(number=>{
 console.log(number)
 return number < 3
})
//output => 1 2 3
```

`Array.every()` method lets you check if every elements of a given array satisfies the given condition but if condition fails 
then it exit the loop
