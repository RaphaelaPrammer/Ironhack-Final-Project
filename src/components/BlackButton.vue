<template>
  <button @click="$emit('logOut')" class="black-button"><slot /></button>
</template>

<script setup>
const emit = defineEmits(["logOut"]);
</script>
<style scoped>
.black-button {
  background-color: var(--colorBlack);
  box-shadow: var(--box-shadow-imgs);
  color: white;
  font-size: 1.2rem;
  font-weight: 600;
  border-radius: 10px;
  width: 70px;
  padding: 4px 0px;
  margin-bottom: 15px;
}
</style>
