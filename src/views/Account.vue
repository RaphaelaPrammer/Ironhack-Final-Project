<template>
  <Nav />

  <h1>hello {{ yourname }}</h1>

  <h2>Your Account:</h2>
  <!-- <h2>User Name: {{ userStore.profile.username }}</h2> -->
  <h2>Email: {{ userStore.user.email }}</h2>
  <h2>Your Website: {{ website }}</h2>

  <img
    :src="
      avatar_url
        ? avatar_url
        : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
    "
    alt="Profile picture"
  />

  <h2>Edit your Profile</h2>
  <!-- <div>
    <label for="username">Username</label>
    <input id="username" type="text" v-model="username" />
  </div> -->
  <div>
    <label for="yourname">Your Name</label>
    <input id="yourname" type="text" v-model="yourname" />
  </div>
  <div>
    <label for="website">Website</label>
    <input id="website" type="website" v-model="website" />
  </div>

  <BlackButton @logOut="updateProfile">Update Profile</BlackButton>
  <br />
  <BlackButton @logOut="signOut">Log Out</BlackButton>
</template>

<script setup>
import { supabase } from "../supabase";
import { onMounted, ref, toRefs } from "vue";
import { useUserStore } from "../stores/user";
import Nav from "../components/Nav.vue";
import BlackButton from "../components/BlackButton.vue";

const userStore = useUserStore();

// const username = ref("");
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
  email = userStore.user.email;
}

async function signOut() {
  try {
    loading.value = true;
    let { error } = await supabase.auth.signOut();
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}

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
}
</script>

<style>
img {
  width: 200px;
  border-radius: 50%;
}
</style>
