<template>
    <span>
        <app-modal :name="name" maxWidth="xl">

            <div class="space-y-4 sm:space-y-6 px-6 py-4">
                <div class="space-y-2">
                    <div class="text-lg">
                        Confirmez le mot de passe
                    </div>
                    <p class="text-base text-gray-500">
                        {{ content }}
                    </p>
                </div>

                <form-group label="Mot de passe" LabelFor="password" error="Mot de passe incorrect">
                    <form-input type="text" ref="password" v-model="form.password" @keyup.enter.native="confirmPassword" id="password" placeholder="you@example.com" />
                </form-group>

            </div>

            <div class="px-6 py-4 bg-gray-100 text-right">
                <secondary-button @click.native="isConfirmingPassword">
                    Annuler 
                </secondary-button>

                <DarkButton class="ml-2" @click.native="confirmPassword" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Valider
                </DarkButton>
            </div>
        </app-modal>
    </span>
</template>

<script>


export default {
    props: {
        name: {
            default: 'ConfirmationPasswordModal',
        },
        content: {
            default: 'Pour des raisons de sécurité, merci de confirmer votre mot de passe.',
        }
    },

    data() {
        return {
            confirmingPassword: false,

            form: {
                password: '',
                error: '',
                processing : false
            },
        }
    },

    methods: {
        isConfirmingPassword () {
            this.confirmingPassword
            ? console.log('hello')
            : this.close()
        },

        close () {
            this.form.password = ''
            this.$modal.hide(this.name)
        },

        startConfirmingPassword() {
                // this.form.error = '';

                // axios.get(route('password.confirmation').url()).then(response => {
                //     if (response.data.confirmed) {
                //         this.$emit('confirmed');
                //     } else {
                //         this.confirmingPassword = true;
                //         this.form.password = '';

                //         setTimeout(() => {
                //             this.$refs.password.focus()
                //         }, 250)
                //     }
                // })
            },

            confirmPassword() {
                this.form.processing = true;

                // axios.post(route('password.confirm').url(), {
                //     password: this.form.password,
                // }).then(response => {
                //     this.confirmingPassword = false;
                //     this.form.password = '';
                //     this.form.error = '';
                //     this.form.processing = false;

                //     this.$nextTick(() => this.$emit('confirmed'));
                // }).catch(error => {
                //     this.form.processing = false;
                //     this.form.error = error.response.data.errors.password[0];
                // });
            }
        }
    }
    </script>
