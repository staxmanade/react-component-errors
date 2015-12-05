
# What is it?

This is a little utility that wraps you're ES6 (or even ES5) React Component's lifecycle methods
with a try/catch and is useful in development because it exposes errors thrown that are swallowed by React by default.
Now you can more easily diagnose errors during development.

NOTE: It's not likely a good idea to run with this `enabled` in production as it could affect performance of your React components.

# Usage with an es7 @decorator
```
import wrapReactComponentMethodsWithTryCatch from `react-component-errors`

@wrapReactComponentMethodsWithTryCatch
class MyComponent {
  componentDidMount(){
    throw new Error("Test error");
  }
  render(){
    return <div>Hello</div>;
  }
}
```


# Usage without a decorator
```
import wrapReactComponentMethodsWithTryCatch from `react-component-errors`

class MyComponent {
  componentDidMount(){
    throw new Error("Test error");
  }
  render(){
    return <div>Hello</div>;
  }
}
wrapReactComponentMethodsWithTryCatch(MyComponent);
```

# Config

```
import {config} from `react-component-errors`
```

`config.enabled`: default is `true` you can set to false to disable wrapping components with try/catch
`config.errorHandler`: default will `console.error` a helpful error message. See example below to override and customize errorHandler.

# Override errorHandler using config
```
import wrapReactComponentMethodsWithTryCatch, {config} from `react-component-errors`

config.errorHandler = (errorReport) => {
  // Do something custom with errorReport

  console.error(errorReport.error) // the Error caught
  console.error(errorReport.component) // the component's name (constructor function)
  console.error(errorReport.method) // the method that we caught an error in (EX: `render`)
  console.error(errorReport.props) // the props passed to the component
  console.error(errorReport.arguments) // any arguments passed to the lifecycle method (if there were any)
};

@wrapReactComponentMethodsWithTryCatch
class MyComponent {
  componentDidMount(){
    throw new Error("Test error");
  }
  render(){
    return <div>Hello</div>;
  }
}
```
