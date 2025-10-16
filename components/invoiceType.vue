<template>
  <div class="flex border border-gray-200">
    <!-- Left panel with type selection -->
    <div class="w-1/2 space-y-2 p-4">
      <URadio
        v-for="type in types"
        :key="type.id"
        v-model="selectedType"
        :value="type.id"
        class="flex items-center"
      >
        <template #label>
          <div class="flex items-center gap-2">
            <UButton
              :color="type.color"
              variant="outline"
              class="flex items-center gap-2 w-[100px] justify-center"
            >
              <span class="text-sm font-medium">{{ type.label }}</span>
            </UButton>
            <span class="text-sm text-gray-500">{{ type.description }}</span>
          </div>
        </template>
      </URadio>
    </div>

    <!-- Right panel with details -->
    <div class="w-1/2 p-4 bg-gray-50">
      <div v-if="selectedTypeDetails" class="space-y-4">
        <UButton
          :color="selectedTypeDetails.color"
          variant="outline"
          class="flex items-center gap-2 w-[100px] justify-center"
        >
          <span class="text-sm font-medium">{{
            selectedTypeDetails.label
          }}</span>
        </UButton>
        <p class="text-gray-600">{{ selectedTypeDetails.content }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const selectedType = ref(1);

const types = [
  {
    id: 1,
    label: "Type 1",
    description: "Type 1",
    color: "green",
    content:
      "Lorem ipsum dolor sit amet consectetur. Facilisis eleifend eget ut arcu tincidunt eleifend. Sed nisi vestibulum ut lacerat ornare sit. Commodo ultrices scelerisque elementum eu nibh. Tempus purus non viverra cursus sit integer.",
  },
  {
    id: 2,
    label: "Type 2",
    description: "Type 2",
    color: "gray",
    content: "Type 2 details...",
  },
  {
    id: 3,
    label: "Type 3",
    description: "Type 3",
    color: "blue",
    content: "Type 3 details...",
  },
  {
    id: 4,
    label: "Type 4",
    description: "Type 4",
    color: "gray",
    content: "Type 4 details...",
  },
  {
    id: 5,
    label: "Type 5",
    description: "Type 5",
    color: "gray",
    content: "Type 5 details...",
  },
  {
    id: 6,
    label: "Type 6",
    description: "Type 6",
    color: "green",
    content: "Type 6 details...",
  },
  {
    id: 7,
    label: "Type 7",
    description: "Type 7",
    color: "green",
    content: "Type 7 details...",
  },
];

const selectedTypeDetails = computed(() =>
  types.find((type) => type.id === selectedType.value)
);
</script>
