gapi-helper
===========

Simplifies authorization and client-loading with the Google APIs Client Library for JavaScript.

Google's [getting started](https://developers.google.com/api-client-library/javascript/start/start-js) page presents sample code for authorizing and loading the API clients. gapi-helper offers a cleaner way to do it, leaving a smaller footprint in the namespace and abstracting away the implementation details so that all you have to do is provide the configuration and add event listeners. You do this:

```javascript
    gapi_helper.configure({
      clientId: '65532434038.apps.googleusercontent.com',
      apiKey: 'AIzaSyBcqK9PMBGCEg1xsw4kghrVR4oUthaA_Do',
      scopes: 'https://www.googleapis.com/auth/plus.me', // and/or other services
      services: {
        plus: 'v1' // and/or other services
      }
    });

    gapi_helper.when('authorized', function () {
      var authorizeButton = document.getElementById('authorize-button');
      authorizeButton.style.visibility = 'hidden';
    });

    gapi_helper.when('authFailed', function () {
      var authorizeButton = document.getElementById('authorize-button');
      authorizeButton.style.visibility = '';
      authorizeButton.onclick = gapi_helper.requestAuth;
    });

    gapi_helper.when('plusLoaded', function () {
      var request = gapi.client.plus.people.get({
        'userId': 'me'
      });
      request.execute(function (resp) {
        var heading = document.createElement('h4');
        var image = document.createElement('img');
        image.src = resp.image.url;
        heading.appendChild(image);
        heading.appendChild(document.createTextNode(resp.displayName));
        document.getElementById('content').appendChild(heading);
      });
    });
```

instead of [this](https://code.google.com/p/google-api-javascript-client/source/browse/samples/authSample.html?r=ecbb774aee1a9edf53675f72a40391537c0d9b39).