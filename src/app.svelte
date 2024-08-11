<script>
	import { Router, Link, Route } from 'svelte-routing';

	export let url = '';
</script>

<Router {url}>
	<nav>
		<Link to="/">Home</Link>
	</nav>
</Router>
