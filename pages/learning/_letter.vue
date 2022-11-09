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
          <video id="video" ref="videoElm" class="cam__feed" autoplay />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// eslint-disable-next-line no-unused-vars
import * as tf from '@tensorflow/tfjs-core'
import '@tensorflow/tfjs-converter'
import '@tensorflow/tfjs-backend-webgl' // Register WebGL backend.
import * as handPoseDetection from '@tensorflow-models/handpose'

import { onBeforeMount, ref } from '@nuxtjs/composition-api'

// const model = handPoseDetection.SupportedModels.MediaPipeHands
const videoElm = ref(null)

onBeforeMount(async () => {
  if (navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices.getUserMedia({ video: true, audio: false })
      .then((stream) => {
        videoElm.value.srcObject = stream
      }).catch((error) => {
        throw (error)
      })
  }

  const detector = await handPoseDetection.load()
  // eslint-disable-next-line no-console
  console.log('ready---------------------------------')

  setInterval(async () => {
    // eslint-disable-next-line prefer-const
    let hands = await detector.estimateHands(videoElm.value, { flipHorizontal: true })
    if (hands.length > 0) {
      // eslint-disable-next-line no-console
      console.log(hands[0])
    }
  }, 2000)
})

</script>

<script>
export default {
  name: 'Letter'
}
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
