<template>
    <Teleport to="body">
        <Transiton name="modal-outside">
            <div
                v-show="modalActive "
                class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-between px-8">
                <Transition name="modal-inside">
                    <div
                        v-if="modalActive"
                        class="p-4 bg-white self-start mt-32  max-w-screen-md">
                        <slot/>
                        <button class="text-white mt-8 bg-weather-primary py-2 px-6" @click="$emit('close-modal')">Close</button>
                    </div>
                </Transition>
            </div>
        </Transiton>
    </Teleport>
</template>

<script setup>
defineEmits (['close-modal']);
defineProps({
    modalActive:{
        type:Boolean,
        default:false,
    },
})
</script>

<style scoped>
.modal-outside-enter-active,
.modal-outside-leave-active {
    transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}
.modal-outside-enter-from,
.modal-outside-enter-to {
    opacity: 0;
}
.modal-inside-enter-active {
    transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}
.modal-inside-leave-active {
    transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);   
}
.modal-inside-enter-from {
    opacity: 0;
    transform: scale(0.8);
}
.modal-inside-enter-to {
    transform: scale(0.8);
}
</style>