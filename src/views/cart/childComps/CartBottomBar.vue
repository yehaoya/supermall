<template>
<div class="bottom-bar">
 <div class="check-content">
   <check-button
     :is-checked="isSelectAll"
     class="check-button"
     @click.native="checkClick"
   ></check-button>
   <span>全选</span>
 </div>
  <div class="price">合计：{{totalPrice}}</div>
  <div class="calculate" @click="calcClick">去计算 {{checklength}}</div>
</div>
</template>

<script>
  import CheckButton from "../../../components/content/checkButton/CheckButton";
  import {mapGetters} from 'vuex'

    export default {
        name: "CartBottomBar",
      components: {
        CheckButton
      },
      computed: {
          ...mapGetters(['cartList']),
        totalPrice(){
          return  '￥' + this.cartList.filter(item=> item.isChecked
          ).reduce((previousValue,item)=>{
            return previousValue + item.count * item.price
          },0).toFixed(2)
        },
        checklength() {
          return this.cartList.filter(item => item.isChecked).length
        },
        isSelectAll() {
          if(this.cartList.length === 0) return false

          // return !(this.cartList.filter(item => !item.isChecked).length)

          // return !this.cartList.find(item => !item.isChecked)

          for(let item of this.cartList) {
            if(!item.isChecked){
              return false
            }
          }
          return true
        }
      },
      methods: {
        checkClick() {
         if(this.isSelectAll) {//全部选中
           this.cartList.forEach(item => item.isChecked = false)
         } else {//部分或全部不选中
           this.cartList.forEach(item => item.isChecked = true)
         }
        },
        calcClick() {
          if(!this.isSelectAll) {
            this.$toast.show('请选择购买的商品',2000)
          }
        }
      }
    }
</script>

<style scoped>
.bottom-bar {
  height: 40px;
  background-color: #eee;
  position: relative;
  line-height: 40px;
  display: flex;
  font-size: 14px;
}
  .check-content {
    display: flex;
    align-items: center;
    margin-left: 10px;
    width: 60px;
  }
  .check-button {
    width: 22px;
    height: 22px;
    line-height: 22px;
    margin-right: 5px;

  }
  .price {
    margin-left: 30px;
    flex: 1;
  }
  .calculate {
    width: 90px;
    background-color: red;
    color: #fff;
    text-align: center;
  }
</style>
