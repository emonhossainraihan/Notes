## Contents
- [withRouter](#with-router)

## withRouter

When you include a main page component in your app, it is often wrapped in a `<Route>` component like this:
```js
<Route path="/movies" component={MoviesIndex} />
```
By doing this, the `MoviesIndex` component has access to `this.props.history` so it can redirect the user with `this.props.history.push`.

Some components (commonly a header component) appear on every page, so are not wrapped in a `<Route>`:
```js
render() {
  return (<Header />);
}
```

This means the header cannot redirect the user.

To get around this problem, the header component can be wrapped in a [`withRouter`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/withRouter.md) function, either when it is exported:
```js
export default withRouter(Header)
```
This gives the `Header` component access to `this.props.history`, which means the header can now redirect the user.
