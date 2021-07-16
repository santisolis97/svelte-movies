<script>
	import StarRating from 'svelte-star-rating';
	import noposter from '../images/noposter.png';
	export let title;
	export let overview;
	export let poster;
	export let backdrop;
	export let release;
	export let rate;
	// export let genres;
	export let genresList;
</script>

<div class="movieCard">
	<div class="movie_card" id="bright">
		<div class="info_section">
			<div class="movie_header">
				{#if poster}
					<img
						class="locandina"
						src={'https://image.tmdb.org/t/p/original' + poster}
						alt="movieImage"
					/>
				{:else}
					<img class="locandina" src={noposter} alt="movieImage" />
				{/if}
				<h1>{title}</h1>
				<h4>{release}</h4>
				<StarRating rating={rate / 2} />
				<p class="type">
					{#each genresList as genre, index}
						{#if index + 1 < genresList.length}
							{genre.name + ', '}
						{:else}
							{genre.name + '.'}
						{/if}
					{/each}
				</p>
			</div>
			<div class="movie_desc">
				<p class="text">
					{overview}
				</p>
			</div>
		</div>
		{#if backdrop}
			<div
				class="blur_back bright_back"
				style={'background: url(https://image.tmdb.org/t/p/original' + backdrop + ');'}
			/>
		{:else}
			<div
				class="blur_back bright_back"
				style="background: rgb(255,255,255);background: linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(97,97,97,1) 35%);"
			/>
		{/if}
	</div>
</div>

<style>
	@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,700,800');
	.movieCard {
		font-family: 'Montserrat';
	}
	.movie_card {
		position: relative;
		display: block;
		width: 800px;
		height: fit-content;
		margin: 80px auto;
		overflow: hidden;
		border-radius: 10px;
		transition: all 0.4s;
		box-shadow: 0px 0px 120px -25px rgba(0, 0, 0, 0.5);
	}
	.movie_card:hover {
		transform: scale(1.05);
		box-shadow: 0px 0px 80px -25px rgba(0, 0, 0, 0.5);
		transition: all 0.4s;
	}
	.movie_card .info_section {
		position: relative;
		width: 100%;
		height: 100%;
		background-blend-mode: multiply;
		z-index: 2;
		border-radius: 10px;
	}
	.movie_card .info_section .movie_header {
		position: relative;
		padding: 25px;
		height: 40%;
	}
	.movie_card .info_section .movie_header h1 {
		color: black;
		font-weight: 400;
	}
	.movie_card .info_section .movie_header h4 {
		color: #555;
		font-weight: 400;
	}

	.movie_card .info_section .movie_header .type {
		display: inline-block;
		color: #959595;
		margin: 0;
	}
	.movie_card .info_section .movie_header .locandina {
		position: relative;
		float: left;
		margin-right: 20px;
		height: 120px;
		box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.5);
	}
	.movie_card .info_section .movie_desc {
		padding: 25px;
		height: 50%;
	}
	.movie_card .info_section .movie_desc .text {
		color: #545454;
		margin: 0;
	}
	.movie_card .blur_back {
		position: absolute;
		top: 0;
		z-index: 1;
		height: 100%;
		right: 0;
		background-size: cover !important;
		border-radius: 11px;
	}
	@media screen and (min-width: 768px) {
		.movie_header {
			width: 65%;
		}
		.movie_desc {
			width: 50%;
		}
		.info_section {
			background: linear-gradient(to right, #e5e6e6 50%, transparent 100%);
		}
		.blur_back {
			width: 80%;
			background-position: center !important;
		}
	}
	@media screen and (max-width: 768px) {
		.movie_card {
			width: 95%;
			margin: 70px auto;
			min-height: 350px;
			height: auto;
		}
		.blur_back {
			width: 100%;
			background-position: 50% 50% !important;
		}
		.movie_header {
			width: 100%;
			margin-top: 85px;
		}

		.info_section {
			background: linear-gradient(to top, #e5e6e6 50%, transparent 100%);
			display: inline-grid;
		}
	}
</style>
