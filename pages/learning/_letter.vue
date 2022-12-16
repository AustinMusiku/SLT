<template>
	<div class="grid">
		<Loading
			:is-loading="isLoading"
			:loading-header="loadingHeader"
			:loading-text="loadingText"
		></Loading>
		<div class="grid__container">
			<div class="two-parts">
				<div class="demo">
					<div class="demo__txt">
						<h1 class="super-heading txt--light">
							{{ route.params.letter }}
						</h1>
					</div>
					<div class="demo__img">
						<img
							class="demo-img"
							:src="`/img/Sign_language_${route.params.letter}.svg`"
							:alt="route.params.letter"
						/>
						<img
							class="test-img"
							src="/img/test.jpg"
							alt="test image"
						/>
					</div>
				</div>

				<div class="cam">
					<Popup
						:show-popup="showPopup"
						@closePopup="closePopup"
					/>
					<video
						id="video"
						ref="videoElm"
						class="cam__feed"
						autoplay
					/>
					<canvas
						id="video-canvas"
						ref="videoCanvas"
						class="cam__feed"
					/>
					<canvas
						id="bounding-box-canvas"
						ref="boundingBoxCanvas"
						class="cam__feed"
					/>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup>
import '@tensorflow/tfjs-converter'
import '@tensorflow/tfjs-backend-webgl'
import { load } from '@tensorflow-models/handpose'
import * as tmImage from '@teachablemachine/image'
import {
	onBeforeUnmount,
	onMounted,
	reactive,
	ref,
	useRoute
} from '@nuxtjs/composition-api'

const route = useRoute()

const videoElm = ref(null)
const videoCanvas = ref(null)
const boundingBoxCanvas = ref(null)
const loopId = ref(null)
const model = ref(null)
const showPopup = ref(false)
const isLoading = ref(true)
const loadingHeader = ref('please wait')
const loadingText = ref('loading assets...')

const predictedHand = reactive({
	letter: '',
	probability: 0
})

const DETECTION_SIZE = 224

const MODEL_URL = '/model/model.json'
const METADATA_URL = '/model/metadata.json'

tmImage.load(MODEL_URL, METADATA_URL).then((value) => {
	model.value = value
})

function closePopup() {
	showPopup.value = false
}

onMounted(() => {
	const videoCanvasCtx = videoCanvas.value.getContext('2d')
	const boundingBoxCtx = boundingBoxCanvas.value.getContext('2d')

	const { clientWidth: camDivWidth, clientHeight: camDivHeight } =
		document.querySelector('.cam')
	const canvasAspectRatio = camDivWidth / camDivHeight

	videoCanvas.value.width = camDivWidth
	videoCanvas.value.height = camDivHeight

	boundingBoxCanvas.value.width = DETECTION_SIZE
	boundingBoxCanvas.value.height = DETECTION_SIZE

	let camWidth, camHeight, detector

	const loop = async () => {
		// mirror video on canvas
		videoCanvasCtx.drawImage(
			videoElm.value,
			(camWidth - camHeight * canvasAspectRatio) / 2,
			0,
			camHeight * canvasAspectRatio,
			camHeight,
			0,
			0,
			camDivWidth,
			camDivHeight
		)

		const hand = await detectHand(false)

		if (hand) {
			const { letter, probability } = await predictHand(false)
			predictedHand.letter = letter.toLowerCase()
			predictedHand.probability = probability

			if (predictedHand.letter === route.value.params.letter) {
				showPopup.value = true
			}
		}

		loopId.value = requestAnimationFrame(loop)
	}

	const detectHand = async (isTest) => {
		const srcElement = isTest
			? document.querySelector('.test-img')
			: videoCanvas.value

		const [hand] = await detector.estimateHands(srcElement)
		if (!hand) {
			return null
		}

		if (isTest) {
			return hand || null
		}

		// draw hand within bounding box to canvas
		const [topLeftX, topLeftY] = hand.boundingBox.topLeft
		const [bottomRightX, bottomRightY] = hand.boundingBox.bottomRight

		const boundingBoxWidth = bottomRightX - topLeftX
		const boundingBoxHeight = bottomRightY - topLeftY

		// draw rectangle around bounding box
		videoCanvasCtx.strokeStyle = '#e8c30d'
		videoCanvasCtx.lineWidth = 3
		// videoCanvasCtx.font = 'bold 48px serif'
		// write text
		// videoCanvasCtx.fillText('found!!!', 100, 100)
		videoCanvasCtx.strokeRect(
			topLeftX,
			topLeftY,
			boundingBoxWidth,
			boundingBoxHeight
		)

		// draw bounding box area on a canvas element
		boundingBoxCtx.clearRect(0, 0, boundingBoxWidth, boundingBoxHeight)
		boundingBoxCtx.drawImage(
			videoCanvas.value,
			topLeftX,
			topLeftY,
			boundingBoxWidth,
			boundingBoxHeight,
			0,
			0,
			DETECTION_SIZE,
			DETECTION_SIZE
		)

		return hand
	}

	const predictHand = async (isTest) => {
		const srcElement = isTest
			? document.querySelector('.test-img')
			: boundingBoxCtx.canvas

		const predictions = await model.value.predict(srcElement)
		const topPrediction = predictions.sort(
			(x, y) => parseFloat(y.probability) - parseFloat(x.probability)
		)[0]

		return {
			letter: topPrediction.className,
			probability: topPrediction.probability.toFixed(4)
		}
	}

	const init = async () => {
		detector = await load()
		if (detector) {
			// warm up model
			await detectHand(true)
			loadingHeader.value = 'Almost there'
			loadingText.value = 'Warming up models...'
			await predictHand(true)
		}

		const hasCamera = navigator.mediaDevices.getUserMedia({ video: true })
		if (hasCamera) {
			const stream = await navigator.mediaDevices.getUserMedia({
				video: true
			})
			videoElm.value.srcObject = stream
			const { width, height } = await stream.getTracks()[0].getSettings()
			camWidth = width
			camHeight = height
		}
		isLoading.value = false

		await loop()
	}

	window.addEventListener('load', () => {
		loadingText.value = 'Loading models...'
	})
	init()
})

onBeforeUnmount(() => {
	// clear hand detection loop
	cancelAnimationFrame(loopId.value)
	loopId.value = null

	// unmount camera
	const tracks = videoElm.value.srcObject.getTracks()
	for (const track of tracks) {
		track.stop()
	}
	videoElm.value.srcObject = null

	// remove event listener
	window.removeEventListener('load', () => {
		loadingText.value = 'Loading models...'
	})
})
</script>

<script>
export default {
	name: 'Letter'
}
</script>

<style lang="scss">
.two-parts {
	height: 100vh;
	width: 100vw;

	.demo,
	.cam {
		height: 100%;
	}

	.demo {
		padding-top: 10em;
		display: flex;
		flex-direction: column;
		gap: 4em;

		.demo__txt,
		.demo__img {
			margin: 0 auto 0;
			width: fit-content;
		}

		.demo__txt {
			text-align: center;

			h1 {
				font-family: $ff-heading;
				text-transform: uppercase;
			}
		}

		.demo__img {
			fill: #fff;

			.test-img {
				display: none;
			}
		}
	}

	.cam {
		position: relative;
		background-color: #000000;
		overflow: hidden;

		#video {
			display: none;
		}

		#bounding-box-canvas {
			display: none;
			width: 224px;
			height: 224px;
		}

		.cam__feed {
			position: relative;
			width: 100%;
			height: 100%;
		}
	}
}
</style>
