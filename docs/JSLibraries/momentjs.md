## Moment JS - Dates and Times

[Moment Docs](https://momentjs.com/docs/)

Basic Usage:

```javascript
import moment from 'moment';

//just calling moment creates a moment object that you can do stuff with
const now = moment();

//Format a moment object
const prettyDate = now.format('MMM Do, YYYY')

//If you want to store a date/time in the database, get the valueOf(), which is unix timestamp
const unixTimestamp = now.valueOf()

//Converting a unixTimestamp back to a moment object.
const aMoment = moment(unixTimeStamp)
```

[Formatting Date Options](https://momentjs.com/docs/#/displaying/)

## Compare Dates

Use the Query methods to compare dates.

[Moment Query Methods](https://momentjs.com/docs/#/query/)