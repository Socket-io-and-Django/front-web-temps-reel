<script setup>
import { io } from 'socket.io-client'
import { onUpdated, ref, watch, watchEffect } from 'vue'

const socket = io("ws://localhost:3000")
const nickname = ref('')
const showNickNameForm = ref(true)
const chatText = ref('')
const messages = ref([])
const connectedToChat = ref(false)

const handleNickameForm = () => {
    if (nickname) {
        socket.emit('send_nickname', nickname.value)
        showNickNameForm.value = false
    }
}
const handleChatForm = () => {
    if (chatText) {
        socket.emit('chat_message', chatText.value)
        chatText.value = ''
    }
}

socket.on('welcome_message', (res) => {
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    connectedToChat.value = true
})
watch(connectedToChat, () => {
    console.log(connectedToChat.value);
    if (connectedToChat.value) {
        socket.on('chat_message', (res) => {
            messages.value.push({
                from: res.from,
                txt: res.txt
            })
        })

        socket.on('new_user_connection', (res) => {
            messages.value.push({
                from: res.from,
                txt: res.txt
            })
        })
    }
})
// onUpdated(() => {
//     console.log(nickname.value);
// })
</script>

<template>
    <h2>Chat component</h2>
    <form @submit.prevent="handleNickameForm" v-if="showNickNameForm">
        <label>Nickname : </label>
        <input type="text" required v-model="nickname">
        <button>Submit nickname</button>
    </form>
    <div v-else class="chat-container">
        <ul id="messages">
            <li v-for="msg in messages" :key="msg"
                :style="[msg.from === 'server' ? { color: 'red' } : { color: 'black' }, msg.from === nickname ? { textAlign: 'end' } : { textAlign: 'start' }]">
                {{ `${msg.from} : ${msg.txt}` }}
            </li>
        </ul>
        <form @submit.prevent="handleChatForm" id="chatForm">
            <input type="text" required v-model="chatText" />
            <button>Send</button>
        </form>
    </div>
</template>

<style scoped>
#messages {
    list-style-type: none;
    margin: 0;
    padding: 0;
}

.chat-container {
    border: 2px solid;
}
#chatForm {
    display: flex;
    flex-wrap: wrap;
}
#chatForm>input {
    max-width: 100%;
    flex: 1;
}
#messages>li {
    padding: 0.5rem 1rem;
}
</style>