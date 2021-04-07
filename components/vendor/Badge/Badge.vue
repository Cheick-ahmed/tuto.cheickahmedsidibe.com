<template>
	<span class="inline-flex items-center rounded-md text-sm font-medium" :class="`bg-${color}-50 text-${color}-800 ${resolveSize}`">
		<slot></slot>
	</span>
</template>

<script>
export default {
	props : {
		color : {
			default : 'indigo',
			type : String
		},
		size : {
			type : String,
			default : 'large'
		}
	},
	computed : {
		resolveSize () {
			return this.size === 'large'
			? 'px-6 py-3'
			: 'px-4 py-2'
		}
	}
}
</script>

