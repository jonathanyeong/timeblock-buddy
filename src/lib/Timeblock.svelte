<script>
  export let config;

  const setBlock = (timeblock) => {
    if (config.checked) {
      config.checked = false
    } else {
      config.checked = true
    }
  }
</script>

<div class="h-20 border-2 cursor-pointer hover:bg-blue-100" on:click={() => setBlock(config)} class:bg-blue-300={config.checked} >
  <!-- Add a block with an event name if there is one -->
</div>

