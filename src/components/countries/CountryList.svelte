<script>
  let countryCodesPromise = []
  let countriesInfo = undefined
  const getCountryCodes = async () => fetch(
    'https://restcountries-v1.p.rapidapi.com/all',
    {
      headers: {
        'X-RapidAPI-Host': 'restcountries-v1.p.rapidapi.com',
        'X-RapidAPI-Key': 'iA8iGIOx5CmshSj0Grfk03n2T8Aop17s53Ojsnk1DeBR67bCKJ'
      }
    })
    .then(response => response.json().then(json => json))
    .catch(error => error)

  const handleGetCountryCodes = () => {
    countriesInfo = getCountryCodes()
    countryCodesPromise = getCountryCodes()
  }
</script>

<style>
  ul {
    list-style-type: none;
    display: flex;
    flex-direction: column;
    margin: 0;
    padding: 0
  }

  main {

  }

  ul li:first-child {
    position: sticky;
    top: 0;
    display: flex;
    flex-basis: 100%;
    padding: 0;
    justify-content: space-evenly;
    background: #fff;
    font-weight: bold;
    height: 30px;
  }

  li {
    border-bottom: 1px solid #ccc;
    display: flex;
    justify-content: space-evenly;
    padding: 0;
  }

  li div:first-child {
    border-left: 1px solid #ccc;
  }

  li div {
    display: flex;
    flex-basis: 25%;
    border-right: 1px solid #ccc;
    font-size: 16px;
    height: 30px;
    padding: 0 15px;
    align-items: center;
  }
</style>

{#if !countriesInfo}
  <button disabled={countryCodesPromise.length > 0} on:click={handleGetCountryCodes}>Get Country Codes</button>
{/if}

<main>
  {#await countryCodesPromise}
    <p>...waiting</p>
  {:then countryCodes}
    <ul>
      {#if countryCodes.length}
        <li>
          <div>Country</div>
          <div>Capital</div>
          <div>Population</div>
          <div>Currency</div>
        </li>
      {/if}
      {#each countryCodes as countryInfo}
        <li>
          <div>{countryInfo.name}</div>
          <div>{countryInfo.capital}</div>
          <div>{countryInfo.population}</div>
          <div>{countryInfo.currencies[0]}</div>
        </li>

      {/each}
    </ul>

  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</main>
