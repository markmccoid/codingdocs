# React Components

## react-dates from air bnb

[react-dates](https://github.com/airbnb/react-dates)

Example usage of a SingleDatePicker and DateRangePicker

```javascript
import 'react-dates/initialize';
import { SingleDatePicker } from 'react-dates';
import 'react-dates/lib/css/_datepicker.css';

class aComponent extends React.Component {
  onDatesChange = ({startDate, endDate}) => {
    this.props.dispatch(setStartDate(startDate));
    this.props.dispatch(setEndDate(endDate));
  }
  onFocusChange = (calendarFocused) => {
    this.setState(() => ({ calendarFocused }))
  }
render() {  
    <SingleDatePicker
    date={this.state.createdAt}
    onDateChange={this.onDateChange}
    focused={this.state.calendarFocused}
    onFocusChange={this.onFocusChange}
    numberOfMonths={1}
    isOutsideRange={(day) => false}
    />

    <DateRangePicker 
    startDate={this.props.filters.startDate}
    endDate={this.props.filters.endDate}
    onDatesChange={this.onDatesChange}
    focusedInput={this.state.calendarFocused}
    onFocusChange={this.onFocusChange}
    numberOfMonths={1}
    isOutsideRange={() => false}
    showClearDates
    />
  }
}
```

## React Select

Drop down list box. 

To install:

```
$ yarn add react-select@next
```

Here is the format for a single select drop down box:

```javascript
import Select from 'react-select'; 

const Component = (props) => {
  const filterOptions = [{value: 'date', label:'Date'}, {value: 'amount', label: 'Amount'}];
  
  return (<Select options={filterOptions} 
	defaultValue={filterOptions[0]}
	styles={{control: styles => ({ ...styles, minWidth: '150px' })}}
	value={filterOptions.find((filter) => filter.value === this.props.filters.sortBy)}
	onChange={(e) => {
    	if (e.value === 'date') {
        	this.props.dispatch(sortByDate());
       	} else {
        	this.props.dispatch(sortByAmount());
        }
	  }
	}
  />)
}

```

First, you pass the options as an array of objects with a  `value` and `label` property.

If this is going to be a select list that always has a value, meaning you don't null it out, then you can set a **defaultValue**. But the default value must be in the form of an object with a  `value` and `label` property.

Same deal with the **value** property.  You must send an object with a `value` and `label` property.

### onChange

the onChange event gets an object with a `value` and `label` property.  In the example above, I'm checking what the value is and dispatching a redux action based on the selected value.

If the Select component is set to be clearable `isClearable` option set to true, then the a null is passed instead of an object.  So, if you have a clearable select list, you will need to check if the passed value, **e** in this case, you must check to see if it is null:

```javascript
<Select options={carFilterOptions} 
	placeholder="Car Filter..."
	isClearable={true}
	styles={{control: styles => ({ ...styles, minWidth: '150px' })}}
	value={carFilterOptions.find((filter) => filter.value === this.props.filters.carIdFilter)}
	onChange={(e) => {
          console.log(e)
          let carFilter = e ? e.value : ''
          this.props.dispatch(setCarFilter(carFilter));
	}}
/>
```

I used to put a blank in the select list to allow the list to be "cleared", but this is cleaner.