<script>
	import { themes } from '$lib/themes';
	import { onMount } from 'svelte';
	import 'virtual:uno.css';
	import '../app.css';

	let { children } = $props();
	const title = 'Svelte 5 & daisyUI 5 Template';
	let currentTheme = $state(themes[0]);
	let scrollY = $state(0);

	onMount(() => {
		currentTheme = localStorage.getItem('theme') ?? themes[0];
	});

	$effect(() => {
		document.documentElement.setAttribute('data-theme', currentTheme);
		localStorage.setItem('theme', currentTheme);
	});
</script>

<svelte:window bind:scrollY />

<div
	class="bg-primary text-primary-content sticky top-0 flex items-center p-1 text-4xl transition duration-700"
	class:shadow-2xl={scrollY !== 0}
>
	<div class="flex flex-1 items-center">
		<div class="i-ri:svelte-fill"></div>
		<div class="hidden md:block">
			{title}
		</div>
	</div>
	<div class="join flex flex-none items-center">
		<button
			title="To Light Theme"
			aria-label="To Light Theme"
			class="btn btn-ghost btn-square join-item"
			onclick={() => (currentTheme = 'light')}
		>
			<span class="i-line-md:sunny-filled-loop"></span>
		</button>
		<button
			title="To Dark Theme"
			aria-label="To Dark Theme"
			class="btn btn-ghost btn-square join-item"
			onclick={() => (currentTheme = 'dark')}
		>
			<span class="i-line-md:moon-filled-loop"></span>
		</button>
		<div title="Change Theme" class="dropdown dropdown-end">
			<div tabindex="0" role="button" class="btn btn-ghost join-item block">
				<div class="flex h-full items-center gap-0.5">
					<div class="i-ic:baseline-color-lens"></div>
					<div class="i-material-symbols:keyboard-arrow-down"></div>
				</div>
			</div>
			<ul
				class="text-base-content bg-base-300 dropdown-content menu rounded-box z-1 h-[30rem] max-h-[calc(100vh-10rem)] flex-nowrap overflow-y-auto p-2 shadow-2xl"
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
							<div class="flex flex-none items-center gap-1 border p-1.5" data-theme={theme}>
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
