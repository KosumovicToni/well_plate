<script lang="ts">
  import Mobile from "$lib/components/Mobile.svelte";
  import App from "$lib/components/App.svelte";
  import { onMount } from "svelte";

  let isMobile = $state(false);

  onMount(() => {
    const mediaQuery = window.matchMedia("(max-width: 768px)");

    // Set the start value
    isMobile = mediaQuery.matches;

    // Listen the dimesion changhes
    const handler = (e: MediaQueryListEvent) => (isMobile = e.matches);
    mediaQuery.addEventListener("change", handler);

    return () => mediaQuery.removeEventListener("change", handler);
  });
</script>

{#if isMobile}
  <Mobile />
{:else}
  <App />
{/if}
