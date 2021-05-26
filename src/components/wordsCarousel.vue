<template>
  <div class="q-pa-md">
    <q-carousel
      style="width:300px"
      arrows
      animated
      v-model="slide"
      height="200px"
      infinite
    >
      <!--gallery-->
      <q-carousel-slide :name="1" img-src="../sign_images/hello.jpg">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">שלום</div>
        </div>
      </q-carousel-slide>

      <q-carousel-slide :name="2" img-src="../sign_images/great.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">מצויין</div>
        </div>
      </q-carousel-slide>

      <q-carousel-slide :name="3" img-src="../sign_images/man.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">גבר</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="4" img-src="../sign_images/woman.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">אישה</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="5" img-src="../sign_images/today.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">היום</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="6" img-src="../sign_images/who.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">מי</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="7" img-src="../sign_images/wait.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">רגע</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="8" img-src="../sign_images/clear.png">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">ברור</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="9" img-src="../sign_images/sorry.jpg">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">סליחה</div>
        </div>
      </q-carousel-slide>
      <q-carousel-slide :name="10" img-src="../sign_images/forbidden.jpg">
        <div class="absolute-bottom custom-caption">
          <div class="text-h4">אסור</div>
        </div>
      </q-carousel-slide>

    </q-carousel> <!-- end slide-->
  </div> <!-- end class-->
</template>

<script>
export default {
  name: "wordsCarousel",
  data: function () {
    return {
      slide: 1,
    }
  }
}
</script>

<style scoped>

</style>
