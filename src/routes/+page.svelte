<script lang="ts">
  import Navbar from "$lib/components/Navbar.svelte";
  import Footer from "$lib/components/Footer.svelte";
  import Pot from "$lib/components/Pot.svelte";
  import Farmaco from "$lib/components/Farmaco.svelte";
  import AddFarmaco from "$lib/components/addFarmaco.svelte";

  let rows = $state(6);
  let cols = $state(8);

  type farmaco = {
    name: string;
    dose: number;
    unit: string;
    color: string;
  };

  const alf = ["", "A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"];

  let pop: boolean = $state(false);
  let farmaci: farmaco[] = $state([]);
</script>

<div class="flex flex-col w-screen h-screen">
  <Navbar />
  <div
    class="flex flex-col md:flex-row lg:flex-row h-screen w-screen items-center bg-white"
  >
    <div class="flex flex-col grow text-center font-bold lg:ml-auto my-5">
      <h1 class="text-3xl mb-2">Lista Farmaci</h1>

      {#each farmaci as farmaco}
        <Farmaco
          name={farmaco.name}
          dose={farmaco.dose}
          unit={farmaco.unit}
          color={farmaco.color}
        />
      {/each}
      <div class="relative">
        <button
          class="font-bold rounded-lg border-2 w-20 transition duration-500 opacity-60 hover:opacity-100"
          onclick={() => {
            pop = !pop;
          }}>+</button
        >

        {#if pop}
          <AddFarmaco bind:pop bind:farmaci />
        {/if}
      </div>
    </div>
    <div class="flex flex-col grow">
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

            {#each { length: cols } as _}
              <Pot {farmaci} />
            {/each}
          {/if}
        </div>
      {/each}
    </div>
  </div>
  <Footer bind:rows bind:cols />
</div>
