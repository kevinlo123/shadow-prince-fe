<script lang="ts" module>
	import { Rectangle, type RectangleProps } from 'pixi-svelte';

	export type Props = RectangleProps & {
		key?: string;
	};
</script>

<script lang="ts">
	const { key, ...rectangleProps }: Props = $props();
</script>

<Rectangle borderRadius={50} {...rectangleProps} />

<!-- ADD YOUR DESIGN -->

<!-- <script lang="ts" module>
	import { Sprite, type SpriteProps } from 'pixi-svelte';
	import type { sharedAssetsPixi } from 'constants-shared/assets';

	export type Props = SpriteProps & {
		key: keyof typeof sharedAssetsPixi;
	};
</script>

<script lang="ts">
	const props: Props = $props();
</script>

<Sprite {...props} /> -->
