<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    let showOnlyAvailable = false;

    function toggleFilter() {
        dispatch('filter', { showOnlyAvailable });
    }
</script>

<div class="filter-control">
    <label class="switch">
        <input type="checkbox" bind:checked={showOnlyAvailable} on:change={toggleFilter}>
        <span class="slider round"></span>
    </label>
    <span class="label-text">Show only available coffees</span>
</div>

<style>
    .filter-control {
        margin-bottom: 20px;
        font-family: Arial, sans-serif;
        display: flex;
        align-items: center;
    }

    .label-text {
        margin-left: 10px;
    }

    /* Estilos para el switch */
    .switch {
        position: relative;
        display: inline-block;
        width: 50px;
        height: 24px;
    }

    .switch input {
        opacity: 0;
        width: 0;
        height: 0;
    }

    .slider {
        position: absolute;
        cursor: pointer;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: #ccc;
        transition: 0.4s;
        border-radius: 24px; /* Hace el switch redondeado */
    }

    .slider:before {
        position: absolute;
        content: "";
        height: 18px;
        width: 18px;
        left: 4px;
        bottom: 3px;
        background-color: white;
        transition: 0.4s;
        border-radius: 50%;
    }

    input:checked + .slider {
        background-color: #4caf50; /* Color cuando está activado */
    }

    input:checked + .slider:before {
        transform: translateX(26px);
    }

    /* Redondeado del toggle */
    .slider.round {
        border-radius: 24px;
    }

    .slider.round:before {
        border-radius: 50%;
    }
</style>