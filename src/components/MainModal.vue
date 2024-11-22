<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal">
      <h2>{{ title }}</h2>
      <FolderTree :folders="folders" @select="handleSelect" />
      <div class="modal-actions">
        <button @click="handleOk">Ок</button>
        <button @click="$emit('close')">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits } from "vue";
import FolderTree from "./FolderTree.vue";
import { Folder } from "../types";

defineProps<{
  title: string;
  folders: Folder[];
}>();

const emit = defineEmits<{
  (e: "close"): void;
  (e: "select", folderId: number): void;
}>();

const selectedFolderId = ref<number | null>(null);

const handleSelect = (folderId: number) => {
  selectedFolderId.value = folderId;
};

const handleOk = () => {
  if (selectedFolderId.value !== null) {
    emit("select", selectedFolderId.value);
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  min-width: 300px;
}

.modal-actions {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

button {
  padding: 5px 10px;
  cursor: pointer;
}
</style>
