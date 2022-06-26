<template>

  <div>
    <h2>{{ title }}</h2>
    <ul v>
      <name v-for="person of names" :output="person.name + ' ' + person.surname"></name>
    </ul>
    <input v-model="inputData" type="text">
    <button @click="onButtonClicK">Add name</button>
    <p v-if="inputData == 'Алёша'">Ну ты и Алёша</p>
    <br>
    <br>
    <button @click="activeTab=1">1</button>
    <button @click="activeTab=2">2</button>
    <button @click="activeTab=3">3</button>
    <br>
    <p v-show="activeTab==1">Ну ты и Алёша</p>
    <p v-show="activeTab==2">Ну ты и Петя</p>
    <p v-show="activeTab==3">Ну ты и Вася</p>
    <br>
    <br>
    <button @click="loadData">Load data</button>

  </div>

</template>

<script>
// http://localhost/api/v3/get-names.php
import Name from '@/components/name'
import axios from 'axios'
export default {
  name: "CustomView",
  components: {
    Name,
  },
  data(){
    return {
      names: [

      ],
      title: "Names:",
      inputData: '',
      activeTab: 0,
    }
  },
  mounted() {
    this.loadData();
  },
  methods: {
    onButtonClicK() {
      this.names.push(this.inputData);
      this.inputData = '';
    },
    loadData(){
      axios.get('http://localhost/api/v3/get-names.php')
          .then(res => {
            this.names = res.data
          })
          .catch(e => console.error(e))
    },
  }
}
</script>

<style scoped>

</style>