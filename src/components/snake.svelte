<script lang="ts">
  export let position;
  export let bodyPositionLog;
</script>

<div
  style="top: {position.y}px; left: {position.x}px"
  class="block w-5 h-5 absolute bg-black"
>
</div>
{#each bodyPositionLog as bodyPosition}
<div
  style="top: {bodyPosition.y}px; left: {bodyPosition.x}px"
  class="block w-5 h-5 absolute bg-black"
></div>
{/each}