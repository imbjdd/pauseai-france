<script lang="ts">
	import { slide } from 'svelte/transition'
	import { page } from '$app/stores'

	export let open = false
	export let id: string

	const details_id = `${id}-details`
	const title_id = `${id}-title`
	const handleClick = () => (open = !open)

	$: if ($page.url.hash === `#${id}`) {
		open = true
	}
</script>

<div class="accordion" {id}>
	<button on:click={handleClick} class="header" aria-expanded={open} aria-controls={details_id}>
		<h3 class="title" id={title_id}>
			<slot name="head" />
		</h3>

		<span class="icon">{open ? '\u2212' : '+'}</span>
	</button>

	{#if open}
		<div class="details" transition:slide id={details_id} aria-labelledby={title_id}>
			<slot name="details" />
		</div>
	{/if}
</div>

<style>
	.accordion:global(:not(:last-child)) {
		border-bottom: solid 2px #e6e6e6;
	}

	.header {
		cursor: pointer;
		display: flex;
		width: 100%;
		border: none;
		background-color: transparent;
		text-align: left;
		align-items: center;
		padding: 1.5rem 0;
	}

	.header .title {
		flex: 1;
	}

	.details {
		padding: 2rem 0 1.5rem 0;
	}

	.icon {
		font-size: 3rem;
		font-weight: 200;
		line-height: 0;
		margin-left: 1rem;
	}
</style>
