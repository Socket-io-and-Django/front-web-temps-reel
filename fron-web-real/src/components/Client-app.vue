<script setup>

import {defineEmits, defineProps} from "vue";
import ButtonApp from "@/components/Button-app.vue";

const emit = defineEmits(['accept-to-chat']);

const props = defineProps({
  client: Object
})
</script>

<template>
  <div>
    <h3>
      {{props.client.username}}
    </h3>
    <p>{{props.client.id}}</p>
  </div>
  <button-app
      :text="'accept Chat'"
      v-on:btn-click="emit('accept-to-chat', props.client.id)"
  />
</template>

<style scoped>

</style>