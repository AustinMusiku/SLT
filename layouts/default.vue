<template>
	<div class="wrapper">
		<Nav />
		<Nuxt />
	</div>
</template>

<script setup>
import Nav from '../components/Nav.vue'
</script>

<style lang="scss">
.wrapper {
	height: 100vh;
	background-color: $clr-blue;
	// background: linear-gradient(-55deg, $clr-yellow -50%, $clr-blue 50%);
}
</style>
