<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight': totalCount > 0}">
            <span class="icon-shopping_cart" :class="{'highlight': totalCount > 0}"></span>
          </div>
          <div class="num" v-show="total > 0">{{ totalCount }}</div>
        </div>
        <div class="price" :class="{'highlight': totalPrice > 0}">￥{{ totalPrice }}</div>
        <div class="desc">另需配送费￥{{ deliveryPrice }}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">
          {{ payDesc }}
        </div>
      </div>
    </div>
    <div class="ball-container">
      <div
        transition="drop"
        class="ball"
        v-for="(index, ball) in balls"
        v-show="ball.show"
        :key="index">
        <div class="inner inner-hook"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [{
          price: 10,
          count: 1
        }];
      }
    },
    deliveryPrice: {
      type: Number
    },
    minPrice: {
      type: Number
    }
  },
  data() {
    return {
      balls: [
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        }
      ],
      dropBalls: []
    };
  },
  computed: {
    totalPrice() {
      return this.selectFoods.reduce((total, food) =>
        total += food.price * food.count
      , 0);
    },
    totalCount() {
      return this.selectFoods.reduce((count, food) =>
        count += food.count
      , 0);
    },
    payDesc() {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}起送`;
      } else if (this.totalPrice < this.minPrice) {
        const diff = this.minPrice - this.totalPrice;
        return `还差￥${diff}元起送`;
      } else {
        return '去结算';
      }
    },
    payClass() {
      return this.totalPrice < this.minPrice ? 'not-enough' : 'enough';
    }
  },
  methods: {
    drop(el) {
      for (let i = 0; i < this.balls.length; i++) {
        const ball = this.balls[i];
        if (!ball.show) {
          ball.show = true;
          ball.el = el;
          this.dropBalls.push(ball);
          return;
        }
      }
    }
  },
  transitions: {
    drop: {
      beforeEnter(el) {
        let count = this.balls.length;
        while (count--) {
          const ball = this.balls[count];
          if (ball.show) {
            const rect = ball.el.getBoundingClientRect();
            const x = rect.left - 32;
            const y = -(window.innerHeight - rect.top - 22);
            el.style.display = '';
            el.style.webKitTransform = `translate3d(0, ${y}px, 0)`;
            el.style.transform = `translate3d(0, ${y}px, 0)`;
            const inner = el.querySelector('.inner-hook');
            inner.style.webKitTransform = `translate3d(${x}px, 0, 0)`;
            inner.style.transform = `translate3d(${x}px, 0, 0)`;
          }
        }
      },
      enter(el) {
        // 主动触发浏览器重绘
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight;
        this.$nextTick(() => {
          el.style.webKitTransform = 'translate3d(0, 0, 0)';
          el.style.transform = 'translate3d(0, 0, 0)';
          const inner = el.querySelector('.inner-hook');
          inner.style.webKitTransform = 'translate3d(0, 0, 0)';
          inner.style.transform = 'translate3d(0, 0, 0)';
        });
      },
      afterEnter(el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
        }
      }
    }
  }
};
</script>

<style lang="stylus" scoped>
.shopcart
  position fixed
  left 0
  bottom 0
  z-index 50
  width 100%
  height 48px
  .content
    display flex
    background #141d27
    font-size 0
    color rgba(255, 255, 255, 0.4)
    .content-left
      flex 1
      .logo-wrapper, .price, .desc
        display inline-block
        vertical-align top
        box-sizing border-box

      .logo-wrapper
        position relative
        top -10px
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        border-radius 50%
        background #141d27
        .logo
          width 100%
          height 100%
          border-radius 50%
          text-align center
          background #2b343c
          &.highlight
            background rgb(0, 160, 220)
          .icon-shopping_cart
            line-height 44px
            font-size 24px
            color #80858a
            &.highlight
              color #fff
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 9px
          font-weight 700
          background rgb(240, 20, 20)
          box-shadow 0 4px 8px rgba(0, 0, 0, 0.4)
      .price
        line-height 24px
        margin-top 12px
        padding-right 12px
        border-right 1px solid rgba(255, 255, 255, 0.1)
        font-size 16px
        font-weight 700
        &.highlight
          color #fff

      .desc
        line-height 24px
        margin 12px 0 0 12px
        font-size 10px
    .content-right
      flex 0 0 105px
      width 105px
      .pay
        height 48px
        line-height 48px
        text-align center
        font-size 12px
        &.not-enough
          background #2b333b
        &.enough
          background #00b43c
          color #fff
  .ball-container
    .ball
      position fixed
      left 32px
      bottom 22px
      z-index 200
      &.drop-transition
        transition all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
        .inner
          width 16px
          height 16px
          border-radius 50%
          background rgb(0, 160, 220)
          transition all 0.4s linear
</style>
