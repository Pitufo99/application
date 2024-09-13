y<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    export let coffee = { id: 0, title: '', image: '', rating: 0, available: false };  // Recibe el objeto coffee como prop

    const dispatch = createEventDispatcher();  // Crea el despachador de eventos

    function rate(rating: number) {
        dispatch('rate', { id: coffee.id, rating });  // Envía el evento con el ID del café y el nuevo rating
    }
</script>

<div class="card">
    <div class="card-img">
        <img src={coffee.image ?? "src/lib/images/default-coffee.jpg"} alt={coffee.title}>
    </div>
    <div class="card-inf">
        <div class="name-prize">
            <h2 class="name">{coffee.title}</h2>
            <h2 class="prize">${(Math.random() * 10).toFixed(2)}</h2>
        </div>
        <div class="rating">
            {#each Array(5) as _, i}
            <button 
                on:click={() => rate(i + 1)} 
                class:filled={i < coffee.rating}
                aria-label="Rate {i + 1} star{i !== 0 ? 's' : ''}"
                title="Rate {i + 1} star{i !== 0 ? 's' : ''}"
            >
                ★
            </button>
            {/each}
        </div>
        <div class="availability">
            {coffee.available ? '✅ Available' : '❌ Not Available'}
        </div>
    </div>
</div>

<style>
    .card {
    width: 200px;
    padding: 10px;
    text-align: center;
    border-radius: 15px;
    margin: 15px;
    display: inline-block;
}

.card-img {
    width: 100%;
    height: 120px;
    padding: 5px;
    text-align: center;
    border-radius: 10px;
    margin-bottom: 10px;
}

img {
    width: 100%;
    height: 100%;
    object-fit: cover; /* Para asegurarte que la imagen cubra todo sin deformarse */
    border-radius: 5px;
}

.card-inf {
    display: flex;
    flex-direction: column;
    align-items: flex-start; /* Alinea el contenido a la izquierda */
}

.name-prize {
    display: flex;
    justify-content: space-between;
    width: 100%;
}

.name {
    font-size: 1rem;
    text-align: left;
}

.prize {
    font-size: 1rem;
    font-weight: bold;
    text-align: right;
    background-color: antiquewhite;
    color: black;
    border-radius: 5px;
}
.rating button {
        background: none;
        border: none;
        font-size: 1.5rem;
        color: #ddd;
        cursor: pointer;
        transition: color 0.2s;
        padding: 0;
        margin: 0 2px;
    }
    .rating button:hover,
    .rating button.filled {
        color: gold;
    }
</style>