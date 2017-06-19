
## Tips for Component Design

1. Avoid Weak Elements
1. **Declare PropTypes** - Important to also include comments in the PropType declaration as this is where we are getting info when building our docs.
1. Don't Hard Code HTML IDs
1. Declare Logical Defaults
1. Consider Accessibility
1. **Consider Configuration Objects** - Instead of loads of props, if there are a number of props that fit together, group them in an object type prop.  
1. Consider Server-side Rendering 
1. Honor the single responsiblilty Principle

## Atomic Design

Just means your component is a small part/piece of the whole. 

Atoms -> Molecules -> Organisms

Some tips with Atomic Design:

1. **Honor the underlying elements API** - If we are wrapping an in put, then the props that the component accepts should match the underlyting HTML API.  Things like "value", "type", etc.  Obviously, only as necessary, meaning if your component is always a type "text", then you don't accept that as a prop.  But if you accept a value property, don't rename it to something else.
1. **Pass props via the spread** - An example would be ` <input type="text" {...props} /> `.   This would pass through anything the user entered on your component to the base HTML Element.
1. **Use Spread with Destructuring** - Above we recommend using the spread to pass down props.  However, you need to be careful to not pass your non-dom props on to the HTML.  You can do this by destructoring out your custom props and assign the rest to a "rest" array. 
    ```javascript
    const Hello = ({ name, ...rest }) => {
      <div {..rest}>Hi {name} </div>
    }
    ```

1. Think about creating formatting components.  Things like:
    ```javascript
    <Cash>6</Cash> //$6.00
    <Cash showDecimal={false}>6.42</Cash> //$6
    ```
