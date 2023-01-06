
<script setup>
import MessageApp from "@/components/Message-app.vue";
import {defineProps} from "vue";
const props = defineProps({
  messages: Array
})
</script>

<template>
  <section v-for="message in props.messages" v-bind:key="message.time">
    <MessageApp
        v-bind:message="message" />
  </section>
</template>

<style scoped>

*, *::before, *::after {
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

#container {
  display: flex;
  flex-direction: column;
  height: 100vmin;
}

</style>