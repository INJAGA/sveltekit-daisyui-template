<script lang="ts">
	import { themes } from '$lib/themes';
	import { onMount } from 'svelte';
	import 'virtual:uno.css';
	import '../app.css';

	let { children } = $props();
	const title = 'Svelte 5 & daisyUI 5 Template';
	let currentTheme = $state('system');
	let opensDrawer = $state(false);
	let systemDarkMode = $state(false);

	onMount(() => {
		currentTheme = localStorage.getItem('theme') ?? 'system';

		const darkModeMediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
		systemDarkMode = darkModeMediaQuery.matches;
		const listener = (event: MediaQueryListEvent) => (systemDarkMode = event.matches);
		darkModeMediaQuery.addEventListener('change', listener);
		return () => darkModeMediaQuery.removeEventListener('change', listener);
	});

	$effect(() => {
		document.documentElement.setAttribute('data-theme', currentTheme);
		localStorage.setItem('theme', currentTheme);
	});
</script>

<div class="drawer">
	<input id="my-drawer" type="checkbox" class="drawer-toggle" bind:checked={opensDrawer} />
	<div class="drawer-content">
		<div
			class="sticky top-0 flex items-center bg-primary px-1 py-0.5 text-primary-content shadow-xl transition duration-700"
		>
			<div class="flex flex-1 items-center gap-1 overflow-hidden">
				<button
					class="btn btn-square btn-ghost"
					aria-label="Open Side Bar"
					onclick={() => {
						opensDrawer = true;
					}}
				>
					<span class="i-material-symbols:menu-rounded text-2xl"></span>
				</button>
				<div class="overflow-hidden text-xl font-bold text-nowrap text-ellipsis">
					{title}
				</div>
			</div>
			<div class="join flex">
				<!-- svelte-ignore a11y_consider_explicit_label -->
				<button
					class="btn join-item btn-square text-2xl btn-ghost"
					onclick={() => {
						currentTheme = { system: 'light', light: 'dark' }[currentTheme] ?? 'system';
					}}
				>
					<span
						class={{
							light: 'i-line-md:sunny-filled-loop',
							dark: 'i-line-md:moon-filled-loop',
							system: 'i-material-symbols:brightness-auto'
						}[currentTheme] ?? 'i-ic:baseline-color-lens'}
					></span>
				</button>
				<div title="Change Theme" class="dropdown dropdown-end">
					<div tabindex="0" role="button" class="btn join-item btn-square text-2xl btn-ghost">
						<span class="i-material-symbols:keyboard-arrow-down"></span>
					</div>
					<!-- svelte-ignore a11y_no_noninteractive_tabindex -->
					<ul
						tabindex="0"
						class="dropdown-content menu z-1 h-[30rem] max-h-[calc(100vh-10rem)] flex-nowrap overflow-x-clip overflow-y-auto rounded-box bg-base-300 p-2 text-base-content shadow-2xl"
					>
						{#each themes as theme}
							<li>
								<button
									class="flex items-center"
									class:menu-active={theme === currentTheme}
									onclick={() => (currentTheme = theme)}
								>
									<div class="flex-1 capitalize">
										{theme}
									</div>
									<div
										class="flex flex-none items-center gap-1 border p-1.5"
										data-theme={theme === 'system' ? (systemDarkMode ? 'dark' : 'light') : theme}
									>
										<div class="badge badge-primary">A</div>
										<div class="badge badge-secondary">A</div>
										<div class="badge badge-neutral">A</div>
										<div class="badge badge-accent">A</div>
									</div>
								</button>
							</li>
						{/each}
					</ul>
				</div>
			</div>
		</div>
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
