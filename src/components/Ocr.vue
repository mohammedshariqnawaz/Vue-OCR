<template>
  <div class="container ">
    <div class="row ml-5 mr-5 ">
      <div class="col-md-12">
        <h1 class="display-4 text-center ">OCR WebApp</h1>
      </div>
      <div class="col-md-12">
        <h5 class="text-left"><strong> Upload Image </strong></h5>
        <b-form-file
          v-model="currentImage"
          :state="Boolean(currentImage)"
          accept="image/*"
          placeholder="Choose a image  here..."
          drop-placeholder="Drop image here..."
          @change="uploadImage"
        ></b-form-file>
        <!-- <div class="mt-3">
      Selected file: {{ currentImage ? currentImage.name : "" }}
    </div> -->
      </div>
    </div>

    <div class="row mt-3">
      <div class="col-md-8 d-flex justify-content-center">
        <div class="leftcard">
          <!-- <p class="lead" v-if="previewImage == null">Image Preview</p> -->
          <img :src="previewImage" alt="" />
        </div>
      </div>
      <div class="col-md-4 ">
        <b-jumbotron class="jumbo">
          <p
            class=" lead text-center"
            v-if="currentText == '' && showLoader == false"
          >
            Click Convert to Text
          </p>
          <p
            class=" lead text-center"
            v-else
            ref="textToCopyFrom"
            @click="copyText"
          >
            {{ currentText }}
          </p>
          <GridLoader v-show="showLoader" :color="'#F53E54'" />
          <div class="info-msg">
            <i class="fa fa-info-circle"></i>
            Click on Text to Copy to Clipboard
          </div>
        </b-jumbotron>
      </div>

      <div class="col" style="margin-top:-20px;">
        <button class="btn" @click="getText">Convert Text</button>
      </div>
    </div>
  </div>
</template>

<script>
import { createWorker } from "tesseract.js";
export default {
  name: "Ocr",
  data: function() {
    return {
      currentImage: null,
      currentText: "Click on Convert Text",
      previewImage:
        "https://cdn.dribbble.com/users/652746/screenshots/1753453/animated_entypo_icons.gif",
      showLoader: false,
    };
  },
  methods: {
    uploadImage(e) {
      const image = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = (e) => {
        this.previewImage = e.target.result;
      };
    },
    getText() {
      const worker = createWorker({
        logger: (m) => console.log(m),
      });

      (async () => {
        this.showLoader = true;
        await worker.load();
        await worker.loadLanguage("eng");
        await worker.initialize("eng");
        const {
          data: { text },
        } = await worker.recognize(this.currentImage);
        this.currentText = this.currentText + text;
        await worker.terminate();
        this.showLoader = false;
      })();
      this.currentText = "";
    },
    copyText() {
      let para = this.$refs.textToCopyFrom;
      var range = document.createRange();
      range.selectNode(para);
      window.getSelection().removeAllRanges();
      window.getSelection().addRange(range);
      document.execCommand("copy");
      window.getSelection().removeAllRanges();
    },
  },
};
</script>

<style scoped>
@import url("//maxcdn.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css");
h1 {
  font-family: "Dosis", sans-serif;
  color: #f53e54;
}
.leftcard {
  border: 3px solid;
  width: 100%;
  height: 500px;
  border-radius: 6px;
}
.leftcard img {
  height: 400px;
  height: 100%;
  width: 100%;
  border-radius: 4px;
}
button {
  background-color: #f53e54;
  color: #fff;
}

.jumbo {
  position: relative;
  border: 3px solid;
  width: 100%;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  /* background-color: #F53E54; */
}

.jumbo p {
  height: 100%;
  overflow-y: auto;
}
.info-msg {
  width: 100%;
  color: #059;
  background-color: #bef;
  position: absolute;
  bottom: 0px;
  left: 0;
}
</style>
