<template>
  <div>
    <ul>
      <li v-for="folder in folders" :key="folder.id">
        <span 
          @click="selectFolder(folder.id)" 
          :class="{'font-bold': selectedFolderId === folder.id}" 
          class="cursor-pointer"
        >
          {{ folder.name }}
        </span>
        <FolderTree
          v-if="folder.children && folder.children.length > 0 && folder.isOpen"
          :folders="folder.children"
          @select="selectFolder"
        />
      </li>
    </ul>
    <button @click="confirmSelection" class="mt-2 bg-blue-500 text-white px-4 py-2 rounded">Ок</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  props: {
    folders: {
      type: Array as () => Array<{ id: number; name: string; children: any[]; isOpen?: boolean }>,
      required: true,
    },
  },
  emits: ['select', 'close'],
  setup(props, { emit }) {
    const selectedFolderId = ref<number | null>(null);

    const initializeFolders = (folders: any[]) => {
      folders.forEach(folder => {
        folder.isOpen = false;
        if (folder.children) {
          initializeFolders(folder.children);
        }
      });
    };

    initializeFolders(props.folders);

    const toggle = (folder: any) => {
      folder.isOpen = !folder.isOpen;
    };

    const selectFolder = (id: number) => {
      selectedFolderId.value = id; // Обновление выбранной папки
      console.log('Выбрана папка с ID:', id); // Логирование
      emit('select', id); // Генерация события выбора
    };

    const confirmSelection = () => {
      console.log('Подтверждение выбора с ID:', selectedFolderId.value); // Логирование перед отправкой
      emit('select', selectedFolderId.value); // Передача выбранного ID
      emit('close'); // Закрытие модального окна
    };

    return { toggle, selectFolder, confirmSelection, selectedFolderId };
  },
});
</script>

<style scoped>
/* Стили для дерева папок */
</style>