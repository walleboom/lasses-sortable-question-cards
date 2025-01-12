<script lang="ts">
    import collapse from "svelte-collapse";

    let {
        open = true,
        header,
        body,
        ...otherProps
    } = $props();
</script>

<div {...otherProps}>
    {@render header()}
    <div use:collapse={{ open }}>
        {@render body()}
    </div>
</div>
