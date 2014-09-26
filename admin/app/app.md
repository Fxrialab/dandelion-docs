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
    }])
```
Tìm hiểu thêm tại

```cpp
https://docs.angularjs.org/api/ngRoute/provider/$routeProvider
https://docs.angularjs.org/api/ng/provider/$locationProvider
```

Ví dụ 

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
- $rootScope, $scope: là sự giao tiếp giữa controller và view trong angular
 Ví dụ
```cpp
Controller
$rootScope.authenticated = true;
View
<div ng-if='authenticated' ng-include src="'partials/nav.html'"></div>
Nếu sự kiện authenticated bằng true thì sẽ hiện thị div
```
- $location: get đường dẫn.
```cpp
  $location.path('/login')
  return
  http://admin.dandelionet.org/#!/login
```
- $cookieStore: lưu cookie khi đăng nhập
```cpp
  var token = 1233333355
  $cookieStore.put('token');
```
- $routeParams: lấy điều kiện get id
```cpp
.when('/dashboard/:token', {
            ...
  })
http://admin.dandelionet.org/#!/dashboard/1233333355
$routeParams.token =  1233333355      
```
- Data: có file data.js chứa trong forder servies, dùng để kết nối với api bằng phương thức post, get, put, delete...
