<template>
	<textarea
		:value="value"
		class="box-border shadow-sm focus:ring-blue-600 focus:border-blue-600 block w-full sm:text-sm border-gray-300 rounded-md py-3" 
		@input="$emit('input', $event.target.value); resize($event)"
		style="min-height : 150px"
	/>
</template>

<script>
export default {
	props : {
		value : {
			required : false
		}
	},
	methods : {
		resize (e) {
			e.target.style.height = 'auto'
			e.target.style.height = `${e.target.scrollHeight}px`
		}
	}
}
</script>
