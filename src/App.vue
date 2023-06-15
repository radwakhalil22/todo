<template>
  <div class="p-4  flex flex-col place-items-center">
    <h1 class="text-2xl font-bold mb-4 text-slate-900 text-rose-800">Todo List</h1>
    <FormComponent :todo="editingTodo" :isEditing="isEditing" @submit="handleFormSubmit" />

    <ul class="mt-4 w-16 md:w-32 lg:w-60">
      <li v-for="(todo, index) in todos" :key="index" class="mb-2 flex justify-between">
        <div>{{ todo.title }}</div>
        <div>
          <button @click="editTodo(index)" class="mr-2 px-2 py-1 text-sm text-white bg-blue-500 rounded-lg mr-2">Edit</button>
          <button @click="deleteTodo(index)" class="px-2 py-1 text-sm text-white bg-red-500 rounded-lg">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from 'vue';
import FormComponent from './components/FormComponent.vue';

export default {
  components: {
    FormComponent,
  },
  setup() {
    const todos = ref([]);
    const isEditing = ref(false);
    const editingTodo = ref({ title: '' });

    const handleFormSubmit = (todo) => {
      if (isEditing.value) {
        const index = todos.value.findIndex((t) => t.title === editingTodo.value.title);
        if (index !== -1) {
          todos.value[index] = { ...todo };
        }
        isEditing.value = false;
        editingTodo.value = { title: '' };
      } else {
        if (todo && todo.title && todo.title.trim() !== '') {
          todos.value.push({ ...todo });
        }
      }
    };

    const editTodo = (index) => {
      editingTodo.value = { ...todos.value[index] };
      isEditing.value = true;
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };

    return { todos, handleFormSubmit, editTodo, deleteTodo, editingTodo, isEditing };
  },
};
</script>

<style>
button {
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  cursor: pointer;
}
</style>
