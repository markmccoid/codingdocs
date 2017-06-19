# Naming Conventions

- **Array** – use plural nouns. e.g. items
- **Number** – use prefix num or postfix count, index etc that can imply a number. e.g. numItems, itemCount, itemIndex
- **Bool** – use prefix is, can, has
- **is**: for visual/behavior variations. e.g. isVisible, isEnable, isActive
- **can**: for behavior variations or conditional visual variations. e.g. canToggle, canExpand, canHaveCancelButton
- **has**: for toggling UI elements. e.g. hasCancelButton, hasHeader
- **Object** – use noun. e.g. item
- **Node** – use prefix node. containerNode
- **Element** – use prefix element. hoverElement

## Function Naming
If you are passing a function as a prop down to another component, use handleAction in the calling component and onAction in the called component:

```javascript
handleClick() {
	//...do something
}

const MyComponent = (props) => {
	return (
		<div>
			<CalledComponent onClick={handleClick} />
		</div>
		);
}
```