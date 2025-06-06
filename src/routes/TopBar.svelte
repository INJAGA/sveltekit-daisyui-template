<script lang="ts">
	import ThemeButtons from './ThemeButtons.svelte';

	const { toggleDrawer }: { toggleDrawer: () => void } = $props();
	const title = 'Svelte & daisyUI Template';
</script>

<div
	class="sticky top-0 flex items-center bg-primary px-1 py-0.5 text-primary-content shadow-xl transition duration-700"
>
	<div class="flex flex-1 items-center gap-1 overflow-hidden">
		<button class="btn btn-square btn-ghost" aria-label="Open Side Bar" onclick={toggleDrawer}>
			<span class="i-material-symbols:menu-rounded text-2xl"></span>
		</button>
		<div class="overflow-hidden text-xl font-bold text-nowrap text-ellipsis">
			{title}
		</div>
	</div>
	<ThemeButtons />
</div>
