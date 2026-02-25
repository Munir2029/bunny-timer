<!--<h1>Welcome to SvelteKit</h1>-->
<!--<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>-->
<script>
    // EN: Timer state
    // DE: Timer-Zustand
    // AR:
    // - حالة المؤقت
    let totalMinutes = 10;
    let minutesLeft = totalMinutes;
    let running = false;
    let timerId = null;
    let celebrate = false;

    // EN: Progress from 0 to 1
    // DE: Fortschritt von 0 bis 1
    // AR:
    // - التقدّم من 0 إلى 1
    $: progress = (totalMinutes - minutesLeft) / totalMinutes;

    function start() {
        if (running) return;
        running = true;

        timerId = setInterval(() => {
            if (minutesLeft > 0) {
                minutesLeft -= 1;
            } else {
                celebrate = true;
                pause();
            }
        }, 1000);
    }

    function pause() {
        running = false;
        if (timerId) clearInterval(timerId);
        timerId = null;
    }

    function setDuration(minutes) {
        totalMinutes = minutes;
        reset();
    }

    function reset() {
        pause();
        celebrate = false;
        minutesLeft = totalMinutes;
    }
</script>

<h1>Bunny Timer</h1>

<p>minutesLeft: {minutesLeft}</p>

<div class="track">
    <div
            class="bunny {running ? 'jump' : ''} {celebrate ? 'party' : ''}"
            style="left: calc(18px + (100% - 16px - 56px) * {progress});"
            aria-label="bunny"
    ><img src="/bunny.jpg" alt="bunny" />
    </div>

    <div class="carrot" aria-label="carrot">🥕</div>
</div>

{#if celebrate}
    <div class="celebrate" aria-label="celebration">
        <div class="confetti">🎉✨🎉✨🎉</div>
        <div class="msg">Time’s up! Bunny got the carrot 🥕</div>
    </div>
{/if}
<div class="duration">
    <button on:click={() =>setDuration(5)}>5m</button>
    <button on:click={() =>setDuration(10)}>10m</button>
    <button on:click={() =>setDuration(30)}>30m</button>
</div>

<div class="buttons">
    <button on:click={start} disabled={running}>Start</button>
    <button on:click={pause} disabled={!running}>Pause</button>
    <button on:click={reset}>Reset</button>
</div>

<style>

    :global(body) {
        margin: 0;
        background: #EAF7E4;
    }

    .track {
        position: relative;
        margin-top: 16px;
        height: 80px;
        border: 2px solid #cbd5e1;
        border-radius: 16px;
        padding: 16px;
        background: linear-gradient(180deg, #ffffff, #f8fafc);
        overflow: hidden;
    }
    .track::before {
        content: "";
        position: absolute;
        left: 16px;
        right: 56px;   /* اترك مساحة للجزر */
        top: 50%;
        transform: translateY(-50%);
        height: 16px;
        border-radius: 999px;
        background: #e2e8f0;
        box-shadow: inset 0 0 0 2px #cbd5e1;
    }
    .track::after {
        content: "";
        position: absolute;
        left: 24px;
        right: 64px;
        top: 50%;
        transform: translateY(-50%);
        border-top: 3px dashed #94a3b8;
        opacity: 0.8;
    }

    .bunny {
        position: absolute;
        top: 50%;
        transform: translate(-50%, -50%);
        font-size: 30px;
        background: #cc1876;
        border: 2px solid #cbd5e1;
        border-radius: 999px;
        padding: 6px 10px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    .bunny img {
        width: 40px;
        height: 40px;
        display: block;

    }

    .carrot {
        position: absolute;
        right: 16px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 30px;
        background: white;
        border: 2px solid #cbd5e1;
        border-radius: 999px;
        padding: 6px 10px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.08);
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
    .jump {
        animation: hop 0.6s infinite;
    }

    @keyframes hop {
        0%   { transform: translate(-50%, -50%) translateY(0); }
        40%  { transform: translate(-50%, -50%) translateY(-10px); }
        100% { transform: translate(-50%, -50%) translateY(0); }
    }


    .celebrate {
        margin-top: 12px;
        padding: 12px;
        border-radius: 12px;
        background: rgba(255,255,255,0.8);
        border: 2px solid #cbd5e1;
        text-align: center;
    }

    .confetti {
        font-size: 22px;
        animation: confettiPop 0.6s infinite;
    }

    .msg {
        margin-top: 6px;
        font-weight: 600;
    }

    .party {
        animation: partyDance 0.35s infinite;
    }

    @keyframes partyDance {
        0%   { transform: translate(-50%, -50%) rotate(0deg); }
        25%  { transform: translate(-50%, -50%) rotate(6deg); }
        50%  { transform: translate(-50%, -50%) rotate(0deg); }
        75%  { transform: translate(-50%, -50%) rotate(-6deg); }
        100% { transform: translate(-50%, -50%) rotate(0deg); }
    }

    @keyframes confettiPop {
        0% { transform: scale(1); }
        50% { transform: scale(1.08); }
        100% { transform: scale(1); }
    }
</style>
