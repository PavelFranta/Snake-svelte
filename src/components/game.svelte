<script lang="ts">
  import GameOver from "./gameOver.svelte";
  import Snake from "./snake.svelte";
  import { fade } from 'svelte/transition';
  import Food from "./food.svelte";

  enum direction {
	Left,
	Right,
	Up,
	Down,
  }

	let playgroundSize = 800;
	let snakePositionX = Math.round(playgroundSize/2)
  let snakePositionY = Math.round(playgroundSize/2)
	let currentSnakeDirection:direction = direction.Left;
  let currentFoodPositionX = Math.floor(Math.random() * 778);
  let currentFoodPositionY = Math.floor(Math.random() * 778);
	let gameStarted = false
	let speed = 1
  let gameInterval;
  let gameOver = false;

  $: position = {x: snakePositionX, y: snakePositionY};
  $: foodPosition = {x: currentFoodPositionX, y: currentFoodPositionY}

	function handleKeydown(event) {
		parseDirection(event);
		if (!gameStarted) {
      gameOver = false;
			gameStarted = true;
			gameInterval = setInterval(move ,speed * 5);
		}
	}

	function move() {
		switch (currentSnakeDirection) {
			case direction.Up:
        snakePositionY = snakePositionY - 1
			break;
			case direction.Down:
        snakePositionY = snakePositionY + 1
			break;
			case direction.Left:
        snakePositionX = snakePositionX - 1
			break;
			case direction.Right:
        snakePositionX = snakePositionX + 1
			break;
			default:
				break;
		}

		timeToEat() {
			if (position.x) {
				
			}
		}

    if (snakePositionX < 0 || snakePositionX > 778 || snakePositionY < 0 || snakePositionY > 778) {
      gameStarted = false;
      gameOver = true;
      clearInterval(gameInterval);
    }
	}

	function parseDirection(event) {
		switch (event.key) {
			case 'ArrowUp':
				currentSnakeDirection = direction.Up;
			break;
			case 'ArrowDown':	
				currentSnakeDirection = direction.Down;
			break;
			case 'ArrowLeft':
				currentSnakeDirection = direction.Left;
			break;
			case 'ArrowRight':
				currentSnakeDirection = direction.Right;
			break;
			default:
				break;
		}
	}
</script>

<svelte:window on:keydown={handleKeydown} />
<div
	style="height: {playgroundSize}px; width: {playgroundSize}px"
  class="border-4 rounded-md relative border-black"
>
 <Snake {position} />
 <Food {foodPosition} />
 {#if gameOver}
   <div
    class="flex h-full justify-center items-center"
    in:fade
  >
     <GameOver />
   </div>
 {/if}
</div>



