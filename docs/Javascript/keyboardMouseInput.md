# Keyboard and Mouse Input

## Keyboard

### keydown vs keypress

First things first, react has it's own syntetic events **onKeyDown** and **onKeyPress** that map to *keydown* and *keypress* respectivly.

Also, the event object passed to the callback when one of these events is triggered has different properties if you are using React's event or the browsers event listeners.

The other distinction to make is that the **keypress** event is fired when a key is pressed down and that key normally produces a character value.  This means that the keypress will not give you keys like "escape", "alt", "ctrl", etc. 

However **keydown** is fired when a key is pressed down.  This is the guy to use if you need to get access to these no character producing keys.
 
### Browser Event Listener

In the example below, we are setting up an event listener to fire on keydown and then in the callback checking for the esc key.

```js
function handleKeyDown(event) {
	if ( event.keyCode === 27 ) {
		//Do something, the escape key was pressed
	}
}

document.addEventListener("keydown", handleKeyDown);
```

### React Synthetic Events
[React Docs on Events](https://facebook.github.io/react/docs/events.html)

