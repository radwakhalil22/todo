<template>
  <div class="p-4 w-16 md:w-32 lg:w-60 ">
    <form @submit.prevent="handleSubmit">
      <input
        type="text"
        v-model="localTodo.title"
        placeholder="Enter todo title"
        class="border rounded p-2 mb-2"
      />
      <button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded">
        {{ isEditing ? 'Update' : 'Create' }}
      </button>
    </form>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  props: {
    todo: {
      type: Object,
      default: () => ({ title: '' }),
    },
    isEditing: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, { emit }) {
    const localTodo = ref({ ...props.todo });


    watch(
      () => props.todo,
      (newValue) => {
        localTodo.value = { ...newValue };
      }
    );

    const handleSubmit = () => {
      emit('submit', localTodo.value);
      localTodo.value = { title: '' };
    };

    return { localTodo, handleSubmit };
  },
};
</script>

<style scoped>
input,
button {
  width: 100%;
}
</style>
