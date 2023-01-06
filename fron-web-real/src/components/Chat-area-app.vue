<script setup>
// @ is an alias to /src
// import RegisterForm from "@/components/Register-form-app.vue";
// import LogInApp from "@/components/LogIn-app.vue";
import ButtonApp from "@/components/Button-app.vue";
import {defineProps, ref} from "vue";
import  { io } from "socket.io-client";
import SendMessageApp from "@/components/Send-message-app.vue";
import MessagesAreaApp from "@/components/Messages-area-app.vue";
import ConsultantsAreaApp from "@/components/Consultants-area-app.vue";
import ClientsAreaApp from "@/components/Clients-area-app.vue";

const messages = ref([]);
const connected = ref(false);
const askToJoin = ref(false);
// const registered = ref(true);
const availableConsultants = ref([]);
const waitingList = ref([]);
const userRoles = [];
const props = defineProps({
  token: String,
})

function sendMessage(message) {
  socket.value.emit('chatMessage', message.text);
  // messages.value.push(message);
}
function askToChat(id) {
  socket.value.emit('askToChat', id);
  askToJoin.value=true;
  // messages.value.push(message);
}

// function waitingList(id) {
//   socket.value.emit('waitingList');
//   askToJoin.value=true;
//   // messages.value.push(message);
// }

function acceptToChat(id) {
  socket.value.emit('acceptToChat', id);
  console.log("acceptToChat")
  // askToJoin.value=true;
  // messages.value.push(message);
}
const socket = ref({});
function connectToSocket(token) {
  // console.log("in connection front", token.accessToken)
  socket.value = io("ws://localhost:9000", {
    auth: {
      token: token
    }
  });
  socket.value.on("connect_error",(er) => {
    console.error("Oops! Authentication failed:", er.message, " , ", er.cause)
  })
  socket.value.on("connect", () => {
    connected.value = true;
  })
  socket.value.on("roles", (roles) => {
    if(roles.length !== 0){
      roles.forEach(role => {
        userRoles.push(role);
      })
    }
    console.log("roles: ", userRoles)
  })
  socket.value.on('availableConsultants', (available) => {
    console.log("availableConsultants  :", available)

    available.forEach( constant => {
      console.log("consultant", constant)
      availableConsultants.value.push(constant);
    })
    console.log("availableConsultants", availableConsultants.value)
  })
  // message from server
  socket.value.on('message', (message) => {
    messages.value.push(message);
  })

  socket.value.on('waitingList', (clientsWaitingList) => {

      if(clientsWaitingList) {
        clientsWaitingList.forEach( client => {
          waitingList.value.push(client);
        })
          console.log('waitingList client: ', waitingList.value)

    }
  })
}
</script>

<template>
  <div class="chat-area">
    <button-app
        v-show="!connected"
        v-on:btn-click="connectToSocket(props.token)"
        v-bind:text="'connect'"
        v-bind:color="'green'"
    />
    <div class="chat">
      <MessagesAreaApp
          v-if="connected"
          v-bind:messages="messages" />
      <SendMessageApp
          v-if="askToJoin || userRoles.includes(4242)"
          v-on:send-new-message="sendMessage"
          v-bind:messagesNumber="messages.length"
      />

    </div>
    <ConsultantsAreaApp
        v-if="!askToJoin && availableConsultants.length !== 0 && !userRoles.includes(4242)"
        v-bind:consultants="availableConsultants"
        v-on:ask-to-chat="askToChat"
    />
    <ClientsAreaApp
        v-if="waitingList.length !== 0 && userRoles.includes(4242)"
        v-bind:clients="waitingList"
        v-on:accept-to-chat="acceptToChat"

    />
<!--  <FooterApp />-->
  </div>
</template>

<style scoped>

*, *::before, *::after {
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

.chat-area {
  display: flex;
  /*flex-direction: column;*/
  flex-wrap: wrap;
  height: 80vmin;
}
.chat {
  display: flex;
  flex-direction: column;
  height: 70vmin;
}
.chat-area:first-child {
  margin-right: auto;
}
.chat-area:last-child {
  margin-left: auto;
}

.chat-control label {
  display: block;
}



/*.chat-area {*/
/*  display: flex;*/
/*  align-items: center;*/
/*  justify-content: space-between;*/
/*}*/

/*.chat-area label {*/
/*  flex: 1;*/
/*}*/

/*.chat-area input {*/
/*  flex: 2;*/
/*  height: 20px;*/
/*}*/
</style>
