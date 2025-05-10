<template>
    <div class="bg-gray-600 pb-3 pt-6 px-3 relative rounded-lg w-full">
        <input
        v-if="todo.edit"
        v-model="title"
        type="text"
        class="bg-transparent border-b-2 border-blue-500 font-semibold text-lg text-white w-full" />
        <h3
        v-else
        class="border-b-2 border-transparent font-semibold text-lg text-white">{{ todo.title }}</h3>

        <span class="text-gray-300 text-sm">{{ createdAt }}</span>

        <div class="absolute flex flex-grow-0 flex-row flex-shrink-0 gap-x-3 -top-4 right-0">
            <button
            v-if="todo.edit"
            @click="update"
            class="bg-green-500 text-white px-2 py-1 rounded-full">
                Save
            </button>
            <button
            v-else
            @click="$emit('editTodo')"
            class="bg-green-500 text-white px-2 py-1 rounded-full">
                Edit
            </button>
            <button
            @click="$emit('deleteTodo')"
            class="bg-red-500 text-white px-2 py-1 rounded-full">
                Delete
            </button>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { DateTime } from 'luxon';

const { todo } = defineProps({
    todo: { type: Object, required: true },
});

const createdAt = computed(() => {
    const time = DateTime.fromSeconds(todo.createdAt);

    const timeGap = currentTime.diff(time, ['days', 'hours', 'minutes', 'seconds']);

    if (timeGap.days > 0) {
        return `${timeGap.days} days ago`;
    }

    if (timeGap.hours > 0 && timeGap.days === 0) {
        return `${timeGap.hours} hours ago`;
    }

    if (timeGap.minutes > 0 && timeGap.hours === 0) {
        return `${timeGap.minutes} minutes ago`;
    }

    if (timeGap.seconds > 5 && timeGap.minutes === 0) {
        return `${timeGap.seconds} seconds ago`;
    }

    return 'Just now';
});
const isSame = computed(() => title.value === todo.title);

const currentTime = DateTime.now();
const title = ref(todo.title);

const emits = defineEmits(['editTodo', 'deleteTodo', 'updateTodo']);

function update()
{
    if (! todo.edit || title.value.trim().length === 0 || title.value.trim() === todo.title) {
        return;
    }

    emits('updateTodo', { id: todo.id, title: title.value.trim() });
}
</script>