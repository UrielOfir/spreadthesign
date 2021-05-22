<template>

  <q-page class="flex flex-center">

    <div class="row">
    <h4> Sign language identifier  </h4>
    </div>
    <div class="break"></div>
    <div class="row">
      <img src="../images/logo.png" style="width:150px">
    </div>
    <div class="break"></div>
    <div class="row">
      <q-btn color="black" type="button" @click="init()">Start</q-btn>

    </div>
    <div class="break"></div>
    <div class="row">
      <q-card>
        <q-card-section id="webcam-container"></q-card-section>
        <div v-for="prediction in predictions" v-bind:key="prediction.className">
          <q-card-section>
            {{ prediction.className }}: {{ prediction.probability.toFixed(2) }}
          </q-card-section>
        </div>
      </q-card>
    </div>
    <div class="break"></div>
    <div class="row">
      <div class="q-pa-md">
        <q-carousel
          style="width:300px"
          arrows
          animated
          v-model="slide"
          height="300px"
          infinite
        >
          <!--<q-carousel-slide :name="1" img-src="../sign_images/hello.jpg"  >
            <div class="absolute-bottom custom-caption">
              <div class="text-h4">שלום</div>
            </div>
          </q-carousel-slide> -->
          <q-carousel-slide :name="1" img-src="https://cdn.quasar.dev/img/parallax1.jpg">
            <div class="absolute-bottom custom-caption">
              <div class="text-h4">שלום</div>
            </div>
          </q-carousel-slide>

          <q-carousel-slide :name="2" img-src="../sign_images/hello.jpg">
            <div class="absolute-bottom custom-caption">
              <div class="text-h4">שלום</div>
            </div>
          </q-carousel-slide>

          <q-carousel-slide :name="3" img-src="../sign_images/great.png">
            <div class="absolute-bottom custom-caption">
              <div class="text-h4">שלום</div>
            </div>
          </q-carousel-slide>



        </q-carousel> <!-- end slide-->
      </div> <!-- end class-->
    </div> <!-- end row-->


    <div class="break"></div>
    <a href="https://sivanyesh.wixsite.com/spread-the-signs">Visit our site!</a> <!--link-->
  </q-page>

</template>
<!--js-->
<script>
import * as tf from '@tensorflow/tfjs';
import * as tmImage from '@teachablemachine/image';

export default {
  name: 'PageIndex',
  data: function () {
    return {
      slide: 1,
      model: "",
      webcam: new tmImage.Webcam(200, 200, true),
      labelContainer: "",
      maxPredictions: "",
      predictions: [{className: "", probability: 0}],
      URL: "https://teachablemachine.withgoogle.com/models/1h_dpunSf/",
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
      // request access to the webcam
      await this.webcam.play();
      window.requestAnimationFrame(this.loop);
      // append elements to the DOM

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
    }
  },
  async created() {
    await this.webcam.setup();
    document.getElementById("webcam-container").appendChild(this.webcam.canvas);
  }
}
</script>


<!--css-->
<style>
.break {
  flex-basis: 100%;
  height: 0;
}

.my-card {
  width: 100%;
  max-width: 250px;
}

body {
  background-image: url("../images/4426.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}

.custom-caption {
  text-align: center;
  padding: 12px;
  color: white;
  background-color: rgba(0, 0, 0, .3);
}
h4{
  /*font-style: italic;*/
  font-family: "Lucida Console", "Courier New", monospace;
}

</style>
