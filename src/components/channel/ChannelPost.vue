<template>
  <div
    class="container post-card"
    style="padding: 0"
    role="button"
    @click="goToWatch"
    v-cloak
  >
    <div class="card non-border" style="background-color: transparent">
      <ThumbnailVideo :video="video" />
      <div class="card-text non-border">
        <PostCaption
          :video="video"
          :showViews="true"
          :showCreatedDate="true"
          className="mt-4"
        />
      </div>
    </div>
  </div>
</template>
<script>
import PostCaption from "../home/PostCaption.vue";
import ThumbnailVideo from "../common/ThumbnailVideo.vue";
export default {
  components: {
    PostCaption,
    ThumbnailVideo,
  },
  props: ["video"],
  data() {
    return {
      src: "",
      attrs: {
        class: "mb-6",
      },
    };
  },
  methods: {
    goToWatch: function (e) {
      this.$router.push("/watch/" + this.video._id).catch(() => {});
    },
  },
  watch: {
    video(val) {
      this.duration = val.duration;
    },
  },
  created: function () {
    this.src = this.video.thumbnail_url;
  },
};
</script>
<style src="src/assets/styles/post.css">
</style>