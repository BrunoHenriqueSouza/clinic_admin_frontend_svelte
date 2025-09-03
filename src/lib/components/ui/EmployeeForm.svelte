<script>
	import { createEventDispatcher } from 'svelte';
	import Button from '../ui/Button.svelte';

	export let employee = null;

	const dispatch = createEventDispatcher();

	let form = {
		name: employee?.name || '',
		email: employee?.email || '',
		role: employee?.role || '',
		specialization: employee?.specialization || '',
		phone: employee?.phone || '',
		crp: employee?.crp || '',
		department: employee?.department || '',
		salary: employee?.salary || 0,
		status: employee?.status || 'active'
	};

	let errors = {};
	let loading = false;

	function validateForm() {
		errors = {};
		
		if (!form.name.trim()) errors.name = 'Nome é obrigatório';
		if (!form.email.trim()) errors.email = 'Email é obrigatório';
		if (!form.role.trim()) errors.role = 'Cargo é obrigatório';
		if (!form.phone.trim()) errors.phone = 'Telefone é obrigatório';
		if (!form.crp.trim()) errors.crp = 'CRP é obrigatório';
		if (!form.department.trim()) errors.department = 'Departamento é obrigatório';
		if (form.salary <= 0) errors.salary = 'Salário deve ser maior que zero';

		return Object.keys(errors).length === 0;
	}

	async function handleSubmit() {
		if (!validateForm()) return;

		loading = true;

		try {
			// Simulação de API call
			await new Promise(resolve => setTimeout(resolve, 1000));
			
			dispatch('save', {
				...form,
				id: employee?.id || Date.now()
			});
		} catch (error) {
			console.error('Erro ao salvar funcionário:', error);
		} finally {
			loading = false;
		}
	}

	function handleCancel() {
		dispatch('cancel');
	}

	function formatCurrency(value) {
		return value.toLocaleString('pt-BR', {
			style: 'currency',
			currency: 'BRL'
		});
	}
</script>

<form on:submit|preventDefault={handleSubmit} class="space-y-4">
	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div>
			<label for="name" class="block text-sm font-medium text-gray-700 mb-1">Nome Completo</label>
			<input
				type="text"
				id="name"
				bind:value={form.name}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.name}
				placeholder="Digite o nome completo"
			>
			{#if errors.name}
				<p class="mt-1 text-sm text-red-600">{errors.name}</p>
			{/if}
		</div>

		<div>
			<label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
			<input
				type="email"
				id="email"
				bind:value={form.email}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.email}
				placeholder="email@exemplo.com"
			>
			{#if errors.email}
				<p class="mt-1 text-sm text-red-600">{errors.email}</p>
			{/if}
		</div>
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div>
			<label for="role" class="block text-sm font-medium text-gray-700 mb-1">Cargo</label>
			<input
				type="text"
				id="role"
				bind:value={form.role}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.role}
				placeholder="Ex: Psicólogo Clínico"
			>
			{#if errors.role}
				<p class="mt-1 text-sm text-red-600">{errors.role}</p>
			{/if}
		</div>

		<div>
			<label for="department" class="block text-sm font-medium text-gray-700 mb-1">Departamento</label>
			<select
				id="department"
				bind:value={form.department}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.department}
			>
				<option value="">Selecione um departamento</option>
				<option value="Psicologia">Psicologia</option>
				<option value="Administração">Administração</option>
				<option value="Recepção">Recepção</option>
				<option value="Limpeza">Limpeza</option>
			</select>
			{#if errors.department}
				<p class="mt-1 text-sm text-red-600">{errors.department}</p>
			{/if}
		</div>
	</div>

	<div>
		<label for="specialization" class="block text-sm font-medium text-gray-700 mb-1">Especialização</label>
		<input
			type="text"
			id="specialization"
			bind:value={form.specialization}
			class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
			placeholder="Ex: Terapia Cognitivo-Comportamental"
		>
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div>
			<label for="phone" class="block text-sm font-medium text-gray-700 mb-1">Telefone</label>
			<input
				type="tel"
				id="phone"
				bind:value={form.phone}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.phone}
				placeholder="(11) 99999-9999"
			>
			{#if errors.phone}
				<p class="mt-1 text-sm text-red-600">{errors.phone}</p>
			{/if}
		</div>

		<div>
			<label for="crp" class="block text-sm font-medium text-gray-700 mb-1">CRP</label>
			<input
				type="text"
				id="crp"
				bind:value={form.crp}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.crp}
				placeholder="CRP 06/123456"
			>
			{#if errors.crp}
				<p class="mt-1 text-sm text-red-600">{errors.crp}</p>
			{/if}
		</div>
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div>
			<label for="salary" class="block text-sm font-medium text-gray-700 mb-1">Salário</label>
			<input
				type="number"
				id="salary"
				bind:value={form.salary}
				step="0.01"
				min="0"
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				class:border-red-300={errors.salary}
				placeholder="0,00"
			>
			{#if errors.salary}
				<p class="mt-1 text-sm text-red-600">{errors.salary}</p>
			{/if}
		</div>

		<div>
			<label for="status" class="block text-sm font-medium text-gray-700 mb-1">Status</label>
			<select
				id="status"
				bind:value={form.status}
				class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
			>
				<option value="active">Ativo</option>
				<option value="inactive">Inativo</option>
			</select>
		</div>
	</div>

	<div class="flex justify-end space-x-3 pt-4">
		<Button variant="outline" on:click={handleCancel} disabled={loading}>
			Cancelar
		</Button>
		<Button type="submit" disabled={loading}>
			{#if loading}
				<svg class="animate-spin -ml-1 mr-2 h-4 w-4 text-white" fill="none" viewBox="0 0 24 24">
					<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
					<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
				</svg>
				Salvando...
			{:else}
				{employee ? 'Atualizar' : 'Salvar'}
			{/if}
		</Button>
	</div>
</form>