<script setup>

import {defineProps} from "vue";

const props = defineProps({
  message: Object
})
</script>

<template>
  <div>
    <h3>
      {{props.message.username}}
    </h3>
    <p>{{props.message.text}}</p>
  </div>
  <div>
    {{props.message.time}}
  </div>
</template>

<style scoped>

</style>