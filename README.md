# Albums
* Basics of Android App Development with React Native
* Part of [The Complete React Native and Redux Course](https://www.udemy.com/the-complete-react-native-and-redux-course/)
by [Stephen Grider](https://github.com/stephengrider)


## 001 Create and render a simple component
* Instead of importing *ReactNative*, `import { Text, AppRegistry } from 'react-native';`. Now *Text* is available to use for the component creation and *AppRegistry* is also available to use for the component rendering.
```
import React from 'react';
import { Text, AppRegistry } from 'react-native';
```
* Create a simple functional component with JSX. It will be returning just a *Text* tag with a simple message.  
```
const App = () => (
    <Text>Some Text</Text>
);
```
* Render the component by registering it with a call to *AppRegistry*. The first argument is a string defining the name given to the project when we created it with the React Native command line tool, it is *albums* in this case, and the second argument is a function which is returning the component, the *App* component in this case.
```
AppRegistry.registerComponent('albums', () => App);
```  

## 002 Create and nest the *header* component
* Create the *src* folder.
* Create the *src/components* folder.
* Create the *src/components/header.js* file.
* Make the necessary imports.
```
import React from 'react';
import { Text } from 'react-native';
```
* Create the *Header* component function, which returns a *Text* tag.
```
const Header = () => {
    return <Text>Albums</Text>;
};
```
* Only the root component, in this case *App*, uses *AppRegistry*. So, in order to make the *header* component available to the other parts of the app, we need to export it.
```
export default Header;
```
* Import the *header* component in the *index* file and nest it inside the *App* component.

```
import Header from './src/components/header';

const App = () => (
    <Header />
);
```
