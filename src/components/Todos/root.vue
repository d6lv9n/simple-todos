<template>
    <div class="flex flex-col gap-y-6 mt-1.5 w-full
    lg:max-w-2xl lg:mx-auto lg:w-1/2">
        <Todo
        v-for="todo in todos"
        @edit="() => {}"
        @delete="() => {}"
        :key="todo.id"
        :todo="todo"
        />
    </div>
</template>

<script setup>
import Todo from './todo.vue';
// import { defineProps } from 'vue';

defineProps({
    todos: {
        type: Array,
        required: true,
    },
});
</script>