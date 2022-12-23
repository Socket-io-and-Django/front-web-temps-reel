<script setup>
import { ref, defineEmits } from "vue";
const username = ref('');
const password = ref('');

const emit = defineEmits(['add-new-user']);

function onSubmit(e) {
  e.preventDefault();
  if(!(username.value && password.value)) {
    alert('Username and Password are required');
    return
  }
  const newUser = {
    username: username.value,
    password: password.value,
  }
  emit('add-new-user', newUser);
  username.value = '';
  password.value = '';
}

</script>

<template>
  <form v-on:submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Username</label>
      <input type="text" name="username" v-model="username" placeholder="Username" />
    </div>
    <div class="form-control">
      <label>Password</label>
      <input
          type="password"
          name="password"
          v-model="password"
          placeholder="Password" />
    </div>

    <input type="submit" value="Register" class="btn btn-block" />

  </form>
</template>

<style scoped>

</style>