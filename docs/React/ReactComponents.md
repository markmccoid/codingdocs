

## React Components

### react-dates from air bnb

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

