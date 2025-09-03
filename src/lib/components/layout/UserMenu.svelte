<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	export let user = null;

	let showDropdown = false;
	let menuRef;

	onMount(() => {
		function handleClickOutside(event) {
			if (menuRef && !menuRef.contains(event.target)) {
				showDropdown = false;
			}
		}
		
		document.addEventListener('click', handleClickOutside);
		return () => document.removeEventListener('click', handleClickOutside);
	});

	function toggleDropdown() {
		showDropdown = !showDropdown;
	}

	function handleLogout() {
		// Implement logout logic
		console.log('Logout clicked');
		showDropdown = false;
		goto('/login');
	}

	function handleProfile() {
		showDropdown = false;
		goto('/profile');
	}
</script>

<div class="relative" bind:this={menuRef}>
	<!-- User Info Trigger -->
	<button 
		class="flex items-center space-x-3 p-2 rounded-lg hover:bg-gray-100 transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-blue-500"
		on:click={toggleDropdown}
	>
		<div class="text-right hidden sm:block">
			<p class="text-sm font-medium text-gray-900">{user?.name || 'Dr. João Silva'}</p>
			<p class="text-xs text-gray-500">{user?.role || 'Administrador'}</p>
		</div>
		
		<div class="relative">
			<img 
				src={user?.avatar || 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=40&h=40&fit=crop&crop=face&auto=format'} 
				alt="Avatar do usuário" 
				class="w-10 h-10 rounded-full border-2 border-gray-200 hover:border-blue-300 transition-all duration-200 hover:scale-105"
			>
			<div class="absolute -bottom-1 -right-1 w-4 h-4 bg-green-400 rounded-full border-2 border-white"></div>
		</div>
		
		<svg 
			class="w-4 h-4 text-gray-500 transition-transform duration-200 {showDropdown ? 'rotate-180' : ''}"
			fill="none" 
			stroke="currentColor" 
			viewBox="0 0 24 24"
		>
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
		</svg>
	</button>

	<!-- Dropdown Menu -->
	{#if showDropdown}
		<div class="
			absolute right-0 top-full mt-2 w-56 bg-white rounded-xl shadow-lg border border-gray-200 py-2 
			transform transition-all duration-200 ease-out
			{showDropdown ? 'opacity-100 scale-100' : 'opacity-0 scale-95'}
		">
			<!-- User Info in Dropdown -->
			<div class="px-4 py-3 border-b border-gray-100">
				<div class="flex items-center space-x-3">
					<img 
						src={user?.avatar || 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=32&h=32&fit=crop&crop=face&auto=format'} 
						alt="Avatar" 
						class="w-8 h-8 rounded-full"
					>
					<div>
						<p class="text-sm font-medium text-gray-900">{user?.name || 'Dr. João Silva'}</p>
						<p class="text-xs text-gray-500">{user?.email || 'joao.silva@clinic.com'}</p>
					</div>
				</div>
			</div>

			<!-- Menu Items -->
			<div class="py-2">
				<button
					class="w-full flex items-center px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 transition-colors duration-200"
					on:click={handleProfile}
				>
					<svg class="w-4 h-4 mr-3 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
					</svg>
					Meu Perfil
				</button>

				<button
					class="w-full flex items-center px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 transition-colors duration-200"
					on:click={() => showDropdown = false}
				>
					<svg class="w-4 h-4 mr-3 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
					</svg>
					Configurações
				</button>

				<button
					class="w-full flex items-center px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 transition-colors duration-200"
					on:click={() => showDropdown = false}
				>
					<svg class="w-4 h-4 mr-3 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
					</svg>
					Ajuda & Suporte
				</button>
			</div>

			<div class="border-t border-gray-100 pt-2">
				<button
					class="w-full flex items-center px-4 py-2 text-sm text-red-600 hover:bg-red-50 transition-colors duration-200"
					on:click={handleLogout}
				>
					<svg class="w-4 h-4 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
					</svg>
					Sair do Sistema
				</button>
			</div>
		</div>
	{/if}
</div>