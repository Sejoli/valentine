<script lang="ts">
    import { onMount } from "svelte";

    let envelopeOpen = $state(false);
    let showLetter = $state(false);
    let visibleParagraphs = $state(0);

    const letterParagraphs = [
        "Sayang...",
        "Aku tahu mungkin aku nggak selalu bisa mengungkapkan perasaanku dengan kata kata. Tapi hari ini, di hari yang spesial ini, aku mau mencoba.",
        "Aku mau bilang kamu adalah wanita terbaik dalam hidupku.  ",
        "Setiap hal baik hal sederhana yang kamu lakukan — tangan yang membelai kepalaku, yang merangkulku dari belakang, tawa yang begitu lepas, cara marahmu yang lucu, bahkan cara kamu tidur yang mulutnya terbuka — semuanya bikin aku tersenyum dan sayang sama kamu. 😊",
        "Aku sangat bersyukur karena punya punya malaikat seperti kamu yang menolongku, yang mendengarku, yang memanjakanku, yang selalu ada di sampingku. Kamu bukan cuma tunangan yang akan kunikahi, tapi kamu juga sahabat terbaikku. 💕",
        "Terima kasih sudah selalu ada, sudah selalu sabar, dan sudah selalu memilih aku setiap harinya. Aku akan selalu berusaha jadi yang terbaik untukmu.",
        "Selamat Hari Valentine, Yunilaku Sayang! Kamu adalah hal terbaik yang pernah terjadi di hidupku. ❤️",
        "Dengan sepenuh hati,\nDari orang yang paling sayang sama kamu_ Sakti 💕",
    ];

    function openEnvelope() {
        envelopeOpen = true;
        setTimeout(() => {
            showLetter = true;
            revealParagraphs();
        }, 800);
    }

    function revealParagraphs() {
        const interval = setInterval(() => {
            if (visibleParagraphs < letterParagraphs.length) {
                visibleParagraphs++;
            } else {
                clearInterval(interval);
            }
        }, 600);
    }

    onMount(() => {
        // Auto open after a small delay for effect
        setTimeout(openEnvelope, 1000);
    });
</script>

<section class="letter-page">
    <div class="letter-container">
        <!-- Envelope -->
        <div class="envelope" class:opened={envelopeOpen}>
            <div class="envelope-top" class:open={envelopeOpen}></div>
            <div class="envelope-body">
                <button
                    class="envelope-heart"
                    onclick={openEnvelope}
                    aria-label="Buka amplop"
                >
                    {envelopeOpen ? "💕" : "💌"}
                </button>
                {#if !envelopeOpen}
                    <p class="tap-hint font-body">ketuk untuk membuka 💕</p>
                {/if}
            </div>
        </div>

        <!-- Letter Content -->
        {#if showLetter}
            <div class="letter-paper glass-card animate-fade-in-up">
                <div class="letter-decoration-top" aria-hidden="true">
                    ✿ ─────── ❤️ ─────── ✿
                </div>

                <div class="letter-body">
                    {#each letterParagraphs as paragraph, i}
                        {#if i < visibleParagraphs}
                            <p
                                class="letter-text font-cursive animate-fade-in-up"
                                class:letter-greeting={i === 0}
                                class:letter-closing={i ===
                                    letterParagraphs.length - 1}
                                style="animation-delay: {i * 0.15}s"
                            >
                                {#each paragraph.split("\n") as line, j}
                                    {#if j > 0}<br />{/if}
                                    {line}
                                {/each}
                            </p>
                        {/if}
                    {/each}
                </div>

                <div class="letter-decoration-bottom" aria-hidden="true">
                    ✿ ─────── 💕 ─────── ✿
                </div>
            </div>

            <!-- Navigation -->
            <div
                class="letter-nav animate-fade-in-up"
                style="animation-delay: {letterParagraphs.length * 0.15 + 0.5}s"
            >
                <a href="/reasons" class="next-button">
                    Lanjut ke Halaman Berikutnya →
                    <span>💖</span>
                </a>
            </div>
        {/if}
    </div>
</section>

<style>
    .letter-page {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 2rem;
        padding-top: 80px;
        padding-bottom: 80px;
    }

    .letter-container {
        max-width: 650px;
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
    }

    /* ---- Envelope ---- */
    .envelope {
        position: relative;
        width: 200px;
        height: 140px;
        cursor: pointer;
        transition: transform 0.5s ease;
    }

    .envelope:hover {
        transform: scale(1.05);
    }

    .envelope.opened {
        transform: scale(0.8);
        opacity: 0.4;
        pointer-events: none;
        transition: all 0.8s ease;
    }

    .envelope-body {
        width: 100%;
        height: 100%;
        background: linear-gradient(135deg, #fda4af, #fb7185);
        border-radius: 8px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        box-shadow: 0 8px 30px rgba(225, 29, 72, 0.2);
        position: relative;
    }

    .envelope-top {
        position: absolute;
        top: 0;
        left: 0;
        width: 0;
        height: 0;
        border-left: 100px solid transparent;
        border-right: 100px solid transparent;
        border-top: 70px solid #f43f5e;
        z-index: 2;
        transition: transform 0.6s ease;
        transform-origin: top center;
    }

    .envelope-top.open {
        transform: rotateX(180deg);
    }

    .envelope-heart {
        font-size: 2.5rem;
        z-index: 3;
        animation: heartbeat 1.5s ease-in-out infinite;
        border: none;
        background: none;
        cursor: pointer;
        padding: 0;
    }

    .tap-hint {
        font-size: 0.75rem;
        color: white;
        margin-top: 0.5rem;
        opacity: 0.9;
    }

    /* ---- Letter ---- */
    .letter-paper {
        padding: 2.5rem;
        width: 100%;
        position: relative;
        box-shadow: 0 10px 40px rgba(225, 29, 72, 0.1);
    }

    .letter-decoration-top,
    .letter-decoration-bottom {
        text-align: center;
        color: #fda4af;
        font-size: 0.9rem;
        letter-spacing: 2px;
    }

    .letter-decoration-top {
        margin-bottom: 1.5rem;
    }

    .letter-decoration-bottom {
        margin-top: 1.5rem;
    }

    .letter-body {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    .letter-text {
        font-size: 1.25rem;
        color: #881337;
        line-height: 1.8;
        opacity: 0;
        animation-fill-mode: forwards;
    }

    .letter-greeting {
        font-size: 1.6rem;
        font-weight: 600;
        color: #e11d48;
    }

    .letter-closing {
        font-weight: 600;
        color: #e11d48;
        margin-top: 0.5rem;
    }

    /* ---- Navigation ---- */
    .letter-nav {
        opacity: 0;
        animation-fill-mode: forwards;
    }

    .next-button {
        display: inline-flex;
        align-items: center;
        gap: 0.5rem;
        padding: 0.85rem 2rem;
        background: linear-gradient(135deg, #e11d48, #f43f5e);
        color: white;
        text-decoration: none;
        font-family: var(--font-body);
        font-weight: 600;
        font-size: 0.95rem;
        border-radius: 50px;
        transition: all 0.3s ease;
        box-shadow: 0 4px 20px rgba(225, 29, 72, 0.3);
    }

    .next-button:hover {
        transform: translateY(-2px);
        box-shadow: 0 8px 30px rgba(225, 29, 72, 0.4);
    }

    @media (max-width: 640px) {
        .letter-paper {
            padding: 1.5rem;
        }
        .letter-text {
            font-size: 1.1rem;
        }
        .letter-greeting {
            font-size: 1.35rem;
        }
        .envelope {
            width: 160px;
            height: 112px;
        }
        .envelope-top {
            border-left: 80px solid transparent;
            border-right: 80px solid transparent;
            border-top: 56px solid #f43f5e;
        }
    }
</style>
