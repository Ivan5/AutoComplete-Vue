<template lang="html">
  <div class="autocomplete">
    <div class="input" @click="toggleVisible">

    </div>
    <div class="popover" v-show="visible">
      <input type="text" v-model="query" placeholder="Start Typing..">
      <div class="options">
        <ul>
          <li v-for="(item,index) in matches" :key="item.id" v-text="item[filterby]" @click="itemClick(index)"> </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props:['items','filterby'],
  data(){
    return{
      visible:false,
      query:''
    }
  },
  methods:{
    toggleVisible(){
      this.visible = !this.visible
    },
    itemClick(index){
      console.log(this.items[index]);
    }
  },
  computed:{
    matches(){
      if(this.query == ''){
        return []
      }
      return this.items.filter((item) => item[this.filterby].toLowerCase().includes(this.query.toLowerCase()))
    }
  }
}
</script>

<style lang="css">
.autocomplete{
  width: 100%;
  position: relative;
}
.input{
  height: 40px;
  border-radius: 3px;
  border: 2px solid lightgray;
  box-shadow: 0 0 10px #eceaea;
  font-size: 25px;
  padding-left: 10px;
  padding-top: 10px;
  cursor: text;
}
.popover{
  min-height: 50px;
  border: 2px solid lightgray;
  position: absolute;
  top:47px;
  left: 0;
  right: 0;
  background: #fff;
  border-radius: 3px;
  text-align: center;
}

.popover input{
  width: 95%;
  margin-top: 5px;
  height: 40px;
  font-size: 16px;
  border-radius: 3px;
  border: 1px solid lightgray;
  padding-left: 8px;
}
.options{
  max-height: 150px;
  overflow-y: scroll;
  margin-top: 5px;
}
ul{
  list-style-type: none;
  text-align: left;
  padding-left: 0;
}

ul li{
  border-bottom: 1px solid lightgray;
  padding: 10px;
  cursor: pointer;
  background: #f1f1f1;
}
</style>
