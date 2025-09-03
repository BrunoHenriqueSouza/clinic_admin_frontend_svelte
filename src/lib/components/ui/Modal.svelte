<script>
	import { createEventDispatcher, onMount } from 'svelte';
	
	export let show = false;
	export let title = '';
	export let maxWidth = 'max-w-lg';
	
	const dispatch = createEventDispatcher();
	
	let dialog;
	
	onMount(() => {
		const handleKeydown = (e) => {
			if (e.key === 'Escape' && show) {
				close();
			}
		};
		
		document.addEventListener('keydown', handleKeydown);
		return () => document.removeEventListener('keydown', handleKeydown);
	});
	
	function close() {
		show = false;
		dispatch('close');
	}
	
	function handleBackdropClick(e) {
		if (e.target === e.currentTarget) {
			close();
		}
	}
</script>

{#if show}
	<div 
		class="fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50"
		on:click={handleBackdropClick}
		bind:this={dialog}
	>
		<div 
			class="bg-white rounded-lg shadow-xl w-full {maxWidth} transform transition-all duration-200 ease-out"
			class:scale-100={show}
			class:scale-95={!show}
		>
			{#if title}
				<div class="px-6 py-4 border-b border-gray-200">
					<div class="flex items-center justify-between">
						<h3 class="text-lg font-semibold text-gray-900">{title}</h3>
						<button
							class="text-gray-400 hover:text-gray-600 transition-colors"
							on:click={close}
						>
							<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
							</svg>
						</button>
					</div>
				</div>
			{/if}
			
			<div class="px-6 py-4">
				<slot />
			</div>
		</div>
	</div>
{/if}