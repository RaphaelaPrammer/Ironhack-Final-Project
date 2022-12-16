<template>
  <Nav />

  <h1 v-if="yourname">Hello &#127789; {{ yourname }} !</h1>
  <h1 v-else>Hello &#127789; {{ userStore.user.email }} !</h1>
  <h2>Your Account:</h2>

  <div class="container-account">
    <Avatar v-model:path="avatar_url" @upload="updateProfile" size="10" />

    <div>
      <h4>
        <img src="../assets/icons/scoobydoo.png" alt="HELLO" />{{
          userStore.user.email
        }}
      </h4>
      <h4><img src="../assets/icons/dog-robot.png" alt="" />{{ website }}</h4>
      <br />
    </div>
    <div>
      <BlackButton @click="changeBoolean">Edit Profile</BlackButton>
      <div v-show="boolean">
        <div class="edit-profile-container">
          <div>
            <label for="yourname">Your Name</label><br />
            <input id="yourname" type="text" v-model.lazy="yourname" />
          </div>

          <div>
            <label for="website">Website</label><br />
            <input id="website" type="website" v-model.lazy="website" />
          </div>
        </div>
        <BlackButton @logOut="updateProfile">Update </BlackButton>
      </div>
    </div>
    <BlackButton @click="signOut">Log Out</BlackButton>
  </div>
</template>

<script setup>
import { supabase } from "../supabase";
import { onMounted, ref, toRefs } from "vue";
import { useUserStore } from "../stores/user";
import Nav from "../components/Nav.vue";
import BlackButton from "../components/BlackButton.vue";
import Avatar from "../components/Avatar.vue";
import { useRouter } from "vue-router";
import { RESOLVE_FILTER } from "@vue/compiler-core";
import { storeToRefs } from "pinia";

const userStore = useUserStore();
const yourname = ref(null);
const avatar_url = ref(null);
const website = ref(null);
const loading = ref(false);
let email = ref(null);

onMounted(() => {
  getProfile();
});

async function getProfile() {
  await userStore.fetchUser();
  yourname.value = userStore.profile.yourname;
  avatar_url.value = userStore.profile.avatar_url;
  website.value = userStore.profile.website;
  email.value = userStore.user.email;
}

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

async function updateProfile() {
  try {
    loading.value = true;

    let { data, error } = await supabase
      .from("profiles")
      .update({
        yourname: yourname.value,
        website: website.value,
        avatar_url: avatar_url.value,
        updated_at: new Date(),
      })
      .match({ user_id: userStore.user.id });

    getProfile();

    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
  boolean.value = false;
}

//Toggle Boolean to open Edit Inputs
const boolean = ref(false);
const changeBoolean = () => {
  boolean.value = !boolean.value;
};
</script>

<style>
h4 img {
  width: 20px;
}
</style>
