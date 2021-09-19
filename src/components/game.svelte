<script lang="ts">
  import GameOver from "./gameOver.svelte";
  import Snake from "./snake.svelte";
  import { fade } from 'svelte/transition';
  import Food from "./food.svelte";

  enum direction {
	Left = 1,
	Right,
	Up,
	Down,
	Space
  }

	let playgroundSize:number = 800;
	let snakePositionX:number = Math.round(playgroundSize/2);
  let snakePositionY:number = Math.round(playgroundSize/2);
  let currentFoodPositionX:number = -99999;
  let currentFoodPositionY:number = -99999;
	let currentSnakeDirection:direction = direction.Left;
	let gameStarted:boolean = false
	let speed:number = 1
  let gameInterval;
  let gameOver:boolean = false;

  $: position = {x: snakePositionX, y: snakePositionY};
  $: foodPosition = {x: currentFoodPositionX, y: currentFoodPositionY}

	function generateFoodPosition() {
		currentFoodPositionX = Math.floor(Math.random() * (playgroundSize - 24))
    currentFoodPositionY = Math.floor(Math.random() * (playgroundSize - 24))
	}

	function resetSnakeAndHideFood() {
		snakePositionX = Math.round(playgroundSize/2)
    snakePositionY = Math.round(playgroundSize/2)
		currentFoodPositionX = -99999;
  	currentFoodPositionY = -99999
	}

	function handleKeydown(event) {
		parseDirection(event)
		gameEngine()
	}

	function gameEngine() {
		if (!gameStarted) {
			switch (gameOver) {
				case true:
					if (currentSnakeDirection === direction.Space) {
						gameOver = false;
						resetSnakeAndHideFood()
					}
					break;
				case false:
					if (currentSnakeDirection) {
						startMove()
						generateFoodPosition();
					}
					break;
				default:
					break;
			}
		}
	}

	function startMove() {
		gameOver = false;
		gameStarted = true;
		gameInterval = setInterval(move ,speed * 10);
	}


	function move() {
		makeAStepWithSnake();
		checkIfSnakeHitWall();
	}

	function makeAStepWithSnake() {
		if (gameStarted) {
			switch (currentSnakeDirection) {
				case direction.Up:
			snakePositionY = snakePositionY - 2
				break;
				case direction.Down:
			snakePositionY = snakePositionY + 2
				break;
				case direction.Left:
			snakePositionX = snakePositionX - 2
				break;
				case direction.Right:
			snakePositionX = snakePositionX + 2
				break;
				default:
					break;
			}
		}
	}

	function checkIfSnakeHitWall() {
		if (snakePositionX < 0 || snakePositionX > 778 || snakePositionY < 0 || snakePositionY > 778) {
			gameStarted = false;
			gameOver = true;
			clearInterval(gameInterval);
		}
	}

	function parseDirection(event) {
		if (gameStarted && event.key === ' ') {
			return;
		}
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
			case ' ':
				currentSnakeDirection = direction.Space;
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



