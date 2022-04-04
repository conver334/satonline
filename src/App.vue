<template>
  <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
  <el-menu-item index="1">在线</el-menu-item>
  <el-menu-item index="2">下载</el-menu-item>
  </el-menu>
  <router-view/>
</template>
<script>
import Navbar from './components/Navbar.vue'
export default {
  components:{Navbar},
  data() {
    return {
      activeIndex: '1'
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      if(keyPath[0]==1)this.$router.push({name:'home'})
      else this.$router.push({name:'download'})
    }
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
