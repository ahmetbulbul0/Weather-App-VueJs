<script>
	export default {
		name: 'Weather App',
		data() {
			return {
				api_key: 'openweatheropentoken', // this is a your openweather api token
				url_base: 'https://api.openweathermap.org/data/2.5/',
				query: '',
				weather: {}
			}
		},
		methods: {
			fetchWeather(e) {
				if (e.key == "Enter") {
					fetch(`${this.url_base}weather?q=${this.query}&lang=tr&units=metric&APPID=${this.api_key}`)
						.then(res => {
							return res.json();
						}).then(this.setResults)
				}
			},
			setResults(results) {
				this.weather = results;
			},
			dateBuilder() {
				let d = new Date();
				let months = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül",
					"Ekim", "Kasım", "Aralık"
				];
				let days = ["Pazar", "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi"];
				let day = days[d.getDay()];
				let date = d.getDate();
				let month = months[d.getMonth()];
				let year = d.getFullYear();
				return `${date} ${month} ${day} ${year}`;
			}
		}
	}
</script>

<template>
	<div id="app">
		<main>
			<div class="search-box">
				<input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
			</div>
			<div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
				<div class="location-box">
					<div class="location">
						{{ weather.name }}, {{ weather.sys.country }}
					</div>
					<div class="date">
						{{ dateBuilder() }}
					</div>
				</div>
				<div class="weather-box">
					<div class="temp">
						{{ Math.round(weather.main.temp) }}°c
					</div>
					<div class="weather">
						{{ weather.weather[0].description }}
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	body {
		font-family: "Montserrat", sans-serif;
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
		background-color: #000;
	}

	#app {
		background-color: #000;
		background-size: cover;
		background-position: bottom;
		transition: .4s;
		width: 100%;
		max-width: 460px;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	main {
		width: 100%;
		background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
	}

	@media screen and (max-width: 510px) {
		main {
			padding: 0 25px;
		}
	}

	.search-box {
		width: 100%;
		margin-bottom: 30px;
	}

	.search-box .search-bar {
		display: block;
		width: 100%;
		padding: 15px;
		color: #313131;
		font-size: 20px;
		appearance: none;
		border: none;
		background: none;
		box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 0 16px 0 16px;
		transition: .4s;
	}

	.search-box .search-bar:focus {
		box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.75);
		border-radius: 16px 0 16px 0;
	}

	.location-box .location {
		color: #fff;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
		text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
	}

	.location-box .date {
		color: #fff;
		font-size: 20px;
		font-weight: 300;
		font-style: italic;
		text-align: center;
	}

	.weather-box {
		text-align: center;
	}

	.weather-box .temp {
		display: inline-block;
		padding: 10px 25px;
		color: #fff;
		font-size: 102px;
		font-weight: 900;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.25);
		border-radius: 16px;
		margin: 30px 0;
		box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}

	.weather-box .weather {
		color: #fff;
		font-size: 48px;
		font-weight: 700;
		font-style: italic;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
		text-transform: capitalize;
	}
</style>