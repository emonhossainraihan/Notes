[Original link](https://medium.com/@cscalfani/goodbye-object-oriented-programming-a59cda4c0e53)
# Inheritance, the First Pillar to Fall

At first glance, Inheritance appears to be the biggest benefit of the Object Oriented Paradigm. 

## Banana Monkey Jungle Problem

In order to reuse any class our first thought will be simply grab that Class from the other project and use it. Well… actually… not just that Class. 
We’re gonna need the parent Class. But… But that’s it. Ugh… Wait… Looks like we gonna also need the parent’s parent too... And then… We’re going to need ALL of the parents. 
Okay… Okay… I handle this. No problem. And great. Now it won’t compile. Why?? Oh, I see… This object contains this other object. So I’m gonna need that too. No problem.
Wait… I don’t just need that object. I need the object’s parent and its parent’s parent and so on and so on with every contained object and ALL the parents of what those contain along with their parent’s, parent’s, parent’s…
There’s a great quote by Joe Armstrong, the creator of Erlang:
> The problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.

## Banana Monkey Jungle Solution
I can tame this problem by not creating hierarchies that are too deep. 

## The Diamond Problem

```js
class PoweredDevice  { 
 constructor(){ }
 start(){ }
}

class Scanner extends PoweredDevice { 
 constructor(){ }
 start(){ }
}

class Printer extends PoweredDevice { 
 constructor(){ }
 start(){ }
}

class CopiedOut extends Scanner {
 class Copied extends Printer {
  //copied can access everything from its parent
 }
}
```

Notice that both the Scanner class and the Printer class implement a function called start.
So which start function does the Copier class inherit? The Scanner one? The Printer one? It can’t be both.
## The Diamond Solution

```js
class PoweredDevice  { 
 constructor(){ }
 start(){ }
}

class Scanner extends PoweredDevice { 
 constructor(){ }
 start(){ }
}

class Printer extends PoweredDevice { 
 constructor(){ }
 start(){ }
}

class Copied {
 const scanner = new Scanner( )
 const printer = new Printer( )
 start() { }
}
```


