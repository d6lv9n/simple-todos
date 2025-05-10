<template>
    <div class="flex flex-col gap-y-8 mt-1.5 w-full
    lg:max-w-2xl lg:mx-auto lg:w-1/2">
        <Todo
        v-for="todo in todos"
        @editTodo="$emit('editTodo', todo.id)"
        @deleteTodo="$emit('deleteTodo', todo.id)"
        @updateTodo="$emit('updateTodo', $event)"
        :key="todo.id"
        :todo="todo"/>
    </div>
</template>

<script setup>
import Todo from './todo.vue';
// import { defineProps } from 'vue';

const { todos } = defineProps({
    todos: { type: Array, required: true },
});

defineEmits(['editTodo', 'deleteTodo', 'updateTodo']);
</script>