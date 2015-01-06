Simple Ajax Image Upload using Laravel and Angularjs
===================
<p>This is an easy way to upload images through ajax request using Laravel and Angularjs</p>

How to Use:
===========
<h3>app.js</h3>
<p>first we will add angular code:</p>
```javascript
var app = angular.module('app', []);

app.controller('uploadsController',  function($scope, $http) {
  $scope.uploadImage = function(files) {
    var formData = new FormData();
    formData.append("file", files[0]);
    response = $http.post('/upload/image', formData, {
          headers: {'Content-Type': undefined },
          transformRequest: angular.identity
      });
    });
});
```
<h3>upload.blade.php</h3>
<p>Inside our view in laravel application:</p>
```html
<div ng-controller="uploadsController">
  <input type="file" name="file" onchange="angular.element(this).scope().uploadImage(this.files)">
</div>
```
