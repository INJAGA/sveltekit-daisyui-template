<script lang="ts">
	import { themes } from '$lib/themes';
	import { onMount } from 'svelte';

	let currentTheme = $state('system');
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
