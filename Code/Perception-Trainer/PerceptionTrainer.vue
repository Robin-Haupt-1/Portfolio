<script setup lang="ts">
import {reactive, ref, computed, onMounted} from "vue";
import {host_ip_server} from '../assets/constants.js'
import FullImageWithHighlightOrColoredBorderForFeedback from '../components/FullImageWithHighlightOrColoredBorderForFeedback.vue'
import TeaserImage from '../components/TeaserImage.vue'

// play related
let last_choice_was_correct = ref(false)

// display variables
let teaser_crop_x = ref(0);
let teaser_crop_y = ref(0);
let full_picture_max_height = ref(900)
let full_picture_max_width = ref(1500)

// timing variables
let teaser_width_length = 200
let teaser_show_milliseconds = 200
let full_picture_show_milliseconds = 2000
let feedback_show_milliseconds = 500
let current_timeout_id = -1


let status = ref("wait-next")
let full_image = ref()
let teaser_image = ref()

// image datasets
let current_set: set
let all_sets: [set]
let preloaded_images: Record<string, HTMLImageElement> = {}


interface pic {
  url: string
  dimensions: {
    width: number
    height: number
  },
  name: string
}

interface set {
  "_id": string
  "pics": pic[]
  "seed": number
  similarity: number
  hand: string
}


function set_unique_timeout(callback: CallableFunction, duration: number) {
  clearTimeout(current_timeout_id)
  current_timeout_id = setTimeout(callback, duration)
}

load_new_image_sets()


function load_new_image_sets() {
  fetch(host_ip_server + "/similar_pics_set_two_apiece?similarity=0&per_set=3")
      .then((response) => response.json())
      .then((r) => {
        all_sets = r
        preload_next_20_images()
      });
}

/* preload the next 20 images to be shown */
function preload_next_20_images() {
  all_sets.slice(0, 20).forEach(set => {
        set["pics"].forEach(img => {
          if (Object.prototype.hasOwnProperty.call(preloaded_images, img["url"])) {
            return
          }

          var image_object = new Image();
          image_object.src = img["url"];
          preloaded_images[img["url"]] = image_object
          console.log("preloading " + img["url"])
        })

      }
  )
}

function flash_full_image_then_teaser() {
  preload_next_20_images()
  current_set = all_sets.shift()

  // mark set as seen
  fetch(host_ip_server + "/seen_set?id=" + current_set["_id"])
      .then((response) => response.json())
      .then((r) => {
        console.log(JSON.stringify(r))
      });

  status.value = "interlude"

  full_image.value = current_set["pics"][Math.floor(current_set["pics"].length * Math.random())]
  teaser_image.value = Math.random() > 0.5 ? full_image.value : current_set["pics"].filter(image => image["url"] !== full_image.value["url"])[Math.floor((current_set["pics"].length - 1) * Math.random())]

  console.log("full image:" + full_image.value["url"])
  console.log("teaser image:" + teaser_image.value["url"])

  // select crop of image to show in teaser
  teaser_crop_x.value = Math.random() * ((teaser_image.value["dimensions"]["width"]) - teaser_width_length)
  teaser_crop_y.value = Math.random() * ((teaser_image.value["dimensions"]["height"]) - teaser_width_length)


// now full image will be shown, after a while hide it
  set_unique_timeout(() => {
    status.value = "interlude";

    // a while after full image is hidden show teaser
    set_unique_timeout(() => {
      status.value = "teaser"

      // a while after teaser was shown hide it and wait for user input
      set_unique_timeout(() => {
        status.value = "teaser-wait-input"

      }, full_picture_show_milliseconds)

    }, teaser_show_milliseconds)


  }, full_picture_show_milliseconds)


  status.value = "full"
  setTimeout(() => {

    delete preloaded_images[full_image.value['url']]
    delete preloaded_images[teaser_image.value['url']]

    console.log("Preloaded images: " + Object.keys(preloaded_images).length)

  }, 100)
}


document.addEventListener('keypress', (e) => {

      // user is voting whether images match:
      if (status.value === "teaser" || status.value === "teaser-wait-input") {

        if (e.key !== "f" && e.key !== "d" && e.key !== "6" && e.key !== "9") {
          return
        }

        status.value = "full-feedback"
        let choice = e.key === "f" || e.key === "9" ? "same" : "different"

        let correct_choice = teaser_image.value["url"] === full_image.value["url"] ? "same" : "different"
        last_choice_was_correct.value = choice === correct_choice

        // if (last_choice_was_correct.value) {
        //  if (full_picture_show_milliseconds > 200) {
        //    teaser_show_milliseconds *= 0.95
        //    full_picture_show_milliseconds *= 0.95
        //  }
        // } else {
        //  teaser_show_milliseconds += 10
        //  full_picture_show_milliseconds += 10
        // }
        // console.log("teaser_show_milliseconds:", teaser_show_milliseconds)
        // console.log("full_picture_show_milliseconds:", full_picture_show_milliseconds)

        set_unique_timeout(() => {
          status.value = "interlude"

          set_unique_timeout(() => {
            flash_full_image_then_teaser()

          }, 200)

        }, feedback_show_milliseconds)

      }

      // user wants to start a new session
      if (status.value === "wait-next") {
        if (e.key === " " || e.key === "+") {
          status.value = "interlude"
          flash_full_image_then_teaser()
        }
      }
    }
)
</script>
<template>
  <br><br>
  <div id="image-wrapper">
    <div id="wait-next-message" v-if="status==='wait-next'">Press space to start next session</div>

    <TeaserImage v-if="status==='teaser'" :image_url="teaser_image['url']" :width_length="teaser_width_length" :image_width="teaser_image['dimensions']['width']" :crop_x="teaser_crop_x"
                 :crop_y="teaser_crop_y"/>

    <FullImageWithHighlightOrColoredBorderForFeedback v-if="status==='full' || status==='full-feedback'" :show_highlight='teaser_image["url"] === full_image["url"] && status==="full-feedback"' :highlight_x="teaser_crop_x"
                                                      :highlight_y="teaser_crop_y"
                                                      :url="full_image['url']" :indicate_success="last_choice_was_correct" :max_width="full_picture_max_width"
                                                      :max_height="full_picture_max_height" :show_success_indicator="status==='full-feedback'" :teaser_width_length="teaser_width_length"/>
  </div>
</template>

<style>

div#image-wrapper {
  margin-top: 30px;
  position: relative;
  width: v-bind(full_picture_max_width+ "px");
  height: v-bind(full_picture_max_height+ "px");
  display: flex;
  align-items: center;
  justify-content: center;
}


</style>
