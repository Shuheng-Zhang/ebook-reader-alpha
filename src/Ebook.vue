<template>
  <div class="ebook">
    <TitleBar :ifMenuAndTitleShow="ifMenuAndTitleShow"></TitleBar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <MenuBar :ifMenuAndTitleShow="ifMenuAndTitleShow"></MenuBar>
  </div>
</template>

<script>
import TitleBar from './components/TitleBar'
import MenuBar from './components/MenuBar'

import Epub from "epubjs";

const EBOOK_URL = "ex.epub";

export default {
  data() {
    return {
      ifMenuAndTitleShow: false
    };
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
  },
  components: {
    TitleBar,
    MenuBar
  }
};
</script>

<style lang="scss" scoped>
@import "assets/styles/global.scss";
.ebook {
  position: relative;

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
}
</style>
