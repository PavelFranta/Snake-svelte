<script lang="ts">
    export let foodPosition;
  </script>
  
  <div
    style="top: {foodPosition.y}px; left: {foodPosition.x}px"
    class="block w-5 h-5 absolute rounded-full bg-green-700"
  >
  </div>