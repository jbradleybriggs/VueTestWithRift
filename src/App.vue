<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Table :columns="columns" :rows="rows"/>
    <!-- <table class="table table-striped table-dark table-hover">
      <tr>
        <th v-for="field in columns" v-bind:key="field" scope="col">
          {{field}}
        </th>
      </tr>
      <tr v-for="(row) in rows" v-bind:key="row" scope="row" >
        <td v-for="(value) in row" v-bind:key="value">
          {{value}}
        </td>
      </tr>
    </table>-->
  </div>
</template>

<script>
//import HelloWorld from "./components/HelloWorld.vue";
import Table from "./components/Table.vue";

export default {
  name: "app",
  components: {
    Table
  },
  data: () => ({
    error: "",
    columns: [],
    rows: [],
    test: {}
  }),
  mounted() {
    var reqUrl = `http://localhost:8081/`;
    var dbRequest = JSON.stringify({
      op: "get",
      db: "lamp_light",
      table: "movies",
      fields: ["title", "year", "genres", "target"],
      conditions: {}
    });
    var req = new Request(reqUrl, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      mode: "cors",
      body: dbRequest
    });

    function unescapeValues(obj) {
      var res = [];
      obj.forEach(row => {
        // row here is another object
        var newRow = {};
        if (typeof row == "object") {
          // row.forEach((val, index) => {

          // })
          for (var field in row) {
            //console.log(row[field]);
            try {
              newRow[field] = decodeURI(row[field]);
              /*if (field == "target") {
                newRow[field] = `<a href="${newRow[field]}">Watch</a>`;
              }*/
            } catch (error) {
              newRow[field] = row[field];
            }
          }
          res.push(newRow);
        }
      });
      return res;
    }

    fetch(req)
      .then(response => response.json())
      .then(val => {
        if (val) {
          this.rows = unescapeValues(val);
          var cols = [];
          for (var field in val[0]) {
            cols.push(field);
          }
          this.columns = cols;
        }
      });
  },
  methods: {}
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: black;
}
html {
  background-color: black;
}
</style>
