<script lang="ts">
  import { Rectangle, SpineProvider, SpineTrack, Sprite } from 'pixi-svelte';
  import { FadeContainer } from 'components-pixi';
  import { SECOND } from 'constants-shared/time';
  import { getContext } from '../game/context';

  const context = getContext();
  const canvas = $derived(context.stateLayoutDerived.canvasSizes());

  const layoutType = $derived(context.stateLayoutDerived.layoutType());
  const isSmallScreen = $derived(['portrait', 'tablet'].includes(layoutType));

	const baseBackgroundProps = $derived(
  isSmallScreen
    // small screens: use the *desktop-style* layout (original responsive bg)
    ? context.stateLayoutDerived.normalBackgroundLayout({ scale: 1 })
    // larger screens: stretch full canvas
    : {
        x: canvas.width / 2,
        y: canvas.height / 2,
        width: canvas.width,
        height: canvas.height,
      },
);

  const showBaseBackground = $derived(context.stateGame.gameType === 'basegame');
  const showFeatureBackground = $derived(context.stateGame.gameType === 'freeSpins');
</script>

<Rectangle {...canvas} backgroundColor={0x000000} zIndex={-3} />

<FadeContainer show={showBaseBackground} duration={SECOND} zIndex={-2}>
  <Sprite key="baseBackground" {...baseBackgroundProps} anchor={{ x: 0.5, y: 0.5 }} />
</FadeContainer>

<FadeContainer show={showFeatureBackground} duration={SECOND} zIndex={-1}>
  <SpineProvider key="foregroundFeatureAnimation"
    {...context.stateLayoutDerived.normalBackgroundLayout({ scale: 0.5 })}>
    <SpineTrack trackIndex={0} animationName={'idle'} loop />
  </SpineProvider>
  <SpineProvider key="foregroundFeatureAnimation"
    {...context.stateLayoutDerived.normalBackgroundLayout({ scale: 0.5 })}>
    <SpineTrack trackIndex={0} animationName={'dust'} loop />
  </SpineProvider>
</FadeContainer>