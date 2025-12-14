<script lang="ts">
	import { Container, Sprite, Circle } from 'pixi-svelte';
	import { Button, type ButtonProps } from 'components-pixi';
	import { OnHotkey } from 'components-shared';
	import { stateBetDerived } from 'state-shared';

	import ButtonBetProvider from './ButtonBetProvider.svelte';
	import { UI_BASE_SIZE } from '../constants';
	import { getContext } from '../context';

	const props: Partial<Omit<ButtonProps, 'children'>> = $props();
	const context = getContext();
	// Only disable clicks when idle and can't afford bet
	// During play, the button becomes a STOP button (handled by ButtonBetProvider)
	const disabled = $derived(
		context.stateXstateDerived.isIdle() && !stateBetDerived.isBetCostAvailable()
	);
	const sizes = { width: UI_BASE_SIZE, height: UI_BASE_SIZE };
</script>

<ButtonBetProvider>
	{#snippet children({ key, onpress })}
		<OnHotkey hotkey="Space" {disabled} {onpress} />
		<Button {...props} {sizes} {onpress} {disabled}>
			{#snippet children({ center, hovered, pressed })}
				<Container
					{...center}
					scale={pressed ? 0.95 : hovered && !disabled ? 1.08 : 1}
				>
					<Circle
						diameter={180}
						backgroundColor={disabled || ['spin_disabled', 'stop_disabled'].includes(key)
							? 0x666666
							: 0x000000}
						anchor={0.5}
						y={4}
						alpha={pressed ? 0.8 : hovered && !disabled ? 1 : 0.9}
					/>
					<Sprite
						key="betButton"
						width={140}
						height={145}
						anchor={0.5}
						alpha={disabled || ['spin_disabled', 'stop_disabled'].includes(key) ? 0.5 : pressed ? 0.85 : 1}
					/>
				</Container>
			{/snippet}
		</Button>
	{/snippet}
</ButtonBetProvider>
