<template>
  <div class="grid">
    <div class="grid__container">
      <div class="two-parts">
        <div class="demo">
          <div class="demo__txt">
            <h1 class="super-heading txt--light">
              {{ $route.params.letter }}
            </h1>
          </div>
          <div class="demo__img">
            <img class="demo-img" src="/img/letterA.png" alt="">
          </div>
        </div>
        <div class="cam">
          <video id="video" class="cam__feed" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted } from '@nuxtjs/composition-api'

onMounted(() => {
  const video = document.getElementById('video')

  navigator.mediaDevices.getUserMedia({ video: true, audio: false })
    .then((stream) => {
      video.srcObject = stream
      video.play()
    }).catch((error) => {
      throw (error)
    })
})

</script>

<style lang="scss">
.two-parts{
  height: 100vh;
  width: 100vw;

  .demo,
  .cam{
    height: 100%;
  }

  .demo{
    padding-top: 10em;
    display: flex;
    flex-direction: column;
    gap: 4em;

    .demo__txt,
    .demo__img{
      margin: 0 auto 0;
      width: fit-content;
    }

    .demo__txt{
      text-align: center;

      h1{
        font-family: $ff-heading;
        text-transform: uppercase;
      }
    }
  }

  .cam{
    background-color: #000000;

    .cam__feed{
      width: 100%;
      height: 100%;
    }
  }
}
</style>
