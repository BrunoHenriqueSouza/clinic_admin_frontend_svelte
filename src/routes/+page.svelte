<script>
	import Card from '$lib/components/ui/Card.svelte';
	import Button from '$lib/components/ui/Button.svelte';
	import { goto } from '$app/navigation';

	export let data;

	function formatCurrency(value) {
		return new Intl.NumberFormat('pt-BR', {
			style: 'currency',
			currency: 'BRL'
		}).format(value);
	}

	function formatTime(timestamp) {
		const date = new Date(timestamp);
		const now = new Date();
		const diff = now - date;
		
		const minutes = Math.floor(diff / (1000 * 60));
		const hours = Math.floor(diff / (1000 * 60 * 60));
		const days = Math.floor(diff / (1000 * 60 * 60 * 24));
		
		if (minutes < 60) return `${minutes} min atrás`;
		if (hours < 24) return `${hours}h atrás`;
		return `${days}d atrás`;
	}

	function getActivityIcon(type) {
		const icons = {
			appointment: 'M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z',
			record: 'M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z',
			patient: 'M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z'
		};
		return icons[type] || icons.appointment;
	}

	function getStatusColor(status) {
		const colors = {
			confirmed: 'bg-green-100 text-green-800',
			pending: 'bg-yellow-100 text-yellow-800',
			cancelled: 'bg-red-100 text-red-800'
		};
		return colors[status] || colors.pending;
	}
</script>

<svelte:head>
	<title>Dashboard - PsiClinic Admin</title>
</svelte:head>

<div class="space-y-6">
	<!-- Welcome Header -->
	<div class="bg-gradient-to-r from-blue-600 to-indigo-700 rounded-lg p-6 text-white">
		<div class="flex items-center justify-between">
			<div>
				<h1 class="text-2xl font-bold mb-2">Bem-vindo de volta! 👋</h1>
				<p class="text-blue-100">Aqui está um resumo das atividades da sua clínica hoje.</p>
			</div>
			<div class="hidden lg:block">
				<svg class="w-16 h-16 text-blue-200" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
				</svg>
			</div>
		</div>
	</div>

	<!-- Stats Grid -->
	<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
		<!-- Funcionários -->
		<Card class="p-6 hover:shadow-md transition-shadow cursor-pointer" on:click={() => goto('/employees')}>
			<div class="flex items-center justify-between">
				<div>
					<p class="text-sm font-medium text-gray-600">Funcionários</p>
					<p class="text-2xl font-bold text-gray-900">{data.stats.employees.total}</p>
					<p class="text-sm text-green-600">
						<span class="font-medium">{data.stats.employees.active}</span> ativos
					</p>
				</div>
				<div class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center">
					<svg class="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.25 2.25 0 11-4.5 0 2.25 2.25 0 014.5 0z" />
					</svg>
				</div>
			</div>
		</Card>

		<!-- Pacientes -->
		<Card class="p-6 hover:shadow-md transition-shadow cursor-pointer" on:click={() => goto('/patients')}>
			<div class="flex items-center justify-between">
				<div>
					<p class="text-sm font-medium text-gray-600">Pacientes</p>
					<p class="text-2xl font-bold text-gray-900">{data.stats.patients.total}</p>
					<p class="text-sm text-green-600">
						<span class="font-medium">+{data.stats.patients.newThisMonth}</span> este mês
					</p>
				</div>
				<div class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center">
					<svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
					</svg>
				</div>
			</div>
		</Card>

		<!-- Consultas Hoje -->
		<Card class="p-6 hover:shadow-md transition-shadow cursor-pointer" on:click={() => goto('/agenda')}>
			<div class="flex items-center justify-between">
				<div>
					<p class="text-sm font-medium text-gray-600">Consultas Hoje</p>
					<p class="text-2xl font-bold text-gray-900">{data.stats.appointments.today}</p>
					<p class="text-sm text-blue-600">
						<span class="font-medium">{data.stats.appointments.thisWeek}</span> esta semana
					</p>
				</div>
				<div class="w-12 h-12 bg-yellow-100 rounded-lg flex items-center justify-center">
					<svg class="w-6 h-6 text-yellow-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
					</svg>
				</div>
			</div>
		</Card>

		<!-- Receita -->
		<Card class="p-6 hover:shadow-md transition-shadow">
			<div class="flex items-center justify-between">
				<div>
					<p class="text-sm font-medium text-gray-600">Receita Mensal</p>
					<p class="text-2xl font-bold text-gray-900">{formatCurrency(data.stats.revenue.thisMonth)}</p>
					<p class="text-sm text-green-600">
						<span class="font-medium">+{data.stats.revenue.growth}%</span> vs mês anterior
					</p>
				</div>
				<div class="w-12 h-12 bg-purple-100 rounded-lg flex items-center justify-center">
					<svg class="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1" />
					</svg>
				</div>
			</div>
		</Card>
	</div>

	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<!-- Próximos Agendamentos -->
		<Card>
			<div class="p-6 border-b border-gray-200">
				<div class="flex items-center justify-between">
					<h2 class="text-lg font-semibold text-gray-900">Próximos Agendamentos</h2>
					<Button variant="ghost" size="sm" on:click={() => goto('/agenda')}>
						Ver todos
					</Button>
				</div>
			</div>
			<div class="divide-y divide-gray-200">
				{#each data.upcomingAppointments as appointment}
					<div class="p-4 hover:bg-gray-50 transition-colors">
						<div class="flex items-center justify-between">
							<div class="flex-1">
								<div class="flex items-center space-x-3">
									<div class="text-sm font-medium text-gray-900">{appointment.time}</div>
									<div class="text-sm text-gray-600">{appointment.patientName}</div>
								</div>
								<div class="mt-1 flex items-center space-x-2">
									<span class="text-xs text-gray-500">{appointment.therapistName}</span>
									<span class="text-xs text-gray-400">•</span>
									<span class="text-xs text-gray-500">{appointment.type}</span>
								</div>
							</div>
							<span class="px-2 py-1 text-xs font-medium rounded-full {getStatusColor(appointment.status)}">
								{appointment.status === 'confirmed' ? 'Confirmado' : appointment.status === 'pending' ? 'Pendente' : 'Cancelado'}
							</span>
						</div>
					</div>
				{/each}
			</div>
		</Card>

		<!-- Atividade Recente -->
		<Card>
			<div class="p-6 border-b border-gray-200">
				<h2 class="text-lg font-semibold text-gray-900">Atividade Recente</h2>
			</div>
			<div class="divide-y divide-gray-200">
				{#each data.recentActivity as activity}
					<div class="p-4 hover:bg-gray-50 transition-colors">
						<div class="flex items-start space-x-3">
							<div class="w-8 h-8 bg-gray-100 rounded-full flex items-center justify-center flex-shrink-0">
								<svg class="w-4 h-4 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={getActivityIcon(activity.type)} />
								</svg>
							</div>
							<div class="flex-1 min-w-0">
								<p class="text-sm text-gray-900">{activity.message}</p>
								<div class="mt-1 flex items-center space-x-2 text-xs text-gray-500">
									<span>{activity.user}</span>
									<span>•</span>
									<span>{formatTime(activity.timestamp)}</span>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</Card>
	</div>

	<!-- Quick Actions -->
	<Card class="p-6">
		<h2 class="text-lg font-semibold text-gray-900 mb-4">Ações Rápidas</h2>
		<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
			<Button variant="outline" class="flex items-center justify-center space-x-2 p-4" on:click={() => goto('/employees')}>
				<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z" />
				</svg>
				<span>Novo Funcionário</span>
			</Button>
			
			<Button variant="outline" class="flex items-center justify-center space-x-2 p-4" on:click={() => goto('/patients')}>
				<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
				</svg>
				<span>Novo Paciente</span>
			</Button>
			
			<Button variant="outline" class="flex items-center justify-center space-x-2 p-4" on:click={() => goto('/agenda')}>
				<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
				</svg>
				<span>Agendar Consulta</span>
			</Button>
			
			<Button variant="outline" class="flex items-center justify-center space-x-2 p-4" on:click={() => goto('/records')}>
				<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
				</svg>
				<span>Novo Prontuário</span>
			</Button>
		</div>
	</Card>
</div>