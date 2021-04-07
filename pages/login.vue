<template>
	<div class="h-screen flex flex-col items-center justify-center">
		<div class="w-6/12">
			<div class="bg-yellow-50 border-l-4 border-yellow-400 px-2 py-3" v-if="tooManyRequestsErrorMessage">
				<div class="flex">
					<div class="flex-shrink-0">
						<!-- Heroicon name: solid/exclamation -->
						<svg class="h-5 w-5 text-yellow-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
							<path fill-rule="evenodd" d="M8.257 3.099c.765-1.36 2.722-1.36 3.486 0l5.58 9.92c.75 1.334-.213 2.98-1.742 2.98H4.42c-1.53 0-2.493-1.646-1.743-2.98l5.58-9.92zM11 13a1 1 0 11-2 0 1 1 0 012 0zm-1-8a1 1 0 00-1 1v3a1 1 0 002 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
						</svg>
					</div>
					<div class="ml-2">
						<p class="text-sm text-yellow-700">
							Trop de tentatives de connexion. Veuillez réessayer dans
							<span class="font-medium text-yellow-700 hover:text-yellow-600">
								{{ loginCountDown }} secondes.
							</span>
						</p>
					</div>
				</div>
			</div>

			<form class="space-y-6" id="loginForm" @submit.prevent="submit">
				<h1 class="text-2xl sm:text-3xl leading-9 font-extrabold md:text-4xl">Connexion   </h1>

				<my-form-group label="Adresse e-mail" LabelFor="email" :error=" validation.email ? validation.email[0] : '' ">
					<my-form-input type="text" v-model="form.email" id="email" placeholder="you@example.com" :class=" { 'focus:border-red-500 border-red-500' : validation.email} " />
				</my-form-group>

				<my-form-group label="Mot de passe" LabelFor="password" :error=" validation.password ? validation.password[0] : '' ">
					<my-form-input type="password" v-model="form.password" id="password" />
				</my-form-group>

				<div class="flex items-center justify-end">
					<div class="text-sm">
						<a href="#" class="font-medium text-blue-600 hover:text-blue-700">
							Mot de passe oublié ?
						</a>
					</div>
				</div>
			</form>

			<div class="mt-6 sm:mx-auto sm:w-full sm:max-w-xl flex items-center justify-between">
				<div>
					<p class="text-base text-gray-600 leading-7 text-center font-medium">
						Pas encore inscrit ?
					</p>
					<nuxt-link :to=" { name : '' } " class="font-bold text-blue-600 hover:text-blue-700">
						S'inscrire
					</nuxt-link>
				</div>
				<div>
					<MyButtonDarkButton form="loginForm" class="space-x-1 py-3 group disabled:opacity-50" :disabled="processing">
						Connexion
					</MyButtonDarkButton>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {

	head () {
		return {
			title : 'La collecte d\'avis clients simpifiée'
		}
	},

	data () {
		return {
			processing : false,
			loginCountDown : 59,
			tooManyRequestsErrorMessage : '',
			timerEnabled : false,
			form : {
				email : '',
				password : ''
			},
			validation : {}
		}
	},

	watch : {
		timerEnabled (value) {
			if (value) {
				setTimeout(() => {
					this.loginCountDown --
				}, 1000)
			}
		},

		loginCountDown : {
			handler(value) {

				if (value > 0 && this.timerEnabled) {
					this.processing = true

					setTimeout(() => {
						this.loginCountDown--
						this.processing = false
					}, 1000);

					setTimeout(() => {
						this.tooManyRequestsErrorMessage = ''
					}, 60000);


				}

			},
			immediate: true
		}
	},

	methods : {
		async submit () {

			if (this.processing === true) {
				return
			}

			this.processing = true

			try {

				await this.$auth.loginWith('laravelSanctum', { data : this.form })

				this.processing = false

				this.$router.replace({
					name : 'dashboard'
				})

			} catch (e) {
				console.log(e)
				if (e.response.status === 422) {
					this.validation = e.response.data.errors
				}

				if (e.response.status === 429) {
					this.tooManyRequestsErrorMessage = e.response.data
					this.timerEnabled = true
					this.loginCountDown = 59
				}

				this.processing = false

			}
		}
	}
}
</script>