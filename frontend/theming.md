```js
//! theming
import { ThemeProvider, createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
body {
  background-color: ${(props) =>
    props.theme.mode === 'dark' ? '#111' : '#EEE'};
  color: ${(props) => (props.theme.mode === 'dark' ? '#EEE' : '#111')}
}
`;

function App() {
  const [theme, setTheme] = useState({ mode: 'dark' });
  return (
    <ThemeProvider theme={theme}>
      <>
        <GlobalStyle />
        <div className="App">
          <h1>Hello everyone</h1>
          <button
            onClick={(e) => {
              setTheme(
                theme.mode === 'dark' ? { mode: 'light' } : { mode: 'dark' }
              );
            }}
          >
            ToggleTheme
          </button>
        </div>
      </>
    </ThemeProvider>
  );
}
```

use a function to lazily initialize the variable (useful when the initial state is the result of an expensive computation). [Here](https://blog.logrocket.com/a-guide-to-usestate-in-react-ecb9952e406c/) you can find more details:
```js
const Message= () => {
   const messageState = useState( () => expensiveComputation() );
   /* ... */
}
```

The initial value will be assigned only on the initial render (if it’s a function, it will be executed only on the initial render).

We can create reuse block and keep persistance:
```js
const getBackground = style('mode', {
  light: '#EEE',
  dark: '#111',
});

const getForeground = style('mode', {
  light: '#111',
  dark: '#EEE',
});

const GlobalStyle = createGlobalStyle`
body {
background-color: ${getBackground};
  color: ${getForeground}
}
`;
function getInitialTheme() {
  const savedTheme = storage.getItem('theme');
  return savedTheme ? JSON.parse(savedTheme) : { mode: 'light' };
}
...
 const [theme, setTheme] = useState(getInitialTheme);
```



