<template>
<div class="goods">
  <good-list-item v-for="item in goods" :goods-item="item" :key=""></good-list-item>

</div>
</template>

<script>
import GoodListItem from "./GoodListItem";


export default {
  name: "GoodsList",
  components: {
    GoodListItem
  },
  props: {
    goods: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
.goods {
  display: flex;
  flex-wrap: wrap;

  justify-content: space-evenly;
}
</style>
