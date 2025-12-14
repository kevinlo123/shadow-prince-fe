<script lang="ts" module>
	import ClusterWinAmount, { type RawWin, type Win } from './ClusterWinAmount.svelte';

	export type EmitterEventClusterWinAmounts = {
		type: 'showClusterWinAmounts';
		wins: RawWin[];
	};
</script>

<script lang="ts">
	import BoardContainer from './BoardContainer.svelte';
	import { getContext } from '../game/context';

	const context = getContext();

	type WinWithId = Win & { id: string };
	let wins: WinWithId[] = $state([]);

	let winIdCounter = 0;

	context.eventEmitter.subscribeOnMount({
		showClusterWinAmounts: async (emitterEvent) => {
			// Create wins with oncomplete already set to resolve to avoid timing issues
			const winsWithPromises = emitterEvent.wins.map((rawWin) => {
				const id = `cluster-win-${winIdCounter++}`;
				let resolve: () => void;
				let resolved = false;
				const promise = new Promise<void>((r) => (resolve = r));
				const win: WinWithId = {
					...rawWin,
					id,
					// Ensure oncomplete only resolves once (FadeContainer may call it multiple times)
					oncomplete: () => {
						if (!resolved) {
							resolved = true;
							resolve();
						}
					},
				};
				return { win, promise };
			});

			wins = winsWithPromises.map(({ win }) => win);

			await Promise.all(winsWithPromises.map(({ promise }) => promise));
			wins = [];
		},
	});
</script>

<BoardContainer>
	{#each wins as win (win.id)}
		<ClusterWinAmount {win} />
	{/each}
</BoardContainer>
