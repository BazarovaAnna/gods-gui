<template>
  <div id="gods">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">Перевернуть сообщение</button>
  </div>
</template>

<script>
export default {
  name: "GodsVi",
  el: '#gods',
  data() {
    return {
      message: 'Привет, Vue.js!'
    }
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
}
</script>

<style scoped>

</style>