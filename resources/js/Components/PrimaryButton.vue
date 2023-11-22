<script setup>
defineProps({
    type: {
        type: String,
        default: 'submit',
    },
});
</script>

<template>
    <button :type="type" class="inline-flex items-center px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-700 focus:bg-gray-700 active:bg-gray-900 hover:outline-none hover:ring-2 hover:ring-gray-500 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-0 transition ease-in-out duration-150">
        <slot />
    </button>
</template>
