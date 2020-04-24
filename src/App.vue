<template>
  <div id="app">

   <el-container>
  <el-aside width="200px"><Aside></Aside></el-aside>
  <el-container>
    <el-header><Header></Header></el-header>
    <el-main>  <router-view/></el-main>
  </el-container>
</el-container>

  
  </div>
</template>

<script>
// @ is an alias to /src
import Aside from '@/views/Aside.vue'
import Header from '@/views/Header.vue'
export default {
  name: 'App',
  components: {
    Aside,
    Header
  }
}
</script>



<style lang="stylus">
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>

