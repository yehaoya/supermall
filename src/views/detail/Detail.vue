<template>
<div id="detail">
  <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
  <scroll class="content"
          ref="scroll"
          :probe-type="3"
          @scroll="contentScroll">

    <detail-swiper :top-images="topImages"></detail-swiper>
    <detail-base-info :goods="goods"></detail-base-info>
    <detail-shop-info :shop="shop"></detail-shop-info>
    <detail-goods-info :detail-info="detailInfo" @imageload="imageload"></detail-goods-info>
    <detail-param-info :param-info="paramInfo" ref="params"></detail-param-info>
    <detail-comment-info :comment-info="commentInfo" ref="comment"></detail-comment-info>
    <goods-list :goods="recommends" ref="recommend"></goods-list>
  </scroll>
  <detail-bottom-bar @addcart="addToCart"></detail-bottom-bar>
  <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
<!--  <toast :message="message"  :show="show"></toast>-->
</div>
</template>

<script>
import DetailNavBar from "./childComps/DetailNavBar";
import DetailSwiper from "./childComps/DetailSwiper";
import DetailBaseInfo from "./childComps/DetailBaseInfo";
import DetailShopInfo from "./childComps/DetailShopInfo";
import Scroll from "../../components/common/scroll/Scroll";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
import DetailParamInfo from "./childComps/DetailParamInfo";
import DetailCommentInfo from "./childComps/DetailCommentInfo";
import GoodsList from "../../components/content/goods/GoodsList";
import DetailBottomBar from "./childComps/DetailBottomBar";
import BackTop from "../../components/content/backTop/BackTop";
// import Toast from "../../components/common/toast/Toast";


import {getDetail, Goods, Shop,GoodsParam,getRecommend} from "../../network/detail";
import {debounce} from "../../common/utils";
import {itemListenerMixin} from "../../common/mixin";

import {mapActions} from 'vuex'

export default {
  name: "Detail",
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsList,
    DetailBottomBar,
    BackTop,
    // Toast
  },
  mixins: [itemListenerMixin],
  data() {
    return {
      iid: null,
      topImages: [],
      goods: { },
      shop: { },
      detailInfo: { },
      paramInfo: { },
      commentInfo: { },
      recommends: [],
      themeTopYs: [],
      getThemeTopYs: null,
      currentIndex: 0,
      isShowBackTop: true,
      // message: '',
      // show: false

    }
  },
  created() {
    this.iid = this.$route.params.iid

    getDetail(this.iid).then(res => {
      // console.log(res);
      const data = res.result;
      this.topImages = data.itemInfo.topImages

      this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

      this.shop = new Shop(data.shopInfo)

      this.detailInfo = data.detailInfo;

      this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }

      // this.$nextTick(
      //   () => {
      //     this.themeTopYs = []
      //
      //     this.themeTopYs.push(0);
      //     this.themeTopYs.push(this.$refs.params.$el.offsetTop)
      //     this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
      //     this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
      //
      //     console.log(this.themeTopYs);
      //   }
      // )
    })


    getRecommend().then(res => {
      this.recommends = res.data.list;
    })

    this.getThemeTopYs = debounce(() => {
      this.themeTopYs = []
      this.themeTopYs.push(0);
      this.themeTopYs.push(this.$refs.params.$el.offsetTop)
      this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
      this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
      this.themeTopYs.push(Number.MAX_VALUE)

      // console.log(this.themeTopYs);
    },100)
  },
  mounted() {

  },

  destroyed() {
    this.$bus.$off('itemImgLoad', this.itemImgListener)
  },
  methods: {
    ...mapActions(['addCart']),
    imageload() {
      this.newRefresh()
      // this.$refs.scroll.refresh();
      this.getThemeTopYs()

    },
    titleClick(index) {
      this.$refs.scroll.scrollTo(0,-this.themeTopYs[index], 200)
    },
    contentScroll(position) {
      const positionY = -position.y ;
      let length = this.themeTopYs.length
      for(let i = 0; i < length-1; i++) {
        // if(positionY > this.themeTopYs[parseInt(i)] && positionY < this.themeTopYs[parseInt(i+1)]) {
        //
        // if(this.currentIndex !== i && ((i < length - 1 && positionY >= this.themeTopYs[parseInt(i)] && positionY < this.themeTopYs[parseInt(i+1)]) ||
        //   (i === length - 1 && positionY >= this.themeTopYs[i])) ) {
        //   this.currentIndex = i;
        //   console.log(this.currentIndex);
        //   this.$refs.nav.currentIndex = this.currentIndex
        // }
        if(this.currentIndex !== i && (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1])) {
          this.currentIndex = i;
            this.$refs.nav.currentIndex = this.currentIndex
        }
      }

      // this.isShowBackTop = (-position.y) > 1000
      //
      // this.isTabFixed = (-position.y) > this.tabOffsetTop

    },
    backClick() {
      // console.log(this.$refs.scroll.message);
      this.$refs.scroll.scrollTo(0,0,500)
    },
    addToCart() {
      //1.获取购物车需要展示的信息
      const product = {}
      product.image = this.topImages[0];
      product.title = this.goods.title;
      product.desc = this.goods.desc;
      product.price = this.goods.realPrice;
      product.iid = this.iid;

      //2.将商品添加到购物车手里
      // this.$store.cartList.push(product)
      // this.$store.commit('addCart', product)

      this.addCart(product).then(res => {
        // this.show = true;
        // this.message = res;
        //
        // setTimeout(() => {
        //   this.show = false;
        //   this.message = ''
        // },1500)

        this.$toast.show()

        console.log(res);
      })

      // this.$store.dispatch('addCart', product).then(res => {
      //   console.log(res);
      // })
    }
  }
}
</script>

<style scoped>
 #detail {
   position: relative;
   z-index: 9;
   background-color: white;
   height: 100vh;

 }

  .content {
    height: calc(100% - 44px - 49px);

  }

  .detail-nav{
    position: relative;
    z-index: 9;
    background-color: white;
  }
</style>
