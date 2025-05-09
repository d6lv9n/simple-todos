<template>
    <div class="bg-gray-600 p-3 relative rounded-lg w-full">
        <h3 class="font-semibold text-lg text-white">{{ props.todo.title }}</h3>

        <span class="text-gray-300 text-sm">{{ createdAt }}</span>

        <div class="absolute -top-3 right-0">
            <button
            @click="() => {}"
            class="bg-green-500 text-white px-2 py-1 rounded-full mr-2">
                Edit
            </button>
            <button
            @click="$emit('delete', todo.id)"
            class="bg-red-500 text-white px-2 py-1 rounded-full">
                Delete
            </button>
        </div>
    </div>
</template>

<script setup>
import { computed } from 'vue';
import { DateTime } from 'luxon';

const createdAt = computed(() => {
    const time = DateTime.fromSeconds(props.todo.createdAt);

    const timeGap = currentTime.diff(time, ['days', 'hours', 'minutes', 'seconds']);

    if (timeGap.days > 0) {
        return `${timeGap.days} days ago`;
    }

    if (timeGap.hours > 0) {
        return `${timeGap.hours} hours ago`;
    }

    if (timeGap.minutes > 0) {
        return `${timeGap.minutes} minutes ago`;
    }

    return `${timeGap.seconds} seconds ago`;
});

const currentTime = DateTime.now();

defineEmits(['edit', 'delete']);

const props = defineProps({
    todo: {
        type: Object,
        required: true,
    },
});
</script>