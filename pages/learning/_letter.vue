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
          <video id="video" class="cam__feed" src="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// import * as handPoseDetection from '@tensorflow-models/hand-pose-detection'
// // Register WebGL backend.
// import '@tensorflow/tfjs-backend-webgl'

import { onMounted } from '@nuxtjs/composition-api'

onMounted(() => {
  // let hands
  // let detector
  const video = document.getElementById('video')
  // const model = handPoseDetection.SupportedModels.MediaPipeHands

  if (navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices.getUserMedia({ video: true })
      .then((stream) => {
        video.srcObject = stream
      }).catch((error) => {
        throw (error)
      })
  }

  // const detectorConfig = {
  //   runtime: 'tfjs'
  // }
  // handPoseDetection
  //   .createDetector(model, detectorConfig)
  //   .then((newDetector) => {
  //     detector = newDetector
  //   })

  // setInterval(async () => {
  //   hands = await detector.estimateHands(video, { flipHorizontal: false })
  //   if (hands.length > 0) {
  //     // console.log(hands)
  //   }
  // }, 1000)
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
