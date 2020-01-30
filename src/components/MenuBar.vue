<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div class="menu-wrapper" v-show="ifMenuAndTitleShow">
        <div class="icon-wrapper">
          <span class="icon-menu icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-progress icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-bright icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-a icon">A</span>
        </div>
      </div>
    </transition>

    <div class="setting-wrapper"></div>
  </div>
</template>

<script>
export default {
  props: {
    ifMenuAndTitleShow: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/global.scss";
.menu-wrapper {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: px2rem(48);
  z-index: 101;
  background-color: #fff;
  box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
  display: flex;
  .icon-wrapper {
    flex: 1;
    @include center;
    .icon-progress {
      font-size: px2rem(28);
    }
    .icon-bright {
      font-size: px2rem(24);
    }
  }
}
</style>
