```js
const poll = async ({fn, validate, interval, maxAttempts}) => {
  let attempts = 0;
  const executePoll = async (resolve, reject) => {
    const result = await fn()
    attempts++
    
    if(validate(result)) resolve(result)
    else if(maxAttempts && maxAttempts === attempts){
      return reject(new Error("Exceed max attempts"))
    } else {
      setTimeout(executePoll, interval, resolve, reject)
    }
  }
  return new Promise(executePoll)
}

const validateUser = (users) => {
  if(users.length){
    return users
  } else {
    return false
  }
}
const fetchData= async () => {
let url = 'https://jsonplaceholder.typicode.com/usersaa';
let response = await fetch(url);

let data = await response.json();
  return data
}

const POLL_INTERVAL = 1000
const pollForNewUser = poll({
  fn: fetchData,
 	validate: validateUser,
  interval: POLL_INTERVAL
})
.then((user)=> console.log(user))
.catch((err)=> console.log(err))
```
