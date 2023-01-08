
<script setup>
import ConsultantApp from "@/components/Consultant-app.vue";
import {defineEmits, defineProps} from "vue";
const props = defineProps({
  consultants: Array
})
const emit = defineEmits(['ask-to-chat']);

</script>

<template>
    <div>Available Consultant</div>
      <div
          v-for="consultant in props.consultants" v-bind:key="consultant.id">

          <ConsultantApp
              v-bind:consultant="consultant"
              v-on:ask-to-chat="emit('ask-to-chat', consultant.id)"
          />
      </div>
</template>

<style scoped>

</style>