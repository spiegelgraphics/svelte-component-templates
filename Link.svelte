<script>
    export let href;
</script>

<a href={href}>
    <slot />
</a>

<style lang="scss">
    a {
        text-decoration: none;
        border-bottom: 1px solid;
        border-color: #eec2b5;
        pointer-events: all;
        cursor: pointer;
        color: #e64415;
        
        @media (prefers-color-scheme: dark) {
            color: #fff;
            border-color: #bbb9b7;
        }
        
        &:hover {
            border-color: #e64415;
            @media (prefers-color-scheme: dark) {
                border-color: #fff;
            }
        }
    }
</style>