```js
//! =============== Promise ==================

/* 
!How do you create a Promise?
!How do you change the status of a Promise?
!How do you listen for when the status of a Promise changes?
*/

function onSuccess() {
  console.log('Success!');
}

function onError() {
  console.log('💩');
}

var promise = new Promise((resolve, reject) => {
  //resolve() status become fulfilled
  //reject() status become rejected
  setTimeout(() => {
    resolve();
  }, 1000);
});

console.log('before: ', promise);
promise.then(onSuccess);
setTimeout(() => console.log('after: ', promise), 1010);

//! Chainning

function getPromise() {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, 2000);
  });
}

function logA() {
  console.log('A');
}

function logB() {
  console.log('B');
}

function logCAndThrow() {
  console.log('C');
  throw new Error();
}

function catchError() {
  console.log('Error!');
}

getPromise().then(logA).then(logB).then(logCAndThrow).catch(catchError);

//! =============== Async/await ==================

async function add(x, y) {
  return x + y;
}

add(2, 3).then((result) => {
  console.log(result);
});

//! =============== Compare ==================

//*Ex-1
console.log('Synchronous 💩');

setTimeout((_) => console.log('Time out 😏'), 0);

Promise.resolve().then((_) => console.log('Promise 😭'));

console.log('Synchronous 💩');

//*Ex-2

var promise = fetch('https://jsonplaceholder.typicode.com/users');

promise
  .then((res) => res.json())
  .then((users) => users.map((user) => console.log(user.name)))
  .catch((err) => console.log('sad', err));

console.log('Synchronous 🍔');

//*Ex-3
//!Invoking console.time('label') will record the current time in milliseconds, 
//then later calling console.timeEnd('label') will display
//!the duration from that point.
//!The time in milliseconds will be automatically printed alongside the label, 
//so you don't have to make a separate call to console.log
//!to print a label:

var getFruit = async (name) => {
  const fruits = {
    pineapple: '🍍',
    banana: '🍌',
    strawberry: '🍓',
  };
  return fruits[name];
};

const makeSmoothie = async () => {
  const a = await getFruit('pineapple');
  const b = await getFruit('banana');
  return [a, b];
};
const makeSmoothie2 = async () => {
  const a = await getFruit('pineapple');
  const b = await getFruit('banana');
  const c = Promise.all([a, b]);
  return c;
};

console.time('test1');
makeSmoothie().then((data) => console.log(data));
console.timeEnd('test1');
console.time('test2');
makeSmoothie2().then((data) => console.log(data));
console.timeEnd('test2');

const makeSmoothie3 = async () => {
  try {
    const a = await getFruit('pineapple');
    const b = await getFruit('banana');
    const c = Promise.all([a, b]);
    throw 'Broken 💩';
    return c;
  } catch (err) {
    console.log(err);
  }
};
makeSmoothie3()
  .then((data) => console.log(data))
  .catch((err) => console.log(err));

//*Ex-4
const fruits = ['peach', 'pineapple', 'strawberry'];

const fruitLoop = async () => {
  for (const f of fruits) {
    const emoji = await getFruit(f);
    log(emoji);
  }
};

const fruitInspection = async () => {
  if ((await getFruit('peach')) === '🍑') {
    console.log('looks peachy!');
  }
};

const getTodo = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/todos/1');

  const { title, userId, body } = await res.json();

  console.log(title, userId, body);
};

//*Ex-5

var p1 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('one'), 1000);
});
var p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('two'), 2000);
});
var p3 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('three'), 3000);
});
var p4 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('four'), 4000);
});
var p5 = new Promise((resolve, reject) => {
  reject(new Error('reject'));
});

var results = await Promise.all(
  [p1, p2, p3, p4, p5].map((p) => p.catch((e) => e))
);
var validResults = results.filter((result) => !(result instanceof Error));
var errors = results.filter((result) => result instanceof Error);
console.log(validResults);
console.log(errors);

```
