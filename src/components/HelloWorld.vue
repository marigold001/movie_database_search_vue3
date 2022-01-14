<template>
  <div class="hello">
      <div id="searchField">
        <i class="fas fa-search"></i>
        <input type="text" v-model="searchTerm" v-on:keyup="updateList" placeholder="Search...">
        <select name="type" id="type" v-model="value" v-on:click="changeValue">
          <option value="&type=movie">Movie</option>
          <option value="&type=seres">Series</option>
          <option value="&type=episode">Episode</option>
        </select>
      </div>
      <ul v-if="info">
        <li v-for="(movie, index) in info" :key="index">
          <img :src="movie.Poster" alt="Movie poster">
          <h2>{{movie.Title}} </h2>
        </li>
      </ul>
        <p v-if="error">{{this.error}}</p>
  </div>
</template>

<script>
// Imports axios node module from node modules
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      info: null,
      apiKey: '3154fa4f',
      searchTerm: '',
      error: 'Search for something',
      value: '&type=movie'
    }
  },
  mounted() {
    
  },
  methods: {
    updateList() {
        axios.get('https://www.omdbapi.com/?apikey=' + this.apiKey + this.value + '&s=' + this.searchTerm)
      .then((response) => {
        if(response.data.Response == "True") {
          this.info = response.data.Search
          this.error = null
        } else {
          if(response.data.Error == "Something went wrong.") {
            this.error = "Search for something."
          } else {
            this.error = response.data.Error
          }
          this.info = null
        }
        console.log(response)
      })
      .catch(this.error = "Something went wrong.")
      .then();
      },
      changeValue() {
        if(this.value == "&type=movie") {
          this.searchTerm = ''
        }
        if(this.value == "&type=series") {
          this.searchTerm = ''
        }
        if(this.value == "&type=episode") {
          this.searchTerm = ''
        }
       }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div#searchField {
  background: #1BBC9B;
  width: 900px;
  margin: 0px auto;
  border-radius: 5px;
}
div#searchField i {
  float: left;
  padding-top: 10px;
  padding-left: 10px;
  line-height: 40px;
  font-size: 15px;
}
div#searchField input {
  padding: 20px;
  float: left;
  font-size: 15px;
  width: 60%;
  background: transparent;
  outline: none;
  border: none;
}
div#searchField input::placeholder {
  font-size: 15px;
  color: #000;
}
div#searchField select {
  padding: 20px;
  font-size: 15px;
  outline: none;
  border: none;
  background: transparent;
}
li {
  border-bottom: 1px solid grey;
  margin: 0px auto;
  max-width: 900px;
  list-style-type: none;
  background: #34495E;
  color: #fff;
}
li:hover {
  background: #2D3E50;
  color: #1BBC9B;
}
li img {
  height: 100px;
  float: left;
  margin-top: 20px;
  margin-left: 10px;
}
h2 {
  display: inline-block;
  line-height: 100px;
  font-size: 30px;
  max-width: 80%;
}
ul {
  padding: 0px;
  margin: 0px;
}
</style>
