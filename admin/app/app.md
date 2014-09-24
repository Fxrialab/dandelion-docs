# Config app

File app.js gồm có

Khai báo app
```cpp
var Dandelion = angular.module('dandelionAdminApp')
```

Config url cho app
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

Ví dụ 
```cpp
http://admin.dandelionet.org/#!/login
```
```cpp
        .run(function() {
});
```
Hàm run này nó thực hiện đầu tiên của app

Ví dụ
 ```cpp
        .run(function() {
        alert('Hello');
});
```
