<script lang="ts">
  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  let active = $state(false);
  let index: number = $state(0);

  let {
    submit = $bindable(),
    reset = $bindable(),
    selected,
    u_sel,
    d_sel,
    hidden = $bindable(),
    count = $bindable(),
    clear,
    farmaci,
  } = $props();

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

  function changeState() {
    active = false;
    count--;

    if (count == 0) submit = false;

    index = selected;

    if (index < 3) {
      if (index == 0) {
        dose = undefined;
        unit = "uM";
      } else {
        dose = 0.3;
        unit = "other";
      }
      console.log(
        (dose! * getPower(unit)) /
          (farmaci[index].dose * getPower(farmaci[index].unit)),
      );
    } else {
      dose = d_sel;
      unit = u_sel;
    }
  }

  function deactivate() {
    active = false;
    count--;
    if (count == 0) reset = false;
  }

  $effect(() => {
    if (active && submit) changeState();
    else if (reset && active) deactivate();
  });

  $effect(() => {
    if (clear) {
      index = 0;
      dose = undefined;
      unit = "uM";
    }
  });
</script>

<div id="pot" class="relative">
  <button
    aria-label="pot"
    class="rounded-full h-7 w-7 lg:h-15 lg:w-15 md:h-13 md:w-13 border-3 print:border-black!"
    style="border-color: {active ? '#0097D9' : 'black'}"
    onclick={() => {
      if (count == 0) hidden = !hidden;

      active = !active;
      if (active) count++;
      else count--;

      if (count == 0) hidden = !hidden;
    }}
  >
    <div
      class="relative h-full w-full flex items-center justify-center rounded-full overflow-hidden"
    >
      <div
        class="absolute inset-0 z-0"
        style="background-color: {farmaci[index]
          .color}; opacity: {typeof farmaci[index].dose !== 'undefined' &&
        typeof dose !== 'undefined'
          ? (dose * getPower(unit)) /
            (farmaci[index].dose * getPower(farmaci[index].unit))
          : 100}"
      ></div>

      <div
        class="relative z-10 flex flex-col font-bold text-center pointer-events-none"
      >
        {#if ["NT", "Blank"].includes(farmaci[index].name)}
          <p class="text-wrap break-words leading-tight max-w-10 text-sm">
            {farmaci[index].name}
          </p>
        {:else}
          <p class="text-wrap break-words leading-tight max-w-10">
            {dose}
            {#if typeof dose !== "undefined"}
              <span class="text-xs">{unit}</span>
            {/if}
          </p>
        {/if}
      </div>
    </div>
  </button>
</div>
