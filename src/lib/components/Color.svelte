<script lang="ts">
    let {color, selected} = $props();
</script>

<div class="w-6 h-6 rounded-full hover:-translate-y-px transition-all" style="background-color: {color};border: white; border-width:0.3rem; border-style:{selected? "hidden" : "solid"}"
    ></div>