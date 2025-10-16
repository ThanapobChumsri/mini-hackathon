<template>
  <div class="w-full max-w-4xl mx-auto p-4">
    <div class="flex gap-4 items-start">
      <!-- Upload Button -->
      <div
        class="w-24 h-24 border-2 border-dashed border-gray-300 rounded-lg flex flex-col items-center justify-center cursor-pointer hover:border-blue-500 transition-colors"
        @drop.prevent="handleDrop"
        @dragover.prevent
        @dragenter.prevent
        @click="$refs.fileInput.click()"
      >
        <input
          type="file"
          multiple
          @change="handleFileChange"
          class="hidden"
          accept="image/*"
          ref="fileInput"
        />
        <div
          class="cursor-pointer w-full h-full flex flex-col items-center justify-center"
        >
          <Icon name="lucide:plus" class="w-8 h-8 text-gray-400 mb-2" />
          <span class="text-sm text-gray-500">Add Files</span>
        </div>
      </div>
      <!-- Preview Grid -->
      <div class="flex flex-wrap gap-4">
        <div
          v-for="(file, index) in files"
          :key="index"
          class="relative group cursor-pointer w-24 h-24 rounded-lg overflow-hidden border-2"
          :class="
            selectedIndex === index ? 'border-blue-500' : 'border-gray-200'
          "
          @click="selectedIndex = index"
        >
          <img
            :src="file.preview"
            :alt="`Preview ${index + 1}`"
            class="w-full h-full object-cover"
          />
          <div
            class="absolute inset-0 bg-black bg-opacity-0 group-hover:bg-opacity-20 transition-all flex items-center justify-center"
          >
            <div class="flex gap-4 opacity-0 group-hover:opacity-100">
              <Icon
                @click="previewFile(index)"
                name="lucide:eye"
                class="w-6 h-6 text-white hover:scale-110 transition-transform cursor-pointer"
              />
              <Icon
                name="lucide:trash-2"
                @click.stop="removeFile(index)"
                class="w-6 h-6 text-white hover:scale-110 transition-transform cursor-pointer"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <UModal v-model="isOpen">
      <div class="p-4 relative">
        <div
          class="absolute top-2 left-2 z-10 bg-black/50 text-white px-2 py-1 rounded"
        >
          {{ selectedIndex + 1 }} / {{ files.length }}
        </div>

        <img
          :src="files[selectedIndex]?.preview"
          class="w-full h-auto object-cover"
        />

        <UButton
          :disabled="selectedIndex === 0"
          icon="i-heroicons-chevron-left-20-solid"
          color="black"
          class="absolute left-2 top-1/2 -translate-y-1/2 rounded-full opacity-75"
          @click="selectedIndex--"
        />

        <UButton
          :disabled="selectedIndex >= files.length - 1"
          icon="i-heroicons-chevron-right-20-solid"
          color="black"
          class="absolute right-2 top-1/2 -translate-y-1/2 rounded-full opacity-75"
          @click="selectedIndex++"
        />
      </div>
      <!-- <UCarousel
          ref="carouselRef"
          v-slot="{ item }"
          :items="files.map((file) => file.preview)"
          :ui="{ item: 'basis-full' }"
          class="rounded-lg overflow-hidden"
          arrows
        >
          <img :src="item" class="object-cover w-full h-[320px]" />
        </UCarousel> -->
    </UModal>
  </div>
</template>

<script setup>
const emit = defineEmits(["change"]);
const isOpen = ref(false);
const fileInput = ref(null);
const files = ref([]);
const selectedIndex = ref(0);
const carouselRef = ref(null);
const test = ref(0);

const props = defineProps({
  files: {
    type: Array,
    default: () => [],
  },
});

watch(
  () => props.files,
  (newFiles) => {
    console.log("test file", newFiles);
    if (newFiles.length === 0) {
      files.value = [];
    } else {
      files.value = newFiles;
    }
  },
  { immediate: true }
);
// watch(isOpen, async (newValue) => {
//   if (newValue) {
//     // Wait for next tick to ensure component is mounted
//     await nextTick();
//     console.log('Carousel ref after modal open:', carouselRef.value);
//   }
// });

onMounted(() => {});

const handleFileChange = (event) => {
  const selectedFiles = Array.from(event.target.files);
  addFiles(selectedFiles);
  // Clear input value to allow uploading the same file again
  if (fileInput.value) {
    fileInput.value.value = "";
  }
};

const handleDrop = (event) => {
  const droppedFiles = Array.from(event.dataTransfer.files);
  addFiles(droppedFiles);
};

const addFiles = (newFiles) => {
  const filesWithPreviews = newFiles.map((file) => ({
    file,
    preview: URL.createObjectURL(file),
  }));
  files.value = [...files.value, ...filesWithPreviews];

  // Emit both the file objects and the preview data
  emit("change", files.value);
};

const removeFile = (index) => {
  // Revoke object URL for the removed file
  URL.revokeObjectURL(files.value[index].preview);

  // Remove file from array
  files.value.splice(index, 1);

  // Update selected index if necessary
  if (selectedIndex.value >= files.value.length) {
    selectedIndex.value = Math.max(0, files.value.length - 1);
  }

  // Emit updated files
  emit("change", files.value);
};

const previewFile = async (index) => {
  isOpen.value = true;
  // setTimeout(() => {
  //   if (carouselRef.value) {
  //     carouselRef.value.select(index + 1);
  //   }
  // }, 50);
};

// Clean up object URLs when component is unmounted
onBeforeUnmount(() => {
  files.value.forEach((file) => {
    URL.revokeObjectURL(file.preview);
  });
});
</script>
