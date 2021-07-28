<script context="module">
  import { createClient } from '@supabase/supabase-js'
  import { formatISO, set, format } from 'date-fns';

  const supabaseUrl = import.meta.env.VITE_SUPABASE_URL.toString();
  const supabaseKey = import.meta.env.VITE_SUPABASE_KEY.toString();

  const supabase = createClient(supabaseUrl, supabaseKey)

	export function load() {
		return {
			props: {
				supabase: supabase,
        plannedBlocks: getPlannedblocks()
			}
		};
	}

  const getPlannedblocks = async function () {
    let { data: planned_blocks, error } = await supabase
      .from('planned_blocks')
      .select('*')
    return planned_blocks
  }
</script>

<script>
  import Day from '$lib/Day.svelte';

  export let supabase;
  export let plannedBlocks
  let projectName;
  let savedBlocks = [];
  $: saveBlock(savedBlocks);

  const saveBlock = async blocks => {
    if (blocks.length > 0) {
      console.log("Save block in index on change");
      console.log(blocks);

      const { error } = await supabase
        .from('planned_blocks')
        .insert([
          { start_date_time: blocks[0].timestamp, end_date_time: blocks[blocks.length - 1].timestamp, task_name: 'STUB task name' }
        ]);

      if (error) {
        console.log("There was an error!", error)
      }
    }
  }

  // const saveBlock = async function(projectName, timeblocks) {
  //   // TODO: Hardcoding the value

  //   const selectedTimeblocks = timeblocks.filter(t => t.checked)
  //   // TODO: Error check to make sure there's stuff selected

  //   const { data, error } = await supabase
  //     .from('timeblocks')
  //     .insert(selectedTimeblocks.map((t) => {
  //       return { interval: t.timestamp, project: projectName }
  //     }));
  //   console.log("Data is", data)
  //   console.log("Error is", error)
  //   // if (error) {
  //   //   // Deal with  error;
  //   // }
  //   // We don't care about the data returned
  // };

</script>

<!-- {#await plannedBlocks}
	<p>...waiting</p>
{:then blocks}
	<p>Planned Blocks from Supabase {JSON.stringify(blocks)}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await} -->

<Day date={Date.now()} bind:savedBlocks={savedBlocks}/>