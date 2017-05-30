---
layout:     post
title:      "One ring to rule all data collection services"
subtitle:   "Handling data collection services in an elegant way for Angularjs"
date:       2017-05-30 23:27:00
author:     "Ja.Herrera"
header-img: "img/posts/post-12012017-backend-header.png"
comments: true
tags: [ Frontend, angularjs, javascript ]
---

Developers don't like to include data collection services because it's a boring work and also affect the performance of the application. So, when we have to add X service,in the best case we create a class *serviceX* to handle each call to this service and we fill all application code with calls to this new service. After we do the same with Y service.
 
A way to handle every service is using `Google Tag Manager` (GTM) which through "tags" you can send information (using datalayers) to other services (Google Analytics, Mixpanel, Flurry, etc) or embed javascript code in your application. I´m not going to explain how GTM works because there is a lot of information on Google even video tutorials, only I want to differentiate `pageViews` (each time we change the page) and `events` (for example when we click on a button).
 
## An approach to handle data collection services with Angularjs

### PageViews

You can think It´s easy to make the call at the beginning of each Controller but sometimes we need to wait for the response from the server to include some information in the datalayer so finally we will have a bunch of code with variables to check when we have all information to send to GTM. A easy solution is using callbacks in `App.js`:

```
$rootScope.$on("$routeChangeSuccess", function(event, next, current) {
	
    var path = $location.$$path;
    var callBackFunction = false;
    var currentData = {};
 
    if (path.indexOf('home') >= 0) {
          callBackFunction = 'callBackHome';
    }
	else if (path.indexOf('inventory') >= 0) {
          callBackFunction = 'callBackInventory';
    }
 
    serviceGTM.queuePageView(path, currentData, callBackFunction);
}
```

In our serviceGTM:

```
var queueEvents = [];
 
queuePageView: function(virtualPageURL, data, callBackFunction) {
 
	var dataLayerItem = {}
 
	//Fill dataLayerItem
 
	if(callBackFunction){
		queueEvents[callBackFunction];
	}
	else{
    //we send directly the dataLayer
		windows.dataLayer.push(dataLayerItem);
	}
}
 
 callBackInventory: function(additionalData) {
 
    if (this.queueEvents['callBackInventory']) {
          var dataLayerItem = this.queueEvents['callBackInventory'];
          
          // Add additionalData in dataLayerItem.
 
          window.dataLayer.push(dataLayerItem);
          delete this.queueEvents['callBackHotelLoaded'];
	}
 
 },
```

When we have the response from server, we call to *callBackInventory* with the additional data and we send the dataLayer.

### Events

Handle `events` is easier than pageViews, we only have to declare a couple of directives in `App.js` when we click on something or if something changes.

```
.directive('ngClick', function($rootScope, serviceGTM) {
    return {
      restrict: 'A',
      priority: 100,
      link: function(scope, element, attr) {
        element.bind('click', function($event) {
 
        	var eventObj = {};
        	if (this.attributes['data-i18n-attr']) {
        		//fill eventObj
        	}
        	else if (this.attributes['placeholder']) {
        		//fill eventObj
        	}
        	...
 
        	//call serviceGTM to send dataLayer
        	serviceGTM.sendGeneralEvent(eventObj);
        }
      }),
    }
 }
 
 .directive('ngChange', function($rootScope, serviceGTM) {
    return {
      restrict: 'A',
      priority: 100,
      link: function(scope, element, attr) {
        element.bind('click', function($event) {
 
        	//do the same
        }
      }),
    }
 }
```
