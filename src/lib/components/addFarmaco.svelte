<script lang="ts">
  import Color from "./Color.svelte";

  let {
    pop = $bindable(),
    farmaci = $bindable(),
    isDirty = $bindable(),
  } = $props();

  let colors: string[] = ["red", "blue", "green", "orange", "purple", "brown"];
  let name: string = $state("");
  let color: string = $state(colors[0]);
  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  function addFarmaco() {
    if (typeof dose == "undefined") dose = 1;
    farmaci.push({ name, dose, unit, color });
    name = "";
    dose = undefined;
    unit = "uM";
    color = colors[0];
  }

  const handleSelect = (col: string) => {
    color = col;
  };
</script>

<div
  class="absolute top-6 z-10 bg-white border-2 rounded-lg w-94 mt-2 h-auto p-2 grid grid-cols-1 justify-center shadow-xl"
>
  <div class="flex justifly-between text-start">
    <label for="">Reagent : </label>
    <input type="text" class="border-b-1 text-center" bind:value={name} />
  </div>
  <div class="flex justifly-between text-start">
    <label for="">max[C] : </label>
    <div class="flex flex-row">
      <input type="text" class="border-b-1 text-center" bind:value={dose} />
      <select name="unit" id="unit" bind:value={unit}>
        <option value="uM">uM</option>
        <option value="nM">nM</option>
        <option value="ug/mL">ug/mL</option>
        <option value="other">other</option>
      </select>
    </div>
  </div>
  <div class="flex w-full items-center justify-start mt-2">
    <label for="color-dropdown" class="flex font-bold"> Color : </label>
    <div class="flex flex-row item-center justify-center">
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
  </div>
  <button
    class="mx-auto font-bold rounded-lg border-2 w-20 mt-2 transition duration-500"
    style="opacity: {name.length == 0 || color.length == 0 ? '0.5' : '1'}"
    disabled={name.length == 0 || color.length == 0}
    onclick={() => {
      addFarmaco();
      pop = !pop;
      isDirty = true;
    }}>add</button
  >
</div>

<style>
  input:focus {
    outline: none;
  }
</style>
