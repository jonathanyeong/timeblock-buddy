<script context="module">
  import { createClient } from '@supabase/supabase-js'
  import { formatISO, set, format } from 'date-fns';

  const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
  const supabaseKey = import.meta.env.VITE_SUPABASE_KEY

  const supabase = createClient(supabaseUrl, supabaseKey)

	export function load() {
		return {
			props: {
				supabase: supabase,
        timeblocks: setTimeblocks(),
        plannedBlocks: getPlannedblocks()
			}
		};
	}

  const getPlannedblocks = async function () {
    let { data: planned_blocks, error } = await supabase
      .from('planned_blocks')
      .select('*')
    console.log(planned_blocks);
    return planned_blocks
  }


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

<script>
  import Day from '$lib/Day.svelte';

  export let supabase;
  export let timeblocks = []
  export let plannedBlocks
  let projectName;

  const saveBlock = async function(projectName, timeblocks) {
    // TODO: Hardcoding the value

    const selectedTimeblocks = timeblocks.filter(t => t.checked)
    // TODO: Error check to make sure there's stuff selected

    const { data, error } = await supabase
      .from('timeblocks')
      .insert(selectedTimeblocks.map((t) => {
        return { interval: t.timestamp, project: projectName }
      }));
    console.log("Data is", data)
    console.log("Error is", error)
    // if (error) {
    //   // Deal with  error;
    // }
    // We don't care about the data returned
  };
</script>

<!-- {#await plannedBlocks}
	<p>...waiting</p>
{:then blocks}
	<p>Planned Blocks from Supabase {JSON.stringify(blocks)}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await} -->

<Day date={Date.now()}/>