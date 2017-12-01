## Styling 101

Styles are added to components via an object you create. By convention, name this object styles.

```javascript
	...
	 <View style={styles.thumbnailContainerStyle}>
	 	<Image
	 		style={styles.thumbnailStyle}
	 source={{ uri: thumbnail_image }}
	 />
	 </View>
	... 
	const styles = StyleSheet.create({
	 headerContentStyle: {
	 justifyContent: 'space-around',
	 flexDirection: 'column'
	 },
	 headerTextStyle: {
	 fontSize: 18
	 },
	 thumbnailStyle: {
	 width: 50,
	 height: 50
	 },
	 thumbnailContainerStyle: {
	 justifyContent: 'center',
	 alignItems: 'center',
	 marginLeft: 10,
	 marginRight: 10,
	 borderColor: '#ddd',
	 borderWidth: 1
	 },
	 imageStyle: {
	 height: 300,
	 flex: 1,
	 width: null
	 }
	});
```

The "flex" box method is the way we can control the flow of components.

## Getting Screen Dimensions

```javascript
import { Dimensions } from 'react-native';
const window = Dimensions.get('window');
//OR Destructure:
let { height, width } = Dimensions.get(“window”);

const styles = StyleSheet.create({
  header: {
    width: window.width,
    height: window.height,
    padding: 10,
  },
});
```

## Extended Stylesheet

This library will add the ability to do media queries, create variables, create themes, etc.

[Extended Stylesheet for React Native](https://github.com/vitalets/react-native-extended-stylesheet)

