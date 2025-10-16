<template>
  <div>
    <Header />
    <div class="content-default flex w-full">
      <!-- <sideBar class="min-w-[250px]" /> -->
      <div class="w-full overflow-y-auto">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
// import sideBar from "~/components/sideBar.vue";
import Header from "@/components/header.vue";
</script>

<style>
.content-default {
  min-height: calc(100vh);
  height: 100vh;
}
</style>
