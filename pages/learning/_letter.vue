<template>
  <div class="grid">
    <div class="grid__container">
      <div class="two-parts">
        <div class="demo">
          <div class="demo__txt">
            <h1 class="super-heading txt--light">
              {{ route.params.letter }}
            </h1>
          </div>
          <div class="demo__img">
            <img class="demo-img" :src="`/img/Sign_language_${ route.params.letter }.svg`" :alt="route.params.letter">
          </div>
        </div>
        <div class="cam">
          <video id="video" ref="videoElm" class="cam__feed" autoplay />
          <canvas id="canvas" ref="canvasElm" class="cam__feed" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// eslint-disable-next-line no-unused-vars
import '@tensorflow/tfjs-converter'
import '@tensorflow/tfjs-backend-webgl'
import { onBeforeUnmount, onMounted, ref, useRoute } from '@nuxtjs/composition-api'

const route = useRoute()

const videoElm = ref(null)
const canvasElm = ref(null)
const loopId = ref(null)

onMounted(() => {
  const ctx = canvasElm.value.getContext('2d')
  const camDivWidth = document.querySelector('.cam').offsetWidth
  const camDivHeight = document.querySelector('.cam').offsetHeight
  canvasElm.value.width = camDivWidth
  canvasElm.value.height = camDivHeight
  const canvasAspectRatio = camDivWidth / camDivHeight
  let camWidth, camHeight

  if (navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices.getUserMedia({ video: true, audio: false })
      .then((stream) => {
        videoElm.value.srcObject = stream
        camWidth = stream.getTracks()[0].getSettings().width
        camHeight = stream.getTracks()[0].getSettings().height
      }).catch((error) => {
        throw (error)
      })
  }

  const loop = () => {
    // mirror video on canvas
    ctx.scale(-1, 1)
    ctx.drawImage(
      videoElm.value, // source video
      (camWidth - camHeight * canvasAspectRatio) / 2, // source x
      0, // source y
      camHeight * canvasAspectRatio, // source width
      camHeight, // source height
      0, // destination x
      0, // destination y
      camDivWidth, // destination width
      camDivHeight // destination height
    )

    loopId.value = requestAnimationFrame(loop)
  }

  loop()
})

onBeforeUnmount(() => {
  // clear hand detection loop
  cancelAnimationFrame(loopId.value)
  loopId.value = null

  // unmount camera
  const tracks = videoElm.value.srcObject.getTracks()

  for (let i = 0; i < tracks.length; i++) {
    const track = tracks[i]
    track.stop()
  }
  videoElm.value.srcObject = null

  // clear canvas
  const ctx = canvasElm.value.getContext('2d')
  ctx.clearRect(0, 0, canvasElm.value.width, canvasElm.value.height)
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

    .demo__img{
      fill: #fff
    }
  }

  .cam{
    position: relative;
    background-color: #000000;
    overflow: hidden;

    #video{
      display: none;
    }

    .cam__feed{
      position: relative;
      width: 100%;
      height: 100%;
    }
  }
}
</style>
