<script setup>
import {computed} from "vue";

let props = defineProps({
  image_url: String,
  crop_x: Number,
  crop_y: Number,
  width_length: Number,
  image_width: Number
})


let teaser_image_css_background_image_val = computed(() => `url("${props.image_url}")`)

</script>

<template>
  <div id="teaser" :style="{'background-image':teaser_image_css_background_image_val}"/>
</template>

<style scoped>

div#teaser {
  background-size: v-bind(image_width+ "px");
  background-repeat: no-repeat;
  background-position-x: v-bind(-crop_x+ "px");
  background-position-y: v-bind(-crop_y+ "px");
  height: v-bind(width_length+ "px");
  width: v-bind(width_length+ "px");
  border: 1px black solid;
  border-radius: 20px;
}

</style>