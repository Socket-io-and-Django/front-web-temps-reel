<script setup>

import { defineProps, defineEmits } from "vue";

const emit = defineEmits(['btn-click']);

function onClick() {
  emit('btn-click');

}
const props = defineProps({
  text: String,
  color: String
})

</script>

<template>
  <button v-on:click="onClick" class="btn" v-bind:style="{background: props.color }">{{props.text}}</button>
</template>

<style scoped>

</style>