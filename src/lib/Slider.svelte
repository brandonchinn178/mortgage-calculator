<script lang="ts">
  import { draggable } from '@neodrag/svelte';

  export type Props = {
    value: number
    min?: number
    max: number
    tickInterval?: number
  }

  // TODO: show ticks
  let {
    value = $bindable(),
    min = 0,
    max,
    tickInterval,
  }: Props = $props()

  const TOTAL_WIDTH = 300

  const range = $derived(max - min)
  const position = $derived((value - min) / range * TOTAL_WIDTH)
</script>

<span class="slider" style:width={`${TOTAL_WIDTH}px`}>
  <span
    class="handle"
    use:draggable={{
      position: { x: position, y: 0 },
      axis: "x",
      bounds: "parent",
      onDrag: ({ currentNode }) => {
        value = Math.round(currentNode.offsetLeft / TOTAL_WIDTH * range + min)
      },
      transform: ({ offsetX, rootNode }) => {
        rootNode.style.left = `${offsetX}px`;
      },
    }}
  ></span>
  <span class="track"></span>
</span>

<style>
  .slider {
    position: relative;
    display: flex;
    height: 30px;
  }

  .handle {
    width: 10px;
    height: 100%;
    border: 1px outset #999;
    background: white;
    box-shadow: 0 0 2px #999;

    position: absolute;

    cursor: ew-resize;
  }

  .track {
    width: 100%;
    height: 2px;
    background: gray;
    align-self: center;
  }
</style>
