<template>
  <div class="container-fluid">
    <!-- <div class="col-2"></div> -->
    <!-- <div class="col-8"> -->
    <table class="table table-striped table-dark table-hover">
      <tr>
        <!-- HEADINGS -->
        <th v-for="(field, index) in columns" v-bind:key="index" scope="col">{{field}}</th>
      </tr>
      <!-- ROWS -->
      <tr v-for="(row, index) in filterFn" v-bind:key="index" scope="row">
        <td v-for="(value, index) in row" v-bind:key="index">{{value}}</td>
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
    rows: [],
    searchValue: ""
  },
  computed: {
    filterFn() {
      return this.rows.filter((row) => {
        for (var field in row) {
          if (row[field].toLowerCase().match(this.searchValue.toLowerCase())) return true ; 
        }
        return false;
      });
    }
  }
}

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
