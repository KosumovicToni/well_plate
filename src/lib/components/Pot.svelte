<script lang="ts">
  let color: string | undefined = $state("white");
  let dose: number | undefined = $state();
  let ref_dose: number | undefined = $state();
  let hidden = $state(true);
  let selected: { name: string; dose: number; color: string } | undefined =
    $state();

  let props = $props();
  let farmaci = props.farmaci;
</script>

<div class="relative">
  <button
    aria-label="pot"
    class="rounded-full h-12 w-12 border-3"
    onclick={() => {
      hidden = !hidden;
    }}
  >
    <div
      class="relative h-full w-full flex items-center justify-center rounded-full overflow-hidden"
    >
      <div
        class="absolute inset-0 z-0"
        style="background-color: {color}; opacity: {typeof ref_dose !==
        'undefined'
          ? dose! / ref_dose
          : 100}"
      ></div>

      <div class="relative z-10 font-bold text-center pointer-events-none">
        <p class="text-wrap break-words leading-tight max-w-10">{dose}</p>
      </div>
    </div>
  </button>

  {#if !hidden}
    <div class="absolute w-48 z-10 bg-white border-2 rounded-lg h-auto p-2">
      <div class="grid grid-cols-2">
        <label for="">Farmaco : </label>
        <select
          bind:value={selected}
          onchange={() => {
            color = selected?.color;
            ref_dose = selected!.dose;
          }}
          name="farmaco"
          id="farmaco"
        >
          {#each farmaci as farmaco}
            <option value={farmaco}>{farmaco.name}</option>
          {/each}
          <option value={undefined}>empty</option>
        </select>
      </div>
      <div class="grid grid-cols-2">
        <label for="">dose : </label>
        <input
          type="number"
          min="0"
          max={ref_dose}
          bind:value={dose}
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
