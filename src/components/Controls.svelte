<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let status;
    export let buttonState;
</script>

<div class="row">
    <button
        disabled={!buttonState.clearEnabled}
        on:click={() => dispatch("clear")}
    >
        <i class="icono-cross" />
    </button>
    {#if status === "running"}
        <button
            disabled={!buttonState.pauseEnabled}
            on:click={() => dispatch("pause")}
        >
            <i class="icono-pause" />
        </button>
    {:else}
        <button
            disabled={!buttonState.playEnabled}
            on:click={() => dispatch("start")}
        >
            <i class="icono-play" />
        </button>
    {/if}
    <button
        disabled={!buttonState.resetEnabled}
        on:click={() => dispatch("reset")}
    >
        <i class="icono-reset" />
    </button>
</div>

<style>
    i {
        width: 0.8rem;
        height: 0.8rem;
    }

    /* .button {
        padding: 0 2rem;
        text-transform: none;
    } */

    button {
        border: 1px;
        margin: 0.5rem;
        padding: 0;
        width: 7rem;
        font-size: 2rem;
        font-weight: 100;
        background-color: transparent;
        cursor: pointer;
    }

    /*.button-control .icono-cross {
    width: 50%;
    height: 10px;
}*/

    /*.button-control .icono-play {
    width: 15px;
    height: 15px;
}
*/

    button:focus {
        background-color: transparent;
    }

    button[disabled]:hover {
        background-color: transparent;
    }

    .icono-reset {
        width: 20px;
        height: 20px;
    }

    .icono-pause {
        width: 4px;
        height: 20px;
    }

    /*.button.button-outline {
    margin: 5px;
    border-color: grey;
    color: grey;
}*/
</style>
