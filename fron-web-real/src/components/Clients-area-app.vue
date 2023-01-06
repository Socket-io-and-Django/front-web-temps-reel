
<script setup>
import ClientApp from "@/components/Client-app.vue";
import {defineEmits, defineProps} from "vue";
const props = defineProps({
  clients: Array
})
const emit = defineEmits(['accept-to-chat']);

</script>

<template>
  <div>
    <ul>
      <div v-for="client in props.clients" v-bind:key="client.id">
        <il>
          <client-app
              v-bind:client="client"
              v-on:accept-to-chat="emit('accept-to-chat', client.id)"
          />
        </il>
      </div>
    </ul>
  </div>
</template>

<style scoped>

</style>