<template>
  <b-modal
    ref="browseFile"
    id="browseFile"
    title="Browse File"
    hide-header
    hide-footer
  >
    <div class="drop drag-component">
      <div class="helper"></div>
      <div
        class="drop display-inline align-center p-5"
        @dragover.prevent
        @drop="onDrop"
      >
        <div class="helper"></div>
        <label class="btn display-inline">
          <img
            :src="require('src/assets/images/Folder_Black.png')"
            width="90px"
            height="90px"
            style="max-width: 90px"
          />

          <p class="padding-20">
            Drag and drop files from your computer, or <b>browse files</b>
          </p>
          <input type="file" name="video" @change="onChange" />
        </label>
      </div>
    </div>
  </b-modal>
</template>
<style src="src/assets/styles/browse-file.css">
</style>
<script>
export default {
  data: function () {
    return {
      video: "",
    };
  },
  methods: {
    save: function () {
      this.$refs["browseFile"].hide();
    },
    onDrop: function (e) {
      e.stopPropagation();
      e.preventDefault();
      const files = e.dataTransfer.files;
      this.createFile(files[0]);
    },
    onChange(e) {
      const files = e.target.files;
      this.createFile(files[0]);
    },
    createFile(file) {
      // if (!file.type.match('video.*')) {
      //   alert('Select an video');
      //   return;
      // }
      if (!file) {
        alert("Failed to load file");
      } else {
        const reader = new FileReader();

        const vm = this;

        reader.onload = function (e) {
          // vm.video = e.target.result;

          // The file reader gives us an ArrayBuffer:
          const buffer = e.target.result;
          const uint8Array = new Uint8Array(buffer);
          const arrayBuffer = uint8Array.buffer;
          const blob = new Blob([arrayBuffer]);
          const url = URL.createObjectURL(blob);
          vm.video = url;

          const vid = document.getElementById("video-drag");
          vid.src = url;
          vid.load();
        };
        reader.readAsArrayBuffer(file);
        this.save();
        // this.$router.push('/editing/mymedia')
        // reader.readAsDataURL(file);
      }
    },
    removeFile() {
      this.video = "";
    },
  },
  mounted() {},
};
</script>