<script context="module">
  import { createClient } from '@supabase/supabase-js'

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
  import {formatISO, parseISO, add} from 'date-fns';

  export let supabase;
  export let plannedBlocks
  let taskName = "";
  let savedBlocks = [];
  $: saveBlock(savedBlocks);

  const saveBlock = async blocks => {
    if (blocks.length > 0 && taskName.length > 0) {
      const { error } = await supabase
        .from('planned_blocks')
        .insert(
          blocks.map((block) => {
            return {
              start_date_time: block.timestamp,
              end_date_time: formatISO(add(parseISO(block.timestamp), { hours: 1 })), // TODO: Oof these functions make me sad.
              task_name: taskName
            }
          })
        );

      if (error) {
        console.log("There was an error!", error)
      }

      taskName = "";
      savedBlocks = [];
    }
  }
</script>

{#await plannedBlocks}
	<p>...refreshing blocks</p>
{:then blocks}
  <Day date={new Date()} bind:savedBlocks={savedBlocks} bind:taskName={taskName} plannedBlocks={blocks} />
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
