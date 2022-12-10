<template>
	<div
		v-if="isPopupOpen"
		class="popup"
	>
		<div class="popup__head">
			<button
				class="btn"
				@click="closePopup"
			>
				[x]
			</button>
		</div>
		<div class="popup__content">
			<div class="content__upper">
				<h1 class="subtitle txt--light">well done!</h1>
			</div>
			<div class="content__lower">
				<!-- <nuxt-link
					class="btn"
					to="/alphabet"
				> -->
				<nuxt-link
					to="/alphabet"
					class="link"
				>
					<svg viewBox="0 0 32 32">
						<g id="chevron-right">
							<line
								class="cls-1"
								x1="21"
								x2="12"
								y1="16"
								y2="25"
							/>
							<line
								class="cls-1"
								x1="12"
								x2="21"
								y1="7"
								y2="16"
							/>
						</g>
					</svg>
					back to alphabet
				</nuxt-link>
			</div>
		</div>
	</div>
</template>

<script setup>
import { ref, watch } from '@nuxtjs/composition-api'

// eslint-disable-next-line no-undef
const emits = defineEmits(['closePopup'])
// eslint-disable-next-line no-undef
const props = defineProps({
	showPopup: {
		type: Boolean
	}
})

const isPopupOpen = ref()
isPopupOpen.value = props.showPopup

const closePopup = () => {
	emits('closePopup')
	isPopupOpen.value = false
}

watch(
	() => props.showPopup,
	(newValue) => {
		isPopupOpen.value = newValue
	}
)
</script>

<style lang="scss">
.popup {
	position: absolute;
	display: flex;
	flex-direction: column;
	grid-gap: 2em;
	gap: 0em;
	padding: 1em 1em 1.5em;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
	width: 300px;
	border-radius: 5px;
	z-index: 500;
	background-color: $clr-blue;

	.popup__head {
		display: flex;
		align-self: self-end;

		.btn {
			background-color: $clr-blue;
			color: $clr-light;
			border: none;
			cursor: pointer;
		}
	}

	.popup__content {
		display: flex;
		flex-direction: column;
		gap: 0.5em;

		.content__upper,
		.content__lower {
			display: flex;
			align-self: center;
		}

		.content__upper {
			.subtitle {
				font-family: $ff-heading;
			}
		}
		.content__lower {
			.btn {
				display: flex;
				align-items: center;
				padding: 0.5em 1.25em 0.5em 0.75em;

				svg {
					height: 16px;
					width: 16px;
					transform: rotate(180deg);

					.cls-1 {
						fill: none;
						stroke: $clr-blue;
						stroke-linecap: round;
						stroke-linejoin: round;
						stroke-width: 1px;
					}
				}
			}

			.link {
				display: flex;
				align-items: center;
				color: $clr-light;
				text-decoration: underline;

				svg {
					height: 16px;
					width: 16px;
					transform: rotate(180deg);

					.cls-1 {
						fill: none;
						stroke: $clr-light;
						stroke-linecap: round;
						stroke-linejoin: round;
						stroke-width: 1px;
					}
				}
			}
		}
	}
}
</style>
