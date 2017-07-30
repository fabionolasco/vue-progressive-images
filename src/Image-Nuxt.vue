<template>
  <span>
    <img 
      v-bind:src="srcValue"
      v-bind:alt="alt"
      v-bind:style="styles" />
  </span>
</template>

<script>
import Vue from 'vue'

let windowEvent
export default {
  name: 'fn-image',
  props: ['src', 'alt', 'blur', 'animate', 'maxRatio'],
  data () {
    return {
      srcValue: this.src,
      blurValue: 0,
      animateValue: 0,
      styles: {}
    }
  },
  methods: {
    updateSrc () {
      let dpi = window.devicePixelRatio
      if (this.srcValue && dpi && dpi >= 1) {
        if (typeof this.maxRatio === 'number' && dpi > this.maxRatio) {
          dpi = this.maxRatio
        }
        this.srcValue = this.srcValue.replace('@0', `@${dpi.toString()[0]}`)
        this.blurValue = 0
        this.updateBlur()
      }
    },
    updateBlur () {
      if (typeof this.blur !== 'undefined') {
        this.styles = {
          filter: `blur(${this.blurValue}px)`,
          '-webkit-filter': `blur(${this.blurValue}px)`,
          '-moz-filter': `blur(${this.blurValue}px)`,
          '-o-filter': `blur(${this.blurValue}px)`,
          '-ms-filter': `blur(${this.blurValue}px)`,
          transition: `${this.animateValue}s filter linear`,
          '-webkit-transition': `${this.animateValue}s -webkit-filter linear`,
          '-moz-transition': `${this.animateValue}s -moz-filter linear`,
          '-ms-transition': `${this.animateValue}s -ms-filter linear`,
          '-o-transition': `${this.animateValue}s -o-filter linear`
        }
      }
    }
  },
  created () {
    if (Vue.config.progressiveImages && Vue.config.progressiveImages.pageLoaded) {
      this.updateSrc()
    }
  },
  mounted () {
    // Check Blur
    if (typeof (this.blur * 1) === 'number') { this.blurValue = this.blur * 1 }
    if (typeof (this.animate * 1) === 'number') { this.animateValue = this.animate * 1 }
    this.srcValue = this.src
    this.updateBlur()
    // Initiate Progressive Loading
    Vue.config.progressiveImages = Vue.config.progressiveImages ? Vue.config.progressiveImages : {}
    if (window.addEventListener) {
      windowEvent = window.addEventListener('load', () => {
        Vue.config.progressiveImages.pageLoaded = true
        this.updateSrc()
      }, false)
    }
  },
  destroy () {
    window.removeEventListener('load', windowEvent)
  }
}
</script>

<style>
</style>
