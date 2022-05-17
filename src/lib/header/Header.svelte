<style>	
	.header {
		width: 100vw;
		/* background-color: white; */
		position: relative;
		top: 0;
		padding-left: 20px;
	}
	.headerTitle { 
		line-height: 8px;
	}
</style>

<header>
	<nav>
		<!-- <ul>
			<li class:active={$page.url.pathname === '/about'}>
				<a sveltekit:prefetch href="/about">About</a>
			</li>
			<li class:active={$page.url.pathname === '/todos'}>
				<a sveltekit:prefetch href="/todos">Todos</a>
			</li>
		</ul> -->
		<div class="header">
			<p class:active={$page.url.pathname === '/'}>
				<a sveltekit:prefetch href="/">🏠</a>
			</p>
			<p class="headerTitle">Countries of the World</p>
			<p class="headerTitle">JAM3 Hackathon 2022 MAY - aileen</p>
		</div>
	</nav>

	<div class="corner">
		<!-- TODO put something else here? github link? -->
	</div>
</header>
