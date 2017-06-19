# axios HTTP Client - Make API Calls

 [https://github.com/mzabriskie/axios](https://github.com/mzabriskie/axios) 

I currently use this to make calls to site APIs that return JSON data.

I have been using the current format:

```javascript	
function getTVInfoById (tvShowID) {
	 var encodedtvShowID = encodeURIComponent(tvShowID);
	    var requestUrl = `${TVMAZESHOWSEARCH_URL}?q=${encodedtvShowID}`;
	
	    return axios.get(requestUrl).then(function(res){
	            if(res.data.cod && res.data.message) {
	                // console.log(res.data.message);
	                throw new Error(res.data.message);
	            } else {
	                // console.log (res.data);
	                //return res.data.main.temp;
	                return res.data;
	            }
	        }, function(res){
	            throw new Error(res.data.message);
	        });
	}
```

The above example is a return from a function. The function is going to return a promise.

**Note** that the if (res.data.cod &&...) stuff is specific to the return from the API. This API returns an error in the data response and thus we are checking for these to be populated and if they are throwing an error. If not, we return the data.

```javascript
axios.get() makes the request and waits for the return (thus the .then()...).
```

We return the promise the *axios.get()* returns, which we setup waiting for the reponse:

```javascript	
var showAPIData =   tvMaze.getTVInfoById(selectedShowId).then((theData) => {
		this.setState({
			showSelected: {
				showSelectedId: selectedShowId,
				tvMazeAPIData: theData
			}
		});
		}, function (e){
			console.log(e);
		}
	);
```
The variable **showAPIData** will have a promise stored in it as this is what the getTVInfoById is returning ( see above function ). Normally there is no need to store it in a variable, I'm not sure if there is a use, but currently I am not storing this. Just running it and getting the data I want back.

So, axios.get() will let you make a single call and get data back, but if you have multiple calls you want to make, you can use the axios.all() and axios.spread() functionality

```javascript
getTVInfoAndEpisodes: function (tvShowId) {
var encodedtvShowId = encodeURIComponent(tvShowId);
var requestUrl = `${TVMAZEIDSEARCH_URL}${encodedtvShowId}`;

return axios.all([
		axios.get(requestUrl),
		axios.get(`${requestUrl}/episodes`)
	])
	.then(axios.spread(function(showInfoResponse, episodeResponse){
		var showData = showInfoResponse.data;
		var episodeData = episodeResponse.data;
		var epObj;
		//Get array of unique seasons
		var seasons = _.uniq(episodeData.map((episode) => {
				return (episode.season);
		}));
		//Produce an Array of objects one for each season
		// [{
		//  season: 1,
		//  episodes: 12,
		//  episodeDetail: [{
		//      episodeNumber: 1,
		//      episodeName:'the name',
		//      episodeAirDate: '2009-03-18'
		//  },
		//  {...}]
		// },
		// {...}]
		var seasonArray = seasons.map((seasonNum) => {
				var seasonObj = {
						season: seasonNum,
						episodes: episodeData.filter((episode) => episode.season === seasonNum).length - 1,
						episodeDetail: episodeData.filter((episode) => episode.season === seasonNum)
								.map((episode) => {
												return ({
																episodeNumber: episode.number,
																episodeName: episode.name,
																episodeAirDate: episode.airdate
														});
										})
				};
				return seasonObj;
		});
	//Last step is to return the show data and the seasonData that we build above.
	return ({showData: showData,
		seasonData: seasonArray} );
	}));
}
```

To break this down, the first part is telling axios which "gets" you want to get:

```javascript
axios.all([
	axios.get(requestUrl),
	axios.get(`${requestUrl}/episodes`)
])
```

This allows you to run your API calls in parallel. However, the "then" function won't be run until data is back from both requests! I think this is a good thing. Takes a bit of the async pain away.

Now, once both requests finish and the "then" function is called, we will use the axios.spread() functionality to put each request's return data into separate parameters.

```javascript
	.then(axios.spread(function(showInfoResponse, episodeResponse){
	 ...
	}
```

This allows you to drop the return data into separate parameters. As far as I can tell, the order of the parameters should match the order of the .get call is the axios.all()'s array.

Note that the **JavaScript Promise** object has a .all method that behaves very similarly to the axios.all:

```javascript
const weather = new Promise((resolve) => {
	setTimeout(() => {
		resolve({ temp: 29, conditions: 'Sunny with Clouds'});
	}, 2000);
	});
	const tweets = new Promise((resolve) => {
		setTimeout(() => {
		resolve(['I like cake', 'BBQ is good too!']);
		}, 500);
		});
	Promise
		.all([weather, tweets])
		.then(responses => {
		const [weatherInfo, tweetInfo] = responses;
		console.log(weatherInfo, tweetInfo)
});
```