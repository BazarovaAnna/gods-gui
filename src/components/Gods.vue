<template>
  <div id="gods">
    <div class="content">
      <div><img class="cursed-image" src="../assets/cursed.png" alt="cursed" v-show="cursed"/></div>
      <table class="info">
        <tr>
        <td class="main-info">
          <p>Имя: {{ nameP }}</p>
          <p>Род деятельности: {{ job }}</p>
          <p>Болезнь: {{ disease }}</p>
        </td>
        </tr>
        <tr>
          <td class="add-info" v-if="true">{{ addInfo }}</td>
        </tr>
        <!--<tr>
          <td>{{dataInfo}}</td>
        </tr>-->
      </table>
      <div class="buttons-layer">
        <button class="heal" v-on:click="heal">Исцелить</button>
        <button class="take" v-on:click="death">З̶̯͒̚а̴̼̬̑͛б̷̛͙͒р̸͈͕͛̕а̶̧̪̽̓т̸̛̤͖́ь̶̺́͑ ̷̱͝д̶͖͋̊у̴̛̤ш̴̣̈́̌у̸͎͖̒ </button>
      </div>
    </div>
  </div>

</template>

<script>

import axios from "axios";

export default {
  name: "GodsVi",
  el: '#gods',
  mounted: function () {
    // Attach event listener to the root vue element
    this.$el.addEventListener('click', this.onClick);
    this.getData('http://localhost:8080/api/v1/god/god/prayers/unanswered/last');
    //this.dealWithData();
  },
  beforeUnmount: function () {
    this.$el.removeEventListener('click', this.onClick)
  },
  data() {
    return {
      addInfo: 'Нажмите любую кнопку, чтобы продолжить',
      nameP: 'Здесь будет имя смертного',
      job: 'Тут вы увидите его место в жизни',
      disease: 'Здесь будет указана хворь смертного',
      clickCount:0,
      cursed:false,
      dataInfo: "init",
      prayerID: null,
      endOfPrayers: false
    }
  },
  methods: {
    heal: function () {
      //java method
      console.log("healing method");
      this.clickCount=0;
      this.cursed=false;
      this.postData('http://localhost:8080/api/v1/god/god/prayers/'+this.prayerID, "accepted")
          .then(
            this.getData('http://localhost:8080/api/v1/god/god/prayers/unanswered/last')
        );

    },
    death:function () {
      //other method
      console.log("taking soul");
      this.clickCount=0;
      this.cursed=false;
      this.postData('http://localhost:8080/api/v1/god/god/prayers/'+this.prayerID, "rejected")
          .then(
            this.getData('http://localhost:8080/api/v1/god/god/prayers/unanswered/last')
        );
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
    dealWithData (){
      //Данные лежат в dataInfo и здесь парсятся куда надо
      if (this.dataInfo){
        let nam=""
        console.log(this.dataInfo);
        if (this.dataInfo['patient'].patientName)
          nam+=this.dataInfo['patient'].patientName;

        if (this.dataInfo['patient'].patientPatronymic)
          nam+=" "+this.dataInfo['patient'].patientPatronymic;

        if (this.dataInfo['patient'].patientSurname)
          nam+=" "+this.dataInfo['patient'].patientSurname;

        this.nameP=nam;
        this.job=this.dataInfo.patient.socialStatus;
        this.disease=this.dataInfo.diseaseName;
        this.addInfo=this.dataInfo.infoMsg;
        this.prayerID=this.dataInfo.prayerId;
      } else {
        this.addInfo='На сегодня молитвы закончились! Спасибо!';
        this.nameP= 'Здесь будет имя смертного'
        this.job= 'Тут вы увидите его место в жизни';
        this.disease= 'Здесь будет указана хворь смертного';
        this.endOfPrayers=true;
      }
    },
    async postData(url, status){
      if (this.endOfPrayers===false){
      console.log(url,status);
      if(this.prayerID){
        return axios
            .post(url,{
              "status": status
            },{
              headers: {
                "Content-Type": "application/json"
              },
              auth: {
                username: 'god',
                password: 'god_pass'
              }
            });
      }

      console.log("finished");
    }},
    getData (url){
      axios
          .get(url, {
            headers: {
              "Content-Type": "application/json"
            },
            auth: {
              username: 'god',
              password: 'god_pass'
            }} )
          .then(response => {
            this.dataInfo = response.data;
            console.log("in get", response.data);
            console.log(this.dataInfo);

            this.dealWithData();
          })
          .catch(error => {
            if (error.response) {
              // Request made and server responded
              console.log(error.response.data);
              console.log(error.response.status);
              console.log(error.response.headers);
            } else if (error.request) {
              // The request was made but no response was received
              console.log(error.request);
            } else {
              // Something happened in setting up the request that triggered an Error
              console.log('Error', error.message);
            }
          });
    }

  }
}
</script>

<style scoped>
  @import '../assets/styles/gods/index.css';
</style>