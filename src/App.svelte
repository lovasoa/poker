<script lang="ts">
	import Card from "./Card.svelte"
    import {COLOR_NAMES, RANK_NAMES} from "./_constants";
	let cards_in_hand:number=4;
	let selected : Set<string> = new Set;
	const id = (color:string, rank:string) => `${color}_${rank}`;
	const total_cards = COLOR_NAMES.length * RANK_NAMES.length;

	function select(id:string){
		if(!selected.delete(id)) selected.add(id);
		selected = selected;
	}

	function probability_of(card:string, selected:Set<String>){
		if(selected.has(card)) return 0;
		else return 1/(total_cards-selected.size);
	}

	$: color_prob = COLOR_NAMES.map(color => sum(RANK_NAMES.map(rank => probability_of(id(color, rank), selected))));
	$: rank_prob = RANK_NAMES.map(rank => sum(COLOR_NAMES.map(color => probability_of(id(color, rank), selected))));

	function sum(vs:number[]):number {
		return vs.reduce((a,b)=>a+b, 0);
	}

	function show(n:number){
		return (100*n).toLocaleString("en", {maximumFractionDigits:0});
	}
</script>

<table class=deck>
	<thead>
		<th></th>
		{#each RANK_NAMES as rank,j(j)}
		<th title="Probability of holding a {rank}">{show(rank_prob[j])}&nbsp;%</th>
		{/each}
	</thead>
	{#each COLOR_NAMES as color,i (i)}
		<tr>
		<th class="line_proba" title="Probability of holding a {color}">
			{show(color_prob[i])}&nbsp;%
		</th>

		{#each RANK_NAMES as rank}
		<td class="card" on:click={_ => select(id(color, rank))}>
			<Card {color} {rank} hidden={selected.has(id(color, rank))} />
		{/each}
	{/each}
</table>


<!--
<div id=config>
	<label>
		Number of cards in hand
		<input type=number bind:value={cards_in_hand}>
	</label>
</div>
-->

<style>

	.card {
		margin: 3px;
		min-width: 64px;
		max-height: 20vh;
	}
	.line_proba{
		font-size: .8em;
	}
</style>