<script lang="ts">
    import {writable} from 'svelte/store';
    const progress = writable(0.5);
</script>

<progress value="{$progress}" class='w-full inline-block'></progress>

<div class='ml-1 mt-1'>
    <button class='btn btn-primary' onclick={()=> progress.set(0)}>0%</button>
    <button class='btn btn-secondary' onclick={()=> progress.set(0.25)}>25%</button>
    <button class='btn btn-info' onclick={()=> progress.set(0.5)}>50%</button>
    <button class='btn btn-success' onclick={()=> progress.set(0.75)}>75%</button>
    <button class='btn btn-warning' onclick={()=> progress.set(1)}>100%</button>
</div>
