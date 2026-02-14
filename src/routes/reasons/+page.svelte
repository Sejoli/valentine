<script lang="ts">
    import { onMount } from "svelte";

    interface Reason {
        emoji: string;
        title: string;
        description: string;
        flipped: boolean;
    }

    let reasons: Reason[] = $state([
        {
            emoji: "😍",
            title: "Suka Cerita",
            description:
                "Kamu selalu cerita sama aku, dan aku suka mendengarkannya ",
            flipped: false,
        },
        {
            emoji: "🤗",
            title: "Pelukan Hangatmu",
            description:
                "Setiap kali kamu meluk aku, aku merasa nyaman,aman dan hangat..yah walau g bisa tidur sih",
            flipped: false,
        },
        {
            emoji: "💪",
            title: "Kamu Kuat",
            description:
                "yang ini g bisa dilawan. main gelitikpun aku kalah sangking kuatnya, tapi aku suka",
            flipped: false,
        },
        {
            emoji: "🥘",
            title: "Masakanmu",
            description:
                "aku suka setiap masakan yang kamu buat. asin sedikit gak ngaruh sih hehe",
            flipped: false,
        },
        {
            emoji: "🎯",
            title: "Kamu Perhatian",
            description:
                "Kamu selalu ingat hal-hal kecil tentang aku yang bahkan aku sendiri lupa. Itu sangat berarti.",
            flipped: false,
        },

        {
            emoji: "🧸",
            title: "Kamu Lembut",
            description:
                "Kelembutan hatimu itu langka. Cara kamu peduli sama orang lain, samaku bikin aku makin cinta.",
            flipped: false,
        },
        {
            emoji: "🌙",
            title: "Teman Curhat",
            description:
                "Kamu pendengar terbaik. Aku bisa cerita apapun Kamu safe space-ku.",
            flipped: false,
        },
        {
            emoji: "💕",
            title: "Kamu adalah Kamu",
            description:
                "Alasan paling utama: karena kamu adalah kamu. Nggak perlu jadi siapa-siapa lagi. Kamu sudah sempurna di mataku.",
            flipped: false,
        },
    ]);

    let flippedCount = $state(0);
    let showCelebration = $state(false);
    let visible = $state(false);

    function flipCard(index: number) {
        if (!reasons[index].flipped) {
            reasons[index].flipped = true;
            flippedCount++;
            if (flippedCount === reasons.length) {
                setTimeout(() => (showCelebration = true), 500);
            }
        }
    }

    onMount(() => {
        setTimeout(() => (visible = true), 200);
    });
</script>

<section class="reasons-page">
    <div class="reasons-container">
        <!-- Header -->
        <div class="reasons-header" class:visible>
            <h1 class="reasons-title font-heading">
                <span class="title-emoji">💖</span>
                08 Alasan Kenapa Aku Sayang Kamu
                <span class="title-emoji">💖</span>
            </h1>
            <p class="reasons-subtitle font-body">
                Klik setiap kartu untuk membuka alasannya 🥰
            </p>
            <div class="counter font-body">
                <span class="counter-heart">💗</span>
                Kamu sudah buka <strong>{flippedCount}</strong> dari
                <strong>{reasons.length}</strong> alasan
            </div>
        </div>

        <!-- Cards Grid -->
        <div class="cards-grid">
            {#each reasons as reason, i}
                <div
                    class="flip-card"
                    class:flipped={reason.flipped}
                    class:visible
                    style="animation-delay: {0.3 + i * 0.1}s"
                    onclick={() => flipCard(i)}
                    role="button"
                    tabindex="0"
                    onkeydown={(e) => {
                        if (e.key === "Enter" || e.key === " ") flipCard(i);
                    }}
                >
                    <div class="flip-card-inner">
                        <!-- Front -->
                        <div class="flip-card-front">
                            <span class="card-number font-heading">{i + 1}</span
                            >
                            <span class="card-question">❓</span>
                            <p class="card-tap font-body">Klik aku!</p>
                        </div>
                        <!-- Back -->
                        <div class="flip-card-back">
                            <span class="card-emoji">{reason.emoji}</span>
                            <h3 class="card-title font-heading">
                                {reason.title}
                            </h3>
                            <p class="card-description font-body">
                                {reason.description}
                            </p>
                        </div>
                    </div>
                </div>
            {/each}
        </div>

        <!-- Celebration -->
        {#if showCelebration}
            <div class="celebration animate-fade-in-up">
                <p class="celebration-text font-cursive">
                    🎉 Kamu sudah buka semua! Tapi alasan aku sayang kamu
                    sebenarnya nggak terhitung 🥰
                </p>
                <a href="/moments" class="next-button">
                    Lihat Momen Kita Bersama →
                    <span>📸</span>
                </a>
            </div>
        {/if}
    </div>
</section>

<style>
    .reasons-page {
        min-height: 100vh;
        padding: 2rem;
        padding-top: 80px;
        padding-bottom: 100px;
    }

    .reasons-container {
        max-width: 900px;
        margin: 0 auto;
    }

    /* ---- Header ---- */
    .reasons-header {
        text-align: center;
        margin-bottom: 2.5rem;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.6s ease;
    }

    .reasons-header.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .reasons-title {
        font-size: 2rem;
        color: #e11d48;
        font-weight: 700;
        margin-bottom: 0.5rem;
        line-height: 1.4;
    }

    .title-emoji {
        display: inline-block;
        animation: heartbeat 1.5s ease-in-out infinite;
    }

    .reasons-subtitle {
        color: #881337;
        font-size: 1rem;
        margin-bottom: 1rem;
    }

    .counter {
        display: inline-flex;
        align-items: center;
        gap: 0.5rem;
        padding: 0.5rem 1.25rem;
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(8px);
        border-radius: 50px;
        font-size: 0.9rem;
        color: #881337;
        border: 1px solid rgba(225, 29, 72, 0.15);
    }

    .counter-heart {
        animation: heartbeat 1.5s ease-in-out infinite;
    }

    /* ---- Cards Grid ---- */
    .cards-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
        gap: 1.25rem;
    }

    /* ---- Flip Card ---- */
    .flip-card {
        perspective: 1000px;
        height: 220px;
        cursor: pointer;
        opacity: 0;
        transform: translateY(30px);
        transition:
            opacity 0.5s ease,
            transform 0.5s ease;
    }

    .flip-card.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .flip-card-inner {
        position: relative;
        width: 100%;
        height: 100%;
        transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        transform-style: preserve-3d;
    }

    .flip-card.flipped .flip-card-inner {
        transform: rotateY(180deg);
    }

    .flip-card-front,
    .flip-card-back {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;
        border-radius: 16px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 1.5rem;
    }

    .flip-card-front {
        background: linear-gradient(135deg, #fda4af, #fb7185);
        color: white;
        gap: 0.5rem;
        box-shadow: 0 6px 25px rgba(225, 29, 72, 0.2);
    }

    .flip-card-front:hover {
        box-shadow: 0 10px 35px rgba(225, 29, 72, 0.35);
    }

    .card-number {
        font-size: 2rem;
        font-weight: 700;
        opacity: 0.9;
    }

    .card-question {
        font-size: 1.5rem;
        animation: float 3s ease-in-out infinite;
    }

    .card-tap {
        font-size: 0.8rem;
        opacity: 0.8;
    }

    .flip-card-back {
        background: white;
        transform: rotateY(180deg);
        gap: 0.5rem;
        border: 2px solid rgba(225, 29, 72, 0.1);
        box-shadow: 0 6px 25px rgba(225, 29, 72, 0.12);
    }

    .card-emoji {
        font-size: 2.5rem;
    }

    .card-title {
        font-size: 1.1rem;
        font-weight: 600;
        color: #e11d48;
    }

    .card-description {
        font-size: 0.85rem;
        color: #881337;
        line-height: 1.5;
        text-align: center;
    }

    /* ---- Celebration ---- */
    .celebration {
        text-align: center;
        margin-top: 2.5rem;
        padding: 2rem;
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(10px);
        border-radius: 20px;
        border: 1px solid rgba(225, 29, 72, 0.1);
    }

    .celebration-text {
        font-size: 1.3rem;
        color: #881337;
        margin-bottom: 1.5rem;
        line-height: 1.6;
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
        .reasons-title {
            font-size: 1.5rem;
        }
        .cards-grid {
            grid-template-columns: repeat(2, 1fr);
            gap: 0.75rem;
        }
        .flip-card {
            height: 190px;
        }
        .card-description {
            font-size: 0.75rem;
        }
    }
</style>
