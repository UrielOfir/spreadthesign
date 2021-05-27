<template>

  <q-page class="flex flex-center">
    <div class="row">
<!--      <audio id="try">-->
<!--        <source src="../assets/audio/2021-05-25-20:24:31.mp3" type="audio/mpeg">-->
<!--      </audio>-->
      <h4> Sign language identifier </h4>
    </div>
    <div class="break"></div>
    <div class="row">
      <img src="../images/logo.png" style="width:150px">
    </div>
    <div class="break"></div>
    <div class="row">
      <q-btn color="black" type="button" @click="init()">Start</q-btn>
      <q-btn color="black" type="button" @click="stop()">Stop</q-btn>
      <q-btn color="black" type="button" @click="webcam.play()">play</q-btn>

    </div>
    <div class="break"></div>
    <div class="row">
      <q-card>
        <q-card-section id="webcam-container"></q-card-section>
        <div v-if="predictions.length > 0">
          <div v-for="prediction in predictions" v-bind:key="prediction.className">
            <q-card-section>
              {{ prediction.className }}
            </q-card-section>
          </div>
        </div>
        <div v-else>
          <q-card-section>
            לא זוהתה מילה
          </q-card-section>
        </div>
      </q-card>
    </div>
    <div class="break"></div>
    <div class="row">
      <div>המודל הנוכחי מזהה רק את המלים מצויין, ברור, היום</div>
     <words-carousel/>
    </div> <!-- end row-->


    <div class="break"></div>

    <div class="row"></div>
    <div class="break"></div>
    <i class="fas fa-at"></i>
    <div class="row">
      <a href="https://sivanyesh.wixsite.com/spread-the-signs">Visit our site!</a> <!--link-->
    </div>
    <div class="break"></div>
  </q-page>

</template>

<!--js-->
<script>

import * as tf from '@tensorflow/tfjs';
import * as tmImage from '@teachablemachine/image';
import wordsCarousel from "components/wordsCarousel";

export default {
  name: 'PageIndex',
  components: {wordsCarousel},
  data: function () {
    return {
      model: "",
      canvas:"",
      webcam: new tmImage.Webcam(200, 200, true),
      labelContainer: "",
      maxPredictions: "",
      predictions: [],
      URL: "https://teachablemachine.withgoogle.com/models/DTVQSSp5L/",
      predictStop: false,
    }
  },
  methods: {
    playAudio(audioName) {
      const audioObj = document.querySelector(`#${audioName}`)
      // audioObj.play()
      // this.predictStop = true;
      audioObj.onended = () => {
        this.predictStop = false;
        console.log("ended")
      }
    },
    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image
    // Load the image model and setup the webcam
    async init() {
      await this.webcam.setup();
      document.getElementById("webcam-container").appendChild(this.webcam.canvas);
      const modelURL = this.URL + "model.json";
      const metadataURL = this.URL + "metadata.json";
      // load the model and metadata
      this.model = await tmImage.load(modelURL, metadataURL);
      this.maxPredictions = this.model.getTotalClasses();
      await this.webcam.play();
      window.requestAnimationFrame(this.loop);
      // append elements to the DOM
    },
    async loop() {
      this.webcam.update(); // update the webcam frame
      if (!this.predictStop) {
        await this.predict();
      }
      window.requestAnimationFrame(this.loop);
    },
// run the webcam image through the image model
    async predict() {
      // predict can take in an image, video or canvas html element
      this.predictions = await this.model.predict(this.webcam.canvas);
      this.predictions = this.predictions.filter(prediction => prediction.probability > 0.8);
      this.playAudio('try');
    },
    stop(){
      this.webcam.pause();
      console.log(this.webcam)
    }
  },
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

h4 {
  font-family: 'Manrope', sans-serif;
}

</style>
