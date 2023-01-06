
<script setup>
import ConsultantApp from "@/components/Consultant-app.vue";
import {defineEmits, defineProps} from "vue";
const props = defineProps({
  consultants: Array
})
const emit = defineEmits(['ask-to-chat']);

</script>

<template>
    <ul>
      <div
          v-for="consultant in props.consultants" v-bind:key="consultant.id">
        <il>
          <ConsultantApp
              v-bind:consultant="consultant"
              v-on:ask-to-chat="emit('ask-to-chat', consultant.id)"
          />
        </il>
      </div>
    </ul>
</template>

<style scoped>

</style>