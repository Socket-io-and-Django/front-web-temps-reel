<template>
  <div class="home">
    <RegisterForm v-on:add-new-user="addUser"/>
  </div>
</template>

<script setup>
// @ is an alias to /src
import RegisterForm from "@/components/RegisterForm.vue";

async function addUser(newUser) {
  const res = await fetch('http://localhost:3000/register', {
    method: 'POST',
    headers: {
      'Content-type': 'application/json',
    },
    credentials: 'include',
    body: JSON.stringify(newUser)
  })
  const data = await res.json()
  console.log("data", data);

}

</script>
