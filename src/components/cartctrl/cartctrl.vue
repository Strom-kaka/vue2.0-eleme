<template>
  <div class="cartctrl">
    <transition name="move">
      <div class="cart-descrease icon-remove_circle_outline"
           v-show="food.count>0"
           @click.stop.prevent="descreaseCount($event)">
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCount($event)"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCount(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        this.$emit('cartAdd', event.target);
      },
      descreaseCount(event) {
        if (!event._constructed) {
          return;
        }

        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartctrl
    font-size 0
    .cart-descrease
      display inline-block
      padding 6px
      line-height 24px
      color rgb(0, 160, 220)
      font-size 24px
      &.move-enter-active
        animation move-in 0.4s linear
      &.move-leave-active
        animation move-out 0.4s linear
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      line-height 24px
      padding-top 6px
      color rgb(147, 153, 159)
      text-align center
      font-size 10px
    .cart-add
      display inline-block
      padding 6px
      line-height 24px
      color rgb(0, 160, 220)
      font-size 24px
</style>
