<template>
  <div class="goods-options  z-index-2">
    <ul class="goods-options-list">
      <li class="goods-options-item"
          v-for="(item, index) in optionsDatas"
          :key="index">
        <a class="goods-options-item-content"
           @click="onOptionsItemClick(item, index)">
          <span class="goods-options-item-content-name"
                :class="{'goods-options-item-content-name-active' : selectOption.id === item.id}"
          >{{item.name}}</span>
          <span class="goods-options-item-content-caret caret"
                v-if="item.subs.length > 0"
                :class="[isShowSubContent && selectOption.id === item.id ? 'goods-options-item-content-caret-open' : 'goods-options-item-content-caret-close']">
          </span>
        </a>
      </li>
    </ul>
    <transition name="fold-height">
      <div class="options-sub-content z-index-2" v-show="isShowSubContent">
        <ul class="options-sub-content-list">
          <li class="options-sub-content-list-item"
              @click="onSubOptionsItemClick(item, index)"
              v-for="(item, index) in selectOption.subs" :key="index">
            <a class="options-sub-content-list-item-content">
              <span class="options-sub-content-list-item-content-name"
                    :class="{'options-sub-content-list-item-content-name-active' : selectOption.id === item.id}">
                {{item.name}}</span>
              <img class="options-sub-content-list-item-content-select"
                   v-show="selectOption.id === item.id"
                   src="@img/options-select.svg"
                   alt=""
                   srcset="">
            </a>
          </li>
        </ul>
      </div>
    </transition>

    <div class="cover" v-show="isShowSubContent" @click="isShowSubContent = false"></div>
  </div>
</template>

<script>
export default {
  name: 'GoodsOptions',
  data: function () {
    return {
      optionsDatas: [
        {
          id: '1',
          name: '默认',
          subs: [
            {
              id: '1',
              name: '默认'
            },
            {
              id: '1-2',
              name: '价格由高到底'
            },
            {
              id: '1-3',
              name: '销量由高到底'
            }
          ]
        },
        {
          id: '2',
          name: '有货优先',
          subs: []
        },
        {
          id: '3',
          name: '直营优先',
          subs: []
        }
      ],
      selectOption: {},
      isShowSubContent: false
    }
  },
  created: function () {
    this.selectOption = this.optionsDatas[0]
  },
  methods: {
    onOptionsItemClick: function (item, index) {
      if (this.isShowSubContent) {
        this.isShowSubContent = false
        return
      }
      if (item.subs.length > 0 && this.selectOption.id === item.id) {
        this.isShowSubContent = true
      }
      this.selectOption = item
    },
    onSubOptionsItemClick: function (item, index) {
      this.selectOption = item
      this.optionsDatas.forEach(options => {
        options.subs.forEach(subOptions => {
          if (subOptions.id === this.selectOption.id) {
            options.id = subOptions.id
            options.name = subOptions.name
          }
        })
      })
      this.isShowSubContent = false
    }
  },
  watch: {
    selectOption: function (newV) {
      this.$emit('optionsChange', newV.id)
    }
  }
}
</script>

<style lang="scss" scoped>
  @import '../../assets/css/style.scss';
  .goods-options {
    width: 100%;
    border-bottom: 1px solid $lineColor;
    &-list {
      display: flex;
      width: 100%;
      height: $goodsOptionsHeight;
      background-color: white;
      .goods-options-item {
        flex-grow: 1;

        &-content {
          height: 100%;
          display: flex;
          justify-content: center;
          align-items: center;

          &-name {
            font-size: $infoSize;
            margin-right: $marginSize;

            &-active{
              color: $mainColor;
            }
          }

          // 子选项展开时，三角形的动画
          &-caret {
            &-open {
              transform:rotate(-180deg);
              transition: all .3s;
            }

            &-close {
              transform:rotate(0deg);
              transition: all .3s;
            }
          }
        }
      }
    }
    // 子选项内容区
    .options-sub-content {
      // 脱离标准文档流
      position: absolute;
      width: 100%;
      max-height: px2rem(180);
      overflow: hidden;
      overflow-y: auto;
      background-color: white;
      &-list {

        &-item {

          &-content {
            display: flex;
            align-items: center;
            border-top: 1px solid $lineColor;
            padding: $marginSize;
            height: px2rem(44);
            box-sizing: border-box;
            &-name {
              font-size: $infoSize;
              display: inline-block;
              flex-grow: 1;

              &-active{
                color: $mainColor;
              }
            }

            &-select {
              width: px2rem(18);
              height: px2rem(18);
            }
          }
        }
      }
    }
    .fold-height-enter-active {
      animation-duration: 3s;
      animation-name: fold-height-open;
    }

    @keyframes fold-height-open {
      0% {
        max-height: 0;
      }
      100% {
        max-height: px2rem(180);
      }
    }
    .fold-height-leave-active {
      animation-duration: 3s;
      animation-name: fold-height-close;
    }
    @keyframes fold-height-close {
      0% {
        max-height: px2rem(180);
      }
      100% {
        max-height: 0;
      }
    }
  }
</style>
