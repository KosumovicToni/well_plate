<script lang="ts">
  import Navbar from "$lib/components/Navbar.svelte";
  import Footer from "$lib/components/Footer.svelte";
  import Pot from "$lib/components/Pot.svelte";
  import Farmaco from "$lib/components/Farmaco.svelte";
  import AddFarmaco from "$lib/components/addFarmaco.svelte";

  type farmaco = {
    name: string;
    dose: number | undefined;
    unit: string | undefined;
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

  let farmaci: farmaco[] = $state([
    { name: "empty", color: "white", dose: undefined, unit: undefined },
    { name: "Blank", color: "white", dose: 1, unit: "other" },
    { name: "NT", color: "white", dose: 1, unit: "other" },
  ]);

  let pop: boolean = $state(false);
  let hidden: boolean = $state(true);
  let a_count: number = $state(0);
  let submit: boolean = $state(false);
  let reset: boolean = $state(false);

  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  let selected: number = $state(0);

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
      farmaci = [
        { name: "empty", color: "white", dose: undefined, unit: undefined },
        { name: "Blank", color: "white", dose: 1, unit: "other" },
        { name: "NT", color: "white", dose: 1, unit: "other" },
      ];
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
          {#each { length: 2 } as _, i}
            <Farmaco
              name={farmaci[i + 1].name}
              dose={undefined}
              unit={undefined}
              bind:color={farmaci[i + 1].color}
            />
          {/each}
        </div>
        <div
          class="print:px-[0px] flex flex-col w-full max-w-[700px] lg:px-[5px] items-center"
        >
          {#each Array.from({ length: farmaci.length - 3 }, (_, i) => i + 3) as index}
            <Farmaco
              name={farmaci[index].name}
              dose={farmaci[index].dose}
              unit={farmaci[index].unit}
              bind:color={farmaci[index].color}
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
    <div id="well-plate" class="flex flex-col my-auto grow pr-3 print:pr-0!">
      {#each { length: rows + 1 } as _, i}
        <div
          class="grid justify-items-center py-2 px-1"
          style="grid-template-columns: repeat({cols + 1}, minmax(0, 80px));"
        >
          {#if i === 0}
            <div class="h-7 w-7 lg:h-15 lg:w-15 md:h-13 md:w-13"></div>

            {#each { length: cols } as _, j}
              <div
                class="flex items-center justify-center h-7 w-7 lg:h-15 lg:w-15 md:h-13 md:w-13"
              >
                <p class="font-bold text-black text-sm">{j + 1}</p>
              </div>
            {/each}
          {:else}
            <div
              class="flex items-center justify-center h-7 w-7 lg:h-15 lg:w-15 md:h-13 md:w-13"
            >
              <p class="font-bold text-black text-sm">{alf[i]}</p>
            </div>

            {#each { length: cols } as _}
              <Pot
                bind:submit
                bind:reset
                bind:hidden
                bind:count={a_count}
                {selected}
                u_sel={unit}
                d_sel={dose}
                clear={clearPot}
                {farmaci}
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
              onchange={() => {}}
              name="farmaco"
              id="farmaco"
            >
              {#each farmaci as farmaco, i}
                <option value={i}>{farmaco.name}</option>
              {/each}
            </select>
          </div>
          {#if selected > 2}
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
              disabled={typeof dose == "undefined" && selected > 2}
              style="opacity: {typeof dose == 'undefined' && selected > 2
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
