<template>
  <div class="bg-secondary-color comment-view">
    <div v-if="!loading && noVideoFound">
      <UnavailableVideo :message="unavailableVideoMessage" />
    </div>
    <div class="row comment-view" v-else-if="!loading">
      <PlayerWrapper :video="video" :onPlay="increaseView" />
      <CommentWrapper :video="video" />
    </div>
  </div>
</template>
<script>
import PlayerWrapper from "../components/video/video-watch/player/PlayerWrapper.vue";
import CommentWrapper from "../components/video/video-watch/comment/CommentWrapper.vue";
import UnavailableVideo from "../components/video/video-watch/UnavailableVideo.vue";
import { RepositoryFactory } from "../utils/repository/RepositoryFactory";
import { convertJSONToObject } from "../utils/utils";
import { increaseViewRequest } from "../utils/http/videoRequest";
const VideoRepository = RepositoryFactory.get("video");

import $ from "jquery";
export default {
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      vm.prevRoute = from;
    });
  },
  components: {
    PlayerWrapper,
    CommentWrapper,
    UnavailableVideo,
  },
  data() {
    return {
      loading: true,
      noVideoFound: false,
      unavailableVideoMessage: false,
      video: {},
      canIncreaseView: true,
    };
  },
  methods: {
    async getVideo(id) {
      this.loading = true;
      try {
        const { data } = await VideoRepository.getVideoById(id);
        if (data) {
          const dataObject = convertJSONToObject(data);
          if (!dataObject.details) {
            if (dataObject.url) {
              this.loading = false;
              return dataObject;
            }
          }
        }
        return null;
      } catch (error) {
        if (error.response?.status === 400) {
          this.noVideoFound = true;
          this.loading = false;
          this.unavailableVideoMessage = error.response.data.message;
        }
      }
    },
    async increaseView() {
      if (this.canIncreaseView) {
        this.canIncreaseView = false;
        setTimeout(() => {
          increaseViewRequest({
            videoId: this.video._id,
            viewerId: JSON.parse(localStorage.getItem("user"))._id,
          });
        }, this.video.duration / 10);
      }
    },
  },
  mounted() {
    $(".button").mouseenter(function (i, obj) {
      $(this).find("circle").css("fill", "white");
      $(this).find("path").css("fill", "#FE2C55");
    });
    $(".button").mouseleave(function (i, obj) {
      $(this).find("circle").css("fill", "#F1F1F2");
      $(this).find("path").css("fill", "black");
    });
  },
  async beforeRouteUpdate(to, from, next) {
    this.video = await this.getVideo(to.params.id);
    next();
  },
  async created() {
    this.video = await this.getVideo(this.$route.params.id);
    this.loading = false;
  },
};
</script>
<style src="src/assets/styles/comment.css"></style>
