auth.js

# Login app

This application use for administrator login in admin page to manage all datas on web.
We write this application by angular js that wrote in file auht.js.

This is code js

```cpp
Dandelion.controller('loginCtrl', function($scope, $rootScope, $location, $http, $cookieStore, Data) {
    if ($cookieStore.get('token')) {
        $location.path();
    }
    $scope.doLogin = function(data) {
        Data.post('login', {
            data: data
        }).then(function(results) {
            Data.toast(results);
            $cookieStore.put('token', results.token);
            $rootScope.wrapper = 'wrapper';
            if (results.status == "success") {
                $location.path('/');
            } else
            {
                $rootScope.wrapper = '';
            }
        });
    };
});
```
- $scope.doLogin: submit form login
```cpp
<form name="loginForm" class="form-horizontal" role="form">
 <input type="text" class="form-control" placeholder="Username" name="username" ng-model="login.username" required focus/>
 <input type="password" class="form-control" placeholder="Password" ng-model="login.password" required/>
  <button type="submit" class="width-35 pull-right btn btn-sm btn-primary" ng-click="doLogin(login)">
  <i class="ace-icon fa fa-key"></i>Login
  </button>
</form>
```
- Data.post: post angular js form
```cpp
api: login

  $data = json_decode(file_get_contents("php://input"));
  $username = $data->data->username;
  $password = $data->data->password;
  $admin = $this->facade->findByAttributes('admin', array('username' => $username, 'status' => 1));
  echo json_encode(array(
    'userID' => $admin->recordID,
    'fullName' => $admin->data->fullName,
    'email' => $admin->data->email,
    'username' => $admin->data->username,
    'token' => $this->f3->get('SESSION.token'),
    'status' => 'success'
                    ));    

```

#Logout

```cpp
$scope.logout = function() {
        Data.get('logout').then(function(results) {
            Data.toast(results);
            $cookieStore.remove('token');
            $rootScope.wrapper = '';
            $location.path('login');
        });
    }
```
$cookieStore.remove('token'): delete cookie token

#Register

```cpp
 $scope.signUp = function(data) {
        Data.post('register/' + $cookieStore.get('token'), {
            data: data
        }).then(function(results) {
            Data.toast(results);
            if (results.status == "success") {
                $location.path('/admin/' + $cookieStore.get('token'));
            }
        });
    };
```
Member Register

```cpp
<form name="signupForm" class="form-horizontal" role="form">
<div class="form-group">
<label class="col-sm-5 control-label no-padding-right" for="username">Username</label>
<div class="col-sm-7">
<span class="block input-icon input-icon-right">
<input type="text" class="form-control" placeholder="Username" name="username" ng-model="signup.username" focus/>
<span ng-show="signupForm.username.$dirty && signupForm.username.$invalid" class="help-inline">Username is not valid</span> 
</span>
</div>
</div>
                        .........
<div class="form-group">
<span class="lbl col-sm-5"> </span>
<div class="col-sm-7">
<button type="submit" class="width-35 pull-right btn btn-sm btn-primary" ng-click="signUp(signup)" data-ng-disabled="signupForm.$invalid">
 Sign Up
</button>
</div>
</div>
</fieldset>
</div>
</form>
```  

#Profile

```cpp
    Data.get('profile?token=' + $routeParams.token).then(function(results) {
        $scope.profile = results;
    });
```  
Get information of successfully register user.

```cpp
$scope.doProfile = function(data) {
        Data.put('update', {
            data: data
        }).then(function(results) {
            $scope.success = results;

        });
    };
```  
Update profile information


user.js

# Lists Users
```cpp
    Data.get('users?token=' + $routeParams.token).then(function(results) {
        var data = results;
        $scope.tableParams = new ngTableParams({
            page: 1, // show first page
            count: 10, // count per page
            sorting: {
                name: 'asc', // initial sorting
                email: 'asc', // initial sorting
            },
            filter: {
                name: '', // initial filter
                email: ''       // initial filter
            }
        }, {
            total: data.length, // length of data
            getData: function($defer, params) {
                // use build-in angular filter
                var orderedData = params.sorting ?
                        $filter('orderBy')(data, params.orderBy()) :
                        data;
                orderedData = params.filter ?
                        $filter('filter')(orderedData, params.filter()) :
                        orderedData;

                $scope.users = orderedData.slice((params.page() - 1) * params.count(), params.page() * params.count());

                params.total(orderedData.length); // set total for recalc pagination
                $defer.resolve($scope.users);
            }
        });
    });
    
```
Get all registered members, so display view html, trong đó có cơ chế tìm kiếm, sắp xếp, phân trang, các bạn vào trang http://bazalt-cms.com/ng-table/example/1/ để tìm hiểu thêm.

```cpp
    $scope.active = function(id) {
        Data.get('useractive?id=' + id + '&token=' + $cookieStore.get('token')).then(function(results) {
            $('.user_' + results.id).html(results.status);
        });
    }
```
Dùng để quản lý user, nếu status bằng true thì user đó sẽ hoạt động, ngược lại thì không.

# Detail user

```cpp
    Data.get('user?id=' + $routeParams.id + '&token=' + $routeParams.token).then(function(results) {
        $scope.user = results.user;
        var data = results.status;
        $scope.tableParams = new ngTableParams({
            page: 1, // show first page
            count: 10          // count per page
        }, {
            total: data.length, // length of data
            getData: function($defer, params) {
                $defer.resolve(data.slice((params.page() - 1) * params.count(), params.page() * params.count()));
            }
        });
        var com = results.comment;
        $scope.tableComment = new ngTableParams({
            page: 1, // show first page
            count: 10          // count per page
        }, {
            total: com.length, // length of data
            getData: function($defer, params) {
                $defer.resolve(com.slice((params.page() - 1) * params.count(), params.page() * params.count()));
            }
        });
    });
}
```
Hiện thị thông tin chi tiết của từng user, trong đó hiện thị profile, status, comment, photos...


group.js

#List Group

```cpp
 function getData() {
        Data.get('groups?token=' + $routeParams.token).then(function(response) {
            $scope.data = response;
            var data = response;
            $scope.tableParams = new ngTableParams({
                page: 1, // show first page
                count: 20, // count per page
                filter: {
                    name: '', // initial filter
                }
            }, {
                total: data.length, // length of data
                getData: function($defer, params) {
                    // use build-in angular filter
                    var orderedData = params.filter() ?
                            $filter('filter')(data, params.filter()) :
                            data;

                    $scope.group = orderedData.slice((params.page() - 1) * params.count(), params.page() * params.count());

                    params.total(orderedData.length); // set total for recalc pagination
                    $defer.resolve($scope.group);
                }
            });
        });
    }
```
Lấy tất cả các group của tất các thành viên, hiện thị ra view html, trong đó có cơ chế tìm kiếm, sắp xếp, phân trang, các bạn vào trang http://bazalt-cms.com/ng-table/example/1/ để tìm hiểu thêm.

post js

#Lists Posts

```cpp
  Data.get('posts?token=' + $routeParams.token).then(function(results) {
        var data = results;
        $scope.tableParams = new ngTableParams({
            page: 1, // show first page
            count: 20, // count per page
            filter: {
                content: '', // initial filter
            }
        }, {
            total: data.length, // length of data
            getData: function($defer, params) {
                // use build-in angular filter
                var orderedData = params.filter() ?
                        $filter('filter')(data, params.filter()) :
                        data;

                $scope.posts = orderedData.slice((params.page() - 1) * params.count(), params.page() * params.count());

                params.total(orderedData.length); // set total for recalc pagination
                $defer.resolve($scope.posts);
            }
        });
    });
```
Hiện thị danh sách posts của các thành viên ra view html, các bạn có thể tham khảo thêm tại http://bazalt-cms.com/ng-table/example/1/


#Lists Comments

```cp
function getData() {
        Data.get('comments?sort=' + $routeParams.sort + '&token=' + $routeParams.token).then(function(results) {
            var data = results;
            $scope.tableParams = new ngTableParams({
                page: 1, // show first page
                count: 20, // count per page
                filter: {
                    content: '', // initial filter
                }
            }, {
                total: data.length, // length of data
                getData: function($defer, params) {
                    // use build-in angular filter
                    var orderedData = params.filter() ?
                            $filter('filter')(data, params.filter()) :
                            data;

                    $scope.posts = orderedData.slice((params.page() - 1) * params.count(), params.page() * params.count());

                    params.total(orderedData.length); // set total for recalc pagination
                    $defer.resolve($scope.posts);
                }
            });
        });
    }

```
Hiện thị danh sách comment của các thành viên ra view html, các bạn có thể tham khảo thêm tại http://bazalt-cms.com/ng-table/example/1/

return view html 


```cpp
        <table ng-table="tableParams"  show-filter="true" template-pagination="pager" class="table table-bordered table-hover table-striped">
                            <tr ng-repeat="post in $data">
                                <td data-title="'Content'" filter="{ 'content': 'text' }">
                                    {{post.content}}
                                </td>
                                <td data-title="'Like'">
                                    {{post.nLike}}
                                </td>
                                <td data-title="'Comment'">
                                    {{post.nComment}}
                                </td>
                                <td data-title="'Owner Name'"  filter="{ 'owner': 'text' }">
                                    {{post.owner}}
                                </td>
<!--                                <td data-title="'Actor Name'">
                                    {{post.actor}}
                                </td>-->
                                <td data-title="'Active'"  filter="{ 'active': 'active'}">
                                    <a href="javascript:void(0)" class="post_{{post.id}}" ng-click="active(post.recordID)">{{post.active}}</a>
                                </td>
                            </tr>
                        </table>
```

plugin.js

#Plugin

Hiện thị danh sách plugin

```cp
    function getData() {
        Data.get('plugin').then(function(response) {
            $scope.data = response;
            var data = response;
            $scope.tableParams = new ngTableParams({
                page: 1, // show first page
                count: 20          // count per page
            }, {
                total: data.length, // length of data
                getData: function($defer, params) {
                    $defer.resolve(data.slice((params.page() - 1) * params.count(), params.page() * params.count()));
                }
            });
        });
    }
```

Cài đặt plugin

```cp
    $scope.install = function(data) {
        Data.get('installPlugin?id=' + data).then(function(results) {
            var data = results;
        });
    };
```

Remove plugin

```cp
  $scope.remove = function(item) {
        var modalInstance = $modal.open({
            templateUrl: 'partials/comfirm.html',
            controller: 'comfirmCtrl',
            resolve: {
                deleteItem: function() {
                    return item;
                }
            }
        });
        modalInstance.result.then(function() {
            reallyDelete(item);
        });
    };
    var reallyDelete = function(item) {
        Data.get('removePlugin?id=' + item + '&token=' + $cookieStore.get('token'), function(response) {
            if (response.success == 'true')
                $('.theme_' + response.id).remove();
        });

    };
```
Hiện thị chi tiết plugin trên modal
```cp
    $scope.detail = function(id) {
        var modalInstance = $modal.open({
            templateUrl: 'partials/themes/viewdetail.html',
            controller: 'detailPluginCtrl',
            resolve: {
                listItem: function() {
                    return Data.get('detailPlugin?id=' + id + '&token=' + $cookieStore.get('token'));
                }
            }
        });
    }
```


theme.js

#Theme

Hiện thị danh sách themes

```cp
    function getData() {
        Data.get('plugin').then(function(response) {
            $scope.data = response;
            var data = response;
            $scope.tableParams = new ngTableParams({
                page: 1, // show first page
                count: 20          // count per page
            }, {
                total: data.length, // length of data
                getData: function($defer, params) {
                    $defer.resolve(data.slice((params.page() - 1) * params.count(), params.page() * params.count()));
                }
            });
        });
    }
```

Cài đặt theme

```cp
    $scope.install = function(data) {
        Data.get('installPlugin?id=' + data).then(function(results) {
            var data = results;
        });
    };
```

Remove theme

```cp
  $scope.remove = function(item) {
        var modalInstance = $modal.open({
            templateUrl: 'partials/comfirm.html',
            controller: 'comfirmCtrl',
            resolve: {
                deleteItem: function() {
                    return item;
                }
            }
        });
        modalInstance.result.then(function() {
            reallyDelete(item);
        });
    };
    var reallyDelete = function(item) {
        Data.get('removePlugin?id=' + item + '&token=' + $cookieStore.get('token'), function(response) {
            if (response.success == 'true')
                $('.theme_' + response.id).remove();
        });

    };
```
Hiện thị chi tiết theme trên modal
```cp
    $scope.detail = function(id) {
        var modalInstance = $modal.open({
            templateUrl: 'partials/themes/viewdetail.html',
            controller: 'detailPluginCtrl',
            resolve: {
                listItem: function() {
                    return Data.get('detailPlugin?id=' + id + '&token=' + $cookieStore.get('token'));
                }
            }
        });
    }
```

upload.js

Upload plugin, theme

```cp
        function($scope, $http, $filter, $window, $cookieStore, Data) {
            $scope.options = {
                url: 'api/uploadTheme',
            };
            $scope.file = function() {
                var data = {
                    token: $cookieStore.get('token'),
                    name: $scope.queue.title,
                    file_name: $scope.queue.filename,
                    file_size: $scope.queue.filesize,
                    description: $scope.queue.description
                };
                Data.post('saveTheme', data).then(function(results) {
                    Data.toast(results);

                });
            };
        }
```
