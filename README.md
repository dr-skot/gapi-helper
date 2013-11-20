gapi-helper
===========

Simplifies authorization and client-loading with the Google APIs Client Library for JavaScript.

Google's [getting started](https://developers.google.com/api-client-library/javascript/start/start-js) page presents sample code for authorizing and loading the API clients. `gapi-helper` offers a cleaner way to do it, leaving a smaller footprint in the namespace and abstracting away the implementation details so that all you have to do is provide configuration and add event listeners. You write this:

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

instead of this:

```javascript
<script>
var clientId = 'your client id';
var apiKey = 'your api key';
var scopes = 'https://www.googleapis.com/auth/plus.me';

function handleClientLoad() {
    gapi.client.setApiKey(apiKey);
    window.setTimeout(checkAuth,1);
}

function checkAuth() {
    gapi.auth.authorize({client_id: clientId, scope: scopes, immediate: true}, handleAuthResult);
}


function handleAuthResult(authResult) {
    if (authResult) {
        // do something when authorized
        makeApiCall();
    } else {
        // do something when authorization fails
    }
}

function makeApiCall() {
    gapi.client.load('plus', 'v1', function() {
        // do something when the client library is loaded
    });
}
</script>
<script src="https://apis.google.com/js/client.js?onload=handleClientLoad"></script>
```

Step 1: load libraries
======================

```
<script src="https://apis.google.com/js/client.js"></script>
<script src="gapi-helper.js"></script>
```

Note that you don't have to add an onload handler (`?onload=handleClientLoad`) like in Google's example. `gapi-helper` automagically detects when the script is loaded.

Step 2: configure
=================

```
gapi_helper.configure({
	clientId: 'your client id',
	apiKey: 'your api key',
	scopes: 'https://www.googleapis.com/auth/calendar', // and/or other services
	services: {
	   calendar: 'v3'
	}
});

```