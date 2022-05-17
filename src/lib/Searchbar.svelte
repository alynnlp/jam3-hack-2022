<script>
	import { onMount } from 'svelte';
// import Country from '../routes/country.svelte';
// import Index from '../routes/index.svelte';
	import CountryStore from '../store/CountryStore';
	import PinnedStore from '../store/PinnedStore';
  const searchUrl =
	"https://restcountries.com/v3.1/all";
	
	export let countries = [];
	let searchTerm = "";
  let results = [];
	let pinnedCountry = [];
	{CountryStore.subscribe((data) => {
		countries = data;
	})}
	{PinnedStore.subscribe((data) => {
		pinnedCountry = data;
	})}

  const searchFunction = async event => {
    event.preventDefault();
    const response = await fetch(`${searchUrl}`);
    const json = await response.json();
    results = json.filter(data => data.name.common.includes(searchTerm)).splice(0, 10);
  };

	const handleAdd = (newPin) => {
		// const currentPinnedNo = PinnedStore.length;
		// if(currentPinnedNo < 5) {
			PinnedStore.update(current => {
				return [newPin, ...current];
			})
		// }
	}
	const handleDelete = (newPin) => {
		// const currentPinnedNo = PinnedStore.length;
		// if(currentPinnedNo < 5) {
			PinnedStore.update(current => {
				return current.filter(country => {
					console.log('?', country.name.common == newPin.name.common)
					console.log('country', country)
					console.log('newPin', newPin)
					country.name.common !== newPin.name.common
				})
			})
		// }
	}

	onMount(async ()=> {
		const response = await fetch(`${searchUrl}`);
    const json = await response.json();
		results = json.splice(0,10);
		//save to store
		CountryStore.update(currentCountry => {
			return [results, ...currentCountry];
		})
	});

</script>

<style>
	.country {
		padding-bottom: 1em;
		display: flex;
		justify-content: space-between;
	}
	.countryWrapper{
		display: flex;
		flex-direction: row;
	}
	.countryFlag {
		padding-right: 2em;
	}
	.countryName {
		font-weight: bold;
	}
	.countryContinent {
		color: grey;
	}
  .searchBar {
    margin: 2em 0 ;
		width: 400px;
		border-radius: 2px;
		border-color: purple;
		padding: 1em;
  }

	.pinned {
		padding: 1em 0;
		border-bottom: 1px solid grey;
	}

	.result {
		padding: 2em 0;
	}

	.noResults {
		margin: auto 0;
		text-align: center;
		font-size: 4em;
	}
</style>

<main class="container container-small">
	<!-- <form on:change={searchFunction} class="form"> -->
    <div class="form-group">
      <!-- <label for="searchTerm">Enter Search Term</label> -->
      <input 
				on:keyup={searchFunction}
				id="searchTerm" 
				class="searchBar"
				type="text" 
				bind:value={searchTerm} 
				placeholder={'Search for a country'}>
    </div>
  <div class="pinned">
		<h2>Pinned 📌</h2>
		<div>	
			{#if pinnedCountry && pinnedCountry.length > 0}
				{#each pinnedCountry as pin}
					<div class="country" >
						<div class="countryWrapper">
		
								<div class="countryFlag">{pin.flag}</div>
								<div>
									<div class="countryName">{pin.name.common}</div>
									<div class="countryContinent">{pin.continents[0]}</div>
								</div>
							
						</div>
						<button class="add" on:click={handleDelete(pin)}>➖</button>
					</div>
				{/each}
			{/if}
		</div>
	</div>
  <div class="result">
		<!-- <form on:submit|preventDefault={handleAdd}> -->
    {#if results.length > 0}
      {#each results as result}
			<div class="country">
				<div class="countryWrapper">
					<div class="countryFlag">{result.flag}</div>
					<div>
						<div class="countryName">{result.name.common}</div>
						<div class="countryContinent">{result.continents[0]}</div>
					</div>
				</div>
				<button class="add" on:click={handleAdd(result)}>➕</button>
			</div>
      {/each}
    {:else if results.length == 0 && searchTerm !== ""}
			<div class="noResults">No results</div>
    {:else if results.length && searchTerm === ""}
			<div>	
				{#each countries as country}
				<div>{country.name}</div>
				{/each}
			</div>
    {/if}
	<!-- </form> -->
  </div>
</main>