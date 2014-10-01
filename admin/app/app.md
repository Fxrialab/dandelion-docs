# Config app

File app.js include:

Report app
```cpp
var Dandelion = angular.module('dandelionAdminApp')
```

Config url for app
```cpp
Dandelion.config(['$routeProvider', '$locationProvider',
    function($routeProvider, $locationProvider) {
    }])
```
Learn more at:

```cpp
https://docs.angularjs.org/api/ngRoute/provider/$routeProvider
https://docs.angularjs.org/api/ng/provider/$locationProvider
```

Example: 

```cpp
Dandelion.config(['$routeProvider', '$locationProvider',
    function($routeProvider, $locationProvider) {
        $locationProvider.html5Mode(false).hashPrefix('!');
        $routeProvider.
                when('/login', {
            templateUrl: 'partials/login.html',
            controller: 'loginCtrl'
        })
                .when('/dashboard/:token', {
            templateUrl: 'partials/dashboard.html'
        })
                .when('/error', {
            templateUrl: 'partials/error.html'
        })
                .when('/', {
            templateUrl: 'partials/dashboard.html'
        })
                .otherwise({
            redirectTo: '/error'
        });
    }])
    

```
```cpp
        .run(function($rootScope, $location, $route, Data, $routeParams, $cookieStore) {

    $rootScope.$on("$routeChangeStart", function(event, next, current) {
        $rootScope.authenticated = false;
        $rootScope.wrapper = '';
//        $rootScope.$location = $location;
        $rootScope.isActive = function(url) {
            var active = (url === $location.path());
            return active;
        };
        var token = $cookieStore.get('token');
        if (token) {
            Data.get('session?token=' + token).then(function(results) {
                if (results.userID) {
                    $rootScope.authenticated = true;
                    $rootScope.userID = results.userID;
                    $rootScope.name = results.fullName;
                    $rootScope.email = results.email;
                    $rootScope.token = results.token;
                    $rootScope.wrapper = 'wrapper';
                } else {
                    var nextUrl = next.$$route.originalPath;
                    if (nextUrl == '/signup' || nextUrl == '/login') {
                    } else {
                        $location.path("/login");
                    }
                }
            });
        } else {
            $location.path("/login");
        }
    });
});
```
- $rootScope, $scope: this is contact between controller and view in angular
 Example:
```cpp
Controller
$rootScope.authenticated = true;
View
<div ng-if='authenticated' ng-include src="'partials/nav.html'"></div>
If authenticated is true so it will show div
```
- $location: get Link.
```cpp
  $location.path('/login')
  return
  http://admin.dandelionet.org/#!/login
```
- $cookieStore: save cookie when login
```cpp
  var token = 1233333355
  $cookieStore.put('token');
```
- $routeParams: Terms to get id
```cpp
.when('/dashboard/:token', {
            ...
  })
http://admin.dandelionet.org/#!/dashboard/1233333355
$routeParams.token =  1233333355      
```
- $cookieStore.put('token', results.token): Save mode cookie of angulajs when login

```cpp
Check the existence of token, if no token so it will be back login page.
    if ($cookieStore.get('token')) {
        $location.path();
    }
```

- Data: there is file data.js contain in forder servies, that connect to api with methods post, get, put, delete...
