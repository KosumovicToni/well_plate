<script lang="ts">
  import Color from "./Color.svelte";

  let {
    name = $bindable(),
    dose = $bindable(),
    unit = $bindable(),
    color = $bindable(),
    pop = $bindable(),
    farmaci = $bindable(),
    colors,
  } = $props();

  function addFarmaco() {
    farmaci.push({ name, dose: 1, unit, color });
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
  class="absolute z-10 bg-white border-2 rounded-lg w-auto mt-2 h-auto p-2 grid grid-cols-1 justify-center"
>
  <div class="flex justifly-between text-start">
    <label for="">Farmaco : </label>
    <input type="text" class="border-b-1 text-center" bind:value={name} />
  </div>
  <div class="flex justifly-between text-start">
    <label for="">Dose : </label>
    <div>
      <input type="nuber" class="border-b-1 text-center" bind:value={dose} />
      <select name="unit" id="unit" bind:value={unit}>
        <option value="uM">uM</option>
        <option value="nM">nM</option>
        <option value="ug/mL">ug/mL</option>
        <option value="other">other</option>
      </select>
    </div>
  </div>
  <div class="flex w-full items-center justify-start mt-2">
    <label for="color-dropdown" class="flex font-bold"> Color: </label>
    <div class="grid grid-cols-6 item-center">
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
    class="font-bold rounded-lg border-2 w-20 mt-2 transition duration-500"
    style="opacity: {name.length == 0 || color.length == 0 ? '0.5' : '1'}"
    disabled={name.length == 0 || color.length == 0}
    onclick={() => {
      addFarmaco();
      pop = !pop;
    }}>add</button
  >
</div>

<style>
  input:focus {
    outline: none;
  }
</style>
