# Search Input Box

I wanted to be able to have a standard input element autocomplete/search an array as a user typed.

To accomplish this you need the following:

1. Class based React component (with state.backspace set to false)
2. Array of items to search (passed as props in example)
3. Controlled Input element 
4. onKeyDown function
5. onChange function

In the example below, we are expecting an array of *descriptions* to be passed in as a prop.  We will then use a state property called *backspace* to indicate if the **backspace** or **delete** key was pressed.

Here is the input element:

```javascript
<input
  className="text-input"
  type="text"
  placeholder="Description"
  autoFocus
  ref={(input) => this.descInput = input}
  value={this.state.description}
  onChange={this.onDescriptionChange}
  onKeyDown={this.onKeyDown}
/>
```

Here are the **onKeyDown** and **onChange** functions, I have left a couple of console.log statements in that you should remove, but are good to see what is happening.

```javascript
   onKeyDown = (e) => {
    const keyPressed = e.key;
    const description = e.target.value;
    //Check if Backspace or Delete
    if (keyPressed === 'Backspace' || keyPressed === 'Delete') {
      this.setState((prevState) => ({backspace: true}))
    }
  }
  onDescriptionChange = (e) => {
    //Get input value
    //CHECK FOR this.state.backspace and if true, set state to target.value passed
    // and set backspace to false
    const description = e.target.value;
    console.log(`description: ${description}`);
    if (this.state.backspace) {
      return this.setState(() => ({ 
          description,
          backspace: false 
        })
      );
    }
    //Setup match expression
    const matchExpr = description.length > 0 ? '^' + description : /.^/;
    //Create RegExp Object
    const expr = new RegExp(matchExpr, 'ig');
    //Try and Find a match in array of service descriptions
    const foundItem = this.props.descArray.find((desc) => desc.match(expr));
    console.log(`foundItem ${foundItem}`);
    //If not found, return description, else return found item and set selection range
    const finalValue = foundItem || description;
    console.log(`finalValue: ${finalValue} -- length: ${finalValue.length}`);

    const startPos = description.length;
    const endPos = finalValue.length;
    //this.test.setSelectionRange(1, 3);
    this.setState(() => {
        return ({ 
          description: finalValue
        })
      },
      () => {
        if (foundItem) {
          this.descInput.setSelectionRange(startPos, endPos);
        }
      }
    );
  }
```

A few things I think might make this more robust

1. When searching return the smallest string.  Since I'm using find, I get the first item found.  This may be a long description and I think it would be best to just return the shortest string first.
2. I'm only looking for strings that start with the input values typed.  This is a bit limiting, but I think is best because we are only showing one result.
3. In regards to item 2, we could pop up a list of all values matching so user could select.  Maybe use a select element.  But for the app I was working on, this would have been overkill.
4. I'm doing a case insensitive search and as such, changing the case of the first letter if an item is found is difficult.  Not sure of an easy way around this that would make sense to the user.  