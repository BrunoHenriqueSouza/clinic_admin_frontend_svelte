<script>
	import { page } from '$app/stores';
	import { createEventDispatcher } from 'svelte';

	export let sidebarCollapsed = false;
	export let showMobileSidebar = false;
	export let isMobile = false;

	const dispatch = createEventDispatcher();

	const menuItems = [
		{
			name: 'Dashboard',
			href: '/',
			icon: 'M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2H5a2 2 0 00-2-2z'
		},
		{
			name: 'Funcionários',
			href: '/employees',
			icon: 'M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.25 2.25 0 11-4.5 0 2.25 2.25 0 014.5 0z'
		},
		{
			name: 'Pacientes',
			href: '/patients',
			icon: 'M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.501 20.118a7.5 7.5 0 0114.998 0A17.933 17.933 0 0112 21.75c-2.676 0-5.216-.584-7.499-1.632z'
		},
		{
			name: 'Prontuários',
			href: '/records',
			icon: 'M19.5 14.25v-2.625a3.375 3.375 0 00-3.375-3.375h-1.5A1.125 1.125 0 0113.5 7.125v-1.5a3.375 3.375 0 00-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 00-9-9z'
		},
		{
			name: 'Agenda',
			href: '/agenda',
			icon: 'M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 012.25-2.25h13.5A2.25 2.25 0 0121 7.5v11.25m-18 0A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75m-18 0v-7.5A2.25 2.25 0 015.25 9h13.5A2.25 2.25 0 0121 11.25v7.5'
		}
	];

	function handleNavClick() {
		if (isMobile) {
			dispatch('closeMobile');
		}
	}

	$: isActive = (href) => {
		if (href === '/') {
			return $page.url.pathname === '/';
		}
		return $page.url.pathname.startsWith(href);
	};
</script>

<aside class="
	fixed inset-y-0 left-0 z-50 
	{isMobile 
		? showMobileSidebar 
			? 'translate-x-0' 
			: '-translate-x-full'
		: 'translate-x-0'
	} 
	{sidebarCollapsed && !isMobile ? 'w-16' : 'w-64'}
	bg-white shadow-xl border-r border-gray-200 transition-all duration-300 ease-in-out
	lg:static lg:inset-auto lg:translate-x-0
">
	<!-- Logo -->
	<div class="p-4 border-b border-gray-200">
		<div class="flex items-center space-x-3">
			<div class="w-10 h-10 bg-gradient-to-br from-blue-600 to-indigo-600 rounded-xl flex items-center justify-center shadow-lg">
				<svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" />
				</svg>
			</div>
			{#if !sidebarCollapsed || isMobile}
				<div class="transition-opacity duration-300">
					<h1 class="font-bold text-lg text-gray-900">PsiClinic</h1>
					<p class="text-xs text-gray-500">Admin System</p>
				</div>
			{/if}
		</div>
	</div>

	<!-- Navigation -->
	<nav class="mt-6 px-3">
		<ul class="space-y-2">
			{#each menuItems as item}
				<li>
					<a
						href={item.href}
						class="
							flex items-center px-3 py-3 rounded-lg transition-all duration-200
							{isActive(item.href) 
								? 'bg-blue-100 text-blue-700 shadow-sm' 
								: 'text-gray-700 hover:bg-gray-100 hover:text-gray-900'
							}
						"
						on:click={handleNavClick}
					>
						<svg class="w-5 h-5 {sidebarCollapsed && !isMobile ? '' : 'mr-3'}" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={item.icon} />
						</svg>
						{#if !sidebarCollapsed || isMobile}
							<span class="font-medium transition-opacity duration-300">{item.name}</span>
						{/if}
					</a>
				</li>
			{/each}
		</ul>
	</nav>

	<!-- Footer -->
	{#if !sidebarCollapsed || isMobile}
		<div class="absolute bottom-0 left-0 right-0 p-4 border-t border-gray-200">
			<div class="text-xs text-gray-500 text-center">
				<p>© 2025 PsiClinic</p>
				<p>v1.0.0</p>
			</div>
		</div>
	{/if}
</aside>