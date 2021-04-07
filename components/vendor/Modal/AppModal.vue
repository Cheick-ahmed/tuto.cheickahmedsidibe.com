<template>
    <div>
        <transition leave-active-class="duration-200">
            <div v-if="show" class="fixed z-50 top-0 inset-x-0 px-4 pt-6 sm:px-0 sm:flex sm:items-top sm:justify-center">
                <div class="fixed inset-0 transform transition-all" @click.prevent="$modal.hide(name)">
                    <div class="absolute inset-0 bg-black opacity-75"></div>
                </div>

                <transition enter-active-class="ease-out duration-300" enter-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to-class="opacity-100 translate-y-0 sm:scale-100" leave-active-class="ease-in duration-200" leave-class="opacity-100 translate-y-0 sm:scale-100" leave-to-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
                    <div v-if="show" class="overflow-hidden rounded-lg  transform transition-all sm:w-full" :class="maxWidthClass">
                        <slot :params="params"></slot>
                    </div>
                </transition>
            </div>
        </transition>
    </div>
</template>

<script>
export default {
    props: {
        name : {
            required : true,
            type : String
        },
        maxWidth: {
            default: '2xl'
        },
        closeable: {
            default: true
        },
    },

    data () {
        return {
            show : false,
            params : {}
        }
    },

    methods: {
        close() {
            if (this.closeable) {
                this.$modal.hide(this.name)
            }
        },
    },

    beforeMount () {
        this.$modal.$event.$on('show', (modal, params) => {
            if (this.name === modal) {
                this.params = params
                this.show = true
            }
        })
        this.$modal.$event.$on('hide', (modal) => {
            if (this.name === modal) {
                this.show = false
            }
        })
    },

    mounted() {
        const closeOnEscape = (e) => {
            if (e.key === 'Escape' && this.show) {
                this.close()
            }
        }

        document.addEventListener('keydown', closeOnEscape)

        this.$once('hook:destroyed', () => {
            document.removeEventListener('keydown', closeOnEscape)
        })
    },

    computed: {
        maxWidthClass() {
            return {
                'sm': 'sm:max-w-sm',
                'md': 'sm:max-w-md',
                'lg': 'sm:max-w-lg',
                'xl': 'sm:max-w-xl',
                '2xl': 'sm:max-w-2xl',
                '3xl' : 'sm:max-w-3xl',
                '4xl' : 'sm:max-w-4xl',
                '5xl' : 'sm:max-w-5xl',
            }[this.maxWidth]
        }
    }
}
</script>
