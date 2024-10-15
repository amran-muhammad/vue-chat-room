<template>
  <div>
    <h2>Chat</h2>
    <div>
      <input v-model="username" placeholder="Enter your name" />
      <button @click="register">Join Chat</button>
    </div>
    <ul>
      <li v-for="(msg, index) in messages" :key="index">
        <strong>{{ msg.user }}:</strong> {{ msg.message }}
      </li>
    </ul>
    <input v-model="message" @keyup.enter="sendMessage" placeholder="Type your message..." />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<script>
import { io } from 'socket.io-client';

export default {
  data() {
    return {
      socket: null,
      username: '',
      message: '',
      messages: []
    };
  },
  created() {
    // this.socket = io('http://localhost:3000');
    this.socket = io('https://nodejs-chat-room.vercel.app:3000');
    this.socket.on('chat message', (data) => {
      this.messages.push(data);
    });
  },
  methods: {
    register() {
      if (this.username.trim()) {
        this.socket.emit('register', this.username);
      }
    },
    sendMessage() {
      if (this.message.trim()) {
        this.socket.emit('chat message', this.message);
        this.message = '';
      }
    }
  }
};
</script>

<style>
/* Add your styles here */
</style>