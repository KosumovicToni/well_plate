<script lang="ts">
  import Pot from "$lib/components/Pot.svelte";
  import Farmaco from "$lib/components/Farmaco.svelte";
  import AddFarmaco from "$lib/components/addFarmaco.svelte";

  let rows = $state(8);
  let cols = $state(12);

  type farmaco = {
    name: string;
    dose: number;
    unit: string;
    color: string;
  };

  const alf = ["", "A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"];

  let pop: boolean = $state(false);
  let farmaci: farmaco[] = $state([]);
  let colors: string[] = ["red", "blue", "green", "orange", "purple", "brown"];

  let name: string = $state("");
  let color: string = $state(colors[0]);
  let dose: number | undefined = $state();
  let unit: string = $state("uM");
</script>

<div class="flex flex-col w-screen h-screen">
  <div class="flex justify-between">
    <h1 class="p-2 text-2xl font-bold">weel-plate</h1>
    <button
      aria-label="pdf"
      class="rounded-lg border-2 m-2 hover:bg-gray-50 transition-all"
      onclick={() => {
        console.log("pdf printing");
      }}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="size-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m2.25 0H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z"
        />
      </svg>
    </button>
  </div>
  <div
    class="flex flex-col h-screen w-screen items-center justify-center bg-white"
  >
    <div class="grid grid-cols-1 lg:grid-cols-4 w-screen justify-items-center">
      <div
        class="grd grd-cols-1 justify-item-center text-center font-bold lg:ml-auto my-5"
      >
        <h1 class="text-3xl mb-2">Lista Farmaci</h1>

        {#each farmaci as farmaco}
          <Farmaco
            name={farmaco.name}
            dose={farmaco.dose}
            unit={farmaco.unit}
            color={farmaco.color}
          />
        {/each}

        <button
          class="font-bold rounded-lg border-2 w-20 transition duration-500 opacity-60 hover:opacity-100"
          onclick={() => {
            pop = !pop;
          }}>+</button
        >

        {#if pop}
          <AddFarmaco
            bind:name
            bind:dose
            bind:unit
            bind:color
            bind:pop
            bind:farmaci
            {colors}
          />
        {/if}
      </div>
      <div class="col-span-3">
        {#each { length: rows + 1 } as _, i}
          <div
            class="grid justify-items-center gap-4 py-1"
            style="grid-template-columns: repeat({cols + 1}, minmax(0, 40px));"
          >
            {#if i === 0}
              <div class="w-10"></div>

              {#each { length: cols } as _, j}
                <div class="flex items-center justify-center w-12 h-12">
                  <p class="font-bold text-black text-sm">{j + 1}</p>
                </div>
              {/each}
            {:else}
              <div class="flex items-center justify-center w-12 h-12">
                <p class="font-bold text-black text-sm">{alf[i]}</p>
              </div>

              {#each { length: cols } as _, j}
                <Pot {farmaci} />
              {/each}
            {/if}
          </div>
        {/each}
      </div>
    </div>
  </div>
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
</div>
