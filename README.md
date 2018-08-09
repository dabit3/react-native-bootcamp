# React Native Bootcamp - Day 3

## Native Components

### Accessing the user's Camera Roll

To access the user's camera roll we need to first add & link the native component.

To do this, we'll need to go ahead & open the Xcode project.

Once the Xcode project is open, do the following:

1. Right click on __Libraries__ in the folders on the left.   
2. Choose __Add files to "YOURPROJECTNAME"__.
3. Navigate to the `node_modules` folder of your project, & choose the following item: `node_modules/react-native/Libraries/CameraRoll/RCTCameraRoll.xcodeproj`
4. Click on __Build Phases__ -> __Link Binary With Libraries__
5. Click on the __+__ button, & choose `libRCTCameraRoll.a`.
6. Click on Info.plist
7. Add a new property: `Privacy - Photo Library Usage Description` with the value of 'This application would like to access your camera roll'.

Once we've added the necessary libraries, we can now run the project.

The following code will allow us to read the user's camera roll from JavaScript on both iOS & Android:

```js
import React, {Component} from 'react';
import {PermissionsAndroid, Platform, StyleSheet, Text, View, CameraRoll} from 'react-native';

export default class App extends Component {
  checkPlatform = async() => {
    if (Platform.OS === 'ios') {
      this.getPhotos()
    }
    if (Platform.OS === 'android') {
      try {
        const granted = await PermissionsAndroid.request(
          PermissionsAndroid.PERMISSIONS.READ_EXTERNAL_STORAGE,
          {
            'title': 'Alert',
            'message': 'This app would like to access your camera roll.'
          }
        )
        console.log('granted: ', granted)
        if (granted === PermissionsAndroid.RESULTS.GRANTED) {
          this.getPhotos()
        } else {
          console.log("Camera permission denied")
        }
      } catch (err) {
        console.warn(err)
      }
    }

  }
  async getPhotos() {
    try {
      const photos = await CameraRoll.getPhotos({ first: 10, assetType: 'All' })
      console.log('photos: ', photos)
    } catch (err) {
      console.log('error:', err)
    }
  }
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text onPress={this.checkPlatform} style={styles.instructions}>Get Photos</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

```

## Animations

The recommended way to animate in React Native for most cases is by using the [Animated API](https://facebook.github.io/react-native/docs/animated).

There are three main Animated methods that you can use to create animations:

Animated.timing() — Maps time range to easing value.
Animated.decay() — starts with an initial velocity and gradually slows to a stop.
Animated.spring() — Simple single-spring physics model (Based on Rebound and Origami). Tracks velocity state to create fluid motions as the toValue updates, and can be chained together.

Along with these three Animated methods, there are three ways to call these animations along with calling them individually. We will be covering all three of these as well:

Animated.parallel() — Starts an array of animations all at the same time.
Animated.sequence() — Starts an array of animations in order, waiting for each to complete before starting the next. If the current running animation is stopped, no following animations will be started.
Animated.stagger() — Array of animations may run in parallel (overlap), but are started in sequence with successive delays. Very similar to Animated.parallel() but allows you to add delays to the animations.

Let's start by taking a look at how to create the core animation that is Animaed.timing()

### Animated.timing

In order to set up an animation, you need to do these things:

1. Import the Animated component from React Native - `import { Animated } from 'react-native`   
2. Create a new Animated value as a class property - `this.animatedValue = new Animated.Value(0)`   
3. Create an animated component using the `Animated` component & the passing a style to is using the Animated value you created in step 2: `<Animated.View style={{ height: 40, width: 40, marginLeft: this.animatedValue }} />`
4. Trigger the animation:

```js
animate = () => {
  Animated.timing(
    this.animatedValue,
    {
      toValue: 100,
      duration: 1500
    }
  ).start()
}
```

`Animated.timing` also has a callback that is fired when the animation is complete:

```js
animate = () => {
  Animated.timing(
    this.animatedValue,
    {
      toValue: 100,
      duration: 1500
    }
  ).start(() => {
    alert('animation complete!')
  })
}
```

You can also interpolate the animated values in order to do things like animated strings (colors, rotation, etc..):

```js
// starting animated value set to 0
this.animatedValue = 0

// final animated value set to 1
animate = () => {
  Animated.timing(
    this.animatedValue,
    {
      toValue: 1,
      duration: 1500
    }
  ).start(() => {
    alert('animation complete!')
  })
}

// in the render method, we interpolate the 0 to 1 value into two colors:
render() {
  const backgroundColor = this.animatedValue.interpolate({ inputRange: [0, 1], outputRange: ['black', 'red'] })
  return <View style={{ height: 40, width: 40, backgroundColor: backgroundColor }} />
}

```

### Animated.parallel

You can also create parallel animations by passing in an array of animations:

```js
class Parallel extends React.Component {
  this.animatedValue1 = new Animated.Value(0)
  this.animatedValue2 = new Animated.Value(0)
  this.animatedValue3 = new Animated.Value(0)

  componentDidMount () {
    this.animate()
  }
  animate () {
    const createAnimation = function (value, duration, easing, delay = 0) {
      return Animated.timing(
        value,
        {
          toValue: 1,
          duration,
          easing,
          delay
        }
      )
    }
    Animated.parallel([
      createAnimation(this.animatedValue1, 2000, Easing.ease),
      createAnimation(this.animatedValue2, 1000, Easing.ease, 1000),
      createAnimation(this.animatedValue3, 1000, Easing.ease, 2000)        
    ]).start()
  }
  render () {
    const scaleText = this.animatedValue1.interpolate({
      inputRange: [0, 1],
      outputRange: [0.5, 2]
    })
    const spinText = this.animatedValue2.interpolate({
      inputRange: [0, 1],
      outputRange: ['0deg', '720deg']
    })
    const introButton = this.animatedValue3.interpolate({
      inputRange: [0, 1],
      outputRange: [-100, 400]
    })
    return (
      <View style={[styles.container]}>
        <Animated.View 
          style={{ transform: [{scale: scaleText}] }}>
          <Text>Welcome</Text>
        </Animated.View>
        <Animated.View
          style={{ marginTop: 20, transform: [{rotate: spinText}] }}>
          <Text
            style={{fontSize: 20}}>
            to the App!
          </Text>
        </Animated.View>
        <Animated.View
          style={{top: introButton, position: 'absolute'}}>
          <TouchableHighlight
            onPress={this.animate.bind(this)}
            style={styles.button}>
            <Text
              style={{color: 'white', fontSize: 20}}>
              Click Here To Start
            </Text>
        </TouchableHighlight>
        </Animated.View>
      </View>
    )
  }
}
```

### Animated.sequence & Animated.stagger()

Animated.sequence() & Animated.stagger() both work in a similar way in the sense that you pass in an array of animations.

Animated.sequence - Starts an array of animations in order, waiting for each to complete before starting the next. If the current running animation is stopped, no following animations will be started.

Animated.stagger - Array of animations may run in parallel (overlap), but are started in sequence with successive delays. Very similar to Animated.parallel() but allows you to add delays to the animations.

```js
// Sequence in use
Animated.sequence([
  Animated.timing(
    animatedValue,
    {
      //config options
    }
  ),
  Animated.spring(
     animatedValue2,
     {
       //config options
     }
  )
])

// Stagger in use:
Animated.stagger(1000, [
  Animated.timing(
    animatedValue,
    {
      //config options
    }
  ),
  Animated.spring(
     animatedValue2,
     {
       //config options
     }
  )
])
```

## Navigation

There are two main libraries that I recommend for navigation in React Native: [React Navigation](https://reactnavigation.org/) & [React Native Navigation](https://github.com/wix/react-native-navigation/tree/v2)

For our examples, we'll be using React Navigation.

To get started, we first need to install React Navigation:

```bash
yarn add react-navigation
```

There are three main types of navigation in most mobile applications, & also available to us in React Navigation:   

1. Stack Navigation - When navigating from page to page, a transition is overlayed with the new route or popped to the old route. The idea is this ends up being an array of routes that you can push to / pop from.   

2. Tab Navigation - Tabs are shown to the user, either at the top or bottom of the app, allowing the user to click on a tab to view the contents of the tab. Many times the tab view will also be a StackNavigator making for nested navigation.   

3. Drawer Navigation - This is typically when a menu slides out from either the left or right & the user can click on an item to navigate.   

Let's take a look at each & also how to have a tab navigator with a nested stack navigator.

### StackNavigator

To create a stack navigator, you need at least one route.

```js
import React from 'react'
import { View, Text, } from 'react-native'
import { createStackNavigator } from 'react-navigation'

const Home = (props) => <View>
  <Text>Home</Text>
  <Text onPress={() => props.navigation.navigate('Page2')}>Go to page 2</Text>
</View>

const Page2 = (props) => <View>
  <Text>Page2</Text>
  <Text onPress={() => props.navigation.goBack()}>Go back</Text>
</View>

const Navigation = createStackNavigator({
  Home: { screen: Home },
  Page2: { screen: Page2 }
})

export default Navigation
```

### TabNavigator

A tab navigator has a very similar API to the stack navigator:

```js
import React from 'react'
import { View, Text, } from 'react-native'
import { createBottomTabNavigator } from 'react-navigation'

const Home = (props) => <View>
  <Text>Home</Text>
  <Text onPress={() => props.navigation.navigate('Page2')}>Go to page 2</Text>
</View>

const Page2 = (props) => <View>
  <Text>Page2</Text>
  <Text onPress={() => props.navigation.navigate('Home')}>Go Home</Text>
</View>

const Navigation = createTcreateBottomTabNavigatorbNavigator({
  Home: { screen: Home },
  Page2: { screen: Page2 }
})

export default Navigation
```

### DrawerNavigator

A drawer navigator also has a similar API to the other navigators we've looked at so far.

The main difference being how the drawer is opened & closed:

```js
this.props.navigation.openDrawer();
this.props.navigation.closeDrawer();
```

```js
import React from 'react'
import { View, Text, } from 'react-native'
import { createDrawerNavigator } from 'react-navigation'

const Home = (props) => <View>
  <Text>Home</Text>
  <Text onPress={() => props.navigation.openDrawer()}>View Menu</Text>
</View>

const Page2 = (props) => <View>
  <Text>Page2</Text>
  <Text onPress={() => props.navigation.openDrawer()}>View Menu</Text>
</View>

const Navigation = createDrawerNavigator({
  Home: { screen: Home },
  Page2: { screen: Page2 }
})

export default Navigation
```

### Nested Navigation

 But in reality, most apps will end up having a combination or navigator nested within each other. A prime example of this is a stack navigator nested within a drawer navigator:

 ```js
 import React from 'react'
import { View, Text, } from 'react-native'
import { createStackNavigator, createBottomTabNavigator } from 'react-navigation'

const Home = (props) => <View>
  <Text>Home</Text>
  <Text onPress={() => props.navigation.navigate('Page2')}>Go to page 2</Text>
</View>

const Page2 = (props) => <View>
  <Text>Page2</Text>
  <Text onPress={() => props.navigation.goBack()}>Home</Text>
</View>

const SignUp = (props) => <View>
  <Text>Sign Up Page</Text>
</View>

const Main = createStackNavigator({
  Home: { screen: Home },
  Page2: { screen: Page2 }
})

const Tabs = createBottomTabNavigator({
  SignUp: { screen: SignUp },
  Main: { screen: Main } 
})

export default Tabs

```

## Authentication

T
