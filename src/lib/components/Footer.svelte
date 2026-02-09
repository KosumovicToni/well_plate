<script lang="ts">
  let { rows = $bindable(), cols = $bindable() } = $props();
  let selected: string = $state("p-96");
  const ps: string[] = ["p-96", "p-48", "p-24", "p-12", "p-6"];

  function changeDim() {
    switch (selected) {
      case "p-384":
        rows = 14;
        cols = 24;
        return;

      case "p-96":
        rows = 8;
        cols = 12;
        return;

      case "p-48":
        rows = 6;
        cols = 8;
        return;

      case "p-24":
        rows = 4;
        cols = 6;
        return;

      case "p-12":
        rows = 3;
        cols = 4;
        return;

      case "p-6":
        rows = 2;
        cols = 3;
        return;
    }
  }
</script>

<div
  class="print:hidden flex flex-row w-full items-center justify-between py-12"
>
  <div class="flex text-center w-1/2 items-center justify-center">
    <p class="text-sm">© well-plate 2026</p>
  </div>

  <div class="flex w-1/2 items-center justify-center">
    <div class="flex flex-row justify-center items-center gap-x-4">
      <div>
        <label for="p-type"></label>
        <select
          bind:value={selected}
          onchange={() => {
            changeDim();
          }}
        >
          {#each ps as p}
            <option value={p}>{p}</option>
          {/each}
        </select>
      </div>
      <div>
        <label for="">Rows : </label>
        <input type="number" min="1" max="9" bind:value={rows} />
      </div>
      <div>
        <label for="">Columns : </label>
        <input type="number" min="1" max="12" bind:value={cols} />
      </div>
    </div>
  </div>
</div>
