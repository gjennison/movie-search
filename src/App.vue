<template>
    <div id="app">
        <input placeholder="enter film" type="text" v-model="title" @keyup="request"/>

        <div class="columns">
            <div class="column" v-for="movie in movies" :key="movie.imdbID">
                <h3>
                {{movie.title}}
                </h3>
                <img v-if="returnImgPath(movie.poster_path) != 'null'" :src="returnImgPath(movie.poster_path)">
                <p v-if="returnImgPath(movie.poster_path) === 'null'"><i>missing image</i></p>
            </div>
        </div>
    </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
        movies: [],
        title: ""
    }
  },

      methods:{
        request: function(){

            var xhttp = new XMLHttpRequest;

            var comp = this;
            var json;

            xhttp.open("GET", "https://api.themoviedb.org/3/search/movie?api_key=20444f8220ad153ffa58841455d771c1&query=" + this.title);
            xhttp.send();
            xhttp.onload = function(){
                json = JSON.parse(xhttp.responseText);
                comp.movies = json.results;
            }
        },

        returnImgPath: function(path){
            if(path === null){
                return "null"
            }
            else
                return "https://image.tmdb.org/t/p/w185" + path;
        }
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.columns{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.column{
  background: #B7CECE;
  margin: 24px;
}

img{
  width: 185px;
  height: 256px;
  margin-bottom: 24px;
}
</style>
