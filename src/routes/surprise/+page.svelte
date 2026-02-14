<script lang="ts">
    import { onMount } from "svelte";

    let phase = $state<"countdown" | "reveal" | "celebrate">("countdown");
    let countdownValue = $state(3);
    let heartRainActive = $state(false);
    let clickCount = $state(0);
    let rainHearts: {
        id: number;
        left: number;
        size: number;
        duration: number;
        emoji: string;
        delay: number;
    }[] = $state([]);

    const loveEmojis = [
        "❤️",
        "💕",
        "💖",
        "💗",
        "💝",
        "🥰",
        "💘",
        "💞",
        "💓",
        "🌹",
        "✨",
        "💕",
    ];

    onMount(() => {
        startCountdown();
    });

    function startCountdown() {
        phase = "countdown";
        countdownValue = 3;

        const interval = setInterval(() => {
            countdownValue--;
            if (countdownValue <= 0) {
                clearInterval(interval);
                setTimeout(() => {
                    phase = "reveal";
                    setTimeout(() => {
                        phase = "celebrate";
                        triggerFireworks();
                    }, 2000);
                }, 500);
            }
        }, 1000);
    }

    function triggerFireworks() {
        // Spawn heart particles
        spawnHeartRain(30);
    }

    function spawnHeartRain(count: number) {
        heartRainActive = true;
        const newHearts = Array.from({ length: count }, (_, i) => ({
            id: Date.now() + i + Math.random(),
            left: Math.random() * 100,
            size: 0.8 + Math.random() * 1.5,
            duration: 2 + Math.random() * 4,
            emoji: loveEmojis[Math.floor(Math.random() * loveEmojis.length)],
            delay: Math.random() * 2,
        }));
        rainHearts = [...rainHearts, ...newHearts];

        // Clean up after animation
        setTimeout(() => {
            rainHearts = rainHearts.filter((h) => !newHearts.includes(h));
            if (rainHearts.length === 0) heartRainActive = false;
        }, 6000);
    }

    function handleLoveClick() {
        clickCount++;
        spawnHeartRain(10 + clickCount * 5);
    }
</script>

<section class="surprise-page">
    <!-- Heart Rain -->
    {#if heartRainActive}
        <div class="heart-rain" aria-hidden="true">
            {#each rainHearts as heart (heart.id)}
                <span
                    class="rain-heart"
                    style="
            left: {heart.left}%;
            font-size: {heart.size}rem;
            animation-duration: {heart.duration}s;
            animation-delay: {heart.delay}s;
          "
                >
                    {heart.emoji}
                </span>
            {/each}
        </div>
    {/if}

    <div class="surprise-container">
        <!-- Countdown Phase -->
        {#if phase === "countdown"}
            <div class="countdown-phase">
                <p class="countdown-label font-body">Bersiap-siap... 🥁</p>
                <div class="countdown-number font-heading">
                    {#key countdownValue}
                        <span class="count-digit">{countdownValue}</span>
                    {/key}
                </div>
            </div>
        {/if}

        <!-- Reveal Phase -->
        {#if phase === "reveal"}
            <div class="reveal-phase">
                <div class="reveal-content">
                    <span class="reveal-emoji">💕</span>
                    <h1 class="reveal-text font-heading">Aku Sayang Kamu!</h1>
                    <span class="reveal-emoji">💕</span>
                </div>
            </div>
        {/if}

        <!-- Celebrate Phase -->
        {#if phase === "celebrate"}
            <div class="celebrate-phase animate-fade-in">
                <div class="celebrate-content">
                    <div class="big-heart animate-heartbeat">❤️</div>

                    <h1 class="celebrate-title font-heading">
                        Selamat Hari Valentine Yunilaku! 🎉
                    </h1>

                    <p class="celebrate-message font-cursive">
                        Kamu adalah hal terbaik yang pernah terjadi dalam
                        hidupku. Aku bersyukur memilikimu. Aku akan selalu
                        menyayangimu hari ini, besok, dan selamanya. 💕
                    </p>

                    <button
                        class="love-button animate-pulse-glow"
                        onclick={handleLoveClick}
                    >
                        <span class="button-text">
                            {clickCount === 0
                                ? "Aku Sayang Kamu! 🥰"
                                : clickCount < 3
                                  ? "Aku BANYAK Sayang Kamu!! 💕"
                                  : clickCount < 6
                                    ? "SAYANG BANGET!! 🥰💕💖"
                                    : "SUPER DUPER SAYANG!!! 💕❤️💖💗💝🥰"}
                        </span>
                    </button>

                    {#if clickCount > 0}
                        <p class="click-counter font-body animate-fade-in">
                            {clickCount === 1
                                ? "💕 Yeay!"
                                : clickCount < 5
                                  ? `💕 Kamu sudah klik ${clickCount}x! Terus klik! 😊`
                                  : `💕 ${clickCount}x klik! Cinta kita nggak terhitung! 🥰`}
                        </p>
                    {/if}

                    <div class="footer-message">
                        <div class="footer-divider" aria-hidden="true">
                            ✿ ─────── ❤️ ─────── ✿
                        </div>
                        <p class="footer-text font-cursive">
                            Dari seseorang yang mencintaimu_ Sakti 💕
                        </p>
                        <p class="footer-date font-body">
                            Valentine's Day 2026 🌹
                        </p>
                    </div>
                </div>
            </div>
        {/if}
    </div>
</section>

<style>
    .surprise-page {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        overflow: hidden;
        padding: 2rem;
        background: linear-gradient(135deg, #fff1f2, #fce7f3, #fdf2f8);
        background-size: 200% 200%;
        animation: gradientShift 6s ease infinite;
    }

    .surprise-container {
        z-index: 1;
        text-align: center;
        max-width: 650px;
        width: 100%;
    }

    /* ---- Heart Rain ---- */
    .heart-rain {
        position: fixed;
        top: -10%;
        left: 0;
        width: 100%;
        height: 120vh;
        pointer-events: none;
        z-index: 50;
        overflow: hidden;
    }

    .rain-heart {
        position: absolute;
        top: -5%;
        animation: rainHeart linear forwards;
        filter: drop-shadow(0 0 4px rgba(225, 29, 72, 0.3));
    }

    /* ---- Countdown ---- */
    .countdown-phase {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
    }

    .countdown-label {
        font-size: 1.2rem;
        color: #881337;
    }

    .countdown-number {
        width: 120px;
        height: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .count-digit {
        font-size: 5rem;
        font-weight: 700;
        color: #e11d48;
        animation: countdownPulse 1s ease forwards;
        display: inline-block;
        text-shadow: 0 0 30px rgba(225, 29, 72, 0.3);
    }

    /* ---- Reveal ---- */
    .reveal-phase {
        animation: scaleUp 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
    }

    .reveal-content {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 1rem;
    }

    .reveal-text {
        font-size: 3rem;
        font-weight: 700;
        color: #e11d48;
        text-shadow: 0 0 40px rgba(225, 29, 72, 0.3);
    }

    .reveal-emoji {
        font-size: 2.5rem;
        animation: heartbeat 1s ease-in-out infinite;
    }

    /* ---- Celebrate ---- */
    .celebrate-phase {
        animation-fill-mode: forwards;
    }

    .celebrate-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1.5rem;
    }

    .big-heart {
        font-size: 5rem;
        filter: drop-shadow(0 0 30px rgba(225, 29, 72, 0.4));
    }

    .celebrate-title {
        font-size: 2.5rem;
        font-weight: 700;
        color: #e11d48;
        line-height: 1.3;
        text-shadow: 0 2px 20px rgba(225, 29, 72, 0.2);
    }

    .celebrate-message {
        font-size: 1.3rem;
        color: #881337;
        line-height: 1.8;
        max-width: 500px;
    }

    .love-button {
        padding: 1.1rem 2.5rem;
        background: linear-gradient(135deg, #e11d48, #f43f5e);
        color: white;
        border: none;
        font-family: var(--font-body);
        font-size: 1.1rem;
        font-weight: 600;
        border-radius: 50px;
        cursor: pointer;
        transition: all 0.3s ease;
        margin-top: 0.5rem;
    }

    .love-button:hover {
        transform: translateY(-3px) scale(1.05);
        box-shadow: 0 8px 35px rgba(225, 29, 72, 0.4);
    }

    .love-button:active {
        transform: scale(0.95);
    }

    .click-counter {
        font-size: 0.95rem;
        color: #fb7185;
        animation-fill-mode: forwards;
    }

    .footer-message {
        margin-top: 2rem;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.5rem;
    }

    .footer-divider {
        color: #fda4af;
        font-size: 0.85rem;
        letter-spacing: 2px;
    }

    .footer-text {
        font-size: 1.2rem;
        color: #881337;
    }

    .footer-date {
        font-size: 0.85rem;
        color: #fb7185;
        font-weight: 500;
    }

    @media (max-width: 640px) {
        .reveal-text {
            font-size: 2rem;
        }
        .reveal-emoji {
            font-size: 1.5rem;
        }
        .celebrate-title {
            font-size: 1.8rem;
        }
        .celebrate-message {
            font-size: 1.05rem;
        }
        .big-heart {
            font-size: 3.5rem;
        }
        .love-button {
            padding: 0.9rem 2rem;
            font-size: 0.95rem;
        }
    }
</style>
