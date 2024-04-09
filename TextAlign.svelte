<script>
	export let type = 'iframe'; // alternative: embed
	export let disable = false;
</script>

<div
	class="text-align"
	class:is-iframe={type === 'iframe'}
	class:is-embed={type === 'embed'}
	class:is-disabled={disable}
>
	<slot />
</div>

<style lang="scss">
	.text-align {
		&:not(.is-disabled) {
			width: 100%;
			margin: 0 auto;

			&.is-iframe {
				max-width: calc(66.66666666666667% - 3rem);

				@media (max-width: 920px) {
					max-width: calc(83.33333333333334% - 3rem);
				}
				@media (max-width: 520px) {
					max-width: calc(100% - 2rem);
				}
			}

			&.is-embed {
				max-width: calc(100% - 2rem);

				@media (min-width: 720px) and (max-width: 1011px) {
					max-width: calc(83.33333333333334% - 3rem);
				}

				@media (min-width: 1012px) {
					max-width: calc(66.66666666666667% - 3rem);
				}
			}
		}

		&.is-disabled {
			display: contents;
		}
	}
</style>
