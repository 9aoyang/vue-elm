<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div v-if="seller.supports" class="support">
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper"></div>
  </div>
</template>
<script>
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {};
  },
  components: {}
};
</script>

<style lang="stylus" scoped>
@import '../../common/css/mixin'

.header
  color #fff
  background #000000
  .content-wrapper
    padding 24px 12px 18px 24px
    font-size 0
    .avatar
      display inline-block
    .content
      font-size 14px
      margin-left 16px
      display inline-block
      .title
        margin 2px 0 8px
        .brand
          display inline-block
          vertical-align top
          width 30px
          height 18px
          bg-image('brand')
          background-size 30px 18px
          background-repeat no-repeat
        .name
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
</style>
