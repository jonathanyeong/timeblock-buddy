<script>
  import {formatISO, set, format} from 'date-fns';
  import Timeblock from '$lib/Timeblock.svelte'
  export let date = Date.now();
  // export let times = 18; // Default value
  const setTimeblocks = () => {
    const numTimeblocks = 10; // 10 hours in the day

    let timeblocks = []

    const startTime = 8 // 0800 hours
    for (let i = 0; i < numTimeblocks; i++) {
      const date = set(new Date(), {hours: startTime+i, minutes: 0, seconds: 0 })
      let newBlock = { id: format(date, 'h:ms aaa'), timestamp: formatISO(date), text: format(date, 'h:ms aaa'), checked: false };
      timeblocks.push(newBlock);
    }
    return timeblocks;
  }

  let timeblocks = setTimeblocks();

  const handleSave = () => {
    console.log("Save timeblocks")
    console.log(timeblocks)
  }
</script>
<h2 class="text-2xl mb-2">{ format(date, "iii, MMM d")}</h2>
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-4" on:click|preventDefault={handleSave}>Save Blocks</button>
<!-- Border bottom is found on the last timeblock element -->
<div class="w-60">
  {#each timeblocks as timeblock}
    <div class="flex">
      <div class="flex-initial w-20">
        <p>{timeblock.text}</p>
      </div>
      <div class="flex-1">
        <Timeblock config={timeblock} />
      </div>
    </div>
  {/each}
</div>

