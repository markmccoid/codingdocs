## Intro

[Sematic UI Site](http://react.semantic-ui.com/)

## Accordion Component
**[Official Docs](http://react.semantic-ui.com/modules/accordion)** 

It makes sense that to use the Accordion, you will be looping through data to build up each section. The issue that I had is in the structure of the component:

```javascript
<Accordion>
	<Accordion.Title>
	<Icon name='dropdown' />
	<strong>Title Text</strong>
	</Accordion.Title>;
	<Accordion.Content>
		<p>Accordion Content</p>
	</Accordion.Content>
</Accordion>
```

You can see that you have a title and content wrapped inside the outer Accordion component tags. The issue comes when you loop to get the title and contents. I found that you cannot wrap these in a div (to conform with JSX rules). This breaks the Accordion, so in my Map statement, I save each portion (Title and Content) to a variable and then return and array with both in them. This kept me from having to wrap the result in any outer component:

```javascript
var griddleComponents = sortedSeasons.map((season) => {
let aTitle = <Accordion.Title>
	<Icon name='dropdown' />
	<strong>Season {season.season}</strong>
	</Accordion.Title>;
let aContent =	<Accordion.Content>
									<Griddle results={season.episodeDetail}
									columnMetadata= {griddleColumnMetadata}
									showSettings={true}
									resultsPerPage={10}
									enableInfiniteScroll={true} useFixedHeader={true} bodyHeight={300}
									/>
						</Accordion.Content>;
	return ([aTitle,aContent]);
});
```

**Shorthand way to create an accordion that will get by the above issue:**

```javascript
			const panels = [{
				 title: 'What is a dog?',
				 content: 'Seriously, you need to get a new example',
	 	active: true
				 }, {
				 title: 'What kinds are there?',
				 content: 'Lots of better examples',
				}];
	
	<Accordion panels={panels} />
```

You can create an array of objects, with each object having a title and content entry, then pass it as the **panels ** property. the **active** property is optional but will open that panel when loaded.