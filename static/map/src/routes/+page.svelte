<script lang="ts">
    import { onMount } from 'svelte';
    import CoffeeFilter from './CoffeeFilter.svelte';
    import Card from './Card.svelte';

    interface Coffee {
        id: number;
        title: string;
        image: string;
        rating: number;
        available: boolean;
    }

    let coffees: Coffee[] = [];
    let filteredCoffees: Coffee[] = [];
    let error = '';
    let loading = true;
    let showOnlyAvailable = false;

    // Función para filtrar los cafés
    function applyFilter() {
        filteredCoffees = showOnlyAvailable 
            ? coffees.filter(coffee => coffee.available) 
            : coffees;
    }

	function filterCoffees() {
        filteredCoffees = showOnlyAvailable
            ? coffees.filter(coffee => coffee.available)
            : coffees;
    }

    // Maneja el evento de filtro
    function handleFilter(event: CustomEvent) {
        showOnlyAvailable = event.detail.showOnlyAvailable;
        applyFilter();
    }

    // Función para manejar la actualización del rating
    function updateRating(event: CustomEvent) {
        const { id, rating } = event.detail;
        coffees = coffees.map(coffee => 
            coffee.id === id ? { ...coffee, rating } : coffee
        );
        applyFilter();  // Re-aplica el filtro después de actualizar el rating
    }

    onMount(async () => {
        try {
            const response = await fetch('https://api.sampleapis.com/coffee/hot');
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            const fetchedCoffees = await response.json();
            coffees = fetchedCoffees.map((coffee: any) => ({
                ...coffee,
                rating: 0,
                available: Math.random() < 0.5  // 50% chance of being available
            }));
            applyFilter();
        } catch (e) {
            console.error('Error fetching coffee data:', e);
            error = e instanceof Error ? e.message : String(e);
        } finally {
            loading = false;
        }
    });

	filterCoffees();
</script>

<main>
    <h1>Our selection</h1>
    <p>Introducing our Coffee Collection, a selection of unique coffees from different roast types and origins, expertly roasted in small batches and shipped fresh weekly.</p>

    <CoffeeFilter on:filter={handleFilter} />

	{#if filteredCoffees.length === 0}
	<p>No coffees found.</p>
	{:else}
	{#each filteredCoffees as coffee (coffee.id)}
    <Card {coffee} on:rate={updateRating} />
	{/each}
	{/if}
</main>


<style>
main {
	width: 90%; /* Se ajusta al 90% del ancho del viewport, puede ajustarse según necesidad */
	max-width: 600px; /* Ancho máximo para pantallas más grandes */
	border: 1px solid rgb(24, 27, 32);
	border-radius: 10px;
	box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.5); /* Mejora la sombra */
	margin: 20px auto; /* Margin arriba y abajo para separarlo */
	position: relative;
	background-color: #1B1D1F;
	color: white;
	font-family: 'Courier New', Courier, monospace;
	padding: 20px; /* Espacio interno */
	box-sizing: border-box; /* Incluye borde y padding dentro de las dimensiones */
}
</style>