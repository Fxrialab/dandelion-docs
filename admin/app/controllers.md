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
