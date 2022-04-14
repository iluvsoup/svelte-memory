<script>
	// FUTURE GOALS: multiplayer

	// TODO: fix it breaking when you go too fast
	// TODO: award points based on how many tries they need
	// TODO: I think it would be cool if when you got a pair right/wrong, it flipped around with either a red themed X or green themed check before resetting
	// change text using TERNARY OPERATORS

	import Card from './Card.svelte'

	const GRID_SIZE_X = 5;
	const GRID_SIZE_Y = 4;

	let cardTypes = ["🦄", "🦝", "🦃", "🐑", "🐸", "🐊", "🦊", "🐱", "🐴", "🐮", "🐷", "🐪", "🐐", "🦙", "🦒", "🐘", "🦏", "🐭", "🐰", "🐿️", "🦫", "🐻", "🐼", "🦨", "🦗", "🐝", "🐛", "🦋", "🐌", "🐙", "🦈", "🐡", "🐟", "🐬", "🦖", "🦩", "🦉", "🦢", "🦆", "🐓"]

	function shuffleArray(array) {
		for (var i = array.length - 1; i > 0; i--) {
				var j = Math.floor(Math.random() * (i + 1));
				[array[i], array[j]] = [array[j], array[i]]
		}
		return array;
	}

	function generatePairs() {
		let numberOfCards = (GRID_SIZE_X * GRID_SIZE_Y)
		let tmp = []

		let cloned = Array.from(cardTypes)

		for (let i = 0; i < numberOfCards; i=i+2) {
			let randomIndex = Math.floor(Math.random() * cloned.length)
			tmp[i] = cloned[randomIndex]
			tmp[i+1] = cloned[randomIndex]

			cloned.splice(randomIndex, 1)
		}

		return shuffleArray(tmp)
	}

	let pairs = generatePairs()

	let grid = [...Array(GRID_SIZE_X)].map((_, i) => [...Array(GRID_SIZE_Y)].map(
		(_, j) => ({flipped: false, type: pairs[(GRID_SIZE_Y*i)+j], invis: false})
	))

	let clicked = []
	let foundPairs = 0

	$: won = foundPairs == (GRID_SIZE_X*GRID_SIZE_Y) / 2 ? true : false

	function handleClick(x,y) {
		if (won || grid[x][y].flipped) { return }

		if (!grid[x][y].flipped) {
			grid[x][y].flipped = true
			clicked.push({x:x, y:y})

			if (clicked.length == 2) {
				setTimeout(() => {
					grid[clicked[0].x][clicked[0].y].flipped = false
					grid[clicked[1].x][clicked[1].y].flipped = false

					if (grid[clicked[0].x][clicked[0].y].type == grid[clicked[1].x][clicked[1].y].type) {
						foundPairs++
						
						grid[clicked[0].x][clicked[0].y].invis = true
						grid[clicked[1].x][clicked[1].y].invis = true
					}

					clicked = []
				}, 1000);
			}
		}
	}
</script>

<main>
	<h1 class="center">Memory</h1>

	{#if !won}
		<div class="center">
			{#each grid as column, x}
				<div class="column">
					{#each column as card, y}
						<Card {...grid[x][y]} on:click={() => handleClick(x,y)}/>
					{/each}
				</div>
			{/each}
		</div>
	{:else}
		<h1 class="center">You won!</h1>
	{/if}
</main>

<style>
	.center {
		display: flex;
		justify-content: center;
		align-items: center;
		text-align: center;
	}

	h1 {
		font-size: 3em;
	}
</style>