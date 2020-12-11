<template>
  <div class="container ">
    <div class="row ml-5 mr-5 ">
      <div class="col">
        <h1 class="display-4 text-center mb-4">Pic2Words</h1>
      </div>

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

    <div class="row mt-4">
      <div class="col-md-6 d-flex justify-content-center">
        <b-jumbotron class=" jumbo preview ">
          <p class="lead" v-if="previewImage == null">Image Preview</p>
          <img :src="previewImage" alt=""
        /></b-jumbotron>
      </div>
      <div class="col-md-6 d-flex justify-content-center">
        <b-jumbotron class="jumbo">
          <p
            class=" lead text-center"
            v-if="currentText == '' && showLoader == false"
          >
            Click Convert to Text
          </p>
          <p v-else>{{ currentText }}</p>
          <GridLoader v-show="showLoader" :color="'black'" />
        </b-jumbotron>
      </div>
      <div class="col">
        <button class="btn" @click="getText">Convert to Text</button>
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
      currentText: "",
      previewImage: null,
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
        console.log(this.previewImage);
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
  },
};
</script>

<style scoped>
h1 {
  font-family: "Dosis", sans-serif;
  color: #388087;
}
button {
  background-color: #6fb3b8;
  color: #fff;
}
.jumbo {
  height: 400px;
  width: 450px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #6fb3b8;
}

.jumbo img {
  max-width: 100%;
  max-height: 100%;
  height: auto;
}

.jumbo p {
  overflow: hidden;
}
</style>
