<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="general-container">
    <div class="img"></div>
    <div class="container">
      <div class="header">
        <div class="header-description">
          <div class="header-img">
            <img
              src="https://media.istockphoto.com/id/1188848722/es/vector/icono-de-impresi%C3%B3n-de-pata-de-animal-impresi%C3%B3n-de-pata-de-perro-o-gato.jpg?s=612x612&w=0&k=20&c=SuPNtRgRFp8quBl2tSqB7Ddg1nvPcW1JkduXWI0LnwA="
              alt="header-img"
            />
          </div>
          <h3 class="header-title">Log In to ToDo App</h3>
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

          <button class="button" type="submit">Sign In</button>
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

      <div v-show="errorMsg">{{ errorMsg }}</div>
    </div>
  </div>
  <!-- <div class="container">
    <h3 class="header-title">Log In to ToDo App</h3>
    <p class="header-subtitle">Estamos en la ruta de login. Aquí deberíais crear un form con la lógica correspondiente para que este permita al usuario loguearse con su email y su contraseña. Miraros la lógica de SignUp si necesitáis inspiración :)</p>
    <p>Dont have an account? <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link"/></p>
  </div> -->
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
  } catch (error) {}
};
</script>

<style>
/* body {
  background-color: black;
} */
</style>
