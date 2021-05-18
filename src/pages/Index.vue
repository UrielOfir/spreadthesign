<template>
  <q-page class="flex flex-center">
    <div class="row">
      <h3>זיהוי שפת הסימנים</h3>
    </div>
    <div class="break"></div>
    <div class="row">
      <q-btn type="button" @click="init()">Start</q-btn>
    </div>
    <div class="break"></div>
    <div>
      <div id="webcam-container"></div>
      <div v-for="prediction in predictions" v-bind:key="prediction">
        <q-card>
          {{ prediction.className }}: {{ prediction.probability.toFixed(2) }}
        </q-card>
      </div>
      <a href="https://sivanyesh.wixsite.com/spread-the-signs">Visit our site!</a>
    </div>
  </q-page>
</template>

<script>
import * as tf from '@tensorflow/tfjs';
import * as tmImage from '@teachablemachine/image';

export default {
  name: 'PageIndex',
  data: function () {
    return {
      model: "",
      webcam: "",
      labelContainer: "",
      maxPredictions: "",
      predictions: [],
      URL: "https://teachablemachine.withgoogle.com/models/Pfh6d3YOo/",
    }
  },
  methods: {
    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image
    // Load the image model and setup the webcam
    async init() {
      const modelURL = this.URL + "model.json";
      const metadataURL = this.URL + "metadata.json";
      // load the model and metadata
      // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
      // or files from your local hard drive
      // Note: the pose library adds "tmImage" object to your window (window.tmImage)
      this.model = await tmImage.load(modelURL, metadataURL);
      this.maxPredictions = this.model.getTotalClasses();
      // Convenience function to setup a webcam
      const flip = true; // whether to flip the webcam
      this.webcam = new tmImage.Webcam(200, 200, flip); // width, height, flip
      await this.webcam.setup(); // request access to the webcam
      await this.webcam.play();
      window.requestAnimationFrame(this.loop);
      // append elements to the DOM
      document.getElementById("webcam-container").appendChild(this.webcam.canvas);
    },
    async loop() {
      this.webcam.update(); // update the webcam frame
      await this.predict();
      window.requestAnimationFrame(this.loop);
    },
// run the webcam image through the image model
    async predict() {
      // predict can take in an image, video or canvas html element
      this.predictions = await this.model.predict(this.webcam.canvas);
      // for (let i = 0; i < this.maxPredictions; i++) {
      //   const classPrediction =
      //     prediction[i].className + ": " + prediction[i].probability.toFixed(2);
      //   this.labelContainer.childNodes[i].innerHTML = classPrediction;
      // }
    }
  },
  created() {
  }
}
</script>

<style>
.break {
  flex-basis: 100%;
  height: 0;
}
</style>
