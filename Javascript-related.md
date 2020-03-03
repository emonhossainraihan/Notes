
# difference between call, apply and bind
## Object: 

    var obj = {num:2};

## function or method:

    var addToThis = function(a) {
    	return this.num + a;
    };

suppose we want to add this functionality to our object(obj) then we can use `call`,`apply` or `bind`:

    console.log(addToThis.call(obj, 3));

here we follow `functionName.call(obj,functionarguments)` which executed and give the result.

    var arr=[3,5];
    var addToThis2 = function(a,b) {
    	return this.num + a + b;
    };
    console.log(addToThis2.apply(obj, arr)); 

here we follow `functionName.call(obj,array)` which executed and give the result.

    var bound = addToThis.bind(obj);
    console.log(bound(3))

here we follow `functionName.bind(obj)` which return a function. **A[pply] for array and C[all] for comma**

# Memoization 
caching data for re-using which optimizated a lot. 

    const prevValues = []
    
    function square(n) {
    	if(prevValues[n] != null) {
      	return prevValues[n]
      }
      let result = 0
      for (let i=1; i<=n; i++) {
      	for(let j=1; j<=n; j++) {
        	result += 1
        }
      }
      prevValues[n] = result
      return result
    }
    console.log(square(30000))

In dynamic programming:

    function fib(n,prevValues = []) {
      if(prevValues[n] != null) {
      	return prevValues[n]
      }
      let result
      if(n <= 2) {
      	result = 1
      } else {
      	result = fib(n -1, prevValues) + fib(n -2, prevValues)
      }
      prevValues[n] = result
      return result
    }
    console.log(fib(40))