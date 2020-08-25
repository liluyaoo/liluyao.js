<template>
  <transition name="slide-up">
    <div class="setting-wrapper" v-show="menuVisible && settingVisible === 1">
      <div class="setting-theme">
        <div class="setting-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(index)">
          <div class="preview" :class="{'selected': item.name === defaultTheme}" :style="{background: item.style.body.background}"></div>
          <div class="text" :class="{'selected': item.name === defaultTheme}">{{item.alias}}</div>
        </div>
      </div>
    </div>
  </transition>
</template>

// 还存在两个问题： 1 第一次点击全局无法同步，再次点击主题就同步了 2 刷新后无法保留原有主题样式
<script>
    import { ebookMixin } from '../../utils/mixin'
    import { themeList } from '../..//utils/book'
    import { saveTheme } from '../../utils/localStorage'

    export default {
        mixins: [ebookMixin],
        computed: {
            themeList() {
                return themeList(this)
            }
        },
        methods: {
            setTheme(index) {
                const theme = this.themeList[index]
                this.setDefaultTheme(theme.name).then(() => {
                    this.currentBook.rendition.themes.select(this.defaultTheme)
                    this.initGlobalStyle()
                })
                saveTheme(this.fileName, theme)
            }
        }
    }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../assets/styles/global.scss";
    .setting-wrapper {
        position: absolute;
        bottom: px2rem(48);
        left: 0;
        width: 100%;
        z-index: 101;
        height: px2rem(90);
        background: white;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba($color: #000000, $alpha: .15);
        .setting-theme {
            height: 100%;
            display: flex;
            .setting-theme-item {
                flex: 1;
                display: flex;
                flex-direction: column;
                padding: px2rem(5);
                box-sizing: border-box;
                .preview {
                    flex: 1;
                    border: px2rem(1) solid #ccc;
                    box-sizing: border-box;
                    &.selected {
                        box-shadow: 0 px2rem(4) px2rem(6) 0 rgba(0, 0, 0, .1);
                    }
                }
                .text {
                    flex: 0 0 px2rem(20);
                    font-size: px2rem(14);
                    color: #ccc;
                    @include center;
                    &.selected {
                        color: #333;
                    }
                }
            }
        }
    }
</style>
