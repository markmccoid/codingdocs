# App Creation

To create an app use the following:

	//Create the project -- this creates a directory of projectName
	react-native init projectName
	
	//Run the simulator
	react-native run-ios

The **index.ios.js** is the entry point for the ios app. If you want to create an app that works on both Android and ios you can have the main app code in an underlying app. Either way, you need to use the AppRegistry component/function to start the app. Below is an example using an underlying App component for both Android and ios.

	import { AppRegistry } from 'react-native';
	import App from './src/App';
	
	AppRegistry.registerComponent('projectName', () => App);

The AppRegistry.registerComponent function has two arguments. The first must be the name you used when you ran the react-native init command. The next argument must be a function returning the a react component. Above, we are importing the App component from the src directory.

NOTE: To get a view to scroll you need to put a style of flex: 1 on the top level view of the components you want to be scrollable:

	 <View style={{ flex: 1 }}>
	 <Header headerText='Albums!'/>
	 <AlbumList />
	 </View>

# Styles

Styles are added to components via an object you create. By convention, name this object styles.

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

The "flex" box method is the way we can control the flow of components.