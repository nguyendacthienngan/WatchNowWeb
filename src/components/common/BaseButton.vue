<template>
  <button
    type="button"
    :class="`btn btn-outline-danger ${classes}`"
    @click="onClick"
  >
    {{ label }}
  </button>
</template>
<script>
export default {
  props: ["label", "onClick", "classes"],
};
</script>
<style scoped>
.btn-outline-danger {
  border-radius: 20px;
  padding-right: 20px;
  padding-left: 20px;
}
</style>