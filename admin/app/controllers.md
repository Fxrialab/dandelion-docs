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
- $cookieStore.put('token', results.token): chế độ lưu cookie của angulajs khi đăng nhập

```cpp
Kiểm tra sự tồn tạ của token, nếu chưa có token thì nó sẽ quay lại trang login
    if ($cookieStore.get('token')) {
        $location.path();
    }
```
