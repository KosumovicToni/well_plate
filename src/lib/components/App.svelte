<script lang="ts">
  import Navbar from "$lib/components/Navbar.svelte";
  import Footer from "$lib/components/Footer.svelte";
  import Pot from "$lib/components/Pot.svelte";
  import Farmaco from "$lib/components/Farmaco.svelte";
  import AddFarmaco from "$lib/components/addFarmaco.svelte";

  type farmaco = {
    name: string;
    dose: number;
    unit: string;
    color: string;
  };

  const alf = [
    "",
    "A",
    "B",
    "C",
    "D",
    "E",
    "F",
    "G",
    "H",
    "I",
    "J",
    "K",
    "L",
    "M",
    "N",
  ];

  let { isDirty = $bindable() } = $props();

  let rows = $state(8);
  let cols = $state(12);

  let farmaci: farmaco[] = $state([]);
  let defaults: { name: string; color: string }[] = $state([
    { name: "Blank", color: "white" },
    { name: "NT", color: "white" },
  ]);

  let pop: boolean = $state(false);
  let hidden: boolean = $state(true);
  let a_count: number = $state(0);
  let submit: boolean = $state(false);
  let reset: boolean = $state(false);

  let color: string | undefined = $state("white");
  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  let ref_dose: number | undefined = $state();
  let ref_unit: string = $state("uM");

  const empty: { name: string; dose: undefined; unit: string; color: string } =
    { name: "empty", dose: undefined, unit: "uM", color: "white" };

  let selected:
    | { name: string; dose: number; unit: string; color: string }
    | undefined = $state();

  let clearAll = $state(false);
  let clearPot = $state(false);

  $effect(() => {
    if (clearPot) {
      clearPot = false;
      if (farmaci.length == 0) isDirty = false;
    }
  });

  $effect(() => {
    if (clearAll) {
      clearAll = false;
      clearPot = true;
      farmaci = [];
      isDirty = false;
    }
  });
</script>

<div class="flex flex-col w-screen h-screen">
  <Navbar bind:rows bind:cols />
  <div class="flex flex-col md:flex-row lg:flex-row h-screen w-screen bg-white">
    <div
      class="flex flex-col grow text-center my-auto font-bold lg:ml-auto my-5"
    >
      <h1 class="print:hidden text-3xl mb-2">Legend</h1>
      <div id="legend" class="flex flex-col w-full items-center p-12">
        <div class="flex flex-col items-center xl:flex-row w-full xl:w-[700px]">
          {#each defaults as def}
            <Farmaco
              name={def.name}
              dose={undefined}
              unit={undefined}
              bind:color={def.color}
            />
          {/each}
        </div>
        <div
          class="print:px-[0px] flex flex-col w-full max-w-[700px] lg:px-[5px] items-center"
        >
          {#each farmaci as farmaco}
            <Farmaco
              name={farmaco.name}
              dose={farmaco.dose}
              unit={farmaco.unit}
              bind:color={farmaco.color}
            />
          {/each}
        </div>
      </div>
      <div class="relative flex flex-col items-center">
        <button
          class="print:hidden font-bold rounded-lg border-2 w-20 transition duration-500 opacity-60 hover:opacity-100"
          onclick={() => {
            pop = !pop;
          }}>+</button
        >

        {#if pop}
          <AddFarmaco bind:pop bind:farmaci bind:isDirty />
        {/if}
      </div>
    </div>
    <div id="well-plate" class="flex flex-col my-auto grow">
      {#each { length: rows + 1 } as _, i}
        <div
          class="grid justify-items-center lg:gap-7 gap-5 lg:p-2 p-1"
          style="grid-template-columns: repeat({cols + 1}, minmax(0, 40px));"
        >
          {#if i === 0}
            <div class="w-10"></div>

            {#each { length: cols } as _, j}
              <div
                class="flex items-center justify-center h-11 w-11 lg:h-15 lg:w-15 md:h-13 md:w-13"
              >
                <p class="font-bold text-black text-sm">{j + 1}</p>
              </div>
            {/each}
          {:else}
            <div
              class="flex items-center justify-center h-11 w-11 lg:h-15 lg:w-15 md:h-13 md:w-13"
            >
              <p class="font-bold text-black text-sm">{alf[i]}</p>
            </div>

            {#each { length: cols } as _}
              <Pot
                bind:submit
                bind:reset
                selected={{ color, dose, unit, ref_unit, ref_dose }}
                name={selected?.name}
                bind:hidden
                bind:count={a_count}
                clear={clearPot}
              />
            {/each}
          {/if}
        </div>
      {/each}
    </div>
    {#if !hidden}
      <div
        class="print:hidden absolute top-0 flex flex-col items-center w-full z-20 h-auto p-2 font-bold justify-center"
      >
        <div class="bg-white border-2 py-2 rounded-lg w-70">
          <div class="flex flex-row justify-center gap-x-3">
            <label for="">Reagent : </label>
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
              {#each defaults as def}
                <option value={def}>{def.name}</option>
              {/each}
              <option value={empty}>{empty.name}</option>
            </select>
          </div>
          {#if !(typeof selected == "undefined" || ["empty", "NT", "Blank"].includes(selected!.name))}
            <div class="flex flex-row justify-center">
              <label for="dose" class="inline-block">[C] : </label>
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
          {/if}
          <div class="flex w-full justify-center gap-x-2">
            <button
              class="rounded-lg text-center text-white mt-2 p-1 bg-red-600"
              onclick={() => {
                hidden = true;
                reset = true;
              }}>cancel</button
            >
            <button
              class="rounded-lg text-center text-white bg-sky-600 mt-2 p-1"
              disabled={typeof selected == "undefined" ||
                (typeof dose == "undefined" &&
                  !["empty", "NT", "Blank"].includes(selected.name))}
              style="opacity: {typeof selected == 'undefined' ||
              (typeof dose == 'undefined' &&
                !['empty', 'NT', 'Blank'].includes(selected.name))
                ? '0.5'
                : '1'}"
              onclick={() => {
                hidden = true;
                submit = true;
                isDirty = true;
              }}>apply</button
            >
          </div>
        </div>
      </div>
    {/if}
  </div>
  <Footer bind:clearAll bind:clearPot />
</div>

<style>
  @media print {
    @page {
      size: landscape;
    }

    #well-plate {
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
      zoom: 0.8;
    }
    #legend {
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
      zoom: 0.8;
    }
  }
</style>
