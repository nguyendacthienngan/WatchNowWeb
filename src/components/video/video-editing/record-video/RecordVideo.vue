<template>
  <div>
    <b-modal
      ref="recordVideo"
      id="recordVideo"
      title="Record Video"
      hide-header
      hide-footer
    >
      <span id="toggle-btn" @click="hideModal">
        <i class="fa fa-times fa-2x exit-button"></i>
      </span>
      <br />
      <div class="w-100 text-center">
        <div v-if="!isStarted" style="padding: 50px">
          <h4>Record video</h4>
          <p>Click on the button to start recording</p>
          <button
            id="start-camera"
            class="btn btn-primary"
            @click="recordVideo"
          >
            Open Webcam
          </button>
        </div>
        <video style="width: 0" id="recorded-video" autoplay="autoplay" muted>
          <source type="video/webm" />
        </video>
      </div>
      <div class="container">
        <div v-if="isStarted" class="row text-center justify-content-between">
          <div class="d-flex justify-content-start p-3">
            <button class="btn btn-new btn-cancel" @click="hideModal">
              Cancel
            </button>
          </div>
          <div class="d-flex justify-content-center p-2">
            <img
              id="recordButton"
              :src="require('src/assets/images/recording.svg')"
              width="60px"
              height="55px"
              @click="startRecord"
            />
          </div>
          <div class="d-flex justify-content-end p-3 button">
            <a
              id="download-video"
              download="test.webm"
              class="btn btn-new btn-save"
              >Save</a
            >
          </div>
        </div>
      </div>
    </b-modal>
  </div>
</template>
<style scoped>
#toggle-btn {
  float: right;
}
#recordButton {
  cursor: pointer;
}
#recorded-video {
  /* width: 100%    !important;
        height: auto   !important;   */
  margin-bottom: 5px;
}
#toggle-btn {
  margin-bottom: 5px;
  cursor: pointer;
}
.btn-new {
  border-radius: 15px;
  padding-right: 15px !important;
  padding-left: 15px !important;
}
.btn-cancel {
  background-color: #deecf1 !important;
  padding-top: 0 !important;
  padding-bottom: 0 !important;
}
.btn-save {
  background-color: #c82333 !important;
  color: white !important;
  padding-bottom: 0 !important;
}
</style>
<script>
import $ from "jquery";
export default {
  data() {
    return {
      camera_stream: null,
      media_recorder: null,
      blobs_recorded: [],
      isRecording: false,
      isStarted: false,
    };
  },
  methods: {
    hideModal() {
      this.$refs["recordVideo"].hide();
      this.camera_stream.getTracks().forEach(function (track) {
        track.stop();
      });
      this.isStarted = false;
    },
    async recordVideo() {
      this.isStarted = true;
      const recordVideo = document.getElementById("recorded-video");
      $("#recorded-video").animate({ height: "auto", width: "100%" }, 100);

      this.camera_stream = await navigator.mediaDevices.getUserMedia({
        video: true,
        audio: false,
      });
      recordVideo.srcObject = this.camera_stream;
    },
    startRecord() {
      if (this.isRecording == false) {
        this.isRecording = true;
        // set MIME type of recording as video/webm
        this.media_recorder = new MediaRecorder(this.camera_stream, {
          mimeType: "video/webm",
        });

        // event : new recorded video blob available
        const vm = this;

        this.media_recorder.addEventListener("dataavailable", function (e) {
          vm.blobs_recorded.push(e.data);
        });

        // event : recording stopped & all blobs sent
        this.media_recorder.addEventListener("stop", function () {
          // create local object URL from the recorded video blobs
          const video_local = URL.createObjectURL(
            new Blob(vm.blobs_recorded, { type: "video/webm" })
          );
          const download_link = document.querySelector("#download-video");

          download_link.href = video_local;
        });

        // start recording with each recorded blob having 1 second video
        this.media_recorder.start(1000);
      } else {
        this.media_recorder.stop();
        const recordVideo = document.getElementById("recorded-video");
        recordVideo.pause();
      }
    },
    stopRecord() {
      this.media_recorder.stop();
    },
  },
  mounted() {},
};
</script>