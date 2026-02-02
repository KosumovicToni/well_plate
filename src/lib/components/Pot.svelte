<script lang="ts">
    let color:string = $state("white");
    let opacity:number = $state(100);
    let hidden = $state(true)
</script>

<div class="relative">
    <div class="rounded-full h-12 w-12 border-3" onclick={()=>{ hidden =! hidden}}>
        <div class="rounded-full h-full w-full"
        style="background-color: {color}; opacity: {opacity/100}" ></div>
    </div>
    
    {#if !hidden}
    <div class="absolute z-10 bg-white border-2 rounded-lg w-38 h-auto p-2">
        <div class="grid grid-cols-2 ">
            <label for="">Color : </label>
            <input type="text" bind:value={color} class="border-b-1">
        </div>
        <div class="">
            <label for="">Opacity : </label>
            <input type="number" min="0" max="100" bind:value={opacity} class="border-b-1">
        </div>
    </div>
    {/if}
</div>

<style>
    input:focus{
    outline: none;
}
</style>