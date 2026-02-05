<script lang="ts">
  let { rows = $bindable(), cols = $bindable() } = $props();
</script>

<div class="grid grid-cols-1 justify-items-center p-12">
  <div class="grid grid-cols-2">
    <div>
      <label for="">Rows : </label>
      <input type="number" min="1" max="12" bind:value={rows} />
    </div>
    <div>
      <label for="">Columns : </label>
      <input type="number" min="1" max="12" bind:value={cols} />
    </div>
  </div>
</div>
