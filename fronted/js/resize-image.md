# resize a image before upload to server

```javascript
function handleImg(obj, img) {
  var fileReader = new FileReader();
  var oldFilName = img.name;

  fileReader.readAsDataURL(img);

  fileReader.onload = function (event) {
    var image = new Image();
    image.src = event.target.result;
    image.onload = function () {
      var newWidth = image.width;
      var newHeight = image.height;
      var fixedValue = 960;
      var fixedValue2 = 768;

      if (image.width > fixedValue && image.width > image.height) {
        newWidth = fixedValue;
        scaleFactor = newWidth / image.width;
        newHeight = image.height * scaleFactor;
      }

      if (image.height > fixedValue2 && image.height > image.width) {
        newHeight = fixedValue2;
        scaleFactor = newHeight / image.height;
        newWidth = image.width * scaleFactor;
      }

      var elem = document.createElement("canvas");
      elem.width = newWidth;
      elem.height = newHeight;
      elem.getContext("2d").drawImage(image, 0, 0, newWidth, newHeight);
      let promise1 = new Promise(function (resolve) {
        elem.toBlob(
          function (blob) {
            file = new File([blob], oldFilName + ".jpg", {
              type: "image/jpeg",
            });
            resolve(file);
          },
          "image/jpeg",
          0.8
        );
      });
      promise1.then(function (file) {
        const dataTransfer = new DataTransfer();
        dataTransfer.items.add(file);
        obj.files = dataTransfer.files;
      });
    };
  };
}

var media_file = $(".media_file");
media_file.on("change", function (event) {
  var img = event.target.files[0];
  handleImg(this, img);
});
```
