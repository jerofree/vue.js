<template>
  <div id="app">
  <h1 v-html="title"></h1>
    <input v-model="newItem" v-on:blur="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}"
          v-on:click="toggleFinsh(item)">
        {{item.label}}
      </li>
    </ul>
    <p>child tells me:{{childWords}}</p>
    <componentA msgfromfather="you die!" v-on:child-tell-me-something="listenTomyBoy"></componentA>
  </div>
</template>

<script>
  import Store from './store'
  import componentA from './components/componentA'

export default {
 data: function () {
   return {
      title:'this is a todo list',
      items:Store.fetch(),
     newItem:"",
     childWords:''
   }
 },
  components:{componentA},
  methods:{
    toggleFinsh:function (item) {
      item.isFinished=!item.isFinished;
    },
    addNew:function(){
      this.items.push({
        label:this.newItem,
        isFinished:false
      }),
      this.newItem="";
      Store.save()
    },
    listenTomyBoy:function (msg) {
      this.childWords=msg;
    }

  },
  watch:{
   items:{
     handler:function (items) {
       Store.save(items);
     },
     deep:true
   }
  }

}
</script>

<style>
  html{
    height: 100%;
  }
  body{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
  .finished{
    text-decoration: underline;
  }

</style>
