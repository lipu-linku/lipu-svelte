<script lang="ts">
    import basic from '$lib/assets/basic.png'
    import grid from '$lib/assets/grid.png'
    import lukapona from '$lib/assets/luka-pona.png'

    export let selected_view: string;
    const views = {
        "basic": {
            "name": "Basic view",
            "icon": basic,
        },
        "grid": {
            "name": "Grid view",
            "icon": grid,
        },
        "luka-pona": {
            "name": "luka pona view",
            "icon": lukapona,
        }
    }

</script>

<label id="view_selector_wrapper" title="Select View">
    <select
        id="view_selector"
        class="main_input"
        bind:value={selected_view}
    >
        {#each Object.entries(views) as [key, view]}
            <option value={key}>{view['name']}</option>
        {/each}
    </select>
    <img src={views[selected_view]['icon']} alt="Select View" />
</label>

<style>
    #view_selector {
        font: inherit;
        background-color: var(--bg-color);
        color: var(--txt-color);

        cursor: pointer;
        position: absolute;

        top: 0;
        left: 0;
        width: 100%;
        height: 100%;

        opacity: 0;
    }

    #view_selector + img {
        pointer-events: none;
        /*border-radius: 100%;*/
    }
    #view_selector:focus + img {
        outline: 2px solid var(--highlight-color);
        outline-offset: 2px;
    }

    #view_selector_wrapper {
        position: relative;
    }

    img {
        width: 2rem;
        height: 2rem;
        display: block;
    }
    :global(.lightmode) img {
        filter: invert(100%);
    }
</style>
