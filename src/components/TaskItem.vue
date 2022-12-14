<template>
  <div
    class="container-task"
    v-bind:class="[
      props.task.is_complete ? 'task-complete' : 'task-not-complete',
    ]"
  >
    <div>
      <div class="top-status">
        <!-- <div
          v-bind:class="
            props.task.is_complete ? 'btn-complete' : 'btn-not-complete'
          "
        ></div> -->
        <h5
          class="top-category"
          :class="
            props.task.category === 'family'
              ? 'task-family'
              : props.task.category === 'business'
              ? 'task-business'
              : props.task.category === 'leisure'
              ? 'task-leisure'
              : 'task-other'
          "
        >
          {{ task.category }}
        </h5>
      </div>

      <h3>
        {{ task.title }}
      </h3>
      <p>
        {{ task.description }}
      </p>
    </div>
    <!-- BUTTONS  -->
    <div class="container-buttons">
      <!-- Button change status  -->
      <button
        title="Task not completed"
        class="btn-not-complete"
        v-if="props.task.is_complete"
        @click="changeStatus"
      ></button>
      <button
        title="task completed"
        class="btn-complete"
        v-else
        @click="changeStatus"
      ></button>

      <!-- EDIT Button  -->
      <button
        title="edit task"
        class="btn-edit"
        @click="changeBooleanFunction"
      ></button>

      <!-- DELETE BUTTON  -->
      <button
        title="delete Task"
        class="btn-delete"
        @click="deleteTask"
      ></button>

      <!-- EDIT INPUT FIELDS  -->

      <div class="edit-input-fields" v-show="changeBoolean">
        <input type="text" placeholder="Change Title" v-model="name" />
        <input
          placeholder="change Description"
          type="text"
          v-model="description"
        />

        <!-- Edit Category  -->

        <div class="edit-category">
          <div>
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
          </div>
          <div>
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
                type="radio"
                id="other"
                name="category"
                value="other"
                v-model="category"
              />
              <label for="other"> Other</label>
            </div>
          </div>
        </div>

        <BlackButton @logOut="changeTask">Save</BlackButton>
        <!-- <button class="btn-edit-complete" @click="changeTask">Save</button> -->
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";
import BlackButton from "./BlackButton.vue";

const taskStore = useTaskStore();
const emit = defineEmits(["getTasksHijo"]);

const name = ref(props.task.title);
const description = ref(props.task.description);
const category = ref(props.task.category);

const props = defineProps({
  task: Object,
});

//-----------------------------------------------------
// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.

const deleteTask = async () => {
  await taskStore.deleteTask(props.task.id);
  emit("getTasksHijo");
};
//----------------------------------------------------------
//Functioin para cambiar task

const changeBoolean = ref(false);
const changeBooleanFunction = () => {
  changeBoolean.value = !changeBoolean.value;
};
const changeTask = async () => {
  await taskStore.changeTask(
    name.value,
    description.value,
    category.value,
    props.task.id
  );
  changeBoolean.value = false;
  emit("getTasksHijo");
};

//------------------------------------------------------------
// Function for Completion Tasks----------

const changeStatus = async () => {
  await taskStore.changeStatus(props.task.id, !props.task.is_complete);

  emit("getTasksHijo");
};
</script>

<style scoped>
.top-category {
  margin-top: 7px;
  margin-bottom: 7px;
  padding: 5px;
  border-radius: 5px;
}
.task-family {
  background: #7a4b94b0;
}
.task-business {
  background: #7d82b8c2;
}
.task-leisure {
  background: #b7e3ccb6;
}
.task-other {
  background: #ffcf56ab;
}
</style>

<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.
-->
