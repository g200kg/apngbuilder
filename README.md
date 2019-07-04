APNGBuilder
===========

<img src="images/test.png" width="200"/>

APNGBuilder is a JS library for build APNG (Animation PNG) image from PNG-image / Canvas.

## Files
 apngbuilder.js --- JS library  
 testimg.html --- sample file (build from PNG image URL)  
 testblob.html --- sample file (build from PNG blob)  
 testcanvas.html --- sample file (build from Canvas)  

## How to Use

Load `apngbuilder.js` in the HTML file as follows:

```
<script src="apngbuilder.js"></script>
```  

APNGBuilder consists of one class, APNGBuilder.

|method           |  description                                                               |
|-----------------|----------------------------------------------------------------------------|
|new APNGBuilder()| Construct new APNGBuilder instance                                         |
|addFrame(src)    | Add a frame image. The `src` can be one of the following : <ul><li>a URL that represents a PNG image</li><li>Blob object of PNG image</li><li>Canvas object</li></ul> This method return Promise, then recommend to use async/await to call.  |
|setDelay(t)      | Set frame delay in sec. The default value is 0.1 (sec).                    |
|setNumPlays(n)   | Set max loop count of the animation. If this value is 0 (default), it loops infinitely.| 
|getAPng()        | Get the blob of APNG image data. The blob can be displayed via &lt; img &gt; tag and URL.createObjectURL(), or downloaded as local files via &lt; a &gt; tag.|

* Images to be added to a frame must be PNG format other than index color.
* addFile(url)'s url may be a DataURL or a BlobURL. Frames will be added as many times as called this method.   
* The size of the APNG image is adjusted to the largest size of the PNG images to be added.
* The delay time set by setDelay() is commonly used for all frames.


## Sample Code  

#### [LiveDemo (from image)](https://g200kg.github.io/apngbuilder/testimg.html)  
#### [LiveDemo (from blob)](https://g200kg.github.io/apngbuilder/testblob.html)
#### [LiveDemo (from canvas)](https://g200kg.github.io/apngbuilder/testcanvas.html)


```html
<html>
<head>
<script src="apngbuilder.js"></script>
<script>
async function main() {
  const apb = new APNGBuilder();    /* Create APNGBuilder instance */
  await apb.addFrame("images/1.png");  /* Add each frames */
  await apb.addFrame("images/2.png");
  await apb.addFrame("images/3.png");

  apb.setDelay(0.5);                /* Setup frame delay-time */
  apb.setNumPlays(0);               /* Setup number of loops. infinite if 0 */

  const blob = apb.getAPng();       /* Get the APNG blob */
  document.getElementById("result").src = URL.createObjectURL(blob);
}

function download(){
  let a = document.createElement("a");
  a.download = "test.png";
  a.href = document.getElementById("result").src;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
}

onload=main;
</script>
</head>
<body>
<img id="result"/>
<button onclick="download()">Download</button>
</body>
</html>
```

## License
  Released under the MIT license.