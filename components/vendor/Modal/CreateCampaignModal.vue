<template>
	<app-modal :name="name" maxWidth="lg">
		<div class="py-6 px-4 bg-gray-900 sm:px-6">
			<div class="flex items-center justify-between">
				<h2 id="slide-over-heading" class="text-lg font-medium text-white">
					Nouvelle campagne
				</h2>
				<div class="ml-3 h-7 flex items-center">
					<button @click.prevent="close" class="bg-gray-900 rounded-md text-gray-200 hover:text-white focus:outline-none focus:ring-2 focus:ring-white">
						<span class="sr-only">Close panel</span>
						<!-- Heroicon name: outline/x -->
						<svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
						</svg>
					</button>
				</div>
			</div>
		</div>
		<div class="py-6 px-4 bg-white sm:px-6">
			<form @submit.prevent="submit">
				<div class="space-y-6">
					<form-group label="Titre de la campagne" LabelFor="title" :error=" validation.title ? validation.title[0] : '' ">
						<form-input type="text" v-model="form.title" id="title" :class=" { 'focus:border-red-500 border-red-500' : validation.title} " />
					</form-group>

			<!-- 		<form-group label="Description" LabelFor="description">
						<FormTextArea type="text" v-model="form.description" id="description" :class=" { 'focus:border-red-500 border-red-500' : validation.description} " />
						<p class="mt-2 text-sm text-gray-500">Décrivez en quelques mots votre campagne (facultatif)</p>
					</form-group> -->

	<!-- 				<div>
						<label for="template" class="block text-sm font-medium text-gray-900">Audience</label>
						<select id="template" name="template" class="mt-1 block w-full pl-3 pr-10 py-3 text-base border-gray-300 focus:outline-none focus:ring-blue-600 focus:border-blue-600 sm:text-sm rounded-md">
							<option selected>Default</option>
						</select>
					</div> -->

					<div>
						<label for="template" class="block text-sm font-medium text-gray-900">Template</label>
						<select id="template" name="template" class="mt-1 block w-full pl-3 pr-10 py-3 text-base border-gray-300 focus:outline-none focus:ring-blue-600 focus:border-blue-600 sm:text-sm rounded-md">
							<option selected>Default</option>
						</select>
					</div>

				</div>
				<div class="mt-5 sm:mt-8 flex items-center flex-row-reverse">
					<PrimaryButton class="ml-2 space-x-2 group disabled:opacity-50" :disabled="processing">
						<span>Créer</span>
						<svg v-show="processing" class="animate-spin h-5 w-5 text-white group-hover:text-black" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
							<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
							<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
						</svg>
					</PrimaryButton>
					<SecondaryButton type="button" @click.native="close">Annuler</SecondaryButton>
				</div>
			</form>
		</div>
	</app-modal>
</template>

<script>
export default {
	data () {
		return {
			name : 'CreateCampaignModal',
			processing : false,
			form : {
				title : '',
				description : ''
			},
			validation : {}
		}
	},
	methods : {
		async submit () {
			this.processing = true

			try {
				let campaign = await this.$axios.$post('campaigns', this.form)

				setTimeout(() => {
					this.processing = false
					this.close()
					this.$router.replace({
						name : 'dashboard-campaigns-slug-settings',
						params : {
							slug : campaign.data.uuid
						}
					})
				},1000)

			} catch (e) {
				this.validation = e.response.data.errors

				setTimeout(() => {
					this.processing = false
				},500)
			}

		},
		reset () {
			this.form.title = '',
			this.form.description = '',
			this.validation = {}
		},
		close () {
			this.$modal.hide(this.name)
			this.reset()
		}
	}
}
</script>
