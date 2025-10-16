<template>
  <div class="relative w-full px-8 py-4 mt-20">
    <div class="relative">
      <!-- Dots and Line -->
      <div class="relative flex justify-between items-center mx-[60px]">
        <!-- Line -->
        <div class="absolute w-full h-[4px]">
          <!-- Active Line -->
          <div
            class="absolute left-0 h-full bg-[#2969E3] transition-all duration-300"
            :style="{ width: `${progressWidth}%` }"
          ></div>
          <!-- Inactive Line -->
          <div
            class="absolute h-full bg-gray-200 transition-all duration-300"
            :style="{ left: `${progressWidth}%`, right: 0 }"
          ></div>
        </div>

        <!-- Steps -->
        <div
          v-for="(step, index) in steps"
          :key="index"
          class="relative flex flex-col items-center"
        >
          <!-- Icon above dot -->
          <div class="absolute -top-20">
            <UIcon
              :name="step.icon"
              class="w-16 h-16"
              :class="[
                index <= currentStep ? 'text-[#2969E3]' : 'text-[#DDDDE7]',
              ]"
            />
          </div>

          <!-- Dot -->
          <!-- <div
            class="w-3 h-3 rounded-full z-10"
            :class="[index <= currentStep ? 'bg-[#2969E3]' : 'bg-[#DDDDE7]']"
          ></div> -->

          <!-- dot with white border -->
          <div class="relative">
            <div class="absolute inset-0 -m-1 bg-white rounded-full"></div>
            <div
              class="relative w-3 h-3 rounded-full"
              :class="[index <= currentStep ? 'bg-[#2969E3]' : 'bg-[#DDDDE7]']"
            ></div>
          </div>
        </div>
      </div>
      <!-- Text Row -->
      <div class="flex justify-between mt-4">
        <div
          v-for="(step, index) in steps"
          :key="`text-${index}`"
          class="w-32 text-center"
        >
          <p
            class="text-sm font-medium"
            :class="index <= currentStep ? '' : 'text-[#DDDDE7]'"
          >
            {{ step.name }}
          </p>
          <p
            class="text-xs mt-1 whitespace-pre-line"
            :class="index <= currentStep ? 'text-gray-600' : 'text-[#DDDDE7]'"
          >
            {{ step.detail }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  steps: {
    type: Array,
    required: true,
  },
  currentStep: {
    type: Number,
    default: 0,
  },
});

// Calculate progress width
const progressWidth = computed(() => {
  if (props.currentStep === 0) return 0;
  return (props.currentStep / (props.steps.length - 1)) * 100;
});
</script>
