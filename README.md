AgileNPM
============

<div id="top">
This package is built specifically for the Agile Consulting Application Boilerplate
The package includes static variables and methods that are commonly used within applications.
</div>

Installation
------------------

```bash
# Basic Node.JS installation
Meteor npm install agile-npm@https://github.com/informedecommerce/agile-npm.git
```

Via application package json:

```bash
# Additional typescript definitions
"agile-npm": "git+https://github.com/informedecommerce/agile-npm.git"
```

<ul>
   <li><a href="#states">Javascript object list of US States</a></li>
   <li><a href="#mimes">Javascript object list of file mime types</a></li>
   <li><a href="#dcolor">Dark Color Generator</a></li>
   <li><a href="#lcolor">Light Color Generator</a></li>
   <li><a href="#colorconv">Color Conversions</a></li>
   <li><a href="#blob">Base64 to blob for cordova file uploading images</a></li>
   <li><a href="#upload">Javascript object list of file mime types</a></li>
   <li><a href="#download">Javascript object list of file mime types</a></li>
</ul>

<div id="states">
   <h3>US States - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   US States are available in an array of json objects.
   
   ```json
[
   {
    abreviation: "AL",
    full: "Alabama"
}, {
    abreviation: "AK",
    full: "Alaska"
}, 
   ]
```
   
   </div>
   
   <div id="mimes">
   <h3>Mime Types - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   Mime Types are a json object with the file extension as the key
   
```json
{
    "323": "text/h323",
    "3g2": "video/3gpp2",
    "3gp": "video/3gpp",
   .... }
```
   </div>
   
   <div id="dcolor">
   <h3>Dark Color Generator - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   Returns a randomly generated color for use with light text
   
   ```js
var dark_color = Agile.darkColorGen();
```
   
   </div>
   
<div id="lcolor">
   <h3>Light Color Generator - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   Returns a randomly generated color for use with dark text
   
   ```js
var light_color = Agile.lightColorGen()
```
   
   </div>
   
   <div id="colorconv">
   <h3>Color Conversions - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   There are two options for converting colors. Hex to RGB or RGB to Hex 
   Pass strings
   Return Strings
   
   ```js
var rgb_color = Agile.hexToRGB('#ffffff')
var hex_color = Agile.rgbToHEX('255,255,255')
```
   
   </div>
   
   <div id="states">
   <h3>US States - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   US States are available in an array of json objects.
   
   ```json
[
   {
    abreviation: "AL",
    full: "Alabama"
}, {
    abreviation: "AK",
    full: "Alaska"
}, 
   ]
```
   
   </div>
   
   <div id="blob">
    <h3>Base64 to Blob - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   Base 64 to Blob conversion for handling base64 item conversion to a file object. 
   Examples in developer/cordova.vue
   
   Usage 
   
   ```js
Agile.b64toBlob(image_data, 'cordova_camera_base64.jpg', (file) => {
               console.log('Blob Returned')
               console.log(file)
            //Use Agile.upload here to upload returned file object
})
```
   
   </div>

<div id="upload">
    <h3>S3 Uploading - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
   The upload method uploads to AWS S3 and uses settings found in settings.json
   Examples found in the developer directory for webapp and cordova.
   
   Use
  ```js
Agile.upload(file, 'test/cordova/camera/', (data) => {
                  console.log(data)
                  
                  if(data.error){
                     this.$toast.show(data.message, 'Error', this.toast_opts.error)
                      this.process_running = false
                     this.upload_progress = 0
                     return false
                  }
                  
                  if(data.complete){
                     console.warn('S3 Upload Complete')
                     this.image = data.Location
                     this.process_running = false
                     this.upload_progress = 0
                        this.$toast.show('Base64 Image Uploaded to S3', 'Sucess', this.toast_opts.success)
                  }else{
                     console.log(data)
                     this.process_running = false
                     this.upload_progress = data.progress
                  }
                   
               })
```
</div>

<div id="download">
    <h3>Downloading - <small><a href="#top">Back to Top</a></small></h3>
   <hr>
This was specifically adapted for including cordova functionality and within a Meteor packaged application based on the Agile Consulting boilerplate.
   This is an Http downloader with local file handling for cordova applications



This version defaults to the browser based FileSaver.js methods if Meteor.isCordova returns false.

If cordova is detected it downloads via xHttp and uses the <a href="https://github.com/apache/cordova-plugin-file" target="_blank">cordova-plugin-file</a> plugin to store the file and the <a href="https://github.com/pwlin/cordova-plugin-file-opener2" target="_blank">cordova-plugin-file-opener2</a> plugin to open the downloaded file.

For more info on using cordova in the Agile Consulting Boilerplate see: <a href="https://github.com/informedecommerce/Meteor-MobileConfig" target="_blank">https://github.com/informedecommerce/Meteor-MobileConfig</a>

Current version:
Cordova usage has a callback handler to report download progress

```js
downloadTestMethod(url = null, file_name = null, storage_location = null, callback = null){
         Agile.saveAs(url, file_name, (response) =>{
            console.warn('Download callback')
            console.log(response)
            this.DL_PROGRESS = response.progress //set reactive var to display or use the progress
         })
        }
```

When a download is complete, the response will return the file attributes in an object
```js
   {
                                              file: fileEntry,
                                              progress: 100,
                                              file_name: file_name,
                                              url: url,
                                              storage_location: storage_location
                                           }
   response.file will contain downloaded file details.
```

FileSaver.js
============
<a href="https://github.com/eligrey/FileSaver.js">https://github.com/eligrey/FileSaver.js</a>
FileSaver.js is the solution to saving files on the client-side, and is perfect for
web apps that generates files on the client, However if the file is coming from the
server we recommend you to first try to use [Content-Disposition][8] attachment response header as it has more cross-browser compatiblity.

Looking for `canvas.toBlob()` for saving canvases? Check out
[canvas-toBlob.js][2] for a cross-browser implementation.

Supported Browsers
------------------

| Browser        | Constructs as | Filenames    | Max Blob Size | Dependencies |
| -------------- | ------------- | ------------ | ------------- | ------------ |
| Firefox 20+    | Blob          | Yes          | 800 MiB       | None         |
| Firefox < 20   | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |
| Chrome         | Blob          | Yes          | [2GB][3]      | None         |
| Chrome for Android | Blob      | Yes          | [RAM/5][3]    | None         |
| Edge           | Blob          | Yes          | ?             | None         |
| IE 10+         | Blob          | Yes          | 600 MiB       | None         |
| Opera 15+      | Blob          | Yes          | 500 MiB       | None         |
| Opera < 15     | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |
| Safari 6.1+*   | Blob          | No           | ?             | None         |
| Safari < 6     | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |
| Safari 10.1+   | Blob          | Yes          | n/a           | None         |

Feature detection is possible:

```js
try {
    var isAgileDownloaderSupported = !!new Blob;
} catch (e) {}
```

### IE < 10

It is possible to save text files in IE < 10 without Flash-based polyfills.
See [ChenWenBrian and koffsyrup's `saveTextAs()`](https://github.com/koffsyrup/AgileDownloader.js#examples) for more details.

### Safari 6.1+

Blobs may be opened instead of saved sometimes—you may have to direct your Safari users to manually
press <kbd>⌘</kbd>+<kbd>S</kbd> to save the file after it is opened. Using the `application/octet-stream` MIME type to force downloads [can cause issues in Safari](https://github.com/eligrey/AgileDownloader.js/issues/12#issuecomment-47247096).

### iOS

saveAs must be run within a user interaction event such as onTouchDown or onClick; setTimeout will prevent saveAs from triggering. Due to restrictions in iOS saveAs opens in a new window instead of downloading, if you want this fixed please [tell Apple how this WebKit bug is affecting you](https://bugs.webkit.org/show_bug.cgi?id=167341).

Syntax
------
### Import `saveAs()` from agile-downloader
```js
import { saveAs } from 'agile-downloader';
```

```js
AgileDownloader saveAs(Blob/File/Url, optional DOMString filename, optional Object { autoBom })
```

Pass `{ autoBom: true }` if you want AgileDownloader.js to automatically provide Unicode text encoding hints (see: [byte order mark](https://en.wikipedia.org/wiki/Byte_order_mark)). Note that this is only done if your blob type has `charset=utf-8` set.

Examples
--------

### Saving text using `require()`
```js
var AgileDownloader = require('agile-downloader');
var blob = new Blob(["Hello, world!"], {type: "text/plain;charset=utf-8"});
AgileDownloader.saveAs(blob, "hello world.txt");
```

### Saving text

```js
var blob = new Blob(["Hello, world!"], {type: "text/plain;charset=utf-8"});
AgileDownloader.saveAs(blob, "hello world.txt");
```

### Saving URLs

```js
AgileDownloader.saveAs("https://httpbin.org/image", "image.jpg");
```
Using URLs within the same origin will just use `a[download]`.
Otherwise, it will first check if it supports cors header with a synchronous head request.
If it does, it will download the data and save using blob URLs. 
If not, it will try to download it using `a[download]`.

The standard W3C File API [`Blob`][4] interface is not available in all browsers.
[Blob.js][5] is a cross-browser `Blob` implementation that solves this.

### Saving a canvas
```js
var canvas = document.getElementById("my-canvas");
canvas.toBlob(function(blob) {
    saveAs(blob, "pretty image.png");
});
```

Note: The standard HTML5 `canvas.toBlob()` method is not available in all browsers.
[canvas-toBlob.js][6] is a cross-browser `canvas.toBlob()` that polyfills this.

### Saving File

You can save a File constructor without specifying a filename. If the
file itself already contains a name, there is a hand full of ways to get a file
instance (from storage, file input, new constructor, clipboard event). 
If you still want to change the name, then you can change it in the 2nd argument.

```js
// Note: Ie and Edge don't support the new File constructor,
// so it's better to construct blobs and use saveAs(blob, filename)
var file = new File(["Hello, world!"], "hello world.txt", {type: "text/plain;charset=utf-8"});
AgileDownloader.saveAs(file);
```


</div>


