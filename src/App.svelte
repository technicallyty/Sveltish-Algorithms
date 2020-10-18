<script>
	import GoldsPoreSort from "./components/GoldsPoreSort.svelte";
	import InsertionSort from "./components/InsertionSort.svelte";
	import MergeSort from "./components/MergeSort.svelte";
	import QuickSort from "./components/QuickSort.svelte";

	//	Strings to Sort
	let strings = [
		"05CA627BC2B6F03",
		"065DE6671F040BA",
		"0684FB893D5754E",
		"07C9A2D183E4B65",
		"09F48E7862D2616",
		"1FAB3D47905C286",
		"286E1D0342D7859",
		"30E530C4786AF21",
		"328DE4765C10BA9",
		"34F2756F18E90BA",
		"90BA34F0756F180",
		"D7859286E2D0342",
	];
	//	The starting Selection
	$: selection = strings[0];
	//	sel iterates after the full run-reset-swap of a string, bringing in the next string from strings array
	let sel = 0;
	//	tracks the iterations of a particular string run
	let iterations = 0;
	//	helper variable to hold the mutation of each string. (e.g. swapping the string[last] to front)
	let mutation = "";
	//	speed of algorithm. can be set with slider on the html page. range between 50ms - 1s
	let speed = 1000;

	//	Event emitted by each sorting method.
	//	acts as a semaphore to let the App know when it can reset them all.
	function handleThreadDone(event) {
		threadsDone = threadsDone + 1;
	}
	//	Utility method to shift/mutate the current string.
	function shifty(s) {
		return s.unshift(s.pop());
	}

	//	The semaphore like helper variable. 
	let threadsDone = 0;
	// $: <-- this nifty symbol will run whatever is inside it
	//		when state variables inside it are effected.
	//	Effectively, this is used to run whenever a thread says its done.
	$: {
		//	This function body makes sure the string gets mutated after
		//	its full rotation. If its reached max mutations,
		//	it selects the next string in the array.
		if (iterations != 14) {
			if (threadsDone === 4) {
				if (mutation.length === 0) {
					mutation = strings[sel].split("");
					shifty(mutation);
					var y = mutation.join("");
					selection = y;
				} else {
					shifty(mutation);
					var y = mutation.join("");
					selection = y;
				}
				iterations++;
				threadsDone = 0;
			}
		} else {
			if (threadsDone === 4) {
				iterations = 0;
				mutation = "";
				if (sel + 1 == strings.length) {
					sel = 0;
				} else {
					sel = sel + 1;
				}
				selection = strings[sel];
				iterations++;
				threadsDone = 0;
			}
		}
	}
</script>

<style>
	header {
		width: 800px;
		margin: 0 auto 0 auto;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
	}
	string {
		margin: 5px;
		color: black;
		padding: 5px 10px 5px 10px;
		border-radius: 40px;
	}
	block {
		margin-top: 3rem;
		display: flex;
		flex-grow: 1;
		min-height: auto;
		justify-content: space-evenly;
	}
	algoColumn {
		display: flex;
		margin: 10px;
		flex: auto;
		text-align: center;
		color: white;
		flex-direction: column;
	}
	algoTitle {
		font-size: 30px;
		color: black;
	}

	sortItem {
		margin: 5px;
	} 
	slideWrapper{
		margin: 50px;
	}
</style>

<svelte:head>
	<title>Project 2</title>
</svelte:head>
<slideWrapper>
	<h3>Speed: {speed}ms</h3>
	<div class="slidecontainer">
		<input type="range" min="50" max="1000" value={speed} on:change={(c) => {speed = c.target.value; speed = speed;}} />
	</div>
</slideWrapper>
<header>
	{#key strings}
		{#each strings as s, i}
			<string
				style="background-color: {sel == i ? 'blueviolet' : 'white'}">
				{s}
			</string>
		{/each}
	{/key}
</header>
<block>
	<algoColumn>
		<algoTitle>Insertion Sort</algoTitle>
		<sortItem>
			<InsertionSort
				stringToSort={[selection.split('')]}
				on:message={handleThreadDone}
				{speed} />
		</sortItem>
	</algoColumn>
	<algoColumn>
		<algoTitle>Gold's Poresort</algoTitle>
		<sortItem>
			<GoldsPoreSort
				stringToSort={[selection.split('')]}
				on:message={handleThreadDone}
				{speed} />
		</sortItem>
	</algoColumn>
	<algoColumn>
		<algoTitle>Mergesort</algoTitle>
		<sortItem>
			<MergeSort
				stringToSort={[selection.split('')]}
				on:message={handleThreadDone}
				{speed} />
		</sortItem>
	</algoColumn>
	<algoColumn>
		<algoTitle>Quicksort</algoTitle>
		<sortItem>
			<QuickSort
				stringToSort={[selection.split('')]}
				on:message={handleThreadDone}
				{speed} />
		</sortItem>
	</algoColumn>
</block>
