<script>
  export let config;
</script>

<div on:click={handleClick}><p>{config.text}</p></div>

<style>
  div {
    height: 150px;
    width: 200px;
    border: 1px solid gray;
  }
</style>