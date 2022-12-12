<template>
  <!-- TEST HAMBURGER -----------------  -->
  <nav>
    <div class="navbar">
      <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
      <router-link to="/">
        <img src="../assets/icons/home-icon.png" alt="" />
      </router-link>
      <button class="btn-hamburger" @click="toggle"></button>

      <!-- Menu for Desktop screen width:  -->
      <div class="nav-big-screen">
        <router-link to="/">Task Manager</router-link>
        <router-link to="/account">Your Account</router-link>

        <div class="container-welcome-logout-big-screen">
          <p>Welcome {{ userYourName }}</p>

          <BlackButton @logOut="signOut"> Log Out </BlackButton>
        </div>
      </div>
    </div>
    <!-- Menu for small screen width:  -->
    <div class="nav-small-screen" v-show="changeBoolean">
      <ul class="links">
        <li class="nav-item">
          <router-link class="nav-link" to="/">Task Manager</router-link>
        </li>

        <li class="nav-item">
          <router-link class="nav-link" to="/account">Your Account</router-link>
        </li>

        <div class="container-welcome-logout">
          <li class="nav-item">
            <p>Welcome {{ userYourName }}</p>
          </li>
          <li class="nav-item">
            <BlackButton @logOut="signOut"> Log Out </BlackButton>
          </li>
        </div>
      </ul>
    </div>
  </nav>
  <!-- Original!! --Hide Router and last button!!-------- -->

  <!-- <nav class="navbar">
    <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link" />
    <router-link to="/">
      <img src="../assets/imgs/pfote.jpg" alt="" />
    </router-link>

    <ul>
      <li>
        <router-link class="link" to="/">Task Manager</router-link>
      </li>

      <li>
        <router-link class="link" to="/account">Your Account</router-link>
      </li>
    </ul>

    <div>
      <ul class="log-out-welcome">
        <li>
          <p>Welcome {{ userEmail }}</p>
        </li>
        <li>
          <BlackButton @logOut="signOut"> Log Out </BlackButton>
          <button @click="signOut" class="button-log-out">Log out</button>
        </li>
      </ul>
    </div>
  </nav> -->
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from "vue";
import BlackButton from "./BlackButton.vue";

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;
const getProfile = useUserStore().profile;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;
const userYourName = getProfile.yourname;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  try {
    // call the user store and send the users info to backend to signOut

    await useUserStore().signOut();
    // then redirect user to the homeView
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    errorMsg.value = error.message;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
  return;
  errorMsg.value = "error";
};

//HAMBURGER
const changeBoolean = ref(false);
const toggle = () => {
  changeBoolean.value = !changeBoolean.value;
};
</script>

<style></style>
