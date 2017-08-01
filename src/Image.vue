
<template>
  <span>
    <img 
      v-bind:src="srcValue"
      v-bind:alt="alt"
      v-bind:style="styles" />
  </span>
</template>

<script>
import Vue from 'vue';

export default {
  name: 'fn-image',
  props: ['src', 'alt', 'blur', 'animate', 'maxRatio'],
  data() {
    return {
      srcValue: this.src,
      styles: {},
    };
  },
  methods: {
    updateSrc() {
      let dpi = window.devicePixelRatio || 1;
      if (this.srcValue && dpi && dpi >= 1) {
        if (typeof this.maxRatio === 'number' && dpi > this.maxRatio) {
          dpi = this.maxRatio;
        }
        this.srcValue = this.srcValue.replace('@0', `@${dpi.toString()[0]}`);
        if (this.blur) {
         setTimeout(() => { this.updateBlur(0); }, 0);
        }
      }
    },
    updateBlur(x) {
      this.styles.filter = `blur(${x}px)`;
      this.styles['-webkit-filter'] = `blur(${x}px)`;
      this.styles['-moz-filter'] = `blur(${x}px)`;
      this.styles['-o-filter'] = `blur(${x}px)`;
      this.styles['-ms-filter'] = `blur(${x}px)`;
    }
  },
  created() {
    // Init Styles
    let blurValue = typeof (this.blur * 1) === 'number' ? this.blur * 1 : 0;
    let animateValue = typeof (this.animate * 1) === 'number' ? this.animate * 1 : 0;
    if (this.blur) {
      this.styles = {
        filter: `blur(${blurValue}px)`,
        '-webkit-filter': `blur(${blurValue}px)`,
        '-moz-filter': `blur(${blurValue}px)`,
        '-o-filter': `blur(${blurValue}px)`,
        '-ms-filter': `blur(${blurValue}px)`,
        transition: `${animateValue}s filter linear`,
        '-webkit-transition': `${animateValue}s -webkit-filter linear`,
        '-moz-transition': `${animateValue}s -moz-filter linear`,
        '-ms-transition': `${animateValue}s -ms-filter linear`,
        '-o-transition': `${animateValue}s -o-filter linear`,
      };
    }
    // Cause image to show again
    this.windowEvent = false;
    if (Vue.config.progressiveImages && Vue.config.progressiveImages.pageLoaded) {
      this.updateSrc();
    }
  },
  mounted() {
    // Initiate progressive loading after all content is rendered
    Vue.config.progressiveImages = Vue.config.progressiveImages ? Vue.config.progressiveImages : {};
    if (window.addEventListener) {
      this.windowEvent = () => {
        Vue.config.progressiveImages.pageLoaded = true;
        this.updateSrc();
      }
      window.addEventListener('load', this.windowEvent, false);
    }
  },
  destroy() {
    window.removeEventListener('load', this.windowEvent);
  },
};
</script>
