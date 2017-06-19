# React Router

- [React-Router v4](#react-router-v4)
- [React-Router v2](#react-router-v2)

## React Router v4

<div><a name="react-router-v4"></a></div>

This new version of react router is totally different from previous versions.  
Everything is “component” based.  A bit hard to get your head around, but here goes.

## Imports and npm
React router is now broken up into either *react-router-dom* or *react-router-native*.  The former for browser based code and the latter for react native applications.

We will review the main components, which are:
- Router
- Route
- Switch
- Link
- Redirect

### \<Router\>
This is the top level component that you will wrap all of your other components in.
*Sidenote*
> If using redux, Router will go inside of redux’s Provider component.  Also, there is a weird thing where components using connect won’t rerender.  To make them rerender, you must wrap the connect call with react-router’s withRouter higher order function

```js	
class MyConnectedComponent extends React.Component{
		render() {
		....
	}
}
export default withRouter(connect()(MyConnectedComponent));
```

One way of using Router, would be to have an index.js file that will wrap you main app.js file.  app.js would have your top bar/navigation and then your initial routes.
You can also embed Routes in your ReacDOM render call, just make sure only one Child component is under the Router component.  This can easily be done by wrapping Routes in a \<div\>.

As I start working more with this, you can see where you can use routes deeper within the application to conditionally show components.

### \<Route path=‘/‘ component={} /\>

Route is the component that you will use to define your routes.  Here are the basic props:
- **path** - This is the path to match to.  Begins with ‘/‘
- **exact** - Boolean, just include if you want an exact match
- **component** or **render** - either pass a component or you can pass a anonymous function.

### Dynamic Routes
You can also make routes dynamic.  For example, if you wanted to have a path to an item include the item id or name as part of the path, you can use a colon in front of the dynamic part of the path:

```js
<Route path="/items/:itemId" ... />
```

The above path would then match if the url was /item/12334 or anything after the /item/.
When using dynamic routes, the component or render function gets passed a prop called match with a params object.  In this object you can access the defined dynamic route.  In the above case, it would be *match.params.itemId*
The best way to access or go to these dynamic routes is to use the Link component.
	<Link to=`/items/${itemId}` />

You could map through an array to create multiple links that would match with multiple pages with each page getting an itemId that could then be rendered.
Here is an example:

```js
	<Route 
	   path='/items/:itemId'
	   render={ ({match}) => {
	       const item = lookupAndGetItem(itemId); // or use find on an array
	       return (<Item item={item} />);
	      }}
	/>
```

# React-Router v2

<div><a name="react-router-v2"></a></div>
```
npm install —save react-router
```
Allows us to use the "route" in the URL to go to different sets of components.
For example, [http://myWebSite.com](#)(http://mywebsite.com) is the root route "/" and [http://myWebSite.com/firstRoute](#)(http://mywebsite.com/firstRoute) would be another route.
The main Route component will be set up in the base or index.js code.
![](https://www.dropbox.com/s/ltu5jkqlkk8ymq6/react-router1.jpg?raw=1)
In your base component (index.js) you will have something like this:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import { Router, browserHistory } from 'react-router';

import reducers from './reducers';
import routes from './routes';

const createStoreWithMiddleware = applyMiddleware()(createStore);
ReactDOM.render(
	<Provider store={createStoreWithMiddleware(reducers)}>
		<Router history={browserHistory} routes={routes}/>
	</Provider>
	, document.querySelector('.container'));
```
	
The above example also has redux, but you can see that the Router component has history and routes props. The history prop can take either "browserHistory", "hashHistory" and something else I can't remember.
The browser history means that it will be looking for routes immediately after the /. The hash history will be looking for routes after a /#.
You will also set up a routes.js files that will define all the routes.

```js
	import React from 'react';
	import { Route, IndexRoute } from 'react-router';
	
	import App from './components/app';
	
	const Greeting = () => {
		return <div> Hit there </div>;
	};
	
	export default (
	<Route path="/" component={App}>
		<Route path="greet" component={Greeting} />
		<Route path="greet2" component={Greeting} />
		<Route path="greet3" component={Greeting} />
	</Route>
	);
```

You will not ever have a component in your routes.js file, however it is here to show how you can have children under the main component.
The "/" path means that the App component will show as the root. Since the App ("/") route has children, the App will also show even when on the children routes.
One other very important item you will need if you have a route with child components/routes, is a reference to the this.props.children prop in the parent component/route. This means that in the App component we need something like this:
```js
import React, { Component } from 'react';

export default class App extends Component {
	render() {
		return (
			<div>
			Main App
			{this.props.children}
			</div>
		);
	}
}	
```

This means that when you are at the root, you will only see "Main App", but when you go to "/greet", you will see "Main App" and the child component.
Most of the time you will want to show your Main app component and another component when the root route is selected. To do this you will use the IndexRoute react-router component.

```js
import React from 'react';
import { Route, IndexRoute } from 'react-router';

import App from './components/app';
import PostsIndex from './components/posts_index';

export default (
	<Route path="/" component={App}>
		<IndexRoute component={PostsIndex} />
		<Route path="greet" component={Greeting} />
		<Route path="greet2" component={Greeting} />
		<Route path="greet3" component={Greeting} />
	</Route>
);
```

Note that when "/greet" is chosen, you will no longer see the IndexRoute component.

## Navigating To New Pages
When you want a user to be able to navigate to a new page via a link, you can use react routers **Link** component.

```js
import React from 'react';
import { connect } from 'react-redux';
import { Link } from 'react-router';

class PostsIndex extends React.Component {
	render() {
		return (
			<div>
				<div className="text-xs-right">
				<Link to="posts/new" className="btn btn-primary"> Add Post </Link>
			</div>
				list of blog posts
			</div>	
		);
	}
}
```

Many time you will want to programmatically go to a new route. This may be handled differently depending on the type of history you are using, but here are a few that I know of:

 **hashHistory.push('/todo');** 

Also, note, that if you are in a middleware function, you most likely will have a replace function as a parameter. This can be used to move to a new route. [See Middleware Page](#)(#RRM)

#### Using Context for Router Info
There are times when you will want to have Router information (like the push function) in a component that is far away from the Router component that encloses your app.  You can use React Context for this.  I'm not sure if it would be better to import something from react-router versus doing the following, but, whatever works.

```js
class test extends React.Component {
//Must first declare that we want to receive context from React
	static contextTypes = {
		router: React.PropTypes.object
	}

	componentWillMount() {
	//using the router push method when component mounts
		if (something) {
			this.context.router.push('/');
		}
	}

	componentWillUpdate(nextProps) {
		//Need to check after the initial mount, so that when
		//component gets rerendered we know if we need to do something.
		if (nextProps.something) {
			this.context.router.push('/');
		}
	}
}
```

## React-Router Middleware 
Middleware allows you to do something before react-router does something.
This example is for when you want to check if a user is logged in before taking them to a route:

```js
//React Router middleware below.
var requireLogin = (nextState, replace, next) => {
	//firebase.auth().currentUser -> null means not logged in, else returns current user
	if (!firebase.auth().currentUser) {
		replace('/');
	}
		next();
	};

// <TodoApp />,
ReactDOM.render(
	<Provider store={store}>
		<Router history={hashHistory}>
			<Route path="/" component={Main}>
			<IndexRoute component={AppLogin} />
			<Route path="todo" component={TodoApp} onEnter={requireLogin} />
			</Route>
		</Router>
	</Provider>,
	document.getElementById('app')
);
```

The **requireLogin ** function is the middleware. We can inject this into any of react-router's routes. Here we are using it on the TodoApp components route. 
 **onEnter=requireLogin ** is how we inject this middleware. Now anytime this route is called, first react-router will run the **requireLogin()** function.