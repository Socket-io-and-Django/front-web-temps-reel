<script setup>
import { io } from 'socket.io-client'
import { onUpdated, ref, watch, watchEffect } from 'vue'

const socket = io("ws://localhost:3000")
const nickname = ref('')
const showNickNameForm = ref(true)
const chatText = ref('')
const messages = ref([])
const connectedToChat = ref(false)
const inputType = ref('text')
const error = ref('')
let askedInfoType = ''
let vehiculeInfo = {}
const serverTextColor = 'blue'
const chatContainer = ref(null)

const emitToServer = (emitType, value) => {
    socket.emit(emitType, value)
    messages.value.push({
        from: 'user',
        txt: chatText.value
    })
    chatText.value = ''
    error.value = ''
}

const handleChatForm = () => {
    if (chatText) {
        // socket.emit('chat_message', chatText.value)
        // chatText.value = ''
        if (inputType.value === 'number') {
            let parsedValue = parseInt(chatText.value)
            if (askedInfoType === 'help_choice') {
                if ([1, 2, 3, 4].includes(parsedValue)) {
                    // socket.emit('send_help_type', parsedValue)
                    // messages.value.push({
                    //     from: 'user',
                    //     txt: chatText.value
                    // })
                    // chatText.value = ''
                    // error.value = ''
                    emitToServer('send_help_type', parsedValue)
                } else {
                    error.value = 'Réponse pas entre 1 et 4'
                }
            } else if (askedInfoType === 'year_choice') {
                console.log(new Date().getFullYear());
                if (parsedValue >= 1901 && parsedValue <= new Date().getFullYear()) {
                    emitToServer('send_vehicule_year', parsedValue)
                } else {
                    error.value = 'Année non valide'
                }
            } else if (askedInfoType === 'km_since_maintenance_date') {
                if (parsedValue > 0 && parsedValue) {
                    emitToServer('send_km_since_maintenance_date', parsedValue)
                } else {
                    error.value = 'Nombre de kilomètres non valide'
                }
            } else if (askedInfoType === 'do_revision') {
                console.log('ici');
                if ([1, 2].includes(parsedValue)) {
                    emitToServer('send_do_revision', parsedValue)
                } else {
                    error.value = 'Réponse différent de 1 ou 2'
                }
            }
        } else if (inputType.value === 'date') {
            let parsedValue = new Date(chatText.value)
            if (askedInfoType === 'maintenance_date') {
                if (parsedValue.getFullYear() >= vehiculeInfo.vehiculeYear && parsedValue <= new Date()) {
                    emitToServer('send_last_maintenance_date', parsedValue)
                } else {
                    error.value = 'Date non valide'
                }
            }
        }
    }
}

const stopChat = () => {
    messages.value = []
    inputType.value = 'number'
    askedInfoType = 'help_choice'
    chatText.value = ''
    error.value = ''
    socket.emit('reset_bot')
}

// watch((chatText) => {
//     if (inputType === 'number') {

//     }
// })
watch(messages, () => {
    console.log(chatContainer.value.scrollHeight);
    // chatContainer.value.scrollTop = chatContainer.value.scrollIntoView(false)
    console.log('ici');
}, { deep: true })

socket.on('reset_chat', () => {
    stopChat(socket)
})

socket.on('ask_help_type', (res) => {
    console.log(res.txt);
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    inputType.value = 'number'
    askedInfoType = 'help_choice'
})

socket.on('ask_vehicule_year', (res) => {
    console.log(res.txt);
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    inputType.value = 'number'
    askedInfoType = 'year_choice'
})

socket.on('ask_last_maintenance_date', (res) => {
    console.log(res.txt);
    vehiculeInfo = res.vehiculeInfo
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    inputType.value = 'date'
    askedInfoType = 'maintenance_date'
})

socket.on('ask_km_since_last_maintenance', (res) => {
    console.log(res.txt);
    vehiculeInfo = res.vehiculeInfo
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    inputType.value = 'number'
    askedInfoType = 'km_since_maintenance_date'
})

socket.on('ask_do_revision', (res) => {
    console.log(res.txt);
    vehiculeInfo = res.vehiculeInfo
    messages.value.push({
        from: res.from,
        txt: res.txt
    })
    inputType.value = 'number'
    askedInfoType = 'do_revision'
})
</script>

<template>
    <h2>Chatbot</h2>
    <div class="chat-container" ref="chatContainer">
        <ul id="messages">
            <li v-for="msg in messages" :key="msg"
                :style="[msg.from === 'server' ? { color: serverTextColor } : { color: 'black' }, msg.from === 'user' ? { textAlign: 'end' } : { textAlign: 'start' }]">
                <pre>{{ `${msg.txt}` }}</pre>
            </li>
        </ul>
        <span class="error" v-if="error">{{ error }}</span>
        <button id="stopButton" @click="stopChat">Arrêter</button>
        <form @submit.prevent="handleChatForm" id="chatForm">
            <input :type="inputType" required v-model="chatText" />
            <button>Envoyer</button>
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
    display: flex;
    flex-direction: column;
    max-height: 300px;
    overflow-y: scroll;
}

#chatForm {
    display: flex;
    flex-wrap: wrap;
}

pre {
    white-space: pre-wrap;
}

#chatForm>input {
    max-width: 100%;
    flex: 1;
}

#messages>li {
    padding: 0.5rem 1rem;
}

.error {
    color: red;
    text-align: center;
}

#stopButton {
    align-self: end;
    margin-bottom: 2px;
    padding: 0 10px;
}
</style>