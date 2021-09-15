<template>
<div class="wrapper">
  <ul class="content">
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
    <li>列表信息</li>
  </ul>
</div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
  name: "Category",
  data() {
    return {
      scrool: null
    }
  },
  created() {

  },
  mounted() {
    this.scrool = new BScroll(document.querySelector('.wrapper'),{

    })
  }
}
</script>

<style scoped>
.wrapper {
  height: 150px;
  background-color: red;

  overflow: hidden;
  /*overflow-y: scroll;*/
}
</style>
