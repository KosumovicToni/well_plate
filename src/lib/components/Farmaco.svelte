<script lang="ts">
  let { name, dose, unit, color } = $props();
</script>

<div
  class="flex flex-row items-center justify-between p-3 m-1 border-2 rounded-lg font-bold bg-white w-full lg:max-w-3/4 gap-2 text-xl"
>
  <div class="min-w-0 lg:max-w-12s0 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {name}
    </p>
  </div>
  <div class="min-w-0 max-w-64 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {dose}
    </p>
  </div>
  <div class="min-w-0 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {unit}
    </p>
  </div>
  <div
    class="w-6 h-6 rounded-full border border-black/5 flex-shrink-0"
    style="background-color: {color}"
  ></div>
</div>
