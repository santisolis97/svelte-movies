<script>
	export const prerender = true;
	import logo from '../images/logo.svg';
	import { onMount } from 'svelte';
	import MovieCard from '$lib/movieCard.svelte';
	import Fa from 'sveltejs-fontawesome';
	import { faSearch } from '@fortawesome/free-solid-svg-icons';
	import { Jumper } from 'svelte-loading-spinners';

	const apiKey = import.meta.env.VITE_API_KEY;

	let inputMovie = '';
	let data = [];
	let movies = [];
	let genres = [];
	let currentPage = 1;
	let loading = true;
	const onKeyPress = () => {
		currentPage = 1;
		search();
	};
	$: currentPage, search();
	const search = async () => {
		loading = true;
		if (inputMovie === '') {
			getMovies();
		} else {
			const res = await fetch(
				`https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&language=en-US&query=${inputMovie}&page=${currentPage}&include_adult=true`
			);
			data = await res.json();

			movies = data.results;
		}
		loading = false;
	};
	const getMovies = async () => {
		loading = true;
		const res = await fetch(
			`https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&language=en-US&sort_by=popularity.desc&include_adult=true&include_video=false&page=${currentPage}`
		);
		data = await res.json();

		movies = data.results;
		if (res.ok) {
			return movies;
		}
	};
	const getGenres = async () => {
		const resGenres = await fetch(
			`https://api.themoviedb.org/3/genre/movie/list?api_key=${apiKey}&language=en-US`
		);
		genres = await resGenres.json();
		genres = genres.genres;
		if (resGenres.ok) {
			return genres;
		}
	};
	onMount(async () => {
		movies = search();
		genres = getGenres();
	});
</script>

<svelte:head>
	<title>Svelte-movies</title>
</svelte:head>

<div class="header">
	<div class="logo">
		<img src={logo} alt="movies-logo" />
	</div>
	<div class="inputDiv">
		<input
			on:keyup|preventDefault={(event) => {
				if (event.keyCode === 13) {
					onKeyPress();
				}
			}}
			bind:value={inputMovie}
			class="movies-input"
			placeholder={'Enter movie title and press enter '}
			type="text"
		/>
		<div class="icon">
			<Fa icon={faSearch} size="sm" color="#F83D00" />
		</div>
	</div>
</div>
<div class="moviesList">
	{#await genres then value}
		{#if !loading}
			{#each movies as movie}
				<MovieCard
					title={movie.title}
					overview={movie.overview}
					poster={movie.poster_path}
					backdrop={movie.backdrop_path}
					release={movie.release_date}
					rate={movie.vote_average}
					genresList={value.filter((item) => movie.genre_ids.includes(item.id))}
				/>
			{/each}
			<div class="pagesNavigator">
				<button
					class="navigatorButton clickable"
					on:click={() => {
						currentPage--;
					}}
				>
					{'<'}</button
				>
				<button class="navigatorButton"> {currentPage}</button>
				<button
					class="navigatorButton clickable"
					on:click={() => {
						currentPage++;
					}}
					>{'>'}
				</button>
			</div>
		{/if}
		{#if loading}
			<div class="loader">
				<Jumper size="60" color="#FF3E00" unit="px" duration="1s" />
			</div>
		{/if}
	{/await}
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Oswald&display=swap');
	.header {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	.movies-input {
		width: 100%;
		border-radius: 20px;
		height: 30px;
		padding: 5px 10px;
		margin: 0 auto;
		border: none;
		box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
	}
	.movies-input:focus-visible {
		outline: none;
	}
	.movies-input::placeholder {
		color: #585858;
	}
	.inputDiv {
		margin-top: 50px;
		position: relative;
		width: 70%;
		margin-left: auto;
		margin-right: auto;
	}
	.icon {
		position: absolute;
		right: 0;
		top: 50%;
		transform: translate(0, -40%);
	}
	.logo {
		height: 15vh;
	}
	.logo img {
		width: 100%;
		height: 100%;
	}
	.pagesNavigator {
		margin: 0 auto;
		display: flex;
		column-gap: 5px;
		justify-content: center;
	}
	.navigatorButton {
		background-color: var(--accent-color);
		display: flex;
		font-family: monospace;
		height: fit-content;
		align-items: center;
		justify-content: center;
		box-shadow: none;
		border: none;
		padding: 7px 10px;
		color: white;
		border-radius: 10px;
		border: 2px solid white;
		box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
	}
	.navigatorButton.clickable {
		cursor: pointer;
	}
	.loader {
		margin: 0 auto;
		width: fit-content;
		/* padding: 200px 0; */
	}
</style>
