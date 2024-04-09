<script>
	import { onMount } from 'svelte';

	export let id;
	export let viz = undefined;

	let observer, container;

	function setViz() {
		[viz = undefined] =
			container.getElementsByTagName('datawrapper-visualization') || [];
		if (viz) observer.disconnect();
	}

	onMount(() => {
		const Observer =
			MutationObserver ||
			window.MutationObserver ||
			window.WebKitMutationObserver ||
			window.MozMutationObserver;
		observer = new Observer(setViz);
		observer.observe(container, {
			subtree: true,
			childList: true,
		});
		return () => observer.disconnect();
	});
</script>

<div bind:this={container}>
	<script
		type="text/javascript"
		defer
		src="https://datawrapper.dwcdn.net/{id}/embed.js"
		charset="utf-8"
	></script><noscript
		><img
			src="https://datawrapper.dwcdn.net/{id}/full.png"
			alt=""
		/></noscript
	>
</div>
