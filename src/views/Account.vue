<template>
  <Nav />

  <h1>Hello {{ yourname }} !</h1>

  <h2>Your Account:</h2>
  <!-- <h2>User Name: {{ userStore.profile.username }}</h2> -->

  <div class="container-account">
    <img
      :src="
        avatar_url
          ? avatar_url
          : 'https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/WelshCorgi.jpeg/300px-WelshCorgi.jpeg'
      "
      alt="Profile picture"
      class="account-avatar-img"
    />
    <div>
      <h4>
        <img src="../assets/icons/scoobydoo.png" alt="" />{{
          userStore.user.email
        }}
      </h4>
      <h4><img src="../assets/icons/dog-robot.png" alt="" />{{ website }}</h4>
      <br />
      <BlackButton @logOut="signOut">Log Out</BlackButton>
    </div>
    <div>
      <BlackButton @click="changeBoolean">Edit Profile</BlackButton>
      <div v-show="boolean">
        <!-- <div>
    <label for="username">Username</label>
    <input id="username" type="text" v-model.lazy="username" />
  </div> -->
        <div class="edit-profile-container">
          <div>
            <label for="yourname">Your Name</label><br />
            <input id="yourname" type="text" v-model.lazy="yourname" />
          </div>

          <div>
            <label for="website">Website</label><br />
            <input id="website" type="website" v-model.lazy="website" />
          </div>

          <div><label for="avatar">Update Profile Pic</label><br /></div>
        </div>
        <BlackButton @logOut="updateProfile">Update </BlackButton>
      </div>
    </div>
  </div>
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
  email.value = userStore.user.email;
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
