<script lang="ts">
  let color: string | undefined = $state("white");
  let dose: number | undefined = $state();
  let unit: string = $state("uM");
  let pot_name: string = $state("");

  let ref_dose: number | undefined = $state();
  let ref_unit: string = $state("uM");

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

    color = farmaci[index].color;
    pot_name = farmaci[index].name;
    ref_unit = farmaci[index].unit;
    ref_dose = farmaci[index].dose;

    if (index < 3) {
      dose = undefined;
      ref_dose = undefined;
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
    color = farmaci[index].color;
  });

  $effect(() => {
    if (clear) {
      color = "white";
      dose = undefined;
      unit = "uM";
      ref_dose = undefined;
      pot_name = "";
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
        style="background-color: {color}; opacity: {typeof ref_dose !==
          'undefined' && typeof dose !== 'undefined'
          ? (dose * getPower(unit)) / (ref_dose * getPower(ref_unit))
          : 100}"
      ></div>

      <div
        class="relative z-10 flex flex-col font-bold text-center pointer-events-none"
      >
        {#if ["NT", "Blank"].includes(pot_name)}
          <p class="text-wrap break-words leading-tight max-w-10 text-sm">
            {pot_name}
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
