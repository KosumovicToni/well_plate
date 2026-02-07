<script lang="ts">
  let color: string | undefined = $state("white");
  let dose: number | undefined = $state();
  let unit: string = $state("uM");

  let ref_dose: number | undefined = $state();
  let ref_unit: string = $state("uM");

  let active = $state(false);

  let {
    submit = $bindable(),
    reset = $bindable(),
    selected,
    hidden = $bindable(),
    count = $bindable(),
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
    color = selected.color;
    dose = selected.dose;
    unit = selected.unit;
    ref_unit = selected.ref_unit;
    ref_dose = selected.ref_dose;
    if (count == 0) submit = false;
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
</script>

<div class="relative">
  <button
    aria-label="pot"
    class="rounded-full h-11 w-11 lg:h-16 lg:w-16 md:h-14 md:w-14 border-3 print:border-black!"
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
        <p class="text-wrap break-words leading-tight max-w-10">
          {dose}
          {#if typeof dose !== "undefined"}
            <span class="text-xs">{unit}</span>
          {/if}
        </p>
      </div>
    </div>
  </button>
</div>
