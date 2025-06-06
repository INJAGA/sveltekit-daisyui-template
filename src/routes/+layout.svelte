<script lang="ts">
	import 'virtual:uno.css';
	import '../app.css';
	import TopBar from './TopBar.svelte';

	let { children } = $props();
	let opensDrawer = $state(false);
</script>

<div class="drawer">
	<input id="my-drawer" type="checkbox" class="drawer-toggle" bind:checked={opensDrawer} />
	<div class="drawer-content">
		<TopBar
			toggleDrawer={() => {
				opensDrawer = !opensDrawer;
			}}
		/>
		{@render children()}
	</div>
	<div class="drawer-side">
		<label for="my-drawer" aria-label="close sidebar" class="drawer-overlay"></label>
		<ul class="menu min-h-full w-60 bg-base-200 p-4 text-base-content md:w-80">
			<li><button>Item 1</button></li>
			<li><button>Item 2</button></li>
		</ul>
	</div>
</div>
