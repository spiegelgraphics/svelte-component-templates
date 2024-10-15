<script>
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';
	import Select from 'svelte-select/Select.svelte';

	export let showSelectLabel = true;
	export let selectLabel = '';
	export let data = [];
	export let selectOptions = {};
	export let selectedItem;
	export const selectFocused = writable(false);
	export const clearInput = () => {
		selectedItem = undefined;
		listOpen = false;
	};

	const defaultSelectOptions = {
		dataFieldNames: { key: 'id', label: 'name' },
		additionalSearchFields: [],
		nrListItemsVisible: 3,
		placeholder: 'Suche',
		searchable: true,
		clearable: true,
		floatingUI: { placement: 'bottom-start' },
	};
	const floatingUI = {
		...defaultSelectOptions.floatingUI,
		...(selectOptions.floatingUI || {}),
	};
	const options = {
		...defaultSelectOptions,
		...selectOptions,
		floatingUI,
	};
	const dataKeys = options.dataFieldNames;
	const searchFields = [dataKeys.label, ...options.additionalSearchFields];
	const nrSearchFields = searchFields.length;
	const isFirefox = navigator.userAgent.toLowerCase().indexOf('firefox') >= 0;
	const letterSpacing = isFirefox ? 0.06 : 0.04;

	let items;
	let focused;
	let selectBox;
	let inputElement;
	let listOpen = false;
	let preSelectedItem;

	// Für init muss Variable als undefined vorausgesetzt;
	// ggf. (bei Bedienfehler) zurücksetzen
	if (selectedItem !== undefined) {
		selectedItem = undefined;
	}

	$: $selectFocused = focused;

	$: {
		items = data.map((item) => {
			const selectItem = { item };

			searchFields.forEach((searchFieldName) => {
				selectItem[searchFieldName] = item[searchFieldName] || item;
			});
			// das ID-/Key-Feld auf jeden Fall mitnehmen, auch wenn kein Suchfeld
			if (!selectItem[dataKeys.key]) {
				selectItem[dataKeys.key] = item[dataKeys.key] || item;
			}

			if (item.selected && !preSelectedItem) {
				preSelectedItem = item;
			}

			return selectItem;
		});
	}

	const itemFilter = (label, filterText, option) => {
		// console.log("\n itemFilter...", label, filterText, option);
		let hit;
		if (filterText) {
			for (let i = 0; i < nrSearchFields; i++) {
				const key = searchFields[i];
				const label = option[key] + '' || '';
				hit = label.toLowerCase().includes(filterText.toLowerCase());
				// console.log("hit?", hit, label);
				if (hit) {
					break;
				}
			}
			return hit;
		} else {
			return true;
		}
	};

	const onInput = () => {
		if (!inputElement) {
			inputElement = selectBox.querySelector('input');
		}
		inputElement.blur();
	};

	onMount(() => {
		if (preSelectedItem) {
			selectedItem = preSelectedItem;
		}
	});
</script>

<div class="autocomplete-select">
	{#if showSelectLabel && selectLabel}
		<div class="label">{selectLabel}</div>
	{/if}
	<div class="select-box">
		<div
			bind:this={selectBox}
			class="sp-search-field"
			style="--items-visible:{options.nrListItemsVisible};--input-letter-spacing:{letterSpacing}rem;"
		>
			<Select
				itemId={dataKeys.key}
				label={dataKeys.label}
				items={items}
				clearable={options.clearable}
				placeholder={options.placeholder}
				floatingConfig={floatingUI}
				itemFilter={itemFilter}
				closeListOnChange={false}
				listOpen={listOpen}
				showChevron={true}
				hideEmptyState={true}
				listOffset={-1}
				bind:value={selectedItem}
				bind:focused={focused}
				on:input={onInput}
			>
				<!-- Lupe -->
				<div slot="chevron-icon">
					<svg
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
					>
						<circle
							cx="10"
							cy="11"
							r="5.35"
							stroke="black"
							stroke-width="1.3"
						/>
						<path
							d="M14 15L18.5 19.5"
							stroke="black"
							stroke-width="1.3"
							stroke-linecap="round"
						/>
					</svg>
				</div>

				<!-- Item der select-list -->
				<div
					slot="item"
					class="listItem"
					let:item
					let:index
				>
					{item[dataKeys.label]}
				</div>

				<!-- Anzeige über dem Input-Feld liegend -->
				<div
					slot="selection"
					let:selection
				>
					{selection[dataKeys.label]}
				</div>
			</Select>
		</div>
	</div>
</div>

<style lang="scss">
	@import 'src/scss/config';

	// ---------------------
	// Wrapper-Formatierung
	// ---------------------

	.autocomplete-select {
		transition: opacity 1.6s linear;
		@include fontFamily('SpiegelSansUI-Regular');
	}

	.listItem {
		height: var(--item-height);
		cursor: pointer;
		user-select: none;
		user-focus-pointer: none !important;
	}

	.label {
		font-weight: 700;
	}

	.select-box {
		box-sizing: border-box;
		margin: 0.75rem 0 2rem;
		position: relative;
		z-index: 19; // Social-Leiste mit z-Index 20 >> in Embed-Anwendung 19 größter z-Index
	}

	// ---------------------
	// Select-Formatierungen
	// ---------------------

	// nur einen Indikator zur Zeit zeigen (Lupe oder X), wird X eingefügt, wird die Lupe zu Element 2
	:global(.sp-search-field .indicators div:nth-child(2)) {
		display: none !important;
	}

	// Select-List nicht via Slot, da zu viel Logik implementiert werden muss,
	// daher .svelte-select-list nur über :global zugänglich
	:global(.sp-search-field .svelte-select-list) {
		border-top: none !important;
		overscroll-behavior: contain;
	}

	.sp-search-field {
		// light-/dark-mode differenzierung
		--int-border: 1px solid #989694;
		--int-border-focused: 1px solid #000;
		--int-item-hover-bg: #d2d8dc;
		--int-item-is-active-bg: #d2d8dc;
		--int-placeholder-color: #989694;
		--int-chevron-color: #5c5a58;

		@media (screen and prefers-color-scheme: dark) {
			--int-border: 1px solid #bbb9b7;
			--int-border-focused: 1px solid #{$basic-white-dark};
			--int-item-hover-bg: #434444;
			--int-item-is-active-bg: #434444;
			--int-placeholder-color: #bbb9b7;
			--int-chevron-color: #bbb9b7;
		}

		$itemHeight: 38px;

		--background: var(--int-background-color-default);
		--padding: 0;
		--border: var(--int-border);
		--border-hover: var(--int-border);
		--border-focused: var(--int-border-focused);
		--border-radius: $spBorderRadius;
		--chevron-color: var(--int-chevron-color);
		--placeholder-color: var(--int-placeholder-color);
		--input-color: var(--int-font-color-default);
		--input-padding: 0.75rem;
		--input-margin: 0;
		--value-container-padding: 0 0.375rem 0 0.75rem;
		--item-hover-bg: var(--int-item-hover-bg);
		--item-active-background: var(--int-item-is-active-bg);
		--item-is-active-bg: var(--int-item-is-active-bg);
		--item-is-active-color: var(--int-font-color-default);
		--item-first-border-radius: 0;
		--item-last-border-radius: $spBorderRadius;
		--item-padding: 0.75rem;
		--item-height: #{$itemHeight};
		--item-line-height: 1;
		--selected-item-padding: 0;
		--list-max-height: calc(var(--item-height) * var(--items-visible));
		--list-background: var(--int-background-color-default);
		--list-border: var(--int-border-focused);
		--list-border-radius: $spBorderRadius;
		--list-shadow: none;
	}

	circle,
	path {
		stroke: var(--chevron-color);
	}
</style>
