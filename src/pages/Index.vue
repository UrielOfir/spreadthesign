<template>

  <q-page class="flex flex-center">
    <div>
      <audio id="metzuian">
        <source src="../assets/audio/metzuian.mp3" type="audio/mpeg">
      </audio>
    </div>
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
    <div dir="rtl" class="row">
      <div>
        נא לבחור האם לזהות את המלים: שלום, היום, ברור, גבר מצטער בלבד.
      </div>
    </div>
    <div class="break"></div>
    <div class="row">
      <q-btn-toggle
        v-model="onlyFiveWords"
        class="my-custom-toggle"
        no-caps
        rounded
        unelevated
        toggle-color="primary"
        color="white"
        text-color="primary"
        :options="[
          {label: 'חמש מלים', value: true},
          {label: 'כל המלים', value: false}
        ]"
        @click="init()"
      />
    </div>

    <div class="break"></div>
    <div class="row">

      <q-btn type="button" class="btn" @click="init()">Start</q-btn>

      <q-btn type="button" class="btn" @click="stop()">Stop</q-btn>

      <q-btn type="button" class="btn" @click="webcam.play(); predictStop=false">play</q-btn>

    </div>
    <div class="break"></div>
    <div class="row">
      <q-card>
        <q-card-section id="webcam-container"></q-card-section>
        <div v-if="prediction">
          <q-card-section>
            {{ prediction.className }}
          </q-card-section>
        </div>
        <div v-else>
          <q-card-section>
            לא זוהתה מילה
          </q-card-section>
        </div>
      </q-card>
    </div>
    <div class="break"></div>
    <words-carousel/>
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
      canvas: "",
      webcam: new tmImage.Webcam(400, 400, true),
      labelContainer: "",
      maxPredictions: "",
      predictions: [],
      prediction: null,
      URL: "https://teachablemachine.withgoogle.com/models/DTVQSSp5L/",
      URL10: "https://teachablemachine.withgoogle.com/models/DTVQSSp5L/",
      URL5: "https://teachablemachine.withgoogle.com/models/USMunKY-N/",
      predictStop: false,
      soundPlaying: false,
      onlyFiveWords: false,
    }
  },

  methods: {
    async playAudio(audioName) {
      const audioObj = document.querySelector(`#${audioName}`)
      audioObj.play()
      this.soundPlaying = true;
      audioObj.onended = () => {
        this.soundPlaying = false;
        console.log("ended")
      }
    },
    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image
    // Load the image model and setup the webcam
    async init() {
      await this.webcam.setup();
      document.getElementById("webcam-container").appendChild(this.webcam.canvas);
      this.URL = this.onlyFiveWords ? this.URL5 : this.URL10;
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
      if (!this.predictStop && !this.soundPlaying) {
        await this.predict();
      }
      window.requestAnimationFrame(this.loop);
    },
// run the webcam image through the image model
    async predict() {
      // predict can take in an image, video or canvas html element
      this.predictions = await this.model.predict(this.webcam.canvas);
      this.prediction = this.predictions.find(prediction => prediction.probability > 0.8);
      this.playAudio('metzuian');
    },
    stop() {
      this.webcam.pause();
      this.predictStop = true;
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
  background-image: url("../images/new.jpg");
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
  /*font-family: 'Manrope', sans-serif;*/
  font-family: 'Courier New', monospace;
  font-weight: bold;
}

.btn {
  background-color: rgba(194, 232, 232, 0.51);
  color: black;
  margin: 10px;
}
</style>
