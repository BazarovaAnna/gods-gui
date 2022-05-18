<template>
  <div id="gods">
    <p>{{ message }}</p>
    <button class="heal" v-on:click="reverseMessage">Исцелить</button>
    <button class="take" v-on:click="reverseMessage">З̶̯͒̚а̴̼̬̑͛б̷̛͙͒р̸͈͕͛̕а̶̧̪̽̓т̸̛̤͖́ь̶̺́͑ ̷̱͝д̶͖͋̊у̴̛̤ш̴̣̈́̌у̸͎͖̒ </button>
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
      //java method
      this.message = this.message.split('').reverse().join('')
    }
  }
}
</script>

<style scoped>
  @import '../assets/styles/gods/index.css';
</style>