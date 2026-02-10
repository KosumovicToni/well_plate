<script lang="ts">
  let { clearPot = $bindable(), clearAll = $bindable() } = $props();
</script>

<div
  class="print:hidden flex flex-row w-full items-center justify-between py-12"
>
  <div class="flex text-center w-1/2 items-center justify-center">
    <p class="text-sm">© well-plate 2026</p>
  </div>

  <div class="flex w-1/2 flex-row items-center justify-center gap-x-4">
    <button
      class="font-bold p-1 rounded-md hover:bg-red-600 hover:text-white transition-all"
      onclick={() => {
        clearAll = true;
      }}>Clear All</button
    >
    <button
      class="font-bold p-1 rounded-md hover:bg-red-600 hover:text-white transition-all"
      onclick={() => {
        clearPot = true;
      }}>Clear Pots</button
    >
  </div>
  <!--
      <div>
        <label for="">Rows : </label>
        <input type="number" min="1" max="9" bind:value={rows} />
      </div>
      <div>
        <label for="">Columns : </label>
        <input type="number" min="1" max="12" bind:value={cols} />
      </div> -->
</div>
