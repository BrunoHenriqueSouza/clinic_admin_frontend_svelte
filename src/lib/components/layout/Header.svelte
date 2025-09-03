<script>
	import { createEventDispatcher } from 'svelte';
	import UserMenu from './UserMenu.svelte';

	export let pageTitle = '';
	export let user = null;

	const dispatch = createEventDispatcher();

	function toggleSidebar() {
		dispatch('toggleSidebar');
	}
</script>

<header class="bg-white/95 backdrop-blur-sm border-b border-gray-200 px-4 lg:px-6 py-4 flex items-center justify-between sticky top-0 z-30">
	<div class="flex items-center space-x-4">
		<button 
			class="p-2 rounded-lg hover:bg-gray-100 transition-colors duration-200 lg:hidden"
			on:click={toggleSidebar}
		>
			<svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
			</svg>
		</button>
		
		<h1 class="text-xl lg:text-2xl font-semibold text-gray-900">{pageTitle}</h1>
	</div>

	<div class="flex items-center space-x-4">
		<!-- Search -->
		<div class="hidden md:flex items-center bg-gray-100 rounded-lg px-3 py-2 max-w-sm">
			<svg class="w-4 h-4 text-gray-500 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
			</svg>
			<input 
				type="text" 
				placeholder="Buscar..." 
				class="bg-transparent border-none outline-none text-sm w-full text-gray-900 placeholder-gray-500"
			>
		</div>

		<!-- Notifications -->
		<button class="relative p-2 rounded-lg hover:bg-gray-100 transition-colors duration-200">
			<svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
			</svg>
			<span class="absolute -top-1 -right-1 w-3 h-3 bg-red-500 rounded-full"></span>
		</button>

		<!-- User Menu -->
		<UserMenu {user} />
	</div>
</header>