<script setup lang="ts">
/* eslint-disable prettier/prettier */
import {reactive, ref, computed, onMounted} from "vue";
import {calculate_resized_dimensions, host_ip_server} from '../assets/constants.ts'
import FullImageWithHighlightOrColoredBorderForFeedback from '../components/FullImageWithHighlightOrColoredBorderForFeedback.vue'
import TeaserImage from '../components/TeaserImage.vue'
import {set_unique_timeout} from "../assets/constants.ts";
import axios from 'axios'

// play related
let last_choice_was_correct = ref(false)

// display variables
let image_container_height = ref(900)
let image_container_width = ref(1500)

// timing variables
let teaser_show_milliseconds = 10000
let full_picture_show_milliseconds = 10000
let feedback_show_milliseconds = 5000

// cropping
let crop_size_pct = .2
let crop_x_pct = ref(0 / 10)
let crop_y_pct = ref(0 / 10)
let do_crop_image = ref(true)

let status = ref("loading-sets")
let full_image = ref({
  "url": host_ip_server + "/image?url=https%3A//upload.wikimedia.org/wikipedia/commons/f/f4/WP_Robert_Smirke.jpg",
  "dimensions": {
    "height": 2083,
    "width": 1517
  },
  "name": "File:WP_Robert_Smirke.jpg"
})

let teaser_image = ref({
  "url": host_ip_server + "/image?url=https%3A//upload.wikimedia.org/wikipedia/commons/f/f4/WP_Robert_Smirke.jpg",
  "dimensions": {
    "height": 2083,
    "width": 1517
  },
  "name": "File:WP_Robert_Smirke.jpg"
})

// image datasets
let current_set: set
let all_sets: [set]
let preloaded_images: Record<string, HTMLImageElement> = {}


interface session_report_interface {
  user_choice: string,
  correct_choice: string,
  user_made_correct_choice: boolean,
  session_started_timestamp: number,
  feedback_given_timestamp: number,
  session_duration_ms: number
}

// session report
let session_report: session_report_interface = {}

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


load_new_image_sets()


function load_new_image_sets() {
  fetch(host_ip_server + "/similar_pics/set_two_apiece?similarity=0&per_set=3")
      .then((response) => response.json())
      .then((r) => {
        all_sets = r
        preload_next_20_images()
        status.value = "wait-next"
      });
}

/* preload the next 20 images to be shown */
function preload_next_20_images() {
  all_sets.slice(0, 20).forEach(set => {
        set["pics"].forEach(img => {
          let url_with_size = `${img['url']}&max_height=${image_container_height.value}&max_width=${image_container_width.value}&do_crop=${do_crop_image.value ? 'True' : 'False'}`
          if (Object.prototype.hasOwnProperty.call(preloaded_images, url_with_size)) {
            return
          }

          var image_object = new Image();
          image_object.src = url_with_size
          preloaded_images[img["url"]] = image_object
          //console.log("preloading " + img["url"])
        })

      }
  )
}

function flash_full_image_then_teaser() {

  session_report = {
    session_started_timestamp: Date.now(),
    feedback_given_timestamp: -1,
    user_choice: "",
    correct_choice: "",
    user_made_correct_choice: false,
    session_duration_ms: -1
  }

  console.log(session_report)
  preload_next_20_images()
  current_set = all_sets.shift()

  // mark set as seen
  fetch(host_ip_server + "/similar_pics/seen_set?id=" + current_set["_id"])
      .then((response) => response.json())
      .then((r) => {
        //console.log(JSON.stringify(r))
      });

  status.value = "interlude"

  full_image.value = current_set["pics"][Math.floor(current_set["pics"].length * Math.random())]
  teaser_image.value = Math.random() > 0.5 ? full_image.value : current_set["pics"].filter(image => image["url"] !== full_image.value["url"])[Math.floor((current_set["pics"].length - 1) * Math.random())]

  console.log("full image:" + full_image.value["url"])
  console.log("teaser image:" + teaser_image.value["url"])

  // select crop of image to show in teaser
  crop_x_pct.value = Math.random()
  crop_y_pct.value = Math.random()


// now full image will be shown, after a while hide it
  set_unique_timeout(() => {
    status.value = "interlude";

    // a while after full image is hidden show teaser
    set_unique_timeout(() => {
      status.value = "teaser"

      // a while after teaser was shown hide it and wait for user input
      set_unique_timeout(() => {
        status.value = "teaser-wait-input"

      }, teaser_show_milliseconds)

    }, 200)


  }, full_picture_show_milliseconds)


  status.value = "full"
  setTimeout(() => {
    delete preloaded_images[full_image.value['url']]
    delete preloaded_images[teaser_image.value['url']]
    console.log("Preloaded images: " + Object.keys(preloaded_images).length)
  }, 100)
}

// calculated dimensions
// these get overwritten soon
let teaser_image_actual_width = ref(teaser_image.value.dimensions.width)
let teaser_image_actual_height = ref(teaser_image.value.dimensions.height)

let full_image_actual_width = ref(full_image.value.dimensions.width)
let full_image_actual_height = ref(full_image.value.dimensions.height)


let crop_width_length_px = computed(() => {
  return Math.min(image_container_width.value, image_container_height.value) * crop_size_pct
})

let highlight_border_thickness_px = computed(() => {
  return image_container_width.value * 0.02
})


setInterval(() => {
  do_crop_image.value = window.matchMedia("(max-width:800px)").matches;

  image_container_width.value = document.querySelector("#image-wrapper").clientWidth
  image_container_height.value = document.querySelector("#image-wrapper").clientHeight

  let teaser_image_resized_dimensions = do_crop_image.value ? {
    height: Math.min(teaser_image.value.dimensions.height, image_container_height.value),
    width: Math.min(teaser_image.value.dimensions.width, image_container_width.value)
  } : calculate_resized_dimensions(teaser_image.value.dimensions.height, teaser_image.value.dimensions.width, image_container_height.value, image_container_width.value)

  teaser_image_actual_height.value = teaser_image_resized_dimensions.height
  teaser_image_actual_width.value = teaser_image_resized_dimensions.width

  let full_image_resized_dimensions = do_crop_image.value ? {
    height: Math.min(full_image.value.dimensions.height, image_container_height.value),
    width: Math.min(full_image.value.dimensions.width, image_container_width.value)
  } : calculate_resized_dimensions(full_image.value.dimensions.height, full_image.value.dimensions.width, image_container_height.value, image_container_width.value)

  full_image_actual_height.value = full_image_resized_dimensions.height
  full_image_actual_width.value = full_image_resized_dimensions.width

  preload_next_20_images()
}, 1000)


// USER INTERACTION
function start_session() {
  status.value = "interlude"
  flash_full_image_then_teaser()
}

function skip_session() {
  flash_full_image_then_teaser()
}

function vote_whether_pics_match(choice: string) {
  session_report.feedback_given_timestamp = Date.now()
  session_report.session_duration_ms = session_report.feedback_given_timestamp - session_report.session_started_timestamp
  session_report.user_choice = choice
  session_report.correct_choice = teaser_image.value["url"] === full_image.value["url"] ? "same" : "different"
  session_report.user_made_correct_choice = session_report.correct_choice === session_report.user_choice
  last_choice_was_correct.value = session_report.user_made_correct_choice
  status.value = "full-feedback"

  set_unique_timeout(() => {
    status.value = "interlude"

    set_unique_timeout(() => {
      flash_full_image_then_teaser()

    }, 200)

  }, feedback_show_milliseconds)
  send_session_report()

}

function send_session_report() {

  axios.post(host_ip_server + "/session_report", {
        game_variant: "similar_pics_only_one_teaser",
        images: {
          teaser_image: Object.assign(teaser_image.value, {
            actual_dimensions: {
              height: teaser_image_actual_height.value,
              width: teaser_image_actual_width.value
            },
            crop: {
              x_px: get_crop_px(crop_x_pct.value, teaser_image_actual_width.value), // from top left
              y_px: get_crop_px(crop_y_pct.value, teaser_image_actual_height.value)

            }
          }),
          full_image: Object.assign(full_image.value, {
            actual_dimensions: {
              height: full_image_actual_height.value,
              width: full_image_actual_width.value
            },
            crop: {
              x_px: get_crop_px(crop_x_pct.value, full_image_actual_width.value),
              y_px: get_crop_px(crop_y_pct.value, full_image_actual_height.value)

            }
          }),
        },
        image_cropped_to_fill_container: do_crop_image.value,
        max_height: image_container_height.value,
        max_width: image_container_width.value,
        timing: {
          teaser_show_ms: teaser_show_milliseconds,
          feedback_show_ms: feedback_show_milliseconds,
          full_picture_show_ms: full_picture_show_milliseconds
        },
        performance: session_report,
        viewport_width: window.innerWidth, // if less than 800, assumes on mobile and crops image to fit container / screen (mostly)
        crop: {
          size_percentage_of_container: crop_size_pct,
          crop_x_px: crop_x_pct.value,
          crop_y_px: crop_y_pct.value

        }
      }
  ).then(r => {
    console.log(r.data)
  }).catch(e => {
    console.log(e)
  })
}

function seed_is_cool() {
  fetch(host_ip_server + "/similar_pics/seed_is_cool?id=" + current_set["seed"])
      .then((response) => response.json())
      .then(r => {
        console.log("seed is cool!")
        console.log(r)
      });
}

function seed_is_bad() {
  fetch(host_ip_server + "/similar_pics/seed_is_bad?id=" + current_set["seed"])
      .then((response) => response.json())
      .then(r => {
        console.log("seed is cool!")
        console.log(r)
      });
}

document.addEventListener('keypress', (e) => {
      // user thinks pic is cool
      if (e.key === "a") {
        seed_is_cool()
      }
      if (e.key === "s") {
        skip_session()
      }
      if (e.key === "+") {
        make_everything_faster()

      }
      if (e.key === "-") {
        make_everything_slower()
      }
      // user is voting whether images match:
      if (status.value === "teaser" || status.value === "teaser-wait-input") {

        if (e.key === "f" || e.key === "d") {
          vote_whether_pics_match(e.key === "f" ? "same" : "different")
        }

      }

      // user wants to start a new session
      if (status.value === "wait-next") {
        if (e.key === " " || e.key === "+") {
          start_session()
        }
      }
    }
)

function get_crop_px(pct: number, dimension: number) {
  return pct * (dimension - crop_width_length_px.value - highlight_border_thickness_px.value)
}

function log_timings() {
  console.log("teaser show ms:", teaser_show_milliseconds)
  console.log("full picture show ms:", full_picture_show_milliseconds)
  console.log("feedback show ms:", feedback_show_milliseconds)

}

function make_everything_faster() {
  teaser_show_milliseconds *= 0.9
  full_picture_show_milliseconds *= 0.9
  //feedback_show_milliseconds *= 0.9
  log_timings()
}

function make_everything_slower() {
  teaser_show_milliseconds *= 1.1
  full_picture_show_milliseconds *= 1.1
  //feedback_show_milliseconds *= 1.1

  log_timings()
}

</script>
<template>
  <br><br>
  <div id="top-buttons" class="only-mobile">
    <button @click="make_everything_faster">+</button>&nbsp;&nbsp;&nbsp;&nbsp;
    <button @click="make_everything_slower">-</button>&nbsp;&nbsp;&nbsp;<br></div>
  <div id="image-wrapper">
    <div id="wait-next-message" v-if="status==='loading-sets'">Loading image sets...</div>
    <div id="wait-next-message" v-if="status==='wait-next'">Press space to start next session</div>
    <TeaserImage v-if="status==='teaser'"
                 :image_url="`${teaser_image['url']}&max_height=${image_container_height}&max_width=${image_container_width}&do_crop=${do_crop_image?'True':'False'}`"
                 :width_length="crop_width_length_px"
                 :image_width="teaser_image_actual_width"
                 :crop_x="get_crop_px(crop_x_pct,teaser_image_actual_width)"
                 :crop_y="get_crop_px(crop_y_pct,teaser_image_actual_height)"/>

    <FullImageWithHighlightOrColoredBorderForFeedback v-if="status==='full' || status==='full-feedback'"
                                                      :show_highlight='teaser_image["url"] === full_image["url"] && status==="full-feedback"'
                                                      :highlight_x="get_crop_px(crop_x_pct,full_image_actual_width)"
                                                      :highlight_y="get_crop_px(crop_y_pct,full_image_actual_height)"
                                                      :url="`${full_image['url']}&max_height=${image_container_height}&max_width=${image_container_width}&do_crop=${do_crop_image?'True':'False'}`"
                                                      :indicate_success="last_choice_was_correct"
                                                      :max_width="image_container_width"
                                                      :max_height="image_container_height"
                                                      :show_success_indicator="status==='full-feedback'"
                                                      :teaser_width_length="crop_width_length_px"
                                                      :highlight_border_thickness="highlight_border_thickness_px"/>
  </div>
  <br><br><br><br>
  <div id="bottom-bar" class="only-mobile">
    <div id="buttons">
      <button v-if="status==='wait-next'" @click="start_session">Start session</button>
      <button v-if="status!=='wait-next'" @click="seed_is_bad">Bad</button>
      <button v-if="status!=='wait-next'" @click="seed_is_cool">Cool</button>
      <button v-if='status.startsWith("teaser")' @click="skip_session">Skip</button>
      <button v-if='status.startsWith("teaser")' @click="()=>{vote_whether_pics_match('different')}">Diff</button>
      <button v-if='status.startsWith("teaser")' @click="()=>{vote_whether_pics_match('same')}">Same</button>

    </div>
  </div>
</template>

<style>

div#image-wrapper {
  margin-top: 30px;
  position: relative;
  width: 95vw;
  height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-width: 800px) {
  .only-mobile {
    display: none !important;
  }

}

div#bottom-bar {
  width: 100%;
  height: 100px;
  position: fixed;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: end;
  background-color: lightcyan;
}

div#buttons {
  height: 100px;
  width:100%;
  padding-right: 20%;
  display: flex;
  justify-content: end;
}

div#buttons button {
  margin:0 10px 0 10px;
}

</style>
