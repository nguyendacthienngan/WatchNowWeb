<template>
  <div :class="`d-flex ${className}`">
    <div class="avatar" v-if="showAvatar" @click="goToChannel">
      <v-img
        :src="avatarSrc"
        width="42px"
        height="42px"
        class="img-responsive rounded-circle"
      />
    </div>
    <div>
      <div class="row">
        <div
          class="col d-flex flex-column"
          style="padding-bottom: 0; padding-top: 0"
        >
          <b class="ellipsis-2 mb-1">{{ video.title }}</b>
          <div class="video-info">
            <div
              v-if="showChannelName"
              @click="goToChannel"
              class="caption-title"
              role="button"
            >
              {{ channelUsername }}
            </div>
            <div>
              <span v-if="showViews"
                >{{ formatSuffixNumber(video.total_views) }} views</span
              >
              <span v-if="showCreatedDate">
                • {{ formatToChinaDate(video.created_at) }}</span
              >
            </div>
            <div v-if="showTags" class="d-flex flex-row gap-2">
              <span v-for="tag in video.tags" :key="tag">
                <v-chip>{{tag}}</v-chip>
              </span>
            </div>
          </div>
        </div>
        <div
          v-if="showDescription"
          :class="`ellipsis-${descriptionLines} mt-2`"
          :role="`${canReadMoreDescription ? 'button' : ''}`"
          @click="showDescriptionDialog"
        >
          <span>{{ video.description }}</span>
          <v-dialog
            v-model="isShowingDescriptionDialog"
            scrollable
            width="1000px"
            height="800px"
          >
            <v-card>
              <v-toolbar color="primary" dark>Description</v-toolbar>
              <v-card-text>
                <h5 class="pt-4 pl-4">{{ video.title }}</h5>
                <h6 class="pa-4 description" v-html="description"></h6>
              </v-card-text>
            </v-card>
          </v-dialog>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defaultAvatarUrl } from "src/constants/user";
import { formatToChinaDate } from "src/utils/date";
import { formatSuffixNumber } from "src/utils/number";
import linkifyHtml from "linkify-html";

export default {
  props: {
    video: {
      type: Object,
    },
    showAvatar: {
      type: Boolean,
      default: false,
    },
    showChannelName: {
      type: Boolean,
      default: false,
    },
    showViews: {
      type: Boolean,
      default: false,
    },
    showDescription: {
      type: Boolean,
      default: false,
    },
    descriptionLines: {
      type: Number,
      default: 2,
    },
    showCreatedDate: {
      type: Boolean,
      default: false,
    },
    showTags: {
      type: Boolean,
      default: false,
    },
    className: {
      type: String,
      default: "",
    },
    canReadMoreDescription: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      isShowingDescriptionDialog: false,
    };
  },
  computed: {
    avatarSrc() {
      if (!this.video.user) return defaultAvatarUrl;
      return this.video.user.avatar;
    },
    channelUsername() {
      return this.video.user?.username;
    },
    description() {
      return linkifyHtml(this.video.description);
    },
  },
  methods: {
    formatToChinaDate,
    formatSuffixNumber,
    showDescriptionDialog() {
      this.isShowingDescriptionDialog = true;
    },
    goToChannel(e) {
      e.stopPropagation();
      this.$router.push(`/channel/${this.video.user.channel_id}`);
    },
  },
};
</script>
<style src="src/assets/styles/post-caption.css">
</style>

<style>
.video-info {
  font-size: 14px;
  color: gray;
}
.caption-title {
  color: gray;
  display: inline;
}
.caption-title:hover {
  color: black;
}
.description {
  white-space: break-spaces;
  line-height: 30px;
  color: black;
}
.description a {
  color: blue !important;
}
</style>