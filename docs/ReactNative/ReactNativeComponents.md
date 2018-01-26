# Quick Guide to Useful Components and Modules

## iPhone X SafeAreaView Component

Available in React Native 0.50.1, this component deals with the new iPhone X layout.

```jsx
import {
  ...
  SafeAreaView
} from 'react-native';
class Main extends React.Component {
  render() {
    return (
      <SafeAreaView style={styles.safeArea}>
        <App />
      </SafeAreaView>
    )
  }
}
const styles = StyleSheet.create({
  ...,
  safeArea: {
    flex: 1,
    backgroundColor: '#ddd'
  }
})
```

What has worked for existing apps is to just set some background color that works well with your existing app design, in the above example that is a light gray, and `flex: 1`. We then just wrap the top level UI component of our app in the SafeAreaView and we’re good to go.