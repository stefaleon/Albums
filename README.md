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

## 003 Style the *header* component
* Inside *src/components/header.js* create a *styles* object.
* Add the *textStyle* property and give it a *fontSize* of 20.
```
const styles = {
    textStyle: {
        fontSize: 20    
    }
};
```
* Use it inside the component with destructuring and apply the style to the *Text* tag using the *style* property.
```
const Header = () => {
    const { textStyle } = styles;

    return <Text style={textStyle}>Albums</Text>;
};
```
* Import the *View* tag in order to wrap the *Text* tag with it:
`import { Text, View } from 'react-native';`
* In the *styles* object add color styling for the *View* tag.
```
viewStyle: {
    backgroundColor: '#F8F8F8'
}
```
* In the component, wrap the *Text* tag with the *View* tag and apply the *viewStyle*.
```
    const { textStyle, viewStyle } = styles;

    return (
        <View style={viewStyle}>
            <Text style={textStyle}>Albums</Text>
        </View>
    );
```

## 004 More styling in *viewStyle*
* Center the content vertically with `justifyContent: 'center'`.
* Center the content horizontally with `alignItems: 'center'`.
* Add a `height: 60` and `paddingTop: 15`.
* Add a shadow on the border with
```
shadowColor: '#000',
shadowOffset: { width: 0, height: 2 },
shadowOpacity: 0.2
```
* Finally assign `elevation: 2` and `position: 'relative'`.


## 005 Refactor *header* to receive text as props
* Edit the *Header* function definition so that it is called with a *props* object as its first argument when it is created: `const Header = (props) => {...`
* Replace the hardcoded text with the *headerText* property:
```
<Text style={textStyle}>{props.headerText}</Text>
```
* The *headerText* property has to be passed by the parent component, which is *App*, when the *Header* is rendered. In the *index* file, the *Header* tag is thus modified.
```
const App = () => (
    <Header headerText={'Albums'} />
);
```

## 006 Create the *AlbumList* component
* In *src/components* create the *AlbumList.js* file.
* Make the necessary imports.
```
import React from 'react';
import { Text, View } from 'react-native';
```
* Create the *AlbumList* function and export it.

```
const AlbumList = () => {
    return (
        <View>
            <Text>Album List</Text>
        </View>
    );
};

export default AlbumList;
```

* Import *AlbumList* in *index* and nest it in the *App* component. Since we return more than one nested components, we have to wrap the *App* component's return in a *View* tag. Import the *View* component from *'react-native'* first: `import { AppRegistry, View } from 'react-native';`
```
const App = () => (
    <View>
        <Header headerText={'Albums'} />
        <AlbumList />
    </View>
);
```

## 007 Refactor *AlbumList* to a class based component
* Import the *Component* class from *React*: `import React, { Component } from 'react';`.
* Change the *AlbumList* definition, from a function to a class and define the *render* method, which will return the JSX for the *AlbumList* class component.
```
class AlbumList extends Component {
    render() {
        return (
            <View>
                <Text>Album List</Text>
            </View>
        );
    }
}
```

## 008 Fetching the *Albums* data from the API

* Add the *componentWillMount* lifecycle method in *Albumlist*.
* Install *axios: `$ npm install --save axios`*.
* Inside the *componentWillMount* lifecycle method we will make the http request to the *Albums* API with the use of a *get* type request.
```
componentWillMount() {
    axios.get('https://rallycoding.herokuapp.com/api/music_albums')
        .then(response => this.setstate);
}
```

## 009 Set and update the *albums* state

* In order to rerender the component when the API data are fetched, we will enable component level state inside the *Albumlist* component.
* Set up the initial state with a class level property. The *albums* piece of state will be initialized as an empty array.
 ```
 state = { albums: [] };
 ```
* In the axios promise return, the request handler calls *setState* in order to set the new state of *albums*. Now the array will contain the fetched data, which is a set of objects, defined by *response.data*.
 ```
 axios.get('https://rallycoding.herokuapp.com/api/music_albums')
     .then(response => this.setState({ albums: response.data }));
```

## 010 Render a list of *albums*
* The list of albums is now available as *this.state.albums*.
* We will map over the array of *albums*. Make the *renderAlbums* helper method. For now this will be returning the title of each album, which is available as *album.title*. Add the *album.title* in the *Text* tag as a value for the unique *key* property required from the iterator.
```
renderAlbums() {
    return this.state.albums.map(album =>
         <Text key={album.title}>{album.title}</Text>);
}
```
* Call *renderAlbums* inside the main render of *AlbumList*.
```
render() {
    return (
        <View>
            {this.renderAlbums()}
        </View>
    );
}
```

## 011 Create the *AlbumDetail* component
* In *src/components* create the *AlbumDetail.js* file.
* Make the necessary imports.
```
import React from 'react';
import { Text, View } from 'react-native';
```

* Create the *AlbumDetail* function and export it.

```
const AlbumDetail = () => {

};

export default AlbumDetail;
```
* Import *AlbumDetail* in *AlbumList* and replace the *Text* tag in *renderAlbums* with an *AlbumDetail* tag. There is no need to display the *album.title* anymore and we can make the *AlbumDetail* tag a self-closing one. Pass the *album* variable, which holds the data, as a prop we may call also *album* to *AlbumDetail*.
```
import AlbumDetail from './AlbumDetail';
```
```
renderAlbums() {
    return this.state.albums.map(album =>
        <AlbumDetail key={album.title} album={album}/>);
}
```
* We can also now remove the *Text* import from *AlbumList*.
* In *AlbumDetail* we can now consume the passed prop. We will display just the album title for now.
```
const AlbumDetail = (props) => {
    return (
        <View>
            <Text>{props.album.title}</Text>
        </View>
    );
};
```

## 012 Create the *Card* component
* The *Card* component will be created in order to reduce the amount of styling code on other components.
* In *src/components* create the *Card.js* file.
* Make the necessary imports.
```
import React from 'react';
import { View } from 'react-native';
```
* Create the *Card* function and a *styles* object.
Add the *containerStyle* custom property which contains the styling.
Assign *styles.containerStyle* to the *View* which is the return of *Card* and export *Card*.

```
const Card = () => {
    return (
        <View style={styles.containerStyle}></View>
    );
};

const styles = {
    containerStyle: {
        borderWidth: 1,
        borderRadius: 2,
        borderColor: '#ddd',
        borderBottomWidth: 0,
        shadowColor: '#000',
        shadowOffset: { width: 0, height: 2 },
        shadowOpacity: 0.1,
        shadowRadius: 2,
        elevation: 1,
        marginLeft: 5,
        marginRight: 5,
    }
};

export default Card;
```

## 013 Use the *Card* component in *AlbumDetail*
*  In *src/components/AlbumDetail.js* `import Card from './Card';`.
* Replace the *View* tag with a *Card* tag.
```
<Card>
    <Text>{props.album.title}</Text>
</Card>
```
* We need to instruct *Card* on how to render any children. In *src/components/Card.js*, add a reference to *props* as an argument in the *Card* definition. In order to render any components passed to the *Card*, we add a reference to *props.children* inside its *View* tag.
```
const Card = (props) => {
    return (
        <View style={styles.containerStyle}>
            {props.children}
        </View>
    );
};
```

## 014 Create the *CardSection* component
* In *src/components* create the *CardSection.js* file.
* Make the necessary imports.
```
import React from 'react';
import { View } from 'react-native';
```
* Create the *CardSection* function. Add a reference to *props* as an argument in the *CardSection* definition. It returns a *View* tag which, in order to render any components passed to the *CardSection*, contains a reference to *props.children*.
The *View* tag is styled  with *styles.containerStyle* which is about to be defined right afterwards.
```
const CardSection = (props) => {
    return (
        <View style={styles.containerStyle}>
            {props.children}
        </View>
    );
};
```
* Define a *styles* object. Add the *containerStyle* custom property which contains the styling.
```
const styles = {
    containerStyle: {
        borderBottomWidth: 1,
        padding: 5,
        backgroundColor: '#fff',
        justifyContent: 'flex-start',
        flexDirection: 'row',
        borderColor: '#ddd',
        position: 'relative'
    }
};
```
* Finally export *CardSection*.
```
export default CardSection;
```

## 015 Use the *CardSection* component in *AlbumDetail*
*  In *src/components/AlbumDetail.js* `import CardSection from './CardSection';`.
* Nest *CardSection* tags inside the *Card* tag. Display the artist name as well as the album title.
```
const AlbumDetail = (props) => {
    return (
        <Card>
            <CardSection>
                <Text>{props.album.title}</Text>
            </CardSection>
            <CardSection>
                <Text>{props.album.artist}</Text>
            </CardSection>
        </Card>
    );
};
```

## 016 Edit the layout of the first *CardSection*
* In *src/components/AlbumDetail.js*, refactor the contents inside the *Card* tag. Nest two *View* tags inside the *CardSection* component. The first will contain the artist image. Wrap the *Text* lines, those containing the album title and the artist name, inside the second *View* tag.  
```
<Card>
    <CardSection>
        <View>            
        </View>
        <View>
            <Text>{props.album.title}</Text>
            <Text>{props.album.artist}</Text>
        </View>
    </CardSection>
</Card>
```
* Define a *styles* object. Add the *headerContentStyle* custom property which contains the styling.
```
const styles = {
    headerContentStyle: {
        flexDirection: 'column',
        justifyContent: 'space-around'        
    }
};
```
* Apply the style to the secong *View* container: `<View style={styles.headerContentStyle}>`
