<template>
  <div style="height:100%;">
    <div v-transfer-dom>
      <loading v-model="isLoading"></loading>
    </div>
    <view-box ref="viewBox" :body-padding-top="0" :body-padding-bottom="isShowTabbar ? '55px' : '0'">
      <transition
        @after-enter="$vux.bus && $vux.bus.$emit('vux:after-view-enter')"
        :name="viewTransition" :css="!!direction">
          <router-view class="router-view"></router-view>
      </transition>
      <tabbar class="vux-demo-tabbar" icon-class="vux-center" v-show="isShowTabbar" slot="bottom">
        <tabbar-item :link="{path:'/'}" :selected="route.path === '/'">
          <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe637;</span>
          <span slot="label">首页</span>
        </tabbar-item>
        <tabbar-item :link="{path:'/withdraw'}" :selected="route.path === '/withdraw'">
          <span class="demo-icon-22" slot="icon">&#xe633;</span>
          <span slot="label">
            <span>提现</span></span>
        </tabbar-item>
        <tabbar-item :link="{path:'/friends'}" :selected="route.path === '/friends'">
          <span class="demo-icon-22" slot="icon">&#xe633;</span>
          <span slot="label">
            <span>邀请好友</span></span>
        </tabbar-item>
        <tabbar-item :link="{path:'/user'}" :selected="route.path === '/user'">
          <span class="demo-icon-22" slot="icon">&#xe633;</span>
          <span slot="label">
            <span>我</span></span>
        </tabbar-item>
      </tabbar>
    </view-box>
  </div>
</template>

<script>
import { ViewBox, Tabbar, TabbarItem, Loading, TransferDom } from "vux";
import { mapState } from "vuex";
export default {
  name: "app",
  directives: {
    TransferDom
  },
  components: {
    ViewBox,
    Tabbar,
    TabbarItem,
    Loading
  },
  computed: {
    ...mapState({
      route: state => state.route,
      path: state => state.route.path,
      deviceready: state => state.app.deviceready,
      demoTop: state => state.vux.demoScrollTop,
      isLoading: state => state.vux.isLoading,
      direction: state => state.vux.direction
    }),
    viewTransition() {
      if (!this.direction) return "";
      return "vux-pop-" + (this.direction === "forward" ? "in" : "out");
    },
    isShowTabbar() {
      let whiteUrl = ['/','/withdraw','/user','/friends'] 
      if (Array.indexOf(whiteUrl,this.path) < 0) {
        return false;
      }
      return true;
    }
  },
  mounted(){
    console.log(this.path)
  }
};
</script>

<style lang="less">
@import "~vux/src/styles/reset.less";
@import "./theme.less";
body {
  // background-color: #fbf9fe;
  background-color: #eeeeee;
}
html,
body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}

.demo-icon-22 {
  font-family: "vux-demo";
  font-size: 22px;
  color: #888;
}

.vux-demo-tabbar .weui-bar__item_on .demo-icon-22 {
  color: #ff6764;
}
.vux-demo-tabbar
  .weui-tabbar_item.weui-bar__item_on
  .vux-demo-tabbar-icon-home {
  color: rgb(53, 73, 94);
}
.demo-icon-22:before {
  content: attr(icon);
}
.vux-demo-tabbar-component {
  background-color: #ff6764;
  color: #fff;
  border-radius: 7px;
  padding: 0 4px;
  line-height: 14px;
}
.weui-tabbar__icon + .weui-tabbar__label {
  margin-top: 0 !important;
}
.vux-demo-header-box {
  z-index: 100;
  position: absolute;
  width: 100%;
  left: 0;
  top: 0;
}

@font-face {
  font-family: "vux-demo"; /* project id 70323 */
  src: url("//at.alicdn.com/t/font_70323_wlronpvr565yiudi.eot");
  src: url("//at.alicdn.com/t/font_70323_wlronpvr565yiudi.eot?#iefix")
      format("embedded-opentype"),
    url("//at.alicdn.com/t/font_70323_wlronpvr565yiudi.woff") format("woff"),
    url("//at.alicdn.com/t/font_70323_wlronpvr565yiudi.ttf") format("truetype"),
    url("//at.alicdn.com/t/font_70323_wlronpvr565yiudi.svg#iconfont")
      format("svg");
}

.demo-icon {
  font-family: "vux-demo";
  font-size: 20px;
  color: #04be02;
}

.demo-icon-big {
  font-size: 28px;
}

.demo-icon:before {
  content: attr(icon);
}

.router-view {
  width: 100%;
  top: 0;
}
.vux-pop-out-enter-active,
.vux-pop-out-leave-active,
.vux-pop-in-enter-active,
.vux-pop-in-leave-active {
  will-change: transform;
  transition: all 500ms;
  height: 100%;
  top: 0;
  position: absolute;
  backface-visibility: hidden;
  perspective: 1000;
}
.vux-pop-out-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.vux-pop-out-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.vux-pop-in-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.vux-pop-in-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.menu-title {
  color: #888;
}
</style>
