<template>
    <button :type="type" class="inline-flex items-center justify-center rounded-md border border-black shadow-sm px-4 py-2 bg-black text-sm font-normal tracking-wide text-white hover:bg-white hover:text-black focus:outline-none sm:text-base transition-colors duration-150">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        props: {
            type: {
                type: String,
                default: 'submit',
            },
        }
    }
</script>
