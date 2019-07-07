<template>
  <div class="container-fluid">
    <!-- <div class="col-2"></div> -->
    <!-- <div class="col-8"> -->
    <table class="table table-striped table-dark table-hover">
      <tr>
        <!-- HEADINGS -->
        <th v-for="field in columns" v-bind:key="field" scope="col">{{field}}</th>
      </tr>
      <!-- ROWS -->
      <tr v-for="(row) in rows" v-bind:key="row" scope="row">
        <td v-for="(value) in row" v-bind:key="value">{{value}}</td>
      </tr>
    </table>
    <!-- </div> -->
    <!-- <div class="col-2"></div> -->
  </div>
</template>

<script>
export default {
  name: "table",
  components: {},
  props: {
    columns: [],
    rows: []
  }
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
}

td,
th {
  text-align: left;
}

tr:hover {
  background-color: darkslategray;
}
</style>
