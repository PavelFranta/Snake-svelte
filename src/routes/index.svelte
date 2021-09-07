<script>
	import Game from '../components/game.svelte';
	import { fly, fade } from 'svelte/transition';

	let showTitle = false
	let showMenu = false;
	let showGame = false;

	setTimeout(() => {
		showTitle = true;
	}, 1);
	setTimeout(() => {
		showMenu = true;
	}, 900);

	function goToRandomPage() {
		window.location = 'https://www.sdilejteneztozakazou.cz/homepage/'
	}

	function startGame() {
		showMenu = false;
		showGame = true;
	}
</script>

<div class="h-full w-full">
	{#if showTitle}
		<h1 class="text-9xl pl-10 pt-10 pb-28"
			in:fly="{{ y: 800, duration: 1000 }}">
			𝚜𝚗𝚊𝚔𝚎
		</h1>
	{/if}
	<div class="flex justify-center relative">
		{#if showMenu}
			<div
				class="flex flex-col w-72 m-auto items-center text-4xl"
				in:fade
				out:fly="{{ x: -800, duration: 1000 }}"
			>
				<button
					class="py-4 px-7 border hover:scale-110 transition-all w-full rounded-md mb-8"
					on:click={startGame}
				>
					Nová hra
				</button>
				<button
					class="py-4 px-7 border hover:scale-110 transition-all w-full rounded-md mb-8"
				>
					Obtížnost
				</button>
				<button
					class="py-4 px-7 border hover:scale-110 transition-all w-full rounded-md mb-8"
				>
					TOP scores
				</button>
				<button
					class="py-4 px-7 border hover:scale-110 transition-all w-full rounded-md"
					on:click={goToRandomPage}
				>
					Magic EXIT
				</button>
			</div>
		{/if}
		{#if showGame}
		  <div
				in:fly="{{ x: 800, duration: 1000 }}"
				class="absolute m-auto"
			>
				<Game />
			</div>
		{/if}
	</div>
</div>