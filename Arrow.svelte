<script>
	export let upwards = false;
</script>

<div
	class="chevron"
	class:open={upwards}
>
	<svg
		viewBox="0 0 24 24"
		fill="none"
		style="baseProfile:tiny;stroke-linecap:round;stroke-linejoin:round;version:1"
	>
		<g class="chevron-down">
			<path
				d="M6 9l6 6 6 -6"
				stroke-width="1.5"
				stroke-linecap="round"
			></path>
		</g>
	</svg>
</div>

<style lang="scss">
	.chevron {
		width: 20px;
		height: 18px;
		overflow: hidden;
		margin-top: -3px;

		svg {
			width: 24px;
			height: 24px;
			stroke: #989694;

			.chevron-down {
				transition: transform 360ms ease-in-out;
				transform-origin: 50% 50%;
			}
		}
	}

	.chevron.open {
		svg {
			.chevron-down {
				transform: rotate(180deg);
				stroke: #000000;
				@media (prefers-color-scheme: dark) {
					stroke: #ffffff;
				}
			}
		}
	}
</style>
