Connects your forms to redux. Documentation can be found:

[Redux Form docs](http://redux-form.com/6.0.0-rc.3/docs/GettingStarted.md/)

## Basic Implementation

Redux form is a higher order component that wraps one of your components so that it can inject form state into your redux store. 

```javascript
//If you have no need for any state from your redux store
...
export default reduxForm({
	form: yourFormName,
	validate: validateFunction
})(YourComponent);

//If you DO NEED state from your redux store, then create a variable with the reduxForm higher order component and send that to the redux connection function:
...
const YourComponent = reduxForm({
	form: yourFormName,
	validate: validateFunction
})(YourComponent);
export default connect(mapStateToProps)(YourComponent);

//I think there may be a more consise way to write the above, but not sure exactly how
```

There are also other properties you can send to the reduxForm call, one being initialValues. However, if you send a prop called initialValues then you do not need to send it with the reduxForm call.

## Field Component

The main component that reduxForm uses to define the form inputs is the Field component.

I have mainly used it and passed it custom inputs, but it can be used by simply telling it to use a basic HTML input type.

Here is what a basic field looks like:

```javascript
//using the default supported DOM inputs (input, textarea or select)
<Field name="description" component="input" type="text" placeholder="Description"/>
	
//Using a custom component for input more easily couples error html
const renderField = (props) => {
	return (
		<div>
			<label style={{height:"28px"}}>{props.placeholder}:
				{props.meta.touched && props.meta.error ? <span className="field-error">{props.meta.error}</span> : null}
			</label>
				<input {...props.input} type={props.type}/>
		</div>
	)
};
<Field name="description" component={renderField} type="text" placeholder="Description"/>
```

One of the things when creating custom components to understand is that the custom component will get props passed to it. The **props.input ** properties contain the properties that your HTML input element will need.

You can also send **custom data** to your components. I have used the data={someData} and then access it with props.data, but I'm assuming you can use anything as long as it doesn't conflict with other props.

You can also augment the onChange event with your own function. Note that the redux forms onChange comes in as props.input.onChange. If you send in your own, note that you STILL MUST run redux forms onChange event.

Here is an example with custom data and a custom onChange:

```javascript
//Custom Component to pass to Field component
const renderDropDownWOnChange = (props) => {
	return (
		<div>
			<label style={{height:"28px"}}>{props.placeholder}:</label>
			<select {...props.input}
				onChange={(e) => {
					props.myOnChange(e);
					props.input.onChange(e);
				}}
				value={props.currApplication}
				>
				{props.data.map(item => <option key={item} value={item}>{item}</option>)}
			</select>
		</div>
	);
};
....

<Field name="application"
		component={renderDropDownWOnChange} type="select"
		placeholder="Application"
		myOnChange={(e) => this.props.onApplicationChange(e.target.value)}
		data={[APP_DROPDOWN_DEFAULT, ...this.props.applicationList]}
		currApplication={this.props.currApplication}
/>
```

## Submitting the Form

When you wrap your Component with the reduxForm() higher Order component, a number of props get injected. The one that is always needed is the **handleSubmit ** function. This will be what your form calls when submitted. You can augment this function and will most assuredly want to. This function gives you the data that was entered into your form so that you can do something with it:

I believe the way this works is that Redux Forms will do what it needs to do to handle the submit and then call your function with the form data.

```javascript
//Notice that handleSubmit accepts a function that is called with the form 
//data when the form is submitted.
//The data object sent will be in the form of 
// data = {fieldName: fieldValue, ...}

...
return (
		<div className="row">
			<div className="columns callout secondary" style={{margin:"0 15px"}}>
				<h4>Add Qlikview Variable</h4>
					<form onSubmit={handleSubmit(data => {
								this.props.onSaveToSever(data);
							})
						}>
						<div className="column small-3">
								<Field name="application"
									component={renderDropDownWOnChange} type="select"
									placeholder="Application"
									myOnChange={(e) => this.props.onApplicationChange(e.target.value)}
									data={[APP_DROPDOWN_DEFAULT, ...this.props.applicationList]}
									currApplication={this.props.currApplication}
								/>
						</div>
						{formNextFields}
					</form>
			</div>
		</div>
	);
}
}
```

A couple of other useful props that get sent -> **invalid, submitting** 

These will allow you to check whether the form has any invalid field and/or if it is being submitted. The easiest way to use these is on the Submit button so that it is disabled if the form is being submitted or is invalid.

```javascript
const { handleSubmit, invalid, submitting, renderAllFields } = this.props;
...
button
	type="submit"
	className="button small"
	style={{marginRight:"5px"}}
	disabled={invalid || submitting}
>Save</button>
```	

## Validation and Normalizing

Normalizing is a prop on the Field component that expects a function. This function will get passed all the input from the Field so that it can "normalize" it. Things like formatting a phone number or simply keeping people from using spaces or special characters.

```javascript
//Here is a "normalizer" function that won't allows spaces in input
const noSpaces = value => value.replace(/ /g,'');

<Field 
	name="name" 
	component={renderField} 
	type="text" 
	placeholder="Name(no spaces)" 
	normalize={noSpaces}
/>
```

Validation can get complicated if it needs to be async, so see the real docs for that. For simple validation, you simply create a function and declare it in the reduxForm() call.

The validate function get a values object that contains a list of all your form fields as you declared them in the Field component declarations. For example, if you had declared the following:

```javascript	
<Field 
	name="name" component={renderField} type="text" 
	placeholder="Name(no spaces)" normalize={noSpaces}
/>
<Field 
	name="description" component={renderField} 
	type="text" placeholder="Description"
/>
<Field 
	name="expression" component={renderTextArea} 
	type="text" placeholder="Expression"
/>
<Field
	name="notes" component={renderTextArea} 
	type="text" placeholder="Notes"
/>
//You would get the following passed to your validate function:
values ={
	name: nameValue,
	description: descriptionValue,
	expression: expressionValue,
	notes: notesValue
	}

//Your Validate function would return an errors object as follows:
errors ={
	name: nameErrorText,
	description: descriptionErrorText,
	expression: expressionErrorText,
	notes: notesErrorText
	}
```

So in your validate function you build the errors object. If there is no error, then you don't include that field name in the error object. Here is a simple sample:

```javascript
//Validation function that redux forms will use to validate input
const validate = values => {
	const errors = {}
	if (!values.name) {
		errors.name = 'Required';
	}
	if (!values.description) {
		errors.description = 'Required';
	}
	if (!values.expression) {
		errors.expression = 'Required';
	}
	if (values.group === GROUP_DROPDOWN_DEFAULT) {
		errors.group = 'Select a Group';
	}
	return errors;
};
```

## Initial Values

Redux form will initialize your form by looking for a prop called " **initialValues** ". This prop should be an object in the same shape as the final form output. This final output being defined by the Field components used in your form. I think it just matches the name property that you give each field with the object name property.

```javascript
<form onSubmit={this.props.handleSubmit(data => {console.log(data)})}>
	<div className="column small-4">
		<label>Name:</label>
		<Field name="name" component="input" type="text" value={name}/>
	</div>
	<div className="column small-8">
		<label>Description:</label>
		<Field name="description" component="input" type="text" value={description}/>
	</div>
	<div className="column small-12">
		<label>Expression:</label>
		<Field name="expression" component="textarea" type="text" value={expression}/>
	</div>
	<div className="column small-8">
		<label>Notes:</label>
		<Field name="notes" component="textarea" type="text" value={notes}/>
	</div>
	<div className="column small-12">
		<button type="submit">Save</button>
		<button className="button small" onClick={(e) => this.clickCancelEdit(e)}>Cancel</button>
	</div>
</form>

//Will be initialized by sending (or loading via redux) the prop intialValues in the form of:
	let initValues = {
		name: qvVar.name,
		description: qvVar.description,
		expression: qvVar.expression,
		notes: qvVar.notes
	};
```

# Custom Field Components

You can create a custom component to act as an "input" field. For example, if you want to have a <div> that when clicked would toggle between true and false, you can do this, you don't always need to use HTML input elements.

When creating a component that will not be using an HTML input element, you will need to know what prop represents the current state of this component and how you are going to change this state.

First, the value is located in props.input.value, assuming you named your incoming props as "props". Remember that when building the Field component:

	<div className="column small-4">
		<Field name="locked" component={this.renderLockedInput} type="" placeholder="Locked?"/>
	</div>

The name prop will tell redux form what intialValue or current state to pull from the redux store for the component you pass in the component prop.

The custom component will then use the props show what the state is and to give the user a way to change the state. For example, below is a customer component that uses a <div> to give the user a different experience than a check box. It allows them to simply click the <div></div>.

![](https://s3-us-west-2.amazonaws.com/notion-static/e7546f2b438b4bf0973bfc1056f60c4b/customfield.jpg)

```javascript
renderLockedInput (props) {
	const lockedValue = props.input.value
	return (
	<div>
	<label style={{height:"28px"}}>{props.placeholder}:</label>
	<div
	className={lockedValue ? "field-item locked" : "field-item"}
	onClick={ e => {
	props.input.onChange(!lockedValue);
	}}
	>
	{lockedValue ?
	<span>Locked <Icon type="lock" style={{float: "right"}}/></span> :
	<span>Unlocked <Icon type="unlock" style={{float: "right"}}/></span> }
	</div>
	</div>
	);
}
```