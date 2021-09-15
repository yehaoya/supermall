<template>
<div class="wrapper" ref="wrapper">
  <div class="content">
    <slot></slot>
  </div>

</div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "Scroll",
  props: {
    probeType: {
      type: Number,
      default: 0,
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }

  },
  data() {
    return {
      scroll: null,
      message: '哈哈哈'
    }
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper,{
      click: true,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    })
    if(this.probeType == 2 || this.probeType == 3) {
      this.scroll.on('scroll', (position) => {
        // console.log(position);

        this.$emit('scroll',position)
      })
    }


    if(this.pullUpLoad) {
      this.scroll.on('pullingUp', () => {
        this.$emit('pullingup')
      })
    }

  },
  methods: {
    scrollTo(x, y, time) {
      this.scroll && this.scroll.scrollTo(x, y, time)
    },
    finishPullUp() {
      this.scroll && this.scroll.finishPullUp()
    },
    refresh() {

     this.scroll && this.scroll.refresh()

    },
    getScrollY() {
       return this.scroll ? this.scroll.y : 0
    }
  }
}
</script>

<style scoped>

</style>
