

# The Movie DB API Reference

## Getting an API Key

Go to [TMDb Site](https://www.themoviedb.org/), login and then go to your account page and there will be an API section where you can access your API Key.

## Configuration Object

API Example call:

```html
https://api.themoviedb.org/3/configuration?api_key=<API_Key>
```

[Configuration JSON Output](https://www.dropbox.com/s/edaw2l5lkpksjtr/ConfigurationObject.json?dl=0)

### GET/configuration

**[TMDb Docs](https://developers.themoviedb.org/3/configuration/get-api-configuration)**

Get the system wide configuration information. Some elements of the API require some knowledge of this configuration data. The purpose of this is to try and keep the actual API responses as light as possible. It is recommended you cache this data within your application and check for updates every few days.

This method currently holds the data relevant to building image URLs as well as the change key map.

To build an image URL, you will need 3 pieces of data. The `base_url`, `size` and `file_path`. Simply combine them all and you will have a fully qualified URL. Here’s an example URL:

```
https://image.tmdb.org/t/p/w500/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg

```

The configuration method also contains the list of change keys which can be useful if you are building an app that consumes data from the change feed.

## TV API Calls

### Search By Title (TV)

[TMDb Docs](https://developers.themoviedb.org/3/search/search-tv-shows)

API Example Call:

```html
https://api.themoviedb.org/3/search/tv?api_key=<API_Key>&page=1&include_adult=false&query=stranger things
```

[Search By Name JSON Object](https://www.dropbox.com/s/q1jxqwd6m7734an/SearchByName_TV.json?dl=0)

### Get Show Detail (TV)

[TMDb Docs](https://developers.themoviedb.org/3/tv/get-tv-details)

API Example Call:

```html
https://api.themoviedb.org/3/tv/32815?api_key=<API_Key>
```

[Get Show Detail JSON Object](https://www.dropbox.com/s/m0kmcsbvpaksgme/GetShowDetail_TV.json?dl=0)

### Get Show Credits/Cast

[TMDb Docs](https://developers.themoviedb.org/3/tv/get-tv-credits)

API Example Call:

```html
https://api.themoviedb.org/3/tv/32815/credits?api_key=<API_Key>
```

[Get Show Credits JSON Object](https://www.dropbox.com/s/o9zhe9dk2qxk4gz/GetShowCredits_TV.json?dl=0)

### Get Images for a TV Show

[TMDb Docs](https://developers.themoviedb.org/3/tv/get-tv-images)

API Example Call:

```html
https://api.themoviedb.org/3/tv/32815/images?api_key=<API_Key>
```

[Get Images JSON Object](https://www.dropbox.com/s/h78io4k89i6y92s/GetImagesTV.json?dl=0)

Quick and dirty of what you get back from this call is:

```json
{
  "backdrops": [],
  "posters": [],
  "id": 12345
}
```

The **backdrops** and **posters** contain an array that looks like this:

```json
{
  "aspect_ratio": 0.6666666666666666,
  "file_path": "/x4bn54rjIFAxKLi6JLUgGwBy3W2.jpg",
  "height": 1500,
  "iso_639_1": "en",
  "vote_average": 5.312,
  "vote_count": 1,
  "width": 1000
}
```

However, I have found that you simply create a URL as detailed below using the *file_path* attribute.

All image attributes just contain the image files name `/zpefjkadfasdf.jpg`, to access the image, you must use information obtained from the configuration object to build the image URL.  You could simply hard code this information in, however, if the base URL ever changed, your app would break.  Probably a good idea to pull this information from the config object when loading the app.

To build an image URL, you will need 3 pieces of data. The `base_url`, `size` and `file_path`. Simply combine them all and you will have a fully qualified URL. Here’s an example URL:

```
https://image.tmdb.org/t/p/w500/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg

```

To get this information from the config object, you will need to pull the following items:

```json
{
  "images": {
    "base_url": "http://image.tmdb.org/t/p/",
    "secure_base_url": "https://image.tmdb.org/t/p/",
    "backdrop_size": [
      "w300".
      ...
    ],
    "logo_sizes": [],
    "poster_sizes": [
      "w92",
      ...
      "original"
    ]
  }
}
```

There are other attributes, however, the above should be enough to generate an image URL.

Not sure of the best way to get at the sizes, will most likely pick the size needed for the image and hardcode.

### Get TV Genre List

[TMDb Docs](https://developers.themoviedb.org/3/genres/get-tv-list)

API Example Call:

```html
https://api.themoviedb.org/3/genre/tv/list?api_key=<API_Key>
```

[Get Show Credits JSON Object](https://www.dropbox.com/s/zsxm6kqiiultayy/GetTVGenreList.json?dl=0)

### Get TV Season and Episode Detail

[TMDb Docs](https://developers.themoviedb.org/3/tv-episodes/get-tv-episode-details)

/tv/{tv_id}/season/{season_number}/[episode/{episode_number}]

There are a number of ways you can call this API to get information about a shows Seasons and Episodes.  All of the Get calls below require the TV Show ID.  This is obtained when you search for a TV Show by title.

Get the **details of a season**, with 32815 being the tv show id:

```html
https://api.themoviedb.org/3/tv/32815?api_key=<API_Key>
```

[Get Show Season JSON Object](https://www.dropbox.com/s/eoyadn2898mhj5b/GetTVSeasonDetail.json?dl=0)

Get the **details of a ALL episodes in a season**, with 32815 being the tv show id:

```html
https://api.themoviedb.org/3/tv/32815/season/1?api_key=<API_Key>
```

[Get Show Episode Detail JSON Object](https://www.dropbox.com/s/iqlbaxghznbd3dt/GetTVEpisodeDetail.json?dl=0)

### Get External ID (IMDB)

 /tv/{tv_id}/external_ids

```html
https://api.themoviedb.org/3/tv/32815/external_ids?api_key=<API_Key>
```

[Get External IDs JSON](https://www.dropbox.com/s/624oo9m79rgx8dd/GetExternalIds_TV.json?dl=0)

### Using IMDB External ID to get to Episode

Url - http://www.imdb.com/title/{imdb_id}/?ref_=ttep_ep1

Example: [http://www.imdb.com/title/tt1668798/?ref_=ttep_ep1](http://www.imdb.com/title/tt1668798/?ref_=ttep_ep1)

## Movie API Calls

### Search for a Movie by Title

[TMDB Docs](https://developers.themoviedb.org/3/search/search-movies)

**Query Parameters**

- page - returns the page specified.  If omitted only returns the first page.  In the return object you will also be handed a list of total pages.  You can use this information to help in navigating multiple pages.
  If you query a page that doesn't exist, your "results" array will be empty.
- query - contains the search text.
- year - an optional parameter that will narrow your searches for movies release is passed year.

API Example Call:

```html
https://api.themoviedb.org/3/search/movie?api_key=<API_Key>&page=1&include_adult=false&query=the house
```

[Search By Name Movie JSON Object](https://www.dropbox.com/s/if5amkjeoj6ub4n/SearchByName_Movie.json?dl=0)

### Get Images for a Movie (Movie)

[TMDB Docs](https://developers.themoviedb.org/3/movies/get-movie-images)

**Query Parameters**

- API Key

API Example Call:

The movie ID {345914} is returned in the movie objects when searching for a movie.

```html
https://api.themoviedb.org/3/movie/345914/images?api_key=<API_Key>
```

[Get Images for Movie JSON Object](https://www.dropbox.com/s/30spv0ejrk6z8du/GetImagesMovie.json?dl=0)

### Get Movie Credits (Movie)

[TMDB Docs](https://developers.themoviedb.org/3/movies/get-movie-credits)

API Example Call:

```html
https://api.themoviedb.org/3/movie/345914/credits?api_key=<API_Key>
```

[Movie Credits JSON Object](https://www.dropbox.com/s/ywqj4xum687igkw/GetMovieCredits.json?dl=0)

### Get (Find) Movies by Genre and More (Movie)

[TDMB Docs](https://developers.themoviedb.org/3/discover/movie-discover)

These API calls will allow you to search/discover movies by a number of different characteristics.  The docs will hold all of these details, but here are a few useful ones:

- with_genres - return movies that match genre id(s).  You can get the Genre list for movies as follows:

  ```html
  https://api.themoviedb.org/3/discover/movie?api_key=<API_Key>&sort_by=popularity.desc&include_adult=false&with_genres=16,28&page=1
  ```

- with_cast

[Get Find Movies JSON Object](https://www.dropbox.com/s/heefzsu7xg0yu4y/GetFindMoviesBy.json?dl=0)

# The OMDb API

This API is an unofficial pull of IMDB's database.  Currently thinking of using it to get the imdb links for the episodes.

Right now the API key I have is limited to 1000/day.

[Upgrade to more API Requests](https://www.patreon.com/bePatron?u=5038490)

Here is my **API Key**: c0247b61

Haven't found any documentation on the API, maybe once you become a patron.  Anyway, I'm looking to just get the IMDB ids for the episodes.  Here is the way to do that.

```html
http://www.omdbapi.com/?i=tt0944947&Season=1&apikey=c0247b61
```



# TV Tracker Plus Data Guide

Data will reside in a *redux* store in the application and initially in a *Firebase* database for permanent (backend) storage.  

There are three places where data is stored, 

- **firebase** - the main storage area for the data.  Code to Access in **firebase** folder.
- **redux** - the application state.
- **TMDb API** - the source for our TV and movie data. Code to access in **api** folder.

To facilitate communication between these different stores, we have the **dataModel** folder.  It houses the code that access the **TMDb API** and the **firebase** code.

For example, when I add a new TV Show, the *dmAddTVShowData* function will access the **TMDb API** code to get the source data, then format it to first send to **firebase** and call the appropriate firebase function and then return properly formatted data for the redux store.

The data structure itself needs some work, but currently it is the following:

```javascript
data = {
    TV: {
        showData: {},
        seasonData: {}, //episode data in this structure
        extraData: {}, //links, etc - probably should be in showData or userData or combo 
        userData: {} //User specific, shows watched, downloaded, custom poster image etc        
    },
    auth: {
        uid: '', //firebase uid
        status: '', //SUCCESS, WORKING, ERROR
        message: {
            code:
            message:
        },
    search: {
        searchTerm: '',
        status: '' //undefined, loading, success, error
	  }
    }
}

```



# Routes

- **/login** -> Login.js main login screen.  Starting point.
- **/** -> Main.js. Main screen with Header and Links to TV, API, Movie
- **/tv** -> TV starting point
- **/tv/add** - TV Add show endpoint.
- **/tv/detail/:id** - When a TV Show is chosen. The id will be passed to this route for display.
- **/api** -> API Starting point

