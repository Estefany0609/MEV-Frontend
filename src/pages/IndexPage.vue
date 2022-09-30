<template>
  <q-page padding>
    <q-btn @click="access"> Ingresar </q-btn>
    <q-btn @click="createLink"> Create Link </q-btn>
    {{ token }} - {{ expiresIn }}
  </q-page>
</template>

<script setup>
import { api } from "src/boot/axios.js";
import { ref } from "vue";

const token = ref("");
const expiresIn = ref("");

const access = async () => {
  try {
    console.log("me diste click");
    const res = await api.post("/auth/login", {
      email: "tefo@gmail.com",
      password: "123456",
    });
    token.value = res.data.token;
    expiresIn.value = res.data.expiresIn;
    console.log(res.data);
    setTime();
  } catch (error) {
    console.log(error);
  }
};

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

const setTime = () => {
  setTimeout(() => {
    console.log("se refresco");
    refreshToken();
  }, expiresIn.value * 1000 - 6000);
};

const refreshToken = async () => {
  try {
    const res = await api.get("/auth/refresh");
    token.value = res.data.token;
    expiresIn.value = res.data.expiresIn;
    setTime();
  } catch (error) {
    console.log(error);
  }
};

refreshToken();
</script>
