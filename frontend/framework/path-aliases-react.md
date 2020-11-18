1. Open the terminal in your project root directory and install the react-app-rewired and react-app-rewire-alias as a dev dependency.

```
npm install react-app-rewired react-app-rewire-alias --save-dev
```

2. Now inside the root directory create a file with the name config-overrides.js and paste the below code.

```js
const {alias} = require('react-app-rewire-alias')

module.exports = function override(config) {
  alias({
    '@components': 'src/components', // key-value pair
    '@assets' : 'src/assets'
  })(config)

  return config
}
```

3. Now open your package.json file and 'Flip' the existing calls from `react-scripts` to `react-app-rewired`
