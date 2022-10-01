<script setup>
import { useUserStore } from "src/stores/user-store.js";
import { ref } from "vue";
import { useQuasar } from "quasar";
import { useRouter } from "vue-router";

const $q = useQuasar();

const userStore = useUserStore();
const router = useRouter();

const email = ref("");
const password = ref("");
const repassword = ref("");

const handleSubmit = async () => {
  try {
    console.log("paso las validaciones");
    await userStore.register(email.value, password.value, repassword.value);
    router.push("/");
    email.value = "";
    password.value = "";
    repassword.value = "";
  } catch (error) {
    console.log("error", error);
    if (error.error) {
      alertDialogBackend(error.error);
    } else if (error.errors[0].msg) {
      alertDialogBackend(error.errors[0].msg);
    } else {
      alertDialogBackend();
    }
  }
};

const alertDialogBackend = (message = "Error en el servidor") => {
  $q.dialog({
    title: "Error",
    message,
  });
};
</script>

<template>
  <q-page class="row justify-center">
    <div class="col-12 col-sm-6 col-md-4">
      <h3>Registro</h3>
      <q-form @submit.prevent="handleSubmit">
        <q-input
          v-model="email"
          label="Ingrese Email"
          type="email"
          :rules="[
            (val) =>
              (val && /[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(val)) ||
              'Formato email incorrecto',
          ]"
        ></q-input>
        <q-input
          v-model="password"
          label="Ingrese Contraseña"
          type="password"
          :rules="[
            (val) =>
              (val && val.length > 5) || 'Contraseña Minimo 6 caracteresr',
          ]"
        ></q-input>

        <q-input
          v-model="repassword"
          label="Repita Contraseña"
          type="password"
          :rules="[
            (val) =>
              (val && val === password) || 'No coinciden las constraseñas',
          ]"
        ></q-input>

        <div>
          <q-btn type="submit"> Login</q-btn>
        </div>
      </q-form>
    </div>
  </q-page>
</template>
