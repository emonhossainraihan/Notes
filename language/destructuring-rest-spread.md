## Object Destructuring

```js
const note = {
  id: 1,
  title: 'My first note',
  date: '01/01/1970',
}

// Create variables from the Object properties
const id = note.id
const title = note.title
const date = note.date

// Destructure properties into variables
const { id, title, date } = note

// Assign a custom name to a destructured value
const { id: noteId, title, date } = note

const note = {
  id: 1,
  title: 'My first note',
  date: '01/01/1970',
  author: {
    firstName: 'Sherlock',
    lastName: 'Holmes',
  },
}

// Destructure nested properties
const {
  id,
  title,
  date,
  author: { firstName, lastName },
} = note
```

## Array Destructuring

```js
const date = ['1970', '12', '01']

// Create variables from the Array items
const year = date[0]
const month = date[1]
const day = date[2]

// Destructure Array values into variables
const [year, month, day] = date

// Skip the second item in the array
const [year, , day] = date

console.log(year)
console.log(day)

// Create a nested array
const nestedArray = [1, 2, [3, 4], 5]

// Destructure nested items
const [one, two, [three, four], five] = nestedArray

console.log(one, two, three, four, five)

```

Destructuring syntax can be applied to destructure the parameters in a function. To test this out, you will destructure the keys and values out of Object.entries().

```js
const note = {
  id: 1,
  title: 'My first note',
  date: '01/01/1970',
}

// Using forEach
Object.entries(note).forEach(([key, value]) => {
  console.log(`${key}: ${value}`)
})

// Using a for loop
for (let [key, value] of Object.entries(note)) {
  console.log(`${key}: ${value}`)
}
```

## Spread

Spread syntax (...) is another helpful addition to JavaScript for working with arrays, objects, and function calls. Spread allows objects and iterables (such as arrays) to be unpacked, or expanded, which can be used to make shallow copies of data structures to increase the ease of data manipulation.

```js
// Create an Array
const tools = ['hammer', 'screwdriver']
const otherTools = ['wrench', 'saw']

// Concatenate tools and otherTools together
const allTools = tools.concat(otherTools)

// Unpack the tools Array into the allTools Array
const allTools = [...tools, ...otherTools]

console.log(allTools)
```

https://www.taniarascia.com/understanding-destructuring-rest-spread/?ck_subscriber_id=890669782



























