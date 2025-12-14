<script lang="ts">
	import type { Snippet } from 'svelte';

	type Props = {
		debug?: boolean;
		disabled?: boolean;
		onclick: () => any;
		children: Snippet;
		'data-test'?: string;
	};

	const { debug, disabled = false, onclick, children, ...rest }: Props = $props();
</script>

<button class="button" class:debug class:disabled {disabled} {onclick} {...rest}>
	{@render children()}
</button>

<style lang="scss">
	.button {
		width: 100%;
		position: relative;
		font-family: 'proxima-nova', sans-serif;
		cursor: pointer;
		display: inline-flex;
		justify-content: center;
		background-color: transparent;
		border-color: transparent;
		padding: 0;
		transition:
			transform 0.15s ease-out,
			filter 0.15s ease-out;

		&:hover:not(.disabled) {
			transform: scale(1.02);
			filter: brightness(1.1);
		}

		&:active:not(.disabled) {
			transform: scale(0.98);
			filter: brightness(0.95);
		}
	}

	.button.debug {
		border-color: white;
		border-width: 2px;
	}

	.button.disabled {
		cursor: not-allowed;
		opacity: 0.5;
	}
</style>
