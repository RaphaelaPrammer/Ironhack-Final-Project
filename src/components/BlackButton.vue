<template>
  <button @click="$emit('logOut')" class="black-button"><slot /></button>
</template>

<script setup>
const emit = defineEmits(["logOut"]);
</script>
<style scoped>
.black-button {
  background-color: var(--colorBlack);
  box-shadow: var(--box-shadow-small);
  color: white;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 10px;
  border: none;
  width: 80px;
  padding: 4px 0px;
  /* margin-bottom: 15px; */
}
.black-button:hover {
  cursor: pointer;
  background: var(--colorMnBlue);
}
</style>
