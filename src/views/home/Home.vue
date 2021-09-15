<template>
<div id="home" class="wrapper">
  <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
  <tab-control
    :titles="['流行','新款','精选']"
    @tabClick="tabClick"
    ref="tabControl1"
    class="tab-control"
    v-show="isTabFixed"
  ></tab-control>
  <scroll class="content"
          ref="scroll"
          :probe-type="3"
          @scroll="contentScroll"
          :pull-up-load="true"
          @pullingup="loadMore"

  >
    <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
    <tab-control
      :titles="['流行','新款','精选']"
      @tabClick="tabClick"
      ref="tabControl2"

    ></tab-control>
    <goods-list :goods="showGoods"></goods-list>
  </scroll>


  <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
</div>
</template>

<script>
import NavBar from "../../components/common/navbar/NavBar";
import TabControl from "../../components/content/tabControl/TabControl";

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";
import GoodsList from "../../components/content/goods/GoodsList";
import BackTop from "../../components/content/backTop/BackTop";


import {getHomeMultidata, getHomeGoods} from "network/home";
import {debounce} from "../../common/utils";
import Scroll from "../../components/common/scroll/Scroll";


export default {
  name: "Home",
  components: {

    NavBar,
    TabControl,

    HomeSwiper,
    RecommendView,
    FeatureView,
    GoodsList,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []},

      },
      currentIndex: 'pop',
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY: 0

    }
  },
  computed: {
    showGoods() {
      return this.goods[this.currentIndex].list
    }
  },
  destroyed() {
    console.log('homedestroyed');
  },
  activated() {
    this.$refs.scroll.scrollTo(0,this.saveY, 0)
  },
  deactivated() {
    this.saveY = this.$refs.scroll.getScrollY()
  },
  created() {
    this.getHomeMultidata()
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')


  },
  mounted() {
    const refresh = debounce(this.$refs.scroll.refresh,50)


    this.$bus.$on('itemImageLoad', () => {
      // console.log('--------');

      refresh()
    })



  },
  methods: {


    //事件监听相关方法
    tabClick(index) {
     // console.log(index);
      switch (index) {
        case 0:
          this.currentIndex = 'pop'
              break
        case 1:
          this.currentIndex= 'new'
              break
        case 2:
          this.currentIndex = 'sell'
              break
      }
      this.$refs.tabControl1.currentIndex = index;
      this.$refs.tabControl2.currentIndex = index;

    },
    backClick() {
     // console.log(this.$refs.scroll.message);
      this.$refs.scroll.scrollTo(0,0,500)
    },
    contentScroll(position) {
      this.isShowBackTop = (-position.y) > 1000

      this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
    loadMore() {
     this.getHomeGoods(this.currentIndex)
    },
    swiperImageLoad() {
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
    },



    //网络请求相关方法
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
       this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1


        this.$refs.scroll.finishPullUp()


      })
    }
  }
}
</script>

<style scoped>
#home {
  /*padding-top: 44px;*/
  height: 100vh;
  position: relative;
}


.home-nav {
  background-color: var(--color-tint);
  color: white;

  /*position: fixed;*/
  /*left: 0;*/
  /*right: 0;*/
  /*top: 0;*/
  /*z-index: 9;*/
}

.content{

  overflow: hidden;

  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;

}

.tab-control {
  position: relative;
  z-index: 9;
}

/*.content {*/
/*  height: calc(100% - 93px);*/
/*  overflow: hidden;*/
/*  margin-top: 44px;*/
/*}*/
</style>
