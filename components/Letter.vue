<template>
	<nuxt-link
		:to="`learning/${letter}`"
		class="heading letter"
	>
		{{ letter }}
	</nuxt-link>
</template>

<script setup>
// eslint-disable-next-line no-undef
defineProps({
	letter: {
		type: String,
		required: true
	}
})
</script>

<style lang="scss">
.letter {
	position: relative;
	padding: 0.5em;
	display: grid;
	place-items: center;
	height: 90px;
	line-height: 0.8;
	color: $clr-light;
	border: 1px solid #ffffff45;
	border-radius: 5px;
	transition: 0.25s;
	font-family: $ff-heading;

	&:hover,
	&:focus {
		color: $clr-blue;
		background-color: $clr-yellow;
	}

	&::after {
		counter-increment: letter;
		content: counter(letter);
		position: absolute;
		top: 0;
		left: 0;
		padding: 0.5em;
		font-size: 0.3em;
		font-family: $ff-body;
		color: $clr-light;
	}

	&:hover::after,
	&:focus::after {
		color: $clr-blue;
	}
}
</style>
