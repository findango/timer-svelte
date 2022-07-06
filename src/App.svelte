<script>
    import { onMount } from "svelte";
    import Display from "./components/Display.svelte";
    import ProgressBar from "./components/ProgressBar.svelte";
    import Controls from "./components/Controls.svelte";
    import Numberpad from "./components/Numberpad.svelte";

    const SECONDS = 1000;
    const MINUTES = 60 * SECONDS;
    const HOURS = 60 * MINUTES;
    const MAX_TIME = 35999000; // 9:59:59

    const appStatus = {
        PAUSED: "paused",
        RUNNING: "running",
        ALARMING: "alarming",
        STOPPED: "stopped",
    };

    let status = appStatus.STOPPED;
    let startTime = 10 * MINUTES;
    let remaining = startTime;
    let digits = "";
    let lastTick = Date.now();
    $: buttonState = configureButtonState(startTime, remaining, status);

    onMount(() => {
        const timer = setInterval(() => {
            if (status !== appStatus.RUNNING) return;

            const elapsed = Date.now() - lastTick;
            remaining = remaining - elapsed;
            lastTick = Date.now();

            if (remaining <= 0) {
                remaining = 0;
                alarm();
            }
        }, 250);

        return () => clearInterval(timer);
    });

    const configureButtonState = (startTime, remaining, status) => {
        return {
            clearEnabled: status !== appStatus.RUNNING,
            playEnabled:
                status === appStatus.PAUSED ||
                (status === appStatus.STOPPED && remaining !== 0),
            pauseEnabled: status === appStatus.RUNNING,
            resetEnabled:
                status === appStatus.PAUSED && remaining !== startTime,
        };
    };

    const start = () => {
        status = appStatus.RUNNING;
        digits = "";
        lastTick = Date.now();
    };

    const pause = () => {
        status = appStatus.PAUSED;
    };

    const reset = () => {
        status = appStatus.PAUSED;
        remaining = startTime;
        digits = "";
    };

    const clear = () => {
        status = appStatus.STOPPED;
        startTime = 0;
        remaining = 0;
        digits = "";
    };

    const alarm = () => {
        status = appStatus.ALARMING;
        setTimeout(() => {
            if (status === appStatus.ALARMING && remaining === 0) {
                status = appStatus.PAUSED;
                remaining = startTime;
            }
        }, 5000);
    };

    const handleKeypad = (e) => {
        if (digits.length === 5 || (digits.length === 0 && e.detail === 0)) {
            return;
        }
        digits = ("000000" + digits + e.detail).substr(-6);
        const [h, m, s] = digits.match(/\d\d/g);
        startTime = Math.min(MAX_TIME, h * HOURS + m * MINUTES + s * SECONDS);
        remaining = startTime;
    };
</script>

<h1>Svelte Timer</h1>

<div class="timer">
    <Display time={remaining} {digits} />
    <ProgressBar total={startTime} progress={remaining} />
    <Controls
        {status}
        {buttonState}
        on:start={start}
        on:pause={pause}
        on:reset={reset}
        on:clear={clear}
    />
    {#if status === appStatus.STOPPED}
        <Numberpad on:click={handleKeypad} />
    {/if}
    {#if status === appStatus.ALARMING}
        <p>RING RING!</p>
    {/if}
</div>

<style>
    .timer {
        width: 600px;
        align-content: center;
    }
</style>
