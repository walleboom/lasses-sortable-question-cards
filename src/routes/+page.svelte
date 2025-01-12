<script lang="ts">
    import { dndzone } from "svelte-dnd-action";
    import { flip } from "svelte/animate";
    import CollapsibleCard from "$lib/CollapsibleCard.svelte";
    import type { DndEvent } from "svelte-dnd-action";

    const flipDurationMs = 200;

    function handleFinalize(
        e: CustomEvent<
            DndEvent<{
                id: number;
                title: string;
                details: string;
                open: boolean;
            }>
        >,
    ) {
        items = e.detail.items;
    }

    function handleConsider(
        e: CustomEvent<
            DndEvent<{
                id: number;
                title: string;
                details: string;
                open: boolean;
            }>
        >,
    ) {
        items = e.detail.items;
    }

    let items = [
        {
            id: 1,
            title: "Some great feedback",
            details: "Some details about this great feedback",
            open: false,
        },
        {
            id: 2,
            title: "This feedback might be usable",
            details: "Might need to ask them to elaborate",
            open: false,
        },
        {
            id: 3,
            title: "No clue what this means",
            details: "This goes to the bin",
            open: false,
        },
    ];
</script>

<section
    class="max-w-4xl m-4"
    use:dndzone={{ items, flipDurationMs }}
    onconsider={handleConsider}
    onfinalize={handleFinalize}
>
    {#each items as item (item.id)}
        <div
            class="card rounded overflow-hidden shadow-lg my-4"
            animate:flip={{ duration: flipDurationMs }}
        >
            <CollapsibleCard open={item.open} class="px-6 py-4 w-full">
                {#snippet header()}
                    <button class="font-bold text-xl text-left" onclick={() => (item.open = !item.open)}>
                        {item.title}
                    </button>
                {/snippet}
                {#snippet body()}
                    <p class="text-gray-700 text-base">{item.details}</p>
                {/snippet}
            </CollapsibleCard>
        </div>
    {/each}
</section>

<style lang="postcss">
    .card {
        background-color: theme(colors.gray.100);
    }
    :global(html) {
        background-color: theme(colors.gray.400);
    }
</style>
