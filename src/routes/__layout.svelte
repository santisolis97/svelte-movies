<script>
	import Header from '$lib/header/Header.svelte';
	import '../app.css';
	import Fa from 'sveltejs-fontawesome';
	import { faGithub } from '@fortawesome/free-brands-svg-icons';
</script>

<Header />

<main>
	<slot />
</main>

<footer>
	<kbd
		><a class="githubLink" href="github.com/santisolis97"
			><Fa icon={faGithub} size="sm" color="#F83D00" /><span class="icon">@santisolis97</span></a
		></kbd
	>
</footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 1024px;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px;
		font-size: 30px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 40px 0;
		}
	}
	.githubLink {
		display: flex;
		align-items: center;
	}
	.icon {
		padding-left: 10px;
	}
</style>
