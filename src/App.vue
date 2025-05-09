<template>
  <div class="flex flex-col gap-y-6 min-h-screen p-6
  sm:max-w-lg sm:mx-auto
  lg:flex-row lg:gap-x-6 lg:items-center lg:max-w-4xl lg:mx-auto">
    <Form
    @add="addTodo"/>

    <Todos
    v-if="todos.length > 0"
    :todos="todos"
    />
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
  console.log('Added todo:', todo);

  if (todos.value.some((t) => t.title === todo.title)) {
    console.error('Todo already exists');
    return;
  }

  if (todos.value.length >= 5) {
    console.error('Todo list is full');
    return;
  }

  // This will trigger the watcher
  todos.value = [ ...todos.value, todo];

  console.log('Todos:', todos.value);
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
  console.log('Mounted');
  mounted.value = true;
});
</script>