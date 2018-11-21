<template>
  <div id="app">
    <Autocomplete :items="customers" filterby="name" title="" @selected="customerSelected" @change="onChange"/>
  </div>
</template>

<script>
import Autocomplete from './components/Autocomplete.vue'

import axios from 'axios';
export default {
  name: 'app',
  mounted(){
    axios.get('https://jsonplaceholder.typicode.com/users')
      .then((response) => {
        this.customers = response.data;
      })
  },
  data(){
    return{
      customers : []
    }
  },
  methods:{
    customerSelected(customer){
      console.log(customer);
    },
    onChange(value){
      console.log(value);
    }
  },
  components: {
    Autocomplete
  },

}
</script>

<style>
#app {
  margin: 0px auto;
  margin-top: 60px;
  width: 400px;
}
</style>
