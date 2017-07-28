# VUE-PROGRESSIVE-IMAGES

NPM package for Vue.JS: 

Super light weight script that provides a way to offer progressive image loading in Vue Apps.

The script will check the browser's screen resolution and then update the SRC property of the IMG tag.

The intended usage is for you to create images as follows:

- photo@0x.png >>> A very small iamges. I use 2 colors, but you might want to use 16.

- photo@1x.png >>> Normal image.

- photo@2x.png >>> For displays with aspect ratio 2.

- photo@3x.png >>> For displays with aspect ratio 3.

You can use any file extension: PNG, JPG, GIF, etc.

The first time the script runs it will wait until all resources are loaded on browser to then perform the update on IMG tags.

For better results, you can combine it with a local or server batch image processing to create those image automatically.

For a manual process you can use Photoshop and the plugin "Retinize It".


## Compatibility

This script is compatible with IE 9+. However, on IE10 and older it will only fallback to the basic image @1x.


## How to use?

```html
<template>
  <div>
    <Image class="photo" src="static/photo@0x.png" alt="My alt text"></Image>
  </div>
</template>

<script>
import Image from './_Image';

export default {
  name: 'my-header',
  components: { Image }
};
</script>

<style>
.photo img {
  width: 200px;
}
</style>
```

## Collaboration

Collaboration is welcomed. Please send your pull request.

## Current Authors

- Fabio Nolasco
