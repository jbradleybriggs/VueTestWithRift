<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->

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
      >
    </div>
    <!-- <Search @emitSearch="updateSearch" :searchValue="searchValue"/> -->
    <Table :columns="columns" :rows="rows" :searchValue="searchValue"/>
  </div>
</template>

<script>
import Search from "./components/Search.vue" ;
import Table from "./components/Table.vue";

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

function getData(op = "get", fields = [], conditions = {}, options={}) {
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
    host: '192.168.21.51',
    username: 'root',
    password: 'hcet',
    db: "accounting_state_static",
    table: "client_group",
    op: op,
    fields: fields,
    conditions: conditions,
    options: options
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
    Search,
    Table
  },
  data: () => ({
    error: "",
    columns: [],
    rows: [],
    test: {},
    searchValue: ""
  }),
  mounted() {
    this.getData("get", [], {});
  },
  methods: {
    getData,
    unescapeValues,
    updateSearch(val) {
      alert(val) ;
      this.searchValue = val ;
    }
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
