#Service js

Lấy data từ api

```cpp
Dandelion.factory("Data", ['$http', 'toaster',
    function($http, toaster) { // This service connects to our REST API

        var serviceBase = '/api/';

        var obj = {};
        // get dữ liệu vào api
        obj.get = function(q) {
            return $http.get(serviceBase + q).then(function(results) {
                return results.data;
            });
        };
         // post dữ liệu vào api
        obj.post = function(q, object) {
            return $http.post(serviceBase + q, object).then(function(results) {
                return results.data;
            });
        };
         // câp nhật dữ liệu vào api
        obj.put = function(q, object) {
            return $http.put(serviceBase + q, object).then(function(results) {
                return results.data;
            });
        };
         // xó dữ liệu trong api
        obj.delete = function(q) {
            return $http.delete(serviceBase + q).then(function(results) {
                return results.data;
            });
        };
        // get url
        obj.url = function(q) {
            return $http.get(serviceBase + q);
        }

        return obj;
    }]);
```
