<template>
    <div class="flex flex-col gap-y-3
    lg:max-w-2xl lg:mx-auto lg:w-1/2">
        <h1 class="font-bold text-center text-white text-4xl">Todo List</h1>
        <span class="text-gray-400">Add your todos below</span>

        <div class="flex flex-row gap-3">
            <input
            v-model="title"
            @input="validate"
            type="text"
            placeholder="Enter your todo"
            class="flex-grow flex-shrink rounded-full px-4 text-black" />

            <button
            @click="add"
            class="bg-blue-500 flex-grow-0 flex-shrink-0 font-semibold text-white px-3 py-1.5 rounded-full">
                Add Todo
            </button>
        </div>
    </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import { DateTime } from 'luxon';

const title = ref('');

const emits = defineEmits(['add']);

function add()
{
    if (title.value.trim().length === 0) {
        return;
    }

    const todo = {
        id: Date.now(),
        title: title.value,
        edit: false,
        // completed: false,
        createdAt: Math.floor(DateTime.now().setZone('UTC').toMillis() / 1000),
    };

    emits('add', todo);

    title.value = '';
}

function validate()
{
    if (title.value.trim().length === 0) {
        return;
    }


}
</script>