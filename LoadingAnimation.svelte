<script>
	export let height = 333;

	height = Math.max(height, 140);
</script>

<div
	class="loadingAni flex h-full items-center justify-center w-full"
	style="--int-loader-height: {height}px"
>
	<div
		class="absolute w-100 h-100 left-6/12 top-6/12 -ml-64 -mt-59 origin-60 overflow-hidden mask-gradient-to-b animate-spin"
	>
		<div class="w-120 h-120 rounded-circle shadow-loader"></div>
	</div>
	<span class="text-primary-base dark:text-dm-primary-base"
		><svg
			class="fill-current"
			width="40"
			height="66"
			viewBox="0 0 24 30"
			role="img"
			aria-hidden="true"
			><path
				class="fill-current"
				d="M20 29.4V13H6.3V5.4h7.2V10H20V3.4C20 1.5 18.4 0 16.5 0h-13C1.6 0 0 1.6 0 3.5v15h13.5v9H6.3v-6.1H0v8C0 31.3 1.6 33 3.5 33h13c1.9-.2 3.5-1.7 3.5-3.6z"
			></path></svg
		>
	</span>
</div>

<style>
	.loadingAni {
		position: relative;
		justify-content: center !important;
		align-items: center !important;
	}

	.fill-current {
		fill: currentColor;
	}

	.w-full {
		width: 100%;
	}

	.absolute {
		position: absolute;
	}

	.h-full {
		height: var(--int-loader-height);
		min-height: 140px;
	}

	.justify-center {
		justify-content: center;
	}

	.items-center {
		align-items: center;
	}

	.flex {
		display: flex;
	}

	.animate-spin {
		-webkit-animation: spin 1.6s linear infinite;
		animation: spin 1.6s linear infinite;
	}

	.origin-60 {
		-webkit-transform-origin: 60% 60%;
		transform-origin: 60% 60%;
	}

	.w-100 {
		width: 6.25rem;
	}

	.left-6\/12 {
		left: 50%;
	}

	.top-6\/12 {
		top: 50%;
	}

	.absolute {
		position: absolute;
	}

	.overflow-hidden {
		overflow: hidden;
	}

	.-ml-64 {
		margin-left: -4rem;
	}

	.-mt-59 {
		margin-top: -3.6875rem;
	}

	.h-100 {
		height: 6.25rem;
	}

	.mask-gradient-to-b {
		-webkit-mask-image: -webkit-linear-gradient(top, #000, transparent);
		mask-image: -webkit-linear-gradient(top, #000, transparent);
	}

	@-webkit-keyframes spin {
		0% {
			-webkit-transform: rotate(0deg);
			transform: rotate(0deg);
		}
		to {
			-webkit-transform: rotate(1turn);
			transform: rotate(1turn);
		}
	}

	@keyframes spin {
		0% {
			-webkit-transform: rotate(0deg);
			transform: rotate(0deg);
		}
		to {
			-webkit-transform: rotate(1turn);
			transform: rotate(1turn);
		}
	}

	.animate-spin {
		-webkit-animation: spin 1.6s linear infinite;
		animation: spin 1.6s linear infinite;
	}

	.w-120 {
		width: 7.5rem;
	}

	.shadow-loader {
		--tw-shadow: inset 0 0 0 2px #e64415;
		box-shadow: var(--tw-ring-offset-shadow, 0 0 transparent),
			var(--tw-ring-shadow, 0 0 transparent), var(--tw-shadow);
	}

	.h-120 {
		height: 7.5rem;
	}

	.rounded-circle {
		border-radius: 50%;
	}

	.dark\:text-dm-primary-base {
		color: #d03d12;
	}

	.text-primary-base {
		color: #e64415;
	}

	* {
		--tw-shadow: 0 0 transparent;
	}

	svg {
		display: inline;
		vertical-align: baseline;
	}
</style>
