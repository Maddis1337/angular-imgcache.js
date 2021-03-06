angular-imgcache.js
================

Simple [imgcache.js](https://github.com/chrisben/imgcache.js) wrapper for AngularJS, can be user with Ionic/Cordova/Phonegap.

## Installation

Install via bower

```sh
bower install --save git://github.com/Maddis1337/angular-imgcache.js
```

Link library and dependencies

```html
<script src="bower_components/angular/angular.js"></script>
<script src="bower_components/imgcache.js/js/imgcache.js"></script>
<script src="bower_components/angular-imgcache.js/angular-imgcache.js"></script>
```

Load module

```javascript
angular.module('MyApp', [
    'ImgCache'
])
```

## Usage

##### Configuration

You can override imgcache.js default options in Angulars config section.

```javascript
.config(function(..., ImgCacheProvider) {

    // set single options
    ImgCacheProvider.setOption('debug', true);
    ImgCacheProvider.setOption('usePersistentCache', true);

    // or more options at once
    ImgCacheProvider.setOptions({
        debug: true,
        usePersistentCache: true
    });

    // ImgCache library is initialized automatically,
    // but set this option if you are using platform like Ionic -
    // in this case we need init imgcache.js manually after device is ready
    ImgCacheProvider.manualInit = true;

    ...

});
```

If you are using platform like Ionic, you have to init ImgCache manually.
Note: you must set `ImgCacheProvider.manualInit = true;` as in example above.

```javascript
.run(function($ionicPlatform, ImgCache) {

    $ionicPlatform.ready(function() {
        ImgCache.$init();
    });

});
```

You can init library manually in other patforms via "deviceready" callback like this:

```html
<body onload="onLoad()">
```

```javascript
function onLoad() {
    document.addEventListener("deviceready", onDeviceReady, false);
}

function onDeviceReady() {
    ImgCache.$init();
}
```

(Thx @emps for this note in #1)

##### Service

Access imgcache.js and its original methods in your components via promise to make sure that imgcache.js library is already initialized

```javascript
.controller('MyCtrl', function($scope, ImgCache) {

    ImgCache.$promise.then(function() {
        ImgCache.cacheFile('...');
    });

});
```

##### Directive

Angular-imgcache.js comes with directive, which first looks into cache for an image. If not present, it downloads image, then stores in cache and uses it.

We can set src of an image with `ng-src` attribute.

**Note: This is a difference from [jBenes' original angular-imagecache.js](https://github.com/jBenes/angular-imgcache.js), because it allows regular images to load even when the FileAPI is blocked by the client**

```html
<img img-cache ng-src="{{imgUrl}}" />
```

Or set elements `background-image` with `ic-bg` attribute.

```html
<div img-cache ic-bg="{{imgUrl}}" ></div>
```
