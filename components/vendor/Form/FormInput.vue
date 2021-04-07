<template>
    <input class="px-3 py-2 shadow-sm focus:ring-blue-600 focus:border-blue-600 block w-full border-gray-300 rounded-md sm:text-sm sm:py-3" :value="value" @input="$emit('input', $event.target.value)" ref="input" autocomplete="off">
</template>

<script>
    export default {
        props: ['value'],
        methods: {
            focus() {
                this.$refs.input.focus()
            }
        }
    }
</script>

