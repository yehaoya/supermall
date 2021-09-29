<template>
    <div class="toast" v-show="isShow">
      <div>{{message}}</div>
    </div>
</template>

<script>
    export default {
        name: "Toast",
      props: {
        //   message: {
        //     type: String,
        //     default: ''
        //   },
        // show: {
        //     type: Boolean,
        //     default: false
        // }
      },
      data() {
          return {
            message: '',
            isShow: false
          }
      },
      methods: {
          show(message = '成功加入购物车', duration = 2000) {
            this.isShow = true;
            this.message = message;

            setTimeout(() => {
              this.isShow = false;
              this.message = ''
            },duration)
          }
      }
    }
</script>

<style scoped>
.toast {
  position: fixed;
  top:50%;
  left:50%;
  color: #fff;
  transform: translate(-50%,-50%);
  background-color: rgba(0,0,0,0.5);
  z-index: 999;
}
</style>
