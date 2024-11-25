<template>
  <div class="p-4">
    <button @click="openModal" class="bg-green-500 text-white px-4 py-2 rounded">Открыть</button>
    <Modal :title="'Выберите папку'" :isVisible="isModalVisible" @close="closeModal">
      <FolderTree :folders="mockFolders" @select="handleSelect" @close="closeModal" />
    </Modal>
    <div v-if="selectedFolder" class="mt-4">
      <h2 class="text-lg font-bold">Выбрана папка:</h2>
      <p>ID: {{ selectedFolder.id }}</p>
      <p>Название: {{ selectedFolder.name }}</p>
      <h3 class="mt-2">Дочерние папки:</h3>
      <ul>
        <li v-for="child in selectedFolder.children" :key="child.id">
          {{ child.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Modal from './components/Modal.vue';
import FolderTree from './components/FolderTree.vue';

const mockFolders = [
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3,
        name: 'Папка 1.2',
        children: [{ id: 4, name: 'Папка 1.2.1', children: [] }],
      },
    ],
  },
  { id: 5, name: 'Папка 2', children: [] },
];

export default defineComponent({
  components: { Modal, FolderTree },
  setup() {
    const isModalVisible = ref(false);
    const selectedFolder = ref<{ id: number; name: string; children: any[] } | null>(null);

    const openModal = () => {
      isModalVisible.value = true;
    };

    const closeModal = () => {
      isModalVisible.value = false;
    };

    const handleSelect = (id: number) => {
      console.log('Выбрана папка с ID:', id);
      selectedFolder.value = findFolderById(mockFolders, id);
    };

    const findFolderById = (folders: any[], id: number) => {
      for (const folder of folders) {
        if (folder.id === id) {
          return folder;
        }
        if (folder.children) {
          const found = findFolderById(folder.children, id);
          if (found) return found;
        }
      }
      return null;
    };

    return { isModalVisible, openModal, closeModal, handleSelect, mockFolders, selectedFolder };
  },
});
</script>

<style>
/* Глобальные стили */
</style>
