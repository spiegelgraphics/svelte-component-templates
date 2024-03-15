<script>
	export let href;
</script>

{#if href}
	<a href={href}>
		<slot />
	</a>
{:else}
	<slot />
{/if}

<style lang="scss">
	a {
		--int-a-color: #e64415;
		--int-a-border-color: #e64415;
		--int-a-hover-border-color: #e64415;
		@media (prefers-color-scheme: dark) {
			--int-a-color: #fff;
			--int-a-border-color: #bbb9b7;
			--int-a-hover-border-color: #fff;
		}

		text-decoration: none;
		color: var(--int-a-color);
		border-bottom: 1px solid;
		border-color: var(--int-a-border-color);
		pointer-events: all;
		cursor: pointer;

		&:hover {
			border-color: var(--int-a-hover-border-color);
		}
	}
</style>
