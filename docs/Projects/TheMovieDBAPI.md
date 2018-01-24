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

##  Search By Title (TV)

[TMDb Docs](https://developers.themoviedb.org/3/search/search-tv-shows)

API Example Call:

```html
https://api.themoviedb.org/3/search/tv?api_key=<API_Key>&page=1&include_adult=false&query=stranger things
```

[Search By Name JSON Object](https://www.dropbox.com/s/q1jxqwd6m7734an/SearchByName_TV.json?dl=0)

## Get Show Detail (TV)

[TMDb Docs](https://developers.themoviedb.org/3/tv/get-tv-details)

API Example Call:

```html
https://api.themoviedb.org/3/tv/32815?api_key=<API_Key>
```

[Get Show Detail JSON Object](https://www.dropbox.com/s/m0kmcsbvpaksgme/GetShowDetail_TV.json?dl=0)

## Get Show Credits/Cast

[TMDb Docs](https://developers.themoviedb.org/3/tv/get-tv-credits)

API Example Call:

```html
https://api.themoviedb.org/3/tv/32815/credits?api_key=<API_Key>
```

[Get Show Credits JSON Object](https://www.dropbox.com/s/o9zhe9dk2qxk4gz/GetShowCredits_TV.json?dl=0)

