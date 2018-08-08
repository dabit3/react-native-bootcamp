# React Native Bootcamp - Day 2

## Styling

### Flexbox

React Native uses Flexbox by default.

By default, all View components inherit FlexBox layout out of the box. This is different than the web, where we had to specify `display: flex` in order to implement it.

Another difference between React Native & web in this regard is that React Native had a default `flexDirection` of `column`, whereas the default `flexDirection` of web flexbox implementation is `row`.

This brings us the the next question, if you're not already familiar with flexBox: What doe `flexDirection` do?

### Creating UI elements

A very useful exercise when learning about styling is to create a button. A button incorporates many different styling techniques & can teach us a lot about not only styling in general but also FlexBox.

```js
const styles = {
  button: {
    padding: 20, borderRadius: 30, backgroundColor: 'blue'
  },
  textContainer: {
    justifyContent: 'center', alignItems: 'center'
  },
  text: {
    color: 'white'
  }
}

<TouchableHighlight
  style={styles.button}
  onPress={() => console.log('hello')}
>
  <View style={styles.textContainer}>
    <Text style={styles.text}>Click Here</Text>
  </View>
</TouchableHighlight>
```

#### Flexbox has many properties, but the main ones you will be using for layout are these:

`flexDirection` - Defines the primary axis. Options are `row` or `column`. Default is `column`.    
`justifyContent` - Defines the layout of the primary axis. Options are `flex-start`, `center`, `flex-end`, `space-around` & `space-between`. Default is `flex-start`.   
`alignItems` - Defines the layout of the secondary axis. Options are `flex-start`, `center`, `flex-end`, & `stretch`. Default is `stretch`.

## Working with lists

There are multiple ways to work with lists in React Native, but to fundamenally understand what is going on we'll start by looking at how JSX handles basic arrays of items & build from there.

To demonstrate this, let's create a basic component & render out an array of Text components:

```js
const people = [<Text>Amanda</Text>,<Text>Nader</Text>,<Text>Allison</Text>]

class App extends React.Component {
  render() {
    return (
      <View>
        {
          people
        }
      </View>
    )
  }
}

```

As we can see above, we can just place an arry of items within our JSX, & React will render them into a list!

This is very good to know, because now we can think of many ways in which we can leverage arrays in the JS language in order to do other cool stuff.

For example, imagine we have an array of objects we'd like to render as a list:

```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 28 }
]
```

Now, we can use the JavaScript `map` array prototype to iterate over these items & return a new array formatted the way that we would like. Let's see how that would look:

```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 28 }
]

class App extends React.Component {
  render() {
    return (
      <View>
        {
          people.map((person, index) => (
            <View>
              <Text>{person.name}</Text>
              <Text>{person.age}</Text>
            </View>
          ))
        }
      </View>
    )
  }
}
```

Now, that's a lot more interesting! Because `map` returns a new array, we can map over __any__ array & return JSX using the values in the array. This is very powerful.

Let's take this one step further. Say we did not want all of this logic in our render method. Well, we can easily move some this functionality into a class method. Let's see how that would look:

```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 28 }
]

class App extends React.Component {
  renderPerson(person) {
    return (
      <View>
        <Text>{person.name}</Text>
        <Text>{person.age}</Text>
      </View>
    )
  }
  render() {
    return (
      <View>
        { people.map(this.renderPerson) }
      </View>
    )
  }
}
```

Above we are passing the item from the iterator (people.map) to another method, `renderPerson`, which is doing the work for us of returning the item for each iteration.

Finally, if we would like to move all of the logic out, we can either store the entire functionality in a variable, function, or class method (below we've moved this into a class method):


```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 28 }
]

class App extends React.Component {
  renderPeople() {
    return people.map((person, index) => (
      <View>
        <Text>{person.name}</Text>
        <Text>{person.age}</Text>
      </View>
    ))
  }
  render() {
    return (
      <View>
        { this.renderPeople() }
      </View>
    )
  }
}
```

So, what's the point of learning all of this? We need to know what's going on under the hood when working with other List abstractions.

In React Native, we sometimes will be working with very large lists. When working with large lists, we do not want to return everything to the UI because it will slow our application down. Instead, we would like to only render the items that are currently in view to the user.

In React Native this is easy to do. We are given native List components that abstract away the complex functionality & allow us to just pass in an array of data & the components handle this for us.

There are a couple of list implementations that are shipped with the framework: [FlatList](https://facebook.github.io/react-native/docs/flatlist) & [SectionList](https://facebook.github.io/react-native/docs/sectionlist).

FlatList renders a basic array of items, & SectionList renders a sectioned list of items providing an optional header for each section.

We will look at FlatList as its the most used.

The basic API of FlatList looks like this:

```js
<FlatList
  data={[{ name: 'Nader', age: 37 }, { name: 'Melissa', age: 33 }]}
  renderItem={(data) => <Text>{ data.item.name }</Text>}
/>
```

We can also add a `keyExtractor` method. From the docs:   
> `keyExtractor` is used to extract a unique key for a given item at the specified index. Key is used for caching and as the react key to track item re-ordering. The default extractor checks item.key, then falls back to using the index, like React does.

Let's update our class with the FlatList & also implement the `keyExtractor` method:

```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 22 }
]

class App extends React.Component {
  render() {
    return (
      <View>
        <FlatList
          data={people}
          renderItem={(data, index) => <Text>{ data.item.name }</Text>}
          keyExtractor={item => item.name}
        />
      </View>
    )
  }
}
```

Finally, let's take a look at how to extract the renderItem method to it's own function like we did when we were writing our lists from scratch:

```js
const people = [
  { name: 'Allison', age: 49 },
  { name: 'Nader', age: 37 },
  { name: 'Amanda', age: 22 }
]

class App extends React.Component {
  renderItem(data) {
    return <Text>{ data.item.name }</Text>
  }
  render() {
    return (
      <View>
        <FlatList
          data={people}
          renderItem={this.renderItem}
          keyExtractor={item => item.name}
        />
      </View>
    )
  }
}
```


## Fetching data over the network

There are a couple of ways I recomment fetching data in React & React Native as far as libraries are concerned.

The first is using [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), which is polyfilled & available out of the box with React Native.

The other us [Axios](https://github.com/axios/axios) which is an open source project that works similarly to fetch but provides a different API that many people like.

I would recommend checking them both out & I have shipped applications using both, but because `fetch` is already available to us, let's work with it to show how to perform network requests.

The demo we'll show here is going to be using the Star Wars API.

The Star Wars API is an open API that allows developers to retrieve data about the Star Wars movie & characters.

What we'll be doing is fetching data from the API & rendering the data in a list using the above FlatList component.

This will be a great next step because we'll be combining a lot of what we've learned up until now, including state, lifecycle methods, & lists.

Let's take a look at how to do this, then we'll walk through what is going on:

```js
import React from 'react';
import {StyleSheet, Text, View, FlatList} from 'react-native';

class App extends React.Component {
  state = { // 1
    people: [],
    loading: true,
  }
  async componentDidMount() { // 2
    try {
      const data = await fetch('https://swapi.co/api/people/')
      const people = await data.json()
      this.setState({
        people: people.results, loading: false
      })
    } catch (err) {
      console.log('error fetching people:', err)
    }
  }
  renderItem(data) { // 3
    return <Text>{ data.item.name }</Text>
  }
  render() {
    return (
      <View style={{ padding: 25 }}>
        { // 4
          this.state.loading && <Text>Loading...</Text>
        }
        <FlatList
          data={this.state.people} // 5
          renderItem={this.renderItem}
          keyExtractor={item => item.name}
        />
      </View>
    )
  }
}
```

1. We create an initial state with two properties: `people` & `loading`. `people` is set to an empty array & `loading` is set to false.   

2. In the `componentDidMount` lifecycle method, we call the API and store the result in a variable called data. Because fetch returns a response, we transform the response to JSON, & then reset the state with `people` set to the result from the API. We also set `loading` to false.   

3. In RenderItem, we return a Text component for each item in the array being rendered.   

4. If `this.state.loading` is true, we show a loading message to the user.   

5. The FlatList component now uses our state variable `this.state.data` as the data to render.

## Working with User Input

