<template>
  <StackLayout>
    <FlexboxLayout justifyContent="center" flexDirection="column">
      <Label padding="10">
        {{ myImage }}
      </Label>
      <Image id="myImage" :src="myImage.src" stretch="aspectFit" />
      <Button class="btn" @tap="takePicture(options)" text="Take picture" />
    </FlexboxLayout>
  </StackLayout>
</template>

<script>
import { mapActions } from "vuex";
import * as Camera from "nativescript-camera";
import { Image } from "tns-core-modules/ui/image";
var ImageSource = require("tns-core-modules/image-source");
var fs = require("tns-core-modules/file-system");
var enumsModule = require("tns-core-modules/ui/enums");
import * as bghttp from "nativescript-background-http";

export default {
  data() {
    return {
      myImage: {
        src: 'https://placehold.it/150x150'
      },
      options: { width: 100, height: 100, keepAspectRatio: true } 
    } 
  },
  computed: {},
  mounted() {
    Camera.requestPermissions(); 
  },
  methods: {
    takePicture() {
      console.log('taking picture')
      let that = this
      Camera.takePicture({ width: 100, height: 100, keepAspectRatio: false, saveToGallery: false  })
        .then(function(picture) {
          console.log("Size: " + picture.options.width + "x" + picture.options.height);
          var img = picture;
          that.myImage.src = picture.android;

          var filepath = picture.android;
          console.log('path ' + filepath);
   
              console.log("Saving");
              var session = bghttp.session("image-upload");
              var imageName = that.extractImageName(filepath);
              console.log(imageName);
              var request = {
                  url: 'http://ptsv2.com/t/rr7f5-1528748153/post',
                  method: "POST",
                  headers: {
                      "Content-Type": "application/octet-stream",
                      "File-Name": imageName
                  },
                  description: "{ 'uploading': '" + imageName + "' }"
              };

              var task = session.uploadFile(filepath, request);
              //params = []
              //var task = session.multipartUpload(params, request);

              task.on("progress", logEvent);
              task.on("error", logEvent);
              task.on("complete", logEvent);
              function logEvent(e) {
                  console.log("currentBytes: " + e.currentBytes);
                  console.log("totalBytes: " + e.totalBytes);
                  console.log("eventName: " + e.eventName);
              }
      });
    },
    extractImageName(fileUri) {
        var pattern = /[^/]*$/;
        var imageName = fileUri.match(pattern);

        return imageName;
    }
  }
};
</script>
