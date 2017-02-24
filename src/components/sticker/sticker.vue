<template>
  <span class="sticker" :class="stickerClass"></span>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      size: {
        type: Number
      },
      type: {
        type: Number
      }
    },
    computed: {
      stickerClass() {
        let stickerSize = 'sticker-' + this.size;
        let stickerType = this.classMap[this.type];
        return stickerSize + ' ' + stickerType;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin"
  .sticker
    display inline-block
    vertical-align top
    &.sticker-1
      width 12px
      height 12px
      background-size 12px 12px
      background-repeat no-repeat
      &.decrease
        bg-img('./img/decrease_1')
      &.discount
        bg-img('./img/discount_1')
      &.guarantee
        bg-img('./img/guarantee_1')
      &.invoice
        bg-img('./img/invoice_1')
      &.special
        bg-img('./img/special_1')
    &.sticker-2
      width 16px
      height 16px
      background-size 16px 16px
      background-repeat no-repeat
      &.decrease
        bg-img('./img/decrease_2')
      &.discount
        bg-img('./img/discount_2')
      &.guarantee
        bg-img('./img/guarantee_2')
      &.invoice
        bg-img('./img/invoice_2')
      &.special
        bg-img('./img/special_2')
    &.sticker-3
      width 12px
      height 12px
      background-size 12px 12px
      background-repeat no-repeat
      &.decrease
        bg-img('./img/decrease_3')
      &.discount
        bg-img('./img/discount_3')
      &.guarantee
        bg-img('./img/guarantee_3')
      &.invoice
        bg-img('./img/invoice_3')
      &.special
        bg-img('./img/special_3')
    &.sticker-4
      width 16px
      height 16px
      background-size 16px 16px
      background-repeat no-repeat
      &.decrease
        bg-img('./img/decrease_4')
      &.discount
        bg-img('./img/discount_4')
      &.guarantee
        bg-img('./img/guarantee_4')
      &.invoice
        bg-img('./img/invoice_4')
      &.special
        bg-img('./img/special_4')
</style>
