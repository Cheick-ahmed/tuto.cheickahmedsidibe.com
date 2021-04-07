<template>
	<app-modal name="SuccessModal"  maxWidth="md">
		<div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-6" >
			<div class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-green-100">
				<!-- Heroicon name: outline/check -->
				<svg class="h-6 w-6 text-green-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
				</svg>
			</div>
			<div class="mt-3 text-center sm:mt-5">
				<h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-headline">
					<slot name="title"></slot>
				</h3>
				<div class="mt-2">
					<p class="text-sm text-gray-500">
						<slot name="body"></slot>
					</p>
				</div>
			</div>

			<div class="mt-5 sm:mt-6">
				<slot name="action"></slot>
			</div>
		</div>
	</div>
</app-modal>
</template>

<script>

export default {
	props: {
		show: {
			default: false
		}
	}
}
</script>
