<template lang="html">
  <div class="autocomplete">
    <div class="input" @click="toggleVisible" v-text="selectedItem ? selectedItem[filterby] : '' ">

    </div>
    <div class="placeholder" v-if="selectedItem == null" v-text="title || 'Select one'">

    </div>
    <div class="popover" v-show="visible">
      <input type="text" v-model="query" placeholder="Start Typing.." @keydown.up="up" @keydown.down="down" @keydown.enter="selectItem" ref="input">
      <div class="options" ref="optionsList">
        <ul>
          <li
            v-for="(item,index) in matches"
            :key="item.id"
            :class="{'selected' : (selected == index)}"
            v-text="item[filterby]"
            @click="itemClick(index)">
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props:['items','filterby','title'],
  data(){
    return{
      itemHeight:39,
      selectedItem: null,
      selected:0,
      visible:false,
      query:''
    }
  },
  methods:{
    toggleVisible(){
      this.visible = !this.visible
      setTimeout(() => {
        this.$refs.input.focus();
      },50);
    },
    itemClick(index){
      this.selected = index;
      this.selectItem();
    },
    selectItem(){
      this.selectedItem = this.matches[this.selected];
      this.visible = false;

      this.$emit('selected', JSON.parse(JSON.stringify(this.selectedItem)));
    },
    up(){
      if(this.selected == 0){
        return;
      }
      this.selected -= 1;
      this.scrollToItem()
    },
    down(){
      if(this.selected >= this.matches.length -1){
        return;
      }
      this.selected += 1;
      this.scrollToItem()

    },
    scrollToItem(){
      this.$refs.optionsList.scrollTop = this.selected * this.itemHeight
    }
  },
  computed:{
    matches(){
      this.$emit('change',this.query);
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
.options ul li.selected{
  background: #58bd4c;
  color:#fff;
  font-weight: 600;
}

.placeholder{
  position: absolute;
  top: 10px;
  left: 10px;
  font-size: 25px;
  color: lightgray;
  pointer-events: none;
}
</style>
