<template>
  <div class="search" :style="{backgroundColor: bgColor}">
    <img class="search-icon" :src="icon" alt="" srcset="">
    <span class="search-hint" :style="{color: hintStyle}">大前端开发，混合京东商城系统</span>
  </div>
</template>

<script>
export default {
  name: 'Search',
  props: {
    bgColor: {
      default: '#ffffff',
      type: String
    },
    hintColor: {
      default: '#999999',
      type: String
    },
    icon: {
      default: require('@img/search.svg'),
      type: String
    }
  },
  computed: {
    searchStyle: function () {
      return {
        backgroundColor: this.bgColor
      }
    },
    hintStyle: function () {
      return {
        color: this.hintColor
      }
    }
  },
  methods: {
  }
}
</script>

<style lang="scss">
  @import '../../assets/css/style.scss';
  .search {
    background-color: white;
    width: 100%;
    margin: px2rem(6);
    border-radius: px2rem(22);
    display: flex;
    align-items: center;
    .search-icon {
      margin-left: $marginSize;
      width: px2rem(16);
    }
    .search-hint {
      margin-left: $marginSize;
      font-size: $minInfoSize;
      color: $textHintColor;
    }
  }
</style>
