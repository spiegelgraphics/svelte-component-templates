<script>
	export let title = '';
	export let subtitle = '';
</script>

<div class="header">
	{#if title}
		<div class="title">{title}</div>
	{/if}
	{#if subtitle}
		<div class="subtitle">{subtitle}</div>
	{/if}
</div>

<style lang="scss">
	@import './src/scss/config';

	.header {
		line-height: 1.1;
		margin-bottom: 0.75rem;
	}

	.title {
		@include fontFamily('SpiegelSlabUICd-ExtraBold');
		font-size: 1.5rem;
		@media screen and (max-width: 488px) {
			font-size: 1.25rem;
		}
		padding-bottom: 0.0625rem;
	}

	.subtitle {
		font-size: 1rem;
	}
</style>
