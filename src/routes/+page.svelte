<script lang="ts">
    import { onMount } from "svelte";

    let showGreeting = $state(false);
    let showName = $state(false);
    let showButton = $state(false);
    let showSparkles = $state(false);
    let typedText = $state("");

    const greetingText =
        "Hai Yunilaku Sayang... Ada sesuatu yang mau aku sampaikan 💕";

    onMount(() => {
        showGreeting = true;

        // Typewriter effect
        let i = 0;
        const typeInterval = setInterval(() => {
            if (i < greetingText.length) {
                typedText = greetingText.slice(0, i + 1);
                i++;
            } else {
                clearInterval(typeInterval);
                setTimeout(() => {
                    showName = true;
                }, 400);
                setTimeout(() => {
                    showSparkles = true;
                }, 800);
                setTimeout(() => {
                    showButton = true;
                }, 1200);
            }
        }, 60);

        return () => clearInterval(typeInterval);
    });
</script>

<section class="landing">
    <!-- Background sparkles -->
    {#if showSparkles}
        <div class="sparkles-container" aria-hidden="true">
            {#each Array(12) as _, i}
                <span
                    class="sparkle-dot"
                    style="
          left: {10 + Math.random() * 80}%;
          top: {10 + Math.random() * 80}%;
          animation-delay: {Math.random() * 3}s;
          font-size: {0.5 + Math.random() * 1}rem;
        ">✨</span
                >
            {/each}
        </div>
    {/if}

    <div class="landing-content">
        <!-- Giant heart -->
        <div class="heart-container">
            <span class="giant-heart animate-heartbeat">❤️</span>
        </div>

        <!-- Greeting text with typewriter -->
        <div class="greeting-section">
            {#if showGreeting}
                <p class="greeting-text font-cursive">
                    {typedText}<span class="cursor">|</span>
                </p>
            {/if}
        </div>

        <!-- Name reveal -->
        {#if showName}
            <div class="name-reveal animate-fade-in-up">
                <h1 class="name-text font-heading">
                    Untuk Kamu yang Spesial 🌹
                </h1>
            </div>
        {/if}

        <!-- CTA Button -->
        {#if showButton}
            <div class="cta-container animate-fade-in-up stagger-3">
                <a href="/love-letter" class="cta-button animate-pulse-glow">
                    <span class="cta-emoji">💌</span>
                    Buka Kejutan
                    <span class="cta-emoji">💌</span>
                </a>
                <p class="cta-hint font-body">
                    klik untuk memulai perjalanan cinta kita 💕
                </p>
            </div>
        {/if}
    </div>

    <!-- Decorative bottom wave -->
    <div class="wave-decoration" aria-hidden="true">
        <svg
            viewBox="0 0 1440 120"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
        >
            <path
                d="M0,60 C360,120 720,0 1080,60 C1260,90 1380,80 1440,60 L1440,120 L0,120 Z"
                fill="rgba(225, 29, 72, 0.05)"
            />
        </svg>
    </div>
</section>

<style>
    .landing {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        overflow: hidden;
        padding: 2rem;
    }

    .sparkles-container {
        position: absolute;
        inset: 0;
        pointer-events: none;
        z-index: 0;
    }

    .sparkle-dot {
        position: absolute;
        animation: sparkle 3s ease-in-out infinite;
    }

    .landing-content {
        text-align: center;
        z-index: 1;
        max-width: 600px;
        width: 100%;
    }

    .heart-container {
        margin-bottom: 2rem;
    }

    .giant-heart {
        font-size: 5rem;
        display: inline-block;
        filter: drop-shadow(0 0 30px rgba(225, 29, 72, 0.4));
    }

    .greeting-section {
        min-height: 3rem;
        margin-bottom: 1.5rem;
    }

    .greeting-text {
        font-size: 1.5rem;
        color: #881337;
        line-height: 1.6;
        display: inline;
    }

    .cursor {
        animation: blink 1s step-end infinite;
        color: #e11d48;
        font-weight: 300;
    }

    .name-reveal {
        margin-bottom: 2.5rem;
    }

    .name-text {
        font-size: 2.2rem;
        font-weight: 700;
        color: #e11d48;
        line-height: 1.3;
        text-shadow: 0 2px 20px rgba(225, 29, 72, 0.2);
    }

    .cta-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
    }

    .cta-button {
        display: inline-flex;
        align-items: center;
        gap: 0.75rem;
        padding: 1rem 2.5rem;
        background: linear-gradient(135deg, #e11d48, #f43f5e);
        color: white;
        text-decoration: none;
        font-family: var(--font-body);
        font-size: 1.15rem;
        font-weight: 600;
        border-radius: 50px;
        transition: all 0.3s ease;
        border: none;
        cursor: pointer;
    }

    .cta-button:hover {
        transform: translateY(-3px) scale(1.03);
        box-shadow: 0 8px 35px rgba(225, 29, 72, 0.4);
    }

    .cta-emoji {
        font-size: 1.3rem;
    }

    .cta-hint {
        font-size: 0.85rem;
        color: #fb7185;
        font-weight: 400;
    }

    .wave-decoration {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        z-index: 0;
    }

    @media (max-width: 640px) {
        .giant-heart {
            font-size: 3.5rem;
        }
        .greeting-text {
            font-size: 1.2rem;
        }
        .name-text {
            font-size: 1.6rem;
        }
        .cta-button {
            padding: 0.85rem 2rem;
            font-size: 1rem;
        }
    }
</style>
