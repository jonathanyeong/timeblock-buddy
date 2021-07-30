<script>
  import {formatISO, set, format, parseISO} from 'date-fns';
  import Timeblock from '$lib/Timeblock.svelte'
  export let date = new Date();
  export let savedBlocks = [];
  export let taskName = "";
  export let plannedBlocks = [];

  const setTimeblocks = () => {
    const numTimeblocks = 10; // 10 hours in the day

    let timeblocks = [];
    let startTimes = plannedBlocks.reduce((acc, currVal) => (
      acc[formatISO(parseISO(currVal.start_date_time))] = currVal.task_name, acc
    ), {});

    const startTime = 8 // 0800 hours
    for (let i = 0; i < numTimeblocks; i++) {
      const date = set(new Date(), {hours: startTime+i, minutes: 0, seconds: 0 })
      let newBlock = {
        id: format(date, 'h:ms aaa'),
        timestamp: formatISO(date),
        text: format(date, 'h:ms aaa'),
        checked: startTimes[formatISO(date)] !== undefined,
        taskName: startTimes[formatISO(date)]
      };

      timeblocks.push(newBlock);
    }
    return timeblocks;
  }

  let timeblocks = setTimeblocks();
  let changedBlock = {};
  let changedBlocks = {};
  $: blockChanged(changedBlock)

  const handleSave = () => {
    let updatedBlocks = []
    for (const index in changedBlocks) {
      timeblocks[parseInt(index)] = {...changedBlocks[index], taskName: taskName}
      updatedBlocks = [...updatedBlocks, timeblocks[parseInt(index)]]
    }
    savedBlocks = updatedBlocks;
  }

  const blockChanged = (block) => {
    console.log("Changed Block")
    console.log(block);
    changedBlocks = block
  }
</script>
<h2 class="text-2xl mb-2">{ format(date, "iii, MMM d")}</h2>
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-4" on:click|preventDefault={handleSave}>Save Blocks</button>
<div class="w-1/3 mb-8">
  <label for="taskName" class="block text-lg font-medium">Task Name</label>
  <input id="taskName" type="text" name="taskName" bind:value={taskName} class="block w-full px-2 py-2 pl-2 border-2 border-gray-400 rounded">
</div>

<!-- Border bottom is found on the last timeblock element -->
<div class="w-60">
  {#each timeblocks as timeblock, index}
    <div class="flex">
      <div class="flex-initial w-20">
        <p>{timeblock.text}</p>
      </div>
      <div class="flex-1">
        <Timeblock config={timeblock} blockIndex={index} bind:changedBlock={changedBlock} />
      </div>
    </div>
  {/each}
</div>

