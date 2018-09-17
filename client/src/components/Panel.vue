<template>
  <div class="card mt-3">
    <div class="card-body">
      <div class="card-title">
        <h3>Leilão</h3>
        <hr>
      </div>
      <div class="card-body">
        <div class="messages" v-for="(val, index) in values" :key="index">
          <p>
            <span class="font-weight-bold">{{ val.user }}: </span>{{ val.value }}
            <small class="font-italic float-right">{{val.date|dateFormat('DD.MM.YYYY')}}</small>
          </p>
        </div>
      </div>
    </div>
    <div class="card-footer">
      <form @submit.prevent="sendMessage">
        <div class="gorm-group">
          <label for="user">Usuário:</label>
          <input type="text" :disabled="(user!==null&&values.length>0)" v-model="user" class="form-control">
        </div>
        <div class="gorm-group pb-3">
          <label for="value">Valor:</label>
          <input type="text" v-money="money" v-model="value" class="form-control">
        </div>
        <button type="submit" :disabled="(!user||!value)" class="btn btn-success">Enviar</button>
      </form>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client'
import { VMoney } from 'v-money'

export default {
  data() {
    return {
      socket : io('localhost:3000'),
      date: null,
      user: null,
      value: null,
      values: [],
      money: {
        decimal: ',',
        thousands: '.',
        prefix: 'R$ '
      },
    }
  },
  methods: {
    sendMessage(e) {
      this.socket.emit('SEND_MESSAGE', {
        user: this.user,
        value: this.value,
        date: new Date().toLocaleTimeString()
      })

      this.value = null
      this.date = null
    }
  },
  mounted() {
    this.socket.on('MESSAGE', (data) => {
      this.values = [...this.values, data]
    })
  },
  directives: {
    money: VMoney
  }
}
</script>

<style>
</style>