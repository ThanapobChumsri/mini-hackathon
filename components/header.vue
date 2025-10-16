<template>
  <div class="bg-gray-100 h-[48px] flex justify-between px-20 items-center shadow-md">
    <div>
      <img src="@/assets/logo.png" alt="logo" class="w-[30px] h-[30px] object-cover" />
    </div>
    <div class="flex gap-4">
      <div @click="router.push('/')" :class="!route.path.includes('tor') ? 'text-blue-500' : ''">Screen Resume</div>
      <div @click="router.push('/tor')" :class="route.path.includes('tor') ? 'text-blue-500' : ''" >TOR</div>
    </div>
    <div>
      <UButton @click="logout">Logout</UButton>
    </div>
  </div>
</template>

<script setup>
import { useRoute, useRouter } from "vue-router";
const router = useRouter();
const route = useRoute();
const logout = () => {
  //TODO integrate api logout
  useCookie("token").value = null;
  router.push("/login");
};
</script>

<style>

</style>