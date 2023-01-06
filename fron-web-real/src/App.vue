<script setup>
import {ref} from "vue";
import ButtonApp from "@/components/Button-app.vue";
import HeaderApp from "@/components/Header-app.vue";
const token = {
  accessToken: ref(""),
  username:ref("")
};

async function Logout() {
  const res = await fetch('http://localhost:3000/logout', {
    method: 'POST',
    headers: {
      'Content-type': 'application/json',
    },
    credentials: 'include',
    body: JSON.stringify({accessToken: token.accessToken.value})
  })

  const data = await res.json();
  if('accessToken' in data) token.accessToken.value = data.accessToken;
}
function userLoggedIn(token){
  token.accessToken.value = token.accessToken;
  token.username.value = token.username;
  console.log("app userLoggedIn: ", token)
}
</script>

<template>
  <HeaderApp v-if="token.accessToken.value !==''"
      :username="token.username.value"/>
  <router-link

      v-on:user-logged-in="userLoggedIn"
      to="/">Home</router-link> |
  <router-link to="/about">About</router-link>
  <router-view
      :username="token.username"
      :token="token.accessToken"
  ></router-view>
  <ButtonApp v-show="token.accessToken.value !==''"
      :text="'LogOut'"
  v-on:btn-click="Logout"/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
