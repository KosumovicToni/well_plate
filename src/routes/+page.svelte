<script lang="ts">
  import Pot from "$lib/components/Pot.svelte";
  let rows = $state(8);
  let cols = $state(12);

  const alf = ["", "A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"];
</script>

<div class="flex flex-col w-screen h-screen">
  <div class="flex flex-col h-screen w-screen items-center justify-center bg-white">
    <div class="grid grid-cols-1 lg:grid-cols-4 w-screen justify-items-center">
      <div class="grd grd-cols-1 justify-item-center text-center font-bold lg:ml-auto my-5 border-b-2 lg:border-none">
        <h1 class="text-3xl mb-2">Lista Farmaci</h1>
        <button class="font-bold rounded-lg border-2 w-20 transition duration-500 opacity-60 hover:opacity-100">+</button>
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
                  <Pot />
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
        <input type="number" min="1" max ="12" bind:value={rows}>
      </div>
      <div>
        <label for="">Columns : </label>
        <input type="number" min="1" max ="12" bind:value={cols}>
      </div>
    </div>
  </div>
</div>