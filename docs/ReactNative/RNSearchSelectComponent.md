# Search Select Component

The Search Select Component allows you to show an input box with a list of items.  Typing in the input box searches the available items allowing you to either select one of the items or keep the item typed in.

![](https://dha4w82d62smt.cloudfront.net/items/1S2u0u3e3b0u0t362s1A/Image%202018-01-30%20at%2012.08.08%20AM.png)

You can call this component with anything, however, I also created a *SearchInitiator* component.

![](https://dha4w82d62smt.cloudfront.net/items/1c3k2U3K2C0D1s263t1B/Image%202018-01-30%20at%2012.12.55%20AM.png)

In my implementation, I have used Wix React-Native-Navigation.

The *SearchInitiator* component takes the following props:

```javascript
SearchSelectInitiator.propTypes = {
 // The function to call onPress. This should initial a wix navigation call
  onPress: PropTypes.func,
 // The label seen to the left
  label: PropTypes.string,
 // The returned value to show.  should be state in the component using the SearchInitiator
  returnValue: PropTypes.string,
}
```

**Example onPress**

```javascript
  // shows SelectServiceScreen
  handleSelectDescription = () => {
    //Push Select Service Screen
    this.props.navigator.push({
      screen: 'car-tracker.SearchSelectScreen',
      title: 'Select Service',
      passProps: {
        onReturnService: this.handleReturnedService,
        selectItems: _.sortBy(_.uniq(this.props.serviceDescriptions))},
      animated: true,
      animationType: 'fade',
    });
  }
```

