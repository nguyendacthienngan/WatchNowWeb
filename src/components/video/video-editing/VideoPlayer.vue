<template>
  <div class="top-border container">
    <br />
    <div class="row">
      <div class="col align-self-center trim">
        <div id="scissors">
          <img :src="require('src/assets/images/scissors.svg')" height="25px" />
          <span class="p-2"></span>
          <span>Trim</span>
        </div>
      </div>
      <div id="video-controls" class="container col-lg order-lg-2 order-2">
        <div class="row">
          <img
            :src="require('src/assets/images/quick-restart.svg')"
            class="
              quickRestart
              rotate180
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="30px"
            height="30px"
          />
          <img
            :src="require('src/assets/images/rewind-button.svg')"
            class="
              rewind
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="30px"
            height="30px"
          />
          <img
            :src="require('src/assets/images/play-button-arrowhead.svg')"
            class="
              btnPlayPause
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="50px"
            height="50px"
          />
          <img
            :src="require('src/assets/images/pause.svg')"
            class="
              btnPause
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="50px"
            height="50px"
          />
          <img
            :src="require('src/assets/images/forward-button.svg')"
            class="
              forward
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="30px"
            height="30px"
          />
          <img
            :src="require('src/assets/images/skip-track.svg')"
            class="
              skipTrack
              video-player
              col
              row
              justify-content-center
              align-self-center
            "
            width="30px"
            height="30px"
          />
        </div>
      </div>
      <div class="col container order-lg-3 order-1">
        <div id="" class="row justify-content-center align-self-center">
          <span class="current-time">00:00:00</span>/
          <span class="duration">00:00:00</span>
        </div>
      </div>
    </div>

    <Timeline></Timeline>
  </div>
</template>
<script>
import interact from "interactjs";
import Timeline from "./Timeline.vue";
export default {
  components: {
    Timeline,
  },
  mounted() {
    interact(".resize-drag")
      .resizable({
        // resize from all edges and corners
        edges: { left: true, right: true, bottom: false, top: false },

        listeners: {
          move(event) {
            const target = event.target;
            let x = parseFloat(target.getAttribute("data-x")) || 0;
            let y = parseFloat(target.getAttribute("data-y")) || 0;

            // update the element's style
            target.style.width = event.rect.width + "px";
            target.style.height = event.rect.height + "px";

            // translate when resizing from top or left edges
            x += event.deltaRect.left;
            y += event.deltaRect.top;

            target.style.transform = "translate(" + x + "px," + y + "px)";

            target.setAttribute("data-x", x);
            target.setAttribute("data-y", y);
            target.textContent =
              Math.round(event.rect.width) +
              "\u00D7" +
              Math.round(event.rect.height);
          },
        },
        modifiers: [
          // keep the edges inside the parent
          interact.modifiers.restrictEdges({
            outer: "parent",
          }),

          // minimum size
          interact.modifiers.restrictSize({
            min: { width: 100, height: 50 },
          }),
        ],

        inertia: true,
      })
      .draggable({
        listeners: { move: window.dragMoveListener },
        inertia: true,
        modifiers: [
          interact.modifiers.restrictRect({
            restriction: "parent",
            endOnly: true,
          }),
        ],
      });
  },
};
</script>
<style src="src/assets/styles/timeline.css">
</style>