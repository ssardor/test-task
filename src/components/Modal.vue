<template>
  <div v-if="isVisible" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
    <div class="bg-white p-4 rounded shadow-lg">
      <h2 class="text-lg font-bold">{{ title }}</h2>
      <slot></slot>
      <div class="mt-4">
        <button @click="close" class="bg-gray-300 px-4 py-2 rounded">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    title: {
      type: String,
      required: true,
    },
    isVisible: {
      type: Boolean,
      required: true,
    },
  },
  emits: ['close'],
  setup(props, { emit }) {
    const close = () => {
      emit('close');
    };

    return { close };
  },
});
</script>

<style scoped>
/* Стили для модального окна */
</style>
