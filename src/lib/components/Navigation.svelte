<script lang="ts">
    let isOpen = $state(false);

    const navItems = [
        { href: "/", label: "Home", emoji: "💕" },
        { href: "/love-letter", label: "Love Letter", emoji: "💌" },
        { href: "/reasons", label: "Alasan", emoji: "💖" },
        { href: "/moments", label: "Momen Kita", emoji: "📸" },
        { href: "/surprise", label: "Kejutan", emoji: "🎉" },
    ];

    function closeMenu() {
        isOpen = false;
    }
</script>

<nav class="nav-container">
    <div class="nav-inner">
        <a href="/" class="nav-logo font-cursive" onclick={closeMenu}>
            💕 Valentine
        </a>

        <button
            class="nav-hamburger"
            onclick={() => (isOpen = !isOpen)}
            aria-label="Menu"
        >
            <span class="hamburger-line" class:open={isOpen}></span>
            <span class="hamburger-line" class:open={isOpen}></span>
            <span class="hamburger-line" class:open={isOpen}></span>
        </button>

        <div class="nav-links" class:open={isOpen}>
            {#each navItems as item}
                <a
                    href={item.href}
                    class="nav-link font-body"
                    onclick={closeMenu}
                >
                    <span class="nav-emoji">{item.emoji}</span>
                    {item.label}
                </a>
            {/each}
        </div>
    </div>
</nav>

<style>
    .nav-container {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        z-index: 100;
        background: rgba(255, 241, 242, 0.85);
        backdrop-filter: blur(16px);
        -webkit-backdrop-filter: blur(16px);
        border-bottom: 1px solid rgba(225, 29, 72, 0.1);
    }

    .nav-inner {
        max-width: 1200px;
        margin: 0 auto;
        padding: 0.75rem 1.5rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .nav-logo {
        font-size: 1.5rem;
        font-weight: 700;
        color: #e11d48;
        text-decoration: none;
        transition: transform 0.3s ease;
    }

    .nav-logo:hover {
        transform: scale(1.05);
    }

    .nav-hamburger {
        display: none;
        flex-direction: column;
        gap: 5px;
        background: none;
        border: none;
        cursor: pointer;
        padding: 4px;
        z-index: 101;
    }

    .hamburger-line {
        width: 24px;
        height: 2px;
        background: #e11d48;
        border-radius: 2px;
        transition: all 0.3s ease;
    }

    .hamburger-line.open:nth-child(1) {
        transform: rotate(45deg) translate(5px, 5px);
    }
    .hamburger-line.open:nth-child(2) {
        opacity: 0;
    }
    .hamburger-line.open:nth-child(3) {
        transform: rotate(-45deg) translate(5px, -5px);
    }

    .nav-links {
        display: flex;
        align-items: center;
        gap: 0.25rem;
    }

    .nav-link {
        text-decoration: none;
        color: #881337;
        font-size: 0.875rem;
        font-weight: 500;
        padding: 0.5rem 1rem;
        border-radius: 12px;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        gap: 0.35rem;
    }

    .nav-link:hover {
        background: rgba(225, 29, 72, 0.1);
        color: #e11d48;
        transform: translateY(-1px);
    }

    .nav-emoji {
        font-size: 1rem;
    }

    @media (max-width: 768px) {
        .nav-inner {
            padding: 0.5rem 1rem;
        }

        .nav-logo {
            font-size: 1.25rem;
        }

        .nav-hamburger {
            display: flex;
        }

        .nav-links {
            position: fixed;
            top: 0;
            right: -100%;
            width: 70%;
            max-width: 300px;
            height: 100vh;
            background: rgba(255, 241, 242, 0.98);
            backdrop-filter: blur(20px);
            flex-direction: column;
            justify-content: center;
            gap: 0.5rem;
            padding: 2rem;
            transition: right 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: -4px 0 30px rgba(225, 29, 72, 0.1);
        }

        .nav-links.open {
            right: 0;
        }

        .nav-link {
            font-size: 1.1rem;
            padding: 0.75rem 1.5rem;
            width: 100%;
        }
    }
</style>
