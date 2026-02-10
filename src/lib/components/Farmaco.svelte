<script lang="ts">
  import Color from "./Color.svelte";

  let { name, dose, unit, color = $bindable() } = $props();
  let pop: boolean = $state(false);
  let colors: string[] = ["red", "blue", "green", "orange", "purple", "brown"];

  const handleSelect = (col: string) => {
    color = col;
    pop = false;
  };
</script>

<div
  id="farmaco"
  class="relative flex flex-row items-center justify-between p-3 m-1 border-2 rounded-lg font-bold bg-white w-full lg:max-w-[700px] gap-2 text-xl"
>
  <div class="min-w-0 lg:max-w-12s0 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {name}
    </p>
  </div>
  <div class="min-w-0 max-w-64 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {dose}
    </p>
  </div>
  <div class="min-w-0 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {unit}
    </p>
  </div>
  <button
    aria-label="change colot"
    class="w-6 h-6 rounded-full border-2 border-black flex-shrink-0"
    style="background-color: {color}"
    onclick={() => {
      pop = !pop;
    }}
  >
  </button>
  {#if pop}
    <div
      class="print:hidden absolute flex items-center justify-center bg-white rounded-lg z-20 inset-y-0 left-0 sm:w-1/2 lg:w-10/12"
    >
      {#each colors as col}
        <button
          type="button"
          onclick={() => handleSelect(col)}
          class="flex items-center w-full px-2 py-1"
        >
          <Color selected={color == col} color={col} />
        </button>
      {/each}
    </div>
  {/if}
</div>
