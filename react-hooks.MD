# React 16.7: Hooks
<img src="https://vignette.wikia.nocookie.net/spongebob/images/0/02/Fish_Hooks_Park-4.png/revision/latest?cb=20140731083503"
		 height="200"
		 width="300"
		 alt="Hooks"
		 ></img>

Hooks are currently a new feature proposal in React v16.7.0-alpha. They allow you to use **state** and other React features without writing a class. To learn more in depth you can watch the demo from [React Conf 2018](https://www.youtube.com/watch?v=dpw9EHDh2bM).

There are 3 main problems that React struggles with: 
* Reusing logic
* Giant components
* Confusing classes

The first problem is reusing logic between multiple components causing "wrapper hell". The second problem is giant components have logic that is split across multiple life cycle methods which can be difficult to follow. The third problem is difficulty understanding classes and when to use a class versus a functional component. There can be a lot of boilerplate code in converting between the two. A lot of developers can also struggle with using `this` and `binding`. 

All of these problems are intertwined. Wrapper hell can be solved by putting more logic into components but then our components become huge. However, if we try to split components into smaller reusable parts then we end up with more nested components in the tree and are back to wrapper hell. The problem is that React does not currently provide a lightweight stateful primitive that is simpler than a class component. This is what hooks will solve.

### What is a Hook?
A Hook is a special function that allows you to "hook into" React features.

### When do we use Hooks?
A hook should be used when you have a functional component that needs state added to it.

## Basic Example
We can take a look at Dan Abramov's demo from React Conf 2018 and see just how much cleaner our code can be from the exact same class component to a functional component using Hooks...
<div>
<img src="https://pbs.twimg.com/media/DqnGs6yWwAAYPXp.jpg"
		 height="700"
		 width="600"
		 alt="Without Hooks">
		 </img>
		 <img src="https://pbs.twimg.com/media/DqnGuEnWsAAt1Xt.jpg"
		 height="700"
		 width="600"
		 alt="With Hooks">
		 </img>
</div>

*Images from* [@threepointone](https://twitter.com/threepointone/status/1056594421079261185)


<div>
<video height="600" width="800" controls="controls">
  <source  type="video/mp4" src="https://video.twimg.com/tweet_video/DqsCilOU0AAoS7P.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>
</div>

*Gif from* [@prchdk](https://twitter.com/prchdk/status/1056960391543062528)

## useState Example
```
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Here we are importing the **useState** Hook. `useState` returns a pair of the *current* state value and a function that updates it. This is similar to how we would typically use `this.state` and `this.setState`. We pass in an argument of `0` to `useState` as the initial state value. 

## useEffect Example
React components often fetch data, subscribe to events, or change the DOM. These operations are called "side effects".  The Effect Hook allows us to perform these side effects from a functional component rather than a class component. It is the same as using `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

Here is an example that sets the document title after React updates the DOM:

```
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

You can see Effects are declared inside the component so they will have access to state and props. React will run the effects after every render. Effects also have the option to "clean up" by returning a function. Hooks allow us to organize effects in a component by relation rather than splitting by lifecycle methods.

## Rules of Hooks
1. Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions.
2. Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions.


## Trying Out Hooks
If you want to try out Hooks, I have a GitHub repository setup [here](https://github.com/tony-decarlo-t1cg/react-hooks). You can clone this repository or create your own react app and run `npm i react@^16.7.0-alpha.0 react-dom@16.7.0-alpha.0`


## Conclusion
Overall, Hooks can be intimidating and confusing at first but ultimately make our lives easier if we learn to use them. Don't worry, there are no plans to remove classes from React. Hooks are backwards-compatible and opt-in. I don't recommend rewriting all of your class components to use Hooks, but try writing your next component with them! 




#### Additional Resources:
* [useState](https://reactjs.org/docs/hooks-state.html)
* [Hooks FAQ](https://reactjs.org/docs/hooks-faq.html)
* [Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)
