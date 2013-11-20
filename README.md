gapi-helper
===========

Simplifies authorization and client-loading with the Google APIs Client Library for JavaScript.

Google's [getting started](https://developers.google.com/api-client-library/javascript/start/start-js) page presents sample code for authorizing and loading the API clients. `gapi-helper` offers a cleaner way to do it, leaving a smaller footprint in the namespace and abstracting away the implementation details so that all you have to do is 1) configure and 2) add event listeners. Like this:

```javascript
<script src="https://apis.google.com/js/client.js"></script>
<script src="gapi-helper.js"></script>
<script>
gapi_helper.configure({
	clientId: 'your client id',
	apiKey: 'your api key',
	scopes: 'https://www.googleapis.com/auth/plus.me', // and/or other services
	services: {
	    plus: 'v1' // and/or other services
	}
});

gapi_helper.when('authorized', function () {
    // do something when authorized
});

gapi_helper.when('authFailed', function () {
    // do something when authorization fails
});

gapi_helper.when('plusLoaded', function () {
    // do something when the client library is loaded
});
</script>
```

For a full rewrite of Google's 
[authSample.html](https://code.google.com/p/google-api-javascript-client/source/browse/samples/authSample.html) see 
[authSample.html](https://github.com/dr-skot/gapi-helper/blob/master/authSample.html) 
in this repository.

Step 1: load libraries
======================

```
<script src="https://apis.google.com/js/client.js"></script>
<script src="gapi-helper.js"></script>
```

Note that you don't have to add an onload handler (eg `?onload=handleClientLoad`) to `client.js` like in Google's example. 
`gapi-helper` automagically detects when the script is loaded.

Step 2: configure
=================

```
gapi_helper.configure({
    clientId: 'your client id',
    apiKey: 'your api key',
    scopes: 'https://www.googleapis.com/auth/calendar', // and/or other services
    services: {
        calendar: 'v3' // and/or other services
    }
});
```

The `clientId` and `apiKey` you get by registering your app in the [Google Cloud Console](https://code.google.com/apis/console/).

When the client.js script has loaded and a configuration has been set (it doesn't matter which happens first),
`gapi_helper` will attempt to obtain authorization and load all the services requested in the configuration, firing events as it goes.

Step 3: listen for events
=========================

```
gapi_helper.when('scriptLoaded', function () {
    // do something when the client.js script is fully loaded
});
```

```
gapi_helper.when('authorized', function () {
    // do something when authorization succeeds
});
```

For example in [authSample.html](https://github.com/dr-skot/gapi-helper/blob/master/authSample.html), 
the authorize button is hidden when authorization succeeds.


```
gapi_helper.when('authFailed', function () {
    // do something when authorization fails
});
```

One thing you might want to do in this case is call `gapi_helper.requestAuth`, which will prompt the user to allow access.

```
gapi_helper.when('calendarLoaded', function () {
    // do something when the client library is loaded
});
```

Each service `x` requested in the configuration fires an `xLoaded` event when the service is loaded and ready to use.
