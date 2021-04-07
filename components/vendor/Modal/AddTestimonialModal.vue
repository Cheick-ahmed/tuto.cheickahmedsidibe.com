<template>
	<app-modal :name="name" maxWidth="3xl">
		<div class="px-4 sm:px-12" :class=" dirty ? 'py-6 bg-blue-700' : 'py-4 bg-red-700' ">
			<div class="flex items-center justify-between">
				<h2 id="slide-over-heading" class="text-xl font-medium text-white">
					Soumettre mon témoignage
				</h2>
				<div class="ml-3 h-7 flex items-center">
					<button @click.prevent="close" class="rounded-md  hover:text-white focus:outline-none focus:ring-2 focus:ring-white" :class=" dirty ? 'py-6 text-gray-200 bg-blue-700' : 'py-4 bg-red-700 text-red-200 bg-red-700' ">
						<span class="sr-only">Close panel</span>
						<!-- Heroicon name: outline/x -->
						<svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
						</svg>
					</button>
				</div>
			</div>
		</div>
		<div class="py-5 px-4 bg-white sm:px-12" :class=" dirty ? 'py-6' : 'py-5' ">
			<form @submit.prevent="submit">
				<div class="grid grid-cols-1 gap-x-4 sm:grid-cols-6" :class=" dirty ? 'gap-y-6' : 'gap-y-2' ">
					<div class="sm:col-span-6">
						<form-group label="Nom et Prénom" LabelFor="name" :error=" validation.name ? validation.name[0] : '' ">
							<form-input type="text" v-model="form.name" id="name" :class=" { 'focus:border-red-500 border-red-500' : validation.name} " />
						</form-group>
					</div>

					<div class="sm:col-span-5">
						<form-group label="Email" LabelFor="email" :error=" validation.email ? validation.email[0] : '' ">
							<form-input type="text" v-model="form.email" id="email" :class=" { 'focus:border-red-500 border-red-500' : validation.email} " />
						</form-group>
					</div>


					<div class="sm:col-span-6">
						<form-group label="Poste" LabelFor="role" :error=" validation.role ? validation.role[0] : '' ">
							<form-input type="text" v-model="form.role" id="role" :class=" { 'focus:border-red-500 border-red-500' : validation.role} " />
						</form-group>
					</div>

					<div class="sm:col-span-5">
						<form-group label="Entreprise" LabelFor="company" :error=" validation.company ? validation.company[0] : '' ">
							<form-input type="text" v-model="form.company" id="company" :class=" { 'focus:border-red-500 border-red-500' : validation.company} " />
						</form-group>
					</div>

					<div class="sm:col-span-6">
						<p class="block text-sm font-medium text-gray-900">
							Photo
						</label>
						<div class="mt-1.5">
							<input type="file" id="photo" class="hidden">
							<div class="flex items-center">
								<span class="h-16 w-16 rounded-full overflow-hidden bg-gray-100">
									<svg class="h-full w-full text-gray-300" fill="currentColor" viewBox="0 0 24 24">
										<path d="M24 20.993V24H0v-2.996A14.977 14.977 0 0112.004 15c4.904 0 9.26 2.354 11.996 5.993zM16.002 8.999a4 4 0 11-8 0 4 4 0 018 0z" />
									</svg>
								</span>
								<label for="photo" class="ml-3 bg-white py-2 px-3 border border-gray-300 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
									Ajouter une photo
								</label>
							</div>
						</div>
					</div>

				</div>
				<div class="mt-5 sm:mt-8 flex items-center flex-row-reverse">
					<PrimaryButton class="ml-2 space-x-2 group disabled:opacity-50" :disabled="processing">
						<span>Envoyer</span>
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
import { isEmpty } from 'lodash'

export default {
	data () {
		return {
			name : 'AddTestimonialModal',
			processing : false,
			form : {
				name : '',
				email : '',
				role : '',
				company : '',
				image_url : 'https://eu.ui-avatars.com/api/?name=Ahmed&bold=true&background=0D8ABC&color=fff&rounded=true'
			},
			validation : {}
		}
	},

	computed : {
		dirty () {
			return isEmpty(this.validation)
		}
	},

	methods : {
		async submit () {
			this.processing = true
			try {
				await this.$axios.post('authors', this.form)
				this.$emit('TestimonialWasAdded', this.form)

				setTimeout(() => {
					this.processing = false
					this.close()
				},500)

			} catch (e) {
				this.validation = e.response.data.errors
				this.processing = false
			}

		},

		reset () {
			this.validation = {}
		},

		close () {
			this.$modal.hide(this.name)
			this.reset()
		}
	},

	beforeMount() {
		this.validation = {}
	}
}
</script>