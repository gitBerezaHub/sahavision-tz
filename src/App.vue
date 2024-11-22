<template>
  <div class="app">
    <button @click="openModal">Открыть</button>
    <Modal
      v-if="isModalOpen"
      title="Заголовок модального окна"
      :folders="mockFolders"
      @close="closeModal"
      @select="handleFolderSelect"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Modal from "./components/MainModal.vue";
import { Folder } from "./types";

const isModalOpen = ref(false);
const mockFolders: Folder[] = [
  {
    id: 1,
    name: "Folder 1",
    children: [
      {
        id: 2,
        name: "Folder 1.1",
        children: [],
      },
      {
        id: 3,
        name: "Folder 1.2",
        children: [
          {
            id: 4,
            name: "Folder 1.2.1",
            children: [
              {
                id: 5,
                name: "Folder 1.2.1.1",
                children: [
                  {
                    id: 6,
                    name: "Folder 1.2.1.1.1",
                    children: [
                      {
                        id: 7,
                        name: "Folder 1.2.1.1.1.1",
                        children: [],
                      },
                    ],
                  },
                ],
              },
            ],
          },
        ],
      },
    ],
  },
  {
    id: 8,
    name: "Folder 2",
    children: [],
  },
];

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleFolderSelect = (folderId: number) => {
  console.log(`Выбранная папка имеет ID = ${folderId}`);
  closeModal();
};
</script>

<style scoped>
.app {
  font-family: Arial, sans-serif;
  padding: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
