<script lang="ts">
  let color: string | undefined = $state("white");
  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  let ref_dose: number | undefined = $state();
  let ref_unit: string = $state("uM");

  let hidden = $state(true);
  let selected:
    | { name: string; dose: number; unit: string; color: string }
    | undefined = $state();

  let { farmaci } = $props();

  function getPower(p_unit: string) {
    switch (p_unit) {
      case "uM":
        return Math.pow(10, -6);
      case "nM":
        return Math.pow(10, -9);
      case "other":
        return 1;
    }
    return Math.pow(10, -3);
  }
</script>

<div class="relative">
  <button
    aria-label="pot"
    class="rounded-full h-11 w-11 lg:h-16 lg:w-16 md:h-14 md:w-14 border-3"
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
          'undefined' && typeof dose !== 'undefined'
          ? (dose * getPower(unit)) / (ref_dose * getPower(ref_unit))
          : 100}"
      ></div>

      <div
        class="relative z-10 flex flex-col font-bold text-center pointer-events-none"
      >
        <p class="text-wrap break-words leading-tight max-w-10">
          {dose}
          {#if typeof dose !== "undefined"}
            <span class="text-xs">{unit}</span>
          {/if}
        </p>
      </div>
    </div>
  </button>

  {#if !hidden}
    <div
      class="absolute flex flex-col w-60 z-20 bg-white border-2 rounded-lg h-auto p-2 font-bold justify-center"
    >
      <div class="flex flex-row justify-center gap-x-3">
        <label for="">Farmaco : </label>
        <select
          bind:value={selected}
          onchange={() => {
            color = selected?.color;
            ref_dose = selected!.dose;
            ref_unit = selected!.unit;
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
      <div class="flex flex-row justify-center">
        <label for="dose" class="inline-block">Dose : </label>
        <div class="flex flex-row justify-between w-2/3 gap-x-1">
          <input
            type="text"
            class="w-1/2 border-b-1 text-center"
            bind:value={dose}
          />
          <select name="unit" id="unit" bind:value={unit} class="w-1/2">
            <option value="uM">uM</option>
            <option value="nM">nM</option>
            <option value="ug/mL">ug/mL</option>
            <option value="other">other</option>
          </select>
        </div>
      </div>
    </div>
  {/if}
</div>

<style>
  input:focus {
    outline: none;
  }
</style>
