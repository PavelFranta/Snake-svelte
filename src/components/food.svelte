<script lang="ts">
    export let foodPosition;
  </script>
  
  <div
    style="top: {foodPosition.y}px; left: {foodPosition.x}px"
    class="block w-6 h-6 absolute rounded-full bg-green-700"
  >
  </div>