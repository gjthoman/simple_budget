<template>
  <span class="currency">${{ animatedNumber | currency }}</span>
</template>

<script>
var TWEEN = require('tween.js')
var requestAnimationFrame = require('raf')

export default {
  name: 'currency',
  props: ['number'],
  data () {
    return {
      animatedNumber: this.number
    }
  },
  watch: {
    number: function (newValue, oldValue) {
      var vm = this
      function animate (time) {
        requestAnimationFrame(animate)
        TWEEN.update(time)
      }
      new TWEEN.Tween({ tweeningNumber: oldValue })
        .easing(TWEEN.Easing.Quadratic.Out)
        .to({ tweeningNumber: newValue }, 500)
        .onUpdate(function () {
          vm.animatedNumber = this.tweeningNumber.toFixed(0)
        })
        .start()
      animate()
    }
  },
  filters: {
    currency (num) {
      return Number(num).toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,')
    }
  }
}
</script>