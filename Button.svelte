<script>
	import { createEventDispatcher } from 'svelte';

	export let text = 'Click';

	const dispatch = createEventDispatcher();
</script>

<button on:click={() => dispatch('buttonClicked', {})}>
	<span>{text}</span>
	<span>
		<svg
			width="6"
			height="10"
			viewBox="0 0 6 10"
			fill="none"
		>
			<path
				d="M1.33325 1.66663L4.66659 4.99996L1.33325 8.33329"
				stroke="white"
				stroke-width="1.5"
				stroke-linecap="round"
			/>
		</svg>
	</span>
</button>

<style lang="scss">
	@import './src/scss/config';

	button {
		@include fontFamily($spSansBold);
		color: #fff;
		height: 44px;
		background: $primary-base;
		border-radius: $spBorderRadius;
		display: flex;
		flex-direction: row;
		align-items: center;
		padding: 13px 13px 13px 17px;
		cursor: pointer;
		border: none;
		justify-content: space-between;
		span:nth-child(2) {
			padding-left: 13px;
		}
		&:hover {
			background: $primary-dark;
		}
		&:active {
			background: $primary-darker;
		}
		@media (prefers-color-scheme: dark) {
			background-color: #d03d12;
		}
	}
</style>
