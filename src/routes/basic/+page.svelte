<script lang="ts">
  let name = 'hello'
  let src = '/favicon.png'
  import Nested from './nested.svelte'
  import Counter from './Counter.svelte'
  import Thing from "./Things.svelte"
  let htmlstr = 'this string contains some <strong>HTML!!!</strong>'
  let count = $state(0)

  function increment() {
    count += 1
  }

  let numbers = $state([1, 2, 3, 4, 5])
  function addNumber() {
    numbers.push(numbers.length + 1)
  }
  let total = $derived(numbers.reduce((acc, curr) => acc + curr, 0))
  $inspect(numbers)
  let elapsed = $state(0)
  let interval = $state(1000)
  $effect(() => {
    let timer = setInterval(() => {
      elapsed += 1
    }, interval)
    return () => clearInterval(timer)
  })

  let colors = ['red','orange','yellow','green','blue','indigo','violet']
  let selected = $state('red')
  let things = $state([
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	]);

  let input = $state('World')
  let yes = $state(false)
</script>

<div class="m-1">
  <p class="text-red-500 text-3xl">{name.toUpperCase()} Svelte</p>
  <img {src} alt="" />
  <Nested />
  <p>
    {@html htmlstr}
  </p>
  <button onclick={increment} class="btn btn-primary btn-sm mt-2 mb-2">
    count is {count}
  </button>

  <div>
    <div class="inline bg-slate-500 p-1">{numbers.join(' + ')} = {total}</div>
  </div>
  <button onclick={addNumber} class="btn btn-primary mt-1 mb-1">
    add a number
  </button>
  <p>elapsed: {elapsed}</p>
  <button
    class="btn btn-primary mt-1 mb-1"
    onclick={() => {
      interval /= 2
    }}>
    speed up
  </button>
  <button
    class="btn btn-primary mt-1 mb-1"
    onclick={() => {
      interval += 2
    }}>
    slow down
  </button>

  <Counter/>
  <Counter classes={'btn btn-info mt-1 mb-1'}/>
  <Counter classes={'btn btn-success mt-1 mb-1'}/>
  <h1 style="color: {selected}">Pick a color</h1>
  <div class="colors-div">
    {#each colors as color}
      <button style="background: {color}" aria-label={color} aria-current={selected === color} onclick={() => {selected = color}}></button>
    {/each}
  </div>

  <label class="flex cursor-pointer gap-2">
    <span class="label-text">Dark mode</span>
    <input type="checkbox" class="toggle theme-controller" value="retro" />
    <span class="label-text">Light mode</span>
  </label>
  <button class="btn btn-primary" onclick={()=> things.shift()}>Remove first thing</button>
  {#each things as thing (thing.id)}
    <Thing name = {thing.name}/>
  {/each}

  <input bind:value={input} type="text" class="input input-bordered input-primary"/>
  <h1>Hello {input}!</h1>

  <label>
    <input type="checkbox" bind:checked={yes} />
    Yes! send me regular email spam
  </label>
  {#if yes}
    <p>Thanks you. We will bombard your inbox and sell your personal details.</p>
  {:else}
   <p>You must opt in to continue, If you're not paying, you're the product.</p>
   {/if}
   <button class="btn btn-primary mt-1 mb-1" disabled={!yes}>
    Submit
  </button>
</div>

<style>
 .colors-div {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 8px;
    max-width: 400px;
  }

  .colors-div button {
    aspect-ratio: 1;
    border-radius: 50%;
    background: var(--color, #fff);
    transform: translate(-2px, -2px);
    filter: drop-shadow(2px 2px 3px rgba(0,0,0,0.2));
    transition: all 0.1s;
  }

  .colors-div button[aria-current='true'] {
    transform: none;
    filter: none;
    box-shadow: inset 3px 3px 4px rgba(0,0,0,0.2);
  }
</style>
