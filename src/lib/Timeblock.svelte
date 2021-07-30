<script>
  export let config;
  export let changedBlock;
  export let blockIndex;

  const setBlock = (timeblock) => {
    if (config.checked) {
      config.checked = false
    } else {
      config.checked = true
    }
    changedBlock[blockIndex] = config
  }

  const taskName = config.taskName || "";
</script>

<div class="h-20 border-2 cursor-pointer hover:bg-blue-100" on:click={() => setBlock(config)} class:bg-blue-300={config.checked} >
  <p>{taskName}</p>
</div>

