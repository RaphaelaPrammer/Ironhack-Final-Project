<template>
  <div class="new-tasks-container">
    <h1>Add a new Task</h1>
    <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
    </div>
    <div class="add-task-container">
      <div class="input-field">
        <input
          type="text"
          placeholder="Add a Task Title - Go to the Park and Play"
          v-model="name"
        />
      </div>
      <div class="input-field">
        <input
          type="text"
          placeholder="Add a Task Description - Bring the ball, cookies and a stick."
          v-model="description"
        />
      </div>
    </div>
    <div class="radio-buttons-container">
      <div class="family">
        <input
          type="radio"
          id="family"
          name="category"
          value="family"
          v-model="category"
        />
        <label for="family"> Family</label>
      </div>
      <div class="business">
        <input
          type="radio"
          id="business"
          name="category"
          value="business"
          v-model="category"
        />
        <label for="business"> Business</label>
      </div>
      <div class="leisure">
        <input
          type="radio"
          id="leisure"
          name="category"
          value="leisure"
          v-model="category"
        />
        <label for="leisure"> Leisure</label>
      </div>
      <div class="other">
        <input
          checked
          type="radio"
          id="other"
          name="category"
          value="other"
          v-model="category"
        />
        <label for="other"> Other</label>
      </div>
    </div>
    <BlackButton @logOut="addTask">Add</BlackButton>
    <!-- <button @click="addTask" class="button">Add</button> -->
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import BlackButton from "./BlackButton.vue";
const taskStore = useTaskStore();
const emit = defineEmits(["getTasksHijo"]);

// variables para los valors de los inputs
const name = ref("");
const description = ref("");
const category = ref("");

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);

// Arrow function para crear tareas.
const addTask = async () => {
  if (name.value.length === 0) {
    // Primero comprobamos que ningún campo del input esté vacío y lanzamos el error con un timeout para informar al user.

    showErrorMessage.value = true;
    errorMessage.value = "The task title or description is empty";
    setTimeout(() => {
      showErrorMessage.value = false;
    }, 5000);
  } else {
    // Aquí mandamos los valores a la store para crear la nueva Task. Esta parte de la función tenéis que refactorizarla para que funcione con emit y el addTask del store se llame desde Home.vue.

    await taskStore.addTask(name.value, description.value, category.value);
    name.value = "";
    description.value = "";
    emit("getTasksHijo");
  }
};
</script>

<style></style>
