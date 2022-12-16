<template>
	<div
		v-if="isLoading"
		class="loading-wrapper"
	>
		<div class="loading">
			<div class="loading__upper">
				<h1 class="subtitle txt--light">{{ loadingHeader }}</h1>
				<p class="txt--light">{{ loadingText }}</p>
			</div>
			<div class="loading__lower">
				<LoadingSpinner />
			</div>
		</div>
	</div>
</template>

<script setup>
// eslint-disable-next-line no-undef
defineProps({
	isLoading: {
		type: Boolean
	},
	loadingHeader: {
		type: String,
		default: '...'
	},
	loadingText: {
		type: String,
		default: '...'
	}
})
</script>

<style lang="scss">
.loading-wrapper {
	position: fixed;
	display: grid;
	place-items: center;
	top: 0;
	left: 0;
	width: 100vw;
	height: 100vh;
	z-index: 100;
	background-color: rgba(3, 3, 4, 0.95);

	.loading {
		display: flex;
		flex-direction: column;
		gap: 1em;
		padding: 3em;
		width: 400px;
		border-radius: 5px;
		z-index: 500;
		background-color: $clr-blue;

		.loading__upper,
		.loading__lower {
			display: flex;
			align-self: center;
		}

		.loading__upper {
			flex-direction: column;
			gap: 1em;

			.subtitle {
				font-family: $ff-heading;
				letter-spacing: 1px;
			}
		}
	}
}
</style>
