<template>
  <button
    :class='[buttonStyle]'
    v-on:click='openUploader()'
  >{{buttonText}}
  </button>
</template>

<script>
  import Uppy from 'uppy/lib/core';
  import Dashboard from 'uppy/lib/plugins/Dashboard';
  import Webcam from 'uppy/lib/plugins/Webcam';
  /* eslint-disable no-unused-vars */
  import 'uppy/dist/uppy.min.css';

  const BASE_URL = 'https://api.cloudinary.com/v1_1/';
  const IMAGE_POSTFIX = '/image/upload';
  const VIDEO_POSTFIX = '/video/upload';

  export default {
    props: {
      cloudName: {
        type: String,
        required: true,
      },
      preset: {
        type: String,
        required: true,
      },
      buttonText: {
        type: String,
        default: 'Upload',
      },
      minNumberOfFiles: {
        type: Number,
        default: 1,
      },
      maxNumberOfFiles: {
        type: Number,
        default: 10,
      },
      maxFileSize: {
        type: Number,
        default: 40000000,
      },
      allowedFileTypes: {
        type: Array,
        default() {
          return ['image/*', 'video/*'];
        },
      },
      tags: {
        type: Array,
        default() {
          return [];
        },
      },
      autoProceed: {
        type: Boolean,
        default: false,
      },
      showProgressDetails: {
        type: Boolean,
        default: true,
      },
      closeModalOnClickOutside: {
        type: Boolean,
        default: true,
      },
      buttonStyle: {
        type: String,
        default: 'uploader',
      },
    },
    methods: {
      // Opens the upload dialog
      openUploader() {
        this.uppy.getPlugin('Dashboard').openModal();
      },
      // Uploads a single file
      uploadFile(file) {
        const formData = new FormData();
        formData.append('file', file.data);
        formData.append('upload_preset', this.preset);
        formData.append('tags', this.tags);
        const xhr = new XMLHttpRequest();

        xhr.upload.addEventListener('loadstart', () => {
          this.uppy.emit('upload-started', file.id);
        });

        xhr.addEventListener('load', (ev) => {
          if (ev.target.status === 200) {
            this.$emit('uploaded', JSON.parse(ev.target.response));
            this.uppy.emit('upload-success', file.id);
          } else {
            this.uppy.emit('upload-error', file.id, new Error('Failed to upload'));
          }
        });

        xhr.addEventListener('error', () => {
          this.uppy.emit('upload-error', file.id, new Error('Failed to upload'));
        });

        xhr.upload.addEventListener('progress', (ev) => {
          if (ev.lengthComputable) {
            const progressData = {
              uploader: this,
              id: file.id,
              bytesUploaded: ev.loaded,
              bytesTotal: ev.total,
            };
            this.uppy.emit('upload-progress', progressData);
            this.$emit('upload-progress', progressData);
          }
        });

        if (file.type.startsWith('video')) {
          xhr.open('POST', `${BASE_URL}${this.cloudName}${VIDEO_POSTFIX}`);
        } else {
          xhr.open('POST', `${BASE_URL}${this.cloudName}${IMAGE_POSTFIX}`);
        }
        xhr.send(formData);
      },
      // Create an instance of Uppy.io
      createUppyInstance() {
        return Uppy({
          autoProceed: this.autoProceed,
          restrictions: {
            maxFileSize: this.maxFileSize,
            maxNumberOfFiles: this.maxNumberOfFiles,
            minNumberOfFiles: this.minNumberOfFiles,
            allowedFileTypes: this.allowedFileTypes,
          },
        })
          .use(Dashboard, {
            target: 'body',
            showProgressDetails: this.showProgressDetails,
            closeModalOnClickOutside: this.closeModalOnClickOutside,
          })
          .use(Webcam, {
            target: Dashboard,
          })
          .run();
      },
      // Complete handler for Uppy
      completeHandler(result) {
        result.successful.forEach((file) => {
          this.uploadFile(file);
        });
      },
    },
    mounted() {
      this.uppy = this.createUppyInstance();
      this.uppy.on('complete', this.completeHandler);
    },
    data() {
      return {
        uppy: {},
      };
    },
  };
</script>

<style scoped>
  .uploader {
    text-decoration: none;
    color: #fff;
    background-color: #26a69a;
    text-align: center;
    letter-spacing: 0.5px;
    -webkit-transition: 0.2s ease-out;
    transition: 0.2s ease-out;
    cursor: pointer;
    font-size: 1rem;
    outline: 0;
    border: none;
    border-radius: 2px;
    display: inline-block;
    height: 36px;
    line-height: 36px;
    padding: 0 2rem;
    text-transform: uppercase;
    vertical-align: middle;
    -webkit-tap-highlight-color: transparent;
  }
</style>
