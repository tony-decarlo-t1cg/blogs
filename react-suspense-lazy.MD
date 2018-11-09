# React 16.6: Suspense and Lazy
As of October 23, 2018, react v16.6.0 has been released with some great new features. In this blog, we'll be talking about how to use the Suspense component to do [code-splitting](https://reactjs.org/docs/code-splitting.html#reactlazy). If you want to checkout some of the other features you can view the changelog [here](https://reactjs.org/blog/2018/10/23/react-v-16-6.html#changelog).

## React.lazy()
**Code-splitting** allows us to **lazy-load** imports, meaning they are only imported when they are being used. This can help to increase the performance of our application. These are known as **dynamic imports**. React.lazy() can render dynamic imports as React components. It takes a **function** that calls our dynamic `import()` and returns a **Promise** that resolves a module with a `default` export containing a React component.

Importing without React.lazy would typically look like this:
```
import OtherComponent from './OtherComponent';

function MyComponent() {
	return (
		<div>
			<OtherComponent />
		</div>
		);
	}
```

Whereas importing with React.lazy would be something like
```
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
	return (
		<div>
			<OtherComponent />
		</div>
		);
	}
```

The only difference between these two blocks of code above is the import statement being added. This is similar to [react-loadable](https://github.com/jamiebuilds/react-loadable).  Here is a similar example:

```
import Loadable from 'react-loadable';
import Loading from './my-loading-component';

const LoadableComponent = Loadable({
  loader: () => import('./my-component'),
  loading: Loading,
});

export default class App extends React.Component {
  render() {
    return <LoadableComponent/>;
  }
}
```

## React Suspense
React Suspense allows you to **suspend** rendering while data is being fetched. When you try to render something that requires data, your `fetch` throws a **promise**. It is important to let your user know that your UI is loading while the fetch is running. In this case we can show a loader to the user. Suspense takes in a prop called `fallback`, that accepts React elements. Let's look at an example: 

```
import React, {lazy, Suspense} from 'react';
const OtherComponent = lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <OtherComponent />
    </Suspense>
  );
}
```

## Simple Example
I wrote a simple example and the code can be found on my [GitHub](https://github.com/tony-decarlo-t1cg/react-suspense-lazy). We can see that when I refresh the page on a simulated Slow 3G network the page renders the `Loading...` message before showing the React logo I am fetching.
![](https://media.giphy.com/media/w8ZiKlUPWR6ph9ZybE/giphy.gif)

## Error boundaries
Modules sometimes cannot load due to a network failure and will cause an error. You can now handle these errors with the following code. We can create a component called `ErrorBoundary` and place it above our lazy components. Here is an example: 
```
import ErrorBoundary from './ErrorBoundary';
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <ErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </ErrorBoundary>
  </div>
);
```

## Route based code splitting
As your application grows, so will your bundle. If your bundle becomes too large then your app will take a long time to load. This is when we introduce "splitting" our bundle. It can be difficult to decide where you want to introduce code splitting, but we can start with **routes**. Here is an example on how to setup route-based code splitting into your application with React.lazy.
```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

## Benefits
* Adapt's to the users device and environment, fast or slow
* No longer need to define extra state variables such as `isFetching`
* Optimize app performance without using an external library
* Error handling
* Route-based code splitting

## Conclusion
React 16.6 had some awesome new additions and allows developers to create high performance and lightweight applications. We will definitely be incorporating these features into our projects. Stay tuned for more updates!
