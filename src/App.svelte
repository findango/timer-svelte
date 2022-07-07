<script>
    import { onMount } from "svelte";
    import Display from "./components/Display.svelte";
    import ProgressBar from "./components/ProgressBar.svelte";
    import Controls from "./components/Controls.svelte";
    import Numberpad from "./components/Numberpad.svelte";

    const SECONDS = 1000;
    const MINUTES = 60 * SECONDS;
    const HOURS = 60 * MINUTES;
    const MAX_TIME = 9 * HOURS + 59 * MINUTES + 59 * SECONDS;
    const ALARM_LENGTH = 5 * SECONDS;

    const appStatus = {
        PAUSED: "paused",
        RUNNING: "running",
        ALARMING: "alarming",
        STOPPED: "stopped",
    };

    let status = appStatus.STOPPED;
    let initialTime = 10 * MINUTES;
    let remaining = initialTime;
    let digits = "";
    let lastTick = Date.now();
    $: buttonState = configureButtonState(initialTime, remaining, status);

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

    const configureButtonState = (initialTime, remaining, status) => {
        return {
            clearEnabled: status !== appStatus.RUNNING,
            playEnabled:
                status === appStatus.PAUSED ||
                (status === appStatus.STOPPED && remaining !== 0),
            pauseEnabled: status === appStatus.RUNNING,
            resetEnabled:
                status === appStatus.PAUSED && remaining !== initialTime,
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
        remaining = initialTime;
    };

    const clear = () => {
        status = appStatus.STOPPED;
        initialTime = 0;
        remaining = 0;
        digits = "";
    };

    const alarm = () => {
        status = appStatus.ALARMING;
        setTimeout(() => {
            if (status === appStatus.ALARMING && remaining === 0) {
                status = appStatus.PAUSED;
                remaining = initialTime;
            }
        }, ALARM_LENGTH);
    };

    const handleKeypad = (e) => {
        if (digits.length === 5 || (digits.length === 0 && e.detail === 0)) {
            return;
        }
        digits += e.detail;
        const [h, m, s] = ("000000" + digits).slice(-6).match(/\d\d/g);
        initialTime = Math.min(MAX_TIME, h * HOURS + m * MINUTES + s * SECONDS);
        remaining = initialTime;
    };
</script>

<main>
    <div class="timer">
        <Display time={remaining} {digits} />
        <ProgressBar total={initialTime} progress={remaining} />
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
</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 400px;
        margin: 0 auto;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    .timer {
        width: 400px;
        height: 800px;
    }
</style>
