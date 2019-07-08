<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->

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

    <Table :columns="columns" :rows="rows"/>
  </div>
</template>

<script>
//import HelloWorld from "./components/HelloWorld.vue";
import Table from "./components/Table.vue";

function doSearch() {
  var s = document.getElementById("searchInput").value;
  // getData("search", ["title", "year", "genres", "target"], { title: `%${s}%` });
  this.getData("search", ["asset_id", "asset_name", "asset_make", "asset_number"], {asset_name: `%${s}%`});
  // this.rows.filter((row) => {
  //   Object.filter()
  // })
}

function unescapeValues(obj) {
  var res = [];
  obj.forEach(row => {
    // row here is another object
    var newRow = {};
    if (typeof row == "object") {
      for (var field in row) {
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

function getData(op = "get", fields = [], conditions = {}) {
  var reqUrl = `http://localhost:8082/`;
  // var dbRequest = JSON.stringify({
  //   op: op,
  //   host: 'localhost',
  //   username: 'root',
  //   password: '0f6fNF9D5Mqf0KTW',
  //   db: "lamp_light",
  //   table: "movies",
  //   fields: fields,
  //   conditions: conditions
  // });
  var dbRequest = JSON.stringify({
    op: op,
    host: '192.168.21.51',
    username: 'root',
    password: 'hcet',
    db: "state_static",
    table: "asset",
    fields: fields,
    conditions: conditions
  });
  var req = new Request(reqUrl, {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    mode: "cors",
    body: dbRequest
  });

  fetch(req)
    .then(response => response.json())
    .then(val => {
      if (val) {
        this.rows = [];
        this.columns = [];
        this.rows = unescapeValues(val);
        var cols = [];
        for (var field in val[0]) {
          cols.push(field);
        }
        this.columns = cols;
      }
    });
}

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
    this.getData("get", ["asset_id", "asset_name", "asset_make", "asset_number"], {});
  },
  methods: {
    doSearch,
    getData,
    unescapeValues
  }
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
