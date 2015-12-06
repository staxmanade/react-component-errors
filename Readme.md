
# What is it?

This is a little utility that wraps you're ES6 (or even ES5) React Component's lifecycle methods
with a try/catch and is useful in development because it exposes errors thrown that are swallowed by React by default.
Now you can more easily diagnose errors during development.

NOTE: It's not likely a good idea to run with this `enabled` in production as it could affect performance of your React components.

## Will this work when Browsers implement Classes?

To be honest, I'm not sure (would love to find out soon). This does work with a [Babel](https://babeljs.io/) transpiled ES6 class.

### More details

You can follow [this GitHub Issue](https://github.com/facebook/react/issues/2461) for more details.
Thanks to inspiration from [skiano/react-safe-render](https://github.com/skiano/react-safe-render/blob/feature/safe-methods/index.js).

# Usage with an es7 @decorator
```
import wrapReactLifecycleMethodsWithTryCatch from 'react-component-errors'

@wrapReactLifecycleMethodsWithTryCatch
class MyComponent extends React.Component {
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
import wrapReactLifecycleMethodsWithTryCatch from 'react-component-errors'

class MyComponent extends React.Component {
  componentDidMount(){
    throw new Error("Test error");
  }
  render(){
    return <div>Hello</div>;
  }
}
wrapReactLifecycleMethodsWithTryCatch(MyComponent);
```

# Config

```
import {config} from 'react-component-errors'
```

`config.enabled`: default is `true` you can set to false to disable wrapping components with try/catch
`config.errorHandler`: default will `console.error` a helpful error message. See example below to override and customize errorHandler.

# Override errorHandler using config

You can see the below running [in a plnkr](http://plnkr.co/edit/VlYsps?p=preview) where we give the helper our own `errorHandler` which uses [Toastr](http://codeseven.github.io/toastr/) to display error messages.

```
'use strict';

import React from 'react';
import ReactDOM from 'react-dom';
import toastr from 'toastr';
import wrapReactLifecycleMethodsWithTryCatch, {config} from 'npm:react-component-errors';

config.errorHandler = (errorReport) => {
   console.error(`Error in ${errorReport.component}.${errorReport.method}(${(errorReport.arguments ? '...' : '')}): ${errorReport.error}`, errorReport);
   toastr.error(`Error in ${errorReport.component}.${errorReport.method}(${(errorReport.arguments ? '...' : '')}): ${errorReport.error}`);
};

@wrapReactLifecycleMethodsWithTryCatch
class MyComponent extends React.Component {

  componentWillMount(){
    throw new Error("Test error");
  }

  render(){
    return <div>{this.state.message}</div>;
  }
}


ReactDOM.render(<MyComponent />, document.getElementById('main'));
```
