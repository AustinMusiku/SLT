<template>
	<div class="grid">
		<div class="grid__container">
			<div class="hero">
				<div class="hero__container">
					<h1 class="super-heading txt--light">
						kenyan sign language
					</h1>
					<p class="txt--light">
						Learn kenyan sign language today with our easy to use
						interface and interactive model
					</p>
					<nuxt-link
						class="btn"
						to="/alphabet"
					>
						Get Started
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
					</nuxt-link>
				</div>
			</div>
		</div>
	</div>
</template>

<style lang="scss">
.hero {
	display: grid;
	place-items: center;
	height: 100vh;
	width: 100vw;
	text-align: center;
	.hero__container {
		h1 {
			// margin-bottom: .25em;
			font-family: $ff-heading;
		}
		p {
			margin: 0.5em auto 1em;
			max-width: 40ch;
		}
		a {
			display: flex;
			flex-direction: row;
			gap: 0.25em;
			svg {
				height: 24px;
				width: 24px;
				.cls-1 {
					fill: none;
					stroke: $clr-blue;
					stroke-linecap: round;
					stroke-linejoin: round;
					stroke-width: 1px;
				}
			}
		}
	}
}
</style>
