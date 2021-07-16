<script>
	export const prerender = true;
	import './home.css';

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
		<div class="faicon">
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
