<template>
  <div class="ebook">
    <TitleBar :ifTitleAndMenuShow="ifTitleAndMenuShow"></TitleBar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <MenuBar
      ref="menuBar"
      :ifTitleAndMenuShow="ifTitleAndMenuShow"
      :fontSizeList="fontSizeList"
      :defalutFontSize="defalutFontSize"
      @setFontSize="setFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      @setTheme="setTheme"
      :bookAvailable="bookAvailable"
      @onProgressChange="onProgressChange"
      :navigation="navigation"
      @jumpTo="jumpTo"
    ></MenuBar>
  </div>
</template>

<script>
import TitleBar from "./components/TitleBar";
import MenuBar from "./components/MenuBar";

import Epub from "epubjs";

const EBOOK_URL = "ex.epub";

export default {
  data() {
    return {
      ifTitleAndMenuShow: false,
      defalutFontSize: 16,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultTheme: 0,
      themeList: [
        {
          name: "default",
          style: {
            body: {
              color: "#000000",
              background: "#ffffff"
            }
          }
        },
        {
          name: "eye",
          style: {
            body: {
              color: "#000000",
              background: "#ceeaba"
            }
          }
        },
        {
          name: "night",
          style: {
            body: {
              color: "#ffffff",
              background: "#000000"
            }
          }
        },
        {
          name: "gold",
          style: {
            body: {
              color: "#000000",
              background: "rgb(241, 236, 226)"
            }
          }
        }
      ],
      bookAvailable: false, // 电子书可用状态
      navigation: {}
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

      // 获取 Theme 对象
      this.themes = this.rendition.themes;

      // 设置默认字体字号
      this.setFontSize(this.defalutFontSize);

      // 注册主题样式
      this.registerTheme();
      // 配置默认主题样式
      this.themes.select(this.defaultTheme);

      // 处理电子书加载完成后事件处理
      this.book.ready
        .then(() => {
          // 获取 Navigation 对象
          this.navigation = this.book.navigation;
          // 生成 Locations 对象
          return this.book.locations.generate();
        })
        .then(res => {
          // 获取 Locations 对象
          this.locations = this.book.locations;
          // 设置电子书可用状态
          this.bookAvailable = true;
        });
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
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;

      if (!this.ifTitleAndMenuShow) {
        this.$refs.menuBar.hideSetting();
      }
    },
    // 隐藏目录面板、标题栏及菜单栏
    hideTitleAndMenu() {
      this.ifTitleAndMenuShow = false;
      this.$refs.menuBar.hideSetting();
      // this.$refs.titleBar.hideContent();
    },
    // 设置字体字号
    setFontSize(fontSize) {
      if (this.themes) {
        this.defalutFontSize = fontSize;
        this.themes.fontSize(fontSize + "px");
      }
    },
    // 注册主题样式
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style);
      });
    },
    // 设置主题样式
    setTheme(index) {
      this.themes.select(this.themeList[index].name);
      this.defaultTheme = index;
    },
    // 根据进度百分比跳转页面
    // progress 数值为[0-100]
    onProgressChange(progress) {
      const percentage = progress / 100;
      const location =
        percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0;
      this.rendition.display(location);
    },
    // 根据目录链接跳转到页面
    jumpTo(href) {
      this.rendition.display(href);
      this.hideTitleAndMenu();
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
