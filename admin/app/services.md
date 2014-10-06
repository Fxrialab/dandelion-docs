#Service js

Get data from Api

```cpp
Dandelion.factory("Data", ['$http', 'toaster',
    function($http, toaster) { // This service connects to our REST API

        var serviceBase = '/api/';

        var obj = {};
        // get data to api
        obj.get = function(q) {
            return $http.get(serviceBase + q).then(function(results) {
                return results.data;
            });
        };
         // post data to api
        obj.post = function(q, object) {
            return $http.post(serviceBase + q, object).then(function(results) {
                return results.data;
            });
        };
         // update data to api
        obj.put = function(q, object) {
            return $http.put(serviceBase + q, object).then(function(results) {
                return results.data;
            });
        };
         // remove data from api
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
