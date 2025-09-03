<script>
	import '../app.css';
	import Header from '$lib/components/layout/Header.svelte';
	import Sidebar from '$lib/components/layout/Sidebar.svelte';
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	export let data;

	let sidebarCollapsed = false;
	let isMobile = false;
	let showMobileSidebar = false;

	onMount(() => {
		const checkMobile = () => {
			isMobile = window.innerWidth < 1024;
			if (!isMobile) showMobileSidebar = false;
		};
		
		checkMobile();
		window.addEventListener('resize', checkMobile);
		
		return () => window.removeEventListener('resize', checkMobile);
	});

	function toggleSidebar() {
		if (isMobile) {
			showMobileSidebar = !showMobileSidebar;
		} else {
			sidebarCollapsed = !sidebarCollapsed;
		}
	}

	function closeMobileSidebar() {
		if (isMobile) {
			showMobileSidebar = false;
		}
	}

	$: pageTitle = getPageTitle($page.url.pathname);

	function getPageTitle(pathname) {
		const routes = {
			'/': 'Dashboard',
			'/employees': 'Gestão de Funcionários',
			'/patients': 'Gestão de Pacientes',
			'/records': 'Gestão de Prontuários',
			'/agenda': 'Agenda de Consultas'
		};
		return routes[pathname] || 'Sistema Administrativo';
	}
</script>

<svelte:head>
	<title>{pageTitle} - PsiClinic Admin</title>
</svelte:head>

<div class="flex min-h-screen bg-gray-50">
	<!-- Mobile Sidebar Overlay -->
	{#if isMobile && showMobileSidebar}
		<div 
			class="fixed inset-0 bg-black bg-opacity-50 z-40 lg:hidden"
			on:click={closeMobileSidebar}
		></div>
	{/if}

	<!-- Sidebar -->
	<Sidebar 
		{sidebarCollapsed} 
		{showMobileSidebar}
		{isMobile}
		on:closeMobile={closeMobileSidebar}
	/>

	<!-- Main Content -->
	<div class="flex-1 flex flex-col {sidebarCollapsed && !isMobile ? 'ml-16' : 'ml-0 lg:ml-54'}">
		<!-- Header -->
		<Header 
			{pageTitle} 
			user={data.user} 
			on:toggleSidebar={toggleSidebar}
		/>

		<!-- Page Content -->
		<main class="flex-1 p-4 lg:p-6 overflow-auto">
			<slot />
		</main>
	</div>
</div>