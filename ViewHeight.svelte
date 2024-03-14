<script>
	import {onMount} from "svelte";


	export let visualHeight;
	export let headerHeight;
	export let footerHeight;
	export let contentHeight;
	export let safeHeight;  // work in progress; see ios safari
	export let threshold = 180;  // tbd: export or const?


	const isApp = document.documentElement.classList.contains("is-app");

	let tidResize;
	let container;
	let containerHeight = 0;
	
	const isSmall = () => window.innerWidth < 520;

	const getHeaderHeight = () => isApp ? 0 : isSmall() ? 100 : 108;
	
	const getFooterHeight = () =>	isApp ? 59 : 0;

	const handleResize = () => {

		if (!container) return;

		({height: containerHeight} = container.getBoundingClientRect());
		const isChangeGreaterTolerance = Math.abs(containerHeight - visualHeight) > threshold;

		if (!visualHeight || containerHeight > visualHeight || isChangeGreaterTolerance) {
			visualHeight = Math.floor(containerHeight);
		}

		headerHeight = getHeaderHeight();
		footerHeight = getFooterHeight();
		contentHeight = visualHeight - headerHeight - footerHeight;

		if (!safeHeight || isChangeGreaterTolerance) {
			safeHeight = Math.floor(document.documentElement.clientHeight) - headerHeight - footerHeight;
		}

	};

	const throttleResize = () => {
		clearTimeout(tidResize);
		tidResize = setTimeout(handleResize, 125);
	};

	onMount(() => {
		throttleResize();
		return () => clearTimeout(tidResize);
	});

	window.addEventListener("resize", throttleResize);

</script>


<div bind:this={container}></div>


<style lang="scss">

	div {
		width: 100%;
		position: absolute;
		z-index: 999999;
		top: 0;
		left: 0;
		opacity: 0;
		pointer-events: none;
		height: 100vh;
	}

</style>
