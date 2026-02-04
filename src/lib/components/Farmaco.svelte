<script lang="ts">
  let { name, dose, color } = $props();
</script>

<div
  class="flex items-center justify-between p-4 m-1 border-2 rounded-lg font-bold bg-white min-w-0 gap-4"
>
  <div class="min-w-0 max-w-68 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {name}
    </p>
  </div>
  <div class="min-w-0 max-w-64 flex-1">
    <p class="text-wrap break-words leading-tight text-gray-800">
      {dose}
    </p>
  </div>
  <div
    class="w-6 h-6 rounded-full border border-black/5 flex-shrink-0"
    style="background-color: {color}"
  ></div>
</div>
