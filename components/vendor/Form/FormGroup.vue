<template>
	<div>
		<label :for="LabelFor" class="block text-sm font-medium text-gray-900" :class="labelClasses">
			{{ label }}
		</label>
		<div class="mt-1">
			<slot></slot>
		</div>
		<p class="mt-1.5 text-base text-red-500 space-x-2 flex items-center" id="email-description" v-if="error">
			<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path>
			</svg>
			<span>
				{{ error }}
			</span>
		</p>
	</div>
</template>

<script>
export default {
	props : {
		label : {
			type : String,
			required : true
		},
		LabelFor : {
			type : String,
			required : true
		},
		labelClasses : {
			required : false
		},
		error : {
			dafault : false
		}
	}
}
</script>