<script>
	/**
	 * Padded Multiline: https://codepen.io/chriscoyier/pen/AEdYyL + white-space: break-spaces
	 * span.padded-multiline ist zusätzliche Verschachtelung analog zur Codepen - Lösung:
	 * div.body > span.padded - multiline > mark
	 * Gegenüber vorherigen Lösungen: Hintergrundbild auf Hintergrundfarbe geändert
	 * behebt Safari-Bug fehlendes Padding
	 *
	 * Options:
	 * Mit background_lm und color_lm bzw. background_dm und color_dm
	 * können Farben für light- und dark-mode gesetzt werden.
	 * "background" und "color" entfallen dann.
	 *
	 * Padding-right und padding-left müssen die gleiche Einheit haben.
	 */

	export let options = {};

	const defaultOptions = {
		background: 'rgba(0, 0, 0, 0.9)',
		color: 'rgba(255, 255, 255, 0.9)',
		padding: '0.2rem 0.5rem',
	};

	let pmOptions;

	$: {
		// Default-Optionen mit übergebenen Optionen zu Options-Objekt zusammenfügen
		pmOptions = { ...defaultOptions, ...options };
		// Paddings einzeln setzen, um abschließendes hMargin zu berechnen
		const [top, right, bottom, left] = pmOptions.padding.split(' ');
		pmOptions['padding-top'] = top;
		pmOptions['padding-right'] = right || top;
		pmOptions['padding-bottom'] = bottom || top;
		pmOptions['padding-left'] = left || right || top;
		const rxPaddingValue = /^(-?[\d.]+)(\D*)$/;
		const [, valRight, unit] = rxPaddingValue.exec(pmOptions['padding-right']);
		let [, valLeft, unit2] = rxPaddingValue.exec(pmOptions['padding-left']);
		if (unit !== unit2) {
			console.warn(
				'padded-multiline: padding-right und padding-left müssen die gleiche Einheit haben. padding-left wird auf padding-right gesetzt.',
			);
			valLeft = valRight;
		}
		// hMargin (Ausgleichs-Margin, daher negativ) berechnen aus padding-right und padding-left
		pmOptions.hMargin = 0 - +valRight - +valLeft + unit;
	}

	$: styles = Object.entries(pmOptions)
		.map(([key, value]) => `--${key}: ${value};`)
		.join(' ');
</script>

<span
	class="padded-multiline"
	style={styles}
>
	<span class="text">
		<slot />
	</span>
</span>

<style lang="scss">
	@import 'src/scss/config';

	.padded-multiline {
		display: inline-block;
		margin: 0 var(--hMargin);
		padding: var(--padding-top, 0.2rem) var(--padding-right, 0.5rem)
			var(--padding-bottom, 0.2rem) var(--padding-left, 0.5rem);

		$lightColor: rgba(255, 255, 255, 0.9);
		$darkColor: rgba(0, 0, 0, 0.9);

		--pml-backgroundColor: var(
			--background_lm,
			var(--color_dm, var(--background, #{$darkColor}))
		);
		--pml-color: var(
			--color_lm,
			var(--background_dm, var(--color, #{$lightColor}))
		);
		@media screen and (prefers-color-scheme: dark) {
			--pml-backgroundColor: var(
				--background_dm,
				var(--color_lm, var(--color, #{$lightColor}))
			);
			--pml-color: var(
				--color_dm,
				var(--background_lm, var(--background, #{$darkColor}))
			);
		}
	}

	.text {
		display: inline;
		border: none;
		border-radius: var(--borderRadius, #{$spBorderRadius});
		color: var(--pml-color);
		background-color: var(--pml-backgroundColor);
		-webkit-box-decoration-break: clone;
		box-decoration-break: clone;
		white-space: break-spaces;
		padding: var(--padding, 0.2rem 0.5rem);
	}
</style>
