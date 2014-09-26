auth.js

# Login app

Ứng dụng này dùng cho người quản lý đăng nhập vào admin để quản lý toàn bộ dữ liệu trên web.
Chúng tôi viết ứng dụng này bằng angular js, ứng dụng này được viết trong file auht.js.

Dưới đây la mã code js

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
$cookieStore.remove('token'): Xóa cookie token

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
Đăng ký thành viên

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
Lấy thông tin của user đã đăng nhập thành công

```cpp
$scope.doProfile = function(data) {
        Data.put('update', {
            data: data
        }).then(function(results) {
            $scope.success = results;

        });
    };
```  
Cập nhật thông tin profile


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
Lấy tất cả các thành viên đã đăng ký, hiện thị ra view html, trong đó có cơ chế tìm kiếm, sắp xếp, phân trang, các bạn vào trang http://bazalt-cms.com/ng-table/example/1/ để tìm hiểu thêm.

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



