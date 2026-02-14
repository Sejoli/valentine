<script lang="ts">
  import { onMount } from "svelte";

  let isPlaying = $state(false);
  let pulseAnimation = $state(false);
  let audio: HTMLAudioElement;

  onMount(() => {
    audio = new Audio("/music/bg-music.mp4");
    audio.loop = true;
    audio.volume = 0.4;

    // Preload
    audio.preload = "auto";
    audio.load();
  });

  function toggleMusic() {
    if (isPlaying) {
      audio.pause();
      isPlaying = false;
      pulseAnimation = false;
    } else {
      audio
        .play()
        .then(() => {
          isPlaying = true;
          pulseAnimation = true;
        })
        .catch((err) => {
          console.warn("Audio play failed:", err);
          alert("Pastikan file musik ada di static/music/bg-music.mp3");
        });
    }
  }
</script>

<button
  class="music-toggle"
  class:playing={pulseAnimation}
  onclick={toggleMusic}
  aria-label={isPlaying ? "Pause music" : "Play music"}
  title={isPlaying ? "Pause musik 🔇" : "Play musik 🎵"}
>
  {isPlaying ? "🎵" : "🔇"}
</button>

<style>
  .music-toggle {
    position: fixed;
    bottom: 24px;
    right: 24px;
    z-index: 1000;
    width: 56px;
    height: 56px;
    border-radius: 50%;
    border: 2px solid rgba(225, 29, 72, 0.3);
    background: rgba(255, 255, 255, 0.85);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    font-size: 1.5rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    box-shadow: 0 4px 20px rgba(225, 29, 72, 0.15);
  }

  .music-toggle:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 30px rgba(225, 29, 72, 0.25);
    border-color: rgba(225, 29, 72, 0.5);
  }

  .music-toggle.playing {
    animation: pulseGlow 2s ease-in-out infinite;
    border-color: rgba(225, 29, 72, 0.5);
  }
</style>
