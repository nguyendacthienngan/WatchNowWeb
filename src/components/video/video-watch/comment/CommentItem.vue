<template>
  <div>
    <div class="d-flex">
      <div class="avatar">
        <img
          :src="avatarUrl"
          width="40px"
          height="40px"
          class="img-responsive rounded-circle"
          style="object-fit: cover"
        />
      </div>
      <div class="w-100">
        <div class="d-flex flex-row">
          <div class="pt-0 w-100 pr-4">
            <span class="channel-name">{{ comment.user.username }}</span>
            <br />
            {{ comment.content }}
            <br />
            <span class="subtitle">
              {{ formatToChinaDate(comment.created_at) }}
              &nbsp;&nbsp;&nbsp;&nbsp;
              <span
                v-if="showReplyButton"
                role="button"
                @click="
                  replyTo(comment._id, comment.content, comment.user.username)
                "
              >
                Reply
              </span>
              &nbsp;&nbsp;&nbsp;&nbsp;
              <b
                role="button"
                @click="viewMoreReplies(comment._id)"
                v-if="!comment.is_reply && comment.number_of_replies !== 0"
              >
                View more replies ({{ comment.number_of_replies }})
              </b>
            </span>
            <br />
          </div>
          <div
            class="
              d-flex
              flex-column
              justify-content-center
              align-items-center
              mr-4
            "
          >
            <v-menu offset-y v-if="showActionMenu">
              <template v-slot:activator="{ on, attrs }">
                <v-icon v-bind="attrs" v-on="on">mdi-dots-horizontal</v-icon>
              </template>
              <v-list>
                <v-list-item
                  v-for="item in filteredCommentMenuItems"
                  :key="item._id"
                  @click="item.action(comment)"
                >
                  <v-avatar color="#E4E6EB" size="35">
                    <span class="white--text">
                      <v-icon
                        color="black"
                        v-text="item.icon"
                        size="24"
                      ></v-icon>
                    </span>
                  </v-avatar>
                  <span style="width: 20px"></span>
                  <v-list-item-title class="lato">
                    {{ item.title }}
                  </v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>
            <svg
              @click="loveSVGFunction($event)"
              class="heart-icon"
              width="23"
              height="21"
              viewBox="0 0 23 21"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                @click="lovePathFunction($event)"
                class=""
                d="M10.5009 19.5312C11.0988 19.9514 11.9012 19.9514 12.4991 19.5312C15.4874 17.4309 18.349 15.0146 20.3285 11.9224C21.3203 10.3733 21.736 9.12052 21.8995 8.09611C21.9655 7.73541 22 7.36328 22 6.98286C22 3.67862 19.3954 1 16.1824 1C14.2623 1 12.5595 1.95663 11.5 3.43174C10.4405 1.95663 8.73766 1 6.81757 1C3.60461 1 1 3.67862 1 6.98286C1 7.36328 1.03452 7.73541 1.10053 8.09611C1.26403 9.12052 1.67972 10.3733 2.67145 11.9224C4.65101 15.0146 7.51264 17.4309 10.5009 19.5312Z"
                fill="white"
                stroke="black"
                stroke-opacity="0.5"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="fetchingReplies"
      style="width: 100%"
      class="d-flex flex-row justify-content-center"
    >
      <v-progress-circular indeterminate color="primary"></v-progress-circular>
    </div>
    <br />
    <div style="padding-left: 50px" v-for="reply in replies" :key="reply._id">
      <CommentItem :video="video" :comment="reply" v-if="!comment.is_reply" />
    </div>
  </div>
</template>

<script>
import { storeToRefs } from "pinia";
import $ from "jquery";
import { useCommentStore } from "src/store/comment";
import { defaultAvatarUrl } from "src/constants/user";
import { formatToChinaDate } from "src/utils/date";
import {
  getCommentRepliesRequest,
  removeCommentRequest,
} from "src/utils/http/commentRequest";
import { hideCommentFromChannelRequest } from "src/utils/http/userRequest";

export default {
  name: "CommentItem",
  setup() {
    const commentStore = useCommentStore();
    const { repliedComment } = storeToRefs(commentStore);
    const { replyTo } = commentStore;
    return { repliedComment, replyTo };
  },
  props: ["comment", "video"],
  data() {
    return {
      userInfo: JSON.parse(localStorage.getItem("user")),
      fetchingReplies: false,
      replies: null,
      commentMenuItems: [
        {
          title: "Remove",
          icon: "mdi-delete",
          action: async (comment) => {
            const isDelete = confirm("Do you want to delete");
            if (!isDelete) return;
            await removeCommentRequest(comment._id);
            if (this.$parent.comment) {
              this.$parent.viewMoreReplies();
            } else {
              this.$parent.fetchComments();
            }
          },
        },
        {
          title: "Hide from channel",
          icon: "mdi-block-helper",
          action: async (comment) => {
            await hideCommentFromChannelRequest(comment.author_id);
            if (this.$parent.comment) {
              this.$parent.viewMoreReplies();
            } else {
              this.$parent.fetchComments();
            }
          },
        },
      ],
    };
  },
  computed: {
    avatarUrl() {
      return this.comment.user.avatar ?? defaultAvatarUrl;
    },
    showActionMenu() {
      return this.filteredCommentMenuItems.length !== 0;
    },
    showReplyButton() {
      if (!this.userInfo || !this.userInfo._id) return false;
      return !this.comment.is_reply;
    },
    filteredCommentMenuItems() {
      const items = [];
      if (this.comment.user?._id === this.userInfo?._id)
        items.push(this.commentMenuItems[0]);
      if (
        !!this.userInfo &&
        this.video?.user.channel_id === this.userInfo?.channelId
      ) {
        items.push(this.commentMenuItems[1]);
      }
      return items;
    },
  },
  methods: {
    formatToChinaDate,
    loveSVGFunction(e) {
      e.currentTarget.classList.toggle("animate");
    },
    lovePathFunction(e) {
      e.currentTarget.classList.toggle("active");
    },
    async viewMoreReplies() {
      this.fetchingReplies = true;
      setTimeout(async () => {
        this.replies = await getCommentRepliesRequest(this.comment._id);
        this.fetchingReplies = false;
      }, 500);
    },
  },
  mounted() {
    $(".heart-icon").mouseenter(function (i, obj) {
      $(this).find("path").css("fill", "#FE2C55");
      $(this).find("path").css("stroke", "transparent");
    });
    $(".heart-icon").mouseleave(function (i, obj) {
      $(this).find("path").css("fill", "white");
      $(this).find("path").css("stroke", "black");
    });
  },
};
</script>
<style src="src/assets/styles/post-caption.css">
</style>
<style>
.channel-name {
  font-weight: 900;
  font-size: 18px;
  font-family: "Lato", Arial, sans-serif;
}
.subtitle {
  color: grey;
  font-size: 14px;
}
</style>
