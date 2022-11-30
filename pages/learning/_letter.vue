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
          <canvas id="canvass" ref="canvasElmm" class="cam__feed" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { load } from '@tensorflow-models/handpose'
import * as tmImage from '@teachablemachine/image'
import { onBeforeUnmount, onMounted, ref, useRoute } from '@nuxtjs/composition-api'

const route = useRoute()

const videoElm = ref(null)
const canvasElm = ref(null)
const canvasElmm = ref(null)
const loopId = ref(null)
const model = ref(null)

const MODEL_URL = '/model/model.json'
const METADATA_URL = '/model/metadata.json'

tmImage.load(MODEL_URL, METADATA_URL)
  .then((value) => {
    model.value = value
  })

onMounted(() => {
  window.addEventListener('load', async () => {
    const ctx = canvasElm.value.getContext('2d')
    const boundingBoxCtx = canvasElmm.value.getContext('2d')

    const camDivWidth = document.querySelector('.cam').offsetWidth
    const camDivHeight = document.querySelector('.cam').offsetHeight
    const canvasAspectRatio = camDivWidth / camDivHeight

    canvasElm.value.width = camDivWidth
    canvasElm.value.height = camDivHeight

    canvasElmm.value.width = 448
    canvasElmm.value.height = 448

    let camWidth, camHeight
    const detector = await load()

    // setup camera
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

      detection()

      loopId.value = requestAnimationFrame(loop)
    }

    const detection = async () => {
      // run hand detection
      const hands = await detector.estimateHands(canvasElm.value)

      if (hands.length > 0) {
        // draw hand within bounding box to canvas
        const [topLeftX, topLeftY] = hands[0].boundingBox.topLeft
        const [bottomRightX, bottomRightY] = hands[0].boundingBox.bottomRight

        const boundingBoxWidth = bottomRightX - topLeftX
        const boundingBoxHeight = bottomRightY - topLeftY

        // draw rectangle around hand
        ctx.strokeStyle = '#e8c30d'
        ctx.lineWidth = 3
        ctx.strokeRect(
          topLeftX + 0, // x
          topLeftY + 0, // y
          bottomRightX - topLeftX - 0, // width
          bottomRightY - topLeftY - 0 // height
        )

        // expand bounding box by 20 pixels in all directions IF there is room
        // oder follows: left -> top -> right -> bottom
        // if (topLeftX - 5 > 0) { topLeftX -= 5 }
        // if (topLeftY - 5 > 0) { topLeftY -= 5 }
        // if (bottomRightX + 5 > camDivWidth) { bottomRightX += 5 }
        // if (bottomRightY + 5 > camDivHeight) { bottomRightY += 5 }

        // let IMGSIZE, destX, destY

        // // taller
        // if (boundingBoxCtxHeight > boundingBoxCtxWidth) {
        //   IMGSIZE = boundingBoxCtxHeight
        //   destX = (IMGSIZE - boundingBoxCtxWidth) / 2
        //   destY = 0
        // // eslint-disable-next-line brace-style
        // }
        // // wider
        // else if (boundingBoxCtxWidth > boundingBoxCtxHeight) {
        //   IMGSIZE = boundingBoxCtxWidth
        //   destX = 0
        //   destY = (IMGSIZE - boundingBoxCtxHeight) / 2
        // // eslint-disable-next-line brace-style
        // }
        // // equal
        // else {
        //   IMGSIZE = boundingBoxCtxHeight
        //   destX = destY = 0
        // }

        // draw bounding box on a canvas element

        boundingBoxCtx.clearRect(0, 0, boundingBoxWidth, boundingBoxHeight)
        boundingBoxCtx.drawImage(canvasElm.value,
          topLeftX, // source x
          topLeftY, // source y
          boundingBoxWidth, // source width
          boundingBoxHeight, // source height
          0, // destination x
          0, // destination  y
          448, // destination width
          448 // destination height
        )

        const predictions = await model.value.predict(boundingBoxCtx.canvas)
        const sortedPrediction = predictions.sort((x, y) => parseFloat(y.probability) - parseFloat(x.probability))
        // eslint-disable-next-line no-console
        console.log(sortedPrediction[0].className, sortedPrediction[0].probability.toFixed(4))
      } else {
        // eslint-disable-next-line no-console
        console.log('no hands')
      }
    }

    loop()
  })
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
    // position: relative;
    background-color: #000000;
    overflow: hidden;

    #video{
      display: none;
    }
    #canvas{
      // display: none;
    }
    #canvass{
      display: none;
      width: calc(448px * (768 / 768));
      height: 448px;
    }

    .cam__feed{
      position: relative;
      width: 100%;
      height: 100%;
    }
  }
}
</style>
