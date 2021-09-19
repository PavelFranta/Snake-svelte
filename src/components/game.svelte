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

	interface position {
		x:number
		y: number
	}

	let playgroundSize:number = 800
	let snakePositionX:number = Math.round(playgroundSize/2)
  let snakePositionY:number = Math.round(playgroundSize/2)
  let foodPositionX:number = -99999
  let foodPositionY:number = -99999
	let currentSnakeDirection:direction = direction.Left
	let isGameStarted:boolean = false
  let gameInterval
  let isGameOver:boolean = false
	let length:number = 1
	let bodyPositionLog = []

  $: position = {x: snakePositionX, y: snakePositionY};
  $: foodPosition = {x: foodPositionX, y: foodPositionY}

	function generateFoodPosition() {
		foodPositionX = (Math.round(Math.random()*39))*20;
    foodPositionY = (Math.round(Math.random()*39))*20;
	}

	function resetSnakeAndHideFood() {
		snakePositionX = Math.round(playgroundSize/2)
    snakePositionY = Math.round(playgroundSize/2)
		foodPositionX = -99999;
  	foodPositionY = -99999
	}

	function handleKeydown(event) {
		parseDirection(event)
		gameEngine()
	}

	function gameEngine() {
		if (!isGameStarted) {
			switch (isGameOver) {
				case true:
					if (currentSnakeDirection === direction.Space) {
						isGameOver = false;
						bodyPositionLog = []
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
		length = 1;
		isGameOver = false;
		isGameStarted = true;
		gameInterval = setInterval(move , 50);
	}


	function move() { 
		bodyPositionLog.push({x: snakePositionX, y:snakePositionY})
		bodyPositionLog = bodyPositionLog.slice(-length);
		makeAStepWithSnake();
		checkIfSnakeHitWall();
		checkIfSnakeHitHimself();
		checkIfSnakeEatFood();
	}

	function checkIfSnakeHitHimself() {
		if (bodyPositionLog.some(position => position.x === snakePositionX && position.y === snakePositionY)) {
			gameOver();
		}
	}

	function checkIfSnakeEatFood() {
		if (snakePositionX === foodPositionX && snakePositionY === foodPositionY) {
			length++;
			generateFoodPosition();
		}
	}

	function makeAStepWithSnake() {
		if (isGameStarted) {
			switch (currentSnakeDirection) {
				case direction.Up:
			snakePositionY = snakePositionY - 20
				break;
				case direction.Down:
			snakePositionY = snakePositionY + 20
				break;
				case direction.Left:
			snakePositionX = snakePositionX - 20
				break;
				case direction.Right:
			snakePositionX = snakePositionX + 20
				break;
				default:
					break;
			}
		}
	}

	function checkIfSnakeHitWall() {
		if (snakePositionX < 0 || snakePositionX > 780 || snakePositionY < 0 || snakePositionY > 780) {
			gameOver();
		}
	}

	function gameOver() {
		isGameStarted = false;
		isGameOver = true;
		clearInterval(gameInterval);
	}

	function parseDirection(event) {
		if (isGameStarted && event.key === ' ') {
			return;
		}
		switch (event.key) {
			case 'ArrowUp':
				if (currentSnakeDirection !== direction.Down) {
					currentSnakeDirection = direction.Up;
				}
			break;
			case 'ArrowDown':	
				if (currentSnakeDirection !== direction.Up) {
					currentSnakeDirection = direction.Down;
				}
			break;
			case 'ArrowLeft':
				if (currentSnakeDirection !== direction.Right) {
					currentSnakeDirection = direction.Left;
				}
			break;
			case 'ArrowRight':
				if (currentSnakeDirection !== direction.Left) {
					currentSnakeDirection = direction.Right;
				}
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
  class="border-4 rounded-md relative border-black box-content"
>
 <Snake position={position} bodyPositionLog={bodyPositionLog}/>
 <Food {foodPosition} />
 {#if isGameOver}
   <div
    class="flex h-full justify-center items-center"
    in:fade
  >
     <GameOver score={--length} />
   </div>
 {/if}
</div>


 
