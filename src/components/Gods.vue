<template>
  <div id="gods">
    <div class="content">
      <div> {{ dataInfo }}</div>
      <div><img class="cursed-image" src="../assets/cursed.png" alt="cursed" v-show="cursed"/></div>
      <table class="info">
        <tr>
        <td class="main-info">
          <p>Имя: {{ nameP }}</p>
          <p>Род деятельности: {{ job }}</p>
          <p>Возраст: {{ age }}</p>
          <p>Болезнь: {{ disease }}</p>
        </td>
        </tr>
        <tr>
        <td class="add-info" v-if="true">{{ addInfo }}</td>
        </tr>
      </table>
      <div class="buttons-layer">
        <button class="heal" v-on:click="heal">Исцелить</button>
        <button class="take" v-on:click="death">З̶̯͒̚а̴̼̬̑͛б̷̛͙͒р̸͈͕͛̕а̶̧̪̽̓т̸̛̤͖́ь̶̺́͑ ̷̱͝д̶͖͋̊у̴̛̤ш̴̣̈́̌у̸͎͖̒ </button>
      </div>
    </div>
  </div>

</template>

<script>

import axios from "core-js/internals/queue";
//axios.defaults.headers.common['Authorization'] = 'Basic Z29kOmdvZF9wYXNz' // for all requests

export default {
  name: "GodsVi",
  el: '#gods',
  mounted: function () {
    // Attach event listener to the root vue element
    this.$el.addEventListener('click', this.onClick);
  },
  beforeUnmount: function () {
    this.$el.removeEventListener('click', this.onClick)
  },
  data() {
    return {
      addInfo: 'О великие боги, пощадите этого смертного Акакия, болеющего недугом собачья хворь',
      nameP: 'Акакий Акакиевич Бронский',
      job: 'плотник',
      age: 42,
      disease: 'собачья хворь',
      clickCount:0,
      cursed:false,
      dataInfo: null
    }
  },
  methods: {
    heal: function () {
      //java method
      this.clickCount=0
      this.cursed=false
      this.getData('http://localhost:8080/api/v1/god/god/prayers/unanswered/last')
    },
    death:function () {
      //other method
      this.clickCount=0
      this.cursed=false
      this.getData('http://localhost:8080/api/v1/god/god/prayers/unanswered/last')
    },
    onClick: function () {
      this.clickCount+=1;
      if (this.cursed === true){
        this.cursed=false;
      }
      if (this.clickCount === 13){
        this.clickCount=0;
        this.cursed=true;
      }
    },
    getData: async function (url){
      axios
          .get(url, {},{
            auth: {
              username: 'god',
              password: 'god_pass'
            }
          })
          .then(response => {this.dataInfo = response})
          .catch(error => {this.dataInfo = error})
    }

  }
}
</script>

<style scoped>
  @import '../assets/styles/gods/index.css';
</style>