<script lang="ts">
	import {counter} from "./shared.svelte"

	function increment() {
		counter.count += 1
	}
    let {classes='btn btn-primary mt-1 mb-1'} = $props()
</script>
<button onclick={increment} class={classes}>
	click is {counter.count} {counter.count === 1? 'time' : 'times'}
</button>
{#if counter.count > 10}
<p> {counter.count} is greater than 10</p>
{/if}