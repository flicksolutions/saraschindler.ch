<script>
	import '../app.pcss';
	import '@fortawesome/fontawesome-free/css/solid.min.css';
	import '@fortawesome/fontawesome-free/css/fontawesome.min.css';
	import {
		AppShell,
		AppBar,
		getDrawerStore,
		Drawer,
		initializeStores
	} from '@skeletonlabs/skeleton';
	import { afterNavigate } from '$app/navigation';
	import { page } from '$app/stores';
	import { base } from '$app/paths';

	// Scroll to top on page change (is needed because of skeleton app shell)
	afterNavigate((/** @type import('@sveltejs/kit').AfterNavigate */ params) => {
		const isNewPage = params.from?.url.pathname !== params.to?.url.pathname;
		const elemPage = document.querySelector('#page');
		if (isNewPage && elemPage !== null) {
			elemPage.scrollTop = 0;
		}
	});

	initializeStores();
	const drawerStore = getDrawerStore();

	$: classesActive = (/** @type {string} */ href) =>
		base + href === `/${$page.url.pathname.split('/')[1]}`
			? 'bg-primary-500 hover:text-primary-400 text-secondary-500'
			: 'hover:text-primary-500';

	function drawerOpen() {
		const /** @type {import('@skeletonlabs/skeleton').DrawerSettings} */ s = {
				id: 'topnav',
				position: 'top'
			};
		drawerStore.open(s);
	}
	const pages = [
		{ slug: 'Home', path: '/' },
		{ slug: 'About', path: '#about' },
		{ slug: 'Research', path: '#research' }
	];
</script>

<Drawer height="h-auto">
	<nav class="list-nav">
		<ul>
			{#each pages as page}
				<li>
					<a href={`${base}${page.path}`}>
						<span class="flex-auto">{page.slug}</span>
					</a>
				</li>
			{/each}
		</ul>
	</nav>
</Drawer>

<AppShell>
	<svelte:fragment slot="header">
		<AppBar>
			<svelte:fragment slot="lead">
				<a class="text-xl font-bold uppercase" href={`${base}/`}>me</a>
			</svelte:fragment>
			<nav class="hidden h-full flex-none items-center lg:flex">
				{#each pages as page}
					<a
						href={`${base}${page.path}`}
						class="list-nav-item h-full p-4 {classesActive(page.path)}">{page.slug}</a
					>
				{/each}
			</nav>
			<svelte:fragment slot="trail">
				<button class="btn-icon lg:!hidden" on:click={drawerOpen}>
					<i class="fa-solid fa-bars"></i>
				</button>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<!-- Page Route Content -->
	<div class="px-4">
		<slot />
	</div>
</AppShell>
