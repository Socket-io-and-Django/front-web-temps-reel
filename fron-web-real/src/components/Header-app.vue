<script setup>
import {defineProps} from "vue";
const props = defineProps({
  username: String
})

//
// const homePage = computed(() => {
//   if(route.path === '/') {
//     return true;
//   }
//   else {
//     return false;
//   }
// })
</script>

<template>
  <header>
    <h2>Welcome {{props.username}}</h2>
  </header>
</template>


<style scoped>
a {
  color: #333333;
}
footer {
  margin-top: 30px;
  text-align: center;
}
</style>