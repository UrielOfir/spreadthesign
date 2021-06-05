<template>
<!--new commit-->
  <q-page class="flex flex-center">
    <div>
      <audio id="asur">
        <source src="../assets/audio/asur.mp3" type="audio/mpeg">
      </audio>
      <audio id="barur">
        <source src="../assets/audio/barur.mp3" type="audio/mpeg">
      </audio>
      <audio id="gever">
        <source src="../assets/audio/gever.mp3" type="audio/mpeg">
      </audio>
      <audio id="hayom">
        <source src="../assets/audio/hayom.mp3" type="audio/mpeg">
      </audio>
      <audio id="isha">
        <source src="../assets/audio/isha.mp3" type="audio/mpeg">
      </audio>
      <audio id="metzuian">
        <source src="../assets/audio/metzuian.mp3" type="audio/mpeg">
      </audio>
      <audio id="mi">
        <source src="../assets/audio/mi.mp3" type="audio/mpeg">
      </audio>
      <audio id="mitztaer">
        <source src="../assets/audio/mitztaer.mp3" type="audio/mpeg">
      </audio>
      <audio id="rega">
        <source src="../assets/audio/rega.mp3" type="audio/mpeg">
      </audio>
      <audio id="shalom">
        <source src="../assets/audio/shalom.mp3" type="audio/mpeg">
      </audio>
    </div>
    <div class="row">
      <h4> Sign language identifier </h4>
    </div>
    <div class="break"></div>
    <div class="row">
      <img src="../images/logo.png" style="width:150px">
    </div>
    <div class="break"></div>
    <div dir="rtl" class="row">
      <div>
        נא לבחור האם לזהות את המלים: שלום, היום, ברור, גבר מצטער/ת בלבד.

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

      <q-btn type="button" rounded class="btn" @click="init()">Start</q-btn>

      <q-btn type="button" rounded class="btn" v-bind:class="{active: active=='stop'}" @click="stop()">Stop</q-btn>

      <q-btn type="button" rounded class="btn" v-bind:class="{active: active=='play'}" @click="play()">play</q-btn>

    </div>
    <div class="break"></div>
    <div class="row">
      <q-card>
        <q-card-section id="webcam-container"></q-card-section>
        <div v-if="prediction">
          <div class="font_size">
            <q-card-section>
              {{ prediction.className }}
            </q-card-section>
          </div>
        </div>
        <div v-else>
          <div class="font_size2">
            <q-card-section>
              לא זוהתה מילה
            </q-card-section>
          </div>
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

import {mapState} from 'vuex';

export default {
  name: 'PageIndex',
  components: {wordsCarousel},
  data: function () {
    return {
      predictionsArr: [],
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
      onlyFiveWords: false,
      active: '',
      audioHashMap: {
        מצויין: "metzuian",
        שלום: "shalom",
        היום: "hayom",
        ברור: "barur",
        מי: "mi",
        מצטער: "mitztaer",
        גבר: "gever",
        אשה: "isha",
        רגע: "rega",
        אסור: "asur",
      }
    }
  },

  computed: mapState("data", ['audioDocument']),

  methods: {
    playAudio(audioName) {
      const audioObj = document.querySelector(`#${audioName}`)
      audioObj.play()
    //   audioObj.onended = () => {}
    },

    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image
    // Load the image model and setup the webcam
    async init() {
      debugger
      this.active = 'play';
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
      if (!this.predictStop) {
        await this.predict();
      }
      window.requestAnimationFrame(this.loop);
    },
// run the webcam image through the image model
    async predict() {
      // predict can take in an image, video or canvas html element
      this.predictions = await this.model.predict(this.webcam.canvas);
      this.prediction = await this.predictions.find(prediction => prediction.probability > 0.8);
      if (this.prediction) {
        this.predictionsArr.push(this.prediction.className)
      }
      if (this.predictionsArr.length > 20) {
        this.prediction.className = this.mode(this.predictionsArr);
        this.stop()
        this.playAudio(this.audioHashMap[this.prediction.className]);
        this.predictionsArr=[];
      }

    },
    stop() {
      this.webcam.pause();
      this.predictStop = true;
      this.active = 'stop';
    },
    play() {
      this.webcam.play();
      this.predictStop = false
      this.active = 'play';
    },
    mode(array) {
      if (array.length == 0)
        return null;
      let modeMap = {};
      let maxEl = array[0], maxCount = 1;
      for (let i = 0; i < array.length; i++) {
        let el = array[i];
        if (modeMap[el] == null)
          modeMap[el] = 1;
        else
          modeMap[el]++;
        if (modeMap[el] > maxCount) {
          maxEl = el;
          maxCount = modeMap[el];
        }
      }
      return maxEl;
    }
  },

}
</script>
