<template>
  <div>

    <!-- Logo -->
    <img src="../assets/logo.png" alt="Logo" class="centered-logo">

    <!-- Category Dropdown -->
    <select class="search-category">
      <option v-for="category in categories">{{ category }}</option>
    </select>

    <!-- Search Bar -->
    <input autofocus type="text" class="search-input-field" placeholder="Enter search..." id="searchBar">

    <!-- Search Button  -->
    <button v-on:click="onPressSearch" class="search-button">
      <span>Search </span>
    </button>

    <!-- Reset Button  -->
    <button v-on:click="onPressReset" class="search-button">
      <span>Reset </span>
    </button>

    <!-- Inner HTML of Viewing -->
    <div v-if="viewingDocument != null">
      <div class="search-result-container">
        <div class="search-result-lecture">{{ viewingDocument.lecture }}</div>
        <div class="search-result-course">{{ viewingDocument.course }}</div>
        <div class="search-result-category">{{ viewingDocument.category }} -- {{ viewingDocument.professor }}</div>
        <p class="search-result-summary">{{ viewingDocument.summary }}</p>
        <p class="search-result-summary">{{ viewingDocument.summary }}</p>
      </div>
    </div>
    <div v-else>
      <!-- Search Results -->
      <div v-if="viewingSearchResults">
        <div v-for="entry in filtered_db">
          <div v-on:click="onEntryClicked" class="search-result-container">
            <div class="search-result-lecture">{{ entry.lecture }}</div>
            <div class="search-result-course">{{ entry.course }}</div>
            <div class="search-result-category">{{ entry.category }} -- {{ entry.professor }}</div>
            <p class="search-result-summary">{{ entry.summary }}</p>
          </div>
        </div>
      </div>
      <div v-else>
        <div v-for="entry in db">
          <div v-on:click="onEntryClicked" class="search-result-container">
            <div class="search-result-lecture">{{ entry.lecture }}</div>
            <div class="search-result-course">{{ entry.course }}</div>
            <div class="search-result-category">{{ entry.category }} -- {{ entry.professor }}</div>
            <p class="search-result-summary">{{ entry.summary }}</p>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HomePage',
  data () {
    return {
      db: [],
      filtered_db: [],
      categories: [],
      viewingSearchResults: false,
      viewingDocument: null
    }
  },
  methods: {
    onEntryClicked: function(event) {
      console.log(event.target);
    },
    onPressReset: function() {
      this.viewingSearchResults = false;
      this.viewingDocument = null;
    },
    onPressSearch: function() {
      let text = document.getElementById("searchBar").value;
      if(text !== null && text.length > 0) {
        this.viewingSearchResults = true;
        this.viewingDocument = null;
        this.searchDatabase(text);
      }
    },
    searchDatabase: function(text) {
      this.filtered_db = [];

      for(let i = 0; i < this.db.length; i++) {
        let summary = this.db[i].summary;
        if(summary.includes(text)) {
          this.filtered_db.push(this.db[i]);
        }
      }
    },
    requestGet: function(event) {
      let url = "https://hackfsu18.herokuapp.com/get-data";

      axios.get(url, {
          method: 'GET',
          headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-type': 'application/json'
          },
      }).then(response => {
        let data = response.data;
        // console.log(data);

        let keys = Object.keys(data);
        let vals = [];

        vals = keys.map(key => data[key].raw);

        // console.log(vals);

        for(let i = 0; i < vals.length; i++) {
          let myObj = {
            lecture: vals[i].lecture,
            course: vals[i].course,
            category: vals[i].category,
            summary: vals[i].summary,
            professor: vals[i].professor,
            outline: vals[i].outline
          };
          // console.log(myObj);
          this.db.push(myObj);

          let catIndex = this.categories.indexOf(vals[i].category);
          if(catIndex == -1) { // doesnt exists, so add it
            this.categories.push(vals[i].category);
          }
        }
      });
    },
    requestPost: function(event) {
      let obj = {
          "lecture": "MyAwesomeness",
          "course": "CS 420",
          "professor": "Prof. Robinson",
          "category": "AWESOME",
          "text": "some text that i love to decipher and summarize"
      };

      let jsonified = JSON.stringify(obj);
      let url = "https://hackfsu18.herokuapp.com/send-full-text";

      axios.post(url, obj, {
          method: 'POST',
          // baseURL: 'https://hackfsu18.herokuapp.com/',
          // url: '/send-full-text'
          headers: {
            'Access-Control-Allow-Origin': '*',
          //   'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
          //   'Access-Control-Allow-Headers': 'Origin, Content-Type, X-Auth-Token',
            'Content-type': 'application/json'
          },
          // withCredentials: true,
          // credentials: 'same-origin',
      }).then(response => {
        //console.log(response);
        // let data = response["data"];
        // for(d in data) {
        //   console.log(d);
        // }
        //console.log(response["data"]);
      });
    }
  },
  created: function() {
    console.log("HI!!");
    this.requestGet();
    // var me = this;
    // setInterval(function() {
    //   console.log("YEAS!");
    //   me.requestGet();
    // }, 3000);
  }
}
</script>
<style>

html {
  height: 100%;
}

body {
  background: repeating-linear-gradient(90deg, pink, white);
}

.centered-logo {
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-top: 30px;
  width: 35%;
  user-select: none;
}

.centered-div {
  border: 0 auto;
}

.search-category {
  display: inline-block;
  height: 40px;
  border-radius: 4px;
  margin-left: 20%;
  margin-right: 15px;
  width: 140px;
  font-size: 20px;
  border-width: 1px;
  border-color: rgba(0.1, 0.1, 0.1, 0.25);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  transition: all 0.5s;
}

.search-category:hover {
  opacity: 1;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
  transform: scale(1.04);
}

.search-input-field {
  display: inline-block;
  width: 45%;
  padding: 5px;
  margin-top: 50px;
  margin-bottom: 20px;
  margin-left: auto;
  margin-right: auto;
  height: 28px;
  font-size: 20px;
  opacity: 0.75;
  background-image: url("../assets/magnifying_glass.png");
  background-size: 32px 32px;
  background-repeat: no-repeat;
  background-position: right;
  border-width: 1px;
  border-radius: 4px;
  border-color: rgba(0.1, 0.1, 0.1, 0.25);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  transition: all 0.5s;
}

.search-input-field:hover {
  opacity: 1;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
  transform: scale(1.03);
}

.search-button {
  display: block;
  width: 128px;
  height: 40px;
  margin-left: auto;
  margin-right: auto;
  margin-top: 5px;
  margin-bottom: 25px;
  font-size: 20px;
  justify-content: center;
  border-width: 2px;
  vertical-align: middle;
  background-color: #ffffff;
  text-decoration: none;
  border-radius: 8px;
  padding: 8px;
  border-color: rgba(225, 225, 225, 0.65);
  box-shadow: 0 6px 30px rgba(0, 0, 0, 0.3);
  transition: all 0.5s;
}

.search-button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: all 0.5s;
}

.search-button span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

.search-button:hover span {
  padding-right: 25px;
}

.search-button:hover span:after {
  opacity: 1;
  right: 0;
}

.search-button:hover {
  /*background-color: #ffebff;*/
  background-color: rgba(155, 155, 155, 0.5);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.search-result-container {
  display: grid;
  grid-column-gap: 30px;
  width: 80%;
  height: 35%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 10px;
  margin-bottom: 10px;
  border-color: black;
  border-width: 2px;
  border-radius: 8px;
  border-style: solid;
  background-color: rgba(255, 255, 255, 0.8);
  box-shadow: 0 6px 30px rgba(0, 0, 0, 0.3);
  transition: all 0.5s;
}

.search-result-container:hover {
  cursor: pointer;
  background-color: rgba(155, 155, 155, 0.5);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.3);
  transform: scale(1.07);
}

.search-result-lecture {
  width: 40%;
  font-weight: bold;
  text-align: left;
  justify-content: left;
  padding: 10px;
  padding-bottom: 0px;
  grid-column-start: 1;
  grid-column-end: 2;
}

.search-result-course {
  width: 30%;
  font-weight: bold;
  padding: 10px;
  padding-bottom: 0px;
  text-align: center;
  justify-content: center;
  grid-column-start: 2;
  grid-column-end: 3;
}

.search-result-category {
  width: 30%;
  font-weight: bold;
  padding: 10px;
  padding-bottom: 0px;
  text-align: right;
  justify-content: right;
  grid-column-start: 3;
  grid-column-end: 4;
}

.search-result-summary {
  width: auto;
  grid-column-start: 1;
  grid-column-end: 4;
  text-align: justify-all;
  margin-right: 20px;
  margin-left: 20px;
  font-size: 14pt;
}

.search-result-container > div {
  text-align: left;
  padding-left: 10px;
  font-size: 20px;
}

</style>
