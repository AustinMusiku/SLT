<template>
	<nav class="grid">
		<div class="grid__container">
			<nuxt-link
				to="/"
				class="logo icon"
			>
				<img
					src="/img/letter_A 1.png"
					alt="logo icon"
				/>
				<h1 class="subtitle txt--light">SLT</h1>
			</nuxt-link>
		</div>
	</nav>
</template>

<style lang="scss">
nav {
	position: fixed;
	padding: 2em 1em;
	left: 0;
	top: 0;
	width: 100%;
	z-index: 5;

	.logo {
		display: flex;
		flex-direction: row;
		gap: 0.5em;
		align-items: center;
	}
}
</style>
