<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(index, good) in goods" :key="index">
          <span class="text">
            <span v-show="good.type > 0" class="icon" :class="classMap[good.type]"></span>{{ good.name }}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script>
const ERR_OK = 0;

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: []
    };
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;
      }
    });
  },
  components: {

  }
};
</script>

<style lang="stylus" scoped>
  .goods
    display flex
    position absolute
    top: 174px
    bottom 46px
    width 100%
    .menu-wrapper
      flex: 0 0 80px
      width 80px
      background #f3f5f7
    .foods-wrapper
      flex 1
</style>
