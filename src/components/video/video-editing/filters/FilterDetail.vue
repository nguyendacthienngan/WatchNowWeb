<template>
  <div>
    <div class="filter-title">
      {{ title }}
      <b-button
        :class="visible ? null : 'collapsed'"
        :aria-expanded="visible ? 'true' : 'false'"
        :aria-controls="id"
        @click="visible = !visible"
        id="filter-show-button"
        class="vertical-center"
      >
        <i class="fa fa-sort-down"></i>
      </b-button>
    </div>
    <b-collapse :id="id" v-model="visible" class="mt-2 filter row text-center">
      <div class="col">
        <div :id="filter_1">
          <img
            :src="require('src/assets/images/sample/Filter_1.png')"
            width="120px"
            height="122px"
            style="object-fit: scale-down"
          />
        </div>
        <div :id="filter_2" class="">
          <img
            :src="require('src/assets/images/sample/Filter_1.png')"
            width="120px"
            height="122px"
            style="object-fit: scale-down"
          />
        </div>
        <div :id="filter_3" class="">
          <img
            :src="require('src/assets/images/sample/Filter_1.png')"
            width="120px"
            height="122px"
            style="object-fit: scale-down"
          />
        </div>
      </div>
    </b-collapse>
  </div>
</template>
<script>
export default {
  data() {
    return {
      visible: true,
    };
  },
  props: ["title", "filter_1", "filter_2", "filter_3", "id"],
};
</script>
<style>
@media only screen and (min-width: 576px) {
  .filter div {
    justify-content: space-between !important;
    display: flex !important;
  }
}
@media only screen and (max-width: 576px) {
  .filter-title {
    text-align: center;
  }
  .filter div {
    margin-bottom: 20px;
    display: block !important;
    text-align: center;
  }
}
#filter-show-button {
  background-color: transparent;
  border: 1px solid transparent;
  color: black;
}
</style>