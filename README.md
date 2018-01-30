# Vue-Uppy-Cloudinary
[![GitHub issues](https://img.shields.io/github/issues/christianfroseth/vue-uppy-cloudinary.svg)](https://github.com/christianfroseth/vue-uppy-cloudinary/issues)
[![GitHub license](https://img.shields.io/github/license/christianfroseth/vue-uppy-cloudinary.svg)](https://github.com/christianfroseth/vue-uppy-cloudinary/blob/master/LICENSE)

> A VueJs based uploader that utilizes Uppy.io and Cloudinary. 

> <img src="https://opencollective-production.s3-us-west-1.amazonaws.com/ca272d00-958a-11e7-990a-e919fb36989b.png" width="156" height="156" title="VueJS"/><img src="https://uppy.io/images/uppy-social.jpg" width="256" height="100" title="Uppy"/><img src="https://cloudinary-res.cloudinary.com/image/asset/dpr_2.0/logo-e0df892053afd966cc0bfe047ba93ca4.png" width="256" height="56" title="Cloudinary"/>

## [Demo](https://christianfroseth.github.io/vue-uppy-cloudinary/index.html)

## Install
```npm
npm install vue-uppy-cloudinary --save
```

## Use in existing component

#### Import VueUppyCloudinary component and CSS
```vuejs
import VueUppyCloudinary from 'vue-uppy-cloudinary';
import 'vue-uppy-cloudinary/dist/lib/vue-uppy-cloudinary.min.css'
```

#### Add to component 
```vuejs
 components: {
    VueUppyCloudinary,
  },
```

#### Include component in VueJS template
Sign up with Cloudinary for a free account. Input your cloud name and define a unsigned upload preset. The preset will define which transformations to apply to the uploaded image or video and the allowed filetypes and sizes.   
```vuejs
 <vue-uppy-cloudinary
      preset="[your-cloudinary-preset]"
      cloudName="[your-cloudinary-cloud-name]"
    />
```

##Complete component using vue-uppy-cloudinary
```vuejs
<template>
  <div id="app">
    <h1>Demo</h1>
    <vue-uppy-cloudinary
          preset="b0gbylpo"
          cloudName="lthekxbe9"
          @uploaded="uploaded"
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
    methods: {
      uploaded(data) {
        console.log(data);
      }
    },
  }
</script>
```

## Properties
| Property      | Description | Default | Required 
| ------------- |:-------------:| -----:| -----:|
| cloudName   | Cloudinary cloud name| N/A | Yes
| preset      | Unsigned upload preset |  N/A | Yes
| buttonText  | Upload button text| Upload| No
| minNumberOfFiles| Min number of selected files before upload is enabled| 1| No
| minNumberOfFiles| Maximum number of files to upload| 10 | No
| maxFileSize| Maximum files size | 40MB| No
| allowedFileTypes | Comma separated list of file types allowed to upload (client side validation)| /image/\*, video/\*| No
| tags| Array of tags to add as metadata to Cloudinary upload| [] | No
| autoProceed| Should upload start automatically after selecting photos/videos| false | No
| showProgressDetails  | Show progress information during upload | true | No
| closeModalOnClickOutside| Close modal dialog when clicking outside of frame | true | No
| buttonStyle | CSS class to add to the button | default style | No


## Events
 Event | Description 
 | ------------- |:-------------:
 | uploaded  | Fired when upload completes. Contains the data object returned from Cloudinary
 | upload-progress  | Fired during upload. Contains a standard XHR progress object

## Uploaded event
The uploaded event will contain data from Cloudinary such as image url (http and https), public_id, resource_type, etag, format and so on.
The payload will differ depending on your assigned upload preset
