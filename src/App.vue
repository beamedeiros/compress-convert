<template>
  <div>
    <compress-image/>
    <br/>
    <compress-and-convert/>
    <br/>
    <convert-image/>
    <br/>
    <image-quality/>
  </div>
</template>

<script>
import CompressImage from './components/CompressImage.vue';
import CompressAndConvert from './components/CompressAndConvert.vue';
import ConvertImage from './components/ConvertImage.vue'
import ImageQuality from './components/ConvertImageQuality.vue'
export default {
  name:'App',
  components: {
    CompressImage,
    CompressAndConvert,
    ConvertImage,
    ImageQuality
  }
}
</script>
