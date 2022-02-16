# svelte-component-templates
DER SPIEGEL templates for Svelte components


## Content


### [TextAlign.svelte](./TextAlign.svelte)

Align child elements to the type area of spiegel.de.

*Properties*
```{JavaScript}
// type of the app: iframe or embed
export let type = 'iframe';

// disable component (if true, no alignment takes place)
export let disable = false;
```


### [SizeDetector.svelte](./SizeDetector.svelte)

Determine the size of an element without using Svelte iFrames and `bind:clientWidth` etc.

Good for apps that will be embedded directly.

*Properties*
```{JavaScript}
// width and height can be bound from outside
export let width = undefined;
export let height = undefined;
```

*Usage*
```{JavaScript}
<SizeDetector
    bind:width={width}
    bind:height={height}
>
    <div>stuff to be measured</div>
</SizeDetector>
```