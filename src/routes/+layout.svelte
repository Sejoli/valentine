<script lang="ts">
	import "../app.css";
	import FloatingHearts from "$lib/components/FloatingHearts.svelte";
	import MusicToggle from "$lib/components/MusicToggle.svelte";
	import Navigation from "$lib/components/Navigation.svelte";
	import { fly, fade } from "svelte/transition";
	import { page } from "$app/state";
	import type { Snippet } from "svelte";

	interface Props {
		children: Snippet;
	}

	let { children }: Props = $props();
</script>

<svelte:head>
	<title>💕 Happy Valentine's Day, Sayang!</title>
	<meta
		name="description"
		content="A special Valentine's surprise just for you 💕"
	/>
</svelte:head>

<div class="app-wrapper">
	<FloatingHearts />
	<Navigation />
	<main>
		{#key page.url.pathname}
			<div
				class="page-transition"
				in:fly={{ y: 30, duration: 500, delay: 200 }}
				out:fade={{ duration: 200 }}
			>
				{@render children()}
			</div>
		{/key}
	</main>
	<MusicToggle />
</div>

<style>
	.app-wrapper {
		min-height: 100vh;
		position: relative;
		background: linear-gradient(
			135deg,
			#fff1f2 0%,
			#fce7f3 30%,
			#fdf2f8 60%,
			#fff1f2 100%
		);
	}

	main {
		position: relative;
		z-index: 1;
		padding-top: 60px;
	}

	.page-transition {
		min-height: 100vh;
	}
</style>
