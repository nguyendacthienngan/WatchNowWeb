<template>
  <div>
    <label for="uploadmyfile">
      <p>Open the Upload dialog</p>
    </label>
    <input type="file" id="uploadmyfile" @change="requestUploadFile" />
  </div>
</template>


<script>
export default {
  methods: {
    requestUploadFile() {
      const src = this.$el.querySelector("#uploadmyfile");
    },
  },
};
</script>