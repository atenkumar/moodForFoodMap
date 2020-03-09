
<script>
	import Map from './Map.svelte';
	import MapMarker from './MapMarker.svelte';
	import {onMount} from 'svelte'
	import { writable } from 'svelte/store'

	//using a writable store to hold our data
	let businesses = writable([]);
	let craving = writable("");
	let lat = 0;
	let long = 0;
	const length = writable(0);

	onMount(() => {
		getLocation();
	})
	function getLocation()
	{
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition(setLocation);
		} else {
			alert("Geolocation is not supported by this browser.");
		}
	}

	function setLocation(position)
	{
		lat = position.coords.latitude;
		long = position.coords.longitude;
	}
	
 	function restAsk(){
		fetch('https://cors-anywhere.herokuapp.com/https://api.yelp.com/v3/businesses/search?term=' + $craving + '&latitude=' + lat + '&longitude=' + long + '&sort_by=distance', {
			headers: {
				"Authorization": "Bearer LiGV8vc0W0UyHl3bCiywBSyowbmik2x0NnW2sAk7cD3UL7hjt2Njv5tCyIPDK0fXUKJfLdJ8Skw-jdyUqRM-oPBdyPIlGYngBhugVVZU9UNks_ARjx3KJPwMASlSXnYx"
			}
		})
			.then((response) => {
				return response.json();
			})
			.catch((err) => {
				console.error(error)
			})
			.then((data) => {
				console.log(data);
				businesses.set(data.businesses);
				length.set(1);
			});
	}
	const mileConversion = i => `<h1>${(i*0.000621371192).toFixed(1)} mi</h1>`;
	const priceCon = p => `<h1>${p}</h1>`;
	const ratingCon = r => `<h1>${r}</h1>`;
	const addressCon = a => `<h2>${a}</h2>`;

</script>


<style>
	h1, h2, p {
		text-align: center;
		margin: 0 auto;
	}

	h1 {
		font-size: 1em;
		font-weight: 500;
		margin: 0 0 0.5em 0;
	}

	img {
		width: 100%;
		max-width: 400px;
		margin: 0 0 1em 0;
	}

	p {
		margin: 1em auto;
	}

	@media (min-width: 480px) {
		h1 {
			font-size: 4em;
		}
	}

	.business{
		border: 1px solid #ddd;
		border-radius: 1em;
		margin: 2em 1em;
		box-sizing: content-box;
		padding: 1em 2em;
	}
	
	.business img{
		height: 2em;
		margin-right: 1em;
		vertical-align: middle;
	}
	
	span {
		background-color: #eee;
		padding: 1em 2em;
		margin: 1em 0;
		margin-right: 1em;
		display: inline-block;
		border-radius: 10em;
		font-weight: 500;
		font-size: medium;
		text-align: center;
	}
	
	.center{
		text-align: center;
	}

	.textbox{
		height:30px;
		width:300px;
    	font-size:14pt;
		text-align: center;
	}

	.submitbutton
	{
		height:50px;
		width:200px;
	}
</style>

<svelte:head>
	<title>MoodForFood.com</title>
</svelte:head>


<h1>What are you in the mood for?</h1>

<p><strong>
	<input required type ="text" id="craving" bind:value={$craving} class ="textbox"><br>
	<br>
	<button on:click={restAsk} type="button" class="submitbutton">Find Restaurants!</button>
</strong></p>


{#each $businesses as business}
	<div class = "business">
		<h2>
			{#if business.url && business.image_url}
				<img src={business.image_url} style="width:150px;height:150px;" align="middle" alt="Image of {business.name}" />
			{/if}
			<br><a href={business.url}>{business.name}</a><br>
		</h2>
		<div class= "center">
			<span>
				{#if business.price}
					Price: {@html priceCon(business.price)}
				{:else}
					Price: {@html priceCon("N/A")}
				{/if}
			</span>
			<span>
				{#if business.rating}
					Rating: {@html ratingCon(business.rating)}
				{:else}
					Rating: {@html ratingCon("N/A")}
				{/if}
			</span>
			<span>
				Distance: {@html mileConversion(business.distance)} 
			</span>
			<span>
				{#if business.location.address1}
					Address: {@html addressCon(business.location.address1 + " " + business.location.city + ", " + business.location.state + " " + business.location.zip_code)}
				{:else}
					Address: {@html addressCon("Not listed - please check link")}
				{/if}
			</span>
		</div>
	</div>
{/each}

{#if $length != 0}
	<h1 class="center">Map:</h1>
	<Map lat={lat} lon={long} zoom={15} align=center class="center">
		<MapMarker lat={lat} lon={long} label={"Current Location"}/>
		{#each $businesses as business}
			<MapMarker lat={business.coordinates.latitude} lon={business.coordinates.longitude} label={business.name}/>
		{/each}
	</Map>
{/if}

