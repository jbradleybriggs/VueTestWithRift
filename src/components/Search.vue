<template>
  <div class="input-group mb-3" style="padding-left: 15px; padding-right: 15px">
    <input
      type="text"
      class="form-control"
      placeholder="Search Movies"
      aria-label="Search Movies"
      aria-describedby="basic-addon2"
      style="padding: 5px;"
      id="searchInput"
    >
    <div class="input-group-append">
      <button class="btn btn-outline-secondary" type="button" v-on:click="doSearch()">Search</button>
    </div>
  </div>
</template>

<style>
</style>

<script>
export default {
  name: "search",
  components: {},
  props: {
    columns: [],
    rows: []
  }
};
</script>
