<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();
    const click = (name) => () => dispatch(name);

    export let status;
    export let buttonState;
</script>

<div class="row">
    <button disabled={!buttonState.clearEnabled} on:click={click("clear")}>
        <i class="icono-cross" />
    </button>
    {#if status === "running"}
        <button disabled={!buttonState.pauseEnabled} on:click={click("pause")}>
            <i class="icono-pause" />
        </button>
    {:else}
        <button disabled={!buttonState.playEnabled} on:click={click("start")}>
            <i class="icono-play" />
        </button>
    {/if}
    <button disabled={!buttonState.resetEnabled} on:click={click("reset")}>
        <i class="icono-reset" />
    </button>
</div>

<style>
    i {
        width: 1rem;
        height: 1rem;
    }

    button {
        border: 0;
        border-radius: 2rem;
        margin: 0.5rem;
        padding: 0;
        width: 7rem;
        font-size: 3rem;
        font-weight: 100;
        color: #ddd;
        background-color: transparent;
        cursor: pointer;
    }

    button:hover {
        background-color: #3c4e60;
    }

    button[disabled]:hover {
        background-color: transparent;
        cursor: default;
    }

    .icono-reset {
        width: 20px;
        height: 20px;
        transform: rotate(0.5turn) scaleX(-1);
    }

    .icono-pause {
        width: 4px;
        height: 20px;
    }
</style>
