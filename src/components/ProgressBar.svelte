<script>
    export let total = 100;
    export let progress = 0;
    $: progressWidth = 100 * (progress / total);
</script>

<div class="pad">
    <div class="background">
        <div class="progress" style:width="{progressWidth}%" />
    </div>
</div>

<style>
    .pad {
        padding: 0 1rem;
    }

    .background {
        display: inline-block;
        height: 12px;
        width: 100%;
        padding: 0;
        margin: 0;
        background-color: #3c4e60;
    }

    .progress {
        height: 100%;
        padding: 0;
        margin: 0;
        background-color: #faba66;
    }
</style>
