<template>
  <nav>
    <div class="navbar">
      <router-link to="/">
        <img src="../assets/icons/home-icon.png" alt="" />
      </router-link>
      <button class="btn-hamburger" @click="toggle"></button>

      <!-- Menu for Desktop screen width:  -->
      <div class="nav-big-screen">
        <router-link to="/">Task Manager</router-link>
        <router-link to="/account">Your Account</router-link>

        <div class="container-welcome-logout-big-screen">
          <!-- Avatar Image inside the NAV BAR - show the default avatar if no pic is uploaded yet-->
          <img
            v-if="avatar_url"
            class="img-avatar-nav"
            :src="storageAvatarURL + avatar_url"
            alt="avatar-default"
          />
          <img
            v-else
            class="img-avatar-nav"
            src="../assets/imgs/avatar-default.png"
            alt="avatar-default"
          />
          <p v-if="yourname">Welcome {{ yourname }}</p>

          <BlackButton @logOut="signOut"> Log Out </BlackButton>
        </div>
      </div>
    </div>
    <!-- Menu for small screen width:  -->

    <div class="nav-small-screen" v-show="changeBoolean">
      <ul class="links">
        <li>
          <router-link to="/">Task Manager</router-link>
        </li>

        <li>
          <router-link to="/account">Your Account</router-link>
        </li>

        <li v-if="yourname">
          <p>Welcome {{ yourname }}</p>
        </li>
        <!-- Avatar Image inside the NAV BAR - show the default avatar if no pic is uploaded yet-->
        <li>
          <img
            v-if="avatar_url"
            class="img-avatar-nav"
            :src="storageAvatarURL + avatar_url"
            alt="avatar-default"
          />
          <img
            v-else
            class="img-avatar-nav"
            src="../assets/imgs/avatar-default.png"
            alt="avatar-default"
          />
        </li>
        <li>
          <p>{{ email }}</p>
        </li>
        <li>
          <BlackButton @logOut="signOut"> Log Out </BlackButton>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref, toRefs, watch } from "vue";
import BlackButton from "./BlackButton.vue";
import { supabase } from "../supabase";

//constant to save a variable that will hold the use router method

// constant to save a variable that will get the user from store with a computed function imported from vue

const userStore = useUserStore();
const avatar_url = ref(null);
const yourname = ref(null);
const email = ref(null);

const getProfile = async () => {
  await userStore.fetchUser();
  yourname.value = userStore.profile.yourname;
  avatar_url.value = userStore.profile.avatar_url;
  email.value = userStore.user.email;
};
getProfile();

//Const that stores the URL of the Storage for the avatar pictures
const storageAvatarURL = `https://urhcynxgozxdqhzkmsqb.supabase.co/storage/v1/object/public/avatar/`;

//------------------
//------------------
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

//Toggle HAMBURGER
const changeBoolean = ref(false);
const toggle = () => {
  changeBoolean.value = !changeBoolean.value;
};
</script>

<style scoped></style>
