# Using localStorage

Local Storage is a browsers own data store. I have only used in chrome, assume it exists in other browsers.

Main thing to know is that Local Storage is stored in JSON format. So, if you have a JavaScript object, make sure to use JSON.stringify on it before saving to Local Storage and also when reading from Local Storage, be sure to use JSON.parse to put into JS Object.

Below is a sample of using the **localStorage.getItem() ** function

```javascript
function loadData() {
	var stringtvShows = localStorage.getItem('tvShows');
	var tvShows = [];

	try {
	if ( tvShows === null ) {
		return [];
	} else {
		tvShows = JSON.parse(stringtvShows);
	}
	} catch (e) {
		alert("error loading TVShows from localStorage: ", e);
	}
	return Array.isArray(tvShows) ? tvShows : [];
},
```

Below is a sample of saving data using **localStorage.setItem()** 
```javascript
function saveData (tvShows, showData) {

	if (Array.isArray(tvShows)) {
		localStorage.setItem('tvShows', JSON.stringify(tvShows));
		localStorage.setItem('showData', JSON.stringify(showData));
	}
}
```

A few other useful functions:

**localStorage.removeItem('itemName')** - removes the item from Local Storage.