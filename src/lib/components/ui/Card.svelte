<script>
	let className = '';
	export { className as class };
	export let padding = true;
</script>

<div class="bg-white rounded-lg shadow-sm border border-gray-200 {className}" class:p-6={padding}>
	<slot />
</div>