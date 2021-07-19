<script context="module">
	// On Load it gets the page slug and returns it.
	export async function load(ctx) {
		let slug = ctx.page.params.movie;
		let movie;
		const apiKey = import.meta.env.VITE_API_KEY;
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/${slug}?api_key=${apiKey}&language=en-US`
		);

		movie = await res.json();
		console.log(movie);
		return { props: { movie } };
	}
</script>

<script>
	import MovieCard from '$lib/movieCard.svelte';
	import { onMount } from 'svelte';
	export let movie;
</script>

<MovieCard
	id={movie.id}
	title={movie.title}
	overview={movie.overview}
	poster={movie.poster_path}
	backdrop={movie.backdrop_path}
	release={movie.release_date}
	rate={movie.vote_average}
	genresList={movie.genres}
/>

<style>
</style>
