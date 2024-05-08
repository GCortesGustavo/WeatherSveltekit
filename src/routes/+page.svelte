<script>
    import {onMount} from "svelte"

    let weatherData  = null


    const fetchData = async () => {
        const url = 'https://weatherapi-com.p.rapidapi.com/current.json?q=Buenos%20Aires';
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
            weatherData = result
        } catch (error) {
            console.error(error);
        }
    };

    onMount(() => {
        fetchData()
    });
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

<h1>Hola</h1>


<style>
    h1 {
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }
</style>
