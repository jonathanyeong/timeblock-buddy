<script>
  import {formatISO, set, format} from 'date-fns';
  import TimeblockSlot from '$lib/TimeblockSlot.svelte';
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
</script>
<h1>Day { formatISO(date)}</h1>
<p>
  {#each setTimeblocks() as timeblock}
    <TimeblockSlot config={timeblock} />
  {/each}
</p>
