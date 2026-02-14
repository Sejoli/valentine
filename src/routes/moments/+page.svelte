<script lang="ts">
    import { onMount } from "svelte";

    interface Moment {
        date: string;
        title: string;
        description: string;
        emoji: string;
        photo?: string;
        visible: boolean;
    }

    interface Photo {
        src: string;
        caption: string;
        visible: boolean;
    }

    let moments: Moment[] = $state([
        {
            date: "Hari Pertama",
            title: "Pertama Kali Ketemu",
            emoji: "👋",
            photo: "/photos/1.jpeg",
            description: "dikala suka maupun duka dirimu selalu ada",
            visible: false,
        },
        {
            date: "Minggu Pertama",
            title: "Chat Pertama",
            emoji: "💬",
            photo: "/photos/2.jpeg",
            description: "tidak pernah berubah sayangmu, cintamu",
            visible: false,
        },
        {
            date: "Bulan Pertama",
            title: "Date Pertama",
            emoji: "🍽️",
            photo: "/photos/3.jpeg",
            description: "menjadi orang yang spesial dimatamu.",
            visible: false,
        },
        {
            date: "Momen Spesial",
            title: "Jadian! 💕",
            emoji: "💑",
            photo: "/photos/4.jpeg",
            description: " selalu memandangku.",
            visible: false,
        },
        {
            date: "Seiring Waktu",
            title: "Pertengkaran Pertama",
            emoji: "😤",
            photo: "/photos/5.jpeg",
            description: " cintamu yang begitu hangat",
            visible: false,
        },
        {
            date: "Momen Indah",
            title: "Liburan Bersama",
            emoji: "🏖️",
            photo: "/photos/6.jpeg",
            description: "selalu memberikan yang terbaik kepadaku.",
            visible: false,
        },
        {
            date: "Setiap Hari",
            title: "Hal-Hal Kecil",
            emoji: "☕",
            photo: "/photos/7.jpeg",
            description: "memanjakanku",
            visible: false,
        },
        {
            date: "Hari Ini",
            title: "Valentine Bersama",
            emoji: "❤️",
            photo: "/photos/8.jpeg",
            description:
                "Aku mau bilang... aku bersyukur setiap hari karena punya kamu. Happy Valentine Yunilaku 🥰",
            visible: false,
        },
    ]);

    // Photo gallery data — tambahkan foto-foto kalian di sini!
    let photos: Photo[] = $state([
        {
            src: "/photos/gallery-1.jpeg",
            caption: "foto sambil keluarin lidah 💕",
            visible: false,
        },
        {
            src: "/photos/gallery-2.jpeg",
            caption: "senyum tipis dulu 😍",
            visible: false,
        },
        {
            src: "/photos/gallery-3.jpeg",
            caption: "Marry christmas yunilaku ✨",
            visible: false,
        },
        {
            src: "/photos/gallery-4.jpeg",
            caption: "salam dua jari 💖",
            visible: false,
        },
        {
            src: "/photos/gallery-5.jpeg",
            caption: "habis nonton film dewasa 🥰",
            visible: false,
        },
        {
            src: "/photos/gallery-6.jpeg",
            caption: "Kamu dan aku 💕",
            visible: false,
        },
        {
            src: "/photos/gallery-7.jpeg",
            caption: "ngepantai dulu 💗",
            visible: false,
        },
        {
            src: "/photos/gallery-8.jpeg",
            caption: "Bersama selamanya ❤️",
            visible: false,
        },
        {
            src: "/photos/gallery-9.jpeg",
            caption: "selamat ulangtahun sayangku 🌹",
            visible: false,
        },
        {
            src: "/photos/gallery-10.jpeg",
            caption: "jangan lupa pakai kacamata 🌹",
            visible: false,
        },
        {
            src: "/photos/gallery-11.jpeg",
            caption: "kembar ya kita 🌹",
            visible: false,
        },
    ]);

    let headerVisible = $state(false);
    let galleryHeaderVisible = $state(false);
    let lightboxOpen = $state(false);
    let lightboxSrc = $state("");
    let lightboxCaption = $state("");
    let lightboxIndex = $state(0);
    let photoErrors: Set<string> = $state(new Set());

    function openLightbox(index: number) {
        lightboxIndex = index;
        lightboxSrc = photos[index].src;
        lightboxCaption = photos[index].caption;
        lightboxOpen = true;
        document.body.style.overflow = "hidden";
    }

    function closeLightbox() {
        lightboxOpen = false;
        document.body.style.overflow = "";
    }

    function nextPhoto() {
        lightboxIndex = (lightboxIndex + 1) % photos.length;
        lightboxSrc = photos[lightboxIndex].src;
        lightboxCaption = photos[lightboxIndex].caption;
    }

    function prevPhoto() {
        lightboxIndex = (lightboxIndex - 1 + photos.length) % photos.length;
        lightboxSrc = photos[lightboxIndex].src;
        lightboxCaption = photos[lightboxIndex].caption;
    }

    function handleKeydown(e: KeyboardEvent) {
        if (!lightboxOpen) return;
        if (e.key === "Escape") closeLightbox();
        if (e.key === "ArrowRight") nextPhoto();
        if (e.key === "ArrowLeft") prevPhoto();
    }

    function handlePhotoError(src: string) {
        photoErrors = new Set([...photoErrors, src]);
    }

    onMount(() => {
        headerVisible = true;

        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    if (entry.isIntersecting) {
                        const index = entry.target.getAttribute("data-index");
                        const type = entry.target.getAttribute("data-type");

                        if (type === "moment" && index) {
                            moments[parseInt(index)].visible = true;
                        } else if (type === "photo" && index) {
                            photos[parseInt(index)].visible = true;
                        } else if (type === "gallery-header") {
                            galleryHeaderVisible = true;
                        }

                        observer.unobserve(entry.target);
                    }
                });
            },
            { threshold: 0.15, rootMargin: "0px 0px -30px 0px" },
        );

        setTimeout(() => {
            document.querySelectorAll("[data-type]").forEach((el) => {
                observer.observe(el);
            });
        }, 100);

        return () => observer.disconnect();
    });
</script>

<svelte:window onkeydown={handleKeydown} />

<section class="moments-page">
    <div class="moments-container">
        <!-- Header -->
        <div class="moments-header" class:visible={headerVisible}>
            <h1 class="moments-title font-heading">📸 Momen-Momen Kita</h1>
        </div>

        <!-- Timeline -->
        <div class="timeline">
            <div class="timeline-line" aria-hidden="true"></div>

            {#each moments as moment, i}
                <div
                    class="timeline-item"
                    class:visible={moment.visible}
                    class:left={i % 2 === 0}
                    class:right={i % 2 !== 0}
                    data-index={i}
                    data-type="moment"
                >
                    <div class="timeline-dot" aria-hidden="true">
                        <span class="dot-emoji">{moment.emoji}</span>
                    </div>

                    <div class="timeline-card glass-card">
                        {#if moment.photo && !photoErrors.has(moment.photo)}
                            <div class="card-photo-wrapper">
                                <img
                                    src={moment.photo}
                                    alt={moment.description}
                                    class="card-photo"
                                    loading="lazy"
                                    onerror={() =>
                                        handlePhotoError(moment.photo!)}
                                />
                            </div>
                        {/if}
                        <p class="card-description font-body">
                            {moment.description}
                        </p>
                    </div>
                </div>
            {/each}
        </div>

        <!-- Photo Gallery Section -->
        <div
            class="gallery-header"
            class:visible={galleryHeaderVisible}
            data-type="gallery-header"
            data-index="0"
        >
            <div class="gallery-divider" aria-hidden="true">
                ✿ ─────── 📷 ─────── ✿
            </div>
            <h2 class="gallery-title font-heading">Galeri Foto Kita 💕</h2>
        </div>

        <div class="photo-grid">
            {#each photos as photo, i}
                {#if !photoErrors.has(photo.src)}
                    <button
                        class="photo-item"
                        class:visible={photo.visible}
                        data-index={i}
                        data-type="photo"
                        onclick={() => openLightbox(i)}
                        style="animation-delay: {i * 0.08}s"
                    >
                        <div class="photo-wrapper">
                            <img
                                src={photo.src}
                                alt={photo.caption}
                                class="photo-img"
                                loading="lazy"
                                onerror={() => handlePhotoError(photo.src)}
                            />
                            <div class="photo-overlay">
                                <span class="photo-caption font-body"
                                    >{photo.caption}</span
                                >
                            </div>
                        </div>
                    </button>
                {/if}
            {/each}
        </div>

        <!-- Info box when no photos found -->
        <div class="photo-info glass-card">
            <p class="info-emoji">📷</p>
            <p class="info-text font-body">
                <strong>Cara menambahkan foto:</strong><br />
                Taruh file foto kamu di folder <code>static/photos/</code><br />
                Beri nama: <code>1.jpg</code>, <code>2.jpg</code>, ... (untuk
                timeline)<br />
                dan <code>gallery-1.jpg</code>, <code>gallery-2.jpg</code>, ...
                (untuk galeri)
            </p>
        </div>

        <!-- Navigation -->
        <div
            class="moments-nav"
            class:visible={moments[moments.length - 1]?.visible}
        >
            <a href="/surprise" class="next-button">
                Satu Kejutan Lagi... →
                <span>🎉</span>
            </a>
        </div>
    </div>
</section>

<!-- Lightbox -->
{#if lightboxOpen}
    <div
        class="lightbox"
        role="dialog"
        aria-modal="true"
        aria-label="Photo viewer"
    >
        <button
            class="lightbox-backdrop"
            onclick={closeLightbox}
            aria-label="Close"
        ></button>
        <div class="lightbox-content">
            <button
                class="lightbox-close"
                onclick={closeLightbox}
                aria-label="Close photo">✕</button
            >

            <button
                class="lightbox-nav lightbox-prev"
                onclick={prevPhoto}
                aria-label="Previous photo"
            >
                ‹
            </button>

            <div class="lightbox-image-container">
                <img
                    src={lightboxSrc}
                    alt={lightboxCaption}
                    class="lightbox-img"
                />
                <p class="lightbox-caption font-cursive">{lightboxCaption}</p>
                <p class="lightbox-counter font-body">
                    {lightboxIndex + 1} / {photos.length}
                </p>
            </div>

            <button
                class="lightbox-nav lightbox-next"
                onclick={nextPhoto}
                aria-label="Next photo"
            >
                ›
            </button>
        </div>
    </div>
{/if}

<style>
    .moments-page {
        min-height: 100vh;
        padding: 2rem;
        padding-top: 80px;
        padding-bottom: 100px;
    }

    .moments-container {
        max-width: 800px;
        margin: 0 auto;
    }

    /* ---- Header ---- */
    .moments-header {
        text-align: center;
        margin-bottom: 3rem;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.6s ease;
    }

    .moments-header.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .moments-title {
        font-size: 2rem;
        color: #e11d48;
        font-weight: 700;
        margin-bottom: 0.5rem;
    }

    /* ---- Timeline ---- */
    .timeline {
        position: relative;
        padding: 1rem 0;
    }

    .timeline-line {
        position: absolute;
        left: 50%;
        top: 0;
        bottom: 0;
        width: 3px;
        background: linear-gradient(to bottom, #fda4af, #e11d48, #d97706);
        transform: translateX(-50%);
        border-radius: 3px;
    }

    .timeline-item {
        position: relative;
        display: flex;
        align-items: center;
        margin-bottom: 2rem;
        opacity: 0;
        transition: all 0.7s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .timeline-item.left {
        flex-direction: row;
        padding-right: calc(50% + 30px);
        transform: translateX(-40px);
    }

    .timeline-item.right {
        flex-direction: row-reverse;
        padding-left: calc(50% + 30px);
        transform: translateX(40px);
    }

    .timeline-item.visible {
        opacity: 1;
        transform: translateX(0);
    }

    .timeline-dot {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        width: 48px;
        height: 48px;
        background: white;
        border: 3px solid #e11d48;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 2;
        box-shadow: 0 4px 15px rgba(225, 29, 72, 0.2);
    }

    .dot-emoji {
        font-size: 1.3rem;
    }

    .timeline-card {
        padding: 1.5rem;
        flex: 1;
        box-shadow: 0 6px 25px rgba(225, 29, 72, 0.1);
        overflow: hidden;
    }

    .card-photo-wrapper {
        margin: -1.5rem -1.5rem 1rem -1.5rem;
        overflow: hidden;
        border-radius: 16px 16px 0 0;
        max-height: 200px;
    }

    .card-photo {
        width: 100%;
        height: 200px;
        object-fit: cover;
        display: block;
        transition: transform 0.5s ease;
    }

    .timeline-card:hover .card-photo {
        transform: scale(1.05);
    }

    .card-description {
        font-size: 0.9rem;
        color: #881337;
        line-height: 1.6;
    }

    /* ---- Photo Gallery ---- */
    .gallery-header {
        text-align: center;
        margin-top: 4rem;
        margin-bottom: 2rem;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.6s ease;
    }

    .gallery-header.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .gallery-divider {
        color: #fda4af;
        font-size: 0.85rem;
        letter-spacing: 2px;
        margin-bottom: 1rem;
    }

    .gallery-title {
        font-size: 1.8rem;
        color: #e11d48;
        font-weight: 700;
        margin-bottom: 0.5rem;
    }

    .photo-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 0.75rem;
        margin-bottom: 2rem;
    }

    .photo-item {
        border: none;
        padding: 0;
        background: none;
        cursor: pointer;
        opacity: 0;
        transform: translateY(20px) scale(0.95);
        transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .photo-item.visible {
        opacity: 1;
        transform: translateY(0) scale(1);
    }

    .photo-wrapper {
        position: relative;
        border-radius: 12px;
        overflow: hidden;
        aspect-ratio: 1 / 1;
        box-shadow: 0 4px 20px rgba(225, 29, 72, 0.12);
        transition: all 0.3s ease;
    }

    .photo-item:hover .photo-wrapper {
        transform: translateY(-4px);
        box-shadow: 0 10px 35px rgba(225, 29, 72, 0.25);
    }

    .photo-img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        display: block;
        transition: transform 0.5s ease;
    }

    .photo-item:hover .photo-img {
        transform: scale(1.08);
    }

    .photo-overlay {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 2.5rem 0.75rem 0.75rem;
        background: linear-gradient(to top, rgba(0, 0, 0, 0.6), transparent);
        opacity: 0;
        transition: opacity 0.3s ease;
    }

    .photo-item:hover .photo-overlay {
        opacity: 1;
    }

    .photo-caption {
        color: white;
        font-size: 0.8rem;
        font-weight: 500;
        text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
    }

    /* ---- Photo Info ---- */
    .photo-info {
        text-align: center;
        padding: 1.5rem;
        margin-bottom: 2rem;
        border: 2px dashed rgba(225, 29, 72, 0.2);
        background: rgba(255, 255, 255, 0.5);
    }

    .info-emoji {
        font-size: 2rem;
        margin-bottom: 0.5rem;
    }

    .info-text {
        font-size: 0.85rem;
        color: #881337;
        line-height: 1.8;
    }

    .info-text code {
        background: rgba(225, 29, 72, 0.08);
        padding: 0.15rem 0.4rem;
        border-radius: 4px;
        font-size: 0.8rem;
        color: #e11d48;
    }

    /* ---- Lightbox ---- */
    .lightbox {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index: 2000;
        display: flex;
        align-items: center;
        justify-content: center;
        animation: fadeIn 0.3s ease;
    }

    .lightbox-backdrop {
        position: absolute;
        inset: 0;
        background: rgba(0, 0, 0, 0.85);
        backdrop-filter: blur(10px);
        border: none;
        cursor: pointer;
    }

    .lightbox-content {
        position: relative;
        z-index: 1;
        display: flex;
        align-items: center;
        gap: 1rem;
        max-width: 90vw;
        max-height: 90vh;
    }

    .lightbox-close {
        position: absolute;
        top: -40px;
        right: 0;
        background: none;
        border: none;
        color: white;
        font-size: 1.8rem;
        cursor: pointer;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        transition: all 0.3s ease;
    }

    .lightbox-close:hover {
        background: rgba(255, 255, 255, 0.15);
        transform: scale(1.1);
    }

    .lightbox-nav {
        background: rgba(255, 255, 255, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.2);
        color: white;
        font-size: 2rem;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.3s ease;
        flex-shrink: 0;
    }

    .lightbox-nav:hover {
        background: rgba(255, 255, 255, 0.25);
        transform: scale(1.1);
    }

    .lightbox-image-container {
        text-align: center;
        max-width: 70vw;
        max-height: 80vh;
    }

    .lightbox-img {
        max-width: 100%;
        max-height: 70vh;
        border-radius: 12px;
        box-shadow: 0 10px 50px rgba(0, 0, 0, 0.5);
        object-fit: contain;
    }

    .lightbox-caption {
        color: white;
        font-size: 1.15rem;
        margin-top: 1rem;
        text-shadow: 0 1px 5px rgba(0, 0, 0, 0.5);
    }

    .lightbox-counter {
        color: rgba(255, 255, 255, 0.6);
        font-size: 0.8rem;
        margin-top: 0.35rem;
    }

    /* ---- Navigation ---- */
    .moments-nav {
        text-align: center;
        margin-top: 2rem;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.6s ease;
    }

    .moments-nav.visible {
        opacity: 1;
        transform: translateY(0);
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

    /* ---- Mobile ---- */
    @media (max-width: 768px) {
        .moments-page {
            padding: 1rem;
            padding-top: 70px;
            padding-bottom: 80px;
        }

        .timeline-line {
            left: 24px;
        }

        .timeline-item.left,
        .timeline-item.right {
            flex-direction: row;
            padding-right: 0;
            padding-left: 60px;
        }

        .timeline-item.left {
            transform: translateX(-30px);
        }

        .timeline-item.right {
            transform: translateX(-30px);
        }

        .timeline-item.visible.left,
        .timeline-item.visible.right {
            transform: translateX(0);
        }

        .timeline-dot {
            left: 24px;
            width: 40px;
            height: 40px;
        }

        .dot-emoji {
            font-size: 1.1rem;
        }

        .moments-title {
            font-size: 1.4rem;
        }

        .timeline-card {
            padding: 0.85rem;
        }

        .card-photo-wrapper {
            margin: -0.85rem -0.85rem 0.75rem -0.85rem;
            border-radius: 12px 12px 0 0;
            max-height: 150px;
        }

        .card-photo {
            height: 150px;
        }

        .card-description {
            font-size: 0.85rem;
        }

        .photo-grid {
            grid-template-columns: repeat(2, 1fr);
            gap: 0.5rem;
        }

        .gallery-title {
            font-size: 1.3rem;
        }

        .lightbox-nav {
            width: 36px;
            height: 36px;
            font-size: 1.5rem;
        }

        .lightbox-image-container {
            max-width: 90vw;
        }

        .lightbox-image-container img {
            max-height: 65vh;
        }

        .next-button {
            padding: 0.75rem 1.5rem;
            font-size: 0.85rem;
        }
    }

    @media (max-width: 380px) {
        .moments-title {
            font-size: 1.2rem;
        }

        .timeline-item.left,
        .timeline-item.right {
            padding-left: 50px;
        }

        .timeline-dot {
            left: 16px;
            width: 34px;
            height: 34px;
        }

        .timeline-line {
            left: 16px;
        }

        .dot-emoji {
            font-size: 0.9rem;
        }

        .card-photo {
            height: 120px;
        }

        .card-photo-wrapper {
            max-height: 120px;
        }

        .photo-grid {
            grid-template-columns: repeat(2, 1fr);
            gap: 0.4rem;
        }
    }
</style>
