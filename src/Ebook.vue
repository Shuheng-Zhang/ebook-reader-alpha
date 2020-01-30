<template>
  <div class="ebook">
    <div class="title-wrapper" v-show="ifMenuAndTitleShow">
      <div class="left">
        <span class="icon-back icon"></span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="icon-cart icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-person icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-more icon"></span>
        </div>
      </div>
    </div>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
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
  </div>
</template>

<script>
import Epub from "epubjs";

const EBOOK_URL = "ex.epub";

export default {
  data() {
    return {
      ifMenuAndTitleShow: false
    }
  },
  methods: {
    // 解析并渲染电子书
    showEpub() {
      // 加载电子书文件
      this.book = new Epub(EBOOK_URL);

      // 渲染并展示内容
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      this.rendition.display();
    },
    // 上一页
    prevPage() {
      if (this.rendition) {
        this.rendition.prev();
      }
    },
    // 下一页
    nextPage() {
      if (this.rendition) {
        this.rendition.next();
      }
    },
    // 切换标题栏和菜单栏显示状态
    toggleTitleAndMenu() {
      this.ifMenuAndTitleShow = !this.ifMenuAndTitleShow;
    }
  },
  mounted() {
    this.showEpub();
  }
};
</script>

<style lang="scss" scoped>
@import "assets/styles/global.scss";
.ebook {
  position: relative;
  .title-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    background-color: #fff;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, 0.15);
    display: flex;
    .left {
      flex: 0 0 px2rem(60);
      @include center;
    }
    .right {
      flex: 1;
      display: flex;
      justify-content: flex-end;
      .icon-wrapper {
        flex: 0 0 px2rem(40);
        @include center;
        .icon-cart {
          font-size: px2rem(22);
        }
      }
    }
  }
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      width: 100%;
      height: 100%;
      display: flex;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
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
}
</style>
