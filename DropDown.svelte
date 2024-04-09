<script>
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';

	/**
	 * Dropdown-Element, das onChange Custom-Event "dropdownChanged" feuert
	 * (callback => index: number [=selected Index], value: Array-Element aus config.items
	 * Minimal-Konfiguration: {items: ["Eintrag #1", "Eintrag #2", ...]}
	 * Minimal-Anforderung für Objekt, wenn config.items keine Strings enthält: {"label": "Eintrag #1}
	 * @type {selectOnInit: boolean, startOption: number, items: [{label: string, *: *}, ...]}
	 */
	export let config = {
		startOption: 0,
		selectOnInit: 0,
		items: [
			{ label: 'Ah', value: '2' },
			{ label: 'Be', complex: { a: -1, b: 12 } },
		],
	};

	let selectedItem, initSelectedItem;

	const dispatch = createEventDispatcher();

	const dispatchEvent = (indexSelectedItem) => {
		dispatch('dropdownChanged', {
			index: indexSelectedItem,
			value: config.items[indexSelectedItem],
		});
	};

	const setInitSelectedItem = (ixStartOption) => {
		initSelectedItem = +ixStartOption || 0;
		if (initSelectedItem > config.items.length || initSelectedItem < 0) {
			initSelectedItem = 0;
		}

		selectedItem = initSelectedItem;
	};

	onMount(() => {
		if (!!config.selectOnInit) {
			dispatchEvent(selectedItem);
		}
	});

	$: dispatchEvent(selectedItem);

	setInitSelectedItem(config.startOption);
</script>

<select bind:value={selectedItem}>
	{#each config.items as selectItem, index}
		{#if index === initSelectedItem}
			<option
				value={index}
				selected>{selectItem.label || selectItem}</option
			>
		{:else}
			<option value={index}>{selectItem.label || selectItem}</option>
		{/if}
	{/each}
</select>

<style lang="scss">
	@import './src/scss/config';

	select {
		box-sizing: border-box;
		display: flex;

		width: 100%;
		margin: 0 auto;
		padding: 0.75rem;

		background: url("data:image/svg+xml;charset=utf-8,%3Csvg class='stroke-current' width='16' height='16' fill='none' xmlns='http://www.w3.org/2000/svg' aria-hidden='true'%3E%3Cpath d='M12 6l-4 4-4-4' stroke-linecap='round' stroke='%23000'/%3E%3C/svg%3E")
			no-repeat center right 16px;
		-webkit-appearance: none;

		border: 1px solid #989694;
		border-radius: 1.5px;

		text-transform: none;
		font-size: 1rem;
		line-height: 1.26;
		color: #000;
		height: 45px;

		&:focus {
			outline: none;
		}

		&:hover {
			border-color: #000;
		}

		@media (prefers-color-scheme: dark) {
			color: #f1efed;
			background: url("data:image/svg+xml;charset=utf-8,%3Csvg class='stroke-current' width='16' height='16' fill='none' xmlns='http://www.w3.org/2000/svg' aria-hidden='true'%3E%3Cpath d='M12 6l-4 4-4-4' stroke-linecap='round' stroke='%23F1EFED'/%3E%3C/svg%3E")
				no-repeat center right 16px;
			background-color: #1f1e1c;
			&:hover {
				border-color: #fff;
			}
		}
	}
</style>
