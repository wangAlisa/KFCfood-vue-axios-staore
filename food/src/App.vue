<template>
  <div id="app">
    <leftNav></leftNav>
   <div class="main">
      <router-view></router-view>
   </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'app',
  components :{
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft Yahei','Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height:100%;
}
.main{
  float:right;
  width:95%;
  background:#eff2f7;
  height:100%;
  overflow: auto;
  box-sizing: border-box;
}
</style>
