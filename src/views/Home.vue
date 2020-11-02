<template>
  <div class="home">
    <HelloWorld msg="User can change anything here" />
    Here should be the div for user to upload image
    <input type="file" @change="onFileChange" name="img" accept="image/*" />
    <div>
      <canvas id="myCanvas" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/Home.vue";

export default {
  name: "Home",
  components: {
    HelloWorld
  },
  data: () => {
    return {
      uploadedImage: null
    }
  },
  methods: {
    onFileChange(event){
      console.log('event',event.target.result,'sds', URL.createObjectURL(event.target.files[0]));
      // this.uploadedImage = URL.createObjectURL(event.target.files[0]);

      if (event.target.files && event.target.files[0]) {
        const FR = new FileReader();
        FR.addEventListener("load", this.onFileLoad)
        FR.readAsDataURL( event.target.files[0] );
      }
    },

    onFileLoad (e) {
        this.uploadedImage = e.target.result;
        // this.uploadedImage = e.target.result;

        //here are for the canvas worke
        const myCanvas = document.getElementById("myCanvas");
        const ctx = myCanvas.getContext("2d");
        const image = new Image();
        image.src = e.target.result;
        image.onload = function() {
          myCanvas.width = this.width;
          myCanvas.height = this.height;
          ctx.drawImage(image,0, 0 );
        };
    }
  }
};
</script>
