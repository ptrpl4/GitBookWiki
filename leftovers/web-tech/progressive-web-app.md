# PWA

provides a native app-like experience to users

#### links

- https://habr.com/ru/articles/418923/

## under the hood

### service worker

#### links

- about:debugging#/runtime/this-firefox - status page in ff
- chrome://serviceworker-internals/?devtools in chrome
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API 

#### features

 Service Worker has access to Cache Storage for web resources and IndexDB for data. But, most importantly, full freedom to implement business logic.

- **Network request management**: Service Workers can intercept and manage network requests made by a web application, allowing it to cache resources, handle errors, and provide offline support. 
- **Caching**: Service Workers can cache resources, such as images, scripts, and stylesheets, allowing the web application to load faster and reducing the amount of data that needs to be transferred over the network.
- **Offline support**: Service Workers can provide offline support by caching resources and allowing the web application to function even when the user is not connected to the internet.
- **Push notifications**: Service Workers can receive push notifications from a server, allowing the web application to send notifications to the user even when the application is not running.
- **Background synchronization**: Service Workers can perform background synchronization, allowing the web application to synchronize data with a server even when the application is not running.

### application shell

A minimal HTML, CSS, and JavaScript powering a user interface.

Usually cached by the Service Worker on application first load, and then used to render the application on subsequent loads.

#### links

- https://developer.chrome.com/blog/app-shell/

