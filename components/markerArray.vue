<template>
  <div class="flex flex-col items-center justify-center">
    <div class="relative inline-block">
      <img
        @click="placeMarkerArray"
        src="https://media.istockphoto.com/id/1363086064/vector/usa-map-with-divided-states-on-a-transparent-background.jpg?s=612x612&w=0&k=20&c=rtw_UwKtPozK_QaNwOUHHuQMLXHvTU5mgK5hE7fZXyw="
        alt="Sample Image"
      />
      <!-- Render all markers -->
      <div
        v-for="(marker, index) in markers"
        :key="marker.id"
        class="absolute w-4 h-4 bg-[#2969E3] text-white rounded-full -translate-x-1/2 -translate-y-1/2 flex items-center justify-center text-xs"
        :style="{ left: `${marker.x}%`, top: `${marker.y}%` }"
      >
        {{ index + 1 }}
      </div>
    </div>
    <UModal v-model="isOpen" @update:modelValue="updateIsOpen">
      <UCard>
        <template #header>
          <div class="flex gap-2 items-center">
            <div
              class="w-6 h-6 rounded-full bg-[#2969E3] flex items-center justify-center text-white"
            >
              {{
                state.isEdit
                  ? markers.findIndex((marker) => marker.id === state.id) + 1
                  : markers.length + 1
              }}
            </div>
            <div>Top Tank</div>
          </div>
        </template>
        <div class="max-w-2xl mx-auto p-4">
          <!-- Form Container -->
          <UForm :schema="schema" :state="state" @submit="onSubmit">
            <div class="flex gap-4">
              <div class="w-32">Category</div>
              <UFormGroup
                name="category"
                required
                class="validate-input w-full"
              >
                <USelectMenu
                  size="xl"
                  v-model="state.category"
                  :options="test"
                  value-attribute="label"
                />
              </UFormGroup>
            </div>
            <div class="flex gap-4">
              <div class="w-32">Location</div>
              <UFormGroup
                name="location"
                required
                class="validate-input w-full"
              >
                <USelectMenu
                  size="xl"
                  v-model="state.location"
                  :options="test"
                  value-attribute="label"
                />
              </UFormGroup>
            </div>
            <div class="flex gap-4">
              <div class="w-32">Damage Code</div>
              <UFormGroup
                name="damageCode"
                required
                class="validate-input w-full"
              >
                <USelectMenu
                  size="xl"
                  v-model="state.damageCode"
                  :options="test"
                  value-attribute="label"
                />
              </UFormGroup>
            </div>

            <div class="flex gap-4">
              <div class="w-32">Repair Code</div>
              <UFormGroup
                name="repairCode"
                required
                class="validate-input w-full"
              >
                <USelectMenu
                  size="xl"
                  v-model="state.repairCode"
                  :options="test"
                  value-attribute="label"
                />
              </UFormGroup>
            </div>

            <div class="flex gap-4">
              <div class="w-32">Description</div>
              <UFormGroup
                name="description"
                required
                class="validate-input w-full"
              >
                <UTextarea size="xl" v-model="state.description" />
              </UFormGroup>
            </div>

            <div class="flex gap-4">
              <div class="w-32">Images</div>
              <UFormGroup name="images" class="validate-input w-full">
                <UploadFile @change="handleFileChange" :files="state.images" />
              </UFormGroup>
            </div>

            <!-- Form Actions -->
            <div class="grid grid-cols-2 mt-6 gap-4">
              <UButton
                type="button"
                class="w-full justify-center"
                variant="outline"
                @click="updateIsOpen(false)"
              >
                Cancel
              </UButton>
              <UButton
                type="submit"
                class="w-full justify-center"
                color="primary"
              >
                Submit
              </UButton>
            </div>
          </UForm>
        </div>
      </UCard>
    </UModal>
  </div>
</template>

<script setup>
import { object, string, array } from "yup";
import UploadFile from "~/components/upload.vue";
const emit = defineEmits(["output"]);
const props = defineProps({
  markerList: {
    type: Array,
    default: () => [],
  },
});
const state = reactive({
  category: "",
  location: "",
  damageCode: "",
  repairCode: "",
  description: "",
  images: [],
  isEdit: false,
});
const isOpen = ref(false);
const markers = ref( props.markerList || []);
const xPercentArray = ref(null);
const yPercentArray = ref(null);
const test = ref([
  {
    label: "Option 1",
    value: "option1",
  },
]);

const schema = object({
  category: string().required("please select a category"),
  location: string().required("please select a location"),
  damageCode: string().required("please select a damage code"),
  repairCode: string().required("please select a repair code"),
  description: string().required("please type a description"),
  images: array().min(1, "Please select images"),
});

const onSubmit = () => {
  console.log("test", markers.value);
  if (state.isEdit) {
    const markerToEdit = markers.value.find((marker) => marker.id === state.id);

    // If found, update its properties directly
    if (markerToEdit) {
      markerToEdit.x = xPercentArray.value;
      markerToEdit.y = yPercentArray.value;
      markerToEdit.category = state.category;
      markerToEdit.location = state.location;
      markerToEdit.damageCode = state.damageCode;
      markerToEdit.repairCode = state.repairCode;
      markerToEdit.description = state.description;
      markerToEdit.images = state.images;
      markerToEdit.isEdit = false; // Reset edit mode
    }
  } else {
    markers.value.push({
      id: Date.now(),
      x: xPercentArray.value,
      y: yPercentArray.value,
      category: state.category,
      location: state.location,
      damageCode: state.damageCode,
      repairCode: state.repairCode,
      description: state.description,
      images: state.images,
      isEdit: state.isEdit,
    });
  }
  console.log("test eiei", markers.value);
  emit("output", markers.value);
  updateIsOpen(false);
  clearState();
};

const handleFileChange = (value) => {
  console.log("marker vue");

  state.images = value;
};
const clearState = () => {
  state.category = null;
  state.location = null;
  state.damageCode = null;
  state.repairCode = null;
  state.description = null;
  state.images = [];
  state.isEdit = false;
};

const editMarker = (marker) => {
  state.id = marker.id;
  state.category = marker.category;
  state.location = marker.location;
  state.damageCode = marker.damageCode;
  state.repairCode = marker.repairCode;
  state.description = marker.description;
  state.images = marker.images;
  xPercentArray.value = marker.x;
  yPercentArray.value = marker.y;
  state.isEdit = true;
  updateIsOpen(true);
};
//marker array
function removeMarkerArray(markerId) {
  console.log("Removing marker with id:", markerId);
  markers.value = markers.value.filter((marker) => marker.id !== markerId);
  emit("output", markers.value);
}
function placeMarkerArray(event) {
  isOpen.value = true;
  const img = event.currentTarget;
  const rect = img.getBoundingClientRect();

  // Calculate position for new marker
  xPercentArray.value = ((event.clientX - rect.left) / rect.width) * 100;
  yPercentArray.value = ((event.clientY - rect.top) / rect.height) * 100;

  console.log("Added marker at:", xPercentArray.value, yPercentArray.value);
}
const updateIsOpen = (value) => {
  isOpen.value = value;
  if (!value) {
    clearState();
  }
};
defineExpose({
  removeMarkerArray,
  editMarker,
});
</script>

<style></style>
