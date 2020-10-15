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
Why not split our code and seperate our business logic: 
```js
//App.js
import React from 'react';
import './App.css';

//! theming
import { ThemeProvider, createGlobalStyle } from 'styled-components';
import style from 'styled-theming';
import useTheme from './useTheme';
import ToggleMode from './ToggleMode';

const getBackground = style('mode', {
  light: '#EEE',
  dark: '#111',
});

const getForeground = style('mode', {
  light: '#111',
  dark: '#EEE',
});

const getFontSize = style('textZoom', {
  normal: '1em',
  magnify: '1.2em',
});

const GlobalStyle = createGlobalStyle`
body {
background-color: ${getBackground};
color: ${getForeground};
font-size:${getFontSize}
}
`;

function App() {
  const theme = useTheme();

  return (
    <ThemeProvider theme={theme}>
      <>
        <GlobalStyle />
        <div className="App">
          <Particles />
          <h1>Hello everyone</h1>
          <ToggleMode />

          <button
            onClick={(e) => {
              theme.setTheme(
                theme.textZoom === 'normal'
                  ? { ...theme, textZoom: 'magnify' }
                  : { ...theme, textZoom: 'normal' }
              );
            }}
          >
            ToggleZoom
          </button>
          <hr />
        </div>
      </>
    </ThemeProvider>
  );
}

export default App;
```

```js
//useTheme.js
import React, { useState, useEffect } from 'react';

import storage from 'local-storage-fallback';

export default function useTheme(
  defaultTheme = { mode: 'light', textZoom: 'normal' }
) {
  const [theme, _setTheme] = useState(getInitialTheme);
  function getInitialTheme() {
    const savedTheme = storage.getItem('theme');
    return savedTheme ? JSON.parse(savedTheme) : defaultTheme;
  }

  useEffect(() => {
    storage.setItem('theme', JSON.stringify(theme));
  }, [theme]);

  return {
    ...theme,
    setTheme: ({ setTheme, ...theme }) => _setTheme(theme),
  };
}
```

```js
//ToggleMode.js
import React from 'react';
import { ThemeConsumer } from 'styled-components';
import Button from './Button';

export default function ToggleMode() {
  return (
    <ThemeConsumer>
      {(theme) => (
        <Button
          variant="primary"
          onClick={(e) =>
            theme.setTheme(
              theme.mode === 'dark'
                ? { ...theme, mode: 'light' }
                : { ...theme, mode: 'dark' }
            )
          }
        >
          Toggle Mode
        </Button>
      )}
    </ThemeConsumer>
  );
}
```

```js
//Button.js
import styled from 'styled-components';
import style from 'styled-theming';

const getBackground = style.variants('mode', 'variant', {
  normal: {
    light: '#EEE',
    dark: '#EEE',
  },
  primary: {
    light: 'papayawhip',
    dark: 'pink',
  },
});

const Button = styled.button`
  background-color: ${getBackground};
`;

export default Button;
```
