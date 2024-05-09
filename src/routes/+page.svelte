<script>
    import { onMount } from "svelte";

    let weatherData = null;
    let query = "Buenos Aires"; // Definimos la ubicación inicial aquí

    const getWeatherFrom = async (query) => {
        const url = `https://weatherapi-com.p.rapidapi.com/current.json?q=${query}`;
        const options = { 
            method: 'GET',
            headers: {
                'X-RapidAPI-Key': '8468a5cd3fmshe37197beca0be36p181ec5jsn7f2b19de36b7',
                'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com'
            }
        };

        try {
            const response = await fetch(url, options);
            const result = await response.json();
            return result;
        } catch (error) {
            console.error(error);
            return null;
        }
    };

    onMount(async () => {
        weatherData = await getWeatherFrom(query); // Llamamos a la función con la ubicación inicial
    });

    // Función para actualizar los datos meteorológicos basados en la consulta de ubicación
    const updateWeather = async () => {
        weatherData = await getWeatherFrom(query);
    };
</script>


{#if weatherData}
<h1>{weatherData.location.name}</h1>
    <p>Temperatura: {weatherData.current.temp_c}°C</p>
    <p>Condición: {weatherData.current.condition.text}</p>
    {#if weatherData.current.is_day === 1}
        <p>¡Es de día!</p>
    {:else}
        <p>¡Es de noche!</p>
    {/if}
{:else}
    <p>Cargando...</p>
{/if}

<!-- Búsqueda de ubicación -->
<input type="text" placeholder="Ingrese una ubicación" bind:value={query}>
<button on:click={() => updateWeather(query)}>Buscar</button>


<style>
    h1 {
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }
</style>
