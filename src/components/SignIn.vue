<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="general-container">
    <div class="header-img"></div>
    <div class="container-sign-in">
      <div class="header">
        <div class="header-description">
          <div class="header-logo">
            <img src="../assets/imgs/pfote.jpg" alt="header-img" />
          </div>
          <h3 class="header-title">Log In to ToDoG App</h3>
          <p class="header-subtitle">Start organizing your tasks!</p>
        </div>
      </div>

      <form @submit.prevent="signIn" class="form-sign-in">
        <div class="form-sign-in">
          <div class="form-input">
            <label class="input-field-label">E-mail</label>
            <input
              type="email"
              class="input-field"
              placeholder="example@gmail.com"
              id="email"
              v-model="email"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">Password</label>
            <input
              type="password"
              class="input-field"
              placeholder="**********"
              id="password"
              v-model="password"
              required
            />
          </div>
          <BlackButton type="submit">Sign In</BlackButton>
          <!-- <button class="button" type="submit">Sign In</button> -->
          <p>
            Dont have an account?
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />
          </p>
        </div>
      </form>

      <div class="error" v-show="errorMsg">
        <h5>{{ errorMsg }}</h5>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";
import BlackButton from "../components/BlackButton.vue";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);

    redirect.push({ path: "/" });
  } catch (error) {
    errorMsg.value = error.message;

    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
  return;

  errorMsg.value = "error";
};
</script>

<style>
/* body {
  background-color: black;
} */
</style>
