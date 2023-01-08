<script setup>

import {defineProps} from "vue";

const props = defineProps({
  message: Object
})
</script>

<template>
  <div id="message-area">
    <div id="sender">
      {{props.message.username}}
    </div>
    <div id="time">
        {{props.message.time}}
    </div>
    <div id="text">
      {{props.message.text}}
    </div>
  </div>
</template>

<style scoped>

#message-area{
  display: grid;
  gap: 1vmin;
  grid-template-columns: 3fr 1fr;
}

#sender{
  text-align: left;
}
#time{
  font-size: 1vmin;
  text-align: right;
  justify-self: end;
  align-self: start;
}
#text{

  grid-column: 1/3;
  align-self: end;
}

</style>