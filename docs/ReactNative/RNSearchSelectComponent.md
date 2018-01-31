# Search Select Component

The Search Select Component allows you to show an input box with a list of items.  Typing in the input box searches the available items allowing you to either select one of the items or keep the item typed in.

![](https://dha4w82d62smt.cloudfront.net/items/1S2u0u3e3b0u0t362s1A/Image%202018-01-30%20at%2012.08.08%20AM.png)

You can call this component with anything, however, I also created a *SearchSelectInitiator* component.

![](https://dha4w82d62smt.cloudfront.net/items/1c3k2U3K2C0D1s263t1B/Image%202018-01-30%20at%2012.12.55%20AM.png)

In my implementation, I have used Wix React-Native-Navigation.

## The Scenario

Let's set the scene.  I have a screen, most likely an input screen and there is an input field I want my user to fill, however, this field is one that I know will have many of the same values over time.  In addition, if it is used again, I want to make sure it is the same case and spelled the same as previous values.  

The reason is best seen in an example. 

My input field is *Service Description*.  This will be the service being performed in one or two words.

Take a look at these possible values:

````
Oil Change
oil Change
OilChange
Oil
````

I don't want my user to have multiple values that all mean *Oil Change*.  This component will let you pass an array of the current values for the field they are inputting and then as they type, it will filter those values.  If the user sees what he/she wants, a simple tap will select it and allow it to be saved.

## How it Works

There are two distinct components needed.

1. **SearchSelectInitiator** - this component will be found on your Input screen.  This is what the user taps to get to the actual input area where they can type and filter.
2. **SearchSelect** - this component is where the user will see the list of choices and be able to enter a new/filter value.

### SearchSelectInitiator Component

The *SearchSelectInitiator* component takes the following props:

```javascript
SearchSelectInitiator.propTypes = {
 // The function to call onPress. This should be a wix navigation call
  onPress: PropTypes.func,
 // The label seen to the left
  label: PropTypes.string,
 // The returned value to show.  should be state in the component using the SearchSelectInitiator
  returnValue: PropTypes.string,
}
```

**Example onPress**

```javascript
  // shows SelectServiceScreen
  handleSelectDescription = () => {
    //Push Select Service Screen
    this.props.navigator.push({
      screen: 'car-tracker.SearchSelectScreen', //This screen will house our SearchSelect Component
      title: 'Select Service',
      passProps: {
        onReturnService: this.handleReturnedService,
        selectItems: _.sortBy(_.uniq(this.props.serviceDescriptions))},
      animated: true,
      animationType: 'fade',
    });
  }
```

