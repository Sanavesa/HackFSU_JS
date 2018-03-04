<template>
  <div>
    <button v-on:click="requestPost">Post!</button>
    <button v-on:click="requestGet">Get!</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HomePage',
  data () {
    return {
      msg: 'Welcomes!'
    }
  },
  methods: {
    requestGet: function(event) {
      let url = "https://hackfsu18.herokuapp.com/get-data";

      axios.get(url, {
          method: 'GET',
          headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-type': 'application/json'
          },
      }).then(response => console.log(response));
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
      }).then(response => console.log(response));
    }
  }
}
</script>
<style>
h1 {
  font-weight: bold;
}
</style>
