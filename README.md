# Vue-Uppy-Cloudinary
[![GitHub issues](https://img.shields.io/github/issues/christianfroseth/vue-uppy-cloudinary.svg)](https://github.com/christianfroseth/vue-uppy-cloudinary/issues)
[![GitHub license](https://img.shields.io/github/license/christianfroseth/vue-uppy-cloudinary.svg)](https://github.com/christianfroseth/vue-uppy-cloudinary/blob/master/LICENSE)
> A VueJs based uploader that utilizes Uppy.io and Cloudinary

## Install
```npm
npm install vue-uppy-cloudinary --save
```

```vuejs
<template>
  <div id="app">
    <h1>Demo</h1>
    <uppy-cloudinary
      preset="[your-cloudinary-preset]"
      cloudName="[your-cloudinary-cloud-name]"
    />
  </div>
</template>

<script>
import VueUppyCloudinary from 'vue-uppy-cloudinary';
import 'vue-uppy-cloudinary/dist/lib/vue-uppy-cloudinary.min.css'

export default {
  components: {
    VueUppyCloudinary,
  },
};
</script>
```

