<script lang="ts">
	import Button from '@/components/Button.svelte';
	export let name: string;
</script>

<template lang="pug">
	main
		h1 Hello {name}!
		p Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.
		<Button/>
</template>



<style lang="scss">
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: $primary-color;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		user-select: none;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>