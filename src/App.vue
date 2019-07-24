<template>
  <div id="app">
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <div class="container-fluid">
      <div class="row">
        <div class="col text-center">
          <input
            class="text-center"
            v-model="homeTeam"
            type="text"
            name="homeTeamText"
            @input="postData"
          >
        </div>
        <div class="col text-center">
          <input
            class="text-center"
            v-model="awayTeam"
            type="text"
            name="awayTeamText"
            @input="postData"
          >
        </div>
      </div>
      <div class="row">
        <div class="col text-center">
          <input
            class="text-center"
            v-model="homeScore"
            type="number"
            name="homeScoreSpinner"
            @input="postData"
          >
        </div>
        <div class="col text-center">
          <input
            class="text-center"
            v-model="awayScore"
            type="number"
            name="awayScoreSpinner"
            @input="postData"
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  // import HelloWorld from './components/HelloWorld.vue';
  import _ from 'lodash';
  const api = process.env.VUE_APP_SERVER_API;
  const interval = 2000;

  export default {
    name: 'app',
    // components: {
    //   HelloWorld,
    // },
    data() {
      return {
        homeTeam: '',
        awayTeam: '',
        homeScore: 0,
        awayScore: 0,
      };
    },
    created() {
      this.getData();
    },
    methods: {
      getData() {
        fetch(api)
          .then(response => response.json())
          .then((data) => {
            console.log(data);
            this.homeTeam = data.homeTeam;
            this.awayTeam = data.awayTeam;
            this.homeScore = data.homeScore;
            this.awayScore = data.awayScore;
          });
      },
      postData: _.debounce(function post() {
        const data = {
          homeTeam: this.homeTeam,
          awayTeam: this.awayTeam,
          homeScore: this.homeScore,
          awayScore: this.awayScore,
        };
        fetch(api, {
            method: 'POST',
            mode: 'cors',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(data),
          })
          .then(res => console.log(res));
      }, interval)
    }
  };
</script>

<style>
  @import './assets/bootstrap.min.css';

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    font-size: 2em;
  }

  input[type='number'] {
    margin-top: 10px;
    font-size: 3em;
    max-width: 2em;
  }

  input[type='number']::-webkit-inner-spin-button,
  input[type='number']::-webkit-outer-spin-button {
    opacity: 1;
  }
</style>
