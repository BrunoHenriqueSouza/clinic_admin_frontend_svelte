<script>
	import { createEventDispatcher } from 'svelte';
	
	export let variant = 'primary';
	export let size = 'md';
	export let disabled = false;
	export let type = 'button';
	let className = '';
	export { className as class };

	const dispatch = createEventDispatcher();

	const variants = {
		primary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
		secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300 focus:ring-gray-500',
		success: 'bg-green-600 text-white hover:bg-green-700 focus:ring-green-500',
		danger: 'bg-red-600 text-white hover:bg-red-700 focus:ring-red-500',
		ghost: 'text-gray-700 hover:bg-gray-100 focus:ring-gray-500',
		outline: 'border border-gray-300 text-gray-700 hover:bg-gray-50 focus:ring-gray-500'
	};

	const sizes = {
		sm: 'px-3 py-2 text-sm',
		md: 'px-4 py-2 text-sm',
		lg: 'px-6 py-3 text-base'
	};

	function handleClick(event) {
		if (!disabled) {
			dispatch('click', event);
		}
	}
</script>

<button
	{type}
	{disabled}
	class="inline-flex items-center justify-center font-medium rounded-lg transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed {variants[variant]} {sizes[size]} {className}"
	on:click={handleClick}
>
	<slot />
</button>