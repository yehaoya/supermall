<template>
  <div id="app" >
  <keep-alive exclude="Detail">
    <router-view></router-view>
  </keep-alive>
  <main-tar-bar></main-tar-bar>
  </div>
</template>

<script>
import MainTarBar from "./components/content/MainTabBar/MainTarBar";

export default {
  name: 'App',
  components: {
    MainTarBar
  }
}
</script>

<style>
@import "assets/css/base.css";
</style>
