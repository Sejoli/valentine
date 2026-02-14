<script lang="ts">
  import { onMount } from 'svelte';

  interface Heart {
    id: number;
    emoji: string;
    left: number;
    size: number;
    duration: number;
    delay: number;
    opacity: number;
  }

  let hearts: Heart[] = [];
  const emojis = ['❤️', '💕', '💖', '💗', '💝', '🥰', '💘', '✨', '🌹', '💞'];

  onMount(() => {
    hearts = Array.from({ length: 20 }, (_, i) => ({
      id: i,
      emoji: emojis[Math.floor(Math.random() * emojis.length)],
      left: Math.random() * 100,
      size: 0.6 + Math.random() * 1.2,
      duration: 8 + Math.random() * 12,
      delay: Math.random() * 10,
      opacity: 0.3 + Math.random() * 0.5,
    }));
  });
</script>

<div class="floating-hearts" aria-hidden="true">
  {#each hearts as heart (heart.id)}
    <span
      class="heart"
      style="
        left: {heart.left}%;
        font-size: {heart.size}rem;
        animation-duration: {heart.duration}s;
        animation-delay: {heart.delay}s;
        opacity: {heart.opacity};
      "
    >
      {heart.emoji}
    </span>
  {/each}
</div>

<style>
  .floating-hearts {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }

  .heart {
    position: absolute;
    bottom: -10%;
    animation: floatHeart linear infinite;
    filter: drop-shadow(0 0 4px rgba(225, 29, 72, 0.3));
  }
</style>
