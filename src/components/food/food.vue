<template>
  <div v-show="showFlag" class="food" transition="move" v-el:food>
    <div class="food-content">
      <div class="image-header">
        <img alt="" :src="food.image">
        <div class="back" @click="hide()">
          <i class="icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{ food.name }}</h1>
        <div class="detail">
          <span class="sell-count">月售{{ food.sellCount }}</span>
          <span class="rating">好评率{{ food.rating }}</span>
        </div>
        <div class="price">
          <span class="now">￥{{ food.price }}</span><span class="old" v-show="food.oldPrice">{{ food.oldPrice }}</span>
        </div>
      </div>
      <div class="cartcontrol-wrapper">
        <cartcontrol :food="food"></cartcontrol>
      </div>
      <div class="buy"></div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import cartcontrol from 'components/cartcontrol/cartcontrol';

export default {
  props: {
    food: {
      type: Object
    }
  },
  components: {
    cartcontrol
  },
  data() {
    return {
      showFlag: false
    };
  },
  methods: {
    show() {
      this.showFlag = true;
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$els.food, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      });
    },
    hide() {
      this.showFlag = false;
    }
  }
};
</script>

<style lang="stylus" scoped>
.food
  position fixed
  left 0
  top 0
  bottom 48px
  z-index 30
  width 100%
  background #fff
  &.move-transition
    transition all 0.2s
    transform translate3d(0, 0, 0)
  &.move-enter, &.move-leave
    transform translate3d(100%, 0, 0)

  .image-header
    position relative
    width 100%
    height 0
    padding-top: 100%
    img
      position absolute
      top 0
      left 0
      width 100%
      height 100%
    .back
      position absolute
      top 10px
      left 0
      .icon-arrow_lift
        display block
        padding 10px
        font-size 20px
        color #fff
  .content
    padding 18px
    .title
      margin-bottom 8px
      line-height 14px
      font-size 14px
      font-weight 700
      color rgb(7, 17, 27)
    .detail
      margin-bottom 18px
      height 18px
      line-height 10px
      font-size 0
      .sell-count, .rating
        font-size 10px
        color rgb(147, 153, 159)
      .sell-count
        margin-left 12px
    .price
      font-weight 700
      line-height 24px
      .now
        margin-right 18px
        font-size 14px
        color rgb(240, 20, 20)
      .old
        text-decoration line-through
        font-size 10
        color rgb(147, 153, 159)
  .cartcontrol-wrapper
    position absolute
    right 0
    bottom 12px
</style>
