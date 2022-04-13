# Axios-File-upload-with-laravel
Follow this short example
```bash
@extends('layout.app')
@section('title','Home')
@section('content')
  <div class="container">
      <div class="row d-flex justify-content-center">
         <div class="card text-center">
             <div class="card-header">
                 <h6>Laravel Ajax File Upload</h6>
             </div>
             <div class="card-body">
                 <input id="upField" class="form-control" type="file">
                 <button id="upbtn" class="btn btn-primary d-block mt-3" onclick="onUpload()">Upload</button>
             </div>
         </div>
      </div>
  </div>
@endsection
@section('script')
    <script type="text/javascript">
        function onUpload(){
            let spinner= '<div class="spinner-border text-warning" role="status"></div>';
            $('#upbtn').html(spinner);
            let file = document.getElementById('upField').files[0];
            let btn = document.getElementById('upbtn');
            let fileName = file.name;
            let fileSize = file.size;
            let fileExtension = fileName.split('.').pop();
            let myFile = new FormData();
            myFile.append('FileKey', file);
            let config = {headers:{'content_type': 'multipart/form_data'}};
            let url = '/fileUpload';
            axios.post(url, myFile, config)
            .then(function(response){
                if(response.status==200) {
                    $('#upbtn').html('Upload Success');
                    setTimeout(function (){
                        $('#upbtn').html('Upload');
                    }, 3000);
                }else {
                    $('#upbtn').html('Upload Failed');
                    setTimeout(function (){
                        $('#upbtn').html('Upload');
                    }, 3000);
                }
            }).catch(function (error){
                $('#upbtn').html('Upload Failed');
                setTimeout(function (){
                    $('#upbtn').html('Upload');
                }, 3000);
            })
        }
    </script>
 ```
    And the controller file will be like this
 ```bash
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class uploadController extends Controller
{
    function onFileUp(Request $request) {
        $result = $request->file('FileKey')->store('photos');
        return $result;
    }
}
```
## "%" Showing on uploaded file
Neede some changes on config var like the code of bellow
```bash
@extends('layout.app')
@section('title','Home')
@section('content')
  <div class="container">
      <div class="row d-flex justify-content-center">
         <div class="card text-center">
             <div class="card-header">
                 <h6>Laravel Ajax File Upload</h6>
             </div>
             <div class="card-body">
                 <input id="upField" class="form-control" type="file">
                 <button id="upbtn" class="btn btn-primary d-block mt-3" onclick="onUpload()">Upload</button>
                 <h3 id="Up-percent"></h3>
             </div>
         </div>
      </div>
  </div>
@endsection
@section('script')
    <script type="text/javascript">
        function onUpload(){
            let file = document.getElementById('upField').files[0];
            let btn = document.getElementById('upbtn');
            let fileName = file.name;
            let fileSize = file.size;
            let fileExtension = fileName.split('.').pop();
            let myFile = new FormData();
            myFile.append('FileKey', file);
            let config = {headers:{'content_type': 'multipart/form_data'},
                onUploadProgress:function (progressEvent) {
                   let upPercent = Math.round((progressEvent.loaded*100)/progressEvent.total)
                    $('#Up-percent').html(upPercent+"%")
                }
            };
            let url = '/fileUpload';
            axios.post(url, myFile, config)
            .then(function(response){
                if(response.status==200) {
                    $('#Up-percent').html('Upload Success');
                    setTimeout(function () {
                        $('#Up-percent').html(" ")
                    }, 2000)
                }

            }).catch(function (error){
                $('#Up-percent').html("Upload Failed")
                setTimeout(function () {
                    $('#Up-percent').html(" ")
                }, 2000)
            })
        }
    </script>
```
