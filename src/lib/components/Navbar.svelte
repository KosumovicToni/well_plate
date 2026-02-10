<script lang="ts">
  let { rows = $bindable(), cols = $bindable() } = $props();
  let selected: string = $state("96");
  const ps: string[] = ["96", "48", "24", "12", "6"];

  function changeDim() {
    switch (selected) {
      case "384":
        rows = 14;
        cols = 24;
        return;

      case "96":
        rows = 8;
        cols = 12;
        return;

      case "48":
        rows = 6;
        cols = 8;
        return;

      case "24":
        rows = 4;
        cols = 6;
        return;

      case "12":
        rows = 3;
        cols = 4;
        return;

      case "6":
        rows = 2;
        cols = 3;
        return;
    }
  }
</script>

<div class="print:hidden flex justify-between">
  <div class="flex flex-row items-center gap-x-4">
    <h1 class="p-2 text-2xl font-bold">well-plate</h1>
    <div class="">
      <label for="p-type" class="font-bold text-xl">p - </label>
      <select
        class="font-bold text-xl"
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
  </div>
  <button
    aria-label="pdf"
    class=" rounded-lg border-2 m-2 hover:bg-gray-50 transition-all"
    onclick={() => {
      const originalTitle = document.title;
      document.title = "well-plate" + new Date().getTime();
      window.print();
      document.title = originalTitle;
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
