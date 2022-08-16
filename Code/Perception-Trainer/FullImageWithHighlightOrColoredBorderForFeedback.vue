<script setup>
import {onMounted, ref, watch, computed} from "vue";

let props = defineProps({
  url: String,
  show_highlight: Boolean,
  highlight_x: Number,
  highlight_y: Number,
  max_height: Number,
  max_width: Number,
  indicate_success: Boolean,
  show_success_indicator: Boolean,
  teaser_width_length: Number,
})

let success_indicator_color = computed(() => props.indicate_success ? 'green' : 'red')

</script>
<template>
  <div id="wrapper">
    <div id="success_indicator_wrapper" :class="{'success_indicator_active':show_success_indicator}">
      <div id="highlight" v-if="show_highlight===true"></div>
      <img id="full-image-img" :src="url"/></div>
  </div>

</template>
<style scoped>

div#wrapper {
  width: v-bind(max_width+ "px");
  height: v-bind(max_height+ "px");
  display: flex;
  justify-content: center;
  align-items: center;
}

#success_indicator_wrapper {
  position: relative;
  max-width: v-bind(max_width+ "px");
  max-height: v-bind(max_height+ "px");
}

#success_indicator_wrapper.success_indicator_active {
  box-shadow: v-bind(success_indicator_color+ " inset 0 0 " +max_width/15 + "px");
}

img#full-image-img {
  position: relative;
  object-fit: scale-down;
  max-height: 100%;
  max-width: 100%;
  z-index: -2;
}


div#highlight {
  position: absolute;
  border: 10px solid white;
  height: v-bind(teaser_width_length+ "px");
  width: v-bind(teaser_width_length+ "px");
  left: v-bind(highlight_x+ "px");
  top: v-bind(highlight_y+ "px");
  border-radius: 10px;
  mix-blend-mode: difference;


}

</style>