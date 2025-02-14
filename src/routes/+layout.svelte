<script>
	import { themes } from '$lib/themes';
	import { onMount } from 'svelte';
	import 'virtual:uno.css';
	import '../app.css';

	let { children } = $props();
	const title = 'Svelte 5 & daisyUI 5 Template';
	let currentTheme = $state(themes[0]);
	let scrollY = $state(0);
	let opensDrawer = $state(false);

	onMount(() => {
		currentTheme = localStorage.getItem('theme') ?? themes[0];
	});

	$effect(() => {
		document.documentElement.setAttribute('data-theme', currentTheme);
		localStorage.setItem('theme', currentTheme);
	});
</script>

<svelte:window bind:scrollY />

<div class="drawer">
	<input id="my-drawer" type="checkbox" class="drawer-toggle" bind:checked={opensDrawer} />
	<div class="drawer-content">
		<div
			class="sticky top-0 flex items-center bg-primary px-1 py-0.5 text-4xl text-primary-content transition duration-700"
			class:shadow-2xl={scrollY !== 0}
		>
			<div class="flex flex-1 items-center">
				<button
					class="btn btn-square btn-ghost"
					aria-label="Open Side Bar"
					onclick={() => {
						opensDrawer = true;
					}}
				>
					<span class="i-ri:svelte-fill text-2xl"></span>
				</button>
				<!-- TODO: 横幅狭いときに...で表示させたい -->
				<div class="hidden md:block">
					{title}
				</div>
			</div>
			<div class="join flex flex-none items-center">
				<button
					title={currentTheme === 'light' ? 'To Dark Theme' : 'To Light Theme'}
					aria-label={currentTheme === 'light' ? 'To Dark Theme' : 'To Light Theme'}
					class="btn join-item btn-square btn-ghost"
					onclick={() => (currentTheme = currentTheme === 'light' ? 'dark' : 'light')}
				>
					<span
						class={'text-2xl ' +
							(currentTheme === 'light'
								? 'i-line-md:sunny-filled-loop'
								: currentTheme === 'dark'
									? 'i-line-md:moon-filled-loop'
									: 'i-ic:baseline-color-lens')}
					></span>
				</button>
				<div title="Change Theme" class="dropdown dropdown-end">
					<div tabindex="0" role="button" class="btn join-item flex btn-square btn-ghost">
						<div class="i-material-symbols:keyboard-arrow-down text-2xl"></div>
					</div>
					<!-- svelte-ignore a11y_no_noninteractive_tabindex -->
					<ul
						tabindex="0"
						class="dropdown-content menu z-1 h-[30rem] max-h-[calc(100vh-10rem)] flex-nowrap overflow-y-auto rounded-box bg-base-300 p-2 text-base-content shadow-2xl"
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
	</div>
	<div class="drawer-side">
		<label for="my-drawer" aria-label="close sidebar" class="drawer-overlay"></label>
		<ul class="menu min-h-full w-60 bg-base-200 p-4 text-base-content md:w-80">
			<li><button>Item 1</button></li>
			<li><button>Item 2</button></li>
		</ul>
	</div>
</div>
