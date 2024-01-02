<script>
    import { onMount } from "svelte";
    import { watchResize } from "svelte-watch-resize";

    export let type = "lvh";
    export let width = 1;
    export let height = 1;

    let tid;
    let container;
    let initialScrollY = window.scrollY;

    const handleResize = () => {
        if (!container) return;
        clearTimeout(tid);
        
        const isScrollEvent = window.scrollY !== initialScrollY;

        if (!isScrollEvent || height < 23) {
            ({ width, height } = container.getBoundingClientRect());         
        }

        initialScrollY = window.scrollY;
    };

    const throttleResize = () => {
        clearTimeout(tid);
        tid = setTimeout(handleResize, 250);
    };

    onMount(() => {
        setTimeout(() => {
            window.dispatchEvent(new Event("resize"));
            throttleResize();
        }, 125);

        return () => clearTimeout(tid);
    });
    
    window.addEventListener("scroll", () => {
        handleResize(); 
    });
</script>

<div class="dimensionsContainer">
    <div
        class:lvh={type === "lvh"}
        class:dvh={type === "dvh"}
        class:svh={type === "svh"}
        bind:this={container}
        use:watchResize={throttleResize}
    ></div>
</div>

<style lang="scss">
    .dimensionsContainer {
        position: relative;
        height: 0px;
        div {
            width: 100%;
            position: absolute;
            top: 0px;
            left: 0px;
            opacity: 0;                
            &.lvh {                    
                height: 100lvh;
            }
            &.dvh {                
                height: 100dvh;
            }
            &.svh {                
                height: 100svh;
            }
        }
    }
</style>
