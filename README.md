# React Native Bootcamp - Day 1

## Your first React Native App

### 1. Getting Started

1. Go to the React Native docs [Getting Started page](https://facebook.github.io/react-native/docs/getting-started.html), click on __Building Projects with Native Code__, & follow the documentation for getting set up on your operating system.

2. Create a new project using the `react-native init` command:

```bash
react-native init MyProject
```

3. Change into the new directory:

```bash
cd MyProject
```

4. To run your project on iOS, run the following command from the command line:

```bash
react-native run-ios
```

To run your project on Android, first open an Android emulator, then run the following command:

```bash
react-native run-android
```

### 2. Project structure

```
MyProject
│
└───android
│   
└───ios
│   
└───node_modules
│   
│   .babelrc
│   .buckconfig
│   .flowconfig
│   .gitattributes
│   .gitignore
│   .watchmanconfig
│   App.js
│   app.json
│   index.js
│   package.json
```

#### Main things to know:

__android__

This folder contains the Android project. To open this project in Android Studio, only open the Android folder.

If changes are made in this project, the native code needs to be recompiled (`i.e. react-native run-ios`).

__ios__

This folder contains both the iOS as well as the Apple TV projects. Top open this project, open only the ios/MyProject.xcodeproj in Xcode.

If changes are made in this project, the native code needs to be recompiled (`i.e. react-native run-android`).

__index.js__ - Main application entrypoint

```js
import {AppRegistry} from 'react-native';
import App from './App';
import {name as appName} from './app.json';


AppRegistry.registerComponent(appName, () => App);
// AppRegistry.registerComponent gets called only once per application
// This function takes in two arguments:
// First argument - The application name (here, we've imported the appName property from app.json)
// Second argument - The main application component (App)
```

__App.js__ - This component contains the initial boilerplate for the project

__.babelrc__ - This file allows you to update your babel configuration. To do so, install the babel dependency you're interested in using & then update this file with the new dependency

__package.json__ - This file lists all dependencies of the project as well as scripts for running commands within the project

__.flowconfig__ - Preconfigured settings for Flow (A typechecking system that is optional & not enabled by default)

# Intro to React Native

## Basic Components

React Native ships with over 30 UI components & over 40 native device APIs.

UI components display UI along with some functionality, while native device APIs allow you to access native functionality.

Let's take a look at a few components that we're introduced to in App.js:

```js
import {Platform, StyleSheet, Text, View} from 'react-native';
```

### Text
This component allows you to render text to the UI. If you've ever used a __span__, __p__, or __h1__ in HTML, you can think of a __Text__ component as being similar but without any default styling at all.

Usage:

```js
<Text>Hello World</Text>
```

### View 
The most fundamental building block when building UIs in React Native. If you've ever used a __div__ in HTML, then you can think of a __View__ as a being very similar to a __div__.

Usage:

```js
<View>
  <Text>Hello World</Text>
</View>
```

### StyleSheet
A StyleSheet is an abstraction similar to CSS StyleSheets

You can style React components many different ways:
```js
// inline
<Text style={{ color: 'red' }}>Hello World</Text>

// with object
const styles = {
  text: {
    color: 'red'
  }
}
<Text style={styles.text}>Hello World</Text>

// an array
<Text style={[styles.text, { fontSize: 22 }]}>Hello World</Text>
```

But, using the StyleSheet API is recommended for its performance benefits. Here's how we can implement the same styles using the StyleSheet API:

```js
// with object
const styles = StyleSheet.create({
  text: {
    color: 'red'
  }
})
<Text style={styles.text}>Hello World</Text>

// an array
<Text style={[styles.text, { fontSize: 22 }]}>Hello World</Text>
```

### Platform
This component allows us to get information about the current device we're using.

Basic usage:

```js
Platform.OS // returns OS, i.e. ios, android, web, etc..
Platform.Version // returns Version, i.e. 11.2
Platform.isIpad // returns boolean
Platform.isTV // returns boolean
Platform.isTVOS // returns boolean
```

Other usage:

```js
// Platform.select
const greeting = Platform.select({
  ios: 'Hello from iOS',
  android: 'Hello from Android',
  default: 'Hello world'
})
console.log(greeting) // Hello from iOS

// platform specific styling
const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'blue',
      },
      default: {
        backgroundColor: 'orange'
      }
    }),
  },
});
```

## Intro to debugging

Debugging your app is one of the most important things you need to know how to do in order to competently develop in React Native. Let's take a look at some ways to do this.

### Developer Menu

To open the developer menu, click `CMD + d` in iOS or `CTRL + m` on Android.

Here we have 7 options:

- Reload
- Debug JS Remotely
- Enable Live Reload
- Start Systrace
- Enable Hot Reloading
- Toggle Inspector
- Show Perf Monitor

#### Main things to know:

- There is a shortcut for reloading: `CMD + r` on iOS or `m + m` on Android.
- Debug JS Remotely is an easy way to debug & log your JS. This will open a browser window & you can then open developer console to debug.
- Hot reloading is very powerful.
- Toggle inspector is OK. Use the __Touchables__ functionality to highlight touchable components.
- For more serious UI debugging, use [React Devtools](https://github.com/facebook/react-devtools) or [React Native Debugger](https://github.com/jhen0409/react-native-debugger)

## Other React Native "Good to knows"

- You can use any text editor to develop for React Native. I recomment VS Code.
- If you install a JavaScript dependency, you only need to refresh your app or at the most restart the packager to use the new package.
- If you install a native dependency, you must first properly link the dependency & then rerun the native code in order to use it.
- 80% of React Native is just knowing how to use React well
- Keep in mind touchable space when developing for React Native but really mobile in general.


## Understanding React

In React, everything can be thought of in terms of Components.

A Component is a created using either a function or a class.

Each component can return:

A. A piece of UI   
B. A piece of UI with self-contained functionality (state, lifecycle methods, etc..)   
C. Self-contained funcationality without returning UI

### Creating a component using a class

In App.js we see the following class generated for us as part of the initial project boilerplate code:

```js
class App extends Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}
```

As we can see, this component is was created using a class.

When using a class, you have two main benefits:

1. Control of local state   
2. Access to lifecycle methods

In the above class, we have __no__ state but there is a lifecycle method: `render()`

Let's update the component to have some state as well as hook into the other lifecycle methods:

```js
class App extends Component {
  state = { greeting: 'Hello World' } // 1
  static getDerivedStateFromProps() { // 2
    console.log('getDerivedStateFromProps called')
  }
  componentDidMount() { // 4
    console.log('componentDidMount called')
  }
  render() { // 3
    return (
      <View style={styles.container}>
        <Text style={styles.instructions}>{this.state.greeting}</Text>
      </View>
    );
  }
}
```

In the above component, we hook into all of the mounting lifefycle methods. You can see in the comments the order in which they are called.

### Creating a component using a function

This same original App component can be rewritten as a function:

```js
const App = () => 
  <View style={styles.container}>
    <Text style={styles.welcome}>Welcome to React Native!</Text>
    <Text style={styles.instructions}>To get started, edit App.js</Text>
    <Text style={styles.instructions}>{instructions}</Text>
  </View>
```

The above function is created using an ES6 arrow function.

The same function could be rewritten in ES5 this way:

```js
var App = function() {
  return (
    <View style={styles.container}>
      <Text style={styles.welcome}>Welcome to React Native!</Text>
      <Text style={styles.instructions}>To get started, edit App.js</Text>
      <Text style={styles.instructions}>{instructions}</Text>
    </View>
  )
}
```

These both work the same, but most documentation will use ES6 so it's good to know.

To learn more about ES6 arrow functions click [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

### State

State is one of two ways to handle dynamic data in your application, the other being props.

The main difference is that state is controlled by class components, whereas props can be controlled in a multitude of ways.

Let's take another look at a component with some basic state:

```js
class App extends Component {
  state = { greeting: 'Hello World' }
  render() {
    return (
      <Text>{this.state.greeting}</Text>
    );
  }
}
```

`this.state.greeting will be equal to `'Hello World'` in this component.

What if we wanted to change the value of the state?

We can do this, but we need to keep in mind 1 thing: In order to rerender the new value of the greeting, we need to rerender by triggering update lifecyle.

That meains we can't simply do something like this:

```js
changeName = () => {
  this.state.greeting = 'Hello, this is my new greeting!'
}
```

The above function _would_ change the value of state.greeting, but the new value would not show up on the screen, to do that we need to trigger the render method.

There are 3 ways to trigger a rerendering of a component:
1. Calling `setState()`   
2. Calling `forceUpdate` (not recommended for most use cases)   
3. Receiving new props into the component

The best way to change the state in our circumstance would be to call `setState()` which will change the state as well as trigger an update lifecycle:


```js
class App extends Component {
  state = { greeting: 'Hello World' }
  changeGreeting = () => {
    this.setState({ greeting: 'Hello, this is my new greeting!' })
  }
  render() {
    return (
      <View>
        <Text>{this.state.greeting}</Text>
        <Text onPress={this.changeGreeting}>Change Greeting</Text>
      </View>
    );
  }
}
```

### Props

Let's take a look at how to use props.

Instead of hard-coding values, we'll write a component that gets its data passed in as parameters known as "props" in React & React Native.

#### Using props in a Class component

```js
// creation
class Person extends React.Component {
  render() {
    return (
      <Text>Hello, my name is {this.props.name}</Text>
    )
  }
}

// usage
<Person name="Allison">
<Person name="Nader">
<Person name="Amanda">
```

#### Using props in a Functional component

```js
// creation
const Person = (props) => <Text>Hello, my name is {props.name}</Text>

// usage
<Person name="Allison">
```

You may also use an ES6 destruturing assignment to get the individual props out of the function argument:

```js
// creation
const Person = ({ name }) => <Text>Hello, my name is {name}</Text>

// usage
<Person name="Allison">
```


#### Dynamic props

Props can also be dynamic:


```js
const Person = ({ name }) => <Text>Hello, my name is {name}</Text>

class App extends React.Component {
  state = { name: 'Amanda' }
  changeName = () => {
    this.setState({ name: 'Allison' })
  }
  render() {
    return (
      <View>
        <Person name={this.state.name} />
        <Text onPress={this.changeName}>Change Name</Text>
      <View>
    )
  }
}
```