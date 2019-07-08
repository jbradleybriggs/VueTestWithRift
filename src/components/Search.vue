<template>
  <div class="input-group mb-3" style="padding-left: 15px; padding-right: 15px">
      <input
        v-model.lazy="searchValue"
        type="text"
        class="form-control"
        placeholder="Search"
        aria-label="Search"
        aria-describedby="basic-addon2"
        style="padding: 5px;"
        id="searchInput"
        v-on:keyup.enter="doSearch"
      >
    </div>
</template>

<style>
</style>

<script>
export default {
  name: "Search",
  components: {},
  props: {
    searchValue: ""
  },
  doSearch: function() {
    alert("YES")
    this.$emit('emitSearch', this.searchValue) ;
  }
};
</script>
