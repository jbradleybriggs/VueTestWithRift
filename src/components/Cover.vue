<template>
  <div class="cover">
    <img v-bind:src="source" alt="No Cover">
  </div>
</template>

<script>
export default {
  name: "cover",
  components: {},
  props: {
    width: {
      default: 50
    },
    height: {
      default: 100
    },
    source: {
      default: "D:\\Projects\\GitHub\\vtest2\\src\\assets\\cover.jpg"
    }
  }
};
</script>

<style>
</style>
