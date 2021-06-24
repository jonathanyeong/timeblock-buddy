<script context="module">
  import { createClient } from '@supabase/supabase-js'

  const supabaseUrl = '***REMOVED***'
  const supabaseKey = '***REMOVED***'
  const supabase = createClient(supabaseUrl, supabaseKey)

	export function load() {
		return {
			props: {
				supabase: supabase
			}
		};
	}
</script>

<script>
  import { formatISO } from 'date-fns';
  export let supabase;
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

  const handleSubmit = () => {
    console.log("On submit")
    saveBlock(projectName, timeblocks)
  }

  const timeblocks = [
    { id: "0900", timestamp: formatISO(new Date(2021, 5, 24, 9, 0, 0)), text: "9:00am", checked: false },
    { id: "1000", timestamp: formatISO(new Date(2021, 5, 24, 10, 0, 0)), text: "10:00am", checked: false },
    { id: "1100", timestamp: formatISO(new Date(2021, 5, 24, 11, 0, 0)), text: "11:00am", checked: false },
    { id: "1200", timestamp: formatISO(new Date(2021, 5, 24, 12, 0, 0)), text: "12:00am", checked: false },
    { id: "1300", timestamp: formatISO(new Date(2021, 5, 24, 13, 0, 0)), text: "1:00pm", checked: false }
  ]

</script>

<h1>Timeblock buddy</h1>

<form>


  {#each timeblocks as timeblock}
    <label for="{timeblock.id}">{timeblock.text}</label>
    <input id="{timeblock.id}" type="checkbox" bind:checked={timeblock.checked}>
  {/each}

  <label for="projectName">Project Name</label>
  <input id="projectName" type="text" bind:value={projectName}>
  <input on:click|preventDefault={handleSubmit} type="submit" value="Submit">
</form>