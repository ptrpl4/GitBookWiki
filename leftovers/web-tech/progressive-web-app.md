# PWA

A native app-like experience in web applications.

#### links

- https://habr.com/ru/articles/418923/
- https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps

## Under the hood

### Service worker

offline work and cache 

#### links

- about:debugging#/runtime/this-firefox - status page in ff
- chrome://serviceworker-internals/?devtools in chrome
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API - api
- https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API - notfications

#### Features

 Service Worker has access to
	 Cache Storage for web resources and 
	 IndexDB for data. 
	 Also can include business logic.

- **Network request management**: Service Workers can intercept and manage network requests made by a web application, allowing it to cache resources, handle errors, and provide offline support. 
- **Caching**: Service Workers can cache resources, such as images, scripts, and stylesheets, allowing the web application to load faster and reducing the amount of data that needs to be transferred over the network. (also application shell)
- **Offline support**: Service Workers can provide offline support by caching resources and allowing the web application to function even when the user is not connected to the internet.
- **Push notifications**: Service Workers can receive push notifications from a server, allowing the web application to send notifications to the user even when the application is not running.
- **Background synchronization**: Service Workers can perform background synchronization, allowing the web application to synchronize data with a server even when the application is not running.

#### Application shell

- https://developer.chrome.com/blog/app-shell/

A minimal HTML, CSS, and JavaScript powering a user interface.

Usually cached by the Service Worker on application first load, and then used to render the application on subsequent loads.

### Web App manifest

#### links

- https://developer.mozilla.org/en-US/docs/Web/Manifest

#### Info

JSON file defining application name, icon, how PWA will look like (fullscreen, standalone, etc.) and some other parameters.

Allows to ‘install’ PWA as a separate application on the smartphone home screen.

- **name**: The name of the application.
- **short_name**: A short name for the application, used in situations where the full name is too long.
- **description**: A brief description of the application.
- **icons**: An array of icons for the application, in different sizes and formats.
- **start_url**: The URL that the application should start at when launched.
- **display**: The display mode of the application, such as "fullscreen" or "standalone".
- **orientation**: The orientation of the application, such as "portrait" or "landscape".
- **theme_color**: The theme color of the application, used to customize the browser's UI.
- **background_color**: The background color of the application, used to customize the browser's UI.
