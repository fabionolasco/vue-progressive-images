<template>
  <span>
    <img v-bind:src="srcUrl" v-bind:alt="alt" />
  </span>
</template>

<script>
import Vue from 'vue';

let windowEvent;
export default {
  name: 'fn-image',
  data() {
    return {
      srcUrl: this.src,
    };
  },
  props: ['src', 'alt'],
  methods: {
    updateSrc() {
      const dpi = window.devicePixelRatio;
      if (this.srcUrl && dpi && dpi >= 1) {
        this.srcUrl = this.srcUrl.replace('@0', `@${dpi.toString()[0]}`);
      }
    },
  },
  created() {
    if (Vue.config.fabioApp && Vue.config.fabioApp.pageLoaded) {
      this.updateSrc();
    }
  },
  mounted() {
    if (window.addEventListener) {
      windowEvent = window.addEventListener('load', () => {
        Vue.config.fabioApp.pageLoaded = true;
        this.updateSrc();
      }, false);
    }
    Vue.config.fabioApp = Vue.config.fabioApp ? Vue.config.fabioApp : {};
  },
  destroy() {
    window.removeEventListener('load', windowEvent);
  },
};
</script>

<style>
</style>
