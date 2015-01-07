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
    $scope.showLoader = true;
    response = $http.post('/upload/image', formData, {
      headers: {'Content-Type': undefined },
    	transformRequest: angular.identity
    }).success(function(data) {
      if(data.success){
      	$scope.showLoader = false;
			} 
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
<p>We can add a loader image to our view template and show it using <span>ng-show</span> :</p>
```html
<div ng-controller="uploadsController">
  <img src="url/to/loaderImage" ng-show="showLoader" />	
  <input type="file" name="file" onchange="angular.element(this).scope().uploadImage(this.files)">
</div>
```

<h3>UploadsController.php</h3>
<p>Here I use <a href="http://image.intervention.io/" target="_blank">Intervention Image </a> for the upload process,
For more info about integration in Laravel click <a href="http://image.intervention.io/getting_started/installation#laravel" target="_blank">here..</a></p>
<p>'uploadImage' function in the backend inside our controller:</p>
```php
class UploadsController extends \BaseController {

	public function uploadImage(){
		$file = Input::file('file');
		if ($file!=null) {
	
			$ext = $file->getClientOriginalExtension();
			$image_name = str_random(15).'.'.$ext;
		}
	
		if (!file_exists(public_path().'/uploads/images')) {
			mkdir(public_path().'/uploads/images');
		}
		
		Image::make(Input::file('file'))->save(public_path().'/uploads/images/'.$image_name);
	
		return Response::json([ 'success' => true ]);
	}
}
```

<h3>routes.php</h3>
<p>last thing we need is to add our route:</p>
```php
Route::post('upload/image' , 'UploadsController@uploadImage');
```

The MIT License (MIT)

Copyright (c) ``` 2015 ``` Mohammed Kamal, <mohammed.k.meshkawi@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
