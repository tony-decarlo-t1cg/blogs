# React Router v4
React Router is a collection of **navigational components** for your application. It allows your users to navigate through different URLs for you web app and gives access to features such as bookmarking. Basically, the router makes it so when the page changes, something happens.

## Installation
React Router DOM is published to npm and can be installed with `npm i react-router-dom` or `yarn add react-router-dom`. 

## Getting Started
You'll need a React app to get started. You can either run `create-react-app`, follow along with this [codesandbox.io](https://codesandbox.io/s/github/tony-decarlo-t1cg/react-router) example, or clone on [GitHub](https://github.com/tony-decarlo-t1cg/react-router).

First, we're going to do a top level import to bring in all of the components we need from react-router-dom.
```
import { BrowserRouter, Route, Switch, Link } from 'react-router-dom';
```

Second, we'll add BrowserRouter to our App.js.
```
class App extends Component {
  render() {
    return (
      <BrowserRouter>
        <div>Hello</div>
      </BrowserRouter>
    );
  }
}
```

## Basic Routing
Let's create two small components that return a header tag with corresponding text. 
```
const Home = () => <h1>Home</h1>;
const About = () => <h1>About Page</h1>;
```
We create routes with our `<Route>` component that was imported earlier. It allows us to render components based on our location path. We should also specify the `component` prop in order to know which component to render.
```
<Route exact path="/" component={Home} />
<Route exact path="/about" component={About} />
```
Now if we add `/about` in our URL we will see the "About Page" header displayed.

## Links
Your application needs to navigate between these routes instead of manually typing them in. Typically we would use an `<a>` tag but we'll use **Link** from React Router.
```
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/students">Students</Link>
            </li>
          </ul>
```
We now have clickable Links that will navigate us.

## Error Route
We can create an Error Route for when a route is not found. This is done using `<Switch>` and specifying a `<Route>` without a path.
```
const ErrorRoute = () => <h1>Sorry, page not found...</h1>;
class App extends Component {
  render() {
    return (
      <BrowserRouter>
          <Switch>
            <Route exact path="/" component={Home} />
            <Route exact path="/about" component={About} />
            <Route component={ErrorRoute} />
          </Switch>
      </BrowserRouter>
    );
  }
}
```

## Nested Routes
Previously, we were just doing **Static Routing** to simply load a page. However, sometimes we need to display dynamic data. Let's pretend that I'm getting data from a student API that holds information such as their id, name, and class. I want to create a `/students` route to show all of the students name's but also wanted a Link to `/students/:id` so that when I click on their name it will show me all of their individual profile information.
```
const studentAPI = {
  students: [
    { id: 1, name: 'John Doe', class: 'Web Dev 101' },
    { id: 2, name: 'Jane Doe', class: 'Web Dev 201' },
    { id: 3, name: 'Bob Doe', class: 'Web Dev 301' }
  ]
};
```
Let's create a `/students` route that will map out all of the student's names as Links to `/student/:id`
```
const Students = () => {
  const studentList = studentAPI.students.map(student => (
    <ul key={student.id} style={{ listStyleType: 'none' }}>
      <Link to={`/students/${student.id}`}>
        <li>{student.name}</li>
      </Link>
    </ul>
  ));
  return studentList;
};
```
This route would be: 
```
<Route exact path="/students" component={Students} />
```
Now let's create the StudentProfile component so that when we click on a student it will show their corresponding information.
```
const getStudentAPIByID = id => {
  const getStudent = studentAPI.students.find(student => {
    return student.id === id;
  });
  return getStudent;
};

const StudentProfile = props => {
  const studentID = parseInt(props.match.params.id);
  const student = getStudentAPIByID(studentID);
  if (student) {
    return (
      <ul style={{ listStyleType: 'none' }}>
        <li>ID: {student.id}</li>
        <li>Name: {student.name}</li>
        <li>Class: {student.class}</li>
      </ul>
    );
  } else {
    return <h1>Student not found...</h1>;
  }
};
```
This route would be:
```
<Route exact path="/students/:id" component={StudentProfile} />
```

## Conclusion
There is a lot more you can do with React Router but we've covered all of the basics above. I highly recommend bringing in React Router into your projects as it's a great and simple way to navigate through your application and change views. For more information, you can visit the official documentation [here](https://reacttraining.com/react-router/web/guides/quick-start).
