<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    const click = (value) => () => dispatch("click", value);
</script>

<div>
    <div class="row">
        <button on:click={click(1)}>1</button>
        <button on:click={click(2)}>2</button>
        <button on:click={click(3)}>3</button>
    </div>
    <div class="row">
        <button on:click={click(4)}>4</button>
        <button on:click={click(5)}>5</button>
        <button on:click={click(6)}>6</button>
    </div>
    <div class="row">
        <button on:click={click(7)}>7</button>
        <button on:click={click(8)}>8</button>
        <button on:click={click(9)}>9</button>
    </div>
    <div class="row">
        <!-- <span className="button-num" /> -->
        <button on:click={click(0)}>0</button>
        <!-- <span className="button-num" /> -->
    </div>
</div>

<style>
    .row {
        align-items: center;
    }

    button {
        margin: 0.5rem;
        height: 5rem;
        width: 7rem;
        padding: 0;
        /*line-height: 3rem;*/
        font-size: 2rem;
        font-weight: 100;
        background-color: #3c4e60;
        border: 0;
        border-radius: 3rem;

        cursor: pointer;
    }
</style>
