<template>
  <ul class="folder-tree">
    <li v-for="folder in folders" :key="folder.id">
      <div class="folder" @click="toggleFolder(folder)">
        <span v-if="folder.children.length">
          {{ isExpanded(folder) ? "▼" : "▶" }}
        </span>
        <span
          @click.stop="selectFolder(folder)"
          :class="{ selected: isSelected(folder) }"
        >
          {{ folder.name }}
        </span>
      </div>
      <FolderTree
        v-if="isExpanded(folder) && folder.children.length"
        :folders="folder.children"
        @select="handleSelect"
      />
    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits } from "vue";
import { Folder } from "@/types";

const props = defineProps<{
  folders: Folder[];
}>();

const emit = defineEmits<{
  (e: "select", folderId: number): void;
}>();

const expandedFolders = ref<Set<number>>(new Set());
const selectedFolder = ref<number | null>(null);

const toggleFolder = (folder: Folder) => {
  if (expandedFolders.value.has(folder.id)) {
    expandedFolders.value.delete(folder.id);
  } else {
    expandedFolders.value.add(folder.id);
  }
};

const isExpanded = (folder: Folder) => expandedFolders.value.has(folder.id);

const selectFolder = (folder: Folder) => {
  selectedFolder.value = folder.id;
  emit("select", folder.id);
};

const isSelected = (folder: Folder) => selectedFolder.value === folder.id;

const handleSelect = (folderId: number) => {
  emit("select", folderId);
};
</script>

<style scoped>
.folder-tree {
  list-style-type: none;
  padding-left: 20px;
}

.folder {
  cursor: pointer;
  user-select: none;
}

.selected {
  font-weight: bold;
}
</style>
