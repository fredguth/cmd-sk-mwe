<script lang="ts">
	import { Command } from 'cmdk-sv';
	import unidades from './unidades.json';

	// let options = unidades.map((item) => {
	// 	item.DS_UNIDADE;
	// });
	let options = unidades.map((item) => {
		return item.DS_UNIDADE;
	});
	function filterByQuery(items: string[], query: string): string[] {
		return query
			? items.filter((item) => item?.toUpperCase().includes(query.toUpperCase())).slice(0, 6)
			: items?.slice(0, 6);
	}
	let open = true;
	function runCommand(cmd: () => void) {
		open = false;
		cmd();
	}
	let q = '';
	let selectedOption = '';
	$: filteredOptions = filterByQuery(options, q);
	$: console.log(filteredOptions);
</script>

<Command.Root label="Command Menu">
	<Command.Input bind:value={q} />
	<Command.List>
		<Command.Empty>No results found.</Command.Empty>
		{#each filteredOptions as option}
			<Command.Item
				value={option}
				onSelect={() => {
					runCommand(() => {
						selectedOption = option;
					});
				}}>{option}</Command.Item
			>
		{/each}
	</Command.List>
</Command.Root>
