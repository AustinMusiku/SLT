<template>
	<div class="loading-spinner"></div>
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
.loading-spinner {
	position: relative;
	width: 50px;
	height: 50px;
	border-radius: 50%;
	border: 2px solid $clr-blue;
	border-top-color: $clr-light;
	animation: spin 2s linear infinite;

	&::after {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		border-radius: 50%;
		border: 2px solid $clr-blue;
		border-top-color: $clr-light;
		animation: spin 2s linear infinite;
	}

	@keyframes spin {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}
}
</style>
