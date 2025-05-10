<template>
  <div class="flex flex-col gap-y-6 min-h-screen p-6
  sm:max-w-lg sm:mx-auto
  lg:flex-row lg:gap-x-6 lg:items-center lg:max-w-4xl lg:mx-auto">
    <Form
    @add="addTodo"/>

    <Todos
    v-if="todos.length > 0"
    @editTodo="editTodo"
    @deleteTodo="deleteTodo"
    @updateTodo="updateTodo"
    :todos="todos"/>
  </div>
</template>

<script setup>
import { onBeforeMount, onMounted, reactive, ref, watch } from 'vue';
import Form from './components/Form.vue';
import Todos from './components/Todos/root.vue';

const mounted = ref(false);
const todos = ref([]);

watch(todos, (newTodos) => {
  console.log('Todos changed:', newTodos);

  if (! mounted.value) {
    return;
  }

  setLocalTodos(newTodos);
});

function addTodo(todo)
{
  if (todos.value.length >= 5 || todos.value.some((t) => t.title === todo.title)) {
    return;
  }

  // This will trigger the watcher
  todos.value = [ ...todos.value, todo];
}

function editTodo(id)
{
  if (todos.value.length === 0) {
    return;
  }

  const index = todos.value.findIndex(item => item.id === id);

  if (index === -1 || todos.value[index].edit) {
    return;
  }

  todos.value = todos.value.map(todo => {
    return todo.id === id
    ? { ...todo, edit: true }
    : { ...todo, edit: false }
  });

  console.log('new todos:', todos.value);
}

function deleteTodo(id)
{
  if (todos.value.length === 0) {
    return;
  }

  const todo = todos.value.find(item => item.id === id);

  if (! todo || todo.edit) {
    return;
  }

  todos.value = todos.value.filter(item => item.id !== todo.id);
}

function updateTodo({ id, title })
{
  console.log('updateTodo:', { id, title });

  if (todos.value.length === 0 || ! todos.value.find(item => item.id === id)) {
    return;
  }

  todos.value = todos.value.map(todo => {
    return todo.id === id
    ? { ...todo, title, edit: false }
    : { ...todo }
  });
}

function getLocalTodos()
{
  const localTodos = window.localStorage.getItem('todos');

  if (localTodos === null || typeof localTodos !== 'string') {
    return [];
  }

  return localTodos;
}

function setLocalTodos(todos)
{
  window.localStorage.setItem('todos', JSON.stringify(todos));
}

function validateTodos(localTodos)
{
  try {
    const parsedTodos = JSON.parse(localTodos);

    if (! Array.isArray(parsedTodos)) {
      throw new Error('Invalid todos format');
    }

    const isValid = parsedTodos.every((todo) => {
      return (
        typeof todo === 'object'
        && todo !== null
        && typeof todo.title === 'string'
        && typeof todo.edit === 'boolean'
        && typeof todo.createdAt === 'number'
      );
    });

    if (! isValid) {
      throw new Error('Invalid todos format');
    }

    parsedTodos.forEach(item => {
      item.edit = false; // Set edit mode to false
    });

    return parsedTodos;
  } catch (exception) {
    console.error(exception);

    return [];
  }
}

onBeforeMount(() => {
  const localTodos = validateTodos(getLocalTodos());

  setLocalTodos(localTodos);

  todos.value = localTodos;
});
onMounted(() => {
  mounted.value = true;
});
</script>