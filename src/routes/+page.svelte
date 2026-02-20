<!--<h1>Welcome to SvelteKit</h1>-->
<!--<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>-->
<script>
    // EN: Timer state
    // DE: Timer-Zustand
    // AR:
    // - حالة المؤقت
    let totalSeconds = 10;
    let secondsLeft = totalSeconds;
    let running = false;
    let timerId = null;

    // EN: Progress from 0 to 1
    // DE: Fortschritt von 0 bis 1
    // AR:
    // - التقدّم من 0 إلى 1
    $: progress = (totalSeconds - secondsLeft) / totalSeconds;

    function start() {
        if (running) return;
        running = true;

        timerId = setInterval(() => {
            if (secondsLeft > 0) {
                secondsLeft -= 1;
            } else {
                pause();
            }
        }, 1000);
    }

    function pause() {
        running = false;
        if (timerId) clearInterval(timerId);
        timerId = null;
    }

    function setDuration(seconds) {
        totalSeconds = seconds;
        reset()
    }

    function reset() {
        pause();
        secondsLeft = totalSeconds;
    }
</script>

<h1>Bunny Timer</h1>

<p>Seconds left: {secondsLeft}</p>

<div class="track">
    <div
            class="bunny"
            style="left: {progress * 100}%;"
            aria-label="bunny"
    >
        🐰
    </div>

    <div class="carrot" aria-label="carrot">🥕</div>
</div>
<div class="duration">
    <button on:click={() =>setDuration(5)}>5s</button>
    <button on:click={() =>setDuration(10)}>10s</button>
    <button on:click={() =>setDuration(30)}>30s</button>
</div>

<div class="buttons">
    <button on:click={start} disabled={running}>Start</button>
    <button on:click={pause} disabled={!running}>Pause</button>
    <button on:click={reset}>Reset</button>
</div>

<style>
    .track {
        position: relative;
        margin-top: 16px;
        height: 64px;
        border: 2px solid #ccc;
        border-radius: 12px;
        padding: 12px;
    }

    .bunny {
        position: absolute;
        top: 18px;
        transform: translateX(-50%);
        font-size: 28px;
    }

    .carrot {
        position: absolute;
        right: 12px;
        top: 18px;
        font-size: 28px;
    }

    .buttons {
        margin-top: 16px;
        display: flex;
        gap: 10px;
    }

    button {
        padding: 8px 12px;
    }
    .duration{
        margin-top: 16px;
        display: flex;
        gap: 12px;
    }
</style>
