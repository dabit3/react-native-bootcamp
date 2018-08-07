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

If changes are made in this project, the native code needs to be recompiled.

__ios__

This folder contains both the iOS as well as the Apple TV projects. Top open this project, open only the ios/MyProject.xcodeproj in Xcode (`i.e. react-native run-ios`).

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
    const data = Platform.select({
      default: 'Hello world'
    })
    console.log('data:', data)
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

### Creating a component using a function

This same component can be rewritten as a function:

```js
const App = () => 
  <View style={styles.container}>
    <Text style={styles.welcome}>Welcome to React Native!</Text>
    <Text style={styles.instructions}>To get started, edit App.js</Text>
    <Text style={styles.instructions}>{instructions}</Text>
  </View>
```