<script>
	import { fly } from 'svelte/transition';
	import { page } from '$app/stores';
	import { showFilter, topActive, botActive } from '$lib/functions/utils';
	import Burger from '../Burger.svelte';
	import Bot from './Bot.svelte';
	import BotActive from './BotActive.svelte';
	import TopActive from './TopActive.svelte';
	import Top from './Top.svelte';
	import { onMount } from 'svelte';
	import MobileLinks from '../MobileLinks.svelte';
	let toggled = false;
	export let white = false;

	onMount(() => {
		$showFilter = false;
	});

	const toggleTop = () => {
		$botActive = false;
		$showFilter = false;
		$topActive = !$topActive;
	};
	const toggleBot = () => {
		$topActive = false;
		$botActive = !$botActive;
	};
	const toggleFilter = () => {
		$showFilter = !$showFilter;
	};
</script>

<!-- MOBILE -->
{#if $page.url.pathname != '/'}
	<div class={toggled ? 'flex flex-col fixed w-full z-50 bg-white' : ''}>
		<div
			class={$page.params.name
				? 'lg:hidden bg-transparent bg-white fixed w-full flex justify-between divide-x divide-black border-b border-black'
				: 'lg:hidden bg-white fixed z-50 w-full flex justify-between divide-x divide-black border-b border-black'}
		>
			<div
				in:fly={{ x: 20, duration: 200 }}
				class="flex font-medium py-2 w-full px-5"
			>
				<div class="w-full flex items-center">
					<a href="/" class="sm:ml-10 font-bt uppercase">gooseva komanda</a>
				</div>
			</div>
			<div on:click={() => (toggled = !toggled)} class="px-2">
				<Burger {white} {toggled} />
			</div>
		</div>

		<MobileLinks {toggled} />
	</div>
{:else}
	<div class={toggled ? 'flex flex-col fixed w-full z-50 bg-white' : ''}>
		<div
			class:white={white && !toggled}
			class="lg:hidden fixed w-full flex justify-between divide-x divide-black border-b border-black"
		>
			{#if toggled}
				<div
					in:fly={{ x: 20, duration: 200 }}
					class="flex font-medium py-2 w-full items-center"
				>
					<h1 class="ml-5 font-bt uppercase">gooseva komanda</h1>
				</div>
			{:else}
				<a
					href="/projects"
					target="_self"
					in:fly={{ x: -20, duration: 200 }}
					class="flex font-medium py-2 w-full items-center"
				>
					<span class="ml-5">Проекты</span>
				</a>
				<a
					target="_self"
					href="/team"
					in:fly={{ x: -20, duration: 200 }}
					class="flex font-medium py-2 w-full items-center"
				>
					<span class="ml-5">Команда</span>
				</a>
			{/if}
			<div on:click={() => (toggled = !toggled)} class="px-2">
				<Burger {white} {toggled} />
			</div>
		</div>

		<MobileLinks {toggled} />
	</div>
{/if}

<!-- DESKTOP -->
<div>
	<div class="fixed w-full z-10">
		{#if $topActive}
			<div transition:fly={{ y: -50, duration: 200 }}>
				<div class="hidden lg:inline">
					<div class="hidden w-full lg:flex justify-between">
						<div class="hidden xl:flex flex-none font-medium w-2/12 xl:w-1/12 items-start">
							<h1 class="pl-5 2xl:pl-10 w-full border-black border-b py-2 bg-white">Проекты</h1>
						</div>
						<div class="w-full lg:flex justify-between">
							<TopActive on:closeTop={toggleTop} />
						</div>
					</div>
				</div>
			</div>
		{:else}
			<div in:fly={{ delay: 200 }}>
				<div class="hidden lg:inline">
					<div class:white class="hidden w-full lg:flex justify-between border-b border-black">
						{#if $page.url.pathname == '/projects'}
							<div
								class={$showFilter
									? 'flex justify-between items-center cursor-pointer hover:shadow-border w-2/3 xl:w-3/4 border-black border-r bg-white'
									: 'flex justify-between items-center cursor-pointer hover:shadow-border w-2/3 xl:w-3/4 border-black border-r bg-white'}
								on:click={toggleFilter}
							>
								<h1 class="ml-5 2xl:ml-10 font-medium whitespace-nowrap">Навигация по проектам</h1>
								<img
									class={$showFilter
										? 'w-4 h-4 mt-1 mr-5 rotate-90 transition'
										: 'w-4 h-4 mt-1 mr-5 -rotate-90 transition'}
									src="/images/arrow.svg"
									alt=""
								/>
							</div>
						{:else if $page.params.name || $page.params.id}
							<a
								href="/projects"
								class='flex transition duration-200 hover:shadow-border font-medium py-2 items-center border-r border-black w-2/3 xl:w-3/4 bg-white'
							>
								<h1 class="ml-5 2xl:ml-10">Проекты</h1>
							</a>	
						{:else}
							<a
								href="/projects"
								class={white
									? 'flex transition duration-200 hover:shadow-white font-medium py-2 w-full items-center border-r border-white'
									: 'flex transition duration-200 hover:shadow-border font-medium py-2 w-full items-center border-r border-black'}
							>
								<h1 class="ml-5 2xl:ml-10">Проекты</h1>
							</a>
						{/if}
						<div
							class={$page.url.pathname == '/'
								? 'w-full lg:flex justify-between'
								: 'w-1/3 xl:w-1/4 lg:flex justify-between border-r border-black'}
							on:click={toggleTop}
						>
							<Top {white} />
						</div>
					</div>
				</div>
			</div>
		{/if}
	</div>

	<div class="fixed bottom-0 w-full z-50">
		{#if $botActive}
			<div transition:fly={{ y: 50, duration: 200 }}>
				<BotActive on:closeBot={toggleBot} />
			</div>
		{:else}
			<div class="hidden lg:inline w-full bottom-0" in:fly={{ delay: 200 }}>
				<div class="hidden group w-full lg:flex justify-between border-t border-black" class:white>
					{#if $page.url.pathname == '/'}
						<div
							class={white
								? 'flex transition duration-200 hover:shadow-white font-medium py-2 w-full items-center'
								: 'flex transition duration-200 hover:shadow-border font-medium py-2 w-full items-center'}
						>
							<a href="/team" class="ml-5 2xl:ml-10 w-full">Команда</a>
						</div>
					{:else}
						<div
							class="flex transition duration-200 hover:shadow-border font-medium py-2 w-2/3 xl:w-3/4 items-center border-r border-black bg-white"
						>
							<a href="/" class="ml-5 2xl:ml-10 font-bt uppercase w-full">gooseva komanda</a>
						</div>
					{/if}
					<div
						class={$page.url.pathname == '/'
							? 'w-full lg:flex justify-between'
							: 'w-1/3 xl:w-1/4 lg:flex justify-between'}
						on:click={toggleBot}
					>
						<Bot {white} />
					</div>
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.white {
		@apply text-white;
		@apply border-white;
		@apply divide-white;
	}
</style>
