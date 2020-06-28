The right abstractions can make code more readable, adaptable, 
and maintainable by hiding details which are unimportant for the current context, and reducing the 
amount of code required to do the same work

> “Simplicity is about subtracting the obvious and adding the meaningful.” ~John Maeda: The Laws of Simplicity

Abstraction is not a 1-way street. It’s really formed by two complementary concepts:
- **Generalization** — Removing the repeated parts (the obvious) and hiding them behind an abstraction.
- **Specialization** — Applying the abstraction for a particular use-case, adding just what needs to be different (the meaningful).
Consider the following code:

```js
// double the value of a list/array
const doubleList = list => {
  const newList = [];
  for (var i = 0; i < list.length; i++) {
    newList[i] = list[i] * 2;
  }
  return newList;
};
```
There’s nothing inherently wrong with the code, but it contains a lot of details that may not be important for this particular application.
- It includes details of the container/transport data structure being used (the array), meaning that it will only work with arrays. It contains a **state shape dependency**.
- It includes the iteration logic, meaning that if you need other operations which also need to visit each element in the data structure, you’d need to repeat very similar iteration logic in that code, as well. It forces repetition which could violate **DRY (Don’t Repeat Yourself)**.
- It includes an explicit assignment, rather than declaratively describing the operation to be performed. It’s **verbose**.

None of that is necessary. All of it can be hidden behind an abstraction.

```js
const doubleList = list => list.map(x => x * 2);
```
Map abstracts away details such as the type of data you’re mapping over, the type of data structure containing the data, and the iteration logic required to enumerate each data node in the data structure.
