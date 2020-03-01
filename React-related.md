# event.preventDefault()
React uses synthetic events to handle events from button, input and form elements. A synthetic event is a shell around the native DOM event with additional information for React. Sometimes you have to use `event.preventDefault();` in your application.
```
import React from  'react';
const initialList =  [
  'Learn React',
  'Learn Firebase',
  'Learn GraphQL',
];
const  ListWithAddItem  =  ()  =>  {
  const  [value, setValue]  = React.useState('');
  const  [list, setList]  = React.useState(initialList);
  const  handleChange  =  event  =>  {
  setValue(event.target.value);
  };
  const  handleSubmit  =  event  =>  {
  if  (value)  {
  setList(list.concat(value));
  }
  setValue('');
 event.preventDefault();
  };
  return  (
  <div>
  <ul>
  {list.map(item  =>  (
  <li  key={item}>{item}</li>
  ))}
  </ul>
  <form  onSubmit={handleSubmit}>
  <input  type="text"  value={value}  onChange={handleChange}  />
  <button  type="submit">Add Item</button>
  </form>
  </div>
  );
};
export  default ListWithAddItem;
```
In this case, a preventDefault is called on the event when submitting the form to **prevent a browser reload/refresh**. 
**Why is a form submit reloading the browser?** All native HTML elements come with their internal native behavior. For instance, input elements store their internal state. That's why often [React is used to take over for having controlled components by managing the state via React](https://www.robinwieruch.de/react-controlled-components/). The same applies for a form element which has a submit event that is invoked via a submit button element. In the past, it was desired to refresh the browser to flush all state and to submit the data to a backend. Nowadays, a library such as React, gives us more flexibility to deal with the submit event ourselves. In this case, we deal with it by updating the list in our component's state.