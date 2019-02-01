<template>
  <v-container>
    <v-layout
      wrap
    >
      <h1 class="display-2 font-weight-bold mb-3">
        test socket
        <v-chip dark :color="connected ? 'success' : 'error'">{{ connected ? 'connected' : 'not connected' }}</v-chip>
      </h1>
    </v-layout>
    <v-layout>
      <v-text-field
        v-model="url"
        label="URL"
      />
      <v-btn color="info" @click="connect">connect</v-btn>
    </v-layout>
    <v-divider></v-divider>
    <v-layout>
      <v-text-field
        v-model="topic"
        label="Subscribe topic"
      />
      <v-btn @click="subs" color="info">subscribe</v-btn>
    </v-layout>
    <v-layout>
      <v-card color="grey" full-width>
        <v-card-text>
          <p v-for="(d, index) in data" :key="index">{{ d }}</p>
        </v-card-text>
      </v-card>
    </v-layout>
    <v-divider></v-divider>
  </v-container>
</template>

<script>
import io from 'socket.io-client'

let socket

export default {
  data: () => ({
    url: 'localhost:4000',
    topic: 'blood',
    connected: false,
    data: []
  }),

  methods: {
    connect () {
      socket = io(this.url)
      console.log('connecting...')

      socket.on('connect', () => {
        console.log('terhubung socket', socket)
        this.connected = true
      })

      socket.on('disconnect', () => {
        console.error('socket disconnected')
        this.connected = false
      })
      socket.on('reconnect_failed', () => {
        console.error('reconnect failed')
        this.connected = false
      });
    },
    subs () {
      socket.on(this.topic, (data) => {
        console.log('subscribed', data)
        this.data.unshift(data)
      })
    },
  }
}
</script>

<style>

</style>
