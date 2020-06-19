```js
//this

const a=function(){
    console.log(this)
    const b=function(){
        console.log(this)
    const c={
        hi:function(){
            console.log(this)
        }
    }
        c.hi()
    }
    b()
}
a()

//window.a(b())

const Obj={
    name:'Emon',
    sing(){
        console.log('sing',this);
        var anotherSing=function(){
            console.log('anotherSing',this)
        }
        anotherSing()
    }
}
Obj.sing()

//Obj.sing(anotherSing())

//Solution 1

const Obj1={
    name:'Emon',
    sing(){
        console.log('sing',this);
        var anotherSing=function(){
            console.log('anotherSing',this)
        }
        return anotherSing.bind(this)
    }
}
Obj1.sing()

//Solution 2

const Obj2={
    name:'Emon',
    sing(){
        console.log('sing',this);
        var anotherSing=()=>{
            console.log('anotherSing',this)
        }
        anotherSing()
    }
}
Obj2.sing()



//Solution 3

const Obj3={
    name:'Emon',
    sing(){
        console.log('sing',this);
        var self=this
        var anotherSing=function(){
            console.log('anotherSing',self)
        }
        anotherSing()
    }
}
Obj3.sing()


function multiply(a,b){
    return a*b
}
multiply(2,3)

var multiplyByTen=multiply.bind(this,10)

//HigherOder function
const multiplyBy=(num1)=> (num2)=> num1*num2
multiplyBy(2)(10)

// const multiplyBy=function (num1){
//     return function(num2){
//         return num1*num2
//     }
// }

//Closure

function aa(){
    let grandpa='grandpa'
    return function b(){
        let father='father'
        return function c(){
            let son='son'
            return `${grandpa} > ${father} > ${son}`
        }
    }
}

//Inheritance 
const dragon={
    name:'Charmander',
    fight(){
        return 5
    },
    sing(){
        return console.log(`I am ${this.name}, power of fire`)
    }
}
const lizard={
    name:'kiki',
    fight(){
        return 1
    }
}
const lizardSing=dragon.sing.bind(lizard)
lizardSing() 
lizard.__proto__=dragon    //Object.Create(dragon)

dragon.isPrototypeOf(lizard)
for(let prop in lizard){
    if(lizard.hasOwnProperty(prop)){
       console.log(prop) 
    } 
}

//typeof Object.prototype vs typeof Object

//OOP
function CreatePokemon(name,weapon){
    return{
    name:name,
    weapon:weapon,
    attack(){
        return `${name}, attack with ${weapon}`
    }
    }
}

const attackfunction={
    attack(){
        return `${this.name}, attack with ${this.weapon}`
    }
}

function CreatePokemon(name,weapon){                 
    let newPokemon=Object.create(attackfunction); 
    // It's borrow attackfunction via creating Object and add extra feathers 
    newPokemon.name=name
    newPokemon.weapon=weapon
    return newPokemon
}

const charmander=CreatePokemon('charmander','fire')
charmander.attack()

//Constructor Functions  (new)

function Pokemon(name,weapon){
    this.name=name;  //this-> didn't reffer to windows 
    this.weapon=weapon; //The only way to add properties to a constructor is this.something
}

Pokemon.prototype.attack=function(){    //here I can't use arrow function as it is lexically blocked 
    return 'attack with '+this.weapon
}
const charmanderX=new Pokemon('charmander','fire') //this new reffer to charmander => new binding this 

//build-in constructor 

// const Pokemon=new Function('name','weapon',    
// `    this.name=name;
//     this.weapon=weapon;
//     `)
// const charmander=new Pokemon('charmander','fire')



//ES6 class constructor (syntactic sugar) 
class Pokemonclass{
    constructor(name,weapon){
        this.name=name;
        this.weapon=weapon;   
    }
    attack(){
    // Why not we put attack method inside the constructor? 
    // Because every time when we create instance we run constructor hence we shared attack 
    // with it's every instance without running every time
        return 'attack with '+this.weapon
    }
}
const charmanderY=new Pokemonclass('charmander','fire') //charmanderY instanceof Pokemonclass => true
```

```js
const attackfunction={
    attack(){
        return `${this.name}, attack with ${this.weapon}`
    }
}

function CreatePokemon(name,weapon){                 
    let newPokemon=Object.create(attackfunction); 
    // It's borrow attackfunction via creating Object and add extra feathers 
    newPokemon.name=name
    newPokemon.weapon=weapon
    return newPokemon
}

const charmander=CreatePokemon('charmander','fire')
charmander.attack()

// Constructor Functions  (new)

function Pokemon(name,weapon){
    this.name=name;  // this-> didn't reffer to windows 
    this.weapon=weapon; // The only way to add properties to a constructor is this.something
}

Pokemon.prototype.attack=function(){  // here I can't use arrow function as it is lexically blocked 
    return 'attack with '+this.weapon
}
const charmanderX=new Pokemon('charmander','fire') 
// this new reffer to charmander => new binding this 

//build-in constructor 

// const Pokemon=new Function('name','weapon',    
// `    this.name=name;
//     this.weapon=weapon;
//     `)
// const charmander=new Pokemon('charmander','fire')



//ES6 class constructor (syntactic sugar) 
class Pokemonclass{
    constructor(name,weapon){
        this.name=name;
        this.weapon=weapon;   
    }
    attack(){
        // Why not we put attack method inside the constructor? 
        // Because every time when we create instance we run constructor hence we shared 
        // attack with it's every instance without running every time
        return 'attack with '+this.weapon
    }
}
const charmanderY=new Pokemonclass('charmander','fire') //charmanderY instanceof Pokemonclass => true

```

```js
// OOP full 

function CreateHuman(name,YOB,nature){
    return{
        name:name,
        YOB:YOB,
        character(){
            console.log('Character of '+name,' is ',nature)
        }
    }
}
const Emon=CreateHuman('Emon hossain',1999,'cheap')


function CreateHuman2(name,YOB,nature){
    return{
        name,
        YOB,
        nature
    }
}
const Humancharacter={
    character(){
            console.log('Character of '+this.name,' is ',this.nature)
            }
}
var Jakia=CreateHuman2('Jakia',2000,'Cool')
Jakia.character=Humancharacter.character //share functionality manually


function CreateHuman3(name,YOB,nature){
    let newHuman=Object.create(Humancharacter)
    console.log(newHuman.__proto__)
    newHuman.name=name;
    newHuman.YOB=YOB;
    newHuman.nature=nature;
    return newHuman;
}
var Maisha=CreateHuman3('maisha',1990,'Suck')


var rayhan=Object.create(Humancharacter,{
    name:{value:'rayhan'},
    YOB:{value:1998},
    nature:{value:'friendly'}
});

//Before Object.create
function CreateHumanConstructor(name,YOB,nature){
    this.name= name; 
    this.YOB= YOB;
    this.nature= nature;
}
CreateHumanConstructor.prototype.character=function(){
    console.log('Character of '+this.name,' is ',this.nature)
}
var nahid=new CreateHumanConstructor('nahid',1999,'fool')
//without new keyword this.name is pointed to windows not nahid
nahid.character()

//ES6 with class instance (syntactic sugar) 
//prototypal interitance(class is still object) != actual class 
class CreateHumanClass{
    constructor(name,YOB,nature){
    this.name= name; 
    this.YOB= YOB;
    this.nature= nature;
    }
    character(){ //We write here because we don't need it to run everytime
    console.log('Character of '+this.name,' is ',this.nature)
    }
}
const sakib=new CreateHumanClass('sakib',1999,'fool')
const rakib={...sakib}//lost prototypal connection with CreateHumanClass

//interitance 
class friend extends CreateHumanClass{
    constructor(name,YOB,nature,friendtype){
        super(name,YOB,nature);
        this.friendtype=friendtype        
    }
    ftype(){console.log('Friend type is:'+this.friendtype)}
}
const unknown=new friend('unknown',1900,NaN,'typical')
//friend.prototype.isPrototypeOf(unknown)=>true
//CreateHumanClass.prototype.isPrototypeOf(friend.prototype)=>true
```

```js
// FP
//side effect 
const arr=[1,2,3]
// function poparr(arr){
//     arr.pop()
//     return arr
// }
function popwithoutsideeffect(arr){
    let newarr=[].concat(arr)
    newarr.pop()
    return newarr
}
console.log(popwithoutsideeffect(arr))

//closure
const closure=function(){
    let count=5; //outer scope
    return function(){
        count++; //because of closure inner function remember the 
                 //outer scope 
        return count
    }
}
const getintement=closure() //outer function finished running 
getintement() //although inner function remember count 
getintement() 
//currying 
const multiply=(a,b)=>a*b;
const curriedmultiply=(a)=>(b)=>a*b;
const curriedmultiplyby5=curriedmultiply(5);
curriedmultiplyby5(5) //only run (b)=>a*b part 
//partial application
const partialmultiplyby5=multiply.bind(null,5)
partialmultiplyby5(5)

//Memoization(caching) vs normal
function addto80(n){
    console.log('long calculation')
    return n+80
}

let cache={}
function memoizedaddto80(n){
    if(n in cache){
        return cache[n];
    }else{
        console.log('long calculation')
        cache[n]=n+80
        return cache[n]
    }
}
memoizedaddto80(5) //cache={5:85}
//Improvement Memoization using closure
function memoizedaddto80(){
    let cache={};
    return function(n){
            if(n in cache){
        return cache[n];
    }else{
        console.log('long calculation')
        cache[n]=n+80
        return cache[n]
    }
   }
}
const memized=memoizedaddto80();
memized(5)

//Compose
const compose=(f,g)=>(data)=>f(g(data))
const mulby3=(num)=>num*3;
const abs=(num)=>Math.abs(num);
const mulby3andAbs=compose(mulby3,abs);
mulby3andAbs(-3)

//Amazon shopping

const user={
    name:'emon',
    active:true,
    cart:[],
    purchases:[]
}

// function purchaseItem(user,item){
//     return Object.assign({},user,{purchases:item})
// }
// purchaseItem(user,{name:'Laptop',price:344})
const compose1=(f,g)=>(...args)=>{
    f(g(...args));
    }
purchaseItem(
    emptyCart,
    buyItem,
    applytax,
    AddTocart
)(user,{name:'Laptop',price:344})

function purchaseItem(...fns){
    return fns.reduce(compose1) //[f,f,f,f].reduce
}

function AddTocart(user,item){
    const updateCart=user.cart.concat(item)
    return Object.assign({},user,{cart:updateCart})
}

function applytax(user){
    return user
}

function buyItem(user){
    return user
}

function emptyCart(user){
    return user
}


// const compose112=(f,g)=>(...args)=>f(g(...args))
// function add10(num1,num2){return num1+num2+10}
// function add5(num){return num+5}
// debugger
// compose112(add5,add10)(1,2,3)
```

```js
// const compose112=(f,g)=>(...args)=>f(g(...args))
// function add10(num1,num2){return num1+num2+10}
// function add5(num){return num+5}
// debugger
// compose112(add5,add10)(1,2,3)

const promise=new Promise((resolve,reject)=>{
    if(true){
        resolve("It's work man!!")
    }else{
        reject("Oo boy, It's fail")
    }
});
debugger;
promise
.then(result=>{
    throw Error
    console.log(result)
})
.catch(()=>{
    console.log('Error Occur')
})

//Promise
const urls=[
'https://jsonplaceholder.typicode.com/posts',
'https://jsonplaceholder.typicode.com/users',
'https://jsonplaceholder.typicode.com/albums']
debugger;
Promise.all(urls.map(url=>{
    return fetch(url).then(data=>data.json())
}))
.then(data=>{
    console.log('1',data[0])
    console.log('2',data[1])
    console.log('3',data[2])
})
.catch(err=>console.log('print error',err))
.finally(()=>console.log('Extra data'))

//Async before ES2018
const getData=async function(){
    try{
        const[posts,users,albums]=await Promise.all(urls.map(async function(url){
            const response=await fetch(url)
            return response.json()
        }));
        console.log('posts',posts)
        console.log('users',users)
        console.log('albums',albums)
    }catch(err){
        console.log('Ow boy shit',err)
    }
}

//After ES2018
//await before a promise
const getData2=async function(){
    const arrayOfPromises=urls.map(url=>fetch(url))
    for await(let req of arrayOfPromises){
        const data=await req.json()
        console.log(data)
    }
}
```


