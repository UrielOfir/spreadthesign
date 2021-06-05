<template>
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
</template>

<script>
import {mapMutations} from 'vuex'

export default {
  name: "audio",

  methods:{
    playAudio(audioName) {
      const audioObj = document.querySelector(`#${audioName}`)
      audioObj.play()
      this.stop();
      audioObj.onended = () => {}
    },
  }
}
</script>

<style scoped>

</style>
