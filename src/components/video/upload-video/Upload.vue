<template>
  <div class="upload-video bg-white h-100">
    <div class="container">
      <br /><br /><br />
      <v-btn class="ma-2 elevation-0" color="white" @click="backToVideos">
        <v-icon left> mdi-arrow-left </v-icon>
        Back to videos
      </v-btn>
      <div style="padding-left: 20px">
        <h3>Upload your video</h3>
      </div>
      <br />
      <v-stepper non-linear v-model="e1">
        <v-stepper-header class="elevation-0">
          <v-stepper-step editable :complete="e1 > 1" step="1">
            Details
          </v-stepper-step>
          <v-divider></v-divider>
          <v-stepper-step step="2" editable> Checks </v-stepper-step>
        </v-stepper-header>

        <v-stepper-items>
          <v-stepper-content step="1">
            <UploadStepOne
              v-bind:video="video"
              @onContinue="e1 = $event"
              @uploadResult="videoUploadResult = $event"
              @onReturn="e1 = $event"
            />
          </v-stepper-content>

          <v-stepper-content step="2">
            <UploadStepTwo
              v-bind:videoUploadResult="videoUploadResult"
              @onReturn="e1 = $event"
            />
          </v-stepper-content>
        </v-stepper-items>
        <br />
      </v-stepper>
    </div>
  </div>
</template>

<style src="src/assets/styles/browse-file.css">
</style>

<script>
import UploadStepOne from "./UploadStepOne.vue";
import UploadStepTwo from "./UploadStepTwo.vue";

export default {
  components: {
    UploadStepOne,
    UploadStepTwo,
  },
  data() {
    return {
      e1: 1,
      video: {},
      videoUploadResult: {},
    };
  },
  methods: {
    backToVideos() {
      const user = JSON.parse(localStorage.getItem("user"));
      const username = user.username;
      this.$router.push({ path: `/${username}/videos` });
    },
  },
  watch: {
    video(newVal) {
      console.log("Video is changed: ", newVal);
      this.e1 = 2;
    },
    videoUploadResult(newVal) {
      console.log("videoUploadResult is changed: ", newVal);
    },
  },
};
</script>

<style src="src/assets/styles/upload.css">
</style>