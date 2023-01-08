
<script setup>
import MessageApp from "@/components/Message-app.vue";
import {defineProps} from "vue";
const props = defineProps({
  messages: Array
})
</script>

<template>
  <div
      class="messages-list">
    <div
      v-for="message in props.messages" v-bind:key="message.time">
    <MessageApp
        v-bind:message="message" />
    </div>
  </div>
</template>

<style scoped>

*, *::before, *::after {
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

.messages-list {
  /*border: 0.5vmin solid darkorange;*/

}

</style>