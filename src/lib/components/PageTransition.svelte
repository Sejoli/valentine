<script lang="ts">
  import type { Snippet } from 'svelte';
  import { fly, fade } from 'svelte/transition';
  import { page } from '$app/state';

  interface Props {
    children: Snippet;
  }

  let { children }: Props = $props();
</script>

{#key page.url.pathname}
  <div
    class="page-transition"
    in:fly={{ y: 30, duration: 500, delay: 200 }}
    out:fade={{ duration: 200 }}
  >
    {@render children()}
  </div>
{/key}

<style>
  .page-transition {
    min-height: 100vh;
  }
</style>
