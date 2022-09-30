<template>
  <q-page padding>
    <!-- <q-btn @click="userStore.access"> Ingresar </q-btn> -->
    <q-btn @click="createLink"> Create Link </q-btn>
    <!-- <q-btn @click="userStore.logout">Cerrar Sesion</q-btn> -->
    {{ userStore.token }} - {{ userStore.expiresIn }}
  </q-page>
</template>

<script setup>
import { api } from "src/boot/axios.js";
import { useUserStore } from "../stores/user-store.js";

const userStore = useUserStore();

userStore.refreshToken();

const createLink = async () => {
  try {
    const res = await api({
      method: "POST",
      url: "/links",
      headers: {
        Authorization: "Bearer " + token.value,
      },
      data: {
        longLink: "https://axios-http.com/es/docs/post_example",
      },
    });
    console.log(res.data);
  } catch (error) {
    console.log(error);
  }
};
</script>
