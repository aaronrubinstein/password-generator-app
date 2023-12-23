<script>
    import { onMount } from 'svelte';
    
    export let max;
    export let value;

    $: progress = (value / max) * 100;

    function updateShading() {
        document.documentElement.style.setProperty('--slider-shade-pct', `${progress}%`);
    }

    onMount(() => {
        updateShading();
    })

</script>

<input 
    type="range" 
    min="0" 
    {max} 
    bind:value 
    on:input={updateShading} >

<style>
    
    input[type="range"] {
        -webkit-appearance: none;
        appearance: none;
        background: transparent;
        cursor: pointer;
        width: 100%;
    }

    input[type="range"]::-webkit-slider-runnable-track {
        height: 8px;
        /* border-radius: 5px; */
        background: linear-gradient(to right, var(--neon-green) var(--slider-shade-pct), var(--very-dark-grey) var(--slider-shade-pct));
    }

    input[type="range"]::-moz-range-track {
        height: 8px;
        /* border-radius: 5px; */
        background: var(--very-dark-grey);
    }

    input[type="range"]::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        height: 28px;
        width: 28px;
        border-radius: 28px;
        background-color: var(--almost-white);
        /* to vertically center thumb: margin-top = (track height in pixels / 2) - (thumb height in pixels /2)  */
        margin-top: -10px;
        transition: background-color .2s;
    }

    input[type="range"]::-moz-range-thumb {
        height: 28px;
        width: 28px;
        border-radius: 28px;
        border: none;
        background-color: var(--almost-white);
    }

    input[type="range"]::-webkit-slider-thumb:hover {
        background-color: var(--very-dark-grey);
        border: 2px solid var(--neon-green);
    }

    input[type="range"]::-webkit-slider-thumb:active {
        background-color: var(--very-dark-grey);
        border: 2px solid var(--neon-green);
    }

    input[type="range"]::-moz-range-thumb:hover {
        background-color: var(--very-dark-grey);
        border: 2px solid var(--neon-green);
    }

    input[type="range"]::-moz-range-thumb:active {
        background-color: var(--very-dark-grey);
        border: 2px solid var(--neon-green);
    }

</style>