<script>
    import { fly } from 'svelte/transition';

    export let number;
    export let offset = 50;
    export let duration = 600;
    export let delay = 0;
    export let width = 32;
    export let deactivate = false;
</script>

<span
    class="number-switcher"
    style="--width: {width}px;"
>
    {#key number}
        <span
            class="number"
            in:fly={{
                y: -offset,
                duration: deactivate ? 0 : duration,
                opacity: 0,
                delay
            }}
            out:fly|local={{
                y: offset,
                duration: deactivate ? 0 : duration,
                opacity: 0
            }}
        >
            {number}
        </span>
    {/key}
</span>

<style lang="scss">
    .number-switcher {
        position: relative;
        display: inline-block;
        width: var(--width);
        height: auto;
        text-align: center;

        .number {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            transform: translateY(-50%);
        }
    }
</style>