<script lang="ts">

let color: string | undefined = $state("white");
  let opacity: number = $state(100);
  let hidden = $state(true);
  let selected:{name:string,color:string} | undefined = $state();

  let props =  $props();
  let farmaci = props.farmaci;
</script>

<div class="relative">
  <div
    class="rounded-full h-12 w-12 border-3"
    onclick={() => {
      hidden = !hidden;
    }}
  >
    <div
      class="rounded-full h-full w-full"
      style="background-color: {color}; opacity: {opacity / 100}"
    ></div>
  </div>

  {#if !hidden}
    <div class="absolute z-10 bg-white border-2 rounded-lg w-38 h-auto p-2">
      <div class="grid grid-cols-2">
          <label for="">Color : </label>
          <select bind:value={selected} onchange={() => {color = selected?.color}}  name="farmaco" id="farmaco">
            {#each farmaci as farmaco}
            <option value={farmaco}>{farmaco.name}</option>
            {/each}
            <option value={undefined}>empty</option>
          </select>
        </div>
        <div class="grid grid-cols-2">
          <label for="">Opacity : </label>
          <input
            type="number"
            min="0"
            max="100"
            bind:value={opacity}
            class="border-b-1"
          />
        </div>
    </div>
  {/if}
</div>

<style>
  input:focus {
    outline: none;
  }
</style>
