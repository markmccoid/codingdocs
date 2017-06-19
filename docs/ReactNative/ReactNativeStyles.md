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
	const styles = {
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
	};
```

The "flex" box method is the way we can control the flow of components.