<script>
    import { parseDigits, parseTime } from "../time";

    export let digits = "";
    export let time = 0;
    $: ({ hours, mins, secs } = digits.length
        ? parseDigits(digits)
        : parseTime(time + 950));
</script>

<div class="display">
    <div class="digit">{hours}<small>h</small></div>
    <div class="digit">{mins}<small>m</small></div>
    <div class="digit">{secs}<small>s</small></div>
</div>

<style>
    .display {
        display: inline-block;
        margin: 5px;
    }

    .digit {
        display: inline-block;
        color: #5c6e80;
        font-size: 5rem;
        margin: 1rem;
        margin-bottom: 0;
    }

    small {
        font-size: 2rem;
    }
</style>
