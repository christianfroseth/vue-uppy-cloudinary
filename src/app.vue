<template>
  <div id="app">
    <h1>Demo</h1>
    <uppy-cloudinary-uploader
      preset="b0gbylpo"
      cloudName="lthekxbe9"
    />
  </div>
</template>

<script>
import UppyCloudinaryUploader from './components/UppyCloudinaryUploader';

export default {
  name: 'app',
  components: {
    UppyCloudinaryUploader,
  },
};
</script>
