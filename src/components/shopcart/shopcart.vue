<template>
  <div class="shopcart">
    <div class="content" @click="toggleList()">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'light':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'light':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount>99? '99+' :totalCount}}</div>
        </div>
        <div class="price" :class="{'light':totalPrice>0}">¥{{totalPrice}}</div>
        <div class="desc">另需配送费¥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop.prevent="pay()">
        <div class="pay" :class="{'light':totalPrice>=minPrice}">{{payDesc}}</div>
      </div>
    </div>
    <div class="ball-container">
      <transition name="drop"
                  @before-enter="beforeEnter"
                  @enter="enter"
                  @after-enter="afterEnter">
        <div class="ball" v-show="ball.show">
          <div class="inner"></div>
        </div>
      </transition>
    </div>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="empty()">清空</span>
        </div>
        <div class="list-content" ref="listContent">
          <ul>
            <li class="food border-1px" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>¥{{food.price*food.count}}</span>
              </div>
              <div class="cartctrl-wrapper">
                <cartctrl :food="food"></cartctrl>
              </div>
            </li>
          </ul>
        </div>
      </div>

    </transition>
    <transition name="mask">
      <div class="list-mask" @click="hideList()" v-show="listShow"></div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import cartctrl from 'components/cartctrl/cartctrl';
  export default {
    props: {
      selectFoods: {
        type: Array,
        default() {
          return [];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        ball: {
          show: false,
          ele: []
        },
        fold: true
      };
    },
    methods: {
      pay() {
          if (this.totalPrice < this.minPrice) {
              return;
          }
          window.alert(`您需要支付${this.totalPrice}元`);
      },
      hideList() {
        this.fold = true;
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      drop(ele) {
        this.ball.show = true;
        this.ball.ele = ele;
      },
      beforeEnter(ele) {
        let rect = this.ball.ele.getBoundingClientRect();
        let x = rect.left - 32;
        let y = -(window.innerHeight - rect.top - 22);
        ele.style.transition = 'all 0s cubic-bezier(0.49,-0.29,0.75,0.41)';
        ele.style.webkitTransform = `translate3d(0,${y}px,0)`;
        ele.style.transform = `translate3d(0,${y}px,0)`;
        let inner = ele.getElementsByClassName('inner')[0];
        inner.style.transition = 'all 0s linear';
        inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
        inner.style.transform = `translate3d(${x}px,0,0)`;
      },
      enter(ele, done) {
        /* eslint-disable no-unused-vars */
        let rf = ele.offsetHeight;
        this.$nextTick(() => {
          ele.style.display = '';
          ele.style.transition = 'all 0.4s cubic-bezier(0.49,-0.29,0.75,0.41)';
          ele.style.webkitTransform = 'translate3d(0,0,0)';
          ele.style.transform = 'translate3d(0,0,0)';
          let inner = ele.getElementsByClassName('inner')[0];
          inner.style.transition = 'all 0.4s linear';
          inner.style.webkitTransform = 'translate3d(0,0,0)';
          inner.style.transform = 'translate3d(0,0,0)';
        });
        done();
      },
      afterEnter(ele) {
        this.ball.show = false;
        ele.style.display = 'none';
      },
      toggleList() {
        if (!this.totalCount) {
          this.fold = true;
          return;
        }

        this.fold = !this.fold;
      }
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        let diff = this.minPrice - this.totalPrice;
        if (this.totalPrice === 0) {
          return `¥${this.minPrice}起送`;
        } else if (this.totalPrice < this.minPrice) {
          return `还差¥${diff}起送`;
        } else {
          return '去结算';
        }
      },
      listShow() {
        if (!this.totalCount) {
          this.fold = true;
          return false;
        }
        let show = !this.fold;
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.listContent, {
                click: true
              });
            } else {
              this.scroll.refresh();
            }
          });
        }
        return show;
      }
    },
    components: {
      cartctrl
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin"
  @import "../../common/stylus/base.styl"
  .shopcart
    position fixed
    left 0
    bottom 0
    z-index 12
    width 100%
    height 48px
    .content
      display flex
      background #141d27
      font-size 0
      color rgba(255, 255, 255, 0.4)
      .content-left
        flex 1
        .logo-wrapper
          display inline-block
          vertical-align top
          position relative
          top -10px
          margin 0 12px
          padding 6px
          width 56px
          height 56px
          box-sizing border-box
          border-radius 50%
          background #141d27
          .logo
            width 100%
            height 100%
            border-radius 50%
            background #2b343c
            text-align center
            &.light
              background rgb(0, 160, 220)
            .icon-shopping_cart
              font-size 24px
              line-height 44px
              color #808581
              &.light
                color #fff
          .num
            position absolute
            right 0
            top 0
            width 24px
            height 16px
            line-height 16px
            text-align center
            border-radius 16px
            font-size 9px
            font-weight 700
            color #fff
            background rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          display inline-block
          vertical-align top
          margin-top 12px
          line-height 24px
          padding-right 12px
          border-right 1px solid rgba(255, 255, 255, 0.1)
          font-size 16px
          font-weight 700
          &.light
            color #fff
        .desc
          display inline-block
          vertical-align top
          margin 12px 0 0 12px
          line-height 24px
          font-size 10px
      .content-right
        flex 0 0 105px
        width 105px
        .pay
          height 48px
          line-height 48px
          text-align center
          font-size 12px
          font-weight 700
          background #2b333b
          &.light
            background #00b43c
            color #fff
    .ball-container
      .ball
        position absolute
        bottom 22px
        left 32px
        z-index 22
        .inner
          width 16px
          height 16px
          border-radius 50%
          background rgb(0, 160, 240)
    .shopcart-list
      position absolute
      left 0
      top 0
      width 100%
      transform translate3d(0, -100%, 0)
      z-index -1
      &.fold-enter-active, &.fold-leave-active
        transition transform 0.5s
      &.fold-enter, &.fold-leave-active
        transform translate3d(0, 0, 0)
      .list-header
        height 40px
        line-height 40px
        padding 0 18px
        background #f3f5f7
        border-bottom 1px solid rgba(7, 17, 27, 0.1)
        .title
          float left
          font-size 14px
          color rgb(7, 17, 27)
        .empty
          float right
          font-size 12px
          color rgb(0, 160, 220)
      .list-content
        padding 0 18px
        max-height 217px
        background #fff
        overflow hidden
        .food
          position relative
          padding 12px 0
          box-sizing border-box
          border-1px(rgba(7, 17, 27, 0.1))
          .name
            line-height 24px
            font-size 14px
            color rgb(7, 17, 27)
          .price
            position absolute
            right 90px
            bottom 12px
            line-height 24px
            font-size 14px
            font-weight 700
            color rgb(240, 20, 20)
          .cartctrl-wrapper
            position absolute
            right 0
            bottom 6px

    .list-mask
      position fixed
      left 0
      top 0
      z-index -2
      width 100%
      height 100%
      backdrop-filter blur(10px)
      background rgb(7, 17, 27)
      opacity 0.5
      &.mask-enter-active, &.mask-leave-active
        transition opacity 0.5s
      &.mask-enter, &.mask-leave-active
        opacity 0

</style>
