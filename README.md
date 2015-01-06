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
