<template>
	<div class="grid">
		<div class="grid__container">
			<div class="content">
				<div class="content__header">
					<h1 class="heading txt--light">Eager to learn?</h1>
					<p class="txt--light">
						Pick the letter you would like to learn and get going!
						<br />
						<span class="txt--yellow">
							*The letters "J" and "Z" are not included as their
							gestures are motion based
						</span>
					</p>
				</div>
				<div class="content__body">
					<Letters />
				</div>
			</div>
		</div>
	</div>
</template>

<script setup>
import Letters from '~/components/Letters.vue'
</script>

<style lang="scss">
.content {
	display: flex;
	flex-direction: column;
	justify-content: center;
	gap: 2em;
	height: 100vh;
	.content__header {
		h1 {
			margin-bottom: 0.25em;
			text-align: center;
			text-transform: none;
			font-family: $ff-heading;
			letter-spacing: 0.025em;
		}
		p {
			margin: auto;
			max-width: 75ch;
			text-align: center;
		}
	}
}
</style>
