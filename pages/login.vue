<template>
  <div
    class="h-[100vh] bg-opacity-100"
    style="
      background: url('https://images.pexels.com/photos/1054218/pexels-photo-1054218.jpeg?cs=srgb&dl=pexels-stywo-1054218.jpg&fm=jpg');
      background-size: cover;
      background-position: center;
    "
  >
    <UContainer
      class="flex items-center justify-center h-full w-full gap-[40px]"
    >
      <div class="w-[50%]">
        <div class="relative w-full h-full">
          <img
            src="https://upload.wikimedia.org/wikipedia/commons/a/ab/Gentau_Pic_du_Midi_Ossau.jpg"
            alt="1"
            class="object-cover absolute top-[30px] left-[30px] w-[500px] h-[500px]"
          />
          <img
            src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Kodaki_fuji_frm_shojinko.jpg/800px-Kodaki_fuji_frm_shojinko.jpg"
            alt="2"
            class="object-cover w-[500px] h-[500px]"
          />
        </div>
      </div>
      <div class="w-[50%]">
        <UForm
          :schema="register ? schemaRegister : schema"
          :state="register ? stateRegister : state"
          class="space-y-4 p-10 bg-white rounded-lg flex flex-col"
          @submit="onSubmit"
        >
          <div class="text-2xl text-center">
            {{ register ? "Register" : "Login" }}
          </div>
          <div v-if="!register" class="space-y-4">
            <UFormGroup name="username">
              <UInput v-model="state.username" placeholder="username" />
            </UFormGroup>
            <UFormGroup name="password">
              <UInput type="password" v-model="state.password" placeholder="password" />
            </UFormGroup>
          </div>
          <div v-else class="space-y-4">
            <UFormGroup name="email">
              <UInput v-model="stateRegister.email" placeholder="email" />
            </UFormGroup>
            <div class="flex gap-3 w-full">
              <UFormGroup class="basis-1/2" name="firstname">
                <UInput
                  v-model="stateRegister.firstname"
                  placeholder="firstname"
                />
              </UFormGroup>
              <UFormGroup class="basis-1/2" name="lastname">
                <UInput
                  v-model="stateRegister.lastname"
                  placeholder="lastname"
                />
              </UFormGroup>
            </div>
            <UFormGroup name="passwordRegister">
              <UInput type="password" v-model="stateRegister.passwordRegister" placeholder="password" />
            </UFormGroup>
            <UFormGroup name="Tel">
              <UInput v-model="stateRegister.Tel" placeholder="Tel" />
            </UFormGroup>
            <UFormGroup name="id">
              <UInput v-model="stateRegister.id" placeholder="Id" />
            </UFormGroup>
          </div>
          <UButton type="submit" class="flex justify-center">{{
            register ? "Submit" : "Login"
          }}</UButton>
          <div class="flex w-full h-[1px] bg-gray-300"></div>
          <UButton
            type="button"
            @click="register = !register"
            class="flex justify-center"
            variant="outline"
            >{{ register ? "Login" : "Register" }}</UButton
          >
        </UForm>
        <div></div>
      </div>
    </UContainer>
  </div>
</template>

<script setup>
import { object, string } from "yup";
import { useRouter, useRoute } from "vue-router";
definePageMeta({
  middleware: "auth",
  layout: "login",
});

const register = ref(false);
const router = useRouter();
const token = useCookie("token");
const schema = object({
  username: string().required("Required"),
  password: string()
    .min(8, "Must be at least 8 characters")
    .required("Required"),
});
const schemaRegister = object({
  email: string().required("Required"),
  firstname: string().required("Required"),
  lastname: string().required("Required"),
  passwordRegister: string()
    .min(8, "Must be at least 8 characters")
    .required("Required"),
  Tel: string().required("Required"),
  id: string().required("Required"),
});
const stateRegister = reactive({
  email: undefined,
  firstname: undefined,
  lastname: undefined,
  passwordRegister: undefined,
  Tel: undefined,
  id: undefined,
});
const state = reactive({
  username: undefined,
  password: undefined,
});
const onSubmit = () => {
  if (register.value) {
    //TODO integrate api register
    console.log("register", stateRegister);
  } else {
    //TODO integrate api login
    token.value = "test";
    router.push("/");
  }
};
</script>

<style></style>
